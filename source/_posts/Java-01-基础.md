---
title: Java基础
date: 2020-10-20 20:24:36
categories:
- Java
tags:
- 基础
---

#### Day 001

> <!-- Part 001 -->
>
> ※ `Java`是一种介于编译型与解释型之间的语言，在将其源码编译成一种类似于抽象CPU指令的字节码后，通过虚拟机加载并执行，即可实现“一次编译，到处运行”的效果。**注**：SUN公司制定了一系列的虚拟机规范，但是从实践的角度来看，`JVM`的兼容性做得最好，低版本的`Java`字节码完全可以在高版本的`JVM`上运行。
>
> ※ 简单来说，`JRE(Java Runtime Environment)`是`Java`的运行环境，包含了`JVM`标准实现及`Java`核心类库；`JDK(Java Development Kit)`是`Java`的开发工具包，既包含了`Java`的运行环境，又包含了`Java`的编译环境。它们之间的关系如下：

```shell
┌─    ┌──────────────────────────────────┐
│     │     Compiler, debugger, etc.     │
│     └──────────────────────────────────┘
JDK┌─ ┌──────────────────────────────────┐
│  │  │                                  │
│ JRE │      JVM + Runtime Library       │
│  │  │                                  │
└─ └─ └──────────────────────────────────┘
┌───────┐┌───────┐┌───────┐┌───────┐
│Windows││ Linux ││ macOS ││others │
└───────┘└───────┘└───────┘└───────┘
```

> <!-- Part 002 -->
>
> ※ 在`JAVA_HOME`的`bin`目录下存在很多可执行文件，其中：`java`用于解析字节码文件使其得到运行；`javac`用于将源码文件编译为字节码文件；`javadoc`用于从源码文件中自动提取注释并生成文档；`jar`用于将一组字节码文件打包成一个`.jar`文件，便于发布；`jdb`用于开发阶段的运行调试。

> <!-- Part 003 -->
>
> ※ `Java`程序总是从`public static void main(String[] args)`方法开始执行。

```java
public class Main {
    // Java 程序规定的方法必须为静态方法，方法名必须为 main，方法内的参数必须为 String 数组。
    public static void main(String[] args) {
        System.out.println("Hello, chen!");
    }
}
```

> ※ 一个源码文件<span style="color:red">只能</span>定义一个`public`类型的`class`，并且`class`名称要和文件名完全一致。

#### Day 002

> <!-- Part 001 -->
>
> ※ 基本数据类型是指CPU可以直接进行运算的类型，`Java`定义了以下几种基本数据类型：整数类型，包括`byte`、`short`、`int`、`long`；浮点数类型，包括`float`、`double`；字符类型，包括`char`；布尔类型，包括`boolean`。**注**：理论上存储布尔类型只需要一位，但是`JVM`内部通常会把`boolean`表示为<span style="color:red">`4`</span>字节整数。除上述基本数据类型外，其余变量均属于引用类型，包括最常用的`String`字符串。
>
> ※ 可以使用`final`关键字来定义常量：`final double PI = 3.14`。常量一经定义，便不可再次赋值。
>
> ※ 若变量类型的名字太长，写起来比较麻烦，可以使用`var`关键字：

```java
// StringBuilder sb = new StringBuilder();
var sb = new StringBuilder();
```

> 编译器会根据赋值语句自动推断出变量`sb`的类型为`StringBuilder`。因此，使用`var`定义变量，仅仅是少写了变量类型而已。
>
> ※ 定义变量时，应遵循作用域最小化原则，尽量将变量定义在尽可能小的作用域，并且最好不要重复使用变量名。

> <!-- Part 002 -->
>
> ※ 整数的数值表示不但是精确的，而且运算也是精确的。**注**：除数为`0`在编译时并不会报错，在运行时才会报错。此外，溢出不会报错！
>
> ※ 利用右移运算符`>>`对一个<span style="color:red">负数</span>进行右移操作时，高位总是补`1`；利用右移运算符`>>>`对<span style="color:red">整数</span>进行右移操作时，高位总是补`0`。**注**：对`byte`和`short`进行移位时，首先会将它们转换为`int`。

> <!-- Part 003 -->
>
> ※ 整数运算在除数为`0`时会报错，而浮点数运算在除数为`0`时不会报错，只是会返回几个特殊值：

```java
double d1 = 0.0 / 0; // NaN: Not a Number
double d2 = 1.0 / 0; // Infinity: 无穷大
double d3 = -1.0 / 0; // -Infinity: 无穷小
```

> ※ 可以将浮点数强制转换为整数。转换后，浮点数的小数部分将<span style="color:red">被舍弃</span>，若整数部分超过了整型能表示的最大范围，将返回整型的最大值。**注**：若想要四舍五入，只需在转换前为浮点数加`0.5`即可。

> <!-- Part 004 -->
>
> ※ `Java`中的三元运算符为`b ? x : y`；`Python`中的三元运算符为：`x if b else y`。

> <!-- Part 005 -->
>
> ※ `Java`在内存中总是使用`Unicode`表示字符。关于`Unicode`和`UTF-8`之间的关系，可以自行参考<a href="https://www.zhihu.com/question/23374078">知乎</a>。
>
> ※ 在使用`+`连接任意字符串和其它数据类型时，编译器会先将其它数据类型转换为字符串，之后再进行连接。
>
> ※ 引用类型的变量可以指向一个空值`null`，它表示该变量不指向任何对象。**注**：要区分空值`null`和空字符串`""`，后者属于有效的字符串对象。

> <!-- Part 006 -->
>
> ※ 数组初始化存在如下三种方式：

```java
int[] ns = new int[5];
int[] ns = new int[] {1, 2, 3, 4, 5};
int[] ns = {1, 2, 3, 4, 5}
```

> **注**：数组属于引用类型，并且一旦创建后大小便不可变。

#### Day 003

> <!-- Part 001 -->
>
> ※ 通过使用占位符`%?`，`System.out.printf()`可以把参数格式化成指定格式。常见的占位符有：`%d`，格式化输出整数；`%x`，格式化输出十六进制整数；`%f`，格式化输出浮点数；`%e`，格式化输出科学计数法表示的浮点数；`%s`，格式化输出字符串。**注**：一个`%`表示占位符，两个`%%`表示`%`本身。关于格式化参数的更多信息，可以自行参考<a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Formatter.html#syntax">文档</a>。
>
> ※ 从控制台读取字符串及整数的通用流程：

```java
// Step 1: 导入 java.util.Scanner
import java.util.Scanner;
// Step 2: 创建 Scanner 对象并传入 System.in
Scanner scanner = new Scanner(System.in);
// Step 3: 读取用户输入的字符串
String name = scanner.nextLine();
// Step 4: 读取用户输入的整数
int age = scanner.nextInt();
```

> <!-- Part 002 -->
>
> ※ 要判断引用类型的变量是否相等，必须使用`equals()`，因为`==`此时仅用于判断变量是否指向同一个对象。**注**：执行语句`s1.equals(s2)`时，若变量`s1`为`null`，会报`NullPointerException`。为避免这一错误，可以借助短路运算符`&&`：`s1 != null && s1.equals(s2)`。

> <!-- Part 003 -->
>
> ※ 使用`switch`时，`case`语句后面不需要接`{}`。
>
> ※ 从`Java 12`开始，`switch`升级为更简洁的表达式语法，保证只有一条路径会被执行，并且不需要`break`语句：

```java
String fruit = "apple";
switch (fruit) {
    case "apple" -> System.out.println("Selected apple");
    case "pear" -> System.out.println("Selected pear");
    // 若要执行多条语句，需要在 case 语句后面接 {}
    case "mango" -> {
        System.out.println("Selected mango");
        System.out.println("Good choice!");
    }
    default -> System.out.println("No fruit selected");
}
```

> **注**：`Java 12`只是引入了`Switch`表达式作为预览特性；`Java 13`对该特性进行了修改，并且引入了`yield`语句，用于返回值；直到`Java 14`，这一功能才正式作为标准功能提供出来。

#### Day 004

> <!-- Part 001 -->
>
> ※ 打印一维数组存在如下三种方式：
>
> ```java
> int[] ns = { 1, 1, 2, 3, 5, 8 };
> // Way 1:
> for (int i = 0; i < ns.length; i++) {
>     System.out.println(ns[i]);
> }
> // Way 2:
> for (int n: ns) {
>     System.out.println(n);
> }
> // Way 3:
> System.out.println(Arrays.toString(ns));
> ```
>
> ※ 打印二维数组存在如下两种方式：
>
> ```java
> int[][] ns = {
>     { 1, 2, 3, 4 },
>     { 5, 6, 7, 8 },
>     { 9, 10, 11, 12 }
> };
> // Way 1:
> for (int[] arr: ns) {
>     for (int n: arr) {
>         System.out.print(n);
>     }
>     System.out.println();
> }
> // Way 2:
> System.out.println(Arrays.deepToString(ns));
> ```

#### Day 005

> <!-- Part 001 -->
>
> ※ `Person chen`用于定义`Person`类型的变量`chen`，`new Person()`用于创建`Person`实例。
>
> ※ 可变参数兼容数组类参数，而数组类参数不兼容可变参数。关于可变参数，可以自行参考<a href="https://www.cnblogs.com/zhuitian/p/12274443.html">博客</a>。
>
> ※ 引用类型参数的传递，调用方和接收方的参数变量指向的是同一个对象，任意一方对变量进行修改，都会影响另一方。

> <!-- Part 002 -->
>
> ※ 没有在构造方法中初始化字段时，基本数据类型的字段用默认值，引用类型的字段用`null`。
>
> ※ 创建对象实例时，将先会对字段进行初始化，如`String name = "chen"`；然后对构造方法进行初始化。
>
> ※ 一个构造方法可以调用其它构造方法，这样做的目的是便于代码复用。调用其它构造方法的语法为：`this(...)`。
>
> ※ 若自定义了构造方法，编译器将不再自动创建默认构造方法；若想要既能使用带参数的构造方法，又想要保留不带参数的构造方法，只能将两个构造方法都定义出来。

> <!-- Part 003 -->
>
> ※ 方法<span style="color:blue">重载</span>是指方法的方法名相同，但参数不同。**注**：与方法的返回类型无关。

> <!-- Part 004 -->
>
> ※ 子类将自动获得父类所有字段，<span style="color:red">严禁</span>定义与父类重名的字段。
>
> ※ 在定义类时，若没有明确指明所继承的类，编译器将会自动为其加上`extends Object`。
>
> ※ 子类无法继承父类中被`private`修饰的字段，为此，可以将`private`修改为`protected`。
>
> ※ 若父类没有默认的构造方法，那么子类必须显示调用`super()`并给出参数，以便编译器定位到父类中合适的构造方法。**注**：子类默认的构造方法是编译器自动生成而非从父类中继承的。
>
> ※ 将子类类型的实例赋值给父类变量，称为<span style="color:blue">向上转型</span>；将父类类型的实例强制赋值给子类变量，称为<span style="color:blue">向下转型</span>。由于子类类型的功能比父类类型多，因此向下转型很可能会失败。为了避免出错，可以借助`instanceof`操作符，该操作符用于判断变量所指向的实例是否为指定类型或指定类型的子类。**注**：若一个引用变量为`null`，那么对任何`instanceof`的判断都为`false`。

> <!-- Part 005 -->
>
> ※ 方法<span style="color:blue">覆写</span>是指子类定义了一个与父类方法签名和返回类型完全相同的方法。
>
> ※ `@override`注解可以让编译器帮忙检查是否进行了正确的覆写。**注**：`@override`不是必需的。
>
> ※ 多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。
>
> ※ 在子类的覆写方法中，可以通过`super`来调用父类被覆写的方法。
>
> ※ 被`final`修饰的方法不能被覆写，被`final`修饰的类不能被继承。

> <!-- Part 006 -->
>
> ※ 若父类方法本身不需要实现任何功能，只是为了定义方法以令子类覆写，那么可以把父类方法声明为抽象方法：`public abstract void run()`。
>
> ※ 若某个类中包含了抽象方法，那么必须把该类也声明为`abstract`，才能正确编译它。**注**：我们无法实例化一个抽象类。

> <!-- Part 007 -->
>
> ※ 若一个抽象类中没有字段，所有方法均为抽象方法，那么可以将该抽象类改写为接口：`interface`。**注**：一个类只能继承自另一个类，但可以继承多个接口。
>
> ※ 当我们需要给接口新增一个方法时，若新增的方法为`default`方法，那么实现了该接口的子类无需全部修改，只需在需要覆写的地方去覆写新增方法即可。
>
> ※ 抽象类与接口之间的区别：
>
> |            |          抽象类          |              接口               |
> | :--------: | :----------------------: | :-----------------------------: |
> |    继承    | 只能`extends`一个`class` | 可以`implements`多个`interface` |
> |    字段    |     可以定义实例字段     |        不能定义实例字段         |
> |  抽象方法  |     可以定义抽象方法     |        可以定义抽象方法         |
> | 非抽象方法 |    可以定义非抽象方法    |      可以定义`default`方法      |

> <!-- Part 008 -->
>
> ※ 利用`static`修饰的字段，称为静态字段。对于静态字段，类的所有实例都会共享它。
>
> ※ 利用`static`修饰的方法，称为静态方法。**注**：由于静态方法属于类而不属于实例，因此其内部无法访问实例字段，也无法访问`this`变量，<span style="color:red">只能</span>访问静态字段。**TODO**：为什么？
>
> ※ 接口可以定义静态字段，而且静态字段必须为`final`类型。由于接口的字段只能是`public static final`类型，因此可以将这些修饰符都去掉。

> <!-- Part 009 -->
>
> ※ 包主要用于解决类名冲突。
>
> ※ 包可以是多层结构，用`.`隔开。**注**：`java.util`和`java.util.zip`之间没有任何继承关系。
>
> ※ 位于同一个包的类，可以访问包作用域的字段和方法。其中，<span style="color:blue">包作用域</span>是指一个类允许访问同一个包中没有被`public`、`private`修饰的类，以及没有被`public`、`protected`、`private`修饰的字段和方法。
>
> ※ 当编译器遇到一个类名时，若该类名为完整类名，则会直接根据完整类名查找；若该类名为简单类名，则会按照如下顺序依次查找：当前`package`；`import`的包；`java.lang`中的包。**注**：编写类时，编译器会自动帮我们做两个`import`的动作：自动`import`当前包的其它类；自动`import java.lang.*`。

> <!-- Part 010 -->
>
> ※ 定义为`public`的类和接口可以被其它任何类访问；定义为`public`的字段和方法可以被其它类访问，前提是有访问类的权限。
>

> <!-- Part 011 -->
>
> ※ `Java`的内部类可以分为`Inner Class`、`Anonymous Class`和`Static Nested Class`。其中，`Inner Class`和`Anonymous Class`本质上是相同的，都必须依附于`Outer Class`的实例，即隐含地持有`Outer.this`实例，并拥有`Outer Class`的`private`访问权限；`Static Nested Class`为独立类，不依附于`Outer Class`的实例，但拥有`Outer Class`的`private`访问权限。

> <!-- Part 012 -->
>
> ※ `classpath`是一组目录的集合，用于指示`JVM`搜索`.class`的路径及顺序。
>
> ※ `classpath`的设置方式有两种：在系统环境变量中设置；在启动`JVM`时设置。不推荐前者，因为那样会污染整个系统环境。利用后者设置`classpath`的方式如下：

```shell
java -classpath .;C:\work\project\bin;C:\sgared abc.xyz.Hello
java -cp .;C:\work\project\bin;C:\sgared abc.xyz.Hello
```

> 若没有设置系统环境变量，也没有传入`-cp`参数，那么`JVM`默认的`classpath`为`.`，即当前目录。
>
> ※ `jar`用于将`package`组织的目录层级，以及各个目录下的所有文件（包括`.class`文件和其它文件）都打包成一个`jar`文件。
>
> ※ `jar`可以包含一个特殊的`/META-INF/MANIFEST.MF`纯文本文件，用于提供基本信息，如`Main-Class`，这样可以直接运行`jar`。

> <!-- Part 013 -->
>
> ※ `jar`只是存放`.class`的容器，但是并不关心它们之间的依赖，从`Java 9`开始引入的模块便是为了解决依赖这一问题。**注**：模块的后缀名为`jmod`。
>
> ※ 将一堆`class`封装为`jar`只是一个打包的过程，而把一堆`class`封装为模块不但需要打包，还需将依赖关系写入至`module-info.java`中，并且还可以包含二进制代码（通常是`JNI`扩展）。此外，模块支持多版本，即在同一个模块中可以为不同的`JVM`提供不同的版本。
>
> ※ 利用模块，可以按需打包`jre`。关于如何编写模块，运行模块及打包`JRE`，可以自行参考<a href="https://www.liaoxuefeng.com/wiki/1252599548343744/1281795926523938">教程</a>。

#### Day 006

> ※ 