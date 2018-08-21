 ==在使用**left join**时，on and和on where条件的区别如下：==  
==1、on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。==  
==2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉，on后的条件用来生成左右表关联的临时表，where后的条件对临时表中的记录进行过滤。==

实践是检验真理的唯一标准，接下来用实验来证明一下：

先上表结构以及数据：

```
SQL> create table A (id int, type int);
SQL> select * from A;
        ID       TYPE
---------- ----------
         1          1
         2          1
         3          2
 
SQL> create table B(id int ,class int);
SQL> select * from B;
 
        ID      CLASS
---------- ----------
         1          1
         2          2

```

①

```
SQL> select * from A left join B on A.id = B.id where A.type = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1
         2          1          2          2

```

根据上面那段话的解释，where字句是在生成临时表以后再进行过滤的，也就是可以理解为就是一个左连接：select * from A left join B on A.id = B.id;

其运行结果如下：

```
SQL> select * from A left join B on A.id = b.id;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1
         2          1          2          2
         3          2

```

然后加上where A.type = 1对临时表进行过滤，除掉A.type不为1的，显然结果正确。

②

```
SQL> select * from A left join B on A.id = B.id and A.type = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1
         2          1          2          2
         3          2

```

因为左连接不管on and语句是否为真都必须返回左表所有的记录，所以and A.type=1;没有起到任何作用。

 

③

```
SQL> select * from A left join B on A.id = B.id and B.class = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1
         3          2
         2          1

```

根据上面那段话的解释：on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。显然左连接再加上新的条件:B.class = 1筛选掉第二行记录，结果正确。

④

```
SQL> select * from A left join B on A.id = B.id where B.class = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1

```

原因通①。（①，②）和（③，④）就是关键词on and和on where的区别，但结果却完全不同。综上三个例子，

where是生成临时表以后再进行过滤，对左右表都进行筛选。而and后面的语句如果是对left join中的左表进行过滤将不起任何作用，对右表进行过滤的话，那么左表还是返回所有行，只是右表会被过滤掉一部分行。

再来看看内连接inner join  on and和 on where的区别：

由于刚开始表的数据不是太适合，所以先稍微更新一下，这样更好观察inner join和left join在and和where的不同之处。

```
SQL> update A set type = 2 where id = 1;
 
已更新 1 行。
 
SQL> select * from A;
 
        ID       TYPE
---------- ----------
         1          2
         2          1
         3          2
 
SQL> select * from B;
 
        ID      CLASS
---------- ----------
         1          1
         2          2

```

先看看and的：

⑤

```
SQL> select * from A inner join B on A.id = B.id and A.type = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         2          1          2          2

```

显然输出结果与左连接的不一样，先与没有and的内连接比较一下：

```
SQL> select * from A inner join B on A.id = B.id;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          2          1          1
         2          1          2          2

```

显然如果按左连接的逻辑，这个结果就是错误的。但这是Oracle输出的，而不是我瞎打的，显然在内连接时与左连接不同了。这里on and条件和on where条件一样对生成以后的临时表同样会被过滤。显然A表id为1的type不为1，所以它被过滤了。

再上几组来验证一下上面这个猜想是否是正确的：

⑥

再来看看内连接的where：

```
SQL> select * from A inner join B on A.id = B.id where A.type = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         2          1          2          2

```

这个也是和左连接一样生成临时表然后进行过滤，不作解释。



⑦

```
SQL> select * from A inner join B on A.id = B.id and B.class = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          2          1          1

```

⑧

```
SQL> select * from A inner join B on A.id = B.id where B.class = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          2          1          1

```

对比发现：（⑤，⑥）和（⑦，⑧）结果都一样，也就是说**==内连接inner join on and 或者on where不管是对左表还是右表进行过滤，实际都是在生成临时表以后再进行过滤的，而且对左表和右表都起作用，这与左连接left join有本质的区别！！！==**

最后上俩没有使用连接语句的例子：

```
</pre><p></p><pre name="code" class="sql">SQL> select * from A,B where A.id = B.id and A.type = 1;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1
         2          1          2          2
 
 
 
SQL> select * from A,B where A.type = 1 and A.id = B.id;
 
        ID       TYPE         ID      CLASS
---------- ---------- ---------- ----------
         1          1          1          1
         2          1          2          2

```

 

比较简单，不作解释。

总结一下：

​        ==在使用left join时，on和where条件的区别如下：==  

==1、on条件是在生成临时表时使用的条件，它不管on中的条件是否为真，都会返回左边表中的记录。（实际上左连接中如果and语句是对左表进行过滤的，那么不管真假都不起任何作用。如果是对右表过滤的，那么左表所有记录都返回，右表筛选以后再与左表连接返回）==  
==2、where条件是在临时表生成好后，再对临时表进行过滤的条件。这时已经没有left join的含义（必须返回左边表的记录）了，条件不为真的就全部过滤掉，on后的条件用来生成左右表关联的临时表，where后的条件对临时表中的记录进行过滤。==

​        ==**在使用inner join时，不管是对左表还是右表进行筛选，on and和on where都会对生成的临时表进行过滤。**==