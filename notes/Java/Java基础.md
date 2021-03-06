# 前言
本文主要包含 Java 核心基础知识，主要根据以下部分进行节选，选择了个人认为在面试中最为核心的部分。
- 《Java程序员面试笔试宝典》何昊，薛鹏，叶向阳 著
- [《阿里面经OneNote》](https://blog.csdn.net/sinat_22797429/article/details/76293284)
- [【CyC2018/CS-Notes】](CyC2018/CS-Notes)

主要内容：基本概念、面向对象、关键字、基本数据类型与运算、字符串与数组、异常处理、Object 通用方法

---

# 基础
- [关键字](Java关键字.md)
- [字节和字符](#字节和字符)
- [修饰符](#修饰符)
- [代码块](#代码块)	
- [匿名对象](#匿名对象)
- [Java初始化顺序](#Java初始化顺序)
- [Java和C++区别](#Java-c++)
- [深拷贝与浅拷贝](#深拷贝与浅拷贝)
- [字符串常量池](#字符串常量池)
- [解释型语言与编译型语言](#解释型语言与编译型语言)
- [finalize()](#finalize)
- [二、基本数据类型与运算](#基本数据类型与运算)
    - [1. Java的基本数据类型和引用类型，自动装箱和拆箱](#Java的基本数据类型和引用类型，自动装箱和拆箱) 
    - [2. ValueOf缓存池](#ValueOf缓存池)
    - [3. i++ 和 ++i 有什么区别](#i++和++i有什么区别)
    - [4. 位运算符](#位运算符)
    - [5. 原码、补码、反码是什么](#原码、补码、反码是什么)
    - [6. 不用额外变量交换两个整数的值](#不用额外变量交换两个整数的值)
    - [7. 不使用运算符进行a+b操作](#不使用运算符进行a+b操作)
    - <a href="#与或非">8、&和&& 、|和||的区别</a>
- [三、字符串与数组](#字符串与数组) 
    - [1. String、StringBuffer和StringBuilder，以及对String不变性的理](#String)
    - [2. String有重写Object的hashcode和toString吗？如果重写equals不重写hashcode会出现什么问题？](#hashcode-toString-equals)
    - [3. 如果你定义一个类，包括学号，姓名，分数，如何把这个对象作为key？要重写equals和hashcode吗](#hashcode-equals)
    - [4. 字面量](#字面量)
- [四、异常处理](#异常处理)
- [1. 常见异常分为那两种(Exception，Error)，常见异常的基类以及常见的异常](#Exception-Error)
---

## 字节和字符
> 理解编码的关键，是要把 **字符** 和 **字节** 的概念理解准确。这两个概念容易混淆，我们在此做一下区分：

| 类型 | 描述 | 举例 |
| --- | --- | --- |
| 字符 | 人们使用的记号，抽象意义上的一个符号。 | '1', '中', 'a', '$', '￥', …… |
| 字节 | 计算机中存储数据的单元，一个 8 位的二进制数，是一个很具体的存储空间。 | 0x01, 0x45, 0xFA, ……          |
| ANSI 字符串 | 在内存中，如果"字符"是以 **ANSI编码** 形式存在的，一个字符可能使用一个字节或多个字节来表示，那么我们称这种字符串为 **ANSI字符串** 或 **多字节字符串**。 | "中文123" （占7字节） |
| UNICODE 字符串 | 在内存中，如果"字符"是以在 **UNICODE** 中的序号存在的，那么我们称这种字符串为 **UNICODE字符串** 或 **宽字节字符串**。 | L"中文123" （占10字节）       |

> ### 字节与字符区别
> 它们完全不是一个位面的概念，所以两者之间没有"区别"这个说法。不同编码里，字符和字节的对应关系不同：

| 类型 | 概念描述 |
| --- | --- |
| ASCII | 一个英文字母（不分大小写）占一个字节的空间，一个中文汉字占两个字节的空间。一个二进制数字序列，在计算机中作为一个数字单元，一般为 8位二进制数，换算为十进制。最小值 0，最大值 255。 |
| UTF-8 | 一个英文字符等于一个字节，一个中文（含繁体）等于三个字节     |
| Unicode | 一个英文等于两个字节，一个中文（含繁体）等于两个字节。符号：英文标点占一个字节，中文标点占两个字节。举例：英文句号"."占 1 个字节的大小，中文句号"。"占 2 个字节的大小。 |
| UTF-16 | 一个英文字母字符或一个汉字字符存储都需要 2 个字节（Unicode扩展区的一些汉字存储需要4个字节） |
| UTF-32 | 世界上任何字符的存储都需要 4 个字节                          |

参考资料：
- [字符，字节和编码](http://www.regexlab.com/zh/encoding.htm)
---

## 修饰符
> Java 面向对象的基本思想之一是封装细节并且公开接口。Java 语言采用访问控制修饰符来控制类及类的方法和变量的访问权限，从而向使用者暴露接口，但隐藏实现细节。

访问控制分为四种级别：
| 修饰符    | 当前类 | 同 包 | 子 类 | 其他包 |
| --------- | ------ | ----- | ----- | ------ |
| public    | √      | √     | √     | √      |
| protected | √      | √     | √     | ×      |
| default   | √      | √     | ×     | ×      |
| private   | √      | ×     | ×     | ×      |

- 类的成员不写访问修饰时默认为 default。默认对于同一个包中的其他类相当于公开（public），对于不是同一个包中的其他类相当于私有（private）。
- 受保护（protected）对子类相当于公开，对不是同一包中的没有父子关系的类相当于私有。
- Java 中，外部类的修饰符只能是 public 或默认，类的成员（包括内部类）的修饰符可以是以上四种。 
---

## 代码块
> ### 局部代码块
> 定义在方法中

- 作用：限定变量的生命周期。

> ### 构造代码块
> 定义在类中

- 作用：创建对象的时候，构造代码块会自动在构造方法之前执行。如果有一些操作在创建每个对象之前都需要执行，把这些代码放在构造代码块中。

> ### 静态代码块
> 定义在类中

- 作用：随着类的加载而执行，做注册驱动的需求时使用。

> ### 代码块面试题
> 加载顺序：

```
静态块(父) -> 静态块(子) -> 
定义初始化块(父) -> 构造函数(父) -> 
定义初始化块(子) -> 构造函数(子)
```
```
public class BlockTest {
	static {
		System.out.println("BlockTest静态代码块执行");
	}
	
	{
		System.out.println("BlockTest构造代码块执行");
	}

	public BlockTest(){
		System.out.println("BlockTest无参构造执行了");
	}
	
	public static void main(String[] args) {
		System.out.println("BlockTest的主函数执行了");
		Coder c = new Coder();
		Coder c2 = new Coder();
	}
}

class Coder {
	
	static {
		System.out.println("Coder静态代码块执行");
	}
	
	{
		System.out.println("Coder构造代码块执行");
	}
	
	public Coder() {
		System.out.println("Coder无参空构造执行");
	}	
	
}
```
---

## 匿名对象
> 匿名对象就是没有名字的对象：p.method(new PhoneX());

> ### 匿名对象使用场景

- 1. 当方法只调用一次的时候可以使用匿名对象。
- 2. 可以当作参数进行传递，但是无法在传参之前做其他的事情。
```
new Student().age = 18; 
new Student().study();
```
注意：匿名对象可以调用成员变量并赋值，但是赋值并没有意义。  

---

<h2 id="Java初始化顺序">Java初始化顺序</h2>
> 在 Java 语言中，当实例化对象时，对象所在类的所有成员变量首先要进行初始化，只有当所有类成员完成初始化后，才会调用对象所在类的构造函数创建对象。

**初始化一般遵循3个原则**
- 静态对象（变量）优先于非静态对象（变量）初始化，静态对象（变量）只初始化一次，而非静态对象（变量）可能会初始化多次；
- 父类优先于子类进行初始化；
- 按照成员变量的定义顺序进行初始化。 即使变量定义散布于方法定义之中，它们依然在任何方法（包括构造函数）被调用之前先初始化；

**加载顺序**
- 父类（静态变量、静态语句块）
- 子类（静态变量、静态语句块）
- 父类（实例变量、普通语句块）
- 父类（构造函数）
- 子类（实例变量、普通语句块）
- 子类（构造函数）

**实例** 
```java
class Base {
    // 1.父类静态代码块
    static {
        System.out.println("Base static block!");
    }
    // 3.父类非静态代码块
    {
        System.out.println("Base block");
    }
    // 4.父类构造器
    public Base() {
        System.out.println("Base constructor!");
    }
}

public class Derived extends Base {
    // 2.子类静态代码块
    static{
        System.out.println("Derived static block!");
    }
    // 5.子类非静态代码块
    {
        System.out.println("Derived block!");
    }
    // 6.子类构造器
    public Derived() {
        System.out.println("Derived constructor!");
    }
    public static void main(String[] args) {
        new Derived();
    }
}
```
结果是：
```
Base static block!
Derived static block!
Base block
Base constructor!
Derived block!
Derived constructor!
```
---

<h2 id="Java-c++">Java和C++区别</h2>

- Java 是**纯粹的面向对象语言**，所有的对象都继承自 java.lang.Object，**C++ 为了兼容 C 即支持面向对象也支持面向过程**。
- Java 通过虚拟机从而实现**跨平台特性**，但是 C++ 依赖于**特定的平台**。
- Java 没有指针，它的引用可以理解为安全指针，而 C++ 具有和 C 一样的指针。
- Java 支持**自动垃圾回收**，而 C++ 需要**手动回收**。（C++11 中引入智能指针，使用引用计数法垃圾回收）
- **Java 不支持多重继承**，只能通过实现多个接口来达到相同目的，而 **C++ 支持多重继承**。
- Java 不支持操作符重载，虽然可以对两个 String 对象支持加法运算，但是这是语言内置支持的操作，不属于操作符重载，而 C++ 可以。
- Java 内置了线程的支持，而 C++ 需要依靠第三方库。
- Java 的 **goto 是保留字**，但是不可用，C++ 可以使用 goto。
- Java **不支持条件编译**，C++ 通过 #ifdef #ifndef 等预处理命令从而实现**条件编译**。

参考资料：
- [C++工程实践(8)](http://www.cnblogs.com/Solstice/archive/2011/08/16/2141515.html)
- [c++11改进我们的程序之垃圾回收](https://www.cnblogs.com/qicosmos/p/3282779.html)
---

## 深拷贝与浅拷贝

重点 | 描述
---|---
浅拷贝 | 被复制对象的所有变量都含有与原来的对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。换言之，浅拷贝仅仅复制所拷贝的对象，而不复制它所引用的对象。
<div align="center"> <img src="assets/shadow_copy2.jpg" width="550"/></div>

重点 | 描述
---|---
深拷贝 | 对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。
<div align="center"> <img src="assets/deep_copy2.jpg" width="550"/></div>

参考资料：
- [细说 Java 的深拷贝和浅拷贝](https://segmentfault.com/a/1190000010648514)
- [object clone 的用法、原理和用途](https://juejin.im/post/59bfc707f265da0646188bca)
---

## 字符串常量池
> Java 中字符串对象创建有两种形式，一种为字面量形式，如 `String str = "abc";`，另一种就是使用 new 这种标准的构造对象的方法，如 `String str = new String("abc");`，这两种方式我们在代码编写时都经常使用，尤其是字面量的方式。然而 **这两种实现其实存在着一些性能和内存占用的差别**。这一切都是源于 JVM 为了减少字符串对象的重复创建，其维护了一个特殊的内存，这段内存被成为 **字符串常量池** 或 **字符串字面量池**。

- 工作原理

当代码中出现字面量形式创建字符串对象时，JVM首先会对这个字面量进行检查，如果字符串常量池中存在相同内容的字符串对象的引用，则将这个引用返回，否则新的字符串对象被创建，然后将这个引用放入字符串常量池，并返回该引用。

```java
public class Test {
    public static void main(String[] args) {

        String s1 = "abc";
        String s2 = "abc";

        // 以上两个局部变量都存在了常量池中
        System.out.println(s1 == s2); // true


        // new出来的对象不会放到常量池中,内存地址是不同的
        String s3 = new String();
        String s4 = new String();

        /**
     	* 字符串的比较不可以使用双等号,这样会比较内存地址
     	* 字符串比较应当用equals,可见String重写了equals
     	*/
        System.out.println(s3 == s4); // false
        System.out.println(s3.equals(s4)); // true
    }
}
```
---

## 解释型语言与编译型语言
> 我们使用工具编写的字母加符号的代码，是我们能看懂的高级语言，计算机无法直接理解，计算机需要先对我们编写的代码翻译成计算机语言，才能执行我们编写的程序。

将高级语言翻译成计算机语言有编译，解释两种方式。两种方式只是翻译的时间不同。

**1. 编译型语言**

编译型语言写得程序在执行之前，需要借助一个程序，将高级语言编写的程序翻译成计算机能懂的机器语言，然后，这个机器语言就能直接执行了，也就是我们常见的（exe文件）。

**2. 解释型语言**

解释型语言的程序不需要编译，节省了一道工序，不过解释型的语言在运行的时候需要翻译，每个语句都是执行的时候才翻译，对比编译型语言，效率比较低。通俗来讲，就是借助一个程序，且这个程序能试图理解编写的代码，然后按照编写的代码中的要求执行。

**3. 脚本语言**

脚本语言也是一种解释型语言，又被称为扩建的语言，或者动态语言不需要编译，可以直接使用，由解释器来负责解释。
脚本语言一般都是以文本形式存在，类似于一种命令。

**4. 通俗理解编译型语言和解释型语言**

同行讨论编译型语言和解释型语言的时候，这么说过，编译型语言相当于做一桌子菜再吃，解释型语言就是吃火锅。解释型的语言执行效率低，类似火锅需要一边煮一边吃。

---

### finalize()
> finalize() 是 Object 中的方法，当垃圾回收器将要回收对象所占内存之前被调用，即当一个对象被虚拟机宣告死亡时会先调用它 finalize() 方法，让此对象处理它生前的最后事情（这个对象可以趁这个时机挣脱死亡的命运）。要明白这个问题，先看一下虚拟机是如何判断一个对象该死的。

> 可以覆盖此方法来实现对其他资源的回收，例如关闭文件。
　　
> ### 判定死亡
> Java 采用可达性分析算法来判定一个对象是否死期已到。Java中以一系列 "GC Roots" 对象作为起点，如果一个对象的引用链可以最终追溯到 "GC Roots" 对象，那就天下太平。

> 否则如果只是A对象引用B，B对象又引用A，A B引用链均未能达到 "GC Roots" 的话，那它俩将会被虚拟机宣判符合死亡条件，具有被垃圾回收器回收的资格。*

> ### 最后的救赎
> 上面提到了判断死亡的依据，但被判断死亡后，还有生还的机会。

**如何自我救赎：**
- 1. 对象覆写了 finalize() 方法（这样在被判死后才会调用此方法，才有机会做最后的救赎）；
- 2. 在 finalize() 方法中重新引用到 "GC  Roots" 链上（如把当前对象的引用 this 赋值给某对象的类变量/成员变量，重新建立可达的引用）.

**需要注意：**

finalize() 只会在对象内存回收前被调用一次 (The finalize method is never invoked more than once by a Java virtual machine for any given object. )

finalize() 的调用具有不确定性，只保证方法会调用，但不保证方法里的任务会被执行完（比如一个对象手脚不够利索，磨磨叽叽，还在自救的过程中，被杀死回收了）。

> ### finalize()的作用 

虽然以上以对象救赎举例，但 finalize() 的作用往往被认为是用来做最后的资源回收。
　　基于在自我救赎中的表现来看，此方法有很大的不确定性（不保证方法中的任务执行完）而且运行代价较高。所以用来回收资源也不会有什么好的表现。

综上：finalize() 方法并没有什么鸟用。

至于为什么会存在一个鸡肋的方法：书中说 “它不是 C/C++ 中的析构函数，而是 Java 刚诞生时为了使 C/C++ 程序员更容易接受它所做出的一个妥协”。

参考资料：
- [关于finalize()](https://blog.csdn.net/L_wwbs/article/details/70770447?locationNum=1&fps=1)
---

<h2 id="基本数据类型与运算">二、基本数据类型与运算</h2>
> <h3 id="Java的基本数据类型和引用类型，自动装箱和拆箱">1. Java的基本数据类型和引用类型，自动装箱和拆箱</h3>
> 4 类 8 种基本数据类型。4 整数型，2 浮点型，1 布尔型，1 字符型

| 分类 | 类型 | 存储 | 取值范围 | 默认值 | 包装类 |
| --- | --- | --- | --- | --- | --- |
| **整数型** | byte | 8 | 最大存储数据量是 255，最小 -2<sup>7</sup>，最大 2<sup>7</sup>-1，<br />[-128~127] | (byte) 0        | Byte |
_ | short | 16 | 最大数据存储量是 65536，[-2<sup>15</sup>,2<sup>15</sup>-1]，<br />[-32768,32767]，±3万 | (short) 0 | Short |
_ | int | 32 | 最大数据存储容量是 2<sup>31</sup>-1，<br />[-2<sup>31</sup>,2<sup>31</sup>-1]，±21亿，[ -2147483648, 2147483647] | 0  | Integer |
_ | long | 64 | 最大数据存储容量是 2<sup>64</sup>-1，<br />[-2<sup>63</sup>,2<sup>63</sup>-1]， ±922亿亿（±（922+16个零）） | 0L              | Long |
| **浮点型** | float | 32 | 数据范围在 3.4e-45~1.4e38，直接赋值时必须在数字后加上 f 或 F | 0.0f            | Float |
_ | double | 64 | 数据范围在 4.9e-324~1.8e308，赋值时可以加 d 或 D 也可以不加  | 0.0d            | Double    |
| **布尔型** | boolean | 1    | true / flase                                                 | false           | Boolean   |
| **字符型** | char | 16   | 存储 Unicode 码，用单引号赋值                                | '\u0000' (null) | Character |

- 引用数据类型
  - 类（class）、接口（interface）、数组
- 自动装箱和拆箱
  - 基本数据类型和它对应的封装类型之间可以相互转换。自动拆装箱是 `jdk5.0` 提供的新特特性，它可以自动实现类型的转换
  - **装箱**：从**基本数据类型**到**封装类型**叫做装箱
  - **拆箱**：从**封装类型**到**基本数据类型**叫拆箱

```java
// jdk 1.5
public class TestDemo {
    public static void main(String[] args) {
        Integer m =10;
        int i = m;
    }
}
```
上面的代码在 jdk1.4 以后的版本都不会报错，它实现了自动拆装箱的功能，如果是 jdk1.4，就得这样写了
```java
// jdk 1.4
public class TestDemo {
    public static void main(String[] args) {
        Integer b = new Integer(210);
        int c = b.intValue();
    }
}
```
---

> <h3 id="ValueOf缓存池">2. ValueOf缓存池</h3>
> new Integer(123)  与 Integer.valueOf(123)  的区别在于，new Integer(123)  每次都会新建一个对象，而 Integer.valueOf(123)  可能会使用缓存对象，因此多次使用 Integer.valueOf(123)  会取得同一个对象的引用。

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```
编译器会在自动装箱过程调用 valueOf() 方法，因此多个 Integer 实例使用自动装箱来创建并且值相同，那么就会引用相同的对象。
```java
Integer m = 123;
Integer n = 123;
System.out.println(m == n); // true
```
valueOf() 方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接使用缓存池的内容。
```java
// valueOf 源码实现
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```
在 Java 8 中，Integer 缓存池的大小默认为 -128\~127。
```java
static final int low = -128;
static final int high;
static final Integer cache[];

static {
    // high value may be configured by property
    int h = 127;
    String integerCacheHighPropValue =
        sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
    if (integerCacheHighPropValue != null) {
        try {
            int i = parseInt(integerCacheHighPropValue);
            i = Math.max(i, 127);
            // Maximum array size is Integer.MAX_VALUE
            h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
        } catch( NumberFormatException nfe) {
            // If the property cannot be parsed into an int, ignore it.
        }
    }
    high = h;

    cache = new Integer[(high - low) + 1];
    int j = low;
    for(int k = 0; k < cache.length; k++)
        cache[k] = new Integer(j++);

    // range [-128, 127] must be interned (JLS7 5.1.7)
    assert IntegerCache.high >= 127;
}
```
Java 还将一些其它基本类型的值放在缓冲池中，包含以下这些：
- boolean values true and false
- all byte values
- short values between -128 and 127
- int values between -128 and 127
- char in the range \u0000 to \u007F
- 
因此在使用这些基本类型对应的包装类型时，就可以直接使用缓冲池中的对象。

参考资料：
- [StackOverflow : Differences between new Integer(123), Integer.valueOf(123) and just 123](https://stackoverflow.com/questions/9030817/differences-between-new-integer123-integer-valueof123-and-just-123)
---

> <h3 id="i++和++i有什么区别">3. i++ 和 ++i 有什么区别</h3>
- i++

```
i++ 是在程序执行完毕后进行自增，而 ++i 是在程序开始执行前进行自增。
```
i++ 的操作分三步
```
1. 栈中取出 i
2. i 自增 1
3. 将 i 存到栈
```
三个阶段：内存到寄存器，寄存器自增，写回内存（这三个阶段中间都可以被中断分离开）。所以 i++ 不是原子操作，上面的三个步骤中任何一个步骤同时操作，都可能导致 i 的值不正确自增

- ++i
```
在多核的机器上，CPU 在读取内存 i 时也会可能发生同时读取到同一值，这就导致两次自增，实际只增加了一次。
```

- i++ 和 ++i 都不是原子操作

**原子性**：指的是一个操作是不可中断的。即使是在多个线程一起执行的时候，一个操作一旦开始，就不会被其他线程打断。

**JMM三大特性**：原子性，可见性，有序性。
---

> <h3 id="位运算符">4. 位运算符</h3>
> Java 定义了位运算符，应用于整数类型 (int)，长整型 (long)，短整型 (short)，字符型 (char)，和字节型 (byte)等类型。

下表列出了位运算符的基本运算，假设整数变量A的值为60和变量B的值为13

```
A（60）：0011 1100

B（13）：0000 1101
```
| 操作符 | 名称 | 描述 | 例子 |
| --- | --- | --- | --- |
| ＆ | 与 | 如果相对应位都是 1，则结果为 1，否则为 0 | （A＆B）得到 12，即 0000 1100 |
| \| | 或 | 如果相对应位都是 0，则结果为 0，否则为 1                     | （A\|B）得到 61，即 0011 1101 |
| ^ | 异或 | 如果相对应位值相同，则结果为 0，否则为 1                     | （A^B）得到49，即 0011 0001   |
| 〜 | 非 | 按位取反运算符翻转操作数的每一位，即 0 变成 1，1 变成 0      | （〜A）得到-61，即1100 0011   |
| << | 左移       | （左移一位乘2）按位左移运算符。左操作数按位左移右操作数指定的位数。左移 n 位表示原来的值乘 2<sup>n</sup> | A << 2得到240，即 1111 0000   |
| >> |            | （右移一位除2）有符号右移，按位右移运算符。左操作数按位右移右操作数指定的位数 | A >> 2得到15即 1111           |
| >>> | 无符号右移 | 无符号右移，按位右移补零操作符。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充 | A>>>2得到15即0000 1111        |
---

> <h3 id="原码、补码、反码是什么">5. 原码、补码、反码是什么</h3>
> #### 机器数
> 一个数在计算机中的二进制表示形式，叫做这个数的机器数。机器数是带符号的，在计算机用一个数的最高位存放符号，正数为 0，负数为 1。

比如：十进制中的数 +3 ，计算机字长为 8 位，转换成二进制就是 00000011。如果是 -3 ，就是 10000011 。那么，这里的 00000011 和 10000011 就是机器数。

> #### 真值
> 因为第一位是符号位，所以机器数的形式值就不等于真正的数值。例如上面的有符号数 10000011，其最高位 1 代表负，其真正数值是 -3 而不是形式值 131（10000011 转换成十进制等于 131）。所以，为区别起见，**将带符号位的机器数对应的真正数值** 称为机器数的真值。

例：0000 0001 的真值 = +000 0001 = +1，1000 0001 的真值 = –000 0001 = –1

> #### 原码
> 原码就是符号位加上真值的绝对值，即用第一位表示符号，其余位表示值。比如如果是 8 位二进制:

```
[+1]<sub>原</sub> = 0000 0001

[-1]<sub>原</sub> = 1000 0001
```
第一位是符号位。因为第一位是符号位，所以 8 位二进制数的取值范围就是：[1111 1111 , 0111 1111]，即：[-127 , 127]

**原码是人脑最容易理解和计算的表示方式**

> #### 反码

反码的表示方法是：
- **正数**的反码是其本身；
- **负数**的反码是在其原码的基础上，**符号位不变，其余各个位取反**。

> [+1] = [00000001]<sub>原</sub> = [00000001]<sub>反</sub>

> [-1] = [10000001]<sub>原</sub>= [11111110]<sub>反</sub>

可见如果一个反码表示的是负数, 人脑无法直观的看出来它的数值. 通常要将其转换成原码再计算。

> #### 补码

补码的表示方法是：
- **正数**的补码就是其本身；
- **负数**的补码是在其原码的基础上，符号位不变，其余各位取反, 最后+1。(**反码的基础上 +1**)

> [+1] = [0000 0001]<sub>原</sub> = [0000 0001]<sub>反</sub> = [0000 0001]<sub>补</sub>

> [-1]  = [1000 0001]<sub>原</sub> = [1111 1110]<sub>反</sub> = [1111 1111]<sub>补</sub>

对于负数，补码表示方式也是人脑无法直观看出其数值的。 通常也需要转换成原码在计算其数值。

参考资料：
- [原码, 反码, 补码 详解 - ziqiu.zhang - 博客园](http://www.cnblogs.com/zhangziqiu/archive/2011/03/30/ComputerCode.html)
---

> <h3 id="不用额外变量交换两个整数的值">6. 不用额外变量交换两个整数的值</h3>

如果给定整数 a 和 b，用以下三行代码即可交换 a 和b 的值
```java
a = a ^ b;
b = a ^ b;
a = a ^ b;
```
- 假设 a 异或 b 的结果记为 c，**c 就是 a 整数位信息和 b 整数位信息的所有不同信息**。
  - 比如：a = 4 = 100，b = 3 = 011，a^b = c = 111
- a 异或 c 的结果就是 b，比如：a = 4 = 100，c = 111，a^c = 011 = 3 = b
- b 异或c 的结果就是 a，比如：b = 3 = 011，c = 111，b^c = 100 = 4 = a

说明：位运算的题目基本上都带有靠经验积累才会做的特征，也就是准备阶段需要做足够多的题，面试时才会有良好的感觉。

---

> <h3 id="不使用运算符进行a+b操作">7. 不使用运算符进行a+b操作</h3>

- a^b;  得到不含进位之和
- (a & b)<<1;  进位
- 只要进位不为零，则迭代；否则返回

```java
#include <stdio.h>

int add(int a, int b)
{
    int c = a & b;
    int r = a ^ b;
    if(c == 0){
        return r;
    }
    else{
        return add(r, c << 1);
    }
}

int main(int argn, char *argv[])
{
    printf("sum = %d\n", add(-10000, 56789));
    return 0;
}
```
---

> <h3 id="与或非">8. &和&& 、|和||的区别</h3>
> && 和 & 都是表示与，区别是 && 只要第一个条件不满足，后面条件就不再判断。而 & 要对所有的条件都进行判断。

```java
// 例如：
public static void main(String[] args) {  
    if((23!=23) && (100/0==0)){  
        System.out.println("运算没有问题。");  
    }else{  
        System.out.println("没有报错");  
    }  
}  
// 输出的是“没有报错”。而将 && 改为 & 就会如下错误：
// Exception in thread "main" java.lang.ArithmeticException: / by zero
```

- 原因：
  - &&时判断第一个条件为 false，后面的 100/0==0 这个条件就没有进行判断。

  - & 时要对所有的条件进行判断，所以会对后面的条件进行判断，所以会报错。


  （2）|| 和 | 都是表示 “或”，区别是 || 只要满足第一个条件，后面的条件就不再判断，而 | 要对所有的条件进行判断。 看下面的程序： 

```java
public static void main(String[] args) {  
    if((23==23)||(100/0==0)){  
    	System.out.println("运算没有问题。");  
    }else{  
    	System.out.println("没有报错");  
    }  
}
// 此时输出“运算没有问题”。若将||改为|则会报错。
```
- 原因
    - || 判断第一个条件为 true，后面的条件就没有进行判断就执行了括号中的代码
    - 而 | 要对所有的条件进行判断，所以会报错
---

<h2 id="字符串与数组">三、字符串与数组</h2>
> <h3 id="String">1. String、StringBuffer和StringBuilder，以及对String不变性的理解</h3> 

- String、StringBuffer、StringBuilder 
  - 都是 final 类，都不允许被继承
  - String 长度是不可变的，StringBuffer、StringBuilder 长度是可变的
  - StringBuffer 是线程安全的，StringBuilder 不是线程安全的，但它们两个中的所有方法都是相同的，StringBuffer 在 StringBuilder 的方法之上添加了 synchronized 修饰，保证线程安全
  - StringBuilder 比 StringBuffer 拥有更好的性能
  - 如果一个 String 类型的字符串，在编译时就可以确定是一个字符串常量，则编译完成之后，字符串会自动拼接成一个常量。此时 String 的速度比 StringBuffer 和 StringBuilder 的性能好的多



- String 不变性的理解 
  - String 类是被 final 进行修饰的，不能被继承 
  - 在用 + 号链接字符串的时候会创建新的字符串
  - String s = new String("Hello world"); 可能创建两个对象也可能创建一个对象。如果静态区中有 “Hello world” 字符串常量对象的话，则仅仅在堆中创建一个对象。如果静态区中没有 “Hello world” 对象，则堆上和静态区中都需要创建对象。 
  - 在 Java 中, 通过使用 "+" 符号来串联字符串的时候,，实际上底层会转成通过 StringBuilder 实例的 append() 方法来实现。
---

> <h3 id="hashcode-toString-equals">2. String有重写Object的hashcode和toString吗？如果重写equals不重写hashcode会出现什么问题？</h3>

- String 有重写 Object 的 hashcode 和 toString吗？ 

  - String 重写了 Object 类的 hashcode 和 toString 方法。 

- 当 equals 方法被重写时，通常有必要重写 hashCode 方法，以维护 hashCode 方法的常规协定，该协定声明相对等的两个对象必须有相同的 hashCode 

  - object1.euqal(object2) 时为 true， object1.hashCode() ==  object2.hashCode() 为 true 
  - object1.hashCode() ==  object2.hashCode() 为 false 时，object1.euqal(object2) 必定为 false 
  - object1.hashCode() ==  object2.hashCode() 为 true时，但 object1.euqal(object2) 不一定定为 true 

- 重写 equals 不重写 hashcode 会出现什么问题 

  - 在存储散列集合时(如 Set 类)，如果原对象.equals(新对象)，但没有对 hashCode 重写，即两个对象拥有不同的 hashCode，则在集合中将会存储两个值相同的对象，从而导致混淆。因此在重写 equals 方法时，必须重写 hashCode 方法。 

> <h3 id="hashcode-equals">3. 如果你定义一个类，包括学号，姓名，分数，如何把这个对象作为key？要重写equals和hashcode吗</h3>

- 需要重写 equals 方法和 hashcode，必须保证对象的属性改变时，其 hashcode 不能改变。 
---

> <h3 id="字面量">4. 字面量</h3>

在编程语言中，字面量（literal）指的是在源代码中直接表示的一个固定的值。 

八进制是用在整数字面量之前添加 “0” 来表示的。 

十六进制用在整数字面量之前添加 “0x” 或者 “0X” 来表示的

 Java 7 中新增了二进制：用在整数字面量之前添加 “0b” 或者 “0B” 来表示的。  

**在数值字面量中使用下划线**

在 Java7 中，数值字面量，不管是整数还是浮点数都允许在数字之间插入任意多个下划线。并且不会对数值产生影响，目的是方便阅读，规则只能在数字之间使用。

```java
public class BinaryIntegralLiteral {
    public static void main(String[] args) {
        System.out.println(0b010101);
        System.out.println(0B010101);
        System.out.println(0x15A);
        System.out.println(0X15A);
        System.out.println(077);
        System.out.println(5_000);
        /**
         * 输出结果
         * 21 
         * 21 
         * 346 
         * 346 
         * 63
         * 5000
         */
    }
}
```
---

<h2 id="异常处理">四、异常处理</h2>
>   <h3 id="Exception-Error">1. 常见异常分为那两种(Exception，Error)，常见异常的基类以及常见的异常</h3>

- Throwable 是 Java 语言中所有错误和异常的超类（万物即可抛）。它有两个子类：Error、Exception。 
- 异常种类 
  - **Error**：Error 为错误，是程序无法处理的，如 OutOfMemoryError、ThreadDeath 等，出现这种情况你唯一能做的就是听之任之，交由 JVM 来处理，不过 JVM 在大多数情况下会选择终止线程。 
  - **Exception**：Exception 是程序可以处理的异常。它又分为两种 CheckedException（受捡异常），一种是 UncheckedException（不受检异常）。 
    - **受检异常**（CheckException）：发生在编译阶段，必须要使用 try…catch（或者throws）否则编译不通过。 
    - **非受检异常** （UncheckedException）：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序奔溃并且无法恢复。 （发生在运行期，具有不确定性，主要是由于程序的逻辑问题所引起的，难以排查，我们一般都需要纵观全局才能够发现这类的异常错误，所以在程序设计中我们需要认真考虑，好好写代码，尽量处理异常，即使产生了异常，也能尽量保证程序朝着有利方向发展。 ）
- 常见异常的基类（Exception）

  - IOException 
  - RuntimeException 
- 常见的异常

<div align="center"><img src="assets/exception_and_error.png" width="650"/></div>
---

# 更新日志

- 2018/7/11 v1.0 第一版
- 2018/7/31 v2.0 基础版
- 2018/8/30-31 v3.0 初版
