# Java Data Types - Java 数据类型

JVM 可以操作的数据类型分为两类: primitive types（基本数据类型） 和 reference types（引用类型）. 类型检查通常在编译期完成，不同指令操作数的类型可以通过虚拟机的字节码指令本身确定。

### Primitive type

==JVM 所支持的基本数据类型有：数值类型(Numeric types), 布尔类型(Boolean type) 和 returnAddress 类型。其中数值类型又可以分为整型和浮点型两种。==

- 整型：byte(8 bit), short(16 bit), int(32 bit), long(64 bit), char(16 bit unsigned)
- 浮点型：float(32 bit), double(64 bit)
- 布尔型：boolean 通常用 int 型表示，Oracle 中用 byte 表示
- returnAddress：指向一条虚拟机指令的操作码 ，returnAddress类型在Java语言之中并不存在相应的类型，而且也无法在程序运行期间更改。 

### Reference type

==引用类型分为三种：Class Types, Array Types 和 Interface Types, 这些引用类型的值分别由类实例、数组实例和实现了某个接口的类实例==或者数组实例动态创建。引用类型中有一特殊的值`null`, 引用类型的默认值就是 null.

### java 基本数据类型

Java基本类型共有八种，基本类型可以分为三类，字符类型char，布尔类型boolean以及数值类型。数值类型又可以分为整数类型byte、short、int、long和浮点数类型float、double。==JAVA中的数值类型不存在无符号的，它们的取值范围是固定的，不会随着机器硬件环境或者操作系统的改变而改变。实际上，JAVA中还存在另外一种基本类型void，它也有对应的包装类 java.lang.Void，不过我们无法直接对它们进行操作==

### 一.基本数据类型

Java语言提供了八种基本类型。六种数字类型（四个整数型，两个浮点型），一种字符类型，还有一种布尔型。

**byte：**

- byte数据类型是8位、有符号的，以二进制补码表示的整数；
- 最小值是-128（-2^7）；最大值是127（2^7-1）；默认值是0；
- **byte类型用在大型数组中节约空间**，主要代替整数，因为byte变量占用的空间只有int类型的四分之一；
- 例子：byte a = 100，byte b = -50。

**short：**

- short数据类型是16位、有符号的，以二进制补码表示的整数
- 最小值是-32768（-2^15）；最大值是32767（2^15 - 1）；
- Short数据类型也可以像byte那样**节省空间**。一个short变量是int型变量所占空间的二分之一；
- 例子：short s = 1000，short r = -20000。

**int：**

- int数据类型是32位，有符号的，以二进制补码表示的整数；
- 最小值是-2,147,483,648（-2^31）；最大值是2,147,485,647（2^31 - 1）；
- 一般地整型变量默认为int类型；
- 例子：int a = 100000, int b = -200000。

**long：**

- long数据类型是64位、有符号的，以二进制补码表示的整数；
- 最小值是-9,223,372,036,854,775,808（-2^63）；最大值是9,223,372,036,854,775,807（2^63 -1）；
- 这种类型主要使**用在需要比较大整数的系统上**；
- 例子： long a = 100000L，int b = -200000L。

**float：**

- float数据类型是单精度、32位、符合IEEE 754标准的浮点数；
- 1位符号位+8位指数位（q）+23位底数位（b），即b^q；
- float在储存大型浮点数组的时候可节省内存空间；
- 浮点数不能用来表示精确的值，如货币；
- 例子：float f1 = 234.5f。

**double：**

- double数据类型是双精度、64位、符合IEEE 754标准的浮点数；
- 1位符号位+11位指数位（q）+52位底数位，即b^q；
- 浮点数的默认类型为double类型；
- double类型同样不能表示精确的值，如货币；
- 例子：double d1 = 123.4。

**boolean：**

- boolean数据类型表示一位的信息；只有两个取值：true和false；
- 默认值是false；

**char：**

- char类型是一个单一的16位Unicode字符；
- 最小值是’\u0000’（即为0）；
- 最大值是’\uffff’（即为65,535）；
- char数据类型可以储存任何字符；
- 例子：char letter = ‘A’。

**Summary：**

byte,short,int,long 均为有符号的、二进制补码表示的整数，不同在于占用空间情况,实际使用时可根据内存情况选择。其各自可表示的范围为（-2^(x-1)~2^(x-1)-1）;

float,double表示的均是浮点数，不同之一在于其各自精度不同(精度体现在尾数的位数上)，之二在于可表示的范围(由于其各自占用空间不同)。

==Java决定了每种简单类型的大小。这些大小并不随着机器结构的变化而变化。这种大小的不可更改正是Java程序具有很强移植能力的原因之一。==

​	Java基本类型存储在栈中，因此它们的存取速度要快于存储在堆中的对应包装类的实例对象。从Java5.0（1.5）开始，JAVA虚拟机（Java Virtual Machine）可以完成基本类型和它们对应包装类之间的自动转换。因此我们在赋值、参数传递以及数学运算的时候像使用基本类型一样使用它们的包装类，但这并不意味着你可以通过基本类型调用它们的包装类才具有的方法。另外，==所有基本类型（包括void）的包装类都使用了final修饰，因此我们无法继承它们扩展新的类，也无法重写它们的任何方法。基本类型的优势：数据存储相对简单，运算效率比较高.包装类的优势：有的容易，比如集合的元素必须是对象类型，满足了java一切皆是对象的思想==

**JAVA包装类：**

Java语言是一个面向对象的语言，但**Java中的基本数据类型却是不面向对象**的，这在实际使用时存在很多的不便，为解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，**这样八个和基本数据类型对应的类统称为包装类****(Wrapper Class)**，也译为外覆类或数据类型类。==所有的包装类都是抽象类Number的子类。包装类均位于java.lang包。==

==对于包装类说，这些类的用途主要包含两种：==

==a、作为和基本数据类型对应的类类型存在，**方便涉及到对象的操作**。==

==b、包含每种基本数据类型的相关属性如最大值、最小值等，以及相关的操作方法。==

## 二.引用类型

- ==引用类型变量**由类的构造函数创建**，==可以使用它们访问所引用的对象。这些变量在声明时被指定为一个特定的类型，比如Employee、Pubby等。==**变量一旦声明后，类型就不能被改变了。**==
- ==**对象、数组都是引用数据类型**。==
- ==所有引用类型的默认值都是null。==
- ==一个引用变量可以用来引用与任何与之兼容的类型。==

  例子：Animal animal = new Animal(“giraffe”)。

## 三.Java常量

常量就是一个固定值。它们不需要计算，直接代表相应的值。常量指不能改变的量。 在Java中**用final标志**，声明方式和变量类似。

 

# PART TWO：Java 变量类型

Java语言支持的变量类型有：

- 局部变量(本地变量)
- 成员变量
- 类变量

## 一.Java局部变量

- 局部变量**声明在方法、构造方法或者语句块**中(被创建，且仅在其内可见)； 当它们执行完成后，变量将会被销毁；
- ==访问修饰符不能用于局部变量；==
- ==局部变量是**在栈上分配的**。==
- ==**局部变量没有默认值**，所以局部变量量被声明后，**必须初始化后，**才可以使用**。**==

## 二.实例变量

- 实例变量声明在一个类中，但在方法、构造方法和语句块之外；
- 当一个对象被实例化之后，每个实例变量的值就跟着确定；
- **实例变量在对象创建的时候创建，在对象被销毁的时候销毁；**
- 实例变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息；
- 实例变量可以声明在使用前或者使用后；
- **访问修饰符可以修饰实例变量**（package(默认)、private、public和protected）**；**
- 实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见；
- **实例变量具有默认值。**数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；
- 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：**ObejectReference.VariableName**。

## 三.类变量（静态变量）

- 类变量也称为静态变量，**在类中以static关键字声明**，但必须在方法构造方法和语句块之外。
- 无论一个类创建了多少个对象，类只拥有类变量的一份拷贝。
- 静态变量储存在**静态存储区**。经常被声明为常量，很少单独使用static声明变量。
- 静态变量在程序开始时创建，**在程序结束时销毁。**
- 与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为public类型。
- 默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化。
- 静态变量可以通过：**ClassName.VariableName**的方式访问。
- 类变量被声明为public static final类型时，类变量名称必须使用大写字母。如果静态变量不是public和final类型，其命名方式与实例变量以及局部变量的命名方式一致。

## 形式参数传递

==基本类型作为形式参数传递不会改变实际参数，引用类型作为形式参数传递会改变实际参数==。JDK1.5之后含有基本类型的包装类型，即自动拆装箱的功能，故将基本类型的相应对象作为参数传递时会自动拆箱为基本类型，故也不改变实际参数的值。

==只有 boolean 不参与数据类型的转换== 
2、自动类型的转换：

> ==a.常数在表数范围内是能够自动类型转换的== 
> ==b.数据范围小的能够自动数据类型大的转换（注意特例）== 
> ==c. float 到 int，float 到 long，double 到 int，double 到 long 等由浮点类型转换成整数类型时，是不会自动转换的，不然将会丢失精度。== 
> ==d.引用类型能够自动转换为父类的== 
> ==e.基本类型和它们包装类型是能够互相转换的==



# java基本数据类型自动转换规则

## Java基本数据类型转换

Java语言是一种强类型的语言。强类型的语言有以下几个要求： 
（1） 变量或常量必须有类型，而且只能在声明以后才能使用； 
（2） 赋值时类型必须一致，值的类型必须和变量或常量的类型完全一致； 
（3） 运算时类型必须一致，参与运算的数据类型必须一致才能运算。 
但在实际应用中，经常需要在不同类型的值之间进行操作，这时就需要进行数据类型的转换。 
数据类型转换有两种： 
==（1） 自动类型转换：编译器自动完成类型转换，不需要在程序中编写代码；== 
==规则：从存储范围小的类型到存储范围大的类型。== 
==具体规则：byte→short(char)→int→long→float→double.== 
==（2） 强制类型转换：强制编译器进行类型转换，必须在程序中编写代码。该类型转换很可能存在精度的损失。== 
==规则：从存储范围大的类型到存储范围小的类型。== 
==具体规则：double→float→long→int→short(char)→byte.== 



## 二、非赋值运算，自动转换规则

### 1）布尔型不参与转换

### 2.2）规则具体讲解

==（2.2.1）如操作数之一为double，则另一个操作数先被转化为double，再参与算术运算。== 
==（2.2.2）如两操作数均不为double，当操作数之一为float，则另一操作数先被转换为float，再参与运算。== 
==（2.2.3）如两操作数均不为double或float，当操作数之一为long，、则另一操作数先被转换为long，再参与算术运算。== 
==（2.2.4）如两操作数均不为double、float或long，则两操作数先被转换为int，再参与运算。== 



## 三、赋值运算，自动转换规则

将小范围类型变量转换成大范围类型的变量。Java会自动扩宽类型

看上面那张图，各个基本数据类型的取值范围都有了，如果一个取值范围包括了另一个的，那么赋值时，小的可以自动转换成大的

==例子：     long d = 333;   float e = d; //可以==

==short a = 1; char b = a; //不行，因为取值范围不是包含，而是交叉==

==int a = 1; byte b = a; //不行，大的转成小的，要强制类型转换，byte b = (byte) a;==

## 四、一些值得注意的地方

==当运算符为取正运算符（+）。取负运算符（-）或按位取反运算符（~）时，如果操作数为byte、char或short，则先被转换为int，再参与运算==

## 1、自动类型转换

自动类型转换，也称隐式类型转换，是指不需要书写代码，由系统自动完成的类型转换。由于实际开发中这样的类型转换很多，所以 Java 语言在设计时，没有为该操作设计语法，而是由 JVM 自动完成。

**转换规则：从存储范围小的类型到存储范围大的类型。**

具体规则为：byte→short(char)→int→long→float→double

也就是说 byte 类型的变量可以自动转换为 short 类型，示例代码：

byte b=10;
short sh=b;

这里在给sh赋值时，JVM首先将b的值转换成short类型然后再赋值给sh。

当然，在类型转换的时候也可以跳跃，就是byte也可以自动转换为int类型的。

注意问题：在整数之间进行类型转换的时候数值不会发生变化，但是当将整数类型特别是比较大的整数类型转换成小数类型的时候，由于存储精度的不同，可能会存在数据精度的损失。

## 2、强制类型转换

强制类型转换，也称显式类型转换，是指必须书写代码才能完成的类型转换。该类类型转换很可能存在精度的损失，所以必须书写相应的代码，并且能够忍受该种

损失时才进行该类型的转换。

转换规则:从存储范围大的类型到存储范围小的类型。

具体规则为：double→float→long→int→short(char)→byte

语法格式为：(转换到的类型)需要转换的值

double d=3.14;
int i=(int) d;

注意问题:强制类型转换通常都会存储精度的损失，所以使用时需要谨慎。

## **==三、常见面试题：==**

1、short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 +=1;有什么错?

==答：对于short s1=1;s1=s1+1来说，在s1+1运算时会自动提升表达式的类型为int，那么将int赋予给short类型的变量s1会出现类型转换错误。==

==**对于short s1=1;s1+=1来说 +=是java语言规定的运算符，java编译器会对它进行特殊处理，因此可以正确编译。**==

2、char类型变量能不能储存一个中文的汉字，为什么？

==**char类型变量是用来储存Unicode编码的字符的，unicode字符集包含了汉字，所以char类型当然可以存储汉字的，还有一种特殊情况就是某个生僻字没有包含在unicode编码字符集中，那么就char类型就不能存储该生僻字。**==

3、Integer和int的区别

**int是java的8种内置的原始数据类型。**Java为每个原始类型都提供了一个封装类，**Integer就是int的封装类。**

**int变量的默认值为0，Integer变量的默认值为null，这一点说明Integer可以区分出未赋值和值为0的区别**

**Integer类内提供了一些关于整数操作的一些方法，例如上文用到的表示整数的最大值和最小值。**

4、switch语句能否作用在byte上，能否作用在long上，能否作用在string上？

==**byte的存储范围小于int，可以向int类型进行隐式转换，所以switch可以作用在byte上**==

==**long的存储范围大于int，不能向int进行隐式转换，只能强制转换，所以switch不可以作用在long上**==

==**string在1.7版本之前不可以，1.7版本之后switch就可以作用在string上了。**==



## **一、基本数据类型的类型转换**

==基本数据类型中，布尔类型boolean占有一个字节，由于其本身所代码的特殊含义，**boolean类型与其他基本类型不能进行类型的转换（既不能进行自动类型的提升，也不能强制类型转换）， 否则，将编译出错**。==

1.基本数据类型中数值类型的自动类型提升

数值类型在内存中直接存储其本身的值，对于不同的数值类型，内存中会分配相应的大小去存储。如:byte类型的变量占用8位，int类型变量占用32位等。相应的，不同的数值类型会有与其存储空间相匹配的取值范围。具体如下所示：

![](D:\workspace\Github\node\瑞秋\answer\assets\804968-c97646615cbedf4b.png)

图中依次表示了各数值类型的字节数和相应的取值范围。==**在Java中，整数类型（byte/short/int/long）中，对于未声明数据类型的整形，其默认类型为int型。在浮点类型（float/double）中，对于未声明数据类型的浮点型，默认为double型。**==

==将一个int型的3赋给一个byte型的变量c（byte c = 3），居然编译正确，这是为什么呢？==

==原因在于：**jvm在编译过程中，对于默认为int类型的数值时，当赋给一个比int型数值范围小的数值类型变量（在此统一称为数值类型k，k可以是byte/char/short类型），会进行判断，如果此int型数值超过数值类型k，那么会直接编译出错。但是如果此int型数值尚在数值类型k范围内，jvm会自定进行一次隐式类型转换，将此int型数值转换成类型k。如图中的虚线箭头。这一点有点特别，需要稍微注意下。**==

**在其他情况下，当将一个数值范围小的类型赋给一个数值范围大的数值型变量，jvm在编译过程中俊将此数值的类型进行了自动提升。在数值类型的自动类型提升过程中，数值精度至少不应该降低（整型保持不变，float->double精度将变高）。**

**还有一个地方需要注意的是：char型其本身是unsigned型，同时具有两个字节，其数值范围是0 ~ 2^16-1，因为，这直接导致byte型不能自动类型提升到char，char和short直接也不会发生自动类型提升（因为负数的问题），同时，byte当然可以直接提升到short型。**

2.基本数据类型中的数值类型强制转换

当我们需要将数值范围较大的数值类型赋给数值范围较小的数值类型变量时，由于**此时可能会丢失精度**（1讲到的从int到k型的隐式转换除外），我们称之为强制类型转换。

==将一个值为3的int型变量a赋值给byte型变量b（int a = 3; byte b = a）发生编译错误。这两种写法之间有什么区别呢？==

==**区别在于前者3是直接量，编译期间可以直接进行判定，后者a为一变量，需要到运行期间才能确定，也就是说，编译期间为以防万一，当然不可能编译通过了。此时，需要进行强制类型转换。**==强制类型转换所带来的结果是可能会丢失精度，如果此数值尚在范围较小的类型数值范围内，对于整型变量精度不变，但如果超出范围较小的类型数值范围内，很可能出现一些意外情况。

如下经典例子：

packagecom.corn.testcast;

public class TestCast {

​	public static void main(String[] args) {

​		inta = 233;

​		byteb = (byte) a;

​		System.out.println("b:" + b);//输出：-23

​	}

}

**为什么结果是-23？需要从最根本的二进制存储考虑。**

==233的二进制表示为：24位0 + 11101001，byte型只有8位，于是从高位开始舍弃，截断后剩下：11101001，由于二进制最高位1表示负数，0表示正数，其相应的负数为-23。==

3.进行数学运算时的数据类型自动提升与可能需要的强制类型转换

packagecom.corn.testcast;

public class TestCast {

​	public static void main(String[] args) {

​		byte a = 3 + 5;//编译正常 编译成 3+5直接变为

​		int b = 3, c = 5;

​		byte d = b + c;//编译错误：cannot convert from int to byte

​		byte e = 10, f = 11;

​		byte g = e + f;//编译错误 +直接将10和11类型提升为了int

​		byte h = (byte) (e + f);//编译正确

​	}

}

==**当进行数学运算时，数据类型会自动发生提升到运算符左右之较大者**，以此类推。当将最后的运算结果赋值给指定的数值类型时，可能需要进行强制类型转换。==



# 动态代理的简要说明

　　在java的动态代理机制中，有两个重要的类或接口，一个是 InvocationHandler(Interface)、另一个则是 Proxy(Class)。

### 一、 InvocationHandler(interface)的描述：

 每一个动态代理类都必须要实现InvocationHandler这个接口，并且每个代理类的实例都关联到了一个handler，当我们通过代理对象调用 一个方法的时候，这个方法的调用就会被转发为由InvocationHandler这个接口的 invoke 方法来进行调用。

Proxy这个类的作用就是用来动态创建一个代理对象。我们经常使用的是newProxyInstance这个方法



- ==与代理对象相关联的InvocationHandler，只有在代理对象调用方法时，才会执行它的invoke方法==

- ==invoke的三个参数的理解：Object proxy是代理的对象, Method method是真实对象中调用方法的Method类, Object[] args是真实对象中调用方法的参数==

- ==**为什么我们这里可以将其转化为Interface类型的对象？**原因就是在newProxyInstance这个方法的第二个参数上，我们给这个代理对象提供了一组什么接口，那么我这个代理对象就会实现了这组接口，这个时候我们当然可以将这个代理对象强制类型转化为这组接口中的任意一个，因为这里的接口是Subject类型，所以就可以将其转化为Subject类型了。==

  ==**同时我们一定要记住，通过 Proxy.newProxyInstance 创建的代理对象是在jvm运行时动态生成的一个对象，它并不是我们的InvocationHandler类型，也不是我们定义的那组接口的类型，而是在运行是动态生成的一个对象，并且命名方式都是这样的形式，以$开头，proxy为中，最后一个数字表示对象的标号**。==

 

# Java动态代理的原理

### 一、 动态代理的关键代码就是Proxy.newProxyInstance(classLoader, interfaces, handler)，我们跟进源代码看看：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) throws IllegalArgumentException {
　　// handler不能为空
    if (h == null) {
        throw new NullPointerException();
    }

    final Class<?>[] intfs = interfaces.clone();
    final SecurityManager sm = System.getSecurityManager();
    if (sm != null) {
        checkProxyAccess(Reflection.getCallerClass(), loader, intfs);
    }

    /*
     * Look up or generate the designated proxy class.
     */
　　// 通过loader和接口，得到代理的Class对象
    Class<?> cl = getProxyClass0(loader, intfs);

    /*
     * Invoke its constructor with the designated invocation handler.
     */
    try {
        final Constructor<?> cons = cl.getConstructor(constructorParams);
        final InvocationHandler ih = h;
        if (sm != null && ProxyAccessHelper.needsNewInstanceCheck(cl)) {
            // create proxy instance with doPrivilege as the proxy class may
            // implement non-public interfaces that requires a special permission
            return AccessController.doPrivileged(new PrivilegedAction<Object>() {
                public Object run() {
                    return newInstance(cons, ih);
                }
            });
        } else {
　　　　　　　// 创建代理对象的实例
            return newInstance(cons, ih);
        }
    } catch (NoSuchMethodException e) {
        throw new InternalError(e.toString());
    }
}
```

 

### 二、 我们看一下newInstance方法的源代码：

```
private static Object newInstance(Constructor<?> cons, InvocationHandler h) {
    try {
        return cons.newInstance(new Object[] {h} );
    } catch (IllegalAccessException | InstantiationException e) {
        throw new InternalError(e.toString());
    } catch (InvocationTargetException e) {
        Throwable t = e.getCause();
        if (t instanceof RuntimeException) {
            throw (RuntimeException) t;
        } else {
            throw new InternalError(t.toString());
        }
    }
}
```

 

### 三、 当我们通过代理对象调用 一个方法的时候，这个方法的调用就会被转发为由InvocationHandler这个接口的 invoke 方法来进行调用。



# 动态代理的局限性

从动态代理的使用方法中我们看到其实可以被增强的方法都是实现了借口的（不实现借口的public方法也可以通过继承被代理类来使用），代码中的HouseOwner继承了RentHouse 。而对于private方法JDK的动态代理无能为力！
 以上的动态代理是JDK的，对于java工程还有大名鼎鼎的CGLib，但遗憾的是CGLib并不能在android中使用，android虚拟机相对与jvm还是有区别的。

 

Proxy 静态方法生成动态代理类同样需要通过类装载器来进行装载才能使用，它与普通类的唯一区别就是其字节码==是由 JVM 在运行时动态生成==的而非预存在于任何一个 .class 文件中。 



## 代理机制及其特点

首先让我们来了解一下如何使用 Java 动态代理。具体有如下四步骤：

1. 通过实现 InvocationHandler 接口创建自己的调用处理器；
2. ==通过为 Proxy 类指定 ClassLoader 对象和一组 interface 来创建动态代理类==；
3. 通过反射机制获得动态代理类的构造函数，其唯一参数类型是调用处理器接口类型；
4. 通过构造函数创建动态代理类实例，构造时调用处理器对象作为参数被传入。

##### 清单 3. 动态代理对象创建过程

```java
// InvocationHandlerImpl 实现了 InvocationHandler 接口，并能实现方法调用从代理类到委托类的分派转发
// 其内部通常包含指向委托类实例的引用，用于真正执行分派转发过来的方法调用
InvocationHandler handler = new InvocationHandlerImpl(..); 
 
// 通过 Proxy 为包括 Interface 接口在内的一组接口动态创建代理类的类对象
Class clazz = Proxy.getProxyClass(classLoader, new Class[] { Interface.class, ... }); 
 
// 通过反射从生成的类对象获得构造函数对象
Constructor constructor = clazz.getConstructor(new Class[] { InvocationHandler.class }); 
 
// 通过构造函数对象创建动态代理类实例
Interface Proxy = (Interface)constructor.newInstance(new Object[] { handler });
```



实际使用过程更加简单，因为 Proxy 的静态方法 newProxyInstance 已经为我们封装了步骤 2 到步骤 4 的过程，所以简化后的过程如下

##### 清单 4. 简化的动态代理对象创建过程

```java
// InvocationHandlerImpl 实现了 InvocationHandler 接口，并能实现方法调用从代理类到委托类的分派转发
InvocationHandler handler = new InvocationHandlerImpl(..); 
 
// 通过 Proxy 直接创建动态代理类实例
Interface proxy = (Interface)Proxy.newProxyInstance( classLoader, 
     new Class[] { Interface.class }, 
     handler );
```

接下来让我们来了解一下 Java 动态代理机制的一些特点。

首先是动态生成的代理类本身的一些特点。1）包：如果所代理的接口都是 public 的，那么它将被定义在顶层包（即包路径为空），如果所代理的接口中有非 public 的接口（因为接口不能被定义为 protect 或 private，所以除 public 之外就是默认的 package 访问级别），那么它将被定义在该接口所在包（假设代理了 com.ibm.developerworks 包中的某非 public 接口 A，那么新生成的代理类所在的包就是 com.ibm.developerworks），这样设计的目的是为了最大程度的保证动态代理类不会因为包管理的问题而无法被成功定义并访问；2）==类修饰符：该代理类具有 final 和 public 修饰符，意味着它可以被所有的类访问，但是不能被再度继承==；3）==类名：格式是“$ProxyN”，其中 N 是一个逐一递增的阿拉伯数字，代表 Proxy 类第 N 次生成的动态代理类，值得注意的一点是，并不是每次调用 Proxy 的静态方法创建动态代理类都会使得 N 值增加，原因是如果对同一组接口（包括接口排列的顺序相同）试图重复创建动态代理类，它会很聪明地返回先前已经创建好的代理类的类对象，而不会再尝试去创建一个全新的代理类，这样可以节省不必要的代码重复生成，提高了代理类的创建效率。==4）类继承关系：该类的继承关系如图：

##### 图 2. 动态代理类的继承图

![](D:\workspace\Github\node\瑞秋\answer\assets\image002.png)

==由图可见，Proxy 类是它的父类，这个规则适用于所有由 Proxy 创建的动态代理类。而且该类还实现了其所代理的一组接口，这就是为什么它能够被安全地类型转换到其所代理的某接口的根本原因。==

接下来让我们了解一下代理类实例的一些特点。每个实例都会关联一个调用处理器对象，可以通过 Proxy 提供的静态方法 getInvocationHandler 去获得代理类实例的调用处理器对象。在代理类实例上调用其代理的接口中所声明的方法时，这些方法最终都会由调用处理器的 invoke 方法执行，此外，==值得注意的是，代理类的根类 java.lang.Object 中有三个方法也同样会被分派到调用处理器的 invoke 方法执行，它们是 hashCode，equals 和 toString，可能的原因有：一是因为这些方法为 public 且非 final 类型，能够被代理类覆盖；二是因为这些方法往往呈现出一个类的某种特征属性，具有一定的区分度，所以为了保证代理类与委托类对外的一致性，这三个方法也应该被分派到委托类执行。==**当代理的一组接口有重复声明的方法且该方法被调用时，代理类总是从排在最前面的接口中获取方法对象并分派给调用处理器，而无论代理类实例是否正在以该接口（或继承于该接口的某子接口）的形式被外部引用，因为在代理类内部无法区分其当前的被引用类型。**

==接着来了解一下被代理的一组接口有哪些特点。首先，要注意不能有重复的接口，以避免动态代理类代码生成时的编译错误。其次，这些接口对于类装载器必须可见，否则类装载器将无法链接它们，将会导致类定义失败。再次，需被代理的所有非 public 的接口必须在同一个包中，否则代理类生成也会失败。最后，接口的数目不能超过 65535，这是 JVM 设定的限制。==

最后再来了解一下异常处理方面的特点。从调用处理器接口声明的方法中可以看到理论上它能够抛出任何类型的异常，因为所有的异常都继承于 Throwable 接口，但事实是否如此呢？答案是否定的，原因是==我们必须遵守一个继承原则：即子类覆盖父类或实现父接口的方法时，抛出的异常必须在原方法支持的异常列表之内。==所以虽然调用处理器理论上讲能够，但实际上往往受限制，除非父接口中的方法支持抛 Throwable 异常。==那么如果在 invoke 方法中的确产生了接口方法声明中不支持的异常==，那将如何呢？放心，Java 动态代理类已经为我们设计好了解决方法：==它将会抛出 UndeclaredThrowableException 异常。这个异常是一个 RuntimeException 类型，所以不会引起编译错误。通过该异常的 getCause 方法，还可以获得原来那个不受支持的异常对象，以便于错误诊断==。

 

 

 

 