

# 

Java基础：JDK、JRE、JVM的区别与联系

## **1.1 JVM – java virtual machine**

JVM 是 Java 平台的基础,是整个java实现跨平台的 最核心的部分,它有自己的指令集，并且在运行时操作不同的内存区域，其主要工作是解释自己的指令集（即字节码）到 CPU 的指令集或 OS 的系统调用，提供了一种与平台无关的代码执行方法。所有的java程序会首先被编译为.class的类文件，这种类文件可以在虚拟机上执行，也就是说class并不直接与机器的操作系统相交互，而是经过虚拟机间接与操作系统交互，由虚拟机将程序解释给本地系统执行。（ JVM 对上层的 Java 源文件是不关心的，它关注的只是由源文件生成的类文件（ class file ）。类文件的 组成包括 JVM 指令集，符号表以及一些补助信息。 JVM 的实现也是互不相同的，比如垃圾回收算法，线程调度算法（可能不同 OS 有不同的实现））

## **1.2 JRE – java runtime environment**

JRE是指java运行环境。光有JVM还不能成class的 执行，因为在解释class的时候JVM需要调用解释所需要的类库lib。 在JDK的安装目录里的jre目录，里面有两个文件夹bin和lib,在 这里可以认为bin里的就是jvm，lib中则是jvm工 作所需要的类库，而jvm和 lib和起来就称为jre。（jre里有运行.class的java.exe）

## **1.3 JDK – java development kit**

JDK是java开发工具包，在目录下面有 六个文件夹、一个src类库源码压缩包、和其他几个声明文件。其中，真正在运行java时起作用的 是以下四个文件夹：bin、include、lib、 jre。现在我们可以看出这样一个关系，JDK包含JRE，而JRE包 含JVM。

- bin:最主要的是编译器(javac.exe)
- include:java和JVM交互用的头文件
- lib:类库
- jre:java运行环境

注意：这里的bin、lib文件夹和jre里的bin、lib是 不同的，总的来说==JDK是用于java程序的开发==,而==jre则 是只能运行class而没有编译的功能==。eclipse、idea等 其他IDE有自己的编译器而不是用JDK bin目录中自带的，所以在安装时你会发现他们只要求你 选中jre路径就ok了

Java 喊出的带有标志性的口号“ Write Once ， Run Anywhere （一次编写，到处运行）”，正是建立在 JRE 的基础之上。何以实现？就是==在 Java 应用程序和操作系统之间增加了一虚拟层—— JRE== 。

==程序源代码不是直接编译、链接成机器代码，而是先转化到字节码（ bytecode ） 这种特殊的中间形式，字节码再转换成机器码或系统调用。==前者是传统的编译方法，生成的机器代码就不可避免地跟特殊的操作系统和特殊的机器结构相关。

而 ==Java 程序的字节码文件可以放到任意装有 JRE 的计算机运行，再由不同 JRE 的将它们转化成相应的机器代码，这就实现了 Java 程序的可移植性==。

如果你使用JAVA开发应用，就需要安装 JRE+JDK，就是 J2SE 
如果在客户端运行Applet，客户端浏览器必须嵌有JAVA JVM，如果没有，就需要安装，即：在客户端创建JRE（运行时，包含JVM），而客户端是不需要做开发的，所以，JDK就没有必要安装 了

# final, finally, finalize的区别

## 1、final 修饰符（关键字）

   final用于控制成员、方法或者是一个类是否可以被重写或者继承等功能，如果要学好java，那么这个关键字很重要，必须掌握。如果类被声明为final，意味着它不能再派生出新的子类，不能作为父类被继承。将变量或者方法声明为final，可以保证他们在使用中不被改变。其初始化可以在两个地方：一是其定义处，也就是说，在final变量定义时直接给其赋值；二是构造函数中。这2个地方只能选其一，要么在定义处直接给其赋值，要么在构造函数中给值，并且在以后的引用中，只能读取，不可修改。被声明为final的方法也同样只能使用，不能重写。

## 2、finally(用于异常处理)

​     一般是用于异常处理中，提供finally块来执行任何的清除操作，try{} catch(){} finally{}。finally关键字是对java异常处理模型的最佳补充。finally结构使代码总会执行，不关有无异常发生。使得finally可以维护对象的内部状态，并可以清理非内存资源。

 finally在try,catch中可以有，可以没有。如果trycatch中有finally则必须执行finally快中的操作。一般情况下，用于关闭文件的读写操作，或者是关闭数据库的连接等等。

## 3、finalize（用于垃圾回收）

finalize这个是方法名。在java中，允许使用finalize()方法在垃圾收集器将对象从内存中清理出去之前做必要的清理工作。这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。它是Object类中定义的，因此，所有的类都继承了它。finalize()方法是在垃圾收集器删除对象之前对这个对象调用的。

特殊情况下，需要程序员实现finalize，当对象被回收的时候释放一些资源，比如：一个socket链接，在对象初始化时创建，整个生命周期内有效，那么就需要实现finalize，关闭这个链接。 使用finalize还需要注意一个事，调用super.finalize(); **一个对象的finalize()方法只会被调用一次，而且finalize()被调用不意味着gc会立即回收该对象，所以有可能调用finalize()后，该对象又不需要被回收了**，然后到了真正要被回收的时候，因为前面调用过一次，所以不会调用finalize()，产生问题。 一般来说，不推荐使用finalize()方法，它跟析构函数不一样。 



## final, finally, finalize的区别

- final 用于声明属性,方法和类, 分别表示属性不可变, 方法不可覆盖, 类不可继承.
- finally 是异常处理语句结构的一部分，表示无论异常是否发生，都会执行finally块的内容 
- finalize 是Object类的一个方法，这个方法是由垃圾收集器在确定这个对象没有被引用时对这个对象调用的。java技术允许使用finalize()方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等. JVM不保证此方法总被调用.

 

# Java 为什么是高效的 ( High Performance )?

因为 Java 使用 Just-In-Time (即时) 编译器.

把java字节码直接转换成可以直接发送给处理器的指令的程序.



# Java 多态部分 向上转型 向下转型

(多态)转型问题其实并不复杂，只要记住一句话：父类引用指向子类对象。
 什么叫父类引用指向子类对象，且听我慢慢道来.从2个名词开始说起：向上转型(upcasting) 、向下转型(downcasting).

举个例子：有2个类，Father是父类，Son类继承自Father。

```
Father f1 = new Son();   // 这就叫 upcasting （向上转型)
// 现在f1引用指向一个Son对象
Son s1 = (Son)f1;   // 这就叫 downcasting (向下转型)
// 现在f1还是指向Son对象
```

第2个例子：

```
Father f2 = new Father();
Son s2 = (Son)f2;       // 出错，子类引用不能指向父类对象
```

你或许会问，第1个例子中：Son s1 = (Son)f1;问什么 是正确的呢。
 很简单因为f1指向一个子类对象，Father f1 = new Son(); 子类s1引用当然可以指向子类对象了。
 而f2 被传给了一个Father对象，Father f2 = new Father（）；子类s1引用不能指向父类对象。

# 实例

- 向上转型：子类引用的对象转换为父类类型称为向上转型。通俗地说就是是将子类对象转为父类对象。此处父类对象可以是接口
- 向上转型时，父类指向子类引用对象会遗失除与父类对象共有的其他方法，也就是在转型过程中，子类的新有的方法都会遗失掉，在编译时，系统会提供找不到方法的错误

向下转型：父类引用的对象转换为子类类型称为向下转型。

### 父类

```
package testP.test;
class Person {  
    public void eat(){
        System.out.println("Person eatting...");
    }
    public void sleep() {
        System.out.println("Person sleep...");
    }
}
```

### 向上转型

```
package testP.test;

public class son extends Person {
    public void eat() {
        System.out.println("son eatting...");
    }
    
    public void fly() {
        System.out.println("son fly...");
    }
    
     public static void main(String[] args) {
         Person person = new son();
         person.eat(); // 调用的是son里面的eat方法，son重写了Person父类的方法
         person.sleep(); //调用的是父类person的方法
         person.fly(); //报错了，丢失了son类的fly方法
        }
}
```

### 向下转型

在向下转型过程中，分为两种情况：

情况一：如果父类引用的对象如果引用的是指向的子类对象，那么在向下转型的过程中是安全的。也就是编译是不会出错误的。

情况二：如果父类引用的对象是父类本身，那么在向下转型的过程中是不安全的，编译不会出错，但是运行时会出现java.lang.ClassCastException错误。它可以使用instanceof来避免出错此类错误

- 安全不报错

```
package testP.test;

public class son extends Person {
    public void eat() {
        System.out.println("son eatting...");
    }
    
    public void fly() {
        System.out.println("son fly...");
    }
    
     public static void main(String[] args) {
          Person person = new son(); // 向上转型
         son s = (son)person;   //向下转型,编译和运行皆不会出错
         s.fly(); 
         s.eat();
         s.sleep();  
        }
}
```

- 编译器报错

```
package testP.test;

public class son extends Person {
    public void eat() {
        System.out.println("son eatting...");
    }
    
    public void fly() {
        System.out.println("son fly...");
    }
    
     public static void main(String[] args) {
        
         Person upPerson = new Person();
         son so = (son)upPerson; // 向下转型 编译器报错了
         so.fly();
        }
}
```

 

#  面向对象编程三大特性------封装、继承、多态

## **一、封装**

​    封装从字面上来理解就是包装的意思，专业点就是信息隐藏，是指利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体，数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只保留一些对外接口使之与外部发生联系。系统的其他对象只能通过包裹在数据外面的已经授权的操作来与这个封装的对象进行交流和交互。也就是说用户是无需知道对象内部的细节，但可以通过该对象对外的提供的接口来访问该对象。

​    对于封装而言，一个对象它所**封装的是自己的属性和方法**，所以它是不需要依赖其他对象就可以完成自己的操作。使用封装有三大好处：

> 1、良好的封装能够减少耦合。
>
> 2、类内部的结构可以自由修改。
>
> 3、可以对成员进行更精确的控制。
>
> 4、隐藏信息，实现细节。

​    封装把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法，如果不想被外界方法，我们大可不必提供方法给外界访问。但是如果一个类没有提供给外界访问的方法，那么这个类也没有什么意义了。

```java
1. public class Husband {  
2.       
3.     /* 
4.      * 对属性的封装 
5.      * 一个人的姓名、性别、年龄、妻子都是这个人的私有属性 
6.      */  
7.     private String name ;  
8.     private String sex ;  
9.     private int age ;  
10.     private Wife wife;  
11.       
12.     /* 
13.      * setter()、getter()是该对象对外开发的接口 
14.      */  
15.     public String getName() {  
16.         return name;  
17.     }  
18.   
19.     public void setName(String name) {  
20.         this.name = name;  
21.     }  
22.   
23.     public String getSex() {  
24.         return sex;  
25.     }  
26.   
27.     public void setSex(String sex) {  
28.         this.sex = sex;  
29.     }  
30.   
31.     public int getAge() {  
32.         return age;  
33.     }  
34.   
35.     public void setAge(int age) {  
36.         this.age = age;  
37.     }  
38.   
39.     public void setWife(Wife wife) {  
40.         this.wife = wife;  
41.     }  
42. }  

```



​    封装可以使我们容易地修改类的内部实现，而无需修改使用了该类的客户代码。就可以对成员变量进行更精确的控制。

```java
1. public void setAge(int age) {  

2.     if(age > 120){  

1. System.out.println("ERROR：error age input....");    //提示错误信息  

4.     }else{  

5.         this.age = age;  

6.     }  

7. }  

 

1. public String getSexName() {  

2.         if("0".equals(sex)){  

3.             sexName = "女";  

4.         }  

5.         else if("1".equals(sex)){  

6.             sexName = "男";  

7.         }  

8.         else{  

9.             sexName = "人妖";  

10.         }  

11.         return sexName;  

1. }  

```



 

# **二、继承**#

## 2.1 继承综述    

​    继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承我们能够非常方便地复用以前的代码，能够大大的提高开发的效率。

​    继承所描述的是“is-a”的关系，如果有两个对象A和B，若可以描述为“A是B”，则可以表示A继承B，其中B是被继承者称之为父类或者超类，A是继承者称之为子类或者派生类。

​    实际上继承者是被继承者的特殊化，它除了拥有被继承者的特性外，还拥有自己独有得特性。例如猫有抓老鼠、爬树等其他动物没有的特性。同时在继承关系中，继承者完全可以替换被继承者，反之则不可以，例如我们可以说猫是动物，但不能说动物是猫就是这个道理，其实对于这个我们将其称之为“向上转型”。

​    诚然，继承定义了类如何相互关联，共享特性。对于若干个相同或者相识的类，我们可以抽象出他们共有的行为或者属相并将其定义成一个父类或者超类，然后用这些类继承该父类，他们不仅可以拥有父类的属性、方法还可以定义自己独有的属性或者方法。

​    同时在使用继承时需要记住三句话：

> 1、子类拥有父类非private的属性和方法。
>
> 2、子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
>
> 3、子类可以用自己的方式实现父类的方法。（以后介绍）。

​    学习继承一定少不了这三个东西：**构造器、protected关键字、向上转型**

## 2.2 构造器

​    通过前面我们知道子类可以继承父类的属性和方法，除了那些private的外还有一样是子类继承不了的---构造器。对于构造器而言，它只能够被调用，而不能被继承。 调用父类的构造方法我们使用super()即可。

​    构建过程是从父类“向外”扩散的，也就是从父类开始向子类一级一级地完成构建。而且我们并没有显示的引用父类的构造器，这就是java的聪明之处：编译器会默认给子类调用父类的构造器。

​    但是，这个默认调用父类的构造器是有前提的：父类有默认构造器。如果父类没有默认构造器，我们就要必须显示的使用super()来调用父类构造器，否则编译器会报错：无法找到符合父类形式的构造器。

​    对于**子类**而已,其构造器的正确初始化是非常重要的,而且**当且仅当只有一个**方法可以保证这点：**在构造器中调用父类构造器来完成初始化**，而父类构造器具有执行父类初始化所需要的所有知识和能力。

对于继承而言，子类会默认调用父类的构造器，但是如果没有默认的父类构造器，子类必须要显示的指定父类的构造器，而且必须是在子类构造器中做的第一件事(第一行代码)。

## **2.3 protected关键字**

​    private访问修饰符，对于封装而言，是最好的选择，但这个只是基于理想的世界，有时候我们需要这样的需求：我们需要将某些事物尽可能地对这个世界隐藏，但是仍然允许子类的成员来访问它们。这个时候就需要使用到protected。

​    对于protected而言，它指明就类用户而言，他是private，但是对于任何继承与此类的子类而言或者其他任何位于同一个包的类而言，他却是可以访问的。

## **2.4 向上转型**

​    在上面的继承中我们谈到继承是is-a的相互关系，猫继承与动物，所以我们可以说猫是动物，或者说猫是动物的一种。这样将猫看做动物就是向上转型。

```java
1. public class Person {  

2.     public void display(){  

3.         System.out.println("Play Person...");  

4.     }  

5.       

6.     static void display(Person person){  

7.         person.display();  

8.     }  

9. }  

10.   

11. public class Husband extends Person{  

12.     public static void main(String[] args) {  

13.         Husband husband = new Husband();  

14.         Person.display(husband);      //向上转型  

15.     }  

1. }  

```



在这我们通过Person.display(husband)。这句话可以看出husband是person类型。

​    将子类转换成父类，在继承关系上面是向上移动的，所以一般称之为向上转型。由于向上转型是从一个叫专用类型向较通用类型转换，所以它总是安全的，唯一发生变化的可能就是属性和方法的丢失。这就是为什么编译器在“未曾明确表示转型”活“未曾指定特殊标记”的情况下，仍然允许向上转型的原因。

## **2.5 谨慎继承**

在这里我们需要明确，继承存在如下缺陷：

1、父类变，子类就必须变。

2、继承破坏了封装，对于父类而言，它的实现细节对与子类来说都是透明的。

3、继承是一种强耦合关系。

​    所以说当我们使用继承的时候，我们需要确信使用继承确实是有效可行的办法。那么到底要不要使用继承呢？《Think in java》中提供了解决办法：问一问自己是否需要从子类向父类进行向上转型。如果必须向上转型，则继承是必要的，但是如果不需要，则应当好好考虑自己是否需要继承。

## **三、多态**

## 3.1 多态综述  

​    所谓多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。因为在程序运行时才确定具体的类，这样，不用修改源程序代码，就可以让引用变量绑定到各种不同的类实现上，从而导致该引用调用的具体方法随之改变，即不修改程序代码就可以改变程序运行时所绑定的具体代码，让程序可以选择多个运行状态，这就是多态性。

所以对于多态我们可以总结如下：

   指向子类的父类引用由于向上转型了，它只能访问父类中拥有的方法和属性，而对于子类中存在而父类中不存在的方法，该引用是不能使用的，尽管是重载该方法。若子类重写了父类中的某些方法，在调用该些方法的时候，必定是使用子类中定义的这些方法（动态连接、动态调用）。

​    对于面向对象而言，多态分为编译时多态和运行时多态。其中编辑时多态是静态的，主要是指方法的重载，它是根据参数列表的不同来区分不同的函数，通过编辑之后会变成两个不同的函数，在运行时谈不上多态。而运行时多态是动态的，它是通过动态绑定来实现的，也就是我们所说的多态性。

## 3.2 多态的实现条件

​      在刚刚开始就提到了继承在为多态的实现做了准备。子类Child继承父类Father，我们可以编写一个指向子类的父类类型引用，该引用既可以处理父类Father对象，也可以处理子类Child对象，当相同的消息发送给子类或者父类对象时，该对象就会根据自己所属的引用而执行不同的行为，这就是多态。即多态性就是相同的消息使得不同的类做出不同的响应。

多态就是指一个变量, 一个方法或者一个对象可以有不同的形式.

多态主要分为

- 重载overloading

```
就是一个类里有两个或更多名字相同而参数不同的函数.
```

- 覆写overriding

```
是发生在子类中！也就是说必须有继承的情况下才有覆盖发生. 当你继承父类的方法时, 如果你感到哪个方法不爽，功能要变，那就把那个函数在子类中重新实现一遍.
```

重载 静态捆绑 (static binding) 是 Compile 时的多态

覆写 动态捆绑 (dynamic binding) 是 runtime 多态

## ==重载 VS 重写==

关于重载和重写，你应该知道以下几点：

> 1、重载是一个编译期概念、重写是一个运行期间概念。
>
> 2、重载遵循所谓“编译期绑定”，即在编译时根据参数变量的类型判断应该调用哪个方法。
>
> 3、重写遵循所谓“运行期绑定”，即在运行的时候，根据引用变量所指向的实际对象的类型来调用方法
>
> 4、因为在编译期已经确定调用哪个方法，所以重载并不是多态。而重写是多态。重载只是一种语言特性，是一种语法规则，与多态无关，与面向对象也无关。（注：严格来说，重载是编译时多态，即静态多态。但是，Java中提到的多态，在不特别说明的情况下都指动态多态）



**构造器是否可以被 override**？

构造器不能被继承 所以不能被 override 但可以被重载 overload



## 重载(Overload)

重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。

每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

最常用的地方就是构造器的重载。

**重载规则:**

- 被重载的方法必须改变参数列表(参数个数或类型不一样)；
- 被重载的方法可以改变返回类型；
- 被重载的方法可以改变访问修饰符；
- 被重载的方法可以声明新的或更广的检查异常；
- 方法能够在同一个类中或者在一个子类中被重载。
- 无法以返回值类型作为重载函数的区分标准。（==与返回值无关==）

## 重写(Override)

==重写指的是根据运行时对象的类型来决定调用哪个方法，而不是根据编译时的类型。== 静态方法是类的方法，所以它们在编译阶段就使用编译出来的类型进行绑定了（参考继承连调用优先级）。使用对象引用来访问静态方法只是Java设计者给程序员的自由。==我们应该直接使用类名来访问静态方法，而不要使用对象引用来访问。== 

==**重写方法与被重写方法只有函数体不同**==

- 参数列表必须完全与被重写方法的相同；
- 返回类型必须完全与被重写方法的返回类型相同；
- 访问权限不能比父类中被重写的方法的访问权限更低。例如：如果父类的一个方法被声明为public，那么在子类中重写该方法就不能声明为protected。
- 父类的成员方法只能被它的子类重写。
- 声明为final的方法不能被重写。
- ==声明为static的方法不能被重写，但是能够被再次声明。（子类引用调用子类的，父类引用调用父类的，即静态方法是编译器静态绑定的）==
- ==子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为private和final的方法。==
- ==子类和父类不在同一个包中，那么子类只能够重写父类的声明为public和protected的非final方法。==
- ==重写的方法能够抛出任何非强制异常，无论被重写的方法是否抛出异常。但是，重写的方法不能抛出新的强制性异常，或者比被重写方法声明的更广泛的强制性异常，反之则可以。==
- ==构造方法不能被重写。==
- 如果不能继承一个方法，则不能重写这个方法。



## 重写与重载之间的区别

| 区别点   | 重载方法 | 重写方法                                       |
| -------- | -------- | ---------------------------------------------- |
| 参数列表 | 必须修改 | 一定不能修改                                   |
| 返回类型 | 可以修改 | 一定不能修改                                   |
| 异常     | 可以修改 | 可以减少或删除，一定不能抛出新的或者更广的异常 |
| 访问     | 可以修改 | 一定不能做更严格的限制（可以降低限制）         |

------

## 总结

方法的重写(Overriding)和重载(Overloading)是java多态性的不同表现，重写是父类与子类之间多态性的一种表现，重载可以理解成多态的具体表现形式。

- (1)方法重载是一个类中定义了多个方法名相同,而他们的参数的数量不同或数量相同而类型和次序不同,则称为方法的重载(Overloading)。
- (2)方法重写是在子类存在方法与父类的方法的名字相同,而且参数的个数与类型一样,返回值也一样的方法,就称为重写(Overriding)。
- (3)方法重载是一个类的多态性表现,而方法重写是子类与父类的一种多态性表现。



## Java实现多态有三个必要条件**：继承、重写、向上转型。**

​    继承：在多态中必须存在有继承关系的子类和父类。

​    重写：子类对父类中某些方法进行重新定义，在调用这些方法时就会调用子类的方法。

​    向上转型：在多态中需要将子类的引用赋给父类对象，只有这样该引用才能够具备技能调用父类的方法和子类的方法。

​    只有满足了上述三个条件，我们才能够在同一个继承结构中使用统一的逻辑实现代码处理不同的对象，从而达到执行不同的行为。

​    对于Java而言，它多态的实现机制遵循一个原则：当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。

## **3.3 实现形式**

### 在Java中有两种形式可以实现多态：继承和接口。

### **3.2.1、基于继承实现的多态**

​    基于继承的实现机制主要表现在父类和继承该父类的一个或多个子类对某些方法的重写，多个子类对同一方法的重写可以表现出不同的行为。

​    基于继承实现的多态可以总结如下：对于引用子类的父类类型，在处理该引用时，它适用于继承该父类的所有子类，子类对象的不同，对方法的实现也就不同，执行相同动作产生的行为也就不同。

​    如果父类是抽象类，那么子类必须要实现父类中所有的抽象方法，这样该父类所有的子类一定存在统一的对外接口，但其内部的具体实现可以各异。这样我们就可以使用顶层类提供的统一接口来处理该层次的方法。

### **3.2.2、基于接口实现的多态**

#### 接口 Interface

​    继承是通过重写父类的同一方法的几个不同子类来体现的，那么就可就是通过实现接口并覆盖接口中同一方法的几不同的类体现的。

​    在接口的多态中，指向接口的引用必须是指定这实现了该接口的一个类的实例程序，在运行时，根据对象引用的实际类型来执行对应的方法。

​    继承都是单继承，只能为一组相关的类提供一致的服务接口。但是接口可以是多继承多实现，它能够利用一组相关或者不相关的接口进行组合与扩充，能够对外提供一致的服务接口。所以它相对于继承来说有更好的灵活性。

### 3.2.3、经典实例分析

```java
1. public class A {  
2.     public String show(D obj) {  
3.         return ("A and D");  
4.     }  
5.   
6.     public String show(A obj) {  
7.         return ("A and A");  
8.     }   
9.   
10. }  
11.   
12. public class B extends A{  
13.     public String show(B obj){  
14.         return ("B and B");  
15.     }  
16.       
17.     public String show(A obj){  
18.         return ("B and A");  
19.     }   
20. }  
21.   
22. public class C extends B{  
23.   
24. }  
25.   
26. public class D extends B{  
27.   
28. }  
29.   
30. public class Test {  
31.     public static void main(String[] args) {  
32.         A a1 = new A();  
33.         A a2 = new B();  
34.         B b = new B();  
35.         C c = new C();  
36.         D d = new D();  
37.           
38.         System.out.println("1--" + a1.show(b));  
39.         System.out.println("2--" + a1.show(c));  
40.         System.out.println("3--" + a1.show(d));  
41.         System.out.println("4--" + a2.show(b));  
42.         System.out.println("5--" + a2.show(c));  
43.         System.out.println("6--" + a2.show(d));  
44.         System.out.println("7--" + b.show(b));  
45.         System.out.println("8--" + b.show(c));  
46.         System.out.println("9--" + b.show(d));        
47.     }  
48. }  

```

运行结果：

![](D:\workspace\Github\node\瑞秋\answer\assets\20160607110950217.png)

分析如下：

①②③比较好理解，一般不会出错。④⑤就有点糊涂了，为什么输出的不是"B and B”呢？

​    **当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。**（但是如果强制把超类转换成子类的话，就可以调用子类中新添加而超类没有的方法了。）

### 继承链调用优先级

==在继承链中对象方法的调用存在一个优先级：this.show(O)、super.show(O)、this.show((super)O)、super.show((super)O)。==

上面程序中的A,B,C,D存在如下关系：

![](D:\workspace\Github\node\瑞秋\answer\assets\20160607112132795.png)

分析4：

​    a2.show(b)，a2是一个引用变量，类型为A，则this为a2，b是B的一个实例，于是它到类A里面找show(B obj)方法，没有找到，于是到A的super(超类)找，而A没有超类，因此转到第三优先级this.show((super)O)，this仍然是a2，这里O为B，(super)O即(super)B即A，因此它到类A里面找show(A obj)的方法，类A有这个方法，但是由于a2引用的是类B的一个对象，B覆盖了A的show(A obj)方法，因此最终锁定到类B的show(A obj)，输出为"B and A”。

分析5：

​    a2.show(c)，a2是A类型的引用变量，所以this就代表了A，a2.show(c),它在A类中找发现没有找到，于是到A的超类中找(super)，由于A没有超类（Object除外），所以跳到第三级，也就是this.show((super)O)，C的超类有B、A，所以(super)O为B、A，this同样是A，这里在A中找到了show(A obj)，同时由于a2是B类的一个引用且B类重写了show(A obj)，因此最终会调用子类B类的show(A obj)方法，结果也就是B and A。

分析8：

​    b.show(c)，b是一个引用变量，类型为B，则this为b，c是C的一个实例，于是它到类B找show(C obj)方法，没有找到，转而到B的超类A里面找，A里面也没有，因此也转到第三优先级this.show((super)O)，this为b，O为C，(super)O即(super)C即B，因此它到B里面找show(B obj)方法，找到了，由于b引用的是类B的一个对象，因此直接锁定到类B的show(B obj)，输出为"B and B”。

​    按照同样的方法我也可以确认其他的答案。

   **当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。**这我们用一个例子来说明这句话所代表的含义：a2.show(b)；

​    这里a2是引用变量，为A类型，它引用的是B对象，因此按照上面那句话的意思是说有B来决定调用谁的方法,所以a2.show(b)应该要调用B中的show(B obj)，产生的结果应该是“B and B”，但是为什么会与前面的运行结果产生差异呢？这里我们忽略了后面那句话“**被调用的方法必须是在超类中定义过的**”，那么show(B obj)在A类中存在吗？根本就不存在！所以这句话在这里不适用？那么难道是这句话错误了？非也！其实这句话还隐含这这句话：**它仍然要按照继承链中调用方法的优先级来确认**。所以它才会在A类中找到show(A obj)，同时由于B重写了该方法所以才会调用B类中的方法，否则就会调用A类中的方法。

 

# [抽象类与接口](https://www.cnblogs.com/chenssy/p/3376708.html)

> **接口和内部类为我们提供了一种将接口与实现分离的更加结构化的方法。**

​      抽象类与接口是java语言中对抽象概念进行定义的两种机制，正是由于他们的存在才赋予java强大的面向对象的能力。他们两者之间对抽象概念的支持有很大的相似，甚至可以互换，但是也有区别。

# 一、抽象类

​      我们都知道在面向对象的领域一切都是对象，同时所有的对象都是通过类来描述的，但是并不是所有的类都是来描述对象的。如果一个类没有足够的信息来描述一个具体的对象，而需要其他具体的类来支撑它，那么这样的类我们称它为抽象类。比如new Animal()，我们都知道这个是产生一个动物Animal对象，但是这个Animal具体长成什么样子我们并不知道，它没有一个具体动物的概念，所以他就是一个抽象类，需要一个具体的动物，如狗、猫来对它进行特定的描述，我们才知道它长成啥样。

​      在面向对象领域由于抽象的概念在问题领域没有对应的具体概念，所以用以表征抽象概念的抽象类是不能实例化的。

​      同时，抽象类体现了数据抽象的思想，是实现多态的一种机制。它定义了一组抽象的方法，至于这组抽象方法的具体表现形式有派生类来实现。同时抽象类提供了继承的概念，它的出发点就是为了继承，否则它没有存在的任何意义。所以说定义的抽象类一定是用来继承的，同时在一个以抽象类为节点的继承关系等级链中，叶子节点一定是具体的实现类。（不知这样理解是否有错!!!高手指点….）

​      在使用抽象类时需要注意几点：

​         1、抽象类不能被实例化，实例化的工作应该交由它的子类来完成，它只需要有一个引用即可。

​         2、抽象方法必须由子类来进行重写。

​         3、只要包含一个抽象方法的抽象类，该方法必须要定义成抽象类，不管是否还包含有其他方法。

​         4、抽象类中可以包含具体的方法，当然也可以不包含抽象方法。

​         5、子类中的抽象方法不能与父类的抽象方法同名。

​         6、abstract不能与final并列修饰同一个类。

​         7、abstract 不能与private、static、final或native并列修饰同一个方法。、

​      **实例：**

​      定义一个抽象动物类Animal，提供抽象方法叫cry()，猫、狗都是动物类的子类，由于cry()为抽象方法，所以Cat、Dog必须要实现cry()方法。如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public abstract class Animal {
    public abstract void cry();
}

public class Cat extends Animal{

    @Override
    public void cry() {
        System.out.println("猫叫：喵喵...");
    }
}

public class Dog extends Animal{

    @Override
    public void cry() {
        System.out.println("狗叫:汪汪...");
    }

}

public class Test {

    public static void main(String[] args) {
        Animal a1 = new Cat();
        Animal a2 = new Dog();
        
        a1.cry();
        a2.cry();
    }
}

--------------------------------------------------------------------
Output:
猫叫：喵喵...
狗叫:汪汪...
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

​      创建抽象类和抽象方法非常有用,因为他们可以使类的抽象性明确起来,并告诉用户和编译器打算怎样使用他们.抽象类还是有用的重构器,因为它们使我们可以很容易地将公共方法沿着继承层次结构向上移动。（From:Think in java ）

# 二、接口

​      接口是抽象方法的集合。一个类实现一个或多个接口，因此继承了接口的抽象方法.

**接口的特点**

- 不能实例化

- 没有构造体

- 所有方法都是抽象的 (abstract).同时也是隐式的 public. 也就是说声明时, 可以省略 public abstract.

- 只能含有声明为 final static 的 field​   

  

  ​        Java接口是一系列方法的声明，是一些方法特征的集合，一个接口只有方法的特征没有方法的实现，因此这些方法可以在不同的地方被不同的类实现，而这些实现可以具有不同的行为（功能）。 

  

  JAVA的类可以被实现许多个接口，然而一个接口则无法实现其他的接口。

  

  ​	Java接口本身没有任何实现，因为Java接口不涉及表象，而只描述public行为，所以Java接口比Java抽象类更抽象化。但是接口不是类，不能使用new 运算符实例化一个接口。 

  ==（下面的不懂！）==

  ```
  接口通常被使用在Java编程语言，用来做回调函数使用[2] 。Java并不允许方法作为参数传递使用，因此，其中一个解决办法则是可以定义一个接口，把这个接口当成方法的参数，以此来使用该项对象的方法签名。
  
  子接口[编辑] 
  接口可以被延伸为数个不同的接口，可以使用上述所描述的方法，举例来说：
  
  
   public interface VenomousPredator extends Predator, Venomous {
           //介面主體
   }
  
  
  以上的程序片段是合法定义的子接口，与类不同的是，接口允许多重继承，而Predator 及 Venomous 可能定义或是继承相同的方法，比如说kill(Prey prey)，当一个类实现VenomousPredator的时候，它将同时实现这两种方法。
  ```

  

​      接口是一种比抽象类更加抽象的“类”。这里给“类”加引号是我找不到更好的词来表示，但是我们要明确一点就是，接口本身就不是类，从我们不能实例化一个接口就可以看出。如new Runnable();肯定是错误的，我们只能new它的实现类。

​      接口是用来建立类与类之间的协议，它所提供的只是一种形式，而没有具体的实现。同时实现该接口的实现类必须要实现该接口的所有方法，通过使用implements关键字，他表示该类在遵循某个或某组特定的接口，同时也表示着“interface只是它的外貌，但是现在需要声明它是如何工作的”。

​      接口是抽象类的延伸，java了保证数据安全是不能多重继承的，也就是说继承只能存在一个父类，但是接口不同，一个类可以同时实现多个接口，不管这些接口之间有没有关系，所以接口弥补了抽象类不能多重继承的缺陷，但是推荐继承和接口共同使用，因为这样既可以保证数据安全性又可以实现多重继承。

​      在使用接口过程中需要注意如下几个问题：

​         1、1个Interface的方所有法访问权限自动被声明为public。确切的说只能为public，当然你可以显示的声明为protected、private，但是编译会出错！

​         2、接口中可以定义“成员变量”，或者说是不可变的常量，因为接口中的“成员变量”会自动变为为public static final。可以通过类命名直接访问：ImplementClass.name。

​         3、接口中不存在实现的方法。

​         4、实现接口的非抽象类必须要实现该接口的所有方法。抽象类可以不用实现。

​         5、不能使用new操作符实例化一个接口，但可以声明一个接口变量，该变量必须引用（refer to)一个实现该接口的类的对象。可以使用 instanceof 检查一个对象是否实现了某个特定的接口。例如：if(anObject instanceof Comparable){}。

​         6、在实现多接口的时候一定要避免方法名的重复。

# 三、抽象类与接口的区别

​      尽管抽象类和接口之间存在较大的相同点，甚至有时候还可以互换，但这样并不能弥补他们之间的差异之处。下面将从语法层次和设计层次两个方面对抽象类和接口进行阐述。

## 3.1语法层次

​      在语法层次，java语言对于抽象类和接口分别给出了不同的定义。下面已Demo类来说明他们之间的不同之处。

​      使用抽象类来实现:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public abstract class Demo {
    abstract void method1();
    
    
    void method2(){
        //实现
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

​      使用接口来实现

```
interface Demo {
    void method1();
    void method2();
}
```

​      抽象类方式中，抽象类可以拥有任意范围的成员数据，同时也可以拥有自己的非抽象方法，但是接口方式中，它仅能够有静态、不能修改的成员数据（但是我们一般是不会在接口中使用成员数据），同时它所有的方法都必须是抽象的。在某种程度上来说，接口是抽象类的特殊化。

​      对子类而言，它只能继承一个抽象类（这是java为了数据安全而考虑的），但是却可以实现多个接口。

## 3.2设计层次

​      上面只是从语法层次和编程角度来区分它们之间的关系，这些都是低层次的，要真正使用好抽象类和接口，我们就必须要从较高层次来区分了。只有从设计理念的角度才能看出它们的本质所在。一般来说他们存在如下三个不同点：

​      1、 抽象层次不同。抽象类是对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。

​      2、 跨域不同。抽象类所跨域的是具有相似特点的类，而接口却可以跨域不同的类。我们知道抽象类是从子类中发现公共部分，然后泛化成抽象类，子类继承该父类即可，但是接口不同。实现它的子类可以不存在任何关系，共同之处。例如猫、狗可以抽象成一个动物类抽象类，具备叫的方法。鸟、飞机可以实现飞Fly接口，具备飞的行为，这里我们总不能将鸟、飞机共用一个父类吧！所以说抽象类所体现的是一种继承关系，要想使得继承关系合理，父类和派生类之间必须存在"[is](http://www.mydown.com/soft/network/chat/475/444475.shtml)-a" 关系，即父类和派生类在概念本质上应该是相同的。对于接口则不然，并不要求接口的实现者和接口定义在概念本质上是一致的， 仅仅是实现了接口定义的契约而已。

​      3、 设计层次不同。对于抽象类而言，它是自下而上来设计的，我们要先知道子类才能抽象出父类，而接口则不同，它根本就不需要知道子类的存在，只需要定义一个规则即可，至于什么子类、什么时候怎么实现它一概不知。比如我们只有一个猫类在这里，如果你这是就抽象成一个动物类，是不是设计有点儿过度？我们起码要有两个动物类，猫、狗在这里，我们在抽象他们的共同点形成动物抽象类吧！所以说抽象类往往都是通过重构而来的！但是接口就不同，比如说飞，我们根本就不知道会有什么东西来实现这个飞接口，怎么实现也不得而知，我们要做的就是事前定义好飞的行为接口。所以说抽象类是自底向上抽象而来的，接口是自顶向下设计出来的。 

​      我们有一个Door的抽象概念，它具备两个行为open()和close()，此时我们可以定义通过抽象类和接口来定义这个抽象概念：

​      抽象类：

```
abstract class Door{
    abstract void open();
    abstract void close()；
}
```

​      接口

```
interface Door{
    void open();
    void close();
}
```

​      至于其他的具体类可以通过使用extends使用抽象类方式定义Door或者Implements使用接口方式定义Door，这里发现两者并没有什么很大的差异。

​      但是现在如果我们需要门具有报警的功能，那么该如何实现呢？

​      **解决方案一**：给Door增加一个报警方法:clarm();

```
abstract class Door{
    abstract void open();
    abstract void close();
    abstract void alarm();
}
```

​      或者

```
interface Door{
    void open();
    void close();
    void alarm();
}
```

​      这种方法违反了面向对象设计中的一个核心原则 ISP (Interface Segregation Principle)—见批注，在Door的定义中把Door概念本身固有的行为方法和另外一个概念"报警器"的行为方 法混在了一起。这样引起的一个问题是那些仅仅依赖于Door这个概念的模块会因为"报警器"这个概念的改变而改变，反之依然。

​      **解决方案二**

​      既然open()、close()和alarm()属于两个不同的概念，那么我们依据ISP原则将它们分开定义在两个代表两个不同概念的抽象类里面，定义的方式有三种：

​      1、两个都使用抽象类来定义。

​      2、两个都使用接口来定义。

​      3、一个使用抽象类定义，一个是用接口定义。

​      由于java不支持多继承所以第一种是不可行的。后面两种都是可行的，但是选择何种就反映了你对问题域本质的理解。

​      如果选择第二种都是接口来定义，那么就反映了两个问题：1、我们可能没有理解清楚问题域，AlarmDoor在概念本质上到底是门还报警器。2、如果我们对问题域的理解没有问题，比如我们在分析时确定了AlarmDoor在本质上概念是一致的，那么我们在设计时就没有正确的反映出我们的设计意图。因为你使用了两个接口来进行定义，他们概念的定义并不能够反映上述含义。

​      第三种，如果我们对问题域的理解是这样的：AlarmDoor本质上Door，但同时它也拥有报警的行为功能，这个时候我们使用第三种方案恰好可以阐述我们的设计意图。AlarmDoor本质是们，所以对于这个概念我们使用抽象类来定义，同时AlarmDoor具备报警功能，说明它能够完成报警概念中定义的行为功能，所以alarm可以使用接口来进行定义。如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
abstract class Door{
    abstract void open();
    abstract void close();
}

interface Alarm{
    void alarm();
}

class AlarmDoor extends Door implements Alarm{
    void open(){}
    void close(){}
    void alarm(){}
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

​      这种实现方式基本上能够明确的反映出我们对于问题领域的理解，正确的揭示我们的设计意图。其实抽象类表示的是"is-a"关系，接口表示的是"like-a"关系，大家在选择时可以作为一个依据，当然这是建立在对问题领域的理解上的，比如：如果我们认为AlarmDoor在概念本质上是报警器，同时又具有Door的功能，那么上述的定义方式就要反过来了。

​      **批注：**

> ISP（Interface Segregation Principle）：面向对象的一个核心原则。它表明使用多个专门的接口比使用单一的总接口要好。
>
>   一个类对另外一个类的依赖性应当是建立在最小的接口上的。
>
>   一个接口代表一个角色，不应当将不同的角色都交给一个接口。没有关系的接口合并在一起，形成一个臃肿的大接口，这是对角色和接口的污染。

# 四、总结

​      1、 抽象类在java语言中所表示的是一种继承关系，一个子类只能存在一个父类，但是可以存在多个接口。

​      2、 在抽象类中可以拥有自己的成员变量和非抽象类方法，但是接口中只能存在静态的不可变的成员数据（不过一般都不在接口中定义成员数据），而且它的所有方法都是抽象的。

​      3、抽象类和接口所反映的设计理念是不同的，抽象类所代表的是“is-a”的关系，而接口所代表的是“like-a”的关系。



# **什么是对象 (Object)?**

- 对象是程序运行时的实体
- 它的状态存储在 fields (也就是变量)
- 行为是通过方法 (method) 实现的
- 方法上操作对象的内部的状态
- 方法是对象对对象的通信的主要手段

# **一个类是由哪些变量构成的?**

- Local variable 本地变量
- instance variables 实例变量
- class variables 类变量

Local variable
在方法体, 构造体内部定义的变量 在方法结束的时候就被摧毁
instance variables
在类里但是不在方法里 在类被载入的时候被实例化
class variables
在类里但在方法外, 加了 static 关键字. 也可以叫做静态变量

# **静态变量和实例变量的区别？**

在语法定义上的区别：静态变量前要加static关键字，而实例变量前则不加。
在程序运行时的区别：实例变量属于某个对象的属性， 必须创建了实例对象(比如 new 一个)， 其中的实例变量才会被分配空间， 才能使用这个实例变量. 静态变量不属于某个实例对象， 而是属于类， 所以也称为类变量， 只要程序加载了类的字节码， 不用创建任何实例对象, 静态变量就会被分配空间, 静态变量就可以被使用了.

# 接口和抽象的区别

抽象类可以有构造方法 接口不行

抽象类可以有普通成员变量 接口没有

抽象类可以有非抽象的方法 接口必须全部抽象

抽象类的访问类型都可以 接口只能是 public abstract

**一个类可以实现多个接口 但只能继承一个抽象类**

 基础概念题

下面哪一项说法是正确的

```
1. 在一个子类里,一个方法不是 public 就不能重载
2. 覆盖一个方法只需要满足相同的方法名和参数类型
3. 覆盖一个方法必须方法名,参数和返回类型都相同
4. 一个覆盖的方法必须有相同的方法名,参数名和参数类型
```

答案 3

覆盖函数与被覆盖函数只有函数体不同

下面哪一项说法是错误的

```
1. 重载函数的函数名必须相同
2. 重载函数必须在参数个数或类型上有所不同
3. 重载函数的返回值必须相同
4. 重载函数的函数体可以不同
```

答案 3

函数的重载与函数的返回值无关

下面哪一项说法是正确的

```
1. 静态方法不能被覆盖
2. 静态方法不能被声明称私有
3. 私有方法不能被重载
4. 一个重载的方法在基类中不通过检查不能抛出异常
```

答案 1

# 基础程序题

**题目一**

```
class Base{}

class Agg extends Base{
    public String getFields(){
        String name = "Agg";
        return name;
    }
}

public class Avf{
    pulic static void main(String argv[]){
        Base a = new Agg();
        //here
    }
}
```

下面哪个选项的代码替换到//here会调用getFields方法,使出书结果是Agg

```
A. System.out.println(a.getFields());
B. System.out.println(a.name);
C. System.out.println((Base)a.getFields());
D. System.out.println(((Agg)a).getFields());
```

答案 D

Base 类要引用 Agg 类的实例需要把 Base 类显示地转换成 Agg 类,然后调用 Agg 类中的方法. 如果 a 是 Base 类的一个实例,是不存在这个方法的,必须把 a 转换成 Agg 的一个实例

**题目二**

```
class A{

    public A(){
        System.out.println("A");
    }
}

public class B extends A{

    public B(){
        System.out.println("B");
    }

    public static void main(String[] args){
        A a = new B();
        a = new A();
    }
}
```

输出结果是 A B A

**题目三**

```
class A{
    public void print(){
        System.out.println("A");
    }
}

class B extends A{
    public void print(){
        System.out.println("B");
    }
}

public class Test{
    ..
    B objectB = new B();
    objectB.print();

    A as = (A) objectB;
    as.print();

    A asg = objectB;
    asg.print();

    as = new A();
    as.print();
    ..
}
```

输出为 B B B A

**题目四**

```
public class Test {
    public static void main(String[] args){
        Father father = new Father();
        Father child = new Child();
        System.out.println(father.getName());
        System.out.println(child.getName());
    }
}

class Father{
    public static String getName(){
        return "Father";
    }
}

class Child extends Father{
    public static String getName(){
        return "Child";
    }
}
```

输出是 Father Father 因为这里的方法 getName 是静态的. 具体执行哪一个,则要看是由哪个类来调用的.

 

# super 关键词

- 调用父类 (Superclass) 的成员或者方法
- 调用父类的构造函数

注意: 构造体如果没有显式的调用父类的构造体, Java 编译器自动调用父类的无参构造. 如果父类没有无参构造, 就会报错 ( compile-time error). 



# [Java中this和super的用法总结](https://www.cnblogs.com/hasse/p/5023392.html)

这几天看到类在继承时会用到**this**和**super**，这里就做了一点总结，与各位共同交流，有错误请各位指正~

**this**

this是自身的一个对象，代表对象本身，可以理解为：指向对象本身的一个指针。

this的用法在java中大体可以分为3种：

**1.普通的直接引用**

这种就不用讲了，this相当于是指向当前对象本身。

**2.形参与成员名字重名，用this来区分：**

```java
class Person {
    private int age = 10;
    public Person(){
    System.out.println("初始化年龄："+age);
}
    public int GetAge(int age){
        this.age = age;
        return this.age;
    }
}
 
public class test1 {
    public static void main(String[] args) {
        Person Harry = new Person();
        System.out.println("Harry's age is "+Harry.GetAge(12));
    }
}  
```



**运行结果：**

**初始化年龄：10**
**Harry's age is 12**

可以看到，这里age是GetAge成员方法的形参，this.age是Person类的成员变量。

**3.引用构造函数**

这个和super放在一起讲，见下面。

**super**

super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。

super也有三种用法：

1.普通的直接引用

与this类似，super相当于是指向当前对象的父类，这样就可以用super.xxx来引用父类的成员。

2.子类中的成员变量或方法与父类中的成员变量或方法同名

```java
class Country {
    String name;
    void value() {
       name = "China";
    }
}
  
class City extends Country {
    String name;
    void value() {
    name = "Shanghai";
    super.value();      //调用父类的方法
    System.out.println(name);
    System.out.println(super.name);
    }
  
    public static void main(String[] args) {
       City c=new City();
       c.value();
       }
}
```



**运行结果：**

**Shanghai**
**China**

可以看到，这里既调用了父类的方法，也调用了父类的变量。若不调用父类方法value()，只调用父类变量name的话，则父类name值为默认值null。

3.引用构造函数

super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。

this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。

```java
class Person { 
    public static void prt(String s) { 
       System.out.println(s); 
    } 
   
    Person() { 
       prt("父类·无参数构造方法： "+"A Person."); 
    }//构造方法(1) 
    
    Person(String name) { 
       prt("父类·含一个参数的构造方法： "+"A person's name is " + name); 
    }//构造方法(2) 
} 
    
public class Chinese extends Person { 
    Chinese() { 
       super(); // 调用父类构造方法（1） 
       prt("子类·调用父类”无参数构造方法“： "+"A chinese coder."); 
    } 
    
    Chinese(String name) { 
       super(name);// 调用父类具有相同形参的构造方法（2） 
       prt("子类·调用父类”含一个参数的构造方法“： "+"his name is " + name); 
    } 
    
    Chinese(String name, int age) { 
       this(name);// 调用具有相同形参的构造方法（3） 
       prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
    
    public static void main(String[] args) { 
       Chinese cn = new Chinese(); 
       cn = new Chinese("codersai"); 
       cn = new Chinese("codersai", 18); 
    } 
}
```



**运行结果：**

**父类·无参数构造方法： A Person.**
**子类·调用父类”无参数构造方法“： A chinese coder.**
**父类·含一个参数的构造方法： A person's name is codersai**
**子类·调用父类”含一个参数的构造方法“： his name is codersai**
**父类·含一个参数的构造方法： A person's name is codersai**
**子类·调用父类”含一个参数的构造方法“： his name is codersai**
**子类：调用子类具有相同形参的构造方法：his age is 18**

从本例可以看到，可以用super和this分别调用父类的构造方法和本类中其他形式的构造方法。

例子中Chinese类第三种构造方法调用的是本类中第二种构造方法，而第二种构造方法是调用父类的，因此也要先调用父类的构造方法，再调用本类中第二种，最后是重写第三种构造方法。

 

**super和this的异同：**

- super（参数）：调用基类中的某一个构造函数（应该为构造函数中的第一条语句） 
- this（参数）：调用本类中另一种形成的构造函数（应该为构造函数中的第一条语句）
- super:　它引用当前对象的直接父类中的成员（用来访问直接父类中被隐藏的父类中成员数据或函数，基类与派生类中有相同成员定义时如：super.变量名    super.成员函数据名（实参）
- this：它代表当前对象名（在程序中易产生二义性之处，应使用this来指明当前对象；如果函数的形参与类中的成员数据同名，这时需用this来指明成员变量名）
- 调用super()必须写在子类构造方法的第一行，否则编译不通过。每个子类构造方法的第一条语句，都是隐含地调用super()，如果父类没有这种形式的构造函数，那么在编译的时候就会报错。
- super()和this()类似,区别是，super()从子类中调用父类的构造方法，this()在同一类内调用其它方法。
- super()和this()均需放在构造方法内第一行。
- 尽管可以用this调用一个构造器，但却不能调用两个。
- this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。
- this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。
- 从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。

**题目二**

```
import java.util.Date;

public class Test extends Date{

    public static void main(String[] args) {
       new Test().test();
    }

    public void test(){
       System.out.println(super.getClass().getName());
    }
}
```

返回的结果是 Test

因为super.getClass().getName() 调用了父类的 getClass() 方法, 返回当前类

如果想得到父类的名称，应该用如下代码：

```
getClass().getSuperClass().getName()
```

**题目三**

```
public abstract class Car {

    String name = "Car";

    public String getName(){
        return name;
    }

    public abstract void demarre();
}

public class B extends Car{
    String name = "B";

    public String getName(){
        return name;
    }

    public void demarre() {
        System.out.println(getName() + " demarre");
    }
}

public class C extends B{
    String name = "C";

    public String getName(){
        return name;
    }

    public void demarreWithSuper() {
        System.out.println(super.getName() + " demarre");
    }

    public void demarreNoSuper() {
        System.out.println(getName() + " demarre");
    }
}

public class D extends B{
    public String getName(){
        return name;
    }

    public void demarreNoSuper() {
        System.out.println(getName() + " demarre");
    }
}

public class Test {
    public static void main(String[] args) {
        B b = new B();
        b.demarre();

        Car bCar = new B();
        bCar.demarre();

        C c = new C();
        c.demarre(); // c 里并没有定义这个函数
        c.demarreWithSuper();
        c.demarreNoSuper();

        D d = new D();
        d.demarre();

        transfer(c);    // TransferC
        transfer((B)c); // TransferB
        transfer(d);    // TransferB
    }

        public static void transfer(B b){
            System.out.println("TransferB");
            b.demarre();
        }

        public static void transfer(C c){
            System.out.println("TransferC");
            c.demarre();
        }
    }
}
```

输出是 B demarre B demarre C demarre B demarre C demarre B demarre TransferC C demarre TransferB C demarre TransferB B demarre



# Static 关键字

Static 关键字表明一个成员变量或者是成员方法可以在没有所属的类的实例的情况下直接被访问

声明为 **static 的方法**有以下几条限制：

1. 仅能调用其他的 static 方法
2. 只能访问 static 变量.
3. 不能以任何方式引用 this 或 super
4. 不能被覆盖.

声明为 **static 的变量**实质上就是全局变量. (+ final 就是全局**常**量). 当声明一个对象时, 并不产生 static 变量的拷贝, 而是该类所有的实例变量共用同一个 static 变量

# Static 相关问题

### Static 关键字是什么意思？

Static 关键字表明一个成员变量或者是成员方法可以在没有所属的类的实例的情况下直接被访问

### 是否可以 override 一个 static 的方法？

不能被覆盖. 因为方法覆盖是基于运行时动态绑定的, 而 static 方法是编译时静态绑定的.

### 一个 static 方法内部调用非 static 方法?

不可以. 因为非 static 方法是要与对象关联在一起的, 须创建一个对象的实例后, 才可以在该对象上进行方法调用, 而static方法调用时不需要创建对象, 可以直接调用. 也就是说, 当一个 static 方法被调用时, 可能还没有创建任何实例对象, 如果从一个 static 方法中发出对非 static 方法的调用, 那个非 static 方法是关联到哪个对象上的呢? 这个逻辑无法成立, 所以, 一个 static 方法内部发出对非 static 方法的调用.

### 是否可以在 static 环境中访问非 static 变量？

同上



# Path 与 Classpath?

Path 和 Classpath 是操作系统的环境变量.

- Path 定义了系统可以在哪里找到可执行文件(.exe)
- classpath 定义了 .class 文件的位置.



# final 关键字

final 类是不能被继承的 这个类就是最终的了 不需要再继承修改 比如很多 java 标准库就是 final 类

final 方法不能被子方法重写

final + static 变量表示常量

# 一个 .java 源文件是否可以包含多个类

可以得

但只能有一个是 public 的类 而且这个 public 类必须与文件名一样

# & 与 &&

都可以表示逻辑与 and, 但是 && 具有短路功能 第一个表达式错了 第二个就被忽略了。`&` 的表达式是先计算后求与。

除此外 & 可以用作位运算符

# == 和 equal 的区别

- **== 比较引用的地址**
- **equel 比较引用的内容** (Object 类本身除外)

# 作用域的区别

| 作用域    | 当前类 | 同一个 package | 子孙类 | 其他 package |
| --------- | ------ | -------------- | ------ | ------------ |
| public    | O      | O              | O      | O            |
| protected | O      | O              | O      | X            |
| friendly  | O      | O              | X      | X            |
| private   | O      | X              | X      | X            |



# 异常

异常是指java程序运行时（非编译）所发生的非正常情况或错误

Java使用面向对象的方式来处理异常，它把程序中发生的每个异常也都分别封装到一个对象来表示的，该对象中包含有异常的信息

Java对异常进行了分类，所有异常的根类为java.lang.Throwable

Throwable下面又派生了两个子类：Error和Exception



## 异常分类

在java中，异常对象都是派生于**Throwable**类的一个实例。如果java内置的异常类不能够满足需求，用户还可以创建自己的异常类。

**下图是java异常类层次结构图** 
![](D:\workspace\Github\node\瑞秋\answer\assets\20180309211054853.jpg)
可以看出，所有的异常都是由**Throwable**类，下一层分解为两个分支：**Error**和**Exceprion**。 
**Error**层次结构描述了java运行时系统的内部错误和资源耗尽错误。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。应用程序不应该抛出这种类型的对象。 
**Exceprion**这个层次结构又分解为连个分支：一个分支派生于**RuntimeException**；另一个分支包含其他异常。划分两个分支的规则是：由程序错误导致的异常属于RuntimeException；而程序本身没有没有问题，但由于像I/O错误这类异常导致的异常属于其他异常。 
**常见的RuntimeException（运行时异常）：** 
IndexOutOfBoundsException(下标越界异常) 
NullPointerException(空指针异常) 
NumberFormatException （String转换为指定的数字类型异常） 
ArithmeticException -（算术运算异常 如除数为0） 
ArrayStoreException - （向数组中存放与声明类型不兼容对象异常） 
SecurityException -（安全异常） 
**IOException（其他异常）** 
FileNotFoundException（文件未找到异常。） 
IOException（操作输入流和输出流时可能出现的异常。） 
EOFException （文件已结束异常）

### 概念理解

首先明白下面的两个概念 
**unchecked exception（非检查异常）**：runtime 阶段碰到的异常. 在编译的时候不需要检查 (checked)。包括运行时异常（RuntimeException）和派生于Error类的异常。对于运行时异常，java编译器不要求必须进行异常捕获处理或者抛出声明，由程序员自行决定。 
**checked exception（检查异常，编译异常，必须要处理的异常）** ：在编译阶段的异常，并且强制检查。也称非运行时异常（运行时异常以外的异常就是非运行时异常），java编译器强制程序员必须进行捕获处理，比如常见的IOExeption和SQLException。对于非运行时异常如果不进行捕获或者抛出声明处理，编译都不会通过。

​	编译器**强制 checked 异常必须try..catch处理或用throws声明**继续抛给上层调用方法处理, 这就是为什么叫**checked异常**, 而 Runtime 异常可以处理也可以不处理, 所以, 编译器不强制用try..catch处理或用throws声明, 所以 Runtime 异常也称为unchecked异常	

==finally块的语句在try或catch中的**return语句执行之后返回之前执**行且finally里的修改语句可能影响也可能不影响try或catch中 return已经确定的返回值，若finally里也有return语句则覆盖try或catch中的return语句直接返回。== 

​	Java提供了两类主要的异常:runtime exception和checked exception。checked 异常也就是我们经常遇到的IO异常，以及SQL异常都是这种异常。对于这种异常，JAVA编译器强制要求我们必需对出现的这些异常进行catch。所以，面对这种异常不管我们是否愿意，只能自己去写一大堆catch块去处理可能的异常。这类异常一般是外部错误,例如试图从文件尾后读取数据等,这并不是程序本身的错误,而是在应用环境中出现的外部错误.

​	但是另外一种异常：runtime exception，也称运行时异常，我们可以不处理。当出现这样的异常时，总是由虚拟机接管。

​	出现运行时异常后，系统会把异常一直往上层抛，一直遇到处理代码。如果没有处理块，到最上层，如果是多线程就由Thread.run()抛出，如果是单线程就被main()抛出。抛出之后，如果是线程，这个线程也就退出了。如果是主程序抛出的异常，那么这整个程序也就退出了。运行时异常是Exception的子类，也有一般异常的特点，是可以被Catch块处理的。只不过往往我们不对他处理罢了。也就是说，你如果不对运行时异常进行处理，那么出现运行时异常之后，要么是线程中止，要么是主程序终止。 



# 处理异常的方法

1. try catch.
2. throws.

这两种方法有什么区别

第一种方法是自己处理异常.

第二种异常是把异常抛给调用这个方法的模块去处理. 一般 Java 的库就是怎么处理的.



# 字节流与字符流

- 字节流继承于InputStream OutputStream
- 字符流继承于InputStreamReader OutputStreamWriter

字符流使用了缓冲区 (buffer),而字节流没有使用缓冲区

底层设备永远只接受字节数据

字符是字节通过不同的编码的包装

字符向字节转换时，要注意编码的问题

​	缓冲区可以简单地理解为一段内存区域。可以简单地把缓冲区理解为一段特殊的内存。某些情况下，如果一个程序频繁地操作一个资源（如文件或数据库），则性能会很低，此时为了提升性能，就可以将一部分数据暂时读入到内存的一块区域之中，以后直接从此区域中读取数据即可，因为读取内存速度会比较快，这样可以提升程序的性能。在字符流的操作中，所有的字符都是在内存中形成的，在输出前会将所有的内容暂时保存在内存之中，所以使用了缓冲区暂存数据。如果想在不关闭时也可以将字符流的内容全部输出，则可以使用Writer类中的flush()方法完成。 

​	字节流在操作的时候本身是不会用到缓冲区（内存）的，是与文件本身直接操作的，而字符流在操作的时候是使用到缓冲区的

​	字节流在操作文件时，即使不关闭资源（close方法），文件也能输出，但是如果字符流不使用close方法的话，则不会输出任何内容，说明字符流用的是缓冲区，并且可以使用flush方法强制进行刷新缓冲区，这时才能在不close的情况下输出内容

​	那开发中究竟用字节流好还是用字符流好呢？

​	在所有的硬盘上保存文件或进行传输的时候都是以字节的方法进行的，包括图片也是按字节完成，而字符是只有在内存中才会形成的，所以使用字节的操作是最多的。

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



1、sleep是Thread的静态方法，wait是Object的方法，任何对象实例都能调用。

2、sleep不会释放锁，它也不需要占用锁。wait会释放锁，但调用它的前提是当前线程占有锁(即代码要在synchronized中)。

3、它们都可以被interrupted方法中断。

具体来说：

Thread.Sleep(1000) 意思是在未来的1000毫秒内本线程不参与CPU竞争，1000毫秒过去之后，这时候也许另外一个线程正在使用CPU，那么这时候操作系统是不会重新分配CPU的，直到那个线程挂起或结束，即使这个时候恰巧轮到操作系统进行CPU 分配，当前线程也不一定就是总优先级最高的那个，CPU还是可能被其他线程抢占去。另外值得一提的是Thread.Sleep(0)的作用，就是触发操作系统立刻重新进行一次CPU竞争，竞争的结果也许是当前线程仍然获得CPU控制权，也许会换成别的线程获得CPU控制权。

wait(1000)表示将锁释放1000毫秒，到时间后如果锁没有被其他线程占用，则再次得到锁，然后wait方法结束，执行后面的代码，如果锁被其他线程占用，则等待其他线程释放锁。注意，设置了超时时间的wait方法一旦过了超时时间，并不需要其他线程执行notify也能自动解除阻塞，但是如果没设置超时时间的wait方法必须等待其他线程执行notify。

**1、**每个对象都有一个锁来控制同步访问，Synchronized关键字可以和对象的锁交互，来实现同步方法或同步块。**sleep()方法**正在执行的线程主动让出CPU（然后CPU就可以去执行其他任务），在sleep指定时间后CPU再回到该线程继续往下执行(注意：sleep方法只让出了CPU，而并不会释放同步资源锁！！！)；**wait()方法**则是指当前线程让自己暂时退让出同步资源锁，以便其他正在等待该资源的线程得到该资源进而运行，只有调用了notify()方法，之前调用wait()的线程才会解除wait状态，可以去参与竞争同步资源锁，进而得到执行。（注意：notify的作用相当于叫醒睡着的人，而并不会给他分配任务，就是说notify只是让之前调用wait的线程有权利重新参与线程的调度）；

**2、sleep()方法**可以在任何地方使用；**wait()方法**则只能在同步方法或同步块中使用；

**3、sleep()**是线程线程类（Thread）的方法，调用会暂停此线程指定的时间，但监控依然保持，不会释放对象锁，到时间自动恢复；**wait()**是Object的方法，调用会放弃对象锁，进入等待队列，待调用notify()/notifyAll()唤醒指定的线程或者所有线程，才会进入锁池，不再次获得对象锁才会进入运行状态；



​	造成线程安全问题的主要诱因有两点，一是存在共享数据(也称临界资源)，二是存在多条线程共同操作共享数据。因此为了解决这个问题，我们可能需要这样一个方案，当存在多个线程操作共享数据时，需要保证同一时刻有且只有一个线程在操作共享数据，其他线程必须等到该线程处理完数据后再进行，这种方式有个高尚的名称叫==互斥锁==，即能达到互斥访问目的的锁，也就是说当一个共享数据被当前正在访问的线程加上互斥锁后，在同一个时刻，其他线程只能处于等待的状态，直到当前线程处理完毕释放该锁。在 Java 中，关键字 synchronized可以保证在同一个时刻，只有一个线程可以执行某个方法或者某个代码块(主要是对方法或者代码块中存在共享数据的操作)，同时我们还应该注意到synchronized另外一个重要的作用，synchronized可保证一个线程的变化(主要是共享数据的变化)被其他线程所看到（保证可见性，完全可以替代Volatile功能），这点确实也是很重要的。

# synchronized的三种应用方式

synchronized关键字最主要有以下3种应用方式，下面分别介绍

- 修饰实例方法，作用于当前实例加锁，进入同步代码前要获得当前实例的锁
- 修饰静态方法，作用于当前类对象加锁，进入同步代码前要获得当前类对象的锁
- 修饰代码块，指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。

## Java虚拟机对synchronized的优化

锁的状态总共有四种，无锁状态、偏向锁、轻量级锁和重量级锁。随着锁的竞争，锁可以从偏向锁升级到轻量级锁，再升级的重量级锁，但是锁的升级是单向的，也就是说只能从低到高升级，不会出现锁的降级，关于重量级锁，前面我们已详细分析过，下面我们将介绍偏向锁和轻量级锁以及JVM的其他优化手段，这里并不打算深入到每个锁的实现和转换过程更多地是阐述Java虚拟机所提供的每个锁的核心优化思想，毕竟涉及到具体过程比较繁琐，如需了解详细过程可以查阅《深入理解Java虚拟机原理》。

### 偏向锁

偏向锁是Java 6之后加入的新锁，它是一种针对加锁操作的优化手段，经过研究发现，在大多数情况下，锁不仅不存在多线程竞争，而且总是由同一线程多次获得，因此为了减少同一线程获取锁(会涉及到一些CAS操作,耗时)的代价而引入偏向锁。偏向锁的核心思想是，如果一个线程获得了锁，那么锁就进入偏向模式，此时Mark Word 的结构也变为偏向锁结构，当这个线程再次请求锁时，无需再做任何同步操作，即获取锁的过程，这样就省去了大量有关锁申请的操作，从而也就提供程序的性能。所以，对于没有锁竞争的场合，偏向锁有很好的优化效果，毕竟极有可能连续多次是同一个线程申请相同的锁。但是对于锁竞争比较激烈的场合，偏向锁就失效了，因为这样场合极有可能每次申请锁的线程都是不相同的，因此这种场合下不应该使用偏向锁，否则会得不偿失，需要注意的是，偏向锁失败后，并不会立即膨胀为重量级锁，而是先升级为轻量级锁。下面我们接着了解轻量级锁。

### 轻量级锁

倘若偏向锁失败，虚拟机并不会立即升级为重量级锁，它还会尝试使用一种称为轻量级锁的优化手段(1.6之后加入的)，此时Mark Word 的结构也变为轻量级锁的结构。轻量级锁能够提升程序性能的依据是“对绝大部分的锁，在整个同步周期内都不存在竞争”，注意这是经验数据。需要了解的是，轻量级锁所适应的场景是线程交替执行同步块的场合，如果存在同一时间访问同一锁的场合，就会导致轻量级锁膨胀为重量级锁。

### 自旋锁

轻量级锁失败后，虚拟机为了避免线程真实地在操作系统层面挂起，还会进行一项称为自旋锁的优化手段。这是基于在大多数情况下，线程持有锁的时间都不会太长，如果直接挂起操作系统层面的线程可能会得不偿失，毕竟操作系统实现线程之间的切换时需要从用户态转换到核心态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，因此自旋锁会假设在不久将来，当前的线程可以获得锁，因此虚拟机会让当前想要获取锁的线程做几个空循环(这也是称为自旋的原因)，一般不会太久，可能是50个循环或100循环，在经过若干次循环后，如果得到锁，就顺利进入临界区。如果还不能获得锁，那就会将线程在操作系统层面挂起，这就是自旋锁的优化方式，这种方式确实也是可以提升效率的。最后没办法也就只能升级为重量级锁了。

### 锁消除

消除锁是虚拟机另外一种锁的优化，这种优化更彻底，Java虚拟机在JIT编译时(可以简单理解为当某段代码即将第一次被执行时进行编译，又称即时编译)，通过对运行上下文的扫描，去除不可能存在共享资源竞争的锁，通过这种方式消除没有必要的锁，可以节省毫无意义的请求锁时间，如下StringBuffer的append是一个同步方法，但是在add方法中的StringBuffer属于一个局部变量，并且不会被其他线程所使用，因此StringBuffer不可能存在共享资源竞争的情景，JVM会自动将其锁消除。

## synchronized的可重入性

从互斥锁的设计上来说，当一个线程试图操作一个由其他线程持有的对象锁的临界资源时，将会处于阻塞状态，但当一个线程再次请求自己持有对象锁的临界资源时，这种情况属于重入锁，请求将会成功，在java中synchronized是基于原子性的内部锁机制，是可重入的，因此在一个线程调用synchronized方法的同时在其方法体内部调用该对象另一个synchronized方法，也就是说一个线程得到一个对象锁后再次请求该对象锁，是允许的，这就是synchronized的可重入性。如下：

```
public class AccountingSync implements Runnable{
    static AccountingSync instance=new AccountingSync();
    static int i=0;
    static int j=0;
    @Override
    public void run() {
        for(int j=0;j<1000000;j++){

            //this,当前实例对象锁
            synchronized(this){
                i++;
                increase();//synchronized的可重入性
            }
        }
    }

    public synchronized void increase(){
        j++;
    }


    public static void main(String[] args) throws InterruptedException {
        Thread t1=new Thread(instance);
        Thread t2=new Thread(instance);
        t1.start();t2.start();
        t1.join();t2.join();
        System.out.println(i);
    }
}1234567891011121314151617181920212223242526272829
```

正如代码所演示的，在获取当前实例对象锁后进入synchronized代码块执行同步代码，并在代码块中调用了当前实例对象的另外一个synchronized方法，再次请求当前实例锁时，将被允许，进而执行方法体代码，这就是重入锁最直接的体现，需要特别注意另外一种情况，当子类继承父类时，子类也是可以通过可重入锁调用父类的同步方法。注意由于synchronized是基于monitor实现的，因此每次重入，monitor中的计数器仍会加1。

### 中断与synchronized

事实上线程的中断操作对于正在等待获取的锁对象的synchronized方法或者代码块并不起作用，也就是对于synchronized来说，如果一个线程在等待锁，那么结果只有两种，要么它获得这把锁继续执行，要么它就保存等待，即使调用中断线程的方法，也不会生效。

## 等待唤醒机制与synchronized

所谓等待唤醒机制本篇主要指的是notify/notifyAll和wait方法，在使用这3个方法时，必须处于synchronized代码块或者synchronized方法中，否则就会抛出IllegalMonitorStateException异常，这是因为调用这几个方法前必须拿到当前对象的监视器monitor对象，也就是说notify/notifyAll和wait方法依赖于monitor对象，在前面的分析中，我们知道monitor 存在于对象头的Mark Word 中(存储monitor引用指针)，而synchronized关键字可以获取 monitor ，这也就是为什么notify/notifyAll和wait方法必须在synchronized代码块或者synchronized方法调用的原因。

```
synchronized (obj) {
       obj.wait();
       obj.notify();
       obj.notifyAll();         
 }12345
```

需要特别理解的一点是，与sleep方法不同的是wait方法调用完成后，线程将被暂停，但wait方法将会释放当前持有的监视器锁(monitor)，直到有线程调用notify/notifyAll方法后方能继续执行，而sleep方法只让线程休眠并不释放锁。同时notify/notifyAll方法调用后，并不会马上释放监视器锁，而是在相应的synchronized(){}/synchronized方法执行结束后才自动释放锁。



# Transient 关键字

当持久化对象时, 可能有一个特殊的对象数据成员, 我们不想用 serialization 机制来保存它. 为了在一个特定对象的一个域上关闭serialization, 可以在这个域前加上关键字transient.（比如银行卡、密码等私密信息）



## 线程的生命周期

Java语言中定义了5种线程状态，在任意一个时间点，一个线程只能有且只有其中一种状态，这5种状态是： 

- 新建（New）：创建后尚未启动的线程处于这种状态。
- 运行（Runable）：包括了操作系统线程状态中的Running和Ready，也就是处于此状态的线程有可能正在执行，也有可能正在等待着CPU为它分配执行时间。
- 无限期等待（Waiting）：处于这种状态的线程不会被分配CPU执行时间，它们要等待被其他线程显式地唤醒。以下方法会让线程陷入无限期的等待状态：
  - 没有设置timeout参数的Object.wait()方法；
  - 没有设置timeout参数的Thread.join()方法；
  - LockSupport.park()方法；
- 限期等待（Timed Waiting）：处于这种状态的线程也不会被分配CPU执行时间，不过无须等待被其他线程显式地唤醒，在一定时间之后它们会由操作系统自动唤醒。以下方法会让线程进入限期等待状态： 
  - Thread.sleep()方法；
  - 设置了timeout参数的Object.wait()方法；
  - 设置了timeout参数的Thread.join()方法；
  - LockSupport.parkNanos()方法；
  - LockSupport.parkUntil()方法；
- 阻塞（Blocked）：线程被阻塞了，“阻塞状态”与“等待状态”的区别是：“阻塞状态”在等待着获取到一个排它锁，这个事件将在另外一个线程放弃这个锁的时候发生；而“等待状态”则是在等待一段时间，或者唤醒动作的发生。在程序等待进入同步区域（synchronized）的时候，线程将进入这种状态。
- 结束（Terminated）：已终止的线程状态，线程已经结束执行。

![](D:\workspace\Github\node\瑞秋\answer\assets\v2-326a2be9b86b1446d75b6f52f54c98fb_hd.png)

## 线程间的状态转换

### 1、新建(New)

新创建了一个线程对象，还未调用start()方法。

```
Thread thread = new Thread();

```

### 2、就绪（Ready）

线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中 获取cpu 的使用权 。

### 3、运行中（Running）

可运行状态(runnable)的线程获得了cpu 时间片（timeslice） ，执行程序代码。

### 4、限期等待（Timed Waiting）

也可以称作 TIMED_WAITING（有等待时间的等待状态）。

线程主动调用以下方法：

- Thread.sleep方法；
- Object的wait方法，带有时间；
- Thread.join方法，带有时间；
- LockSupport的parkNanos方法，带有时间。

 5、无限期等待（Waiting）

运行中（Running）的线程执行了以下方法：

- Object的wait方法，并且没有使用timeout参数;
- Thread的join方法，没有使用timeout参数；
- LockSupport的park方法；
- Conditon的await方法。

### 6、阻塞（Blocked）

阻塞状态是指线程因为某种原因放弃了cpu 使用权，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得cpu timeslice 转到运行(running)状态。阻塞的情况分两种：

- 同步阻塞：运行(running)的线程进入了一个synchronized方法，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
- 其他阻塞：运行(running)的线程发出了I/O请求时，JVM会把该线程置为阻塞状态。当I/O处理完毕时，线程重新转入可运行(runnable)状态。

### 7、结束（Terminated）

线程run()、main() 方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期。



# sleep()和wait()方法与对象锁、锁池、等待池

一道Java的题目:

> 关于sleep()和wait()，以下描述错误的一项是: 
> \- A sleep是线程类（Thread）的方法，wait是Object类的方法； 
> \- B sleep不释放对象锁，wait放弃对象锁 
> \- C sleep暂停线程、但监控状态仍然保持，结束后会自动恢复 
> \- D wait后进入等待锁定池，只有针对此对象发出notify方法后获得对象锁进入运行状态

**1.关于对象锁**：

**截取网上的一段话**：

> 所有对象都自动含有单一的锁。 
> JVM负责跟踪对象被加锁的次数。如果一个对象被解锁，其计数变为0。在任务（线程）第一次给对象加锁的时候，计数变为1。每当这个相同的任务（线程）在此对象上获得锁时，计数会递增。 
> 只有首先获得锁的任务（线程）才能继续获取该对象上的多个锁。 
> 每当任务离开一个synchronized（同步）方法，计数递减，当计数为0的时候，锁被完全释放，此时别的任务就可以使用此资源。

这段话令人感到迷惑，一个对象不是只有一个锁吗？只有获得这个对象的锁才能对它进行操作，若这个对象的锁被一个线程先获得，那就其他线程就需要等待。那多次加锁什么意思，锁不是依附于对象的吗？ 
在往下的文章中，我暂且理解为一个对象有且只有一把锁，锁在不同线程间传递，一个线程可以多次获得同一个对象的锁。暂且不考虑一个对象上多个锁这种方法是不是确实存在，这对下面影响不大。

**2.关于锁池和等待池**

在Java中，每个对象都有两个池，锁(monitor)池和等待池

- 锁池:假设线程A已经拥有了某个对象(注意:不是类)的锁，而其它的线程想要调用这个对象的某个synchronized方法(或者synchronized块)，由于这些线程在进入对象的synchronized方法之前必须先获得该对象的锁的拥有权，但是该对象的锁目前正被线程A拥有，所以这些线程就进入了该对象的锁池中。
- 等待池:假设一个线程A调用了某个对象的wait()方法，线程A就会释放该对象的锁(因为wait()方法必须出现在synchronized中，这样自然在执行wait()方法之前线程A就已经拥有了该对象的锁)，同时线程A就进入到了该对象的等待池中。如果另外的一个线程调用了相同对象的notifyAll()方法，那么处于该对象的等待池中的线程就会全部进入该对象的锁池中，准备争夺锁的拥有权。如果另外的一个线程调用了相同对象的notify()方法，那么仅仅有一个处于该对象的等待池中的线程(随机)会进入该对象的锁池.

**深入理解**： 
如果线程调用了对象的 wait()方法，那么线程便会处于该对象的等待池中，等待池中的线程不会去竞争该对象的锁。 
当有线程调用了对象的 notifyAll()方法（唤醒所有 wait 线程）或 notify()方法（只随机唤醒一个 wait 线程），被唤醒的的线程便会进入该对象的锁池中，锁池中的线程会去竞争该对象锁。 
优先级高的线程竞争到对象锁的概率大，假若某线程没有竞争到该对象锁，它还会留在锁池中，唯有线程再次调用 wait()方法，它才会重新回到等待池中。而竞争到对象锁的线程则继续往下执行，直到执行完了 synchronized 代码块，它会释放掉该对象锁，这时锁池中的线程会继续竞争该对象锁。

注：wait() ,notifyAll(),notify() 三个方法都是Object类中的方法.

**3.关于wait() ,notifyAll(),notify() 三个方法**

- wait() 
  public final void wait() throws InterruptedException,IllegalMonitorStateException

该方法用来将当前线程置入休眠状态，直到接到通知或被中断为止。在调用 wait()之前，线程必须要获得该对象的对象级别锁，即只能在同步方法或同步块中调用 wait()方法。进入 wait()方法后，当前线程释放锁。在从 wait()返回前，线程与其他线程竞争重新获得锁。如果调用 wait()时，没有持有适当的锁，则抛出 IllegalMonitorStateException，它是 RuntimeException 的一个子类，因此，不需要 try-catch 结构。

- notify() 
  public final native void notify() throws IllegalMonitorStateException

该方法也要在同步方法或同步块中调用，即在调用前，线程也必须要获得该对象的对象级别锁，的如果调用 notify()时没有持有适当的锁，也会抛出 IllegalMonitorStateException。

该方法用来通知那些可能等待该对象的对象锁的其他线程。如果有多个线程等待，则线程规划器任意挑选出其中一个 wait()状态的线程来发出通知，并使它等待获取该对象的对象锁（**notify 后，当前线程不会马上释放该对象锁，wait 所在的线程并不能马上获取该对象锁，要等到程序退出 synchronized 代码块后，当前线程才会释放锁，wait所在的线程也才可以获取该对象锁**），但不惊动其他同样在等待被该对象notify的线程们。**当第一个获得了该对象锁的 wait 线程运行完毕以后，它会释放掉该对象锁，此时如果该对象没有再次使用 notify 语句，则即便该对象已经空闲，其他 wait 状态等待的线程由于没有得到该对象的通知，会继续阻塞在 wait 状态**，直到这个对象发出一个 notify 或 notifyAll。**这里需要注意：它们等待的是被 notify 或 notifyAll，而不是锁。这与下面的 notifyAll()方法执行后的情况不同。**

- notifyAll() 
  public final native void notifyAll() throws IllegalMonitorStateException

该方法与 notify ()方法的工作方式相同，重要的一点差异是：

notifyAll 使所有原来在该对象上 wait 的线程统统退出 wait 的状态（即全部被唤醒，不再等待 notify 或 notifyAll，但由于此时还没有获取到该对象锁，因此还不能继续往下执行），变成等待获取该对象上的锁，一旦该对象锁被释放（notifyAll 线程退出调用了 notifyAll 的 synchronized 代码块的时候），他们就会去竞争。如果其中一个线程获得了该对象锁，它就会继续往下执行，在它退出 synchronized 代码块，释放锁后，其他的已经被唤醒的线程将会继续竞争获取该锁，一直进行下去，直到所有被唤醒的线程都执行完毕。

**4.sleep()不会释放掉锁（监控）** 
最开始的那道题答案是D



# 一个线程的初始状态是什么?

一个线程被创建和开始（调用start方法）之后是 “Ready” 状态.

# synchronized method 和 synchronized statement?

同步方法是用来控制对对象的访问的方法。线程只在获得方法对象或类的锁之后才执行同步方法。同步语句类似于同步方法。一个同步的语句只能在线程获得了同步语句中引用的对象或类的锁之后才能执行。 



# Java 守护线程

Java的线程分为两种：User Thread(用户线程)、DaemonThread(守护线程)。 

​	Daemon的作用是为其他线程的运行提供服务，比如说GC线程。其实User Thread线程和Daemon Thread守护线程本质上来说去没啥区别的，唯一的区别之处就在虚拟机的离开：如果User Thread全部撤离，那么Daemon Thread也就没啥线程好服务的了，所以虚拟机也就退出了。

​	守护线程并非虚拟机内部可以提供，用户也可以自行的设定守护线程，方法：public final void setDaemon(boolean on) ；但是有几点需要注意：

​	1）、thread.setDaemon(true)必须在thread.start()之前设置，否则会跑出一个IllegalThreadStateException异常。你不能把正在运行的常规线程设置为守护线程。  （备注：这点与守护进程有着明显的区别，守护进程是创建后，让进程摆脱原会话的控制+让进程摆脱原进程组的控制+让进程摆脱原控制终端的控制；所以说寄托于虚拟机的语言机制跟系统级语言有着本质上面的区别）

​	2）、 在Daemon线程中产生的新线程也是Daemon的。  （这一点又是有着本质的区别了：守护进程fork()出来的子进程不再是守护进程，尽管它把父进程的进程相关信息复制过去了，但是子进程的进程的父进程不是init进程，所谓的守护进程本质上说就是“父进程挂掉，init收养，然后文件0,1,2都是/dev/null，当前目录到/”）

​	3）、不是所有的应用都可以分配给Daemon线程来进行服务，比如读写操作或者计算逻辑。因为在Daemon Thread还没来的及进行操作时，虚拟机可能已经退出了。

​	写java多线程程序时，一般比较喜欢用java自带的多线程框架，比如ExecutorService，但是java的线程池会将守护线程转换为用户线程，所以如果要使用后台线程就不能用java的线程池。
如下，线程池中将daemon线程转换为用户线程的程序片段：

```java
package com.daemon;  
   
import java.util.concurrent.ExecutorService;  
import java.util.concurrent.Executors;  
import java.util.concurrent.TimeUnit;  
   
public class DaemonThreadTest  
{  
    public static void main(String[] args)  
    {  
        Thread mainThread = new Thread(new Runnable(){  
            @Override 
            public void run()  
            {  
                ExecutorService exec = Executors.newCachedThreadPool();  
                Thread childThread = new Thread(new ClildThread());  
                childThread.setDaemon(true);  
                exec.execute(childThread);  
                exec.shutdown();  
                System.out.println("I'm main thread...");  
            }  
        });  
        mainThread.start();  
    }  
}  
   
class ClildThread implements Runnable  
{  
    @Override 
    public void run()  
    {  
        while(true)  
        {  
            System.out.println("I'm child thread..");  
            try 
            {  
                TimeUnit.MILLISECONDS.sleep(1000);  
            }  
            catch (InterruptedException e)  
            {  
                e.printStackTrace();  
            }  
        }  
    }  
}
```

​	注意到，这里不仅会将守护线程转变为用户线程，而且会把优先级转变为Thread.NORM_PRIORITY。
如下所示，将守护线程采用线程池的方式开启：

运行结果：

```java
I'm main thread...  
I'm child thread..  
I'm child thread..  
I'm child thread..  
I'm child thread..  
I'm child thread..  
I'm child thread..  
I'm child thread..  
I'm child thread..  
I'm child thread..（无限输出）
```

上面代码证实了线程池会将守护线程转变为用户线程。

# 字符串基础问题

**题目二**

```
String s = "a" + "b" + "c" + "d" + "e"
```

创建了几个对象? 1个 赋值号右边都是常量,编译时直接储存它们的字面值, 在编译时直接把结果取出来面成了 "abcde"

**题目三**

```
# 创建了几个String Object?二者之间有什么区别？

    String s = new String("xyz");
```

两个或一个, ”xyz”对应一个对象, 这个对象放在字符串常量缓冲区, 常量”xyz”不管出现多少遍, 都是缓冲区中的那一个. New String每写一遍, 就创建一个新的对象, 它依据那个常量”xyz”对象的内容来创建出一个新String对象. 如果以前就用过’xyz’, 这句代表就不会创建”xyz”自己了, 直接从缓冲区拿.

**题目四**

```
String s1 = new String("777");
String s2 = "aaa777";
String s3 = "aaa" + "777";
String s4 = "aaa" + s1;
System.out.println(s2 == s3);
System.out.println(s2 == s4);
System.out.println(s3 == s4);           

========sout========
true
false
false
```

**题目五**

```
String str = "ABCDEFGH";
String str1 = str.substring(3,5);
System.out.println(str1);
```

输出是 DE substring 是前包括后不包括

**题目六**

```
执行后，原始的 String 对象中的内容到底变了没有？

    String s = "Hello";
    s = s + " world!";
```

没有. 因为 String 被设计成**不可变(immutable)类**, 所以它的所有对象都是不可变对象.

在这段代码中, s原先指向一个String对象, 内容是 "Hello",然后我们对s进行了+操作, 这时，s不指向原来那个对象了, 而指向了另一个 String对象, 内容为"Hello world!", 原来那个对象还存在于内存之中, 只是s这个引用变量不再指向它了.

**题目七**

执行后，输出是什么? 共创建了几个字符串对象?

```
   String s = " Hello ";
   s += " World ";
   s.trim( );
   System.out.println(s);
```

再次强调 String 是**不可变(immutable)类**. 所以共创建了三个对象. 最后输出是 " Hello World ". 只有 s = s.trim() 之后才是 "Hello World".



# String、StringBuilder、StringBuffer 相关问题

三者之间的区别：

都是final类，都不允许被继承；

String类长度是不可变的，StringBuffer和StringBuilder类长度是可以改变的；

StringBuffer类是线程安全的，StringBuilder不是线程安全的；

#### String 和 StringBuffer：

1、String类型和StringBuffer类型的主要性能区别：String是不可变的对象，因此每次在对String类进行改变的时候都会生成一个新的string对象，然后将指针指向新的string对象，所以经常要改变字符串长度的话不要使用string，因为每次生成对象都会对系统性能产生影响，特别是当内存中引用的对象多了以后，JVM的GC就会开始工作，性能就会降低；

2、使用StringBuffer类时，每次都会对StringBuffer对象本身进行操作，而不是生成新的对象并改变对象引用，所以多数情况下推荐使用StringBuffer，特别是字符串对象经常要改变的情况；

3、在某些情况下，String对象的字符串拼接其实是被Java Compiler编译成了StringBuffer对象的拼接，所以这些时候String对象的速度并不会比StringBuffer对象慢。

### **String手动入池**

　　**一个初始为空的字符串池，它由类 String 私有地维护。** 当调用 intern 方法时，如果池已经包含一个等于此 String 对象的字符串（用 equals(Object) 方法确定），则返回池中的字符串。否则，将此 String 对象添加到池中，并返回此 String 对象的引用。特别地，手动入池遵循以下规则：

　　**对于任意两个字符串 s 和 t ，当且仅当 s.equals(t) 为 true 时，s.intern() == t.intern() 才为 true 。** 　　

```
public class TestString{
    public static void main(String args[]){
        String str1 = "abc";
        String str2 = new String("abc");
        String str3 = s2.intern();

        System.out.println( str1 == str2 );   //false
        System.out.println( str1 == str3 );   //true
    }
}12345678910
```

　　所以，对于 String str1 = “abc”，str1 引用的是 **常量池（方法区）** 的对象；而 String str2 = new String(“abc”)，str2引用的是 **堆** 中的对象，所以内存地址不一样。但是由于内容一样，所以 str1 和 str3 指向同一对象。

### 注意

**通过 new String(“…”) 来创建字符串时，在该构造函数的参数值为字符串字面值的前提下，若该字面值不在字符串常量池中，那么会创建两个对象：一个在字符串常量池中，一个在堆中；否则，只会在堆中创建一个对象。对于不在同一区域的两个对象，二者的内存地址必定不同。** 

####  字符串连接符“+”

```
    String str2 = "ab";  //1个对象  
    String str3 = "cd";  //1个对象                                         
    String str4 = str2+str3;                                        
    String str5 = "abcd";    
    System.out.println("str4 = str5 : " + (str4==str5)); // false  12345
```

　　我们看这个例子，局部变量 str2，str3 指向字符串常量池中的两个对象。在运行时，**第三行代码(str2+str3)实质上会被分解成五个步骤，分别是：**

　**(1). 调用 String 类的静态方法 String.valueOf() 将 str2 转换为字符串表示；**

　**(2). JVM 在堆中创建一个 StringBuilder对象，同时用str2指向转换后的字符串对象进行初始化；**　

　**(3). 调用StringBuilder对象的append方法完成与str3所指向的字符串对象的合并；**

　**(4). 调用 StringBuilder 的 toString() 方法在堆中创建一个 String对象；**

　**(5). 将刚刚生成的String对象的堆地址存赋给局部变量引用str4。**

　　==而引用str5指向的是字符串常量池中字面值”abcd”所对应的字符串对象。由上面的内容我们可以知道，引用str4和str5指向的对象的地址必定不一样。这时，内存中实际上会存在五个字符串对象： 三个在字符串常量池中的String对象、一个在堆中的String对象和一个在堆中的StringBuilder对象。==

####  字符串的编译期优化

```
    String str1 = "ab" + "cd";  //1个对象  
    String str11 = "abcd";   
    System.out.println("str1 = str11 : "+ (str1 == str11));   // true

    final String str8 = "cd";  
    String str9 = "ab" + str8;  
    String str89 = "abcd";  
    System.out.println("str9 = str89 : "+ (str9 == str89));     // true
    //↑str8为常量变量，编译期会被优化  

    String str6 = "b";  
    String str7 = "a" + str6;  
    String str67 = "ab";  
    System.out.println("str7 = str67 : "+ (str7 == str67));     // false
    //↑str6为变量，在运行期才会被解析。123456789101112131415
```

　　Java 编译器对于类似**“常量+字面值”的组合，其值在编译的时候就能够被确定了。**在这里，str1 和 str9 的值在编译时就可以被确定，因此它们分别等价于： String str1 = “abcd”; 和 String str9 = “abcd”;

　　**Java 编译器对于含有 “String引用”的组合，则在运行期会产生新的对象 (通过调用StringBuilder类的toString()方法)，因此这个对象存储在堆中。**

 **小结**

- **使用字面值形式创建的字符串与通过 new 创建的字符串一定是不同的，因为二者的存储位置不同：前者在方法区，后者在堆；**
- 我们在使用诸如String str = “abc”；的格式创建字符串对象时，总是想当然地认为，我们创建了String类的对象str。但是事实上， **对象可能并没有被创建。唯一可以肯定的是，指向 String 对象 的引用被创建了。**至于这个引用到底是否指向了一个新的对象，必须根据上下文来考虑；
- **字符串常量池的理念是 《享元模式》；**
- **Java 编译器对 “常量+字面值” 的组合 是当成常量表达式直接求值来优化的；对于含有“String引用”的组合，其在编译期不能被确定，会在运行期创建新对象。**

**String 在克隆中的特殊性**

　　**String 在克隆时只是克隆了它的引用。**

　　奇怪的是，在修改克隆后的 String 对象时，其原来的对象并未改变。原因是：**String是在内存中不可以被改变的对象**。虽然在克隆时，源对象和克隆对象都指向了同一个String对象，但当其中一个对象修改这个String对象的时候，会新分配一块内存用来保存修改后的String对象并将其引用指向新的String对象，而原来的String对象因为还存在指向它的引用，所以不会被回收。这样，对于String而言，虽然是复制的引用，但是当修改值的时候，并不会改变被复制对象的值。**所以在使用克隆时，我们可以将 String类型 视为与基本类型，只需浅克隆即可。**

## String 总结

(1). 使用字面值形式创建字符串时，不一定会创建对象，但其引用一定指向位于字符串常量池的某个对象；

(2). 使用 new String(“…”)方式创建字符串时，一定会创建对象，甚至可能会同时创建两个对象（一个位于字符串常量池中，一个位于堆中）；

(3). String 对象是不可变的，对String 对象的任何改变都会导致一个新的 String 对象的产生，而不会影响到原String 对象；

(4). StringBuilder 与 StringBuffer 具有共同的父类，具有相同的API，分别适用于单线程和多线程环境下。特别地，在单线程环境下，StringBuilder 是 StringBuffer 的替代品，前者效率相对较高；

------

(5). **字符串比较时用的什么方法，内部实现如何？**

　　使用equals方法 ： **先比较引用是否相同(是否是同一对象)，再检查是否为同一类型（str instanceof String）， 最后比较内容是否一致（String 的各个成员变量的值或内容是否相同）。****这也同样适用于诸如 Integer 等的八种包装器类。**

# Java创建数组的几种方式

```
type[] arrayName; 或 type arrayName[]; 
```

附：推荐使用第一种格式，因为第一种格式具有更好的可读性，表示type[]是一种引用类型（数组）而不是type类型。建议不要使用第二种方式

下面是典型的声明数组的方式：

```
 // 声明整型数组

 int[] intArray0 ;

 int intArray1 [];

 // 声明浮点型数组

 float floatArray0 [];

 float[] floatArray1 ;

 // 声明布尔型数组

 boolean boolArray0 [];

 boolean[] boolArray1 ;

 // 声明字符型数组

 char charArray0 [];

 char[] charArray1 ;

 // 声明字符串数组

 String stringArray0[];

 String[] stringArray1;

 // 错误的声明数组的方式，声明数组的时候不能指定其大小

 // int [5] intErrorArray0;

  // int intErrorArray1[5];
```

注：Java语言中声明数组时不能指定其长度（数组中元素的个数），这是因为数组是一种引用类型的变量，，因此使用它定义一个变量时，仅仅表示定义了一个引用变量（也就是定一个了一个指针），这个引用变量还未指向任何有效的内存，所以定义数组时不能指定数组的长度。而且由于定义数组仅仅只是定一个引用变量，并未指向任何有效的内存空间，所以还没有内存空间来存储数组元素，因此这个数组也不能使用，只有在数组进行初始化后才可以使用。

## 2、一维数组的创建

​     Java中使用关键字new创建数组对象，格式为：数组名 = new 数组元素的类型 [数组元素的个数]

 

```
// 创建数组，如果在创建的同时不初始化数组则必须指定其大小

 intArray0 = new int[3];

 // 错误的创建数组的方式，如果创建数组时不指定大小则必须初始化

 // intArray1 = new int[];

 // 创建数组时，不指定数组大小则必须在创建的同时初始化数组

  intArray1 = new int[]{0,1,2};

   使用new创建数组对象但是分配数组时会自动为数组分配默认值，具体如下：

 System.out.println( "intArray0[0]=" + intArray0 [0]);

 floatArray0 = new float[3];

 System.out.println("floatArray0[0]=" + floatArray0[0]);

 boolArray0 = new boolean[3];

 System.out.println("boolArray0[0]=" + boolArray0[0]);

 charArray0 = new char[3];

 System.out.println("charArray0[0]=" + charArray0[0]);

 stringArray0 = new String[3];

 System.out.println("stringArray0[0]=" + stringArray0[0]);

输出如下：

 intArray0[0]=0

 floatArray0[0]=0.0

 boolArray0[0]=false

 charArray0[0]=

 stringArray0[0]=null
```

附:一旦使用new关键字为数组分配了内存空间，每个内存空间存储的内容就是数组元素的值，也就是数组元素就有了初始值，即使这个内存空间存储的内容是空，这个空也是一个值null。也就是说不可能只分配内容空间而不赋初始值，即使自己在创建数组对象（分配内容空间）时没有指定初始值，系统也会自动为其分配

附：诸如基础数据类型的包装类，其默认的初始化值均为null，因为基础数据类型的包装类创建的数组属于引用数组（对象数组），对象数组默认的初始化值都是null

## 3、一维数组的初始化

​     数组的初始化分为静态初始化、动态初始化和默认初始化：

​     静态初始化是数组在初始化时由程序员显式指定每个数组元素的初始值而数组长度由系统决定。

​     动态初始化是数组在初始化时只指定数组长度，由系统为数组元素分配初始值。

​     a、数组静态初始化的语法格式：

​     arrayName = new type[]{element1,element2,element3...}或者使用简化的语法格式：arrayName = {element1,element2,element3...}

​     b、数组动态初始化的语法格式：

​     arrayName = new type[length]；

 // 静态初始化

 **int** intArray2 [] = **new** **int**[]{20,21,22};

  // 静态初始化简化方式

 **int** intArray3 [] = {30,31,32};

  // 动态初始化

 **int**[] intArray4 = **new** **int**[3];          

 // 错误写法：静态初始化不能指定元素个数

 // int intErrorArray5[] = new int[3]{50,51,52};

 // 错误写法：动态初始化必须指定元素个数

 // int intErrorArray6[] = new int[];

注：一维数组这一块记住两点，数组声明的时候是不能指定大小的，也就是说等号左边的中括号中不能包含数字。另外一旦使用new关键字那么肯定在内存中为数组分配了空间，则必然数组有默认值。数组是对象数据类型

注：不要静态初始化和动态初始化同时使用，也就是说不要再进行数组初始化时，既指定数组长度，也为每个数组元素分配初始值。

4、数组进行动态初始化时系统分配初始值的规则

​     数组元素类型是基本类型中的整数类型（byte、short、int、long），则数组元素的值是0

​     数组元素类型是基本类型中的浮点类型（float、double），则数组元素的值是0.0

​     数组元素类型是基本类型中的字符类型（char），则数组元素的值是'\u0000'

​     数组元素类型是基本类型中的布尔类型（boolean），则数组元素的值是false

​     数组元素类型是基本类型中的引用类型（类、接口、数组），则数组元素的值是null
