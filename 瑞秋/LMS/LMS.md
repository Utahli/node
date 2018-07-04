# List、Set、Map的区别

[TOC]

## java集合的主要分为三种类型：

- Set（集）
- List（列表）
- Map（映射）

要深入理解集合首先要了解下我们熟悉的数组：

**数组**是大小固定的，并且同一个数组只能存放类型一样的数据（基本类型/引用类型），而JAVA集合可以存储和操作数目不固定的一组数据。 所有的JAVA集合都位于 java.util包中！ JAVA集合只能存放引用类型的数据，不能存放基本数据类型。

简单说下集合和数组的区别：(参考文章：[《Thinking In Algorithm》03.数据结构之数组](http://blog.csdn.net/speedme/article/details/18180817))

 ```java
而几乎有有的集合都是基于数组来实现的.  
因为集合是对数组做的封装,所以,数组永远比任何一个集合要快  
  
但任何一个集合,比数组提供的功能要多  
  
一：数组声明了它容纳的元素的类型，而集合不声明。这是由于集合以object形式来存储它们的元素。  
   
二：一个数组实例具有固定的大小，不能伸缩。集合则可根据需要动态改变大小。  
   
三：数组是一种可读/可写数据结构－－－没有办法创建一个只读数组。然而可以使用集合提供的ReadOnly方法，以只读方式来使用集合。该方法将返回一个集合的只读版本。
 ```

Java所有“存储及随机访问一连串对象”的做法，array是最有效率的一种。

1. 效率高，但容量固定且无法动态改变；array还有一个缺点是，无法判断其中实际存有多少元素，length只是告诉我们array的容量。


2. Java中有一个**Arrays类，专门用来操作array**。

​     arrays中拥有一组static函数，
equals()：比较两个array是否相等。array拥有相同元素个数，且所有对应元素两两相等。
fill()：将值填入array中。
sort()：用来对array进行排序。
binarySearch()：在排好序的array中寻找元素。
System.arraycopy()：array的复制。

若撰写程序时不知道究竟需要多少对象，需要在空间不足时自动扩增容量，则需要使用容器类库，array不适用。所以就要用到集合。

那我们开始讨论java中的集合。

### 集合分类：

Collection：List、Set
Map：HashMap、HashTable

### 线程安全集合类与非线程安全集合类 

1. LinkedList、ArrayList、HashSet是非线程安全的，Vector是线程安全的;
2. HashMap是非线程安全的，HashTable是线程安全的;
3. StringBuilder是非线程安全的，StringBuffer是线程安全的。

## ArrayList与LinkedList的区别和适用场景

### Arraylist：

**优点**：ArrayList是实现了基于动态数组的数据结构,因为地址连续，一旦数据存储好了，查询操作效率会比较高（在内存里是连着放的）。

**缺点**：因为地址连续， ArrayList要移动数据,所以插入和删除操作效率比较低。   

### LinkedList：

**优点**：LinkedList基于链表的数据结构,地址是任意的，所以在开辟内存空间的时候不需要等一个连续的地址，对于新增和删除操作add和remove，LinedList比较占优势。LinkedList 适用于要头尾操作或插入指定位置的场景

**缺点**：因为LinkedList要移动指针,所以查询操作性能比较低。

###  适用场景分析：

 当需要对数据进行对此访问的情况下选用ArrayList，当需要对数据进行多次增加删除修改时采用LinkedList。

## ArrayList与Vector的区别和适用场景

###  ArrayList有三个构造方法：

```java
public ArrayList(int initialCapacity)//构造一个具有指定初始容量的空列表。    
public ArrayList()//构造一个初始容量为10的空列表。    
public ArrayList(Collection<? extends E> c)//构造一个包含指定 collection 的元素的列表   
```

 ### Vector有四个构造方法： 

```java
public Vector()//使用指定的初始容量和等于零的容量增量构造一个空向量。    
public Vector(int initialCapacity)//构造一个空向量，使其内部数据数组的大小，其标准容量增量为零。 
public Vector(Collection<? extends E> c)//构造一个包含指定 collection 中的元素的向量    
public Vector(int initialCapacity,int capacityIncrement)//使用指定的初始容量和容量增量构造一个空的向量    
```

### ArrayList和Vector都是用数组实现的，主要有这么三个区别：

1. Vector是多线程安全的，线程安全就是说多线程访问同一代码，不会产生不确定的结果。而ArrayList不是，这个可以从源码中看出，Vector类中的方法很多有synchronized进行修饰，这样就导致了Vector在效率上无法与ArrayList相比；


2. 两个都是采用的线性连续空间存储元素，但是当空间不足的时候，两个类的增加方式是不同。


3. Vector可以设置增长因子，而ArrayList不可以。

4. Vector是一种老的动态数组，是线程同步的，效率很低，一般不赞成使用。

   ### 适用场景分析：

5. Vector是线程同步的，所以它也是线程安全的，而ArrayList是线程异步的，是不安全的。如果不考虑到线程的安全因素，一般用ArrayList效率比较高。

6. 如果集合中的元素的数目大于目前集合数组的长度时，在集合中使用数据量比较大的数据，用Vector有一定的优势。

## ArrayList 和 Vector

这两个类都实现了List接口(List接口继承了Collection接口).

他们都是有序集合,即存储在这两个集合中的元素的位置都是有顺序的,相当于一种动态的数组

并且其中的数据是允许重复的

ArrayList与Vector的区别

- Vector的方法都是同步的(Synchronized), 是线程安全的, 也就是线程同步的, 而ArrayList是线程序不安全的. 对于Vector&ArrayList, Hashtable&HashMap, 要记住线程安全的问题, 记住Vector与Hashtable是旧的, 是java一诞生就提供了的, 它们是线程安全的, ArrayList与HashMap是java2时才提供的, 它们是线程不安全的.

- ArrayList与Vector都有一个初始的容量大小, 当存储进它们里面的元素的个数超过了容量时, 就需要增加ArrayList与Vector的存储空间, Vector默认增长为原来两倍,而ArrayList的增长策略在文档中没有明确规定（从源代码看到的是增长为原来的1.5倍）.ArrayList与Vector都可以设置初始的空间大小, Vector还可以设置增长的空间大小, 而ArrayList没有提供设置增长空间的方法.

**总结：即Vector增长原来的一倍,ArrayList增加原来的0.5倍. Vector 线程安全, ArrayList 不是.**



**List特点：**元素有放入顺序，元素可重复 

**Set特点：**元素无放入顺序，元素不可重复，重复元素会覆盖掉，（注意：元素虽然无放入顺序，但是元素在set中的位置是有该元素的HashCode决定的，其位置其实是固定的，加入Set 的Object必须定义equals()方法 ，另外list支持for循环，也就是通过下标来遍历，也可以用迭代器，但是set只能用迭代，因为他无序，无法用下标来取得想要的值。） 

 

## HashSet与Treeset的适用场景

1. TreeSet 是二差树（红黑树的树据结构）实现的,Treeset中的数据是自动排好序的，不允许放入null值 

2. HashSet 是哈希表实现的,HashSet中的数据是无序的，可以放入null，但只能放入一个null，两者中的值都不能重复，就如数据库中唯一约束 

3. HashSet要求放入的对象必须实现HashCode()方法，放入的对象，是以hashcode码作为标识的，而具有相同内容的String对象，hashcode是一样，所以放入的内容不能重复。但是同一个类的对象可以放入不同的实

   ​

   ### 适用场景分析:

   HashSet是基于Hash算法实现的，其性能通常都优于TreeSet。为快速查找而设计的Set，我们通常都应该使用HashSet，在我们需要排序的功能时，我们才使用TreeSet。

###  HashMap与TreeMap、HashTable的区别及适用场景

1. HashMap 非线程安全  


2. HashMap：基于哈希表实现。使用HashMap要求添加的键类明确定义了hashCode()和equals()[可以重写hashCode()和equals()]，为了优化HashMap空间的使用，您可以调优初始容量和负载因子。 
3. TreeMap：非线程安全基于红黑树实现。TreeMap没有调优选项，因为该树总处于平衡状态。 

### 适用场景分析：

HashMap和HashTable:HashMap去掉了HashTable的contains方法，但是加上了containsValue()和containsKey()方法。HashTable同步的，而HashMap是非同步的，效率上比HashTable要高。HashMap允许空键值，而HashTable不允许。

HashMap：适用于Map中插入、删除和定位元素。 

Treemap：适用于按自然顺序或自定义顺序遍历键(key)。 

## 1.1 Collection接口

Collection是最基本的集合接口，声明了适用于JAVA集合（只包括Set和List）的通用方法。 Set 和List 都继承了Conllection,Map。

### 1.1.1  Collection接口的方法：

```java
boolean add(Object o)      ：向集合中加入一个对象的引用   

void clear()：删除集合中所有的对象，即不再持有这些对象的引用   
  
boolean isEmpty()    ：判断集合是否为空   
   
boolean contains(Object o) ： 判断集合中是否持有特定对象的引用   

Iterartor iterator()  ：返回一个Iterator对象，可以用来遍历集合中的元素   
   
boolean remove(Object o) ：从集合中删除一个对象的引用   
   
int size()       ：返回集合中元素的数目   
   
Object[] toArray()    ： 返回一个数组，该数组中包括集合中的所有元素 </span> 
```

 关于：Iterator() 和toArray() 方法都用于集合的所有的元素，前者返回一个Iterator对象，后者返回一个包含集合中所有元素的数组。

### 1.1.2  Iterator接口声明了如下方法： 

```java
hasNext()：判断集合中元素是否遍历完毕，如果没有，就返回true   
  
next() ：返回下一个元素   
  
remove()：从集合中删除上一个有next()方法返回的元素。 
```

## 1.2  Set(集合)

Set是最简单的一种集合。集合中的对象不按特定的方式排序，并且没有重复对象。 Set接口主要实现了两个实现类：

- HashSet： HashSet类按照哈希算法来存取集合中的对象，存取速度比较快 
- TreeSet ：TreeSet类实现了SortedSet接口，能够对集合中的对象进行排序。  

Set 的 add()方法是如何判断对象是否已经存放在集合中？ 

```java
boolean isExists=false;  

Iterator iterator=set.iterator();  

while(it.hasNext()){  
  
	String oldStr=it.next();  
 
	if(newStr.equals(oldStr)){  
  
	isExists=true;  

	}  
} 
```



### Set的功能方法 

Set具有与Collection完全一样的接口，因此没有任何额外的功能，不像前面有两个不同的List。实际上Set就是Collection,只 是行为不同。(这是继承与多态思想的典型应用：表现不同的行为。)Set不保存重复的元素(至于如何判断元素相同则较为负责) 

Set : 存入Set的每个元素都必须是唯一的，因为Set不保存重复元素。加入Set的元素必须定义equals()方法以确保对象的唯一性。Set与Collection有完全一样的接口。Set接口不保证维护元素的次序。 

- HashSet：为快速查找设计的Set。存入HashSet的对象必须定义hashCode()。 


- TreeSet： 保存次序的Set, 底层为树结构。使用它可以从Set中提取有序的序列。 

LinkedHashSet：具有HashSet的查询速度，且内部使用链表维护元素的顺序(插入的次序)。于是在使用迭代器遍历Set时，结果会按元素插入的次序显示。

 

 

## 1.3  List(列表)

List的特征是其元素以线性方式存储，集合中可以存放重复对象。 

### List接口主要实现类包括：

- ArrayList() : 代表长度可以改变得数组。可以对元素进行随机的访问，向ArrayList()中插入与删除元素的速度慢。 
- LinkedList(): 在实现中采用链表数据结构。插入和删除速度快，访问速度慢。 

对于List的随机访问来说，就是只随机来检索位于特定位置的元素。 List 的 get(int index) 方法放回集合中由参数index指定的索引位置的对象，下标从“0” 开始。

### List的功能方法 

实际上有两种List：一种是基本的ArrayList,其优点在于随机访问元素，另一种是更强大的LinkedList,它并不是为快速随机访问设计的，而是具有一套更通用的方法。

- List：次序是List最重要的特点：它保证维护元素特定的顺序。List为Collection添加了许多方法，使得能够向List中间插入与移除元素(这只推 荐LinkedList使用。)一个List可以生成ListIterator,使用它可以从两个方向遍历List,也可以从List中间插入和移除元 素。 
- ArrayList：由数组实现的List。允许对元素进行快速随机访问，但是向List中间插入与移除元素的速度很慢。ListIterator只应该用来由后向前遍历 ArrayList,而不是用来插入和移除元素。因为那比LinkedList开销要大很多。 
- LinkedList ：对顺序访问进行了优化，向List中间插入与删除的开销并不大。随机访问则相对较慢。(使用ArrayList代替。)还具有下列方 法：addFirst(), addLast(), getFirst(), getLast(), removeFirst() 和 removeLast(), 这些方法 (没有在任何接口或基类中定义过)使得LinkedList可以当作堆栈、队列和双向队列使用。

 

## 1.4 Map(映射)

Map 是一种把键对象和值对象映射的集合，它的每一个元素都包含一对键对象和值对象。 Map没有继承于Collection接口 从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象。 

Map 的常用方法： 

1.  **添加，删除操作：** 

   ```java
Object put(Object key, Object value)： 向集合中加入元素   
Object remove(Object key)： 删除与KEY相关的元素   
void putAll(Map t)：  将来自特定映像的所有元素添加给该映像   
void clear()：从映像中删除所有映射 
   ```



2. **查询操作：** 

Object get(Object key)：获得与关键字key相关的值 。Map集合中的键对象不允许重复，也就说，任意两个键对象通过equals()方法比较的结果都是false.，但是可以将任意多个键独享映射到同一个值对象上。 

 

### Map的功能方法

1. 方法put(Object key, Object value)添加一个“值”(想要得东西)和与“值”相关联的“键”(key)(使用它来查找)。方法get(Object key)返回与给定“键”相关联的“值”。可以用containsKey()和containsValue()测试Map中是否包含某个“键”或“值”。 标准的Java类库中包含了几种不同的Map：HashMap, TreeMap, LinkedHashMap, WeakHashMap, IdentityHashMap。它们都有同样的基本接口Map，但是行为、效率、排序策略、保存对象的生命周期和判定“键”等价的策略等各不相同。 
2. 执行效率是Map的一个大问题。看看get()要做哪些事，就会明白为什么在ArrayList中搜索“键”是相当慢的。而这正是HashMap提高速 度的地方。HashMap使用了特殊的值，称为“散列码”(hash code)，来取代对键的缓慢搜索。“散列码”是“相对唯一”用以代表对象的int值，它是通过将该对象的某些信息进行转换而生成的。所有Java对象都 能产生散列码，因为hashCode()是定义在基类Object中的方法。 
3. HashMap就是使用对象的hashCode()进行快速查询的。此方法能够显着提高性能。 
4. Map : 维护“键值对”的关联性，使你可以通过“键”查找“值”
5. HashMap：Map基于散列表的实现。插入和查询“键值对”的开销是固定的。可以通过构造器设置容量capacity和负载因子load factor，以调整容器的性能。 
6. LinkedHashMap： 类似于HashMap，但是迭代遍历它时，取得“键值对”的顺序是其插入次序，或者是最近最少使用(LRU)的次序。只比HashMap慢一点。而在迭代访问时发而更快，因为它使用链表维护内部次序。 
7. TreeMap ： 基于红黑树数据结构的实现。查看“键”或“键值对”时，它们会被排序(次序由Comparabel或Comparator决定)。TreeMap的特点在 于，你得到的结果是经过排序的。TreeMap是唯一的带有subMap()方法的Map，它可以返回一个子树。 
8. WeakHashMao ：弱键(weak key)Map，Map中使用的对象也被允许释放: 这是为解决特殊问题设计的。如果没有map之外的引用指向某个“键”，则此“键”可以被垃圾收集器回收。 
9. IdentifyHashMap： : 使用==代替equals()对“键”作比较的hash map。专为解决特殊问题而设计。

 

## 1.4 区别

### **1.4.1、Collection 和 Map 的区别**

容器内每个为之所存储的元素个数不同。
Collection类型者，每个位置只有一个元素。
Map类型者，持有 key-value pair，像个小型数据库。

### **1.4.2、各自旗下的子类关系**

**Collection**    

​	 --List：将以特定次序存储元素。所以取出来的顺序可能和放入顺序不同。
           	--ArrayList / LinkedList / Vector
     	--Set ： 不能含有重复的元素
           	--HashSet / TreeSet

**Map**
	--HashMap
	--HashTable
	--TreeMap

### **1.4.3、其他特征**

List，Set，Map将持有对象一律视为Object型别。

==Collection、List、Set、Map都是接口，不能实例化。==
    继承自它们的 ArrayList, Vector, HashTable, HashMap是具象class，这些才可被实例化。
vector容器确切知道它所持有的对象隶属什么型别。vector不进行边界检查。

## 总结:

1. 如果涉及到堆栈，队列等操作，应该考虑用List，对于需要快速插入，删除元素，应该使用LinkedList，如果需要快速随机访问元素，应该使用ArrayList。
2. 如果程序在单线程环境中，或者访问仅仅在一个线程中进行，考虑非同步的类，其效率较高，如果多个线程可能同时操作一个类，应该使用同步的类。
3. 在除需要排序时使用TreeSet,TreeMap外,都应使用HashSet,HashMap,因为他们 的效率更高。
4. 要特别注意对哈希表的操作，作为key的对象要正确复写equals和hashCode方法。

5. 容器类仅能持有对象引用（指向对象的指针），而不是将对象信息copy一份至数列某位置。一旦将对象置入容器内，便损失了该对象的型别信息。
6. 尽量返回接口而非实际的类型，如返回List而非ArrayList，这样如果以后需要将ArrayList换成LinkedList时，客户端代码不用改变。这就是针对抽象编程。

## 注意：

1. ==Collection没有get()方法来取得某个元素。只能通过iterator()遍历元素。==
2. Set和Collection拥有一模一样的接口。
3. List，可以通过get()方法来一次取出一个元素。使用数字来选择一堆对象中的一个，get(0)...。(add/get)
4. 一般使用ArrayList。用LinkedList构造堆栈stack、队列queue。
5. Map用 put(k,v) / get(k)，还可以使用containsKey()/containsValue()来检查其中是否含有某个key/value。HashMap会利用对象的hashCode来快速找到key。
6. Map中元素，可以将key序列、value序列单独抽取出来。
   - 使用keySet()抽取key序列，将map中的所有keys生成一个Set。
   - 使用values()抽取value序列，将map中的所有values生成一个Collection。
   - 为什么一个生成Set，一个生成Collection？那是因为，key总是独一无二的，value允许重复。





# HashSet类是如何实现添加元素保证不重复的

弄清怎么个逻辑达到元素不重复的，源码先上 
 HashSet 类中的add()方法：

```
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}123
```

类中map和PARENT的定义：

```
private transient HashMap<E,Object> map;
 // Dummy value to associate with an Object in the backing Map用来匹配Map中后面的对象的一个虚拟值
private static final Object PRESENT = new Object();123
```

put()方法：

```
public V put(K key, V value) {
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key.hashCode());
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
        modCount++;
        addEntry(hash, key, value, i);
        return null;
}
```

可以看到for循环中，遍历table中的元素， 
 **1，如果hash码值不相同，说明是一个新元素，存；** 

如果没有元素和传入对象（也就是add的元素）的hash值相等，那么就认为这个元素在table中不存在，将其添加进table；

**2（1），如果hash码值相同，且equles判断相等，说明元素已经存在，不存；**

**2（2），如果hash码值相同，且equles判断不相等，说明元素不存在，存；**

如果有元素和传入对象的hash值相等，那么，继续进行equles()判断，如果仍然相等，那么就认为传入元素已经存在，不再添加，结束，否则仍然添加；

------

**可见hashcode()和equles()在此显得很关键了**，下面就来解读一下hashcode和equles：

首先要明确：只通过hash码值来判断两个对象时否相同合适吗？答案是不合适的，因为*有可能两个不同的对象的hash码值相同*； 
 **什么是hash码值？** 
 在java中存在一种hash表结构，它通过一个算法，计算出的结果就是hash码值；这个算法叫hash算法； 
 **hash算法是怎么计算的呢？** 
 是通过对象中的成员来计算出来的结果； 
 如果成员变量是基本数据类型的值， 那么用这个值 直接参与计算； 
 如果成员变量是引用数据类型的值，那么获取到这个成员变量的哈希码值后，再参数计算

如:新建一个Person对象，重写hashCode方法

```
public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + age;
        result = prime * result + ((name == null) ? 0 : name.hashCode());
        return result;
    }1234567
```

可以看出，Person对象内两个参数name，age，hash码值是这两者计算后的记过，那么完全有可能两个对象name，age都不同，hash码值相同； 
 下面看下equles()方法：

```
public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        Person other = (Person) obj;
        if (age != other.age)
            return false;
        if (name == null) {
            if (other.name != null)
                return false;
        } else if (!name.equals(other.name))
            return false;
        return true;
    }1234567891011121314151617
```

equles方法内部是分别对name，age进行判断，是否相等。

------

**综合上述，在向hashSet中add()元素时，判断元素是否存在的依据，不仅仅是hash码值就能够确定的，同时还要结合equles方法。**





#### **1、 HashMap计算key的hash值时调用单独的方法，在该方法中会判断key是否为null，如果是则返回0；而Hashtable中则直接调用key的hashCode()方法，因此如果key为null，则抛出空指针异常。**

#### 　　**2、 HashMap将键值对添加进数组时，不会主动判断value是否为null；而Hashtable则首先判断value是否为null。**

#### 　　**3、以上原因主要是由于Hashtable继承自Dictionary，而HashMap继承自AbstractMap。**

#### 　　**4、虽然ConcurrentHashMap也继承自AbstractMap，但是其也过滤掉了key或value为null的键值对。**

使用iterator遍历

```java

private abstract class HashIterator<E> implements Iterator<E> {
        HashMapEntry<K,V> next;        // next entry to return
        int expectedModCount;   // For fast-fail
        int index;              // current slot
        HashMapEntry<K,V> current;     // current entry
 
        HashIterator() {
            expectedModCount = modCount;
            if (size > 0) { // advance to first entry
                HashMapEntry[] t = table;
                while (index < t.length && (next = t[index++]) == null)
                    ;
            }
        }
 
        public final boolean hasNext() {
            return next != null;
        }
 
        final Entry<K,V> nextEntry() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            HashMapEntry<K,V> e = next;
            if (e == null)
                throw new NoSuchElementException();
 
            if ((next = e.next) == null) {
                HashMapEntry[] t = table;
                while (index < t.length && (next = t[index++]) == null)
                    ;
            }
            current = e;
            return e;
        }
 
        public void remove() {
            if (current == null)
                throw new IllegalStateException();
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            Object k = current.key;
            current = null;
            HashMap.this.removeEntryForKey(k);
            expectedModCount = modCount;
        }
    }

```



# sleep() 和 wait() 的区别

举个例子

```
sleep(1000)
```

会把把线程放到一边, 直到**整整**一秒之后才再次启动

```
wait(1000)
```

则是把线程放到一边**至多**一秒. 如果碰到 notify() 或者 notifyAll() 就会提前启动.

而且 wait() 方法是在 Object 类里. 而 sleep() 是在 Thread 类里.