# List、Set、Map的区别



## java集合的主要分为三种类型：

- Set（集）
- List（列表）
- Map（映射）

要深入理解集合首先要了解下我们熟悉的数组：

**数组**是大小固定的，并且同一个数组只能存放类型一样的数据（基本类型/引用类型），而JAVA集合可以存储和操作数目不固定的一组数据。 所有的JAVA集合都位于 java.util包中！ ==JAVA集合只能存放引用类型的数据，不能存放基本数据类型。==

==简单说下集合和数组的区别：==(参考文章：[《Thinking In Algorithm》03.数据结构之数组](http://blog.csdn.net/speedme/article/details/18180817))

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

​ **arrays中拥有一组static函数，**
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

# Interface Iterable

==迭代器接口，这是Collection类的父接口。实现这个Iterable接口的对象允许使用foreach进行遍历，也就是说，所有的Collection集合对象都具有"foreach可遍历性"。这个Iterable接口只==
==有一个方法: iterator()。它返回一个代表当前集合对象的泛型<T>迭代器，用于之后的遍历操作==

## 1.1 Collection接口

Collection是最基本的集合接口，声明了适用于JAVA集合（只包括Set和List）的通用方法。 Set 和List 都继承了Conllection,Map。

### 1.1.1  Collection接口的方法（8个）：

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

![1531329148160](D:\workspace\Github\node\瑞秋\LMS\assets\1531329148160.png)

 关于：Iterator() 和toArray() 方法都用于集合的所有的元素，前者返回一个Iterator对象，后者返回一个包含集合中所有元素的数组。

### 1.1.2  Iterator接口声明了如下方法：

```java
hasNext()：判断集合中元素是否遍历完毕，如果没有，就返回true   
  
next() ：返回下一个元素   
  
remove()：从集合中删除上一个有next()方法返回的元素。 
```

## 1.2  Set(集合)

![1531329167493](D:\workspace\Github\node\瑞秋\LMS\assets\1531329167493.png)

Set是最简单的一种集合。==集合中的对象不按特定的方式排序，并且没有重复对象。== 在使用Set集合的时候，应该注意两点：

**1) 为Set集合里的元素的实现类实现一个有效的equals(Object)方法、**

**2) 对Set的构造函数，传入的Collection参数不能包含重复的元素**

Set接口主要实现了两个实现类：

- HashSet： HashSet类按照哈希算法来存取集合中的对象，存取速度比较快 
- TreeSet ：TreeSet类实现了SortedSet接口，能够对集合中的对象进行排序。  



### Set的功能方法

==Set : 存入Set的每个元素都必须是唯一的，因为Set不保存重复元素。加入Set的元素必须定义equals()方法以确保对象的唯一性。Set与Collection有完全一样的接口。Set接口不保证维护元素的次序。== 

#### 1.1) HashSet

​     **HashSet是Set接口的典型实现，==HashSet使用HASH算法来存储集合中的元素（底层实现为HashMap），==因此具有良好的存取和查找性能。**
     值得主要的是，HashSet集合判断两个元素相等的标准是两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法的返回值相等

#### 1.1.1) LinkedHashSet

​         ==**LinkedHashSet集合也是根据元素的hashCode值来决定元素的存储位置，但和HashSet不同的是，它同时使用链表维护元素的次序，这样使得元素看起来是以插入的顺序保存的。**==
　　当遍历LinkedHashSet集合里的元素时，LinkedHashSet将会按元素的添加顺序来访问集合里的元素。
        LinkedHashSet需要维护元素的插入顺序，因此性能略低于HashSet的性能，但在迭代访问Set里的全部元素时(遍历)将有很好的性能(链表很适合进行遍历)

## 1.2) SortedSet

​     **此接口主要用于排序操作，即实现此接口的子类都属于排序的子类**
         1.2.1) TreeSet
         TreeSet是SortedSet接口的实现类，TreeSet可以确保集合元素处于排序状态
    	 1.3) EnumSet
     EnumSet是一个专门为枚举类设计的集合类，EnumSet中所有元素都必须是指定枚举类型的枚举值，该枚举类型在创建EnumSet时显式、或隐式地指定。EnumSet的集合元素也是有序的，它们以枚举值在Enum类内的定义顺序来决定集合元素的顺序

 

## 1.3  List(列表)

==List的特征是其元素以线性方式存储，集合中可以存放重复对象。== 

![1531329264196](D:\workspace\Github\node\瑞秋\LMS\assets\1531329264196.png)



### List接口主要实现类包括：

- ArrayList() : 代表长度可以改变得数组。可以对元素进行随机的访问，向ArrayList()中插入与删除元素的速度慢。 
- LinkedList(): 在实现中采用链表数据结构。插入和删除速度快，访问速度慢。 

对于List的随机访问来说，就是只随机来检索位于特定位置的元素。 List 的 get(int index) 方法放回集合中由参数index指定的索引位置的对象，下标从“0” 开始。

### List的功能方法 

实际上有两种List：一种是基本的ArrayList,其优点在于随机访问元素，另一种是更强大的LinkedList,它并不是为快速随机访问设计的，而是具有一套更通用的方法。

- List：次序是List最重要的特点：它保证维护元素特定的顺序。List为Collection添加了许多方法，使得能够向List中间插入与移除元素(这只推 荐LinkedList使用。)一个List可以生成ListIterator,使用它可以从两个方向遍历List,也可以从List中间插入和移除元 素。 
- ArrayList：由数组实现的List。允许对元素进行快速随机访问，但是向List中间插入与移除元素的速度很慢。ListIterator只应该用来由后向前遍历 ArrayList,而不是用来插入和移除元素。因为那比LinkedList开销要大很多。 
- LinkedList ：对顺序访问进行了优化，向List中间插入与删除的开销并不大。随机访问则相对较慢。(使用ArrayList代替。)还具有下列方 法：addFirst(), addLast(), getFirst(), getLast(), removeFirst() 和 removeLast(), 这些方法 (没有在任何接口或基类中定义过)使得LinkedList可以当作堆栈、队列和双向队列使用。

 Queue

 Queue用于模拟"队列"这种数据结构(先进先出 FIFO)。队列的头部保存着队列中存放时间最长的元素，队列的尾部保存着队列中存放时间最短的元素。新元素插入(offer)到队列的尾部，
　　 访问元素(poll)操作会返回队列头部的元素，队列不允许随机访问队列中的元素。结合生活中常见的排队就会很好理解这个概念
     3.1) PriorityQueue
     PriorityQueue并不是一个比较标准的队列实现，PriorityQueue保存队列元素的顺序并不是按照加入队列的顺序，而是按照队列元素的大小进行重新排序，这点从它的类名也可以看出来
     3.2) Deque
     Deque接口代表一个"双端队列"，双端队列可以同时从两端来添加、删除元素，因此Deque的实现类既可以当成队列使用、也可以当成栈使用
         3.2.1) ArrayDeque
         是一个基于数组的双端队列，和ArrayList类似，它们的底层都采用一个动态的、可重分配的Object[]数组来存储集合元素，当集合元素超出该数组的容量时，系统会在底层重新分配一个Object[]数组来存储集合元素
         3.2.2) LinkedList

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

![1531329316033](D:\workspace\Github\node\瑞秋\LMS\assets\1531329316033.png)

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

 1.1) LinkedHashMap

​     LinkedHashMap也使用双向链表来维护key-value对的次序，该链表负责维护Map的迭代顺序，与key-value对的插入顺序一致(注意和TreeMap对所有的key-value进行排序进行区
分)

### 3) SortedMap

 正如Set接口派生出SortedSet子接口，SortedSet接口有一个TreeSet实现类一样，Map接口也派生出一个SortedMap子接口，SortedMap接口也有一个TreeMap实现类
     3.1) TreeMap
     ==TreeMap就是一个红黑树数据结构，每个key-value对即作为红黑树的一个节点。TreeMap存储key-value对(节点)时，需要根据key对节点进行排序。TreeMap可以保证所有的 key-value对处于有序状态。同样，TreeMap也有两种排序方式: 自然排序、定制排序==

### 4) WeakHashMap

 WeakHashMap与HashMap的用法基本相似。区别在于，HashMap的key保留了对实际对象的"强引用"，这意味着只要该HashMap对象不被销毁，该HashMap所引用的对象就不会被垃圾回收。
　　但WeakHashMap的key只保留了对实际对象的弱引用，这意味着如果WeakHashMap对象的key所引用的对象没有被其他强引用变量所引用，则这些key所引用的对象可能被垃圾回收，当垃
　　圾回收了该key所对应的实际对象之后，WeakHashMap也可能自动删除这些key所对应的key-value对

### 5) IdentityHashMap

 IdentityHashMap的实现机制与HashMap基本相似，在IdentityHashMap中，当且仅当两个key严格相等(key1 == key2)时，IdentityHashMap才认为两个key相等

### 6) EnumMap

 EnumMap是一个与枚举类一起使用的Map实现，EnumMap中的所有key都必须是单个枚举类的枚举值。创建EnumMap时必须显式或隐式指定它对应的枚举类。EnumMap根据key的自然顺序
　　(即枚举值在枚举类中的定义顺序)



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
==vector容器确切知道它所持有的对象隶属什么型别。vector不进行边界检查。==

## 总结:

1. 如果涉及到堆栈，队列等操作，应该考虑用List，对于需要快速插入，删除元素，应该使用LinkedList，如果需要快速随机访问元素，应该使用ArrayList。
2. 如果程序在单线程环境中，或者访问仅仅在一个线程中进行，考虑非同步的类，其效率较高，如果多个线程可能同时操作一个类，应该使用同步的类。
3. 在除需要排序时使用TreeSet,TreeMap外,都应使用HashSet,HashMap,因为他们 的效率更高。
4. 要特别注意对哈希表的操作，作为key的对象要正确复写equals和hashCode方法。

5. ==容器类仅能持有对象引用（指向对象的指针），而不是将对象信息copy一份至数列某位置。一旦将对象置入容器内，便损失了该对象的型别信息。==
6. ==尽量返回接口而非实际的类型，如返回List而非ArrayList，这样如果以后需要将ArrayList换成LinkedList时，客户端代码不用改变。这就是针对抽象编程。==

## 注意：

1. ==Collection没有get()方法来取得某个元素。只能通过iterator()遍历元素。==
2. Set和Collection拥有一模一样的接口。
3. List，可以通过get()方法来一次取出一个元素。使用数字来选择一堆对象中的一个，get(0)...。(add/get)
4. 一般使用ArrayList。==用LinkedList构造堆栈stack、队列queue。==
5. Map用 put(k,v) / get(k)，还可以使用containsKey()/containsValue()来检查其中是否含有某个key/value。HashMap会利用对象的hashCode来快速找到key。
6. Map中元素，可以将key序列、value序列单独抽取出来。
   - 使用keySet()抽取key序列，将map中的所有keys生成一个Set。
   - 使用values()抽取value序列，将map中的所有values生成一个Collection。
   - 为什么一个生成Set，一个生成Collection？那是因为，key总是独一无二的，value允许重复。

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

### 适用场景分析：

 当需要对数据进行对此访问的情况下选用ArrayList，当需要对数据进行多次增加删除修改时采用LinkedList。

## ArrayList与Vector的区别和适用场景

### ArrayList有三个构造方法：

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

1. ==两个都是采用的线性连续空间存储元素，但是当空间不足的时候，两个类的增加方式是不同。==

1. ==Vector可以设置增长因子，而ArrayList不可以。==

2. Vector是一种老的动态数组，是线程同步的，效率很低，一般不赞成使用。

   ### 适用场景分析：

3. Vector是线程同步的，所以它也是线程安全的，而ArrayList是线程异步的，是不安全的。如果不考虑到线程的安全因素，一般用ArrayList效率比较高。

4. 如果集合中的元素的数目大于目前集合数组的长度时，在集合中使用数据量比较大的数据，用Vector有一定的优势。

## ArrayList 和 Vector

==这两个类都实现了List接口(List接口继承了Collection接口).==

他们都是有序集合,即存储在这两个集合中的元素的位置都是有顺序的,相当于一种动态的数组

并且其中的数据是允许重复的

ArrayList与Vector的区别

- Vector的方法都是同步的(Synchronized), 是线程安全的, 也就是线程同步的, 而ArrayList是线程序不安全的. ==对于Vector&ArrayList, Hashtable&HashMap, 要记住线程安全的问题, 记住Vector与Hashtable是旧的, 是java一诞生就提供了的, 它们是线程安全的, ArrayList与HashMap是java2时才提供的, 它们是线程不安全的.==
- ==ArrayList与Vector都有一个初始的容量大小, 当存储进它们里面的元素的个数超过了容量时, 就需要增加ArrayList与Vector的存储空间, Vector默认增长为原来两倍,而ArrayList的增长策略在文档中没有明确规定（从源代码看到的是增长为原来的1.5倍）.ArrayList与Vector都可以设置初始的空间大小, Vector还可以设置增长的空间大小, 而ArrayList没有提供设置增长空间的方法.==

==**总结：即Vector增长原来的一倍,ArrayList增加原来的0.5倍. Vector 线程安全, ArrayList 不是.**==



**List特点：**元素有放入顺序，元素可重复 

**Set特点：**元素无放入顺序，元素不可重复，重复元素会覆盖掉，（注意：元素虽然无放入顺序，但是元素在set中的位置是有该元素的HashCode决定的，其位置其实是固定的，加入Set 的Object必须定义equals()方法 ，另外==list支持for循环，也就是通过下标来遍历，也可以用迭代器，但是set只能用迭代，因为他无序，无法用下标来取得想要的值。）== 

 

## HashSet与Treeset的适用场景

1. ==TreeSet 是二差树（红黑树的树据结构）实现的,Treeset中的数据是自动排好序的，不允许放入null值== 

2. HashSet 是哈希表实现的,HashSet中的数据是无序的，可以放入null，但只能放入一个null，两者中的值都不能重复，就如数据库中唯一约束 

3. HashSet要求放入的对象必须实现HashCode()方法，放入的对象，是以hashcode码作为标识的，而具有相同内容的String对象，hashcode是一样，所以放入的内容不能重复。但是同一个类的对象可以放入不同的实

   

   ### 适用场景分析:

   HashSet是基于Hash算法实现的，其性能通常都优于TreeSet。为快速查找而设计的Set，我们通常都应该使用HashSet，在我们需要排序的功能时，我们才使用TreeSet。

### HashMap与TreeMap、HashTable的区别及适用场景

1. HashMap 非线程安全  

1. HashMap：基于哈希表实现。使用HashMap要求添加的键类明确定义了hashCode()和equals()[可以重写hashCode()和equals()]，为了优化HashMap空间的使用，您可以调优初始容量和负载因子。 
2. ==TreeMap：非线程安全基于红黑树实现。TreeMap没有调优选项，因为该树总处于平衡状态。== 

### 适用场景分析：

==HashMap和HashTable:HashMap去掉了HashTable的contains方法，但是加上了containsValue()和containsKey()方法。HashTable同步的，而HashMap是非同步的，效率上比HashTable要高。HashMap允许空键值，而HashTable不允许。==

HashMap：适用于Map中插入、删除和定位元素。 

Treemap：适用于按自然顺序或自定义顺序遍历键(key)。 

# HashMap（Java 7）的实现原理



# **一、HashMap的定义和构造函数**

```
public class HashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable123
```

　　HashMap继承自AbstractMap，AbstractMap是Map接口的骨干实现，AbstractMap中实现了Map中最重要最常用和方法，这样HashMap继承AbstractMap就不需要实现Map的所有方法，让HashMap减少了大量的工作。 
而在这里仍然实现Map结构，没有什么作用，应该是为了让map的层次结构更加清晰

### **HashMap的成员变量**

　　int DEFAULT_INITIAL_CAPACITY = 16：默认的初始容量为16 
　　int MAXIMUM_CAPACITY = 1 << 30：最大的容量为 2 ^ 30 
　　float DEFAULT_LOAD_FACTOR = 0.75f：默认的加载因子为 0.75f 
　　Entry< K,V>[] table：Entry类型的数组，HashMap用这个来维护内部的数据结构，它的**长度由容量决定** 
　　int size：HashMap的大小 
　　==int threshold：HashMap的极限容量，**扩容临界点（容量和加载因子的乘积）**==

　　谨记这些成员变量，在HashMap内部经常看到

### **HashMap的四个构造函数**

　　public HashMap()：构造一个具有默认初始容量 (16) 和默认加载因子 (0.75) 的空 HashMap 
　　public HashMap(int initialCapacity)：构造一个带指定初始容量和默认加载因子 (0.75) 的空 HashMap 
　　public HashMap(int initialCapacity, float loadFactor)：构造一个带指定初始容量和加载因子的空 HashMap 
　　public HashMap(Map< ? extends K, ? extends V> m)：构造一个映射关系与指定 Map 相同的新 HashMap

　　这里有两个很重要的参数：**initialCapacity**（初始容量）、**loadFactor**（加载因子），看看JDK中的解释： 
　　==HashMap 的实例有两个参数影响其性能：**初始容量 和加载因子**。== 
　　**容量** ：是哈希表中桶的数量，**初始容量只是哈希表在创建时的容量**，实际上就是Entry< K,V>[] table的容量 
　　**加载因子** ：是哈希表在**其容量自动增加之前可以达到多满的一种尺度**。它衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。对于使用链表法的散列表来说，查找一个元素的平均时间是O(1+a)，因此如果==负载因子越大，对空间的利用更充分，然而后果是查找效率的降低；如果负载因子太小，那么散列表的数据将过于稀疏，对空间造成严重浪费。==系统默认负载因子为0.75，一般情况下我们是无需修改的。 
　　==当哈希表中的条目数超出了加载因子与当前容量的乘积时，则要对该哈希表进行 rehash 操作（即重建内部数据结构），从而哈希表将具有大约**两倍的桶数**。==

# **二、HashMap的数据结构**

　　我们知道在Java中最常用的两种结构是数组和模拟指针(引用)，几乎所有的数据结构都可以利用这两种来组合实现，HashMap也是如此。实际上HashMap是一个“链表散列”，HashMap底层实现还是数组，只是数组的每一项都是一条链。其中参数initialCapacity就代表了该数组的长度。下面为HashMap构造函数的源码： 

```
    public HashMap(int initialCapacity, float loadFactor) {
        //容量不能小于0
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +                                           initialCapacity);
        //容量不能超出最大容量
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        //加载因子不能<=0 或者 为非数字
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);

        //计算出大于初始容量的最小 2的n次方作为哈希表table的长度，下面会说明为什么要这样
        int capacity = 1;
        while (capacity < initialCapacity)
            capacity <<= 1;

        this.loadFactor = loadFactor;
        //设置HashMap的容量极限，当HashMap的容量达到该极限时就会进行扩容操作
        threshold = (int)Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);
        //创建Entry数组
        table = new Entry[capacity];
        useAltHashing = sun.misc.VM.isBooted() &&
                (capacity >= Holder.ALTERNATIVE_HASHING_THRESHOLD);
        init();
    }
```

　　==可以看到，这个构造函数主要做的事情就是：== 
　　1. 对传入的 容量 和 加载因子进行判断处理 
　　2. 设置HashMap的容量极限 
　　==3. **计算出大于初始容量的最小 2的n次方作为哈希表table的长度，然后用该长度创建Entry数组（table），这个是最核心的**==

　　可以发现，一个HashMap对应一个Entry数组，来看看Entry这个元素的内部结构： 

```
    static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;

        /**
         * Creates new entry.
         */
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        }
```

　　Entry是HashMap的一个内部类，它也是维护着一个key-value映射关系，除了key和value，还有**next引用（该引用指向当前table位置的链表），hash值（用来确定每一个Entry链表在table中位置）**

# **三、HashMap的存储实现put（K,V）**

```
    public V put(K key, V value) {
        //如果key为空的情况
        if (key == null)
            return putForNullKey(value);
        //计算key的hash值
        int hash = hash(key);
        //计算该hash值在table中的下标
        int i = indexFor(hash, table.length);
        //对table[i]存放的链表进行遍历
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            //判断该条链上是否有hash值相同的(key相同)  
            //若存在相同，则直接覆盖value，返回旧value
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        //修改次数+1
        modCount++;
        //把当前key，value添加到table[i]的链表中
        addEntry(hash, key, value, i);
        return null;
    }
```

　　从上面的过程中，我们起码可以发现两点： 
　　1. 如果为null，则调用putForNullKey：**这就是为什么HashMap可以用null作为键的原因**，来看看HashMap是如何处理null键的： 

```
    private V putForNullKey(V value) {
        //查找链表中是否有null键
        for (Entry<K,V> e = table[0]; e != null; e = e.next) {
            if (e.key == null) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
        modCount++;
        //如果链中查找不到，则把该null键插入
        addEntry(0, null, value, 0);
        return null;
    }
```

```
    void addEntry(int hash, K key, V value, int bucketIndex) {
        if ((size >= threshold) && (null != table[bucketIndex])) {
            resize(2 * table.length);
            //这一步就是对null的处理，如果key为null，hash值为0，也就是会插入到哈希表的表头table[0]的位置
            hash = (null != key) ? hash(key) : 0;
            bucketIndex = indexFor(hash, table.length);
        }

        createEntry(hash, key, value, bucketIndex);
    }
```

　　2. ==如果链中存在该key，则用传入的value覆盖掉旧的value，同时把旧的value返回：**这就是为什么HashMap不能有两个相同的key的原因**==

　　对于hash操作，最重要也是最困难的就是如何通过确定hash的位置，我们来看看HashMap的做法： 
　　首先求得key的hash值：hash(key)

```
    final int hash(Object k) {
        int h = 0;
        if (useAltHashing) {
            if (k instanceof String) {
                return sun.misc.Hashing.stringHash32((String) k);
            }
            h = hashSeed;
        }

        h ^= k.hashCode();
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }12345678910111213
```

　　这是一个数学计算，可以不用深入，关键是下面这里： 
　　计算该hash值在table中的下标

```
    static int indexFor(int h, int length) {
        return h & (length-1);
    }123
```

对于HashMap的table而言，数据分布需要均匀（最好每项都只有一个元素，这样就可以直接找到），不能太紧也不能太松，太紧会导致查询速度慢，太松则浪费空间。计算hash值后，怎么才能保证table元素分布均与呢？我们会想到取模，但是由于取模的消耗较大，而HashMap是通过&运算符（按位与操作）来实现的：h & (length-1)

　　在构造函数中存在：capacity <<= 1，这样做总是能够保证HashMap的底层数组长度为2的n次方。==当length为2的n次方时，h&(length - 1)就相当于对length取模，而且速度比直接取模快得多，这是HashMap在速度上的一个优化。==至于为什么是2的n次方下面解释。 
　　我们回到indexFor方法，该方法仅有一条语句：h&(length - 1)，这句话除了上面的取模运算外还有一个非常重要的责任：==均匀分布table数据和充分利用空间==。 
　　这里我们假设length为16(2^n)和15，h为5、6、7。 
　　![](D:\workspace\Github\node\瑞秋\LMS\assets\20170209131832298.jpg)
　　当length-1 = 14时，6和7的结果一样，这样表示他们在table存储的位置是相同的，也就是产生了碰撞，6、7就会在一个位置形成链表，这样就会导致查询速度降低详细地看看当length-1 = 14 时的情况： 
　　![](D:\workspace\Github\node\瑞秋\LMS\assets\20170209132046754.jpg)
　　可以看到，这样发生发生的碰撞是非常多的，1,3,5,7,9,11,13都没有存放数据，空间减少，进一步增加碰撞几率，这样就会导致查询速度慢， 
　　==**分析一下：当length-1 = 14时，二进制的最后一位是0，在&操作时，一个为0，无论另一个为1还是0，最终&操作结果都是0，这就造成了结果的二进制的最后一位都是0，这就导致了所有数据都存储在2的倍数位上，所以说，所以说当length = 2^n时，不同的hash值发生碰撞的概率比较小，这样就会使得数据在table数组中分布较均匀，查询速度也较快。**== 
　　然后我们来看看计算了hash值，并用该hash值来求得哈希表中的索引值之后，如何把该key-value插入到该索引的链表中： 
　　调用 addEntry(hash, key, value, i) 方法： 

```
    void addEntry(int hash, K key, V value, int bucketIndex) {
        //如果size大于极限容量，将要进行重建内部数据结构操作，之后的容量是原来的两倍，并且重新设置hash值和hash值在table中的索引值
        if ((size >= threshold) && (null != table[bucketIndex])) {
            resize(2 * table.length);
            hash = (null != key) ? hash(key) : 0;
            bucketIndex = indexFor(hash, table.length);
        }
        //真正创建Entry节点的操作
        createEntry(hash, key, value, bucketIndex);
    }12345678910
```

```
    void createEntry(int hash, K key, V value, int bucketIndex) {
        Entry<K,V> e = table[bucketIndex];
        table[bucketIndex] = new Entry<>(hash, key, value, e);
        size++;
    }12345
```

　　**首先取得bucketIndex位置的Entry头结点，并创建新节点，把该新节点插入到链表中的头部，该新节点的next指针指向原来的头结点** 
　　**这里有两点需要注意：** 
　　**一、链的产生** 
　　==**这是一个非常优雅的设计。系统总是将新的Entry对象添加到bucketIndex处。如果bucketIndex处已经有了对象，那么新添加的Entry对象将指向原有的Entry对象，形成一条Entry链，但是若bucketIndex处没有Entry对象，也就是e\==null,那么新添加的Entry对象指向null，也就不会产生Entry链了。**== 
　　**二、扩容问题** 
　　==**还记得HashMap中的一个变量吗，threshold，这是容器的容量极限，还有一个变量size，这是指HashMap中键值对的数量，也就是node的数量**==

```
threshold = (int)Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);1
```

　　什么时候发生扩容？ 
　　==当不断添加key-value，size大于了容量极限threshold时，会发生扩容== 
　　如何扩容？ 
　　==扩容发生在resize方法中，也就是扩大数组（桶）的数量==

扩容(resize)就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素。当然==Java里的数组是无法自动扩容的，方法是使用一个新的数组代替已有的容量小的数组，==就像我们用一个小桶装水，如果想装更多的水，就得换大水桶。

我们分析下resize的源码，鉴于JDK1.8融入了红黑树，较复杂，为了便于理解我们仍然使用JDK1.7的代码，好理解一些，本质上区别不大，具体区别后文再说。

```
 void resize(int newCapacity) {   //传入新的容量
     Entry[] oldTable = table;    //引用扩容前的Entry数组
     int oldCapacity = oldTable.length;         
     if (oldCapacity == MAXIMUM_CAPACITY) {  //扩容前的数组大小如果已经达到最大(2^30)了
         threshold = Integer.MAX_VALUE; //修改阈值为int的最大值(2^31-1)，这样以后就不会扩容了
         return;
     }
  
     Entry[] newTable = new Entry[newCapacity];  //初始化一个新的Entry数组
     transfer(newTable);                         //！！将数据转移到新的Entry数组里
     table = newTable;                           //HashMap的table属性引用新的Entry数组
     threshold = (int)(newCapacity * loadFactor);//修改阈值
 }

```

这里就是使用一个容量更大的数组来代替已有的容量小的数组，transfer()方法将原有Entry数组的元素拷贝到新的Entry数组里。

```
 void transfer(Entry[] newTable) {
     Entry[] src = table;                   //src引用了旧的Entry数组
     int newCapacity = newTable.length;
     for (int j = 0; j < src.length; j++) { //遍历旧的Entry数组
         Entry<K,V> e = src[j];             //取得旧Entry数组的每个元素
         if (e != null) {
             src[j] = null;//释放旧Entry数组的对象引用（for循环后，旧的Entry数组不再引用任何对象）
             do {
                 Entry<K,V> next = e.next;
                 int i = indexFor(e.hash, newCapacity); //！！重新计算每个元素在数组中的位置
                 e.next = newTable[i]; //标记[1]
                 newTable[i] = e;      //将元素放在数组上
                 e = next;             //访问下一个Entry链上的元素
             } while (e != null);
         }
     }
 }

```

==newTable[i]的引用赋给了e.next，也就是使用了单链表的头插入方式，同一位置上新元素总会被放在链表的头部位置；==这样先放在一个索引上的元素终会被放到Entry链的尾部(如果发生了hash冲突的话），这一点和Jdk1.8有区别，下文详解。在旧数组中同一条Entry链上的元素，通过重新计算索引位置后，有可能被放到了新数组的不同位置上。

下面举个例子说明下扩容过程。假设了我们的hash算法就是简单的用key mod 一下表的大小（也就是数组的长度）。其中的哈希桶数组table的size=2， 所以key = 3、7、5，put顺序依次为 5、7、3。在mod 2以后都冲突在table[1]这里了。这里假设负载因子 loadFactor=1，即当键值对的实际大小size 大于 table的实际大小时进行扩容。接下来的三个步骤是哈希桶数组 resize成4，然后所有的Node重新rehash的过程。

![jdk1.7扩容例图](http://tech.meituan.com/img/java-hashmap/jdk1.7%E6%89%A9%E5%AE%B9%E4%BE%8B%E5%9B%BE.png)

### **我们重新来理一下存储的步骤：**

　　1. 传入key和value，判断key是否为null，如果为null，则调用putForNullKey，以null作为key存储到哈希表中； 
　　2. 然后计算key的hash值，根据hash值搜索在哈希表table中的索引位置，若当前索引位置不为null，则对该位置的Entry链表进行遍历，如果链中存在该key，则用传入的value覆盖掉旧的value，同时把旧的value返回，结束； 
　　3. 否则调用addEntry，用key-value创建一个新的节点，并把该节点插入到该索引对应的链表的头部

# **四、HashMap的读取实现get（key，value）**

```
    public V get(Object key) {
        //如果key为null，求null键
        if (key == null)
            return getForNullKey();
        // 用该key求得entry
        Entry<K,V> entry = getEntry(key);

        return null == entry ? null : entry.getValue();
    }
```

```
    final Entry<K,V> getEntry(Object key) {
        int hash = (key == null) ? 0 : hash(key);
        for (Entry<K,V> e = table[indexFor(hash, table.length)];
             e != null;
             e = e.next) {
            Object k;
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                return e;
        }
        return null;
    }
```



读取的步骤比较简单，调用hash（key）求得key的hash值，然后调用indexFor（hash）求得hash值对应的table的索引位置，然后遍历索引位置的链表，如果存在key，则把key对应的Entry返回，否则返回null

# **五、HashMap键的遍历，keySet()**

 modCount是干嘛的啊? 让我来为你解答。众所周知，HashMap不是线程安全的，但在某些容错能力较好的应用中，如果你不想仅仅因为1%的可能性而去承受hashTable的同步开销，HashMap使用了Fail-Fast机制来处理这个问题，你会发现modCount在源码中是这样声明的。

```
    transient volatile int modCount;

```

volatile关键字声明了modCount，代表了多线程环境下访问modCount，根据JVM规范，只要modCount改变了，其他线程将读到最新的值。其实在Hashmap中modCount只是在迭代的时候起到关键作用。

==使用Iterator开始迭代时，会将modCount的赋值给expectedModCount，在迭代过程中，通过每次比较两者是否相等来判断HashMap是否在内部或被其它线程修改，如果modCount和expectedModCount值不一样，证明有其他线程在修改HashMap的结构，会抛出异常。==

==所以HashMap的put、remove等操作都有modCount++的计算。==

HashMap遍历的核心代码如下：

```
    private abstract class HashIterator<E> implements Iterator<E> {
        Entry<K,V> next;        // next entry to return
        int expectedModCount;   // For fast-fail
        int index;              // current slot
        Entry<K,V> current;     // current entry

        //当调用keySet().iterator()时，调用此代码
        HashIterator() {
            expectedModCount = modCount;
            if (size > 0) { // advance to first entry
                Entry[] t = table;
                //从哈希表数组从上到下，查找第一个不为null的节点，并把next引用指向该节点
                while (index < t.length && (next = t[index++]) == null)
                    ;
            }
        }

        public final boolean hasNext() {
            return next != null;
        }

        //当调用next时，会调用此代码
        final Entry<K,V> nextEntry() {
        	//关键
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            Entry<K,V> e = next;
            if (e == null)
                throw new NoSuchElementException();

            //如果当前节点的下一个节点为null，从节点处罚往下查找哈希表，找到第一个不为null的节点
            if ((next = e.next) == null) {
                Entry[] t = table;
                while (index < t.length && (next = t[index++]) == null)
                    ;
            }
            current = e;
            return e;
        }

        public void remove() {
            if (current == null)
            //关键
                throw new IllegalStateException();
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            Object k = current.key;
            current = null;
            HashMap.this.removeEntryForKey(k);
            //注意这里修改计数的同步改变，使我们可以边用迭代器迭代map同时remove元素
            expectedModCount = modCount;
        }
    }
```

从这里可以看出，HashMap遍历时，按哈希表的每一个索引的链表从上往下遍历，由于HashMap的存储规则，最晚添加的节点都有可能在第一个索引的链表中，这就造成了HashMap的遍历时无序的。



# Java 8系列之重新认识HashMap

# 摘要

HashMap是Java程序员使用频率最高的用于映射(键值对)处理的数据类型。随着JDK（Java Developmet Kit）版本的更新，JDK1.8对HashMap底层的实现进行了优化，例如引入红黑树的数据结构和扩容的优化等。本文结合JDK1.7和JDK1.8的区别，深入探讨HashMap的结构实现和功能原理。

# 简介

Java为数据结构中的映射定义了一个接口java.util.Map，此接口主要有四个常用的实现类，分别是HashMap、Hashtable、LinkedHashMap和TreeMap，类继承关系如下图所示：

![java.util.map类图](http://tech.meituan.com/img/java-hashmap/java.util.map%E7%B1%BB%E5%9B%BE.png)

下面针对各个实现类的特点做一些说明：

(1) HashMap：它根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 HashMap最多只允许一条记录的键为null，允许多条记录的值为null。HashMap非线程安全，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要满足线程安全，可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap。

(2) Hashtable：Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类，并且是线程安全的，任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为==ConcurrentHashMap引入了分段锁==。Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。

(3) LinkedHashMap：LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。

(4) TreeMap：TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。如果使用排序的映射，建议使用TreeMap。==在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常。==

==对于上述四种Map类型的类，要求映射中的key是不可变对象。不可变对象是该对象在创建后它的哈希值不会被改变。如果对象的哈希值发生变化，Map对象很可能就定位不到映射的位置了。==

通过上面的比较，我们知道了HashMap是Java的Map家族中一个普通成员，鉴于它可以满足大多数场景的使用条件，所以是使用频度最高的一个。下文我们主要结合源码，从存储结构、常用方法分析、扩容以及安全性等方面深入讲解HashMap的工作原理。

# 内部实现

搞清楚HashMap，首先需要知道HashMap是什么，即它的存储结构-字段；其次弄明白它能干什么，即它的功能实现-方法。下面我们针对这两个方面详细展开讲解。

## 存储结构-字段

从结构实现来讲，HashMap是数组+链表+红黑树（JDK1.8增加了红黑树部分）实现的，如下如所示。

![hashMap内存结构图](http://tech.meituan.com/img/java-hashmap/hashMap%E5%86%85%E5%AD%98%E7%BB%93%E6%9E%84%E5%9B%BE.png)

这里需要讲明白两个问题：数据底层具体存储的是什么？这样的存储方式有什么优点呢？

(1) 从源码可知，HashMap类中有一个非常重要的字段，就是 Node[] table，即哈希桶数组，明显它是一个Node的数组。我们来看Node[JDK1.8]是何物。

```
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;    //用来定位数组索引位置
        final K key;
        V value;
        Node<K,V> next;   //链表的下一个node
 
        Node(int hash, K key, V value, Node<K,V> next) { ... }
        public final K getKey(){ ... }
        public final V getValue() { ... }
        public final String toString() { ... }
        public final int hashCode() { ... }
        public final V setValue(V newValue) { ... }
        public final boolean equals(Object o) { ... }
}

```

Node是HashMap的一个内部类，实现了Map.Entry接口，本质是就是一个映射(键值对)。上图中的每个黑色圆点就是一个Node对象。

(2) ==HashMap就是使用哈希表来存储的。哈希表为解决冲突，可以采用开放地址法和链地址法等来解决问题，Java中HashMap采用了链地址法。链地址法，简单来说，就是数组加链表的结合。在每个数组元素上都一个链表结构，当数据被Hash后，得到数组下标，把数据放在对应下标元素的链表上。==例如程序执行下面代码：

```
    map.put("美团","小美");
```

系统将调用"美团"这个key的hashCode()方法得到其hashCode 值（该方法适用于每个Java对象），然后再通过Hash算法的后两步运算（高位运算和取模运算，下文有介绍）来定位该键值对的存储位置，有时两个key会定位到相同的位置，表示发生了Hash碰撞。当然Hash算法计算结果越分散均匀，Hash碰撞的概率就越小，map的存取效率就会越高。

如果哈希桶数组很大，即使较差的Hash算法也会比较分散，如果哈希桶数组数组很小，即使好的Hash算法也会出现较多碰撞，所以就需要在空间成本和时间成本之间权衡，其实就是在根据实际情况确定哈希桶数组的大小，并在此基础上设计好的hash算法减少Hash碰撞。那么通过什么方式来控制map使得Hash碰撞的概率又小，哈希桶数组（Node[] table）占用空间又少呢？答案就是好的Hash算法和扩容机制。

在理解Hash和扩容流程之前，我们得先了解下HashMap的几个字段。从HashMap的默认构造函数源码可知，构造函数就是对下面几个字段进行初始化，源码如下：

```
     int threshold;             // 所能容纳的key-value对极限 
     final float loadFactor;    // 负载因子
     int modCount;  
     int size;

```

首先，Node[] table的初始化长度length(默认值是16)，Load factor为负载因子(默认值是0.75)，threshold是HashMap所能容纳的最大数据量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多。

结合负载因子的定义公式可知，threshold就是在此Load factor和length(数组长度)对应下允许的最大元素数目，超过这个数目就重新resize(扩容)，扩容后的HashMap容量是之前容量的两倍。默认的负载因子0.75是对空间和时间效率的一个平衡选择，建议大家不要修改，除非在时间和空间比较特殊的情况下，如果内存空间很多而又对时间效率要求很高，可以降低负载因子Load factor的值；相反，如果内存空间紧张而对时间效率要求不高，可以增加负载因子loadFactor的值，这个值可以大于1。

size这个字段其实很好理解，就是HashMap中实际存在的键值对数量。注意和table的长度length、容纳最大键值对数量threshold的区别。==而modCount字段主要用来记录HashMap内部结构发生变化的次数，主要用于迭代的快速失败。强调一点，内部结构发生变化指的是结构发生变化，例如put新键值对，**但是某个key对应的value值被覆盖不属于结构变化**。==

==在HashMap中，哈希桶数组table的长度length大小必须为2的n次方(一定是合数)，这是一种非常规的设计，常规的设计是把桶的大小设计为素数。相对来说素数导致冲突的概率要小于合数，Hashtable初始化桶大小为11，就是桶大小设计为素数的应用（Hashtable扩容后不能保证还是素数）。HashMap采用这种非常规设计，主要是为了在取模和扩容时做优化，同时为了减少冲突，HashMap定位哈希桶索引位置时，也加入了高位参与运算的过程。==

这里存在一个问题，即使负载因子和Hash算法设计的再合理，也免不了会出现拉链过长的情况，一旦出现拉链过长，则会严重影响HashMap的性能。于是，==在JDK1.8版本中，对数据结构做了进一步的优化，引入了红黑树。而当链表长度太长（默认超过8）时，链表就转换为红黑树，利用红黑树快速增删改查的特点提高HashMap的性能，其中会用到红黑树的插入、删除、查找等算法。==

## 功能实现-方法

HashMap的内部功能实现很多，本文主要从根据key获取哈希桶数组索引位置、put方法的详细执行、扩容过程三个具有代表性的点深入展开讲解。

### 1. 确定哈希桶数组索引位置

不管增加、删除、查找键值对，定位到哈希桶数组的位置都是很关键的第一步。前面说过HashMap的数据结构是数组和链表的结合，所以我们当然希望这个HashMap里面的元素位置尽量分布均匀些，尽量使得每个位置上的元素数量只有一个，那么当我们用hash算法求得这个位置的时候，马上就可以知道对应位置的元素就是我们要的，不用遍历链表，大大优化了查询的效率。HashMap定位数组索引位置，直接决定了hash方法的离散性能。先看看源码的实现(方法一+方法二):

```
方法一：
static final int hash(Object key) {   //jdk1.8 & jdk1.7
     int h;
     // h = key.hashCode() 为第一步 取hashCode值
     // h ^ (h >>> 16)  为第二步 高位参与运算
     return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
方法二：
static int indexFor(int h, int length) {  //jdk1.7的源码，jdk1.8没有这个方法，但是实现原理一样的
     return h & (length-1);  //第三步 取模运算
}

```

==这里的Hash算法本质上就是三步：取key的hashCode值、高位运算、取模运算。==

对于任意给定的对象，只要它的hashCode()返回值相同，那么程序调用方法一所计算得到的Hash码值总是相同的。我们首先想到的就是把hash值对数组长度取模运算，这样一来，元素的分布相对来说是比较均匀的。但是，模运算的消耗还是比较大的，在HashMap中是这样做的：调用方法二来计算该对象应该保存在table数组的哪个索引处。

这个方法非常巧妙，它通过h & (table.length -1)来得到该对象的保存位，而HashMap底层数组的长度总是2的n次方，这是HashMap在速度上的优化。==当length总是2的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率。==

==在JDK1.8的实现中，优化了高位运算的算法，通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的，这么做可以在数组table的length比较小的时候，也能保证考虑到高低Bit都参与到Hash的计算中，同时不会有太大的开销。==

下面举例说明下，n为table的长度。

![hashMap哈希算法例图](http://tech.meituan.com/img/java-hashmap/hashMap%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95%E4%BE%8B%E5%9B%BE.png)

### 2. 分析HashMap的put方法

HashMap的put方法执行过程可以通过下图来理解，自己有兴趣可以去对比源码更清楚地研究学习。

![hashMap put方法执行流程图](http://tech.meituan.com/img/java-hashmap/hashMap%20put%E6%96%B9%E6%B3%95%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

①.判断键值对数组table[i]是否为空或为null，否则执行resize()进行扩容；

②.根据键值key计算hash值得到插入的数组索引i，如果table[i]==null，直接新建节点添加，转向⑥，如果table[i]不为空，转向③；

③.判断table[i]的首个元素是否和key一样，如果相同直接覆盖value，否则转向④，这里的相同指的是hashCode以及equals；

④.判断table[i] 是否为treeNode，即table[i] 是否是红黑树，如果是红黑树，则直接在树中插入键值对，否则转向⑤；

⑤.遍历table[i]，判断链表长度是否大于8，大于8的话把链表转换为红黑树，在红黑树中执行插入操作，否则进行链表的插入操作；遍历过程中若发现key已经存在直接覆盖value即可；

⑥.插入成功后，判断实际存在的键值对数量size是否超多了最大容量threshold，如果超过，进行扩容。

JDK1.8HashMap的put方法源码如下:

```
 public V put(K key, V value) {
     // 对key的hashCode()做hash
     return putVal(hash(key), key, value, false, true);
 }
 
 final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                boolean evict) {
     Node<K,V>[] tab; Node<K,V> p; int n, i;
     // 步骤①：tab为空则创建
     if ((tab = table) == null || (n = tab.length) == 0)
         n = (tab = resize()).length;
     // 步骤②：计算index，并对null做处理 
     if ((p = tab[i = (n - 1) & hash]) == null) 
         tab[i] = newNode(hash, key, value, null);
     else {
         Node<K,V> e; K k;
         // 步骤③：节点key存在，直接覆盖value
         if (p.hash == hash &&
             ((k = p.key) == key || (key != null && key.equals(k))))
             e = p;
         // 步骤④：判断该链为红黑树
         else if (p instanceof TreeNode)
             e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
         // 步骤⑤：该链为链表
         else {
             for (int binCount = 0; ; ++binCount) {
                 if ((e = p.next) == null) {
                     p.next = newNode(hash, key,value,null);
                     //链表长度大于8转换为红黑树进行处理
                     if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st  
                         treeifyBin(tab, hash);
                     break;
                 }
                    // key已经存在直接覆盖value
                 if (e.hash == hash &&
                     ((k = e.key) == key || (key != null && key.equals(k)))) 
                            break;
                 p = e;
             }
         }
         
         if (e != null) { // existing mapping for key
             V oldValue = e.value;
             if (!onlyIfAbsent || oldValue == null)
                 e.value = value;
             afterNodeAccess(e);
             return oldValue;
         }
     }
 
     ++modCount;
     // 步骤⑥：超过最大容量 就扩容
     if (++size > threshold)
         resize();
     afterNodeInsertion(evict);
     return null;
 }

```

之前的indefFor()方法消失 了，直接用(tab.length-1)&hash，所以看到这个，代表的就是数组的下角标。

```
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

上面代码中加了一些注释，希望读者仔细分析这段代码中的逻辑。可以看到，我们将put一个元素的过程分为四步，下面我们分步骤讲解：

##### 第一步：table为空，则调用resize()函数创建一个

这里的table就是我们在第一大点“HashMap基本元素”中说的**Node<K,V>[] table;**也就是HashMap的基本子节点。关于这个元素，这里还需要多说一点，在他声明的地方有一段注释:

```
    /**
     * The table, initialized on first use, and resized as
     * necessary. When allocated, length is always a power of two.
     * (We also tolerate length zero in some operations to allow
     * bootstrapping mechanics that are currently not needed.)
     */
     transient Node<K,V>[] table;

```

其中明确提到:这个table数组在第一次使用时需要初始化。在JDK1.7中源码的构造函数中，我们发现：

```
public HashMap(int initialCapacity, float loadFactor) {
    ......
    this.loadFactor = loadFactor;
    threshold = (int)(capacity * loadFactor);
    table = new Entry[capacity];        //注意这里初始化了
    init();
}

```

也就是说，==在jdk1.7中，当HashMap创建的时候，table这个数组确实会初始化；但是到了jdk1.8中，我们观察上面四个构造函数，除了第四个构造函数调用了resize()外，其他三个常用的构造函数都没有与table初始化相关的迹象，而真正table初始化的地方是在我们上面讲的**putVal()**方法中，即**首次向HashMap添加元素时，调用resize()创建并初始化了一个table数组**。==
  这里笔者的理解是，类似于“懒加载”，用的时候再初始化，这样有利于节省资源~~同时，估计1.7和1.8的代码不是一个工程师写的吧，代码优化之后注释忘了改...关于resize()方法，我们之后再讲。

##### 第二步：计算元素所要储存的位置index,并对null做出处理

储存位置即研究这个这个新添加的元素是通过何种规则添加到什么位置的，我们注意到这句源码:`p = tab[i = (n - 1) & hash]`可以看到，**index = (n - 1) & hash**。**n**是新创建的table数组的长度：`(tab = resize()).length;`，那么**hash**是什么呢？注意到这两段代码:

```
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict) {
    ......

```

可以看待，hash是由**hash(key)**这个方法计算所得：

```
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

```

这里可以看到，首先将得到key对应的哈希值：`h = key.hashCode()`，然后通过**hashCode()的高16位异或低16位实现的**：`(h = k.hashCode()) ^ (h >>> 16)`我们分布来讲解这个index产生的步骤:

###### (1).取key的hashcode值：

==**①Object类的hashCode**==
  返回对象的经过**处理后的内存地址**，由于每个对象的内存地址都不一样，所以哈希码也不一样。这个是**native方法**，取决于JVM的内部设计，一般是某种C地址的偏移。
==**②String类的hashCode**==
  根据String类包含的字符串的内容，根据一种特殊算法返回哈希码，**只要字符串的内容相同，返回的哈希码也相同**。
==**③Integer等包装类**==
  返回的哈希码就是**Integer对象里所包含的那个整数的数值**，例如Integer i1=new Integer(100)，i1.hashCode的值就是100 。由此可见，**2个一样大小的Integer对象，返回的哈希码也一样**。
==**④int，char这样的基础类**==
  ==它们不需要hashCode，如果需要存储时，将进行**自动装箱操作**，计算方法包装类。==

###### (2).hashCode()的高16位异或低16位

在JDK1.8的实现中，优化了高位运算的算法，通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从**速度、功效、质量**来考虑的，这么做可以在**数组table的length比较小的时候，也能保证考虑到高低Bit都参与到Hash的计算中**，同时不会有太大的开销。

###### (3). (n-1) & hash; 取模运算

这个n我们说过是table的长度，那么n-1就是table数组元素应有的下表。这个方法非常巧妙，它通过hash & (table.length -1)来得到**该对象的保存位**，而**HashMap底层数组的长度总是2的n次方**，这是HashMap在速度上的优化。**当length总是2的n次方时，hash&(length-1) 运算等价于对length取模，也就是hash % length，但是&比%具有更高的效率。**

关于为什么要先高16位异或低16位再取模运算，我们这里先看第三步：
  我们知道，n代表的是table的长度length，之前一再强调，表table的长度需要取2的整数次幂，就是为了这里等价这里进行取模运算时的方便——取模运算转化成位运算公式:**a%(2^n) 等价于 a&(2^n-1)**,而&操作比%操作具有更高的效率。
  当length=2n时，(length - 1)正好相当于一个**"低位掩码"**,"与"操作的结果就是散列值的高位全部归零，只保留低位，用来做数组下标访问:

![img](https://upload-images.jianshu.io/upload_images/2179030-075199e111fb3829.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

低位&运算.png

可以看到，当我们的length为16的时候，哈希码(字符串“abcabcabcabcabc”的key对应的哈希码)对(16-1)与操作，对于多个key生成的hashCode，只要哈希码的后4位为0，不论不论高位怎么变化，最终的结果均为0。也就是说，如果支取后四位(低位)的话，这个时候产生"碰撞"的几率就非常大(当然&运算中产生碰撞的原因很多，这里只是举个例子)。==为了解决低位与操作碰撞的问题，于是便有了第二步中**高16位异或低16位**的**“扰动函数”。**==

==右移16位，自己的高半区和低半区异或，就是为了**混合原始哈希码的高位和低位，以此来加大低位随机性。**==

![img](https://upload-images.jianshu.io/upload_images/2179030-caf06575abcc83a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

“扰动”后&操作.png

可以看到:
**扰动函数优化前：1954974080 % 16 = 1954974080 & (16 - 1) = 0**
**扰动函数优化后：1955003654 % 16 = 1955003654 & (16 - 1) = 6**
很显然，减少了碰撞的几率。

##### 第三步，判断该链是否为红黑树并添加元素

```
    else if (p instanceof TreeNode)
        e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value); //是红黑树
    else {
        for (int binCount = 0; ; ++binCount) {      //不是红黑树而是链表
            if ((e = p.next) == null) {
                p.next = newNode(hash, key, value, null);   //
                if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                    treeifyBin(tab, hash);
                break;
            }
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                break;
            p = e;
        }
    }

```

关于是红黑树的部分，不在我们本节的讨论范围之内，我们主要看else里边的内容:
`for (int binCount = 0; ; ++binCount) {`表示循环遍历链表，这个for循环当中实际上经历了以下几个步骤：
①**e = p.next**以及for语句之外后面的`p = e;`实际上是在向后循环遍历链表。
开始的时候P为每个桶的头元素，然后将P的引用域(本来指向的是下一个元素)指向空节点e，这个时候实际上就相当于将p的下一个元素赋值给了e,即e已经变成了p的下一个元素。
②此时我们把这个复制的e单独提出来，进行了两个判断：
第一个if：`if ((e = p.next) == null)`
  如果e也就是p.next == null,那么说明当前的这个P已经是链表最后一个元素了。这个时候==采取**尾插法**添加==一个新的元素:`p.next = newNode(hash, key, value, null);`,即直接将p的引用域指向这个新添加的元素。如果添加新元素之后发现链表的长度超过了`TREEIFY_THRESHOLD - 1`也就是超过了8，那么调用`treeifyBin(tab, hash);`把这个链表转换成红黑树接着玩。
第二个if:`if (e.hash == hash &&((k = e.key) == key || (key != null && key.equals(k))))`
  如果发现key值重复了，也就是要插入的key已经存在，那么直接break，结束遍历(不用再费劲去插入了)。
③然后又将e赋给p，这个时候的p已经向后移动了一位。重复上面的过程，直到循环完整个链表，或者break出来。
整个不是红黑树的for循环(或者else)中就做了这三件事。

##### 第四步：超过最大容量限制，扩容

```
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);

```

具体的扩容操作，我们接下来具体再讲

#### 4.HashMap扩容机制的实现

首先上源码，我们在下面的源码中会加上详细的注释，希望读者们一起跟着走一遍:

```
    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;         //创建一个oldTab数组用于保存之前的数组
        int oldCap = (oldTab == null) ? 0 : oldTab.length;  //获取原来数组的长度
        int oldThr = threshold;             //原来数组扩容的临界值
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {   //如果原来的数组长度大于最大值(2^30)
                threshold = Integer.MAX_VALUE;  //扩容临界值提高到正无穷
                return oldTab;                  //返回原来的数组，也就是系统已经管不了了，随便你怎么玩吧
            }
            //else if((新数组newCap)长度乘2) < 最大值(2^30) && (原来的数组长度)>= 初始长度(2^4))
            //这个else if 中实际上就是咋判断新数组(此时刚创建还为空)和老数组的长度合法性，同时交代了，
            //我们扩容是以2^1为单位扩容的。下面的newThr(新数组的扩容临界值)一样，在原有临界值的基础上扩2^1
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0)
            newCap = oldThr;    //新数组的初始容量设置为老数组扩容的临界值
        else {               // 否则 oldThr == 0,零初始阈值表示使用默认值
            newCap = DEFAULT_INITIAL_CAPACITY;  //新数组初始容量设置为默认值
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);     //计算默认容量下的阈值
        }
        if (newThr == 0) {  //如果newThr == 0，说明为上面 else if (oldThr > 0)
        //的情况(其他两种情况都对newThr的值做了改变),此时newCap = oldThr;
            float ft = (float)newCap * loadFactor;  //ft为临时变量，用于判断阈值的合法性
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);     //计算新的阈值
        }
        threshold = newThr; //改变threshold值为新的阈值
        @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab; //改变table全局变量为，扩容后的newTable
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {  //遍历数组，将老数组(或者原来的桶)迁移到新的数组(新的桶)中
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {  //新建一个Node<K,V>类对象，用它来遍历整个数组
                    oldTab[j] = null;
                    if (e.next == null)
                                //将e也就是oldTab[j]放入newTab中e.hash & (newCap - 1)的位置，
                        newTab[e.hash & (newCap - 1)] = e;  //这个我们之前讲过，是一个取模操作
                    else if (e instanceof TreeNode)     //如果e已经是一个红黑树的元素，这个我们不展开讲
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // 链表重排，这一段是最难理解的，也是ldk1.8做的一系列优化，我们在下面详细讲解
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }

```

上面这段代码中，前面一部分我们做了详细的注释，从下面的for()循环开始，我们需要单独拉出来讲一下，因为这一段是重点的数组迁移，也是比较难以理解的：

```
    Node<K,V> loHead = null, loTail = null;
    Node<K,V> hiHead = null, hiTail = null;

```

首先命名了两组Node<K,V>对象，`loHead = null, loTail = null;`与`hiHead = null, hiTail = null;`，这两组对象是为了针对`(e.hash & oldCap) == 0`是否成立这两种情况，而作出不同的处理；结合后面的：

```
    if (loTail != null) {
        loTail.next = null;
        newTab[j] = loHead;
    }
    if (hiTail != null) {
        hiTail.next = null;
        newTab[j + oldCap] = hiHead;
    }

```

可以知道，如果(e.hash & oldCap) == 0，则 newTab[j] = loHead = e = oldTab[j]，即索引位置没变。反之 (e.hash & oldCap) != 0, newTab[j + oldCap] = hiHead = e = oldTab[j],也就是说，此时把**原数组[j]位置上的桶移到了新数组[j+原数组长度]**的位置上了。
  这是个啥意思呢?我们借用美团点评技术团队[Java 8系列之重新认识HashMap](https://link.jianshu.com/?t=http://tech.meituan.com/java-hashmap.html)一文中的部分解释(这篇文章讲的确实很好，网上很多讲解1.8中HashMap的博客都是抄的这篇):

![img](https://upload-images.jianshu.io/upload_images/2179030-a1bfaa66ad18f9cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

数组重排运算示意.png

我们之前一直说的一个移位运算就是—— **a % (2^n) 等价于 a & (2^n - 1)**，也即是位运算与取模运算的转化，且位运算比取模运算具有更高的效率，这也是为什么HashMap中数组长度要求为2^n的原因。我们复习一下，HanshMap中元素存入数组的下表运算为**index = hash & (n - 1) **，其中n为数组长度为2的整数次幂。
  在上面的图中，n表示一个长度为16的数组，n-1就是15，换算成二进制位1111。这个时候有两种不同的哈希码来跟他进行与操作(对应位置都为1结果为1，否则为0)，这两种哈希码的低四位都是相同的，最大的不同是第五位，key1为0，key2为1；
  这个时候我们进行扩容操作，n由16变为32，n-1=32,换算成二进制位11111，这个时候再和key1,key2进行与操作，我们发现，由于第5位的差异，得到了两种不同的结果：

![img](https://upload-images.jianshu.io/upload_images/2179030-2bd047e7051e8373.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

结果.png

可以看到，得出的结果恰好符合上面我们将的两种情况。这也就是1.8中扩容算法做出的改进，至于为什么这么搞？借用刚那篇文章中的一句话——**由于(hashCode中)新增的1bit是0还是1可以认为是随机的，因此resize的过程，均匀的把之前的冲突的节点分散到新的bucket了。**

这个时候有些同学就有问题了，刚刚说了半天**hash & (n - 1)**，源码中明明是`if ((e.hash & oldCap) == 0) {`，并没有减1啊~~我们可以看看如果不减1的话，16就是10000，和key1(第5位为0)相与结果为0，而和key2(第5位上面为1)就成了16了(！=0)，也符合上面两种情况。扩容之后同理。

### JDK1.8扩容做了哪些优化

经过观测可以发现，我们使用的是2次幂的扩展(指长度扩为原来2倍)，所以，元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置。看下图可以明白这句话的意思，n为table的长度，图（a）表示扩容前的key1和key2两种key确定索引位置的示例，图（b）表示扩容后key1和key2两种key确定索引位置的示例，其中hash1是key1对应的哈希与高位运算结果。

![hashMap 1.8 哈希算法例图1](http://tech.meituan.com/img/java-hashmap/hashMap%201.8%20%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95%E4%BE%8B%E5%9B%BE1.png)

元素在重新计算hash之后，因为n变为2倍，那么n-1的mask范围在高位多1bit(红色)，因此新的index就会发生这样的变化：

![hashMap 1.8 哈希算法例图2](http://tech.meituan.com/img/java-hashmap/hashMap%201.8%20%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95%E4%BE%8B%E5%9B%BE2.png)

因此，我们在扩充HashMap的时候，不需要像JDK1.7的实现那样重新计算hash，只需要看看原来的hash值新增的那个bit是1还是0就好了，是0的话索引没变，是1的话索引变成“原索引+oldCap”，可以看看下图为16扩充为32的resize示意图：

![jdk1.8 hashMap扩容例图](http://tech.meituan.com/img/java-hashmap/jdk1.8%20hashMap%E6%89%A9%E5%AE%B9%E4%BE%8B%E5%9B%BE.png)

这个设计确实非常的巧妙，既省去了重新计算hash值的时间，而且同时，由于新增的1bit是0还是1可以认为是随机的，因此resize的过程，均匀的把之前的冲突的节点分散到新的bucket了。这一块就是JDK1.8新增的优化点。有一点注意区别，JDK1.7中rehash的时候，旧链表迁移新链表的时候，如果在新表的数组索引位置相同，则链表元素会倒置，但是从上图可以看出，JDK1.8不会倒置。有兴趣的同学可以研究下JDK1.8的resize源码，写的很赞，如下:

代码中主要点在于判断高位新增的是1还是0，if ((e.hash & oldCap) == 0) 就在原位置，否则在原索引+oldCap的位置。这个if即判断了新增的位是1还是0，一般我们用hash定址，都是hash & length - 1，而这里是直接hash & length，这样的做法是让低位全部为0，这样就可以判断高位是1还是0了！

例如：

0000 0101 & 0000 1111（15），解为5，而0000 0101 & 0001 0000（16），解为0。

因此，假如现在在原table索引位置为5的地方有两个节点，分别为5和21，那么当16扩容成为32时，他们在新数组的位置是这样的：

0000 0101 & 0001 0000 = 0 所以5在新数组的位置：原索引位置5

0001 0101 & 0001 0000 = 1 所以21在新数组的位置：索引位置 + 16



# 线程安全性

在多线程使用场景中，应该尽量避免使用线程不安全的HashMap，而使用线程安全的ConcurrentHashMap。那么为什么说HashMap是线程不安全的，下面举例子说明在并发的多线程使用场景中使用HashMap可能造成死循环。代码例子如下(便于理解，仍然使用JDK1.7的环境)：

```
public class HashMapInfiniteLoop {  
 
    private static HashMap<Integer,String> map = new HashMap<Integer,String>(2，0.75f);  
    public static void main(String[] args) {  
        map.put(5， "C");  
 
        new Thread("Thread1") {  
            public void run() {  
                map.put(7, "B");  
                System.out.println(map);  
            };  
        }.start();  
        new Thread("Thread2") {  
            public void run() {  
                map.put(3, "A);  
                System.out.println(map);  
            };  
        }.start();        
    }  
}

```

其中，map初始化为一个长度为2的数组，loadFactor=0.75，threshold=2*0.75=1，也就是说当put第二个key的时候，map就需要进行resize。

通过设置断点让线程1和线程2同时debug到transfer方法(3.3小节代码块)的首行。注意此时两个线程已经成功添加数据。放开thread1的断点至transfer方法的“Entry next = e.next;” 这一行；然后放开线程2的的断点，让线程2进行resize。结果如下图。

![jdk1.7 hashMap死循环例图1](http://tech.meituan.com/img/java-hashmap/jdk1.7%20hashMap%E6%AD%BB%E5%BE%AA%E7%8E%AF%E4%BE%8B%E5%9B%BE1.png)

注意，Thread1的 e 指向了key(3)，而next指向了key(7)，其在线程二rehash后，指向了线程二重组后的链表。

线程一被调度回来执行，先是执行 newTalbe[i] = e， 然后是e = next，导致了e指向了key(7)，而下一次循环的next = e.next导致了next指向了key(3)。

![jdk1.7 hashMap死循环例图2](http://tech.meituan.com/img/java-hashmap/jdk1.7%20hashMap%E6%AD%BB%E5%BE%AA%E7%8E%AF%E4%BE%8B%E5%9B%BE2.png)

![jdk1.7 hashMap死循环例图3](http://tech.meituan.com/img/java-hashmap/jdk1.7%20hashMap%E6%AD%BB%E5%BE%AA%E7%8E%AF%E4%BE%8B%E5%9B%BE3.png)

e.next = newTable[i] 导致 key(3).next 指向了 key(7)。注意：此时的key(7).next 已经指向了key(3)， 环形链表就这样出现了。

![jdk1.7 hashMap死循环例图4](http://tech.meituan.com/img/java-hashmap/jdk1.7%20hashMap%E6%AD%BB%E5%BE%AA%E7%8E%AF%E4%BE%8B%E5%9B%BE4.png)

于是，当我们用线程一调用map.get(11)时，悲剧就出现了——Infinite Loop。

## 三种解决方案

### Hashtable替换HashMap

Hashtable 是同步的，但由迭代器返回的 Iterator 和由所有 Hashtable 的“collection 视图方法”返回的 Collection 的 listIterator 方法都是快速失败的：在创建 Iterator 之后，如果从结构上对 Hashtable 进行修改，除非通过 Iterator 自身的移除或添加方法，否则在任何时间以任何方式对其进行修改，Iterator 都将抛出 ConcurrentModificationException。因此，面对并发的修改，Iterator 很快就会完全失败，而不冒在将来某个不确定的时间发生任意不确定行为的风险。由 Hashtable 的键和值方法返回的 Enumeration 不是快速失败的。

注意，迭代器的快速失败行为无法得到保证，因为一般来说，不可能对是否出现不同步并发修改做出任何硬性保证。快速失败迭代器会尽最大努力抛出 ConcurrentModificationException。因此，为提高这类迭代器的正确性而编写一个依赖于此异常的程序是错误做法：迭代器的快速失败行为应该仅用于检测程序错误。

### Collections.synchronizedMap将HashMap包装起来

返回由指定映射支持的同步（线程安全的）映射。为了保证按顺序访问，必须通过返回的映射完成对底层映射的所有访问。在返回的映射或其任意 collection 视图上进行迭代时，强制用户手工在返回的映射上进行同步：

```
Map m = Collections.synchronizedMap(new HashMap());
...
Set s = m.keySet();  // Needn't be in synchronized block
...
synchronized(m) {  // Synchronizing on m, not s!
Iterator i = s.iterator(); // Must be in synchronized block
    while (i.hasNext())
        foo(i.next());
}

```

不遵从此建议将导致无法确定的行为。如果指定映射是可序列化的，则返回的映射也将是可序列化的。

### ConcurrentHashMap替换HashMap

支持检索的完全并发和更新的所期望可调整并发的哈希表。此类遵守与 Hashtable 相同的功能规范，并且包括对应于 Hashtable 的每个方法的方法版本。不过，尽管所有操作都是线程安全的，但检索操作不必锁定，并且不支持以某种防止所有访问的方式锁定整个表。此类可以通过程序完全与 Hashtable 进行互操作，这取决于其线程安全，而与其同步细节无关。
检索操作（包括 get）通常不会受阻塞，因此，可能与更新操作交迭（包括 put 和 remove）。检索会影响最近完成的更新操作的结果。对于一些聚合操作，比如 putAll 和 clear，并发检索可能只影响某些条目的插入和移除。类似地，在创建迭代器/枚举时或自此之后，Iterators 和 Enumerations 返回在某一时间点上影响哈希表状态的元素。它们不会抛出 ConcurrentModificationException。不过，迭代器被设计成每次仅由一个线程使用。

# JDK1.8与JDK1.7的性能对比

HashMap中，如果key经过hash算法得出的数组索引位置全部不相同，即Hash算法非常好，那样的话，getKey方法的时间复杂度就是O(1)，如果Hash算法技术的结果碰撞非常多，假如Hash算极其差，所有的Hash算法结果得出的索引位置一样，那样所有的键值对都集中到一个桶中，或者在一个链表中，或者在一个红黑树中，时间复杂度分别为O(n)和O(lgn)。 鉴于JDK1.8做了多方面的优化，总体性能优于JDK1.7，下面我们从两个方面用例子证明这一点。

## Hash较均匀的情况

为了便于测试，我们先写一个类Key，如下：

```
class Key implements Comparable<Key> {
 
    private final int value;
 
    Key(int value) {
        this.value = value;
    }
 
    @Override
    public int compareTo(Key o) {
        return Integer.compare(this.value, o.value);
    }
 
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass())
            return false;
        Key key = (Key) o;
        return value == key.value;
    }
 
    @Override
    public int hashCode() {
        return value;
    }
}

```

这个类复写了equals方法，并且提供了相当好的hashCode函数，任何一个值的hashCode都不会相同，因为直接使用value当做hashcode。为了避免频繁的GC，我将不变的Key实例缓存了起来，而不是一遍一遍的创建它们。代码如下：

```
public class Keys {
 
    public static final int MAX_KEY = 10_000_000;
    private static final Key[] KEYS_CACHE = new Key[MAX_KEY];
 
    static {
        for (int i = 0; i < MAX_KEY; ++i) {
            KEYS_CACHE[i] = new Key(i);
        }
    }
 
    public static Key of(int value) {
        return KEYS_CACHE[value];
    }
}

```

现在开始我们的试验，测试需要做的仅仅是，创建不同size的HashMap（1、10、100、......10000000），屏蔽了扩容的情况，代码如下：

```
   static void test(int mapSize) {
 
        HashMap<Key, Integer> map = new HashMap<Key,Integer>(mapSize);
        for (int i = 0; i < mapSize; ++i) {
            map.put(Keys.of(i), i);
        }
 
        long beginTime = System.nanoTime(); //获取纳秒
        for (int i = 0; i < mapSize; i++) {
            map.get(Keys.of(i));
        }
        long endTime = System.nanoTime();
        System.out.println(endTime - beginTime);
    }
 
    public static void main(String[] args) {
        for(int i=10;i<= 1000 0000;i*= 10){
            test(i);
        }
    }

```

在测试中会查找不同的值，然后度量花费的时间，为了计算getKey的平均时间，我们遍历所有的get方法，计算总的时间，除以key的数量，计算一个平均值，主要用来比较，绝对值可能会受很多环境因素的影响。结果如下：

![性能比较表1.png](http://tech.meituan.com/img/java-hashmap/%E6%80%A7%E8%83%BD%E6%AF%94%E8%BE%83%E8%A1%A81.png)

通过观测测试结果可知，JDK1.8的性能要高于JDK1.7 15%以上，在某些size的区域上，甚至高于100%。由于Hash算法较均匀，JDK1.8引入的红黑树效果不明显，下面我们看看Hash不均匀的的情况。

## Hash极不均匀的情况

假设我们又一个非常差的Key，它们所有的实例都返回相同的hashCode值。这是使用HashMap最坏的情况。代码修改如下：

```
class Key implements Comparable<Key> {
 
    //...
 
    @Override
    public int hashCode() {
        return 1;
    }
}

```

仍然执行main方法，得出的结果如下表所示：

![性能比较表2.png](http://tech.meituan.com/img/java-hashmap/%E6%80%A7%E8%83%BD%E6%AF%94%E8%BE%83%E8%A1%A82.png)

从表中结果中可知，随着size的变大，JDK1.7的花费时间是增长的趋势，而JDK1.8是明显的降低趋势，并且呈现对数增长稳定。==当一个链表太长的时候，HashMap会动态的将它替换成一个红黑树，这话的话会将时间复杂度从O(n)降为O(logn)。==hash算法均匀和不均匀所花费的时间明显也不相同，这两种情况的相对比较，可以说明一个好的hash算法的重要性。

​      测试环境：处理器为2.2 GHz Intel Core i7，内存为16 GB 1600 MHz DDR3，SSD硬盘，使用默认的JVM参数，运行在64位的OS X 10.10.1上。

# 小结

(1) 扩容是一个特别耗性能的操作，所以当程序员在使用HashMap的时候，估算map的大小，初始化的时候给一个大致的数值，避免map进行频繁的扩容。

(2) 负载因子是可以修改的，也可以大于1，但是建议不要轻易修改，除非情况非常特殊。

(3) HashMap是线程不安全的，不要在并发的环境中同时操作HashMap，建议使用ConcurrentHashMap。

(4) JDK1.8引入红黑树大程度优化了HashMap的性能。

(5) 还没升级JDK1.8的，现在开始升级吧。HashMap的性能提升仅仅是JDK1.8的冰山一角。



# 为什么一般hashtable的桶数会取一个素数

为什么一般hashtable的桶数会取一个素数 
设有一个哈希函数
H( c ) = c % N;
当N取一个合数时，最简单的例子是取2^n，比如说取2^3=8,这时候
H( 11100(二进制） ) = H( 28 ) = 4
H( 10100(二进制) ) = H( 20 ）= 4
==这时候c的二进制第4位（从右向左数）就”失效”了，也就是说，无论第c的4位取什么值，都会导致H( c )的值一样．这时候c的第四位就根本不参与H( c )的运算，这样H( c )就无法完整地反映c的特性，增大了导致冲突的几率．==
==取其他合数时，都会不同程度的导致c的某些位”失效”，从而在一些常见应用中导致冲突．==
==但是取质数，基本可以保证c的每一位都参与H( c )的运算，从而在常见应用中减小冲突几率．．==
（个人意见：有时候不取质数效率也不会太差..但是无疑取质数之比较保险的..)
以上就是我的理解
补充一点，这里是说在常见应用中，往往有些数据会比较相近，这时候用质数比较好，比如要存放的数据是压缩的状态，比如存储一个描述当前搜索状态的表，的这时候哈希不用质数冲突机率就比较大。
如果是随机分布的整数，那么哈希模数只要取到足够大，在概率上来说都是一样的，但是这显然脱离实际应用。
你说的情况 是比较特殊的，因为选取了比较小的一个质数，当选去大质数N时，就可以仅在N进制的某一位失效，结合计算机系统的特性，N进制位表示法往往是不关键的，而常用的2^N进制比较关键，所以可以避免冲突。
其实，偶用一些大数做过测试，用来存放一个压缩为二进制的邻接矩阵，当模数足够大时，即便是合数也能有很接近质数的效果，但在某些（几十个）合数上会造成效率严重下降，所以质数是比较保险的。
你不妨自己做实验，不要去选随机整数，而要考虑一些常见应用，用质数和合数进行测试，主要考察平均装载因子，你得到的结论可能和我一样：合数绝大多数时候效果也不错，但在一部分合数上效果差得出奇，而质数几乎全部都有很好的效果。
我个人认为更普遍意义的理解，如果不取素数的话是会有一定危险的，危险出现在当假设所选非素数m=x*y，如果需要hash的key正好跟这个约数x存在关系就惨了，最坏情况假设都为x的倍数，那么可以想象hash的结果为：1~y，而不是1~m。但是如果选桶的大小为素数是不会有这个问题。



# [HashMap深度分析](https://www.cnblogs.com/wuhuangdi/p/4175991.html)

​    这次主要是分析下HashMap的工作原理，为什么我会拿这个东西出来分析，原因很简单，以前我面试的时候，偶尔问起HashMap，99%的程序员都知道HashMap，基本都会用Hashmap，这其中不仅仅包括刚毕业的大学生，也包括已经工作5年，甚至是10年的程序员。HashMap涉及的知识远远不止put和get那么简单。本次的分析希望对于面试的人起码对于面试官的问题有所应付

 **一、先来回忆下我的面试过程**

 **问：“你用过HashMap，你能跟我说说它吗？”**

 **答：**“当然用过，HashMap是一种<key,value>的存储结构，能够快速将key的数据put方式存储起来，然后很快的通过get取出来”，然后说“HashMap不是线程安全的，
HashTable是线程安全的，通过synchronized实现的。HashMap取值非常快”等等。这个时候说明他已经很熟练使用HashMap的工具了。

 

**问：“你知道HashMap 在put和get的时候是怎么工作的吗？”**

**答：**“HashMap是通过key计算出Hash值，然后将这个Hash值映射到对象的引用上，get的时候先计算key的hash值，然后找到对象”。这个时候已经显得不自信了。

 

**问：“HashMap的key为什么一般用字符串比较多，能用其他对象，或者自定义的对象吗？为什么？”**

**答：**“这个没研究过，一般习惯用String。”

 

**问：“你刚才提到HashMap不是线程安全的，你怎么理解线程安全。原理是什么？几种方式避免线程安全的问题。”**

**答：**“线程安全就是多个线程去访问的时候，会对对象造成不是预期的结果，一般要加锁才能线程安全。”

 

其实，问了以上那些问题，我基本能判定这个程序员的基本功了，一般技术中等，接下来的问题没必要问了。

从我的个人角度来看，HashMap的面试问题能够考察面试者的线程问题、Java内存模型问题、线程可见与不可变问题、Hash计算问题、链表结构问题、二进制的&、|、<<、>>等问题。所以一个HashMap就能考验一个人的技术功底了。

这里就是解决Hash的的冲突的函数，解决Hash的冲突有以下几种方法：

   (1)、开放定址法（线性探测再散列，二次探测再散列，伪随机探测再散列）

　(2)、再哈希法

   (3)、链地址法

   (4)、建立一 公共溢出区

  而HashMap采用的是链地址法，



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
    }
```

equles方法内部是分别对name，age进行判断，是否相等。

------

**综合上述，在向hashSet中add()元素时，判断元素是否存在的依据，不仅仅是hash码值就能够确定的，同时还要结合equles方法。**



#### ==**1、 HashMap计算key的hash值时调用单独的方法，在该方法中会判断key是否为null，如果是则返回0；而Hashtable中则直接调用key的hashCode()方法，因此如果key为null，则抛出空指针异常。**==

#### 　　==**2、 HashMap将键值对添加进数组时，不会主动判断value是否为null；而Hashtable则首先判断value是否为null。**==

#### 　　**3、以上原因主要是由于Hashtable继承自Dictionary，而HashMap继承自AbstractMap。**

#### 　　==**4、虽然ConcurrentHashMap也继承自AbstractMap，但是其也过滤掉了key或value为null的键值对。**==



# HashMap源码分析（JDK1.8）- 你该知道的都在这里了



​       HashMap是Java和Android程序员的基本功， JDK1.8对HashMap进行了优化， 你真正理解它了吗？ 

考虑如下问题：  

1、哈希基本原理？（答：散列表、hash碰撞、链表、**红黑树**）

2、hashmap查询的时间复杂度， 影响因素和原理？ （答：最好O（1），最差O（n）， 如果是**红黑O（logn）**）

3、resize如何实现的， 记住已经没有rehash了！！！（答：拉链entry根据高位bit散列到当前位置i和size+i位置）

4、**为什么获取下标时用按位与&，而不是取模%**？ （答：不只是&速度更快哦，  我觉得你能答上来便真正理解hashmap了）

5、什么时机执行resize？

答：hashmap实例里的元素个数大于threshold时执行resize(即桶数量扩容为2倍并散列原来的Entry)。 PS：threshold=桶数量*负载因子

```
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;   //初始化桶，默认16个元素
    if ((p = tab[i = (n - 1) & hash]) == null)   //如果第i个桶为空，创建Node实例
        tab[i] = newNode(hash, key, value, null);
    else { //哈希碰撞的情况， 即(n-1)&hash相等
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;   //key相同，后面会覆盖value
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);  //红黑树添加当前node
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);  //链表添加当前元素
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);  //当链表个数大于等于7时，将链表改造为红黑树
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;            //覆盖key相同的value并return， 即不会执行++size
        }
    }
    ++modCount;
    if (++size > threshold)    //key不相同时，每次插入一条数据自增1. 当size大于threshold时resize
        resize();
    afterNodeInsertion(evict);
    return null;
}

```

 

6、为什么负载因子默认为0.75f ？ 能不能变为0.1、0.9、2、3等等呢？

答：0.75是平衡了时间和空间等因素； 负载因子越小桶的数量越多，读写的时间复杂度越低（极限情况O(1), 哈希碰撞的可能性越小）； 负载因子越大桶的数量越少，读写的时间复杂度越高(极限情况O(n), 哈希碰撞可能性越高)。 0.1，0.9，2，3等都是合法值。

7、影响HashMap性能的因素？

1、 负载因子；

2、哈希值；理想情况是均匀的散列到各个桶。 一般HashMap使用String类型作为key，而String类重写了hashCode函数。

```
   static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

```

8、HashMap的key需要满足什么条件？ 

答：必须重写hashCode和equals方法， 常用的String类实现了这两个方法。

示例代码：

```
    private static class KeyClass {
        int age;
        String name;
 
        public boolean equals(Object anyObject) {
            if (anyObject == this) {
                return true;
            }
 
            if (anyObject instanceof KeyClass) {
                KeyClass obj = (KeyClass) anyObject;
                if (obj.age == this.age
                        && obj.name == this.name) {
                    return true;
                }
            }
            return false;
        }
 
        public int hashCode() {
            return name==null? age : age|name.hashCode();
        }
    }
 
    public static void main(String[] args) {
        HashMap<KeyClass, String> map = new HashMap<>();
        KeyClass obj1 = new KeyClass();
        KeyClass obj2 = new KeyClass();
        obj1.age = 1;
        obj1.name = "Tom";
        obj2.age = 2;
        obj2.name ="Jack";
        map.put(obj1, "aaa");
        map.put(obj2, "bbb");
        map.put(obj1, "ccc");
        map.put(obj2, "ddd");
 
        map.forEach((key, value) -> {
            System.out.println(key.name + "---" + value);
        });
    }

```

```
Tom---ccc
Jack---ddd

```

9、HashMap允许key/value为null， 但最多只有一个。 为什么？  

答： 如果key为null会放在第一个桶（即下标0）位置， 而且是在链表最前面（即第一个位置）。 

JDK1.8的HashMap源码： http://www.grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8u40-b25/java/util/HashMap.java#HashMap

​        我的习惯是先看注释再看源码并调试， 先翻译一下源码注释吧， 不准之处请指正哈。

Hash table based implementation of the `Map` interface. This implementation provides all of the optional map

   HashTable实现了Map接口类， 这些接口实现了所有可选的map功能， 包括允许空值和空key。

operations, and permits `null` values and the`null` key. (The`HashMap` class is roughly equivalent to`Hashtable`, except that it is unsynchronized and permits nulls.) This class makes no guarantees as to the order of the map; in particular, it does not guarantee that the order will remain constant over time.

​       HashMap和HashTable基本一致，  区别是HashMap是线程不同步的且允许空key。 HashMap不保证map的顺序， 而且顺序是可变的。

This implementation provides constant-time performance for the basic operations (`get` and`put`), assuming the hash function disperses the elements properly among the buckets.

​    如果将数据适当的分散到桶里， HashMap的添加、查询函数的执行周期是常量值。

Iteration over collection views requires time proportional to the "capacity" of the`HashMap` instance (the number of buckets) plus its size (the number of key-value mappings). Thus, it's very important not to set the initial capacity too high (or the load factor too low) if iteration performance is important.

​    使用迭代器遍历所有数据的性能跟HashMap的桶（bucket）数量有直接关系，   为了提高遍历的性能， 不能设置比较大的桶数量或者负载因子过低。

An instance of `HashMap` has two parameters that affect its performance:*initial capacity* and*load factor*. The*capacity* is the number of buckets in the hash table, and the initial capacity is simply the capacity at the time the hash table is created.

​      HashMap实例有2个重要参数影响它的性能： 初始容量和负载因子。 初始容量是指在哈希表里的桶总数， 一般在创建HashMap实例时设置初始容量。

The *load factor* is a measure of how full the hash table is allowed to get before its capacity is automatically increased.

​       负载因子是指哈希表在多满时扩容的百分比比例。

When the number of entries in the hash table exceeds the product of the load factor and the current capacity, the hash table is*rehashed* (that is, internal data structures are rebuilt) so that the hash table has approximately twice the number of buckets.

​       当哈希表的数据个数超过负载因子和当前容量的乘积时， 哈希表要再做一次哈希（重建内部数据结构）， 哈希表每次扩容为原来的2倍。

As a general rule, the default load factor (.75) offers a good tradeoff between time and space costs. Higher values decrease the space overhead but increase the lookup cost (reflected in most of the operations of the`HashMap` class, including `get` and `put`). 

​        负载因子的默认值是0.75， 它平衡了时间和空间复杂度。 负载因子越大会降低空间使用率，但提高了查询性能（表现在哈希表的大多数操作是读取和查询）

The expected number of entries in the map and its load factor should be taken into account when setting its initial capacity, so as to minimize the number of rehash operations. If the initial capacity is greater than the maximum number of entries divided by the load factor, no rehash operations will ever occur.

​       考虑哈希表的性能问题， 要设置合适的初始容量，   从而减少rehash的次数。 当哈希表中entry的总数少于负载因子和初始容量乘积时， 就不会发生rehash动作。

If many mappings are to be stored in a `HashMap` instance, creating it with a sufficiently large capacity will allow the mappings to be stored more efficiently than letting it perform automatic rehashing as needed to grow the table. Note that using many keys with the same `hashCode()` is a sure way to slow down performance of any hash table. To ameliorate impact, when keys are`java.lang.Comparable`, this class may use comparison order among keys to help break ties. 

​      如果有很多值要存储到HashMap实例中， 在创建HashMap实例时要设置足够大的初始容量， 避免自动扩容时rehash。 如果很多关键字的哈希值相同， 会降低哈希表的性能。 为了降低这个影响， 当关键字支持`java.lang.Comparable`时， 可以对关键字做次排序以降低影响。

**Note that this implementation is not synchronized.** If multiple threads access a hash map concurrently, and at

least one of the threads modifies the map structurally, it*must* be synchronized externally. (A structural modification

   哈希表是非线程安全的， 如果多线程同时访问哈希表， 且至少一个线程修改了哈希表的结构， 那么必须在访问hashmap前设置同步锁。（修改结构是指添加或者删除一个或多个entry， 修改键值不算是修改结构。）

is any operation that adds or deletes one or more mappings; merely changing the value associated with a key that an instance already contains is not a structural modification.) This is typically accomplished by synchronizing on some object that naturally encapsulates the map. 

​     一般在多线程操作哈希表时，  要使用同步对象封装map。

If no such object exists, the map should be "wrapped" using the`Collections.synchronizedMap` method. This is best done at creation time, to prevent accidental unsynchronized access to the map:

​      如果不封装Hashmap， 可以使用`Collections.synchronizedMap`  方法调用HashMap实例。  在创建HashMap实例时避免其他线程操作该实例， 即保证了线程安全。

```
   Map m = Collections.synchronizedMap(new HashMap(...));
```

​       JDK1.8对哈希碰撞后的拉链算法进行了优化， 当拉链上entry数量太多（超过8个）时，将链表重构为红黑树。  下面是源码相关的注释：

   * This map usually acts as a binned (bucketed) hash table, but
      * when bins get too large, they are transformed into bins of
      * TreeNodes, each structured similarly to those in
      * java.util.TreeMap. Most methods try to use normal bins, but
      * relay to TreeNode methods when applicable (simply by checking
      * instanceof a node).  Bins of TreeNodes may be traversed and
      * used like any others, but additionally support faster lookup
      * when overpopulated. However, since the vast majority of bins in
      * normal use are not overpopulated, checking for existence of
      * tree bins may be delayed in the course of table methods.

看看HashMap的几个重要成员变量：

 //The default initial capacity - MUST be a power of two.

static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; //为毛不写成16？？？ 大师是想用这种写法告诉你只能是2的幂

 HashMap的初始容量是16个， 而且容量只能是2的幂。  每次扩容时都是变成原来的2倍。

static final float DEFAULT_LOAD_FACTOR = 0.75f;

默认的负载因子是0.75f， 16*0.75=12。即默认的HashMap实例在插入第13个数据时，会扩容为32。

The bin count threshold for using a tree rather than list for a bin. Bins are converted to trees when adding an element to a bin with at least this many nodes. The value must be greater than 2 and should be at least 8 to mesh with assumptions in tree removal about conversion back to plain bins upon shrinkage.
static final int TREEIFY_THRESHOLD = 8;

**注意：这是JDK1.8对HashMap的优化， 哈希碰撞后的链表上达到8个节点时要将链表重构为红黑树，  查询的时间复杂度变为O(logN)。**

The table, initialized on first use, and resized as necessary. When allocated, length is always a power of two. (We also tolerate length zero in some operations to allow bootstrapping mechanics that are currently not needed.) 
transient Node<K,V>[] table;  //HashMap的桶， 如果没有哈希碰撞， HashMap就是个数组，我说的是如果![吐舌头](http://static.blog.csdn.net/xheditor/xheditor_emot/default/tongue.gif)。  数组的查询时间复杂度是O(1)，所以HashMap理想时间复杂度是O(1)；如果所有数据都在同一个下标位置， 即N个数据组成链表，时间复杂度为O(n)， 所以HashMap的最差时间复杂度为O(n)。如果链表达到8个元素时重构为红黑树，而红黑树的查询时间复杂度为O(logN), 所以这时HashMap的时间复杂度为O(logN)。
Holds cached entrySet(). Note that AbstractMap fields are used for keySet() and values().
transient Set<Map.Entry<K,V>> entrySet; //HashMap所有的值，因为用了Set， 所以HashMap不会有key、value都相同的数据。

​                               **![img](https://img-blog.csdn.net/20160914094110132?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)**

​                                                       哈希表的结构

1、 哈希碰撞的原因和解决方法：

​     哈希碰撞是不同的key值找到相同的下标，  对应HashMap里hashcode和容量的模相同。

源码629行    if ((p = tab[i = (n - 1) & hash]) == null) ， 其中n是容量值，    即用哈希值和容量相与得到要保存的位置。 如果不同Key的(n - 1) & hash相同， 那么要存储到同一个数组下标位置， 这个现象就叫哈希碰撞。

​        final V putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict) {

​          ....

​         if ((p = tab[i = (n - 1) & hash]) == null)     //如果该下标没值，则存储到该下标位置
             tab[i] = newNode(hash, key, value, null);      
         else {
             Node<K,V> e; K k;
             if (p.hash == hash &&
                 ((k = p.key) == key || (key != null && key.equals(k))))
                 e = p;      //如果哈希值相同而且key相同， 则更新键值
             else if (p instanceof TreeNode)
                 e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);  //如果下标数据是TreeNode类型，则将新数据添加到红黑树中。
             else {
                 for (int binCount = 0; ; ++binCount) {
                     if ((e = p.next) == null) {
                         p.next = newNode(hash, key, value, null);   //将新Node添加到链表末尾
                         if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                             treeifyBin(tab, hash);    //如果链表个数达到8个时，将链表修改为红黑树结构
                         break;
                     }     

​            .....

​           }

2、JDK1.8对HashMap最大的优化是resize函数，  在扩容时不再需要rehash了， 下面就看看大师是怎么实现的。

Initializes or doubles table size. If null, allocates in accord with initial capacity target held in field threshold. Otherwise, because we are using power-of-two expansion, the elements from each bin must either stay at same index, or move with a power of two offset in the new table.

初始化数组或者扩容为2倍，   初值为空时，则根据初始容量开辟空间来创建数组。否则， 因为我们使用2的幂定义数组大小，数据要么待在原来的下标， 或者移动到新数组的高位下标。 （举例： 初始数组是16个，假如有2个数据存储在下标为1的位置， 扩容后这2个数据可以存在下标为1或者16+1的位置）

Returns:
    the table
final Node<K,V>[] resize() {

​         ....

​         newThr = oldThr << 1; // double threshold,   大小扩大为2倍，出于性能考虑和者告诉使用者它是2的幂， 这里用的是位移， 而不是*2，

   if (e.next == null)
      newTab[e.hash & (newCap - 1)] = e;  //如果该下标只有一个数据，则散列到当前位置或者高位对应位置（以第一次resize为例，原来在第4个位置，resize后会存储到第4个或者第4+16个位置）
  else if (e instanceof TreeNode)
     ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);  //红黑树重构

   else {

​     do {
        next = e.next;
        if ((e.hash & oldCap) == 0) {    
            if (loTail == null)
               loHead = e;
            else
            loTail.next = e;
            loTail = e;
        } else {
            if (hiTail == null)
               hiHead = e;
            else
               hiTail.next = e;
               hiTail = e;
         }
      } while ((e = next) != null);
      if (loTail != null) {
          loTail.next = null;
          newTab[j] = loHead;   //下标不变
      }
      if (hiTail != null) {
          hiTail.next = null;
          newTab[j + oldCap] = hiHead; //下标位置移动原来容量大小
      }

   (e.hash & oldCap) == 0写的很赞！！！ 它将原来的链表数据散列到2个下标位置，  概率是当前位置50%，高位位置50%。     你可能有点懵比， 下面举例说明。  上边图中第0个下标有496和896，  假设它俩的hashcode(int型，占4个字节)是

resize前：

496的hashcode: 00000000  00000000  00000000  00000000

896的hashcode: 01010000  01100000  10000000  00100000

oldCap是16:       00000000  00000000  00000000  00010000

​    496和896对应的e.hash & oldCap的值为0， 即下标都是第0个。

resize后：

496的hashcode: 00000000  00000000  00000000  00000000

896的hashcode: 01010000  01100000  10000000  00100000

oldCap是32:       00000000  00000000  00000000  00100000

   496和896对应的e.hash & oldCap的值为0和1， 即下标都是第0个和第16个。

   看明白了吗？   因为hashcode的第n位是0/1的概率相同， 理论上链表的数据会均匀分布到当前下标或高位数组对应下标。

​       回顾JDK1.7的HashMap，在扩容时会rehash即每个entry的位置都要再计算一遍，  性能不好呀， 所以JDK1.8做了这个优化。

​      再回到文章最开始的问题， HashMap为什么用&得到下标，而不是%？   如果使用了取模%， 那么在容量变为2倍时， 需要rehash确定每个链表元素的位置。![大笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/laugh.gif)

​     很佩服HashMap的作者呀，  大师在运算符的使用上都是这么考究！！！

PS: **顺便说一下ArrayList， 初始容量是10个， 每次扩容是原来的1.5倍。**



# [Java中队列和堆栈](https://www.cnblogs.com/tadckle/p/3530084.html)

队列（queue），先进先出（First in first out，FIFO）。

堆栈（stack），后进先出（Last in first out，LIFO）。

​      Java中有Stack这个类，但是不推荐使用。通常使用Deque来完成队列和堆栈的功能。

​      Deque是一个线性表接口，可以两端进行元素的插入和删除。Deque是“Double ended Queue”的缩写，Deque读音[dɛk] 。使用Deque接口提供的方法就可以完成队列“先进先出”和堆栈“后进先出”的功能：

| 队列 | offer(E e) --- 向队列尾加入元素E poll() --- 获取队列头部元素，并从队列中删去 |
| ---- | ------------------------------------------------------------ |
| 堆栈 | push(E e) --- 向堆栈中压入元素E pop() --- 获取栈顶元素，并从堆栈中删除 |

 

Deque是个接口，其实现类有：

- ArrayDeque，使用“数组”存储数据
- LinkedList，使用“链表”存储数据
- ConcurrentLinkedDeque，线程安全的LinkedList

数据检索多的用ArrayDeque；数据需要频繁插入、更新，则用LinkedList；多线程操作使用ConcurrentLinkedDeque。

代码示例：

```
   1: Deque<String> queue = new LinkedList<String>();
   2: queue.offer("data1");    //队列尾部加入元素
   3: queue.offer("data2");
   4: queue.offer("data3");
   5: System.out.println(queue.poll());    //取得队首元素，并从队列中删去
   6:  
   7: Deque<String> stack = new LinkedList<String>();
   8: stack.push("element1");    //向栈顶压入元素
   9: stack.push("element2");
  10: stack.push("element3");
  11: System.out.println(queue.pop());    //取得栈顶元素，并从栈顶删去
```