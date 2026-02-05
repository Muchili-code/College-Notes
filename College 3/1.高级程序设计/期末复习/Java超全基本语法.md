# Java SE
[TOC]
## Java 语法

### 变量

变量是用于存储数据值的容器。

在 Java 中，有不同**类型**的变量，以下是我们经常使用的：

* `String`
* `int`
* `float`
* `char`
* `boolean`：存储两种状态的值：true 或 false。

```java
int myNum = 5;
float myFloatNum = 5.99f;
char myLetter = 'D';
boolean myBool = true;
String myText = "Hello";

```

#### `final` 关键字

如果你不希望其他人（或你自己）覆盖现有值，请使用 `final` 关键字（这将把变量声明为“final”或“常量”，意味着不可更改且只读）：

```java
final int myNum = 15;
myNum = 20;  // 将产生错误：无法为最终变量赋值

```

### 数据类型

#### 基本数据类型

Java 中有八种基本数据类型：

| 数据类型 | 描述 |
| --- | --- |
| `byte` | 存储从 -128 到 127 的整数 |
| `short` | 存储从 -32,768 到 32,767 的整数 |
| `int` | 存储从 -2,147,483,648 到 2,147,483,647 的整数 |
| `long` | 存储从 -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807 的整数 |
| `float` | 存储小数。足以存储 6 到 7 位小数 |
| `double` | 存储小数。足以存储 15 到 16 位小数 |
| `boolean` | 存储 true 或 false 值 |
| `char` | 存储单个字符/字母或 ASCII 值 |

#### 非基本数据类型

（此处原文为空）

#### Var 关键字

（此处原文为空）

### 类型转换

#### 扩展转换 (Widening Casting)

这是自动完成的，Java 会自动处理。

```java
int myInt = 9;
double myDouble = myInt; // 自动转换：int 转 double

System.out.println(myInt);    // 输出 9
System.out.println(myDouble); // 输出 9.0

```

#### 缩小转换 (Narrowing Casting)

必须通过在值前面加上括号 `()` 中的类型来手动完成。

```java
double myDouble = 9.78d;
int myInt = (int) myDouble; // 手动转换：double 转 int

System.out.println(myDouble); // 输出 9.78
System.out.println(myInt);    // 输出 9

```

### Java 运算符

* [Arithmetic operators 算术运算符](https://www.w3schools.com/java/java_operators_arithmetic.asp)
* [Assignment operators 赋值操作符](https://www.w3schools.com/java/java_operators_assign.asp)
* [Comparison operators 比较运算符](https://www.w3schools.com/java/java_operators_comparison.asp)
* [Logical operators 逻辑运算符](https://www.w3schools.com/java/java_operators_logical.asp)
* Bitwise operators 位操作符

它与 C 语言的语法相似，优先级也像 C。

### Java 字符串 (String)

```java
String greeting = "Hello";

```

**字符串长度**

这对于理解 Java 中的 **方法 (method)** 很重要。

```java
String txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
System.out.println("The length of the txt string is: " + txt.length());

```

我们会发现语法中使用了 `.`。在 Python 中，我们可能会这样做：

```python
text = "Hello, World!"
length = len(text) # =====不同点=====
print(length)  # 输出：13
# 实际上，len 的功能是一个内置函数，不需要 '.'

```

#### 字符串方法

有很多字符串方法。

[Java String 类 | 菜鸟教程](https://www.runoob.com/java/java-string.html)

#### 特殊字符

像 "we are "champions" " 这样的字符串，必须写成如下形式：

```java
String txt = "We are \" champions \" ";
System.out.println(txt);

```

反斜杠 (`\`) 转义字符将特殊字符转换为字符串字符。

### Java 数组

要声明数组，请使用 **方括号** `[ ]` 定义变量类型：

```java
String[] cars;

```

要插入值，可以将值放在 **花括号** `{ }` 内的逗号分隔列表中；通过引用索引号访问数组元素。

```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
System.out.println(cars[0]);
// 输出 Volvo

```

**修改数组元素**：要更改特定元素的值，请引用索引号：

```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
cars[0] = "Opel";
System.out.println(cars[0]);
// 现在输出 Opel 而不是 Volvo

```

#### 数组长度

要找出数组有多少个元素，请使用 `length` 属性：

```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
System.out.println(cars.length); // 对比 txt.length()。注意属性和方法的区别
// 输出 4

```

数组和字符串在使用方法上有一些区别。

#### `new` 关键字

你也可以使用 `new` 指定大小来创建数组。这会创建一个具有固定数量元素空间的空数组，你可以稍后填充它：

```java
String[] cars = new String[4]; // 大小为 4
cars[0] = "Volvo";
System.out.println(cars[0]); // 输出 Volvo
// 以下是正确的语法
// 使用 new
String[] cars = new String[] {"Volvo", "BMW", "Ford", "Mazda"};

// 简写 (最常用)
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};

```

> [!TIP]
> **注意：** 你不能写 `new String[4] {"Volvo", "BMW", "Ford", "Mazda"}`。
> 在 Java 中，使用 `new` 时，你要么：
> * 使用 `new String[4]` 创建一个有 4 个插槽的空数组，稍后填充。
> * 或者使用 `new String[] {"Volvo", "BMW", "Ford", "Mazda"}`（不指定元素数量）来创建数组并同时赋值。
> 
> 
> **提示：** 当开始时已知数值时，通常使用简写语法。当你想要创建一个空数组并在以后填充时，使用带有大小的 `new`。

> [!Warning]
> [ ] :  `new String[4]`
> { } :  ` {"Volvo", "BMW", "Ford", "Mazda"}`

### 其他语法

Java Math, Booleans, If...Else Switch, while 循环, for 循环, break, continue... 这些都和 C 语言一样。

**补充**：==for-each 循环==。

```java
// 模板
for (type variableName : arrayName) {
  // 要执行的代码块
}

// 示例
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
for (String car : cars) {
  System.out.println(car);
}

```

```java
int[][] myNumbers = { {1, 4, 2}, {3, 6, 8, 5, 2} };

for (int[] row : myNumbers) {
  for (int num : row) {
    System.out.println(num);
  }
}

```

## Java 方法

### 创建和调用方法

方法必须在类中声明。它的定义是方法名称后跟括号 **()**。

```java
public class Main {
  static void myMethod() {
    // 要执行的代码
  }
}

```

> [!NOTE]
> `static` 意味着该方法属于 Main 类，而不是 Main 类的对象。

要在 Java 中调用方法，请写出方法名称，后跟两个括号 **()** 和分号 **;**

```java
public class Main {
  static void myMethod() {
    System.out.println("I just got executed!");
  }

  public static void main(String[] args) {
    myMethod();
  }
}

// 输出 "I just got executed!"

```

### 方法参数

(原文跳过)

### 方法重载

通过方法重载，多个方法可以**拥有相同的名称但具有不同的参数**：

> [!caution]
> 只要参数的数量和/或类型不同，多个方法就可以拥有相同的名称。

```java
static int plusMethod(int x, int y) {
  return x + y;
}
static double plusMethod(double x, double y) {
  return x + y;
}
public static void main(String[] args) {
  int myNum1 = plusMethod(8, 5);
  double myNum2 = plusMethod(4.3, 6.26);
  System.out.println("int: " + myNum1);
  System.out.println("double: " + myNum2);
}

```

### Java 作用域

1. 方法作用域 (Method Scope)——直接在**方法**内部声明的变量，在声明它的那行代码之后，该方法内的任何地方都可用；
2. 块作用域 (Block Scope)——在**代码块**内部声明的变量只能由花括号之间的代码访问，并且只能在声明变量的行之后访问；
3. 循环作用域 (Loop Scope)——在 `for` 循环内部声明的变量只存在于循环内部；
4. 类作用域 (Class Scope)——在类内部但在任何方法之外声明的变量具有**类作用域**（也称为字段）。这些变量可以被类中的所有方法访问。

```java
public class Main {
  public static void main(String[] args) {

    // 这里的代码不能使用 x

    { // 这是一个块

      // 这里的代码不能使用 x

      int x = 100;

      // 这里的代码可以使用 x
      System.out.println(x);

    } // 块在这里结束

    // 这里的代码不能使用 x

  }
}

```

这也许是 Java 的一种特性。

## Java 类

### Java OOP (面向对象编程)

OOP 代表**面向对象编程**。面向过程编程是关于编写对数据执行操作的过程或方法，而面向对象编程是关于创建包含数据和方法的对象。

面向对象编程相对于面向过程编程有几个优点：

* OOP 执行更快且更容易
* OOP 为程序提供了清晰的结构
* OOP 有助于保持 Java 代码 DRY "Don't Repeat Yourself"（不要重复自己），并使代码更易于维护、修改和调试
* OOP 使得用更少的代码和更短的开发时间创建完全可重用的应用程序成为可能

### Java 类/对象

简而言之，类是对象的模板，而对象是类的实例。:rose:当单个对象被创建时，它们从类中<font color=red>继承</font>所有的变量和方法。

Java 中的一切都与类和对象以及它们的**属性**和**方法**相关联。例如：在现实生活中，汽车是一个对象。汽车有**属性**，如重量和颜色，以及**方法**，如驾驶和刹车。

#### 创建类

要创建类，请使用关键字 `class`：

```java
public class Main {
  int x = 5;
}

```

> [!tip]
> 类名应该始终以**大写字母开头**，并且 java 文件的名称应该**与类名匹配**。

#### 创建对象

在 Java 中，对象是从类创建的。我们已经创建了名为 `Main` 的类，所以现在我们可以使用它来创建对象。

```java
/*
 ** `public class Main {` : 这是一个类/类构造函数
 ** `public static void main(String[] args)` : 主函数，它不属于 Main 类
 ** `Main myObj = new Main();` : 'Main' 指的是变量类型，myObj 是变量名，'new' 是创建对象的关键字，'Main()' 是类构造函数
*/
public class Main {
  int x = 5;

  public static void main(String[] args) {
    Main myObj = new Main();
    System.out.println(myObj.x);
  }
}

```

#### 使用多个类

你也可以创建一个类的对象并在另一个类中访问它。这通常用于更好地组织类（一个类拥有所有的属性和方法，而另一个类包含 `main()` 方法（要执行的代码））。

> [!note]
> **示例**
> 我们==在同一个目录/文件夹==中创建了两个文件：
> * Main.java
> * Second.java
> 
> 
> //Main.java
> ```java
> public class Main {
>   int x = 5;
> }
> 
> ```
> 
> 
> //Second.java
> ```java
> class Second {
>   public static void main(String[] args) {
>     Main myObj = new Main();
>     System.out.println(myObj.x);
>   }
> }
> 
> ```
> 
> 
> 当两个文件都编译完成后，运行 Second.java 文件
> ```cmd
> C:/Users/Your Name>javac Main.java
> C:/Users/Your Name>javac Second.java
> 
> ```
> 
> 
> ```cmd
> C:/Users/Your Name>java Second
> 
> ```
> 
> 

### Java 类属性

通过创建类的对象，并使用点号语法 (`.`) 来访问属性。

```java
public class Main {
  int x;

  public static void main(String[] args) {
    Main myObj = new Main();
    myObj.x = 40;
    System.out.println(myObj.x);	//但是不会改变Main中的x
  }
}

```

### Java 类方法

要调用方法，写出方法名称后跟两个括号 **()** 和分号 **;**

**Static vs. Public (静态与公共)**

```java
public class Main {
  // Static 方法
  static void myStaticMethod() {
    System.out.println("Static methods can be called without creating objects");
  }

  // Public 方法
  public void myPublicMethod() {
    System.out.println("Public methods must be called by creating objects");
  }

  // Main 方法
  public static void main(String[] args) {
    myStaticMethod(); // 调用静态方法
    // myPublicMethod(); 这会编译报错

    Main myObj = new Main(); // 创建 Main 的对象
    myObj.myPublicMethod(); // 在对象上调用公共方法
  }
}

```

> [!important]
> `static` 方法意味着它可以在不创建类对象的情况下被访问。与之不同，`public` 方法只能由对象访问。

### Java 构造函数

Java 中的构造函数是一种用于初始化对象的**特殊方法**。

```java
// 创建一个 Main 类
public class Main {
  int x;  // 创建一个类属性

  // 为 Main 类创建一个类构造函数
  public Main() {
    x = 5;  // 设置类属性 x 的初始值
  }

  public static void main(String[] args) {
    Main myObj = new Main(); // 创建 Main 类的对象 (这将调用构造函数)
    System.out.println(myObj.x); // 打印 x 的值
  }
}

// 输出 5

```

> [!note]
> 1. 构造函数名称必须**与类名匹配**，并且它不能有**返回类型**（如 `void`）。
> 2. 构造函数在创建类的对象时被调用。
> 3. 所有类默认都有构造函数：如果你自己不创建类构造函数，Java 会为你创建一个。但是，那样你就无法为对象属性设置初始值。
> 
> 

**构造函数参数**

```java
public class Main {
  int x;

  public Main(int y) {
    x = y;
  }

  public static void main(String[] args) {
    Main myObj = new Main(5);
    System.out.println(myObj.x);
  }
}

// 输出 5

```

### this 关键字

当类属性与方法或构造函数参数同名时，通常使用 `this` 关键字以避免混淆。

**访问类属性**

有时构造函数或方法有一个与类变量同名的参数。当这种情况发生时，参数会暂时在该方法或构造函数内**隐藏**类变量。要引用类变量而不是参数，你可以使用 `this` 关键字：

```java
public class Main {
  int x;  // 类变量 x

  // 带有一个参数 x 的构造函数
  public Main(int x) {
    this.x = x; // 指代类变量 x
  }

  public static void main(String[] args) {
    // 创建 Main 的对象并将值 5 传递给构造函数
    Main myObj = new Main(5);
    System.out.println("Value of x = " + myObj.x);
  }
}

```

**从另一个构造函数调用构造函数**

使用 `this()` 在同一个类中调用另一个构造函数。

```java
public class Main {
  int modelYear;
  String modelName;

  // 带有一个参数的构造函数
  public Main(String modelName) {
    // 调用双参数构造函数以复用代码并设置默认年份
    this(2020, modelName);
  }

  // 带有两个参数的构造函数
  public Main(int modelYear, String modelName) {
    // 使用 'this' 为类变量赋值
    this.modelYear = modelYear;
    this.modelName = modelName;
  }

  // 打印汽车信息的方法
  public void printInfo() {
    System.out.println(modelYear + " " + modelName);
  }

  public static void main(String[] args) {
    // 仅使用型号名称创建汽车（使用默认年份）
    Main car1 = new Main("Corvette");

    // 使用型号年份和名称创建汽车
    Main car2 = new Main(1969, "Mustang");

    car1.printInfo();
    car2.printInfo();
  }
}

//2020 Corvette
//1969 Mustang

```

### Java 修饰符

我们将修饰符分为两类：

* **Access Modifiers** - 访问修饰符 - 控制访问级别
* **Non-Access Modifiers** - 非访问修饰符 - 不控制访问级别，但提供其他功能

#### 访问修饰符

对于**类**，你可以使用 `public` 或 *default*：

| 修饰符 | 描述 |
| --- | --- |
| `public` | 该类可以被任何其他类访问 |
| *default* | 该类仅能被同一包中的类访问。这是当你未指定修饰符时使用的。你将在 [Packages chapter](https://www.w3schools.com/java/java_packages.asp) 中了解更多关于包的信息 |

对于**属性、方法和构造函数**，你可以使用以下之一：

| 修饰符 | 描述 |
| --- | --- |
| `public` | 代码对所有类均可访问 |
| `private` | 代码仅在声明的类内部可访问 |
| *default* | 代码仅在同一包中可访问。这是当你未指定修饰符时使用的。你将在 [Packages chapter](https://www.w3schools.com/java/java_packages.asp) 中了解更多关于包的信息 |
| `protected` | 代码在同一包中和**子类**中可访问。你将在 [Inheritance chapter](https://www.w3schools.com/java/java_inheritance.asp) 中了解更多关于子类和超类的信息 |

#### 非访问修饰符

非访问修饰符不控制可见性（如 `public` 或 `private`），而是向类、方法和属性添加**其他特性**。最常用的非访问修饰符是 `final`、`static` 和 `abstract`。

对于**类**，你可以使用 `final` 或 `abstract`：

| 修饰符 | 描述 |
| --- | --- |
| `final` | 该类不能被其他类继承（你将在 [Inheritance chapter](https://www.w3schools.com/java/java_inheritance.asp) 中了解更多关于继承的信息） |
| `abstract` | 该类不能用于创建对象（要访问抽象类，必须从另一个类继承。你将在 [Inheritance](https://www.w3schools.com/java/java_inheritance.asp) 和 [Abstraction](https://www.w3schools.com/java/java_abstract.asp) 章节了解更多关于继承和抽象的信息） |

对于**属性和方法**，你可以使用以下之一：

| 修饰符 | 描述 |
| --- | --- |
| `final` | 属性和方法不能被重写/修改 |
| `static` | 属性和方法属于类，而不属于对象。这意味着所有对象共享同一个 `static` 属性，并且 `static` 方法可以在不创建对象的情况下调用。 |
| `abstract` | 只能在抽象类中使用，并且只能用于方法。该方法没有主体，例如 `abstract void run();`。主体由子类（继承自）提供。你将在 [Inheritance](https://www.w3schools.com/java/java_inheritance.asp) 和 [Abstraction](https://www.w3schools.com/java/java_abstract.asp) 章节了解更多关于继承和抽象的信息 |
| `transient` | 序列化包含它们的对像时，会跳过属性和方法 |
| `synchronized` | 方法一次只能由一个线程访问 |
| `volatile` | 属性的值不会缓存在线程本地，并且总是从“主内存”读取 |

### Java 封装

**封装 (Encapsulation)** 的含义是确保“敏感”数据对用户隐藏。要实现这一点，你必须：

* 将类变量/属性声明为 `private`
* 提供 <u>public</u> **get** 和 **set** 方法来访问和更新 `private` 变量的值

### Java 包

**Java 包 & API**

Java 中的包用于对相关的类进行分组。包分为两类：

* 内置包 (Built-in Packages)（来自 Java API 的包）
* 用户定义的包 (User-defined Packages)（创建您自己的包）

**内置包**

完整列表可以在 Oracle 网站上找到：[https://docs.oracle.com/javase/8/docs/api/](https://docs.oracle.com/javase/8/docs/api/)。它分为**包**和**类**。

**使用 `import` 来使用包**

```java
import package.name.Class;   // 导入单个类
import package.name.*;   // 导入整个包

import java.util.Scanner; // java.util 是一个包，而 Scanner 是一个类

```

### Java 继承

* **subclass** (子类) - 从另一个类继承的类
* **superclass** (超类/父类) - 被继承的类

要从一个类继承，请使用 `extends` 关键字。
如果你不希望其他类继承某个类，请使用 `final` 关键字：

### Java 多态

```java
class Animal {
  public void animalSound() {
    System.out.println("The animal makes a sound");
  }
}

class Pig extends Animal {
  public void animalSound() {
    System.out.println("The pig says: wee wee");	//--
  }
}

class Dog extends Animal {
  public void animalSound() {
    System.out.println("The dog says: bow wow");	//--
  }
}

class Main {
  public static void main(String[] args) {
    Animal myAnimal = new Animal();  // 创建一个 Animal 对象
    Animal myPig = new Pig();  // 创建一个 Pig 对象
    Animal myDog = new Dog();  // 创建一个 Dog 对象
    myAnimal.animalSound();
    myPig.animalSound();
    myDog.animalSound();
  }
}

```

相同的方法但不同的类！！！

### Java super 关键字

在 Java 中，`super` 关键字用于引用子类的**父类**。

<font color = red>`super` 关键字最常用的用途是<u>消除</u>具有相同名称方法的超类和子类之间的混淆。</font>

* 访问父类的属性和方法。
* 调用父类构造函数。

```java
class Animal {
  public void animalSound() {
    System.out.println("The animal makes a sound");
  }
}

class Dog extends Animal {
  public void animalSound() {
    super.animalSound(); // 调用父类方法---
    System.out.println("The dog says: bow wow");
  }
}

public class Main {
  public static void main(String[] args) {
    Dog myDog = new Dog();
    myDog.animalSound();
  }
}

```

```cmd
The animal makes a sound
The dog says: bow wow

```

为了更好地理解多态和 super，最好画出类的关系图。

```java
//调用构造函数
class Animal {
  Animal() {
    System.out.println("Animal is created");
  }
}

class Dog extends Animal {
  Dog() {
    super(); // 调用父类构造函数
    System.out.println("Dog is created");
  }
}

public class Main {
  public static void main(String[] args) {
    Dog myDog = new Dog();
  }
}

```

### Java 内部类 (Inner Classes)

嵌套类的目的是将属于在一起的类分组，这使你的代码更具可读性和可维护性。

要访问内部类，请创建外部类的对象，然后创建内部类的对象：

**私有内部类**

与“常规”类不同，内部类可以是 `private` 或 `protected`。如果你不希望外部对象访问内部类，请将该类声明为 `private`：

**静态内部类**

内部类也可以是 `static`，这意味着你可以在不创建外部类对象的情况下访问它：

> [!caution]
> 就像 `static` 属性和方法一样，`static` 内部类无法访问外部类的成员。

**从内部类访问外部类**

内部类的一个优点是它们可以访问外部类的属性和方法。

### Java 抽象

数据**抽象**是隐藏某些细节并仅向用户显示基本信息的过程。

`abstract` 关键字是一个非访问修饰符，用于类和方法：

* **抽象类 (Abstract class)：** 是一个受限制的类，不能用于创建对象（要访问它，必须从另一个类继承）。
* **抽象方法 (Abstract method)：** 只能在抽象类中使用，它没有主体。主体由子类（继承自）提供。