# hashCode() 和equals() 区别和作用

首先说明一下JDK对equals(Object obj)和hashcode()这两个方法的定义和规范： 

-  在Java中任何一个对象都具备equals(Object obj)和hashcode()这两个方法，因为他们是在Object类中定义的。  
- equals(Object obj)方法用来判断两个对象是否“相同”，如果“相同”则返回true，否则返回false。  
- ==hashcode()方法返回一个int数，在Object类中的默认实现是“将该对象的内部地址转换成一个整数返回”。==  

接下来有两个个关于这两个方法的重要规范(我只是抽取了最重要的两个,其实不止两个)： 

- ==规范1：若重写equals(Object obj)方法，有必要重写hashcode()方法，确保通过equals(Object obj)方法判断结果为true的两个对象具备相等的hashcode()返回值==。说得简单点就是：“如果两个对象相同，那么他们的hashcode应该 相等”。不过请注意：这个只是规范，如果你非要写一个类让equals(Object obj)返回true而hashcode()返回两个不相等的值，编译和运行都是不会报错的。不过这样违反了Java规范，程序也就埋下了BUG。  
- ==规范2：如果equals(Object obj)返回false，即两个对象“不相同”，并不要求对这两个对象调用hashcode()方法得到两个不相同的数。==说的简单点就是：“如果两个对象不相同，他们的hashcode可能相同”。  

根据这两个规范，可以得到如下推论： 

- 1、如果两个对象equals，Java运行时环境会认为他们的hashcode一定相等。  
- 2、如果两个对象不equals，他们的hashcode有可能相等。  
- 3、如果两个对象hashcode相等，他们不一定equals。
-   4、如果两个对象hashcode不相等，他们一定不equals。

==**简而言之，在集合查找时，hashcode能大大降低对象比较次数，提高查找效率！**== 

**当equals()方法被重写时，通常需要重写 hashCode 方法，以维护在hashCode 方法最开始的声明，即相等对象必须具有相等的哈希码。** 

**==object类中的hashcode()方法是一个本地方法，比较的是引用对象的内存地址==，使用new方法创建对象，两次生成的是不同的对象，造成的结果就是两个对象的hashcode()返回的值不一样 。**

==**Integer（基本类型）的equals实际上都是用的自己的实际值比较，String则是逐个char比较相等于否。**== 

## 花边：通用的hashCode重写方案

初始化一个整形变量，为此变量赋予一个非零的常数值，比如int result = 17;
 选取equals方法中用于比较的所有域，然后针对每个域的属性进行计算：

1. 如果是boolean值，则计算f ? 1:0
2. 如果是byte\char\short\int,则计算(int)f
3. 如果是long值，则计算(int)(f ^ (f >>> 32))
4. 如果是float值，则计算Float.floatToIntBits(f)
5. 如果是double值，则计算Double.doubleToLongBits(f)，然后返回的结果是long,再用规则(3)去处理long,得到int
6. 如果是对象引用，如果equals方法中采取递归调用的比较方式，那么hashCode中同样采取递归调用hashCode的方式。否则需要为这个域计算一个范式，比如当这个域的值为null的时候，那么hashCode 值为0
7. 如果是数组，那么需要为每个元素当做单独的域来处理。如果你使用的是1.5及以上版本的JDK，那么没必要自己去重新遍历一遍数组，java.util.Arrays.hashCode方法包含了8种基本类型数组和引用数组的hashCode计算，算法同上

给个简单的例子：

```
@Override
public int hashCode() {
  int result = 17;
  result = 31 * result + getName().hashCode();
  return result;
}
```

 

 java中==运算符作用在基本数据类型和引用数据类型是不同的，
基本数据类型 byte, short, char, int, long, float, double, boolean
他们之间的比较，应用双等号（==）,比较的是他们的值。

引用数据类型用（==）进行比较的时候，
**比较的是在内存中的存放地址，所以，除非是实例化的同一个对象比较后的结果才为true，否则比较后结果为false。**
==注意一下，String，Integer，Date这些类重写了equals()方法，不再是比较类在堆内存中的存放地址了。==



# Object类中常见的方法



**一.Object类中的toString()方法** 

==object 默认方法 toString方法，toString() 输出一个对象的地址字符串（哈希code码）！== 

**二.Object类中的equals()方法** 
==Object类equals()比较的是对象的引用是否指向同一块内存地址！== 

**Object()** 
默认构造方法 
**clone()** 
创建并返回此对象的一个副本。 
**finalize()** 
当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。 
**getClass()** 
返回一个对象的运行时类。 
**hashCode()** 
返回该对象的哈希码值。 

**wait(),sleep()区别？** 
wait():释放资源，释放锁 
sleep():释放资源，不释放锁

**notify()** 
唤醒在此对象监视器上等待的单个线程。 
**notifyAll()** 
唤醒在此对象监视器上等待的所有线程。 
**wait()** 
导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法。 
**wait(long timeout)** 
导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量。 
**wait(long timeout, int nanos)** 
导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量。

# 为什么wait,notify,notifyAll声明在类Object中

**为什么`wait`,`notify`,`notifyAll`声明在类`Object`中**而不是在`Thread`中这个问题是一个非常有名的java核心面试题，在各种级别的Java开发者面试中都会出现（2年，4年，甚至资深的工程师）。

这个问题的魅力在于，它反映了面试者是否知道等待通知机制，他如何看待整个等待和通知功能，以及他的理解是否不浅于这个主题。 像[为什么在Java中不支持多重继承](http://javarevisited.blogspot.com/2011/07/why-multiple-inheritances-are-not.html)和[为什么Java中String是final类型的](http://javarevisited.blogspot.com/2010/10/why-string-is-immutable-in-java.html)问题一样，这些问题都有多个答案并且每一个都有可以证明的理由。

在我的所有面试经验中，我发现`wait`和`notify`仍然是大多数Java程序员特别是2到3年最困惑的，如果要求他们使用`wait`和`notify`编写代码，他们经常会挣扎者难以实现。 所以，如果你要去参加任何Java面试，确保你对`wait`和`notify`机制有清晰认识，并且你能很容易的使用`wait`和`notify`机制编程代码来实现像生产消费者问题或实现阻塞队列等。本文是继续 的我早先的文章有关等待和通知例如 为什么Wait和notify需要从同步块或方法中调用，Java中的等待，睡眠和yield方法之间的区别，如果你还没有阅读你可能会发现有趣。

顺便说一下这篇文章是我的早期文章的延续，有关等待和通知。 [为什么Wait和notify需要从同步块或方法中调用](http://javarevisited.blogspot.com/2011/05/wait-notify-and-notifyall-in-java.html)，[Java中的等待，睡眠和yield方法之间的区别](http://javarevisited.blogspot.com/2011/12/difference-between-wait-sleep-yield.html)，如果你还没有阅读你可能会发现有趣。

### 答案

==`为什么它们不应该在Thread类`这里有一些想法，对我来说是有意义的。==

1. ==`wait`和`nofity`不是常见的普通java方法或同步工具，在Java中它们更多的是实现两个线程之间的通信机制。 如果不能通过类似`synchronized`这样的Java关键字来实现这种机制，那么`Object`类中就是定义它们最好的地方，以此来使任何Java对象都可以拥有实现线程通信机制的能力。记住`synchronized`和`wait`,`notify`是两个不同的问题域，并且不要混淆它们的相似或相关性。 同步类似竞态条件，是提供线程间互斥和确保Java类的线程安全性的，而`wait`和`notify`是两个线程之间的通信机制。==
2. ==每个对象都可以作为锁，这是另一个原因`wait`和`notify`在Object类中声明，而不是Thread类。==
3. ==在Java中，为了进入临界区代码段，线程需要获得锁并且它们等待锁可用，它们不知道哪些线程持有锁而它们只知道锁是由某个线程保持，它们应该等待锁而不是知道哪个线程在同步块内并要求它们释放锁。 这个比喻适合等待和通知在`object`类而不是Java中的线程。==

clone方法执行的是浅拷贝， 在编写程序时要注意这个细节。

# 覆盖Object中的clone方法， 实现深拷贝

==现在为了要在clone对象时进行深拷贝， 那么就要Clonable接口，覆盖并实现clone方法，除了调用父类中的clone方法得到新的对象， 还要将该类中的引用变量也clone出来。==**==Cloneable 接口是一个标识性接口，即该接口不包含任何方法（甚至没有clone()方法），但是如果一个类想合法的进行克隆，那么就必须实现这个接口。==** 

- ==调用Clone()方法的对象所属的类(Class)必须实现 Clonable 接口，否则在调用Clone方法的时候会抛出CloneNotSupportedException；==


# 如何进行彻底的深拷贝

==如果在拷贝一个对象时，要想让这个拷贝的对象和源对象完全彼此独立，那么在**引用链上的每一级对象都要被显式的拷贝**。所以创建彻底的深拷贝是非常麻烦的，尤其是在引用关系非常复杂的情况下， 或者在引用链的某一级上引用了一个第三方的对象， 而这个对象没有实现clone方法， 那么在它之后的所有引用的对象都是被共享的。== 



# 类与类加载器

类加载器虽然只用于实现类的加载动作，但它在Java程序起到的作用却远大于类加载阶段==。对于任意一个类，都需要由**加载它的类加载器和这个类本身**一同确立**其在Java虚拟机中的唯一性**，每一个类加载器，都拥有一个独立的类名称空间。==通俗而言：**比较两个类是否“相等”**（这里所指的“相等”，包括类的Class对象的equals()方法、isAssignableFrom()方法、isInstance()方法的返回结果，也包括使用instanceof()关键字对做对象所属关系判定等情况）**，==只有在这两个类时由同一个类加载器加载的前提下才有意义，否则，即使这两个类来源于同一个Class文件，被同一个虚拟机加载，只要加载它们的类加载器不同，那这两个类就必定不相等。==**

# 双亲委派模型

从jvm的角度来讲，只存在以下两种不同的类加载器：

- **启动类加载器（Bootstrap ClassLoader）**，这个类加载器用C++实现，是虚拟机自身的一部分；
- **所有其他类的加载器**，这些类由Java实现，独立于虚拟机外部，并且全都继承自抽象类`java.lang.ClassLoader`。

从Java开发人员的角度看，类加载器可以划分得更细致一些：

- **启动类加载器（Bootstrap ClassLoader）** 此类加载器负责将存放在` <JAVA_HOME>\lib `目录中的，或者被 -Xbootclasspath 参数所指定的路径中的，并且是虚拟机识别的（仅按照文件名识别，如 rt.jar，名字不符合的类库即使放在lib 目录中也不会被加载）类库加载到虚拟机内存中。 启动类加载器无法被 Java 程序直接引用，用户在编写自定义类加载器时，如果需要把加载请求委派给引导类加载器，直接使用null代替即可。
- **扩展类加载器（Extension ClassLoader）** 这个类加载器是由`ExtClassLoader（sun.misc.Launcher$ExtClassLoader）`实现的。它负责将`<Java_Home>/lib/ext`或者被 `java.ext.dir`系统变量所指定路径中的所有类库加载到内存中，开发者可以直接使用扩展类加载器。
- **应用程序类加载器（Application ClassLoader）** 这个类加载器是由 `AppClassLoader（sun.misc.Launcher$AppClassLoader）`实现的。由于这个类加载器是`ClassLoader`中的`getSystemClassLoader()`方法的返回值，因此一般称为**系统类加载器**。它负责加载**用户类路径（ClassPath）**上所指定的类库，开发者可以直接使用这个类加载器，如果应用程序中没有自定义过自己的类加载器，一般情况下这个就是程序中默认的类加载器。

由开发人员开发的应用程序都是由这三种类加载器相互配合进行加载的，如果有必要，还可以加入自己定义的类加载器。这些类加载器的关系一般如下图所示：

![img](https://pic.yupoo.com/crowhawk/188f5d64/26536d6a.png)

上图展示的类加载器之间的层次关系，称为类加载器的**双亲委派模型（Parents Delegation Model）**。该模型要求除了顶层的启动类加载器外，其余的类加载器都应有自己的父类加载器，==这里类加载器之间的父子关系一般通过**组合（Composition）**关系来实现，而不是通过继承（Inheritance）的关系实现。==

**工作过程**

==如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载，而是把这个请求委派给父类加载器，每一个层次的加载器都是如此，依次递归，因此所有的加载请求**最终都应该传送到顶层的启动类加载器中**，只有当父加载器反馈自己无法完成此加载请求（它搜索范围中没有找到所需类）时，子加载器才会尝试自己加载。==

**优点**

==使用双亲委派模型来组织类加载器之间的关系，使得Java类随着它的类加载器一起具备了一种**带有优先级的层次关系**。例如类`java.lang.Object`，它存放再`rt.jar`中，无论哪个类加载器要加载这个类，最终都是委派给处于模型最顶端的启动类加载器进行加载，因此Object类在程序的各种类加载器环境中都是同一个类。==

相反，如果没有双亲委派模型，由各个类加载器自行加载的话，如果用户编写了一个称为｀`java.lang.Object`的类，并放在程序的ClassPath中，那系统中将会出现多个不同的Object类，程序将变得一片混乱。如果开发者尝试编写一个与`rt.jar`类库中已有类重名的Java类，将会发现可以正常编译，但是永远无法被加载运行。

# java基本数据类型传递与引用传递

总结：

- 一个方法不能修改一个基本数据类型的参数（数值型和布尔型）。
- 一个方法可以修改一个引用所指向的对象状态，但这仍然是按值调用而非引用调用。
- 上面两种传递都进行了值拷贝的过程。



# Java中数组的特性



==数组是基本上所有语言都会有的一种数据类型，它表示一组相同类型的数据的集合，具有固定的长度，并且在内存中占据连续的空间。==

#Java中的数组是对象吗？

Java和C++都是面向对象的语言。在使用这些语言的时候，我们可以直接使用标准的类库，也可以使用组合和继承等面向对象的特性构建自己的类，并且根据自己构建的类创建对象。那么，我们是不是应该考虑这样一个问题：在面向对象的语言中，数组是对象吗？

要判断数组是不是对象，那么首先明确什么是对象，也就是对象的定义。==在较高的层面上，对象是根据某个类创建出来的一个实例，表示某类事物中一个具体的个体。对象具有各种属性，并且具有一些特定的行为。而在较低的层面上，站在计算机的角度，对象就是内存中的一个内存块，在这个内存块封装了一些数据，也就是类中定义的各个属性，所以，对象是用来封装数据的。==以下为一个Person对象在内存中的表示：

![](D:\workspace\Github\node\瑞秋\answer\assets\20131115233005265.png)

注意：

1）小的红色矩形表示一个引用（地址）或一个基本类型的数据，大的红色矩形表示一个对象，多个小的红色矩形组合在一块，可组成一个对象。

2）name在对象中只表示一个引用， 也就是一个地址值，它指向一个真实存在的字符串对象。在这里严格区分了引用和对象。

那么在Java中，数组满足以上的条件吗？在较高的层面上，数组不是某类事物中的一个具体的个体，而是多个个体的集合。那么它应该不是对象。而在计算机的角度，数组也是一个内存块，也封装了一些数据，这样的话也可以称之为对象。以下是一个数组在内存中的表示：

![](D:\workspace\Github\node\瑞秋\answer\assets\20131115233056328.png)

这样的话， 数组既可以是对象， 也可以不是对象。至于到底是不是把数组当做对象，全凭Java的设计者决定。数组到底是不是对象， 通过代码验证：

```
		int[] a = new int[4];
		//a.length;  //对属性的引用不能当成语句
		int len = a.length;  //数组中保存一个字段, 表示数组的长度
		
		//以下方法说明数组可以调用方法,java中的数组是对象.这些方法是Object中的方法,所以可以肯定,数组的最顶层父类也是Object
		a.clone();
		a.toString();

```

 

在数组a上， 可以访问他的属性，也可以调用一些方法。==**这基本上可以认定，java中的数组也是对象，它具有java中其他对象的一些基本特点：封装了一些数据，可以访问属性，也可以调用方法。所以，数组是对象。**==

# Java中数组的类型

Java是一种强类型的语言。既然是对象， 那么就必须属于一个类型，比如根据Person类创建一个对象，这个对象的类型就是Person。那么数组的类型是什么呢？看下面的代码：

```
		int[] a1 = {1, 2, 3, 4};
		System.out.println(a1.getClass().getName());
		//打印出的数组类的名字为[I
		
		String[] s = new String[2];
		System.out.println(s.getClass().getName());
		//打印出的数组类的名字为  [Ljava.lang.String;
		
		String[][] ss = new String[2][3];
		System.out.println(ss.getClass().getName());
		//打印出的数组类的名字为    [[Ljava.lang.String;

```

 打印出a1的类型为[ I ，s 的类型是[Ljava.lang.String;  ,  ss的类型是[[Ljava.lang.String;   

所以，**数组也是有类型的。**只是这个类型显得比较奇怪。你可以说a1的类型是int[]，这也无可厚非。但是我们没有自己创建这个类，也没有在Java的标准库中找到这个类。也就是说不管是我们自己的代码，还是在JDK中，都没有如下定义：

```
	public class int[] {
		
		// ...
		
		// ...
		
		// ...
	}

```

 

这只能有一个解释，那就是**虚拟机自动创建了数组类型**，可以把数组类型和8种基本数据类型一样， 当做java的内建类型。==这种类型的命名规则是这样的：==

* ==每一维度用一个[表示；开头两个[，就代表是二维数组。==
* ==[后面是数组中元素的类型(包括基本数据类型和引用数据类型)==

在java语言层面上,s是数组,也是一个对象,那么他的类型应该是String[]，这样说是合理的。但是在JVM中，他的类型为[java.lang.String。顺便说一句普通的类在JVM里的类型为 包名+类名，也就是全限定名。同一个类型在java语言中和在虚拟机中的表示可能是不一样的。

#Java中数组的继承关系

上面已经验证了，数组是对象，也就是说可以以操作对象的方式来操作数组。并且数组在虚拟机中有它特别的类型。既然是对象，遵循Java语言中的规则 -- Object是上帝， 也就是说所有类的顶层父类都是Object。==数组的顶层父类也必须是Object，这就说明数组对象可以向上直接转型到Object，也可以向下强制类型转换，也可以使用instanceof关键字做类型判定。== 这一切都和普通对象一样。如下代码所示：

```
		//1		在test1()中已经测试得到以下结论: 数组也是对象, 数组的顶层父类是Object, 所以可以向上转型
		int[] a = new int[8];
		Object obj = a ; //数组的父类也是Object,可以将a向上转型到Object
		
		//2		那么能向下转型吗?
		int[] b = (int[])obj;  //可以进行向下转型
		
		//3		能使用instanceof关键字判定吗?
		if(obj instanceof int[]){  //可以用instanceof关键字进行类型判定
			System.out.println("obj的真实类型是int[]");
		}

```

 

# Java中数组的另一种“继承”关系

如下代码是正确的，却很容易让我们疑惑：

```
		String[] s = new String[5];
		Object[] obja = s;   //成立,说明可以用Object[]的引用来接收String[]的对象

```

 Object[]类型的引用可以指向String[]类型的数组对象？ 由上文的验证可以得知数组类型的顶层父类一定是Object，那么上面代码中s的直接父类是谁呢？难道说String[]继承自Object[]，而Object[]又继承自Object? 让我们通过反射的方式来验证这个问题： 

```
		//5		那么String[] 的直接父类是Object[] 还是 Object?
		System.out.println(s.getClass().getSuperclass().getName());
		//打印结果为java.lang.Object,说明String[] 的直接父类是 Object而不是Object[]

```

 由代码可知，String[]的直接父类就是Object而不是Object[]。可是Object[]的引用明明可以指向String[]类型的对象。那么他们的继承关系有点像这样： 

![](D:\workspace\Github\node\瑞秋\answer\assets\20131116003417359.png)

这样的话就违背了Java单继承的原则。String[]不可能即继承Object，又继承Object[]。上面的类图肯定是错误的。那么只能这样解释：数组类直接继承了Object，关于Object[]类型的引用能够指向String[]类型的对象，这种情况只能是Java语法之中的一个特例，并不是严格意义上的继承。也就是说，==**String[]不继承自Object[]，但是我可以允许你向上转型到Object[]，这种特性是赋予你的一项特权。**==

其实这种关系可以这样表述：**如果有两个类A和B，如果B继承（extends）了A，那么A[]类型的引用就可以指向B[]类型的对象。**如下代码所示：

```
	public static class Father {
 
	}
	
	public static class Son extends Father {
 
	}

```

```
		//6	  下面成立吗?  Father是Son的直接父类
		Son[] sons = new Son[3];
		Father[] fa = sons;  //成立
		
		//7		那么Son[] 的直接父类是Father[] 还是  Object[] 或者是Object?
		System.out.println(sons.getClass().getSuperclass().getName());
		//打印结果为java.lang.Object,说明Son[]的直接父类是Object

```

 上面的结论可以扩展到二维数组和多维数组： 

```
		Son[][] sonss = new Son[2][4];
		Father[][] fathers = sonss;

```

上面的代码可以这样理解： 

将Father[][]数组看成是一维数组, 这是个数组中的元素为Father[]，将Son[][]数组看成是一维数组, 这是个数组中的元素为Son[]，因为Father[]类型的引用可以指向Son[]类型的对象，所以，根据上面的结论，==Father[][]的引用可以指向Son[][]类型的对象。==

==数组的这种用法不能作用于基本类型数据：==

```
int[] aa = new int[4];
//Object[] objaa = aa;  //错误的，不能通过编译
```

 ==这是错误的, 因为int不是引用类型，Object不是int的父类，在这里自动装箱不起作用。但是这种方式是可以的：== 

```
Object[] objss = {"aaa", 1, 2.5};
```

 ==这种情况下自动装箱可以工作，也就是说，Object数组中可以存放任何值，包括基本数据类型。==

 

Java为什么会为数组提供这样一种语法特性呢？也就是说这种语法有什么作用？编写过Android中Sqlite数据库操作程序的同学可能发现过这种现象，用一个Object[]引用接收所有的数组对象，在编译SQL语句时，为SQL语句中的占位符提供对应的值。

```
db.execSQL("INSERT INTO person VALUES (NULL, ?, ?)", new Object[]{person.name, person.age}); 
```

所以这种特性主要是用于方法中参数的传递。如果不传递数组，而是依次传递各个值，会使方法参数列表变得冗长。如果使用具体的数组类型，如String[]，那么就限定了类型，失去了灵活性。所以传递数组类型是一种比较好的方式。但是如果没有上面的数组特性（如果有两个类A和B，如果B继承（extends）了A，那么A[]类型的引用就可以指向B[]类型的对象），那么数组类型就只能通过Object类型接收，这样就无法在方法内部访问或遍历数组中的各个元素。如下代码： 

```java
	private static void test3() {
		String[] a = new String[3];
		doArray(a);
	}
 
	private static void doArray(Object[] objs){
		
	}
	
	private static void doArray1(Object obj){
		//不能用Object接收数组，因为这样无法对数组的元素进行访问
		// obj[1]  //错误
		
		//如果在方法内部对obj转型到数组，存在类型转换异常的风险
		// Object[] objs = (Object[]) obj;
	}
	
	private static void doArray2(String[] strs){
		//如果适用特定类型的数组，就限制了类型，失去灵活性和通用性
	}
	
	private static void doArray3(String name, int age, String id, float account){
		//如果不适用数组而是依次传递参数，会使参数列表变得冗长，难以阅读
	}

```

 到此为止，数组的特性就总结完了。上文中加粗的部分为重要结论。下面贴出整个源码： 

# 源码

```java
package com.pansoft.zhangjg.testarray;
 
public class ArrayTest {
 
	/**
	 * @param args
	 */
	public static void main(String[] args) {
		test1();
		test2();
		test3();
	}
 
	/**
	 * 数组具有这种特性：
	 * 如果有两个类A和B，如果B继承（extends）了A，那么A[]类型的引用就可以指向B[]类型的对象
	 * 测试数组的特殊特性对参数传递的便利性
	 */
	private static void test3() {
		String[] a = new String[3];
		doArray(a);
	}
 
	private static void doArray(Object[] objs){
		
	}
	
	private static void doArray1(Object obj){
		//不能用Object接收数组，因为这样无法对数组的元素进行访问
		// obj[1]  //错误
		
		//如果在方法内部对obj转型到数组，存在类型转换异常的风险
		// Object[] objs = (Object[]) obj;
	}
	
	private static void doArray2(String[] strs){
		//如果适用特定类型的数组，就限制了类型，失去灵活性和通用性
	}
	
	private static void doArray3(String name, int age, String id, float account){
		//如果不适用数组而是依次传递参数，会使参数列表变得冗长，难以阅读
	}
	/**
	 * 测试数组的集成关系, 并且他的继承关系是否和数组中元素的类型有关
	 */
	private static void test2() {
		
		//1		在test1()中已经测试得到以下结论: 数组也是对象, 数组的顶层父类是Object, 所以可以向上转型
		int[] a = new int[8];
		Object obj = a ; //数组的父类也是Object,可以将a向上转型到Object
		
		//2		那么能向下转型吗?
		int[] b = (int[])obj;  //可以进行向下转型
		
		//3		能使用instanceof关键字判定吗?
		if(obj instanceof int[]){  //可以用instanceof关键字进行类型判定
			System.out.println("obj的真实类型是int[]");
		}
		
		//4  	下面代码成立吗?
		String[] s = new String[5];
		Object[] obja = s;   //成立,说明可以用Object[]的引用来接收String[]的对象
		
		//5		那么String[] 的直接父类是Object[] 还是 Object?
		System.out.println(s.getClass().getSuperclass().getName());
		//打印结果为java.lang.Object,说明String[] 的直接父类是 Object而不是Object[]
		
		//6	  下面成立吗?  Father是Son的直接父类
		Son[] sons = new Son[3];
		Father[] fa = sons;  //成立
		
		//7		那么Son[] 的直接父类是Father[] 还是  Object[] 或者是Object?
		System.out.println(sons.getClass().getSuperclass().getName());
		//打印结果为java.lang.Object,说明Son[]的直接父类是Object
		
		/**
		 * 做一下总结, 如果A是B的父类, 那么A[] 类型的引用可以指向 B[]类型的变量
		 * 但是B[]的直接父类是Object, 所有数组的父类都是Object
		 */
		
		//8		上面的结论可以扩展到二维数组
		Son[][] sonss = new Son[2][4];
		Father[][] fathers = sonss;
		//将Father[][]数组看成是一维数组, 这是个数组中的元素为Father[]
		//将Son[][]数组看成是一维数组, 这是个数组中的元素为Son[]
		//因为Father[]类型的引用可以指向Son[]类型的对象
		//所以,根据上面的结论,Father[][]的引用可以指向Son[][]类型的对象
		
		/**
		 * 扩展结论:
		 * 因为Object是所有引用类型的父类
		 * 所以Object[]的引用可以指向任何引用数据类型的数组的对象. 如:
		 * Object[] objs = new String[1];
		 * Object[] objs = new Son[1];
		 *
		 */
		
		//9		下面的代码成立吗?
		int[] aa = new int[4];
		//Object[] objaa = aa;  //错误的，不能通过编译
		//这是错误的, 因为Object不是int的父类,在这里自动装箱不起作用
		
		//10 	这样可以吗？
		Object[] objss = {"aaa", 1, 2.5};//成立
	}
 
	/**
	 * 测试在java语言中,数组是不是对象
	 * 如果是对象, 那么他的类型是什么?
	 */
	private static void test1() {
		int[] a = new int[4];
		//a.length;  //对属性的引用不能当成语句
		int len = a.length;  //数组中保存一个字段, 表示数组的长度
		
		//以下方法说明数组可以调用方法,java中的数组是对象.这些方法是Object中的方法,所以可以肯定,数组的最顶层父类也是Object
		a.clone();
		a.toString();
		
		
		/**
		 * java是强类型的语言,一个对象总会有一个特定的类型,例如 Person p = new Person();
		 * 对象p(确切的说是引用)的类型是Person类, 这个Person类是我们自己编写的
		 * 那么数组的类型是什么呢? 下面使用反射的方式进行验证
		 */
		int[] a1 = {1, 2, 3, 4};
		System.out.println(a1.getClass().getName());
		//打印出的数组类的名字为[I
		
		String[] s = new String[2];
		System.out.println(s.getClass().getName());
		//打印出的数组类的名字为  [Ljava.lang.String;
		
		String[][] ss = new String[2][3];
		System.out.println(ss.getClass().getName());
		//打印出的数组类的名字为    [[Ljava.lang.String;
		
		/**
		 * 所以,数组也是有类型的,只不过这个类型不是有程序员自己定义的类, 也不是jdk里面
		 * 的类, 而是虚拟机在运行时专门创建的类
		 * 类型的命名规则是:
		 * 		每一维度用一个[表示;
		 * 		[后面是数组中元素的类型(包括基本数据类型和引用数据类型)
		 * 
		 * 在java语言层面上,s是数组,也是一个对象,那么他的类型应该是String[],
		 * 但是在JVM中,他的类型为[java.lang.String
		 * 
		 * 顺便说一句普通的类在JVM里的类型为 包名+类名, 也就是全限定名
		 */
	}
	
	public static class Father {
 
	}
	
	public static class Son extends Father {
 
	}
}

```

