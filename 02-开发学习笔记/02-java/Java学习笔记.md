[TOC]

[java联机API文档](https://docs.oracle.com/javase/7/docs/api/)

# Java核心技术卷I

## 第1章 Java 程序设计概述

第1章主要介绍了Java的起源、特性和其他编程语言的区别。



## 第2章 Java 程序设计环境

第2章主要介绍了 Java环境的如何搭建，如何使用命令行工作和一些专有名词含义（JDK,JRE等，“**JDK包括JRE**”），最后介绍了如何使用集成开发工具，书上介绍Eclipse，本人使用IDEA（都可以）。

### 2.1 JVM

Java Virtual Machine（Java虚拟机）的缩写

### 2.2 JDK和JRE

**JDK概念：**

Java Development Kit (JDK) 是针对Java开发人员发布的免费[软件开发工具包](https://www.baidu.com/s?wd=软件开发工具包&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)(SDK，Software development kit)。JDK 是整个Java的核心，包括了Java运行环境、Java工具和Java基础类库。2006年太阳微系统宣布将发布基于GPL协议的开源JDK，使JDK成为自由软件。

**配置jdk：**

如：jdk安装在“D:\Program Files\java\jdk1.6.0_10”

第一步：新建“java_home”值，输入“D:\Program Files\java\jdk1.6.0_10”；

第二步：新建“classpath”值，输入“.;%java_home%\lib”；

第三步：在path中增加“%java_home%\bin”；

备注：配置环境变量在“计算机”右击“属性”，之后选择“高级环境变量”，在选择“环境变量”即可。

**JDK和JRE的区别：**

1. 关系

- - **JDK包括JRE**。同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：jconsole，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。
  - 如果你需要运行java程序，只需安装JRE就可以了。如果你需要编写java程序，需要安装JDK。

1. 不同

- - JDK（Java Development Kit）：编写Java程序的**程序员使用**的软件；包含虚拟机（JVM）和编译器。
  - JRE（Java Runtime Environment）：运行Java程序的**用户使用**的软件；包含虚拟机，没有编译器。

## 第3章 Java的基本程序设计结构

### 3.1 Java运行流程

```go
（.java文件）编译->生成字节码文件（.class文件）->运行
```

### 3.2 数据类型

#### 基本类型

- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~

boolean 只有两个值：true、false，可以使用 1 bit 来存储，但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int，使用 1 来表示 true，0 表示 false。JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。

#### 包装类型

基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用自动装箱与拆箱完成。

```java
Integer x = 2;     // 装箱
int y = x;         // 拆箱
```

### 3.3 String

#### 3.3.1 概览

String 被声明为 final，因此它不可被继承。

> 非可变类（immutable）。什么是非可变类呢？简单说来，非可变类的实例是不能被修改的，每个实例中包含的信息都必须在该实例创建的时候就提供出来，并且在对象的整个生存周期内固定不变。

String Pool

> 在JVM中存放着一个字符串池，其中保存着很多String对象，这些对象可以被共享使用。当以字符串直接创建String对象时，会首先在字符串池中查找是否存在该常量。如果不存在，则在String Pool中创建一个，然后将其地址返回。如果在String Pool中查询到已经存在该常量，则不创建对象，直接返回这个对象地址。

#### 3.3.2 不可变的好处

##### 1.可以缓存hash值

因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。

##### 2. String Pool 的需要

如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。只有 String 是不可变的，才可能使用 String Pool。

##### 3. 安全性

String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。

##### 4. 线程安全

String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

#### 3.3.3 String, StringBuffer , StringBuilder

**1、概念和使用场景：**

- 和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。
- StringBuilder 类在 Java 5 中被提出，他的前身是 StringBuffer 。他们的API是相同的。它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。
- 由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。

```java
public class Test{
  public static void main(String args[]){
    StringBuffer sBuffer = new StringBuffer("菜鸟教程官网：");
    sBuffer.append("www");
    sBuffer.append(".runoob");
    sBuffer.append(".com");
    System.out.println(sBuffer);  
  }
}
```

**2、区别：**

1. 可变性

- String不可变
- StringBuffer 和 StringBuilder 可变

2. 线程安全

- String不可变，因此是线程安全的
- StringBuilder 不是线程安全的
- StringBuffer 是线程安全的，内部使用 synchronized 进行同步

**注意：new String("aaaaa")是新创建一个字符串，若是使用intern()方法是取得一个字符串引用，intern()方法具体如下：**

​    在调用`”ab”.intern()`方法的时候会返回`”ab”`，但是这个方法会首先检查字符串池中是否有`”ab”`这个字符串，如果存在则返回这个字符串的引用，否则就将这个字符串添加到字符串池中，然会返回这个字符串的引用。

#### 3.3.4 小数String转成double

1、小数 `Double.parseDouble`

```java
String str="4444.1122";

double num;

java.text.DecimalFormat myformat=new java.text.DecimalFormat("#0.00");

num=Double.parseDouble(str);//装换为double类型

num=Double.parseDouble(myformat.format(num));//保留2为小数

System.out.println(num);
```

2、整数

> 思路：
>
> 1.如果是整数，比如从服务器获取下来是整数4，由于java特性，会将4转换成4.0.我这边的处理方式是：将这个数字强制取整，然后乘以1000（小数点后移3位）如果等于这个数字乘以1000再取整，那么这个数就是整数，只是被java强制转换成了小数。
>
> 2.如果原本就是小数，则利用DecimalFormat直接进行转换。

```java
public static String getDoubleString(double number) {
    String numberStr;
    if (((int) number * 1000) == (int) (number * 1000)) {
        //如果是一个整数
        numberStr = String.valueOf((int) number);
    } else {
        DecimalFormat df = new DecimalFormat("######0.00");
        numberStr = df.format(number);
    }
    return numberStr;
}
```



### 3.4 运算

#### float 与 double

Java 不能隐式执行向下转型，因为这会使得精度降低。

如：字面量属于 double 类型，不能直接将 1.1 直接赋值给 float 变量，因为这是向下转型。

```java
//错误： 
float f = 1.1;
//正确：
float f = 1.1f;
```

#### 隐式类型转换

因为字面量 1 是 int 类型，它比 short 类型精度要高，因此不能隐式地将 int 类型下转型为 short 类型。但是使用 += 或者 ++ 运算符可以执行隐式类型转换。

```java
//错误：
short s1 = 1;

//正确：
s1 += 1;
//或者
s1++;
```

上面的语句相当于将 s1 + 1 的计算结果进行了向下转型：

```java
s1 = (short) (s1 + 1);
```

#### switch

从 Java 7 开始，可以在 switch 条件判断语句中使用 String 对象。

```java
String s = "a";
switch (s) {
    case "a":
        System.out.println("aaa");
        break;
    case "b":
        System.out.println("bbb");
        break;
}
```

**switch 不支持 long**，是因为 switch 的设计初衷是对那些只有少数的几个值进行等值判断，如果值过于复杂，那么还是用 if 比较合适。



## 第4章 对象与类

### 4.1 类

概述：类是构造对象的模板或蓝图。由类构造对象的过程称为创建类的实例

***一定要认识到：*一个对象变量并没有实际包含一个对象，而仅仅引用一个对象。可以把Java的对象变量看做是C++的对象指针。**

#### 4.1.1 封装

对对象的使用者隐藏了数据的实现方式。

***注意：*不要编写返回引用可变对象的访问器方法，需要返回一个可变对象的引用就对它进行克隆**

#### 4.1.2 类之间的关系

- 依赖("uses-a")：使用
- 聚合("has-a")：包含
- 继承("is-a")：特殊和一般

#### 4.1.3 构造器

构造器总是伴随着new操作符的执行被调用。



### 4.2 用户自定义类

#### 4.2.1 final关键字

##### 1.常量数据

声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。

- 对于基本类型，final 使数值不变；
- 对于引用类型，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。

```java
private final StringBuilder evaluations;
evaluations = new StringBuilder();
//final 关键字只是表示存储在evaluations变量中的对象引用不会再指示其他StringBuilder对象，但这个对象可以更改：
public void giveGoldStar(){
    evaluations.append(LocalDate().now() + ":Gold star!\n");
}
```

#### 4.2.2  static关键字

##### 1. 静态变量

静态变量和实例变量的区别：

- 静态变量：又称为类变量，也就是说这个变量属于类的，类所有的实例都共享静态变量，可以直接通过类名来访问它。静态变量在内存中只存在一份。
- 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。

```java
public class A {
    private int x;         // 实例变量
    private static int y;  // 静态变量

    public static void main(String[] args) {
        A a = new A();
        int x = a.x;
        int y = A.y;
    }
}
```

##### 2. 静态方法

- 静态方法在类加载的时候就存在了，它不依赖于任何实例（因此，静态方法必须有实现，不能是没有实现的抽象方法）。
- 只能访问所属类的静态字段和静态方法，方法中不能有this关键字。

##### 3. 静态语句块

静态语句块在类初始化时运行一次。

执行时间：编译的时候，添加在枚举对象初始化的代码（由编译器自动生成）后面。

作用：对静态数据进行初始化

```
public class A {
    static {
        System.out.println("123");
    }

    public static void main(String[] args) {
        A a1 = new A();
        A a2 = new A();
    }
}
结果：
123
```

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2019/png/224970/1564484893845-656a240e-3450-4748-a3d3-128557f18beb.png)

#### 4.2.3 方法参数

- 一个方法不可能修改一个基本数据类型（数值型或布尔型）的参数
- 一个方法可以改变一个对象参数的状态。可通过`对象引用`作为参数进而修改，理由：方法得到的是对象引用的拷贝，对象引用和其他的拷贝同时引用同一个对象，方法结束后，参数不再使用，但对象变量继续引用刚刚被修改的对象。
- 一个方法不能让对象参数引用一个新的对象。

#### 4.2.4 方法重载

存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。

**注意：**返回值不同，其它都相同不算是重载。



### 4.3 文档注释

#### 4.3.1 类注释

类注释必须放import之后，类定义之前

#### 4.3.2 方法注释

放所描述方法之前。

- [@param ]()变量描述
- [@return ]()返回描述
- [@throws ]()类描述

#### 4.3.3 域注释

只需要对公有域（通常指的是静态常量）建立文档。

#### 4.3.4 通用注释

- [@author ]()姓名 
  这个标记产生一个“author”（作者）条目。
- [@version ]()文本 
  这个标记将产生一个“version”（版本）条目。
- [@since ]()文本 
  text可以是关于版本的描述，例如：[@since ]()version1.8.2
- [@deprecated ]()文本 
  这个标记将对类、方法或变量添加一个不再使用的注释。
- [@see ]()/[@link ]()引用,3种形式

1. 1. package.class#feature label
   2. <a href="...">label</a>
   3. "text"

***例如*** 建立一个链接到com.horstmann.corejava.Employee类的raiseSalary(double)方法的超链接：

```java
@see com.horstmann.corejava.Employee#raiseSalary(double)
```

***注意：***用井号（#）而不是（.）分隔类名与方法名/变量名



### 4.4  类设计技巧

- 一定要保证数据私有（不要破坏封装性）
- 一定要对数据初始化
- 不要再类中使用过多的基本类型
- 不是所有的域都需要独立的域访问器和域更改器
- 将指责过多的类进行分解
- 类名和方法名要能够体现它们的指责
- 优先使用不可变的类



### 4.5 包

#### 4.5.1 包的导入

**概念：**

包（package）就是把类组织起来，把自己的代码和别人提供的代码库分开管理，其实就是一个独立的**类和接口的集合**。

目的：包还避免了类间潜在的命名冲突。

**引用包：**

import语句，可以导入一个特定的类或者整个包。

*举例：*

```java
import java.util;  //导入整个包
import java.util.*;  //导入包中所有的类
import java.util.Date; //导入包中的Date类
```

***注意：***导入包中所有类的时候，需要注意包的名字，因为有可能会两个包中可能存在相同名字的类，从而发生命名冲突，导致编译错误。

#### 4.5.2 静态导入

加上static关键字，就可以直接使用System类的静态方法和静态域，不用加类前缀。

```java
import static java.lang.System.*;
```

#### 4.5.3 将类放入包中

```java
package com.horstmann.corejava;
```

这条语句必须位于类的最前面，则这个文件中的类就会被放到com.horstmann.corejava包中。否则就放到默认包（没有名字的包）中。

注意类文件应该放到它属于的包的目录中。

#### 4.5.4 包作用域

**没有标记的类、方法、变量默认为包可见。**

private：必须显示的指明private，则这个类、方法、变量只能被定义他们的类使用。

public：如果没有指定或者标记为了public，则可以被同一个包的所有方法访问。

#### 4.5.5 jar包

**概念：**

JAR（Java ARchive）包就是别人已经写好的一些类，然后将这些类进行打包（合并到单个压缩文件里，就象Zip那样），你可以将这些jar包引入你的项目中，然后就可以直接使用这些jar包中的类和属性以及方法。

**使用场景举例：**

涉及因特网应用时，JAR文件显得特别有用。在JAR文件之前，Web浏览器必须重复多次请求Web服务器，以便下载完构成一个“程序片”（Applet）的所有文件。除此以外，每个文件都是未经压缩的。但在将所有这些文件合并到一个JAR文件里以后，只需向远程服务器发出一次请求即可。同时，由于采用了压缩技术，所以可在更短的时间里获得全部数据。

## 第5章  继承

“is-a”关系是继承的一个明显特征。关键字**extends**表示继承**。**

> The **extends** keyword **extends** a **class** (indicates that a **class** is inherited from another **class**). In **Java**, it is possible to inherit attributes and methods from one **class** to another.

**优点：**

1、可将多个类共有的功能放在一个超类中，这样就可以在更底层的类中重复使用这些功能；

2、对超类的修改将自动反映到其所有的子类、子类的子类等中，而无须修改或重新编译更底层的类，它们将通过继承获得新的信息。

### 5.1  类、超类、子类

#### 5.1.1 继承层次

![image-20190929102535033](/Users/baola/Library/Application Support/typora-user-images/image-20190929102535033.png)

- 在层次结构中越往下，类的用途越具体。在层次结构的顶部定义的是抽象概念，越往下，这些概念越具体。

- 子类可以继承父类的行为（方法）和属性（数据）。

**单继承：**

- Java 的继承形式称为单继承（single inheritance），因为每个Java 类都只能有一个超类（虽然任何超类都可以有多个子类）。

#### 5.1.2 Super

- 访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。
- 访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。

```java
// 父类
public class SuperExample {
    protected int x;
    protected int y;
    public SuperExample(int x, int y) {
        this.x = x;
        this.y = y;
    }
    public void func() {
        System.out.println("SuperExample.func()");
    }
}
```

```java
// 子类
public class SuperExtendExample extends SuperExample {
    private int z;
    public SuperExtendExample(int x, int y, int z) {
        super(x, y);  // 使用super函数，访问父类的构造函数，从而委托父类完成一些初始化的工作。
        this.z = z;
    }
    @Override
    public void func() {
        super.func();  // 使用super.method()，访问父类的某个方法，从而引用父类的方法实现。
        System.out.println("SuperExtendExample.func()");
    }
}
```

```java
SuperExample e = new SuperExtendExample(1, 2, 3); 
e.func();   
SuperExample.func() 
SuperExtendExample.func()
```

### 5.2 重写@Override

存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

为了满足里式替换原则，重写有以下三个限制：

- 子类方法的访问权限必须大于等于父类方法；
- 子类方法的返回类型必须是父类方法返回类型或为其子类型。
- 子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。

[@Override]() 注解，可以让编译器帮忙检查是否满足上面的三个限制条件。

**调用顺序：**

如果您调用了特定对象的某个方法，Java 虚拟机将首先检查该对象所属的类是否有该方法。如果没有，则在其超类中查找，依此类推，直到找到该方法的定义为止。

### 5.3 Object：所有类的超类

Java 类层次结构的顶端是类Object。所有的类都是从这个超类继承而来的。Object 是层次结构中最通用的类，定义了Java 类库中的所有类的行为。

*注：C++中没有根类的概念*

#### 5.3.1 equals()

用于判断一个对象是否等于另一个对象，具体的，判断两个对象是否具有相同的引用。

##### 1、等价关系

**比较顺序：先比较超类的域是否相等，然后再比较子类中的实例域。**

- 自反性

```java
x.equals(x); // true
```

- 对称性

```java
x.equals(y) == y.equals(x); // true
```

- 传递性

```java
if (x.equals(y) && y.equals(z))     
    x.equals(z); // true;
```

- 一致性：多次调用 equals() 方法结果不变

```java
x.equals(y) == x.equals(y); // true
```

- 对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false

```java
x.equals(null); // false;
```

##### 2、等价equals()与相等==

- 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
- 对于引用类型，== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。

```java
Integer x = new Integer(1); 
Integer y = new Integer(1); 
System.out.println(x.equals(y)); // true 
System.out.println(x == y);      // false
```

##### 3、equals(a, b)和a.equals(b)

java.util.Objects 7

```java
static boolean equals(Object a, Object b)
```

如果 a 和 b 都为 null，返回 true；如果只有一个为 null，返回 false；否则，返回 a.equals(b)。

#### 5.3.2 hashCode()

hashCode：散列码

##### hashCode和equals()

​	hashCode() 返回散列值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价。

**在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等**。

#### 5.3.3 toString()

是一种非常有用的调试工具，以便用户能够获得一些有关对象状态的必要信息。

### 5.4 枚举类

#### 5.4.1 枚举类

- 所有枚举类型都是Enum类的子类。

- 每个枚举类型都有一个静态的values方法，它将返回一个包含全部枚举值的数组。
- 相对应的，还有一个valuesof的方法

#### 5.4.2 怎么定义枚举类型？

举例： 

`public enum Size {S, M, L, XL}`

如果需要的话，可以在枚举类中添加一些构造器、方法和实例域。（备注一下，这里域就是指data，实例域）

```java
/**
 * Enums representing types of paimai auction.
 * 拍品类型
 *
 * Created by yuanguang.lx on 2018/6/6.
 */
public enum PaimaiAuctionType {
    // ()里面的格式依赖于构造函数
    Dutch((byte) 0, "降价拍", "Dutch - price down."),
    // OA_CeilingPrice((byte) 1, "封顶价拍", "Ceiling price open ascending."),
    OA_Immediate((byte) 2, "即刻拍", "Immediate open ascending."),
    OA_Ordinary((byte) 3, "普通增价拍", "Ordinary open ascending.");
    // OA_Speed((byte) 4, "极速拍", "Speed open ascending.");

    /**
     * Fetches enumeration corresponding to specified type.
     *
     * @param type: Type of required enumeration.
     * @return The corresponding enumeration. And null if no item corresponds to specified type.
     */
    public static PaimaiAuctionType fromType(byte type) {
        for (PaimaiAuctionType value : PaimaiAuctionType.values()) {
            if (value.type == type) {
                return value;
            }
        }
        return null;
    }

    /**
     * Getter of alias.
     */
    public String getAlias() {
        return this.alias;
    }

    /**
     * Getter of desc.
     */
    public String getDesc() {
        return this.desc;
    }

    /**
     * Getter of type.
     */
    public byte getType() {
        return this.type;
    }

    /**
     * 构造器.
     *
     * @param type:  Code of the type.
     * @param alias: Alias of the type.
     * @param desc:  Description for the type.
     */
    PaimaiAuctionType(byte type, String alias, String desc) {
        this.alias = alias;
        this.desc = desc;
        this.type = type;
    }

    /**
     * Alias of the type. Should be unique.
     */
    private String alias;

    /**
     * Description for the type.
     */
    private String desc;

    /**
     * Code of the type.
     */
    private byte type;
}
```



#### 5.4.3 编译实现

Java enum 枚举类的编译实现： https://blog.csdn.net/yizishou/article/details/71082123

看下面的例子：

原始代码：

```java
/* Color.java */
public enum Color {

    RED, GREEN, BLUE;

}
```

反编译后的代码：

```java
public final class Color extends Enum<Color> {

    public static final Color RED;
    public static final Color GREEN;
    public static final Color BLUE;

    // ---------- javac ----------
    private static final Color[] $VALUES;

    public static Color[] values() {
        return $VALUES.clone();
    }
    // ---------------------------

    // ---------- ECJ ----------
    // private static final Color[] ENUM$VALUES;
    //
    // public static Color[] values() {
    //     Color[] copy = new Color[ENUM$VALUES.length];
    //     System.arraycopy(ENUM$VALUES, 0, copy, 0, ENUM$VALUES.length);
    //     return copy;
    // }
    // -------------------------

    public static Color valueOf(String name) {
        return (Color) Enum.valueOf(Color.class, name);
    }

    private Color(String name, int ordinal) {
        super(name, ordinal);
    }

    static {
        RED = new Color("RED", 0);
        GREEN = new Color("GREEN", 0);
        BLUE = new Color("BLUE", 0);
        $VALUES = new Color[3];
        $VALUES[0] = RED;
        $VALUES[1] = GREEN;
        $VALUES[2] = BLUE;
    }

}
```

- 代码中声明的enum，在编译时会变成**`public final class`**，并且自动继承了Enum。这个动作必须由编译器来完成，直接这样写编译报错。
- 枚举中声明的所有枚举对象，编译时都会变成**`public static final`的常量**。
- 编译时会自动生成**`private`的构造函数**，接收两个参数，一个是枚举对象的名字，一个是位置。在构造函数中直接调用了`super(String, int)`，即`java.lang.Enum`的`protected`构造函数来构造对象。
- 编译时会自动添加**`static`代码块**，来初始化所有的枚举对象，并添加到自动生成的一个数组常量中存储起来。这个数据常量的名字不是固定的，完全取决于编译器，ECJ编译的是`ENUM $VALUES`，javac编译的是 `$VALUES`。
- 编译时会自动生成`public static`方法**`values()`**，这个是大家很常用的方法，返回所有枚举对象的数组，原来也是在编译时期隐式添加的。两种编译器对此方法的实现稍有不同。
- 编译时会自动生成`public static`方法**`valueOf(String)`**，直接调用了`Enum.valueOf`。



### 5.5 反射

能够分析类的能力的程序被称为**反射**。   

反射可以提供**运行时**的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。这种 **动态的获取信息**以及 **动态调用对象的方法**的功能称为 **java 的反射机制**。

反射可以用来：

- 运行中分析类的能力：Java 反射机制在程序**运行时**，对于任意一个类，都能够知道这个类的所有属性和方法，可以对其fields(变量)设值或调用其methods(方法)；对于任意一个对象，都能够调用它的任意一个方法和属性。
- 在运行中查看对象：例如，编写一个toString()方法供所有类使用。
- 实现通用的数组操作代码。
- 利用Method对象，Method对象很像C++中的函数指针。

#### Class类

**概念：**

​	每个类都有一个 Class 对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。

**类加载：**

​	类加载相当于 Class 对象的加载，类在第一次使用时才动态加载到 JVM 中。也可以使用`Class.forName("com.mysql.jdbc.Driver")`这种方式来控制类的加载，该方法会返回一个 Class 对象。

**getClass()：**

​	Object 类中的 getClass() 方法将会返回一个 Class 类型的实例。

#### 5.5.1 利用反射分析类的能力

Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

- Field ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
- Method ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
- Constructor ：可以用 Constructor 的 newInstance() 创建新的对象。

**getName()：**

Feild 、Method 、Constructor均有一个 getName() 的方法，用来获取项目的名称。

举例说明：

定义一个 `FatherClass`类（默认继承自 `Object`类），然后定义一个继承自 `FatherClass`类的 `SonClass`类。

**FatherClass.java**

```java
public class FatherClass {
    public String mFatherName;
    public int mFatherAge;

    public void printFatherMsg(){}
}
```

**SonClass.java**

```java
public class SonClass extends FatherClass{

    private String mSonName;
    protected int mSonAge;
    public String mSonBirthday;
  public static String mSonholiday;
    public void printSonMsg(){
        System.out.println("Son Msg - name : "
                + mSonName + "; age : " + mSonAge);
    }
  
    private void setSonName(String name){
        mSonName = name;
    }

    private void setSonAge(int age){
        mSonAge = age;
    }

    private int getSonAge(){
        return mSonAge;
    }

    private String getSonName(){
        return mSonName;
    }
}
```

##### 5.5.1.1 获取类的所有变量(包括静态变量)信息

`getFields()`

`getDeclaredFields()`

`getType()`

`getModifiers`

```java
/**
 * 通过反射获取类的所有变量
 */
private static void printFields(){
    //1.获取并输出类的名称
    Class mClass = SonClass.class;
    System.out.println("类的名称：" + mClass.getName());

    //2.1 获取所有 public 访问权限的变量，包括本类声明的和从父类继承的
    Field[] fields = mClass.getFields();

    //2.2 获取所有本类声明的全部域（不限制public权限）
    //Field[] fields = mClass.getDeclaredFields();

    //3. 遍历变量并输出变量信息
    for (Field field :
            fields) {
        //获取访问权限并输出
        int modifiers = field.getModifiers();
        //Modifier.toString方法将修饰符打印出来
        System.out.print(Modifier.toString(modifiers) + " ");
        //输出变量的类型及变量名
        System.out.println(field.getType().getName()
                 + " " + field.getName());
    }
}
```

注意上述`getFields()`与  `getDeclaredFields()`之间的区别：

- 调用`getFields()`方法，输出 `SonClass`类以及其所继承的父类( 包括 `FatherClass`和 `Object`) 的 `public`方法。注：`Object`类中没有成员变量，所以没有输出。

**输出结果：**

```java
类的名称：obj.SonClass
  public static java.lang.String mSonholiday
  public java.lang.String mSonBirthday
  public java.lang.String mFatherName
  public int mFatherAge
```

- 调用`getDeclaredFields()`， 输出 `SonClass`类的所有成员变量，不问访问权限。

**输出结果：**

```
类的名称：obj.SonClass
  private java.lang.String mSonName
  protected int mSonAge
  public static java.lang.String mSonholiday
  public java.lang.String mSonBirthday
```



##### 5.5.1.2 获取类的所有方法(包括静态方法)信息

```java
/**
 * 通过反射获取类的所有方法
 */
private static void printMethods(){
    //1.获取并输出类的名称
    Class mClass = SonClass.class;
    System.out.println("类的名称：" + mClass.getName());

    //2.1 获取所有 public 访问权限的方法
    //包括自己声明和从父类继承的
    Method[] mMethods = mClass.getMethods();

    //2.2 获取所有本类的的方法（不限制哪种访问权限）
    //Method[] mMethods = mClass.getDeclaredMethods();

    //3.遍历所有方法
    for (Method method :
            mMethods) {
        //获取并输出方法的访问权限（Modifiers：修饰符）
        int modifiers = method.getModifiers();
        //Modifier.toString方法将修饰符打印出来
        System.out.print(Modifier.toString(modifiers) + " ");
        //获取并输出方法的返回值类型
        Class returnType = method.getReturnType();
        System.out.print(returnType.getName() + " "
                + method.getName() + "( ");
        //获取并输出方法的所有参数
        Parameter[] parameters = method.getParameters();
        for (Parameter parameter:
             parameters) {
            System.out.print(parameter.getType().getName()
                    + " " + parameter.getName() + ",");
        }
        //获取并输出方法抛出的异常
        Class[] exceptionTypes = method.getExceptionTypes();
        if (exceptionTypes.length == 0){
            System.out.println(" )");
        }
        else {
            for (Class c : exceptionTypes) {
                System.out.println(" ) throws "
                        + c.getName());
            }
        }
    }
}
```



注意上述`getMethods()`与  `getDeclaredMethods()`之间的区别：

- 调用`getMethods()`方法：获取 `SonClass`类所有 `public`访问权限的方法，包括从父类继承的。

**输出结果：**

```
类的名称：obj.SonClass
  public void printSonMsg(  )
  public void printFatherMsg(  )
  public static void printSonHodiday(  )
  public final void wait(  ) throws java.lang.InterruptedException
  public final void wait( long arg0,int arg1, ) throws java.lang.InterruptedException
  public final native void wait( long arg0, ) throws java.lang.InterruptedException
  public boolean equals( java.lang.Object arg0, )
  public java.lang.String toString(  )
  public native int hashCode(  )
  public final native java.lang.Class getClass(  )
  public final native void notify(  )
  public final native void notifyAll(  )
```



- 调用 `getDeclaredMethods()`方法，输出的都是 `SonClass`类的方法，不问访问权限。

**输出结果：**

```
类的名称：obj.SonClass
  private int getSonAge(  )
  private void setSonAge( int arg0, )
  public void printSonMsg(  )
  public static void printSonHodiday(  )
  private void setSonName( java.lang.String arg0, )
  private java.lang.String getSonName(  )
```



##### 5.5.1.3 获取类的所有构造器信息

```java
/**
* 通过反射获取类的所有构造器
*/
private static void printConstructors(String[] args) {
        //1.获取并输出类的名称
        Class mClass = SonClass.class;
        System.out.println("类的名称：" + mClass.getName());
        //2.1 获取所有构造器
        Constructor[] constructors =  mClass.getDeclaredConstructors();

        //2.2 获取所有本类的所有公有构造器
       // Constructor[] constructors =  mClass.getConstructors();

        //3. 遍历所有构造器
        for (Constructor c: constructors) {
            String name = c.getName();
            System.out.print("   ");
            //获取访问权限并输出,Modifier.toString方法将修饰符打印出来
            String modifiers = Modifier.toString(c.getModifiers());
            if (modifiers.length() > 0) System.out.print(modifiers + " ");
            System.out.print(name + "(");

            //获取并输出构造器的所有参数
            Class[] paramTypes = c.getParameterTypes();
            for (int j = 0; j < paramTypes.length; j++)
            {
                if (j > 0) System.out.print(", ");
                System.out.print(paramTypes[j].getName());
            }
            System.out.println(");");
        }
    }
```



**输出结果：**

```
类的名称：obj.SonClass
   public obj.SonClass();
```

- 调用 `getDeclaredConsturctors()`方法得到所有构造器。
- 调用 `getMethods()`方法，得到该类的所有公有构造器。



#### 5.5.2 使用反射访问/操作私有变量和方法



在上一节，成功获取了类的变量和方法信息，验证了在运行时 **动态的获取信息**的结论。但是，通过反射，也可以**访问或操作**类的**私有变量/方法**。



举例说明：

**TestClass.java**

```
public class TestClass {

    private String MSG = "Original";
    private void privateMethod(String head , int tail){
        System.out.print(head + tail);
    }
    public String getMsg(){
        return MSG;
    }
}
```



##### 5.5.2.1 操作私有方法

以访问 `TestClass`类中的私有方法 `privateMethod(...)`为例。

```
/**
 * 访问对象的私有方法
 */
private static void getPrivateMethod() throws Exception{
    //1. 获取 Class 类实例
    TestClass testClass = new TestClass();
    Class mClass = testClass.getClass();

    //2. 获取私有方法
    //第一个参数为要获取的私有方法的名称
    //第二个为要获取方法的参数的类型，参数为 Class...，没有参数就是null
    //方法参数也可这么写 ：new Class[]{String.class , int.class}
    Method privateMethod =
            mClass.getDeclaredMethod("privateMethod", String.class, int.class);

    //3. 开始操作方法
    if (privateMethod != null) {
        //获取私有方法的访问权（只是获取访问权，并不是修改实际权限)
        privateMethod.setAccessible(true);

        //使用 invoke 反射调用私有方法
        //privateMethod 是获取到的私有方法
        //testClass 要操作的对象
        //后面两个参数传实参
        privateMethod.invoke(testClass, "Java Reflect ", 666);
    }
}
```



上述第3步的`setAccessible(true)`方法，是获取私有方法的访问权限，如果不加会报异常 **IllegalAccessException**，如下所示，因为当前方法访问权限是“private”。

```
java.lang.IllegalAccessException: Class MainClass can not access a member of class obj.TestClass with modifiers "private"
```



正常运行后，打印如下，**调用私有方法**成功：

```
Java Reflect 666
```



##### 5.5.2.2 修改私有变量



以修改 `TestClass`类中的私有变量 `MSG`为例，其初始值为 "Original" ，我们要修改为 "Modified"。

```
/**
 * 修改对象私有变量的值
 */
private static void modifyPrivateFiled() throws Exception {
    //1. 获取 Class 类实例
    TestClass testClass = new TestClass();
    Class mClass = testClass.getClass();

    //2. 获取私有变量
    Field privateField = mClass.getDeclaredField("MSG");

    //3. 操作私有变量
    if (privateField != null) {
        //获取私有变量的访问权
        privateField.setAccessible(true);

        //修改私有变量，并输出以测试
        System.out.println("Before Modify：MSG = " + testClass.getMsg());

        //调用 set(object , value) 修改变量的值
        //privateField 是获取到的私有变量
        //testClass 要操作的对象
        //"Modified" 为要修改成的值
        privateField.set(testClass, "Modified");
        System.out.println("After Modify：MSG = " + testClass.getMsg());
    }
}
```



**输出结果：**

```
Before Modify：MSG = Original
After Modify：MSG = Modified
```



从输出信息看出 **修改私有变量**成功.

**私有常量也可以修改**，解释涉及JVM的知识点，后面再补充。



#### 5.5.3 使用反射调用属性/方法/构造函数/静态方法(总结前一节)



**定义一个person类**:

```
public class Person{
  public String name;
  private int age;
  protected double k;
  float m; 

public Person(String name, int age) {
    super();
    this.name = name;
    this.age = age;
}
public Person() {
    super();
}

private Person(String name, int age, double k, float m) {
    super();
    this.name = name;
    this.age = age;
    this.k = k;
    this.m = m;
}
public String getName() {
    return name;
}
public void setName(String name) {
    this.name = name;
}
public int getAge() {
    return age;
}
public void setAge(int age) {
    this.age = age;
}
@Override
public String toString() {
    return "Person [name=" + name + ", age=" + age + "]";
}

private void display() {
    System.out.println("调用display");
}
protected void m() { 
    System.out.println("调用m");
}
public void show1(String a) {
    System.out.println(a);
}
public static void show2() {
    System.out.println("调用show3");
}
@Override
public int compareTo(Object o) {
    return 0;
  }
}
```



用测试类**TestClass**进行测试

**TestClass**:

```
public class TestClass {
    
    public void test1() throws Exception {
        Class clazz = Class.forName("Person");
        //公共的属性（public修饰）
        Field field1 = clazz.getField("name");
        //必须要有一个显示的空构造函数
        Person p = (Person) clazz.newInstance();
        System.out.println(p);
        field1.set(p, "Jack");
        System.out.println(p);
        
        //所有的变量（public、protected、default和private）
        Field field2 = clazz.getDeclaredField("age");
        //这里age被private修饰所以需要设置是否可以被访问
        field2.setAccessible(true);
        field2.set(p, 20);
        System.out.println(p);
    }
    
    public void test2() throws Exception {
        //公共方法toString()
        Class clazz = Class.forName("Person");
        Method method1 = clazz.getMethod("toString");
        //调用指定的带参数构造函数
        Constructor constructor = clazz.getConstructor(String.class,int.class);
        Person p = (Person)constructor.newInstance("Rose",19);
   
        method1.invoke(p);
        System.out.println(p);
        
        //该类所有的方法
        //调用私有方法
        Method mehtod2 = clazz.getDeclaredMethod("display");
        //由于display方法被private修饰，所以需要设置可访问
        mehtod2.setAccessible(true);
        mehtod2.invoke(p);
        
        //调用带有参数的方法
        Method method3 = clazz.getDeclaredMethod("show1",String.class);
        method3.invoke(p, "hello");
        
        //调用静态方法
        Method method4 = clazz.getDeclaredMethod("show2");
        method4.invoke(Person.class);        
    } 
}
```



**输出结果：**

```
Person [name=null, age=0]
Person [name=Jack, age=0]
Person [name=Jack, age=20]
Person [name=Rose, age=19]
调用display
hello
调用show3
```



反射也有其缺点，如下：

- 性能开销 ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。
- 安全限制 ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。
- 内部暴露 ：由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。反射代码破坏了抽象性。



## 第6章 接口、lambda表达式与内部类

### 6.1 接口

#### **什么是接口？**

**java的三种：**

1. 接口：就是声明，不能有方法实现
2. 抽象类：可以有实现，也可以没有
3. 类：必须有实现

> ​	接口（interface）可以说成是抽象类的一种特例，接口中的所有方法都必须是抽象的。接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。
>
> ​	接口不是类，是对类的一组需求的描述，这些类遵从接口描述的统一格式进行定义。

​    **从 Java 8 开始，接口也可以拥有默认的方法实现**，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。

​	接口的成员（字段 + 方法）默认都是 **public abstract**的，接口中的成员变量类型默认为public static final，方法定义默认为public abstract类型。并且不允许定义为 private 或者 protected。

#### **为什么用接口？**

​	因为他可以使程序解藕。因为接口中定义了一系列的抽象方法，而抽象方法在一定的程度上为公共方法的书写提供了标准，如方法名、返回类型、参数等。这样我们只需要根据接口实现相应的方法即可向其他的程序提供相应的功能，任何一个类都可以实现接口提供功能，也增加了程序的可扩展性能。

#### 6.1.1 使用接口还是抽象类

**使用接口：**

- 需要让不相关的类都实现一个方法，例如不相关的类都可以实现 Compareable 接口中的 compareTo() 方法；
- 需要使用<font color = dark>多重继承</font>。

**使用抽象类:**   

- 需要在几个相关的类中共享代码。
- 需要能控制继承来的成员的访问权限，而不是都为 public。
- <font color = dark>需要继承非静态和非常量字段。</font>

一般情况下，**接口优先于抽象类**。因为接口没有抽象类严格的类层次结构要求，可以灵活地为一个类添加行为。并且从 Java 8 开始，接口也可以有默认的方法实现，使得修改接口的成本也变的很低。

#### 6.1.2 对象克隆（clone())

##### 6.1.2.1 cloneable

clone() 是 Object 的 protected 方法，它不是 public，一个类不显式的重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。

```java
public class CloneExample {
    private int a;
    private int b;
}
CloneExample e1 = new CloneExample();
// CloneExample e2 = e1.clone(); // 'clone()' has protected access in 'java.lang.Object'
```

重写 clone() 得到以下实现：

```java
public class CloneExample {
    private int a;
    private int b;

    // 重写clone方法，将其声明为public的
    @Override  
    public CloneExample clone() throws CloneNotSupportedException {
        return (CloneExample)super.clone();
    }
}
```

```java
CloneExample e1 = new CloneExample();
try {
    CloneExample e2 = e1.clone();
} catch (CloneNotSupportedException e) {
    e.printStackTrace();
}
```

**输出：**

```
java.lang.CloneNotSupportedException: CloneExample
```

以上抛出了 CloneNotSupportedException，这是因为 CloneExample 没有实现 Cloneable 接口。

应该注意的是，clone() 方法并不是 Cloneable 接口的方法，而是 Object 的一个 protected 方法。Cloneable 接口只是一个标记接口，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，就会抛出 CloneNotSupportedException。

```java
public class CloneExample implements Cloneable {
    private int a;
    private int b;

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```



##### 6.1.2.2 浅拷贝

拷贝变量和原始变量引用同一个对象。也就是说，改变一个变量引用的对象，将会对另一个变量造成影响。

clone函数的默认实现就是浅拷贝。

```java
public class ShallowCloneExample implements Cloneable {

    private int[] arr;

    public ShallowCloneExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

  	// clone函数的默认实现就是浅拷贝。
    @Override
    protected ShallowCloneExample clone() throws CloneNotSupportedException {
        return (ShallowCloneExample) super.clone();
    }
}
```

```java
ShallowCloneExample e1 = new ShallowCloneExample();
ShallowCloneExample e2 = null;
try {
    e2 = e1.clone();
} catch (CloneNotSupportedException e) {
    e.printStackTrace();
}
e1.set(2, 222);
System.out.println(e2.get(2)); // 222
```

##### 6.1.2.2 深拷贝

拷贝变量和原始变量引用的是不同对象。

```java
public class DeepCloneExample implements Cloneable {

    private int[] arr;

    public DeepCloneExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

    @Override
    protected DeepCloneExample clone() throws CloneNotSupportedException {
        DeepCloneExample result = (DeepCloneExample) super.clone();
        result.arr = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            result.arr[i] = arr[i];
        }
        return result;
    }
```

```java
DeepCloneExample e1 = new DeepCloneExample();
DeepCloneExample e2 = null;
try {
    e2 = e1.clone();
} catch (CloneNotSupportedException e) {
    e.printStackTrace();
}
e1.set(2, 222);
System.out.println(e2.get(2)); // 2
```



##### 6.1.2.3  clone() 的替代方案

使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换，最好不要去使用 clone()，**可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象。**

```java
public class CloneConstructorExample {

    private int[] arr;

    public CloneConstructorExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }
    public CloneConstructorExample(CloneConstructorExample original) {
        arr = new int[original.arr.length];
        for (int i = 0; i < original.arr.length; i++) {
            arr[i] = original.arr[i];
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }
}
```

```java
CloneConstructorExample e1 = new CloneConstructorExample();
CloneConstructorExample e2 = new CloneConstructorExample(e1);
e1.set(2, 222);
System.out.println(e2.get(2)); // 2
```



### 6.2  lambda表达式

在Java 8以前的代码中，为了实现带一个方法的接口，往往需要定义一个匿名类并复写接口方法，代码显得很臃肿。比如常见的`Comparator`接口：

```java
String[] oldWay = "Improving code with Lambda expressions in Java 8".split(" ");
Arrays.sort(oldWay, new Comparator<String>() {
    @Override
    public int compare(String s1, String s2) {
        // 忽略大小写排序:
        return s1.toLowerCase().compareTo(s2.toLowerCase());
    }
});
System.out.println(String.join(", ", oldWay));
```

#### lambda函数

##### 1、直接使用的时候实现

**对于只有一个方法的接口，在Java 8中，现在可以把它视为一个函数，用lambda表示式简化如下：**

```java
String[] newWay = "Improving code with Lambda expressions in Java 8".split(" ");
Arrays.sort(newWay, (s1, s2) -> {
    return s1.toLowerCase().compareTo(s2.toLowerCase());
});
System.out.println(String.join(", ", newWay));
```

Java 8没有引入新的关键字lambda，而是**用`()->{}`这个符号表示lambda函数**。函数类型不需要申明，可以由接口的方法签名自动推导出来，对于上面的lambda函数：

```java
(s1, s2) -> {
    return s1.toLowerCase().compareTo(s2.toLowerCase());
});
```

参数由`Comparator<String>`自动推导出`String`类型，返回值也必须符合接口的方法签名。

##### 2、 类内定义方法的时候实现

**实际上，lambda表达式最终也被编译为一个实现类，不过语法上做了简化。**对于Java自带的标准库里的大量单一方法接口，很多都已经标记为`@FunctionalInterface`，表明该接口可以作为函数使用。

以`Runnable`接口为例，现在可以用lambda表达式`()->{}`实现：

```java
public static void main(String[] args) {
    // old way:
    Runnable oldRunnable = new Runnable() {
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName() + ": Old Runnable");
        }
    };
    Runnable newRunnable = () -> {
        System.out.println(Thread.currentThread().getName() + ": New Lambda Runnable");
    };
    new Thread(oldRunnable).start();
    new Thread(newRunnable).start();
}
```

### 6.3  内部类

在 Java 中，可以将一个类定义在另一个类里面或者一个方法里面，这样的类称为内部类。广泛意义上的内部类一般来说包括这四种：

- 成员内部类
- 局部内部类
- 匿名内部类
- 静态内部类

#### 6.3.1 成员内部类

成员内部类是最普通的内部类，它的定义为位于另一个类的内部，形如下面的形式：

```java
class Circle {
    double radius = 0;
     
    public Circle(double radius) {
        this.radius = radius;
    }
     
    class Draw {     //内部类
        public void drawSahpe() {
            System.out.println("drawshape");
        }
    }
}
```

类Draw像是类Circle的一个成员，Circle称为**外部类**。

##### 1、成员内部类访问外部类

成员内部类可以无条件访问外部类的所有成员属性和成员方法（包括private成员和静态成员）。

**注意: **当成员内部类拥有和外部类同名的成员变量或者方法时，会发生隐藏现象，即默认情况下访问的是成员内部类的成员。如果要访问外部类的同名成员，需要以下面的形式进行访问：

```java
外部类.this.成员变量
外部类.this.成员方法
```

##### 2、外部类访问成员内部类

在外部类中如果要访问成员内部类的成员，必须先创建一个成员内部类的对象，再通过指向这个对象的引用来访问：

```java
class Circle {
    private double radius = 0;
 
    public Circle(double radius) {
        this.radius = radius;
        getDrawInstance().drawSahpe();   //必须先创建成员内部类的对象，再进行访问
    }
     
    private Draw getDrawInstance() {
        return new Draw();
    }
     
    class Draw {     //内部类
        public void drawSahpe() {
            System.out.println(radius);  //外部类的private成员
        }
    }
}
```

##### 3、类外面创建成员内部类的对象

成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提是必须存在一个外部类的对象。创建成员内部类对象的一般方式如下：

两种方式：

- new
- 接口

```java
public class Test {
    public static void main(String[] args)  {
        //第一种方式：
        Outter outter = new Outter();
        Outter.Inner inner = outter.new Inner();  //必须通过Outter对象来创建
         
        //第二种方式：
        Outter.Inner inner1 = outter.getInnerInstance();
    }
}
 
class Outter {
    private Inner inner = null;
    public Outter() {
         
    }
    public Inner getInnerInstance() {
        if(inner == null)
            inner = new Inner();
        return inner;
    }   
    class Inner {
        public Inner() {
             
        }
    }
}
```

##### 4、成员内部类的访问权限

内部类可以拥有 private 访问权限、protected 访问权限、public 访问权限及包访问权限。如上面的例子，

1. 如果成员内部类 Inner 用 private 修饰，则只能在外部类的内部访问，如果用 public 修饰，则任何地方都能访问；
2. 如果用 protected 修饰，则只能在同一个包下或者继承外部类的情况下访问；
3. 如果是默认访问权限 public ，则只能在同一个包下访问。

这一点和外部类有一点不一样，**外部类只能被 public 和包访问两种权限修饰**。由于成员内部类看起来像是外部类的一个成员，所以可以像类的成员一样拥有多种权限修饰。



#### 6.3.2 局部内部类

局部内部类是定义在一个方法或者一个作用域里面的类，它和成员内部类的区别在于局部内部类的访问仅限于方法内或者该作用域内。

```java
class People{
    public People() {
         
    }
}
class Man{
    public Man(){
         
    }
     
    public People getWoman(){
        class Woman extends People{   //局部内部类
            int age =0;
        }
        return new Woman();
    }
}
```

**注意**: 局部内部类就像是方法里面的一个局部变量一样，是不能有 public、protected、private 以及 static 修饰符的，但他可以**访问当前代码块内的常量**以及此**外围类的所有成员**。



#### 6.3.3 匿名内部类

匿名内部类应该是平时编写代码时用得最多的，在编写事件监听的代码时使用匿名内部类不但方便，而且使代码更加容易维护。下面这段代码是一段 Android 事件监听代码：

```java
scan_bt.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
         
    }
});
 
history_bt.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
        // TODO Auto-generated method stub   
    }
});
```

这段代码为两个按钮设置监听器，这里面就使用了匿名内部类。这段代码中的：

```java
new OnClickListener() {
    @Override
    public void onClick(View v) {
        // TODO Auto-generated method stub
         
    }
}
```

就是匿名内部类的使用。

##### 匿名内部类的优点

代码中需要给按钮设置监听器对象，使用匿名内部类能够在实现父类或者接口中的方法情况下同时产生一个相应的对象，但是前提是这个父类或者接口必须先存在才能这样使用。当然像下面这种写法也是可以的，跟上面使用匿名内部类达到效果相同。

```java
private void setListener()
{
    scan_bt.setOnClickListener(new Listener1());       
    history_bt.setOnClickListener(new Listener2());
}
 
class Listener1 implements View.OnClickListener{
    @Override
    public void onClick(View v) {
    // TODO Auto-generated method stub
             
    }
}
 
class Listener2 implements View.OnClickListener{
    @Override
    public void onClick(View v) {
    // TODO Auto-generated method stub
             
    }
}
```

这种写法虽然能达到一样的效果，但是既冗长又难以维护，所以一般使用匿名内部类的方法来编写事件监听代码。同样的，**匿名内部类也是不能有访问修饰符和 static 修饰符的**。

**匿名内部类是唯一一种没有构造器的类**。正因为其没有构造器，所以匿名内部类的使用范围非常有限，大部分匿名内部类用于接口回调。匿名内部类在编译的时候由系统自动起名为 Outter$1.class。一般来说，匿名内部类用于继承其他类或是实现接口，并不需要增加额外的方法，只是对继承方法的实现或是重写。

**比较局部内部类和匿名内部类：**

匿名内部类和局部内部类很相似，名字对外不可见，但匿名内部类只能用于实例初始化，而局部内部类可以重载构造器；若需要不止一个该内部类的对象时使用局部内部类。

#### 6.3.4  静态内部类

**静态内部类**也是定义在另一个类里面的类，只不过在类的前面**多了一个关键字static**。**静态内部类是不需要依赖于外部类的**，这点和类的静态成员属性有点类似，并且它不能使用外部类的非static成员变量或者方法，这点很好理解，因为在没有外部类的对象的情况下，可以创建静态内部类的对象，如果允许访问外部类的非static成员就会产生矛盾，因为外部类的非static成员必须依附于具体的对象。

```java
public class Test {
    public static void main(String[] args)  {
        Outter.Inner inner = new Outter.Inner();
    }
}
class Outter {
    public Outter() {
         
    }     
    static class Inner {
        public Inner() {
             
        }
    }
}
```

#### 6.3.5 为什么使用内部类

1. 匿名内部类

   一般来说，匿名内部类用于继承其他类或是实现接口，并不需要增加额外的方法，只是对继承方法的实现或是重写。

2. 多重继承

   内部类使得多重继承的解决方案变得更完整，内部类有效的实现了“多重继承”，内部类允许继承多个非接口类型（类或抽象类，如果拥有的是**抽象**的类或者具体的类，而不是接口，那么只有使用内部类才能实现多重继承)

#### 6.3.6 内部类的继承

由于内部类的构造器必须连接到指向其外围类对象的引用，所以在继承内部类的时候，事情会变得复杂，那个指向外围类对象的“秘密的”引用必须被初始化，需使用特殊的语法，示例代码如下：

```java
class WithInner {
  class Inner {}
}

public class InheritInner extends WithInner.Inner {
  //! InheritInner() {} // Won't compile
  InheritInner(WithInner wi) {
    wi.super();
  }
  public static void main(String[] args) {
    WithInner wi = new WithInner();
    InheritInner ii = new InheritInner(wi);
  }
}
```

从上可以看到，**InheritInner**只继承自内部类，而不是外围类，当要生成一个构造器时，默认的构造器并不算好，而且不能只是传递一个指向外围类对象的引用，必须在构造器内使用如下语法：

```
enclosingClassReference.super();
```

**小结：**

接口和内部类比较深奥复杂，根据情况判断是使用接口还是内部类或两者同时使用。



## 第7章 异常、断言和日志



### 7.1 异常



#### Java异常类层次结构图

**Throwable**可以用来表示任何可以作为异常抛出的类，分为两种： **Error（错误）**和 **Exception（异常）**。

**Error（错误）:是程序无法处理的错误**，表示运行应用程序中较严重问题。大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。例如，Java虚拟机运行错误（Virtual MachineError），当 JVM 不再有继续执行操作所需的内存资源时，将出现 OutOfMemoryError。这些异常发生时，Java虚拟机（JVM）一般会选择线程终止。

这些错误表示故障发生于虚拟机自身、或者发生在虚拟机试图执行应用时，如Java虚拟机运行错误（Virtual MachineError）、类定义错误（NoClassDefFoundError）等。这些错误是不可查的，因为它们在应用程序的控制和处理能力之 外，而且绝大多数是程序运行时不允许出现的状况。对于设计合理的应用程序来说，即使确实发生了错误，本质上也不应该试图去处理它所引起的异常状况。在 Java中，错误通过Error的子类描述。



**Exception（异常）:是程序本身可以处理的异常**。Exception 类有一个重要的子类 **RuntimeException**。RuntimeException 异常由Java虚拟机抛出。**NullPointerException**（要访问的变量没有引用任何对象时，抛出该异常）、**ArithmeticException**（算术运算异常，一个整数除以0时，抛出该异常）和 **ArrayIndexOutOfBoundsException**（下标越界异常）。



**注意：异常和错误的区别：异常能被程序本身可以处理，错误是无法处理。**



Throwable 分为两种：

- **非受检异常**：派生于Error类或RuntimeException类的所有异常。是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。
- **受检异常**：不是非受检测异常的异常。需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；



#### Throwable类常用方法

- **public string getMessage()**:返回异常发生时的详细信息
- **public string toString()**:返回异常发生时的简要描述
- **public string getLocalizedMessage()**:返回异常对象的本地化信息。使用Throwable的子类覆盖这个方法，可以声称本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与getMessage（）返回的结果相同
- **public void printStackTrace()**:在控制台上打印Throwable对象封装的异常信息



#### 异常处理总结

- **try 块：**用于捕获异常。其后可接零个或多个catch块，如果没有catch块，则必须跟一个finally块。
- **catch 块：**用于处理try捕获到的异常。
- **finally 块：**无论是否捕获或处理异常，finally块里的语句都会被执行。当在try块或catch块中遇到return 语句时，finally语句块将在方法返回之前被执行。



**在以下4种特殊情况下，finally块不会被执行：**

1. 在finally语句块第一行发生了异常。 因为在其他行，finally块还是会得到执行
2. 在前面的代码中用了System.exit(int)已退出程序。 exit是带参函数 ；若该语句在异常语句之后，finally会执行
3. 程序所在的线程死亡。
4. 关闭CPU。



**注意**： 当try语句和finally语句中都有return语句时，在方法返回之前，finally语句的内容将被执行，并且finally语句的返回值将会覆盖原始的返回值。如下：



```
public static int f(int value) {
        try {
            return value * value;
        } finally {
            if (value == 2) {
                return 0;
            }
        }
    }
```



如果调用 `f(2)`，返回值将是0，因为finally语句的返回值覆盖了try语句块的返回值。



### 7.2 断言



断言是一种测试和调试阶段所使用的战术性工具。



关键字**assert**，有两种形式：



- assert 条件；
- assert 条件：表达式



### 7.3 日志



日志记录一种在程序的整个生命周期都可以使用的策略性工具。



要生成基本日志，可使用**全局日志记录器（global logger）**并调用其info方法：



```
Logger.getGlobal().info("Message!");
```



记录就会显示`INFO:Message!`



若调用如下代码，将会取消所有日志：



```
Logger.getGlobal().setLevel(Level.OFF)
```



可以调用`getLogger`方法创建获取记录器：



```
private static final Logger myLogger = Logger.getLogger("com.mycompany.myapp");
```



用一个静态变量存储日志记录器的引用可防止日记记录器被垃圾回收。



通常有7个日志记录器级别：



- SEVERE（最高）
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST



默认情况下，只记录前三个级别，但可通过下面方法来设置其他的级别.



```
logger.setLevel(Level.FINE)
```



设置完成后，FINE和更高级别的记录都可以记录下来。



#### 7.3.1 修改日志管理器配置



Logger默认的级别是INFO，比INFO更低的日志将不显示。Logger的默认级别定义是在jre安装目录的lib下面。



```
# Limit the message that are printed on the console to INFO and above. 
java.util.logging.ConsoleHandler.level = INFO
```



#### 7.3.2 处理器Handler



Handler 对象从 Logger 中获取日志信息，并将这些信息导出。例如，它可将这些信息写入控制台或文件中，也可以将这些信息发送到网络日志服务中，或将其转发到操作系统日志中。



可通过执行 setLevel(Level.OFF) 来禁用 Handler，并可通过执行适当级别的 setLevel 来重新启用。Handler 类通常使用 LogManager 属性来设置 Handler 的 Filter、Formatter 和 Level 的默认值。



**默认的日志配置**将级别**等于或高于INFO级别**的所有消息记录到控制台。



例如，想要在**控制台**上看到**FINE**级别的消息，需要进行如下设置：



```
java.util.logging.ConsoleHandler.level=FINE
```



## 第8章 泛型程序设计



### 8.1 Java泛型



Java 泛型（generics）是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。



泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。



```
public class Box<T> {
    // T stands for "Type"
    private T t;
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```



### 8.2 泛型方法



写一个泛型方法，该方法在调用时可以接收不同类型的参数。根据传递给泛型方法的参数类型，编译器适当地处理每一个方法调用。



下面是定义泛型方法的规则：



- 所有泛型方法声明都有一个类型参数声明部分（由尖括号分隔），该类型参数声明部分在方法返回类型之前（在下面例子中的）。
- 每一个类型参数声明部分包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。
- 类型参数能被用来声明返回值类型，并且能作为泛型方法得到的实际参数类型的占位符。
- 泛型方法体的声明和其他方法一样。注意类型参数只能代表引用型类型，不能是原始类型（像int,double,char的等）



**实例**：下面的例子演示了如何使用泛型方法打印不同字符串的元素：



```
public class GenericMethodTest
{
   // 泛型方法 printArray                         
   public static < E > void printArray( E[] inputArray )
   {
      // 输出数组元素            
         for ( E element : inputArray ){        
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
 
    public static void main( String args[] )
    {
        // 创建不同类型数组： Integer, Double 和 Character
        Integer[] intArray = { 1, 2, 3, 4, 5 };
        Double[] doubleArray = { 1.1, 2.2, 3.3, 4.4 };
        Character[] charArray = { 'H', 'E', 'L', 'L', 'O' };
 
        System.out.println( "整型数组元素为:" );
        printArray( intArray  ); // 传递一个整型数组
 
        System.out.println( "\n双精度型数组元素为:" );
        printArray( doubleArray ); // 传递一个双精度型数组
 
        System.out.println( "\n字符型数组元素为:" );
        printArray( charArray ); // 传递一个字符型数组
    } 
}
```



运行结果如下所示：



```
整型数组元素为:
1 2 3 4 5 
双精度型数组元素为:
1.1 2.2 3.3 4.4 
字符型数组元素为:
H E L L O
```



有界的类型参数:



可能有时候，你会想限制那些被允许传递到一个类型参数的类型种类范围。例如，一个操作数字的方法可能只希望接受Number或者Number子类的实例。这就是有界类型参数的目的。



要声明一个有界的类型参数，首先列出类型参数的名称，后跟extends关键字，最后紧跟它的上界。



下面的例子演示了"extends"如何使用在一般意义上的意思"extends"（类）或者"implements"（接口）。该例子中的泛型方法返回三个可比较对象的最大值。



```
public class MaximumTest
{
   // 比较三个值并返回最大值
   public static <T extends Comparable<T>> T maximum(T x, T y, T z)
   {                     
      T max = x; // 假设x是初始最大值
      if ( y.compareTo( max ) > 0 ){
         max = y; //y 更大
      }
      if ( z.compareTo( max ) > 0 ){
         max = z; // 现在 z 更大           
      }
      return max; // 返回最大对象
   }
   public static void main( String args[] )
   {
      System.out.printf( "%d, %d 和 %d 中最大的数为 %d\n\n",
                   3, 4, 5, maximum( 3, 4, 5 ) );
 
      System.out.printf( "%.1f, %.1f 和 %.1f 中最大的数为 %.1f\n\n",
                   6.6, 8.8, 7.7, maximum( 6.6, 8.8, 7.7 ) );
 
      System.out.printf( "%s, %s 和 %s 中最大的数为 %s\n","pear",
         "apple", "orange", maximum( "pear", "apple", "orange" ) );
   }
}
```



运行结果如下所示：



```
3, 4 和 5 中最大的数为 5
6.6, 8.8 和 7.7 中最大的数为 8.8
pear, apple 和 orange 中最大的数为 pear
```



### 8.3 泛型类



泛型类的声明和非泛型类的声明类似，除了在类名后面添加了类型参数声明部分。



和泛型方法一样，泛型类的类型参数声明部分也包含一个或多个类型参数，参数间用逗号隔开。一个泛型参数，也被称为一个类型变量，是用于指定一个泛型类型名称的标识符。因为他们接受一个或多个参数，这些类被称为参数化的类或参数化的类型。



**实例**：定义一个泛型类:



```
public class Box<T> {
   
  private T t;
 
  public void add(T t) {
    this.t = t;
  }
 
  public T get() {
    return t;
  }
 
  public static void main(String[] args) {
    Box<Integer> integerBox = new Box<Integer>();
    Box<String> stringBox = new Box<String>();
 
    integerBox.add(new Integer(10));
    stringBox.add(new String("hello!"));
 
    System.out.printf("整型值为 :%d\n\n", integerBox.get());
    System.out.printf("字符串为 :%s\n", stringBox.get());
  }
}
```



运行结果如下所示：



```
整型值为 :10
字符串为 :hello!
```



### 8.4 约束和局限性



**擦除**



程序中所有泛型类型都将被擦除，替换为它们的非泛型上界。



**擦除的问题**：泛型不能用于显示地引用运行时类型的操作之中，例如转型、instanceof操作和new表达式，因为所有关于参数的类型信息都丢失了 （编程思想p376）。



使用**泛型类、擦除和@Suppress Warning注解** ，就能消除Java类型系统的部分基本限制。



要想支持擦除的转换，就需要强行限制一个类或类型变量不能同时成为两个接口类型的子类，而这两个接口是同一接口的不同参数化。



### 8.5 泛型类型的继承规则



泛型的继承规则和普通类继承不同，例如类`S`和`T`,无论`S`与`T`有什么联系，但Pair和 Pair没有任何关系。



泛型类可以扩展或实现其他的泛型类。例如ArrayList 类实现 List 接口。这意味着， 一个 ArrayList可以被转换为一个 List。但是， 如前面所见， 一个 ArrayList 不是一个 ArrayList  或 List。如下图所示，为泛型列表类型中子类型间的联系







### 8.6 类型通配符



类型通配符一般是使用?代替具体的类型参数。例如 **List<?>**在逻辑上是在逻辑上是**List,List**等所有List<具体类型实参>的父类。



实例:



```
import java.util.*;
 
public class GenericTest {
     
    public static void main(String[] args) {
        List<String> name = new ArrayList<String>();
        List<Integer> age = new ArrayList<Integer>();
        List<Number> number = new ArrayList<Number>();
        
        name.add("icon");
        age.add(18);
        number.add(314);
 
        //getUperNumber(name);//1
        getUperNumber(age);//2
        getUperNumber(number);//3
       
   }
 
   public static void getData(List<?> data) {
      System.out.println("data :" + data.get(0));
   }
   
   public static void getUperNumber(List<? extends Number> data) {
          System.out.println("data :" + data.get(0));
       }
}
```



输出结果：



```
data :18
data :314
```



解析：在(//1)处会出现错误，因为getUperNumber()方法中的参数已经限定了参数泛型上限为Number，所以泛型为String是不在这个范围之内，所以会报错



采用类型通配符可以解决一些问题，例如： Pair不是 Pair 的子类型，却是Pair<? extends Employee> 的子类型，关系如图所示：







另外，类型通配符**下限**通过形如 **List<? super Manager>**来定义，表示类型只能接受Manager及其上层父类类型，如 Object 类型的实例。



**注意1：**`Pair<?>`和`Pair`看起来类似，但并不相同，`Pair<?>`的set()方法不能被调用，甚至不同用Object调用，但可以用任意Object对象调用原始Pair类的setObject方法。



**注意2：**通配符不是类型变量，因此，不能在编写代码中使用“？”作为一种类型。



**注意3：**带有超类型限定的通配符（super）可以向泛型对象写入（能调用set()方法），带有子类型限定的通配符（extend）可以从泛型对象读取（能调用get()方法）。



### 8.7 反射和泛型



利用反射可以获得泛型类的一些信息。



为了表达泛型类型声明，使用java.lang.reflect包中提供的接口Type。此接口包含下列子类型：



- Class类，描述具体类型
- TypeVariable接口，描述类型变量（如 T extends Comparable<? super T>）
- WildcardType接口，描述通配符（如？super T）
- ParameterizedType接口，描述泛型类或接口类型（如Comparable<? super T>）
- GenericArrayType接口，描述泛型数组（如 T[]）



如下图，为**Type类**的继承层次，最后4个子类型是接口，虚拟机将实例化实现这些接口的适当的类。



## 第9章 集合



### 9.1 容器

#### 9.1.1 Collection

http://www.51gjie.com/java/642.html

Collection是最基本的集合接口，一个Collection代表一组Object，即Collection的元素（Elements）。Java SDK不提供直接继承自Collection的类，Java SDK提供的类都是继承自Collection的“子接口”如List和Set。

**继承体系：**

- Collection
  - List  有序(存储顺序和取出顺序一致)，可重复
    - ArrayList ，线程不安全，底层使用数组实现，查询快，增删慢，效率高。
    - LinkedList ， 线程不安全，底层使用链表实现，查询慢，增删快，效率高。
    - Vector ， 线程安全，底层使用数组实现，查询快，增删慢，效率低。每次容量不足时，默认自增长度的一倍（如果不指定增量的话），如下源码可知
  - Set   元素唯一一个不包含重复元素的 collection。更确切地讲，set 不包含满足 e1.equals(e2) 的元素对 e1 和 e2，并且最多包含一个 null 元素。
    - HashSet 底层是由HashMap实现的，通过对象的hashCode方法与equals方法来保证插入元素的唯一性，无序(存储顺序和取出顺序不一致)，。
    - LinkedHashSet 底层数据结构由哈希表和链表组成。哈希表保证元素的唯一性，链表保证元素有序。(存储和取出是一致)
    - TreeSet 基于 TreeMap 的 NavigableSet 实现。使用元素的自然顺序对元素进行排序，或者根据创建 set 时提供的 Comparator 进行排序，具体取决于使用的构造方法。 元素唯一。

**1、Set**

- TreeSet：基于红黑树实现，支持**有序性**操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
- HashSet：基于哈希表实现，支持快速查找，但**不支持有序性**操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
- LinkedHashSet：具有 HashSet 的查找效率，且内部使用双向链表维护元素的插入顺序。

##### 2、List

- List
- ArrayList：基于动态数组实现，支持随机访问。
- Vector：和 ArrayList 类似，但它是线程安全的。
- LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。

**判断某个list中是否包含某个元素：**

方法一：

```java
    private static boolean isContainKey(String[] keys, String targetValue)
    {
        if (keys == null || keys.length == 0)
        {
            return false;
        }

        for (String str : keys)
        {
            if (StringUtils.equals(str, targetValue))
            {
                return true;
            }
        }

        return false;
    }
```

方法二（推荐）：

```java
Boolean var = arr.contains(targetValue);
```

##### 3、Queue

- LinkedList：可以用它来实现双向队列。
- PriorityQueue：基于**堆结构实现**，可以用它来实现优先队列。

#### 9.1.2  Map

![image-20191015163602058](/Users/baola/Library/Application Support/typora-user-images/image-20191015163602058.png)

- TreeMap：基于红黑树实现。
- HashMap：基于哈希表实现。
- HashTable：和 HashMap 类似，但**它是线程安全**的，这意味着同一时刻多个线程可以同时写入 HashTable 并且不会导致数据不一致。它是遗留类，不应该去使用它。现在可以使用 ConcurrentHashMap 来支持线程安全，并且 **ConcurrentHashMap 的效率会更高**，因为 ConcurrentHashMap 引入了分段锁。
- LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

##### 构造map并初始化

方法一：

```java
public class Demo{
    private static final Map<String, String> myMap;
    static
    {
        myMap = new HashMap<String, String>();
        myMap.put("a", "b");
        myMap.put("c", "d");
    }
}
```

方法二：

```java
HashMap<String, String > h = new HashMap<String, String>(){{
      put("a","b");    
}};
```



# Java编程思想

## 第6章 访问权限控制

Java 中有4种访问权限修饰符：

- private
- protected （所有子类可以访问，也提供包访问权限）
- public
- 不加访问修饰符，表示包级可见。

可以对类或类中的成员（字段以及方法）加上访问修饰符。

- 类可见表示其它类可以用这个类创建实例对象。
- 成员可见表示其它类可以用这个类的实例对象访问到该成员。

`protected`用于修饰成员，表示在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。

设计良好的模块会隐藏所有的实现细节，把它的 API 与它的实现清晰地隔离开来。模块之间只通过它们的 API 进行通信，一个模块不需要知道其他模块的内部工作情况，这个概念被称为信息隐藏或封装。因此访问权限应当尽可能地使每个类或者成员不被外界访问。

如果子类的方法**重写**了父类的方法，那么**子类中该方法的访问级别不允许低于父类的访问级别**(子类不能缩小父类方法的访问权限,可以扩大)。这是为了确保可以使用父类实例的地方都可以使用子类实例，也就是确保满足里氏替换原则。

## 第8章 多态

### 8.1 多态概念

多态指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。

在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）。

缺陷：

- 只有非**private**方法才能被覆盖
- 域和静态方法不具有多态性（构造器也不具有多态性，它实际上是static方法）

### 8.2 调用构造器的顺序



1. 在其他任何事物发生之前，将分配给对象的存储空间初始化为二进制的零
2. 调用基类构造器（递归到层次结构的根）
3. 按声明顺序调用成员的初始化方法
4. 调用导出类构造器的主体



**注意**：在构造器内部，必须保证所要使用的成员都已经构建完成，因此，需要首先调用基类构造器；而在销毁过程中，销毁顺序应和初始化顺序相反（先对其导出类进行清理，然后才是基类），因为导出类的清理可能会调用基类中的某些方法，所以需要使基类中的构件仍起作用而不应过早地销毁它们。



### 8.3 向下转型



想访问子类对象的扩展接口，可以通过向下转型的方式，如果失败，返回一个**ClassCastException**异常



## 第9章 接口



#### 9.1 抽象类

包含抽象方法的类叫做抽象类。抽象类和抽象方法都使用 abstract 关键字进行声明。如果一个类中包含抽象方法，那么这个类必须声明为抽象类。

抽象类和普通类最大的区别是，抽象类不能被实例化，需要继承抽象类才能实例化其子类。

```java
public abstract class AbstractClassExample {

    protected int x;
    private int y;

    public abstract void func1();

    public void func2() {
        System.out.println("func2");
    }
}
```



```java
public class AbstractExtendClassExample extends AbstractClassExample {
    @Override
    public void func1() {
        System.out.println("func1");
    }
}
```



```java
// AbstractClassExample ac1 = new AbstractClassExample(); 
// 'AbstractClassExample' is abstract; cannot be instantiated
AbstractClassExample ac2 = new AbstractExtendClassExample();
ac2.func1();
```



从上面例子可以看出，抽象类不能被实例化，即使是不包含任何方法的抽象类，也不能为该类创建任何实例。



#### 9.2 接口



接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。



从 Java 8 开始，接口也可以拥有默认的方法实现，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。



##### 9.2.0 接口编写示例

特性：

- 接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。
- 接口的字段默认都是 static 和 final 的。

```java
public interface InterfaceExample {

    void func1();

    default void func2(){
        System.out.println("func2");
    }
    int x = 123;
    // int y;               // Variable 'y' might not have been initialized
    public int z = 0;       // Modifier 'public' is redundant for interface fields
    // private int k = 0;   // Modifier 'private' not allowed here
    // protected int l = 0; // Modifier 'protected' not allowed here
    // private void fun3(); // Modifier 'private' not allowed here
}
```



```java
@Component
public class InterfaceImplementExample implements InterfaceExample {
    @Override
    public void func1() {
        System.out.println("func1");
    }
}
```



```java
// InterfaceExample ie1 = new InterfaceExample(); 
// 'InterfaceExample' is abstract; cannot be instantiated
InterfaceExample ie2 = new InterfaceImplementExample();
ie2.func1();
System.out.println(InterfaceExample.x);
```



##### 9.2.1 接口和抽象类的区别

- 接口的方法默认是 public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），而抽象类可以有非抽象的方法
- 接口中除了static、final变量，不能有其他变量，而抽象类中则不一定。
- 一个类可以实现多个接口，但只能实现一个抽象类。接口自己本身可以通过extends关键字扩展多个接口。
- 接口方法默认修饰符是public，抽象方法可以有public、protected和default这些修饰符（抽象方法就是为了被重写所以不能使用private关键字修饰！）。
- 从设计层面来说，抽象是对类的抽象，是一种模板设计，而接口是对行为的抽象，是一种行为的规范。

**备注**：在JDK8中，接口也可以定义静态方法，可以直接用接口名调用。如果同时实现两个接口，接口中定义了一样的默认方法，则必须重写，不然会报错。



##### 9.2.2 使用选择

使用接口：

- 需要让不相关的类都实现一个方法，例如不相关的类都可以实现 Compareable 接口中的 compareTo() 方法；
- 需要使用多重继承。

使用抽象类：

- 需要在几个相关的类中共享代码。
- 需要能控制继承来的成员的访问权限，而不是都为 public。
- 需要继承非静态和非常量字段。



**注意**：组合接口时避免使用相同的方法名



# Java小知识

### 

### java判断字符串是否是网址

方法：正则表达式匹配

```java
    public static boolean isHttpUrl(String urls) {
        boolean isurl = false;
        String regex = "(((https|http)?://)?([a-z0-9]+[.])|(www.))"
                + "\\w+[.|\\/]([a-z0-9]{0,})?[[.]([a-z0-9]{0,})]+((/[\\S&&[^,;\u4E00-\u9FA5]]+)+)?([.][a-z0-9]{0,}+|/?)";//设置正则表达式

        Pattern pat = Pattern.compile(regex.trim());//比对
        Matcher mat = pat.matcher(urls.trim());
        isurl = mat.matches();//判断是否匹配
        if (isurl) {
            isurl = true;
        }
        return isurl;
    }
```

### java之Date、Date格式化

```java
import java.text.SimpleDateFormat;
import java.util.Date;

SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyyMMdd-HHmmss-SSS");
String dateStr = simpleDateFormat.format(new Date());
```

### 数据转换

java.math.BigDecimal cannot be cast to java.lang.Double

**解决办法：**可以先toString()然后Double.parse



### java 判断数据类型和方法

1。我从SOLR查询中获取一个数据一，已知数据类型，是string或者int 或者其他
2。我有一个方法（set方法），只有一个参数，但是我不知道参数的数据类型，可能是string 或者int 或者其他
3。使用反射
4。我要判断这两个参数类型 是否相同，或者得到他们具体的类型是什么，请问如何做。
**最佳答案：**
1.<font color =dark>如果你得到是一个Object对象，可以用`if（obj instanceof String）`</font>来判断是否是String对象，int是基本类型不可以这么判断，只能用它的包装类Integer，同样用instanceof 
2.如果set方法只能接受一个参数，而且必须有int的话，可以写多个set方法，如set(String),set(int)，编写不同的处理逻辑
3.instanceof 也是反射的一种方式
4.<font color =dark>如果有2个Object的参数，可以用`if(obj1.getClass()==obj2.getClass())`</font>来判断类型是否相同，如果要得到类型名，可以用obj.getClass().getName()来获得对象的类名
