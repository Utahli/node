# InnoDB记录存储结构

`InnoDB`采取的方式是：将数据划分为若干个页，以页作为磁盘和内存之间交互的基本单位，InnoDB中页的大小一般为 **16**KB。也就是在一般情况下，一次最少从磁盘中读取16KB的内容到内存中，一次最少把内存中的16KB内容刷新到磁盘中。 

设计`InnoDB`存储引擎现在为止设计了4种不同类型的`行格式`，分别是`Compact`、`Redundant`、`Dynamic`和`Compressed`行格式 

`行格式`是在我们创建或修改表的语句中指定的

```
CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称
ALTER TABLE 表名 ROW_FORMAT=行格式名称
```



### COMPACT行格式

废话不多说，直接看图：

![image_1c9g4t114n0j1gkro2r1h8h1d1t16.png-42.4kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey37MIoP0YPpYdY7Y0TO0iaZ3a79QB7GiaHyTqJTicNBQ6Nk202h4JECicib3A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

 

##### 变长字段长度列表

`MySQL`支持一些变长的数据类型，比如`VARCHAR(M)`、`VARBINARY(M)`、各种`TEXT`类型，各种`BLOB`类型，这些变长的数据类型占用的存储空间分为两部分：

1. 真正的数据内容
2. 占用的字节数

因为如果不保存真实数据占用的字节数的话，MySQL服务器也不知道我们存储的数据究竟有多长。在`Compact`行格式中，把所有变长类型的列的长度都存放在记录的开头部位形成一个列表，按照列的顺序逆序存放，我们再次强调一遍，是逆序存放！ 

需要注意的一点是，==变长字段长度列表中只存储值为 **非NULL** 的列内容占用的长度，值为 **NULL** 的列的长度是不储存的 。== 



##### NULL值列表

我们知道表中的某些列可能存储`NULL`值，如果把这些NULL值都放到`记录的真实数据`中存储会很占地方，所以`Compact`行格式把这些值为`NULL`的列统一管理起来，存储到`NULL`值列表中，它的处理过程是这样的：

1. 首先统计表中允许存储`NULL`的列有哪些。
2. 如果表中没有允许存储 **NULL** 的列，则 *NULL值列表* 也不存在了，否则将每个允许存储`NULL`的列对应一个二进制位，二进制位按照列的顺序逆序排列， 
   - 二进制位的值为`1`时，代表该列的值为`NULL`。
   - 二进制位的值为`0`时，代表该列的值不为`NULL`。
3. `MySQL`规定`NULL值列表`必须用整数个字节的位表示，如果使用的二进制位个数不是整数个字节，则在字节的高位补`0`。 



##### 记录头信息

除了`变长字段长度列表`、`NULL值列表`之外，还有一个用于描述记录的`记录头信息`，它是由固定的`5`个字节组成。`5`个字节也就是`40`个二进制位，不同的位代表不同的意思，如图：

![image_1c9geiglj1ah31meo80ci8n1eli8f.png-29.5kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3seeEXfe32gnNpyFDpGADNaia0ytgBf2l35mLFECb1jI4HJmEoFJAOmA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9geiglj1ah31meo80ci8n1eli8f.png-29.5kB

这些二进制位代表的详细信息如下表：

| 名称           | 大小（单位：bit） | 描述                                                         |
| -------------- | ----------------- | ------------------------------------------------------------ |
| `预留位1`      | `1`               | 没有使用                                                     |
| `预留位2`      | `1`               | 没有使用                                                     |
| `delete_mask`  | `1`               | 标记该记录是否被删除                                         |
| `min_rec_mask` | `1`               | 标记该记录是否为B+树的非叶子节点中的最小记录                 |
| `n_owned`      | `4`               | 表示当前槽管理的记录数                                       |
| `heap_no`      | `13`              | 表示当前记录在记录堆的位置信息                               |
| `record_type`  | `3`               | 表示当前记录的类型，`0`表示普通记录，`1`表示B+树非叶节点记录，`2`表示最小记录，`3`表示最大记录 |
| `next_record`  | `16`              | 表示下一条记录的相对位置                                     |

 

#### 记录的真实数据

`记录的真实数据`除了我们插入的那些列的数据，`MySQL`会为每个记录默认的添加一些列（也称为`隐藏列`），具体的列如下：

| 列名             | 是否必须 | 占用空间 | 描述                   |
| ---------------- | -------- | -------- | ---------------------- |
| `row_id`         | 否       | `6`字节  | 行ID，唯一标识一条记录 |
| `transaction_id` | 是       | `6`字节  | 事务ID                 |
| `roll_pointer`   | 是       | `7`字节  | 回滚指针               |

需要注意的是，MySQL服务器会为每条记录都添加 **transaction_id** 和 **roll_pointer** 这两个列，但是 **row_id** 只有在表没有定义主键的时候才会为记录添加，相当于MySQL服务器帮我们来添加一个主键。这些列的值不用我们操心，`MySQL`服务器会自己帮我们添加的。



我们现在向这个表中插入两条记录：

```
mysql> INSERT INTO record_format_demo(c1, c2, c3, c4) VALUES('aaaa', 'bbb', 'cc', 'd'), ('eeee', 'fff', NULL, NULL);
Query OK, 2 rows affected (0.02 sec)
Records: 2  Duplicates: 0  Warnings: 0
mysql> 
```

表中的记录就是这个样子的：

```
mysql> SELECT * FROM record_format_demo;
+------+-----+------+------+
| c1   | c2  | c3   | c4   |
+------+-----+------+------+
| aaaa | bbb | cc   | d    |
| eeee | fff | NULL | NULL |
+------+-----+------+------+
2 rows in set (0.00 sec)
mysql>
```



因为表`record_format_demo`并没有定义主键，所以`MySQL`服务器会为每条记录增加上述的3个列。现在看一下加上`记录的真实数据`的两个记录长什么样吧：

![image_1c9h256f9nke14311adhtu61ie2dn.png-92kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3b0m7SgCx0hOqzK7A66ARzntr7vXLCoAzpVEZNdNdvrPG8nOdSEq13w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9h256f9nke14311adhtu61ie2dn.png-92kB

看这个这个图的时候我们需要注意几点：

1. 表`record_format_demo`使用的是`ascii`字符集，所以`0x61616161`就表示字符串`'aaaa'`，`0x626262`就表示字符串`'bbb'`，以此类推。
2. 注意第1条记录中`c3`列的值，它是`CHAR(10)`类型的，它实际存储的字符串是：`'cc'`，`ascii`字符集中的字节表示是`'0x6363'`，虽然表示这个字符串只占用了2个字节，但整个`c3`列仍然占用了10个字节的空间，除真实数据以外的8个字节的统统都用空格字符填充，空格字符在`ascii`字符集的表示就是`0x20`。
3. 注意第2条记录中`c3`和`c4`列的值都为`NULL`，它们被存储在了前边的`NULL值列表`处，在记录的真实数据处就不再冗余存储，从而节省存储空间。



### 行溢出数据

#### VARCHAR(M)最多能存储的数据

我们知道对于`VARCHAR(M)`类型的列最多可以占用`65535`个字节。其中的`M`代表该类型最多存储的字符数量，如果我们使用`ascii`字符集的话，一个字符就代表一个字节，我们看看`VARCHAR(65535)`是否可用：

```
mysql> CREATE TABLE varchar_size_demo(
    ->     c VARCHAR(65535)
    -> ) CHARSET=ascii ROW_FORMAT=Compact;
ERROR 1118 (42000): Row size too large. The maximum row size for the used table type, not counting BLOBs, is 65535. This includes storage overhead, check the manual. You have to change some columns to TEXT or BLOBs
mysql>
```

从报错信息里可以看出，`MySQL`对一条记录占用的最大存储空间是有限制的，除了`BLOB`或者`TEXT`类型的列之外，其他所有的列（不包括隐藏列和记录头信息）占用的字节长度加起来不能超过`65535`个字节。所以`MySQL`服务器建议我们把存储类型改为`TEXT`或者`BLOB`的类型。这个`65535`个字节除了列本身的数据之外，还包括一些`storage overhead`，比如说我们为了存储一个`VARCHAR(M)`类型的列，需要占用3部分存储空间：

- 真实数据
- 真实数据占用字节的长度
- `NULL`值标识，如果该列有`NOT NULL`属性则可以没有这部分存储空间

如果该`VARCHAR`类型的列没有`NOT NULL`属性，那最多只能存储`65532`个字节的数据，因为真实数据的长度需要占用2个字节，`NULL`值标识需要占用1个字节：

```
mysql> CREATE TABLE varchar_size_demo(
    ->      c VARCHAR(65532)
    -> ) CHARSET=ascii ROW_FORMAT=Compact;
Query OK, 0 rows affected (0.02 sec)

mysql>
```

如果`VARCHAR`类型的列有`NOT NULL`属性，那最多只能存储`65533`个字节的数据，因为真实数据的长度需要占用2个字节，不需要`NULL`值标识：

```
mysql> DROP TABLE varchar_size_demo;
Query OK, 0 rows affected (0.01 sec)

mysql> CREATE TABLE varchar_size_demo(
    ->      c VARCHAR(65533) NOT NULL
    -> ) CHARSET=ascii ROW_FORMAT=Compact;
Query OK, 0 rows affected (0.02 sec)

mysql>
```

如果`VARCHAR(M)`类型的列使用的不是`ascii`字符集，那会怎么样呢？来看一下：

```
mysql> DROP TABLE varchar_size_demo;
Query OK, 0 rows affected (0.00 sec)
mysql> CREATE TABLE varchar_size_demo(
    ->       c VARCHAR(65532)
    -> ) CHARSET=gbk ROW_FORMAT=Compact;
ERROR 1074 (42000): Column length too big for column 'c' (max = 32767); use BLOB or TEXT instead
mysql> CREATE TABLE varchar_size_demo(
    ->       c VARCHAR(65532)
    -> ) CHARSET=utf8 ROW_FORMAT=Compact;
ERROR 1074 (42000): Column length too big for column 'c' (max = 21845); use BLOB or TEXT instead
mysql>
```

从执行结果中可以看出，如果`VARCHAR(M)`类型的列使用的不是`ascii`字符集，那`M`的最大取值取决于该字符集表示一个字符最多需要的字节数。比方说`gbk`字符集表示一个字符最多需要`2`个字符，那在该字符集下，`M`的最大取值就是`32767`，也就是说最多能存储`32767`个字符；`utf8`字符集表示一个字符最多需要`3`个字符，那在该字符集下，`M`的最大取值就是`21845`，也就是说最多能存储`21845`个字符。



#### 记录中的数据太多产生的溢出

我们以`ascii`字符集下的`varchar_size_demo`表为例，插入一条记录：

```
mysql> CREATE TABLE varchar_size_demo(
    ->       c VARCHAR(65532)
    -> ) CHARSET=ascii ROW_FORMAT=Compact;
Query OK, 0 rows affected (0.01 sec)

mysql> INSERT INTO varchar_size_demo(c) VALUES(REPEAT('a', 65532));
Query OK, 1 row affected (0.00 sec)

mysql>
```

其中的`REPEAT('a', 65532)`是一个函数调用，它表示生成一个把字符`'a'`重复`65532`次的字符串。前边说过，`MySQL`中磁盘和内存交互的基本单位是`页`，也就是说`MySQL`是以`页`为基本单位来管理存储空间的，我们的记录都会被分配到某个`页`中存储。而一个页的大小一般是`16KB`，也就是`16384`字节，而一个`VARCHAR(M)`类型的列就最多可以存储`65532`个字节，这样就可能造成一个页存放不了一条记录的尴尬情况。

在`Compact`和`Reduntant`行格式中，对于占用存储空间非常大的列，在`记录的真实数据`处只会存储该列的一部分数据，把剩余的数据分散存储在几个连续的页中，只在`记录的真实数据`处用20个字节存储指向这些页的地址，从而可以找到剩余数据所在的页，如图所示：

![image_1c9j5tdn316v11j8m15631f533r5h4.png-121.7kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3NAkCN8pB5UZduWZl8Fd83JsQdtgXbBpboFjrXib3WFrHvxd1TbhicXhA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9j5tdn316v11j8m15631f533r5h4.png-121.7kB

从图中可以看出来，对于`Compact`和`Reduntant`行格式来说，如果某一列中的数据非常多的话，在本记录的真实数据处只会存储该列的前`786`个字节的数据和一个指向其他页的地址，然后把剩下的数据存放到其他页中，这个过程也叫做`行溢出`。画一个简图就是这样：

![image_1c9jc8v88uo3dpoav8n70um1hu.png-22.7kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3ib7Tq6vJ7UupNM8xYcOJ3eibT7ib9RczQ9b7ibtESicfqgA2HvhPvugd2Ww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9jc8v88uo3dpoav8n70um1hu.png-22.7kB

不只是 **VARCHAR(M)** 类型的列，其他的 **TEXT**、**BLOB** 类型的列在存储数据非常多的时候也会发生`行溢出`。

#### `行溢出`临界点

那发生`行溢出`的临界点是什么呢？也就是说在列存储多少字节的数据时就会发生`行溢出`？

`MySQL`中规定一个页中至少存放两行记录，至于为什么这么规定我们之后再说，现在看一下这个规定造成的影响。以我们以上边的`varchar_size_demo`表为例，它只有一个列`c`，我们往这个表中插入两条记录，每条记录最少插入多少字节的数据才会`行溢出`的现象呢？这得分析一下页中的空间都是如何利用的。

- 每个页除了存放我们的记录以外，也需要存储一些额外的信息，乱七八糟的额外信息加起来需要`136`个字节的空间（现在只要知道这个数字就好了），其他的空间都可以被用来存储记录。

- 每个记录需要的额外信息是`27`字节。

  这27个字节包括下边这些部分：

- - 2个字节用于存储真实数据的长度
  - 1个字节用于存储列是否是NULL值
  - 5个字节大小的头信息
  - 6个字节的`row_id`列
  - 6个字节的`transaction_id`列
  - 7个字节的`roll_pointer`列

假设一个列中存储的数据字节数为n，那么发生`行溢出`现象时需要满足这个式子：

```
136 + 2×(27 + n) > 16384
```

求解这个式子得出的解是：`n > 8098`。也就是说如果一个列中存储的数据不大于`8098`个字节，那就不会发生`行溢出`，否则就会发生`行溢出`。

我们这个只是针对只有一个列的`varchar_size_demo`表来说的，如果表中有多个列，那上边的式子又得改一改了，所以重点就是：你不用关注这个临界点是什么，只要知道如果我们想一个行中存储了很大的数据时，可能发生`行溢出`的现象。

### Dynamic和Compressed行格式

下边要介绍两个比较新的行格式，`Dynamic`和`Compressed`行格式，我现在使用的MySQL版本是`5.7`，它的默认行格式就是`Dynamic`，这俩行格式和`Compact`行格式贼像，只不过在处理`行溢出`数据时有点儿分歧，它们不会在记录的真实数据处存储字符串的前`768`个字节，而是把所有的字节都存储到其他页面中，只在记录的真实数据处存储其他页面的地址，就像这样：

![image_1c9jcd59e1mbu31gcc14q6vgsir.png-17.8kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3NnuO9jdPFbrtMLa6lvWDZKQxtLVlv9tmR6jhFGYEhFXce57Yt6EloQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9jcd59e1mbu31gcc14q6vgsir.png-17.8kB

`Compressed`行格式和`Dynamic`不同的一点是，`Compressed`行格式会把存储到其他页面的数据采用压缩算法进行压缩，以节省空间。

### CHAR(M)列的存储格式

`record_format_demo`表的`c1`、`c2`、`c4`列的类型是`VARCHAR(10)`，而`c3`列的类型是`CHAR(10)`，我们说在`Compact`行格式下只会把变成类型的列的长度逆序存到`变长字段长度列表`中，就像这样：

![image_1c9jdkga71kegkjs14o111ov1ce3kn.png-12.5kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3icQoeLhwNKtic7Utck4qfgk1lNUpen5gAwvE9GtuiaZCqJia97s23h05Aw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9jdkga71kegkjs14o111ov1ce3kn.png-12.5kB

但是这只是因为我们的`record_format_demo`表采用的是`ascii`字符集，这个字符集是一个定长字符集，也就是说表示一个字符采用固定的一个字节，如果采用变长的字符集的话，`c3`列的长度也会被存储到`变长字段长度列表`中，比如我们修改一下`record_format_demo`表的字符集：

```
mysql> ALTER TABLE record_format_demo MODIFY COLUMN c3 CHAR(10) CHARACTER SET utf8;
Query OK, 2 rows affected (0.02 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql>
```

修改该列字符集后记录的`变长字段长度列表`也发生了变化，如图：

![image_1c9jeb6defgf1o981lgfciokjl4.png-43.1kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3uBJNvJzVxfLRzRKyTN7ceCbzZ5SggPtkajauLia2ibiaic1icqHyqlxdVBA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9jeb6defgf1o981lgfciokjl4.png-43.1kB

这就意味着：对于 **CHAR(M)** 类型的列来说，当列采用的是定长字符集时，该列占用的字节数不会被加到变长字段长度列表，而如果采用变长字符集时，该列占用的字节数也会被加到变长字段长度列表。

## 总结

1. 页是`MySQL`中磁盘和内存交互的基本单位，也是`MySQL`是管理存储空间的基本单位。

2. 指定和修改行格式的语法如下：

   ```
   CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称
   ALTER TABLE 表名 ROW_FORMAT=行格式名称
   ```

3. `InnoDB`目前定义了4中行格式

4. - COMPACT行格式

     具体组成如图：

     ![image_1c9g4t114n0j1gkro2r1h8h1d1t16.png-42.4kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey37MIoP0YPpYdY7Y0TO0iaZ3a79QB7GiaHyTqJTicNBQ6Nk202h4JECicib3A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9g4t114n0j1gkro2r1h8h1d1t16.png-42.4kB

   - Redundant行格式

     具体组成如图：

     ![image_1c9h896lcuqi16081qub1v8c12jkft.png-36.2kB](https://mmbiz.qpic.cn/mmbiz_png/RLmbWWew55FVTOrSL5huibzeawNtia5ey3sTj2WHp0xqwYqgadiblaPIXIazt08Hia6Ns8qzlhkDN2Tr5HV6gdvEIw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)image_1c9h896lcuqi16081qub1v8c12jkft.png-36.2kB

   - Dynamic和Compressed行格式
     这两种行格式类似于`COMPACT行格式`，只不过在处理行溢出数据时有点儿分歧，它们不会在记录的真实数据处存储字符串的前768个字节，而是把所有的字节都存储到其他页面中，只在记录的真实数据处存储其他页面的地址。

     另外，`Compressed`行格式会把存储在其他页面中的数据压缩处理。

- 一个页一般是`16KB`，当记录中的数据太多，当前页放不下的时候，会把多余的数据存储到其他页中，这种现象称为`行溢出`。
- 对于 CHAR(M) 类型的列来说，当列采用的是定长字符集时，该列占用的字节数不会被加到变长字段长度列表，而如果采用变长字符集时，该列占用的字节数也会被加到变长字段长度列表。

 