[TOC]

# Java SE

## Java Syntax

### Variables

Variables are containers for storing data values.

In Java, there are different **types** of variables, belows are some we usually use:

- `Stirng`
- `int`
- `float`
- `char`
- `boolean`: store values with two states: ture or false.

```java
int myNum = 5;
float myFloatNum = 5.99f;
char myLetter = 'D';
boolean myBool = true;
String myText = "Hello";
```



#### `final` key word

If you don't want others (or yourself) to overwrite existing values, use the `final` keyword (this will declare the variable as "final" or "constant", which means unchangeable and read-only):

```java
final int myNum = 15;
myNum = 20;  // will generate an error: cannot assign a value to a final variable
```

### Data Types

#### Primitive Data Types

There are eight primitive data types in Java:

| Data Type | Description                                                  |
| :-------: | :----------------------------------------------------------- |
|  `byte`   | Stores whole numbers from -128 to 127                        |
|  `short`  | Stores whole numbers from -32,768 to 32,767                  |
|   `int`   | Stores whole numbers from -2,147,483,648 to 2,147,483,647    |
|  `long`   | Stores whole numbers from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
|  `float`  | Stores fractional numbers. Sufficient for storing 6 to 7 decimal digits |
| `double`  | Stores fractional numbers. Sufficient for storing 15 to 16 decimal digits |
| `boolean` | Stores true or false values                                  |
|  `char`   | Stores a single character/letter or ASCII values             |

#### Non-Primitive Data Types



#### The Var keywords



### Type casting

#### widening casting

It is automatic, Java makes it by itself. 



```java
int myInt = 9;
double myDouble = myInt; // Automatic casting: int to double

System.out.println(myInt);    // Outputs 9
System.out.println(myDouble); // Outputs 9.0
```



#### arrowing Casting

It must be done manually by placing the type in parentheses `()` in front of the value.

```java
double myDouble = 9.78d;
int myInt = (int) myDouble; // Manual casting: double to int

System.out.println(myDouble); // Outputs 9.78
System.out.println(myInt);    // Outputs 9
```

### Java operators

- [Arithmetic operators 算术运算符](https://www.w3schools.com/java/java_operators_arithmetic.asp)
- [Assignment operators 赋值操作符](https://www.w3schools.com/java/java_operators_assign.asp)
- [Comparison operators 比较运算符](https://www.w3schools.com/java/java_operators_comparison.asp)
- [Logical operators 逻辑运算符](https://www.w3schools.com/java/java_operators_logical.asp)
- Bitwise operators 位操作符

It is similar to the syntax of C, and The precedence is also like c.

### Java String

```java
String greeting = "Hello";
```

**String Length**

This is significant for understanding the **method** in java.

```java
String txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
System.out.println("The length of the txt string is: " + txt.length());
```

We find that the `.` is used in the syntax. In python, we may do this. 

```python
text = "Hello, World!"
length = len(text) # =====different=====
print(length)  # 输出：13
# Actually,the function of len is a built-in function, which does not need '.'
```

#### String Methods

There are many of them.

[Java String 类 | 菜鸟教程](https://www.runoob.com/java/java-string.html)

#### Special Characters

like "we are "champions" ", it must be wrote like below

```java
String txt = "We are \" champions \" ";
System.out.println(txt);
```

The backslash (`\`) escape character turns special characters into string characters. 



### Java arrays

To declare an array, define the variable type with **square brackets** `[ ]` :

```java
String[] cars;
```

To insert values to it, you can place the values in a comma-separated list, inside **curly braces** `{ }`; Access an array element by referring to the index number.

```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
System.out.println(cars[0]);
// Outputs Volvo
```

**Change an Array Element**: To change the value of a specific element, refer to the index number

```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
cars[0] = "Opel";
System.out.println(cars[0]);
// Now outputs Opel instead of Volvo
```

#### Array Length

To find out how many elements an array has, use the `length` property:

```java
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
System.out.println(cars.length); // comparison txt.length().ame attribute and method
// Outputs 4
```

There are some difference about the use method between array and string.

#### The `new` Keyword

You can also create an array by specifying its size with `new`. This makes an empty array with space for a fixed number of elements, which you can fill later:

```java
String[] cars = new String[4]; // size is 4
cars[0] = "Volvo";
System.out.println(cars[0]); // Outputs Volvo
//Belows are correct syntaxes
// With new
String[] cars = new String[] {"Volvo", "BMW", "Ford", "Mazda"};

// Shortcut (most common)
String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
```

> [!TIP]
>
> **Note:** You cannot write `new String[4] {"Volvo", "BMW", "Ford", "Mazda"}`.
> In Java, when using `new`, you either:
>
> - Use `new String[4]` to create an empty array with 4 slots, and then fill them later.
> - Or use `new String[] {"Volvo", "BMW", "Ford", "Mazda"}` (without specifying the number of elements) to create the array and assign values at the same time
>
> **Tip:** The shortcut syntax is most often used when the values are known at the start. Use `new` with a size when you want to create an empty array and fill it later.

> [!WARNING]
>
> [ ] :  `new String[4]` 
>
> { } :  ` {"Volvo", "BMW", "Ford", "Mazda"}`

### Other syntax

Java Math, Booleans, If...Else Switch, while loop, for loop, break, continue...These are the same as C

**Addition**: ==for-each loop==.

```java
// Template
for (type variableName : arrayName) {
  // code block to be executed
}

// Example
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



## Java Methods

### Create and Call a Method

A method must be declared within a class. It is defined with the name of the method, followed by parentheses **()**. 

```java
public class Main {
  static void myMethod() {
    // code to be executed
  }
}
```

> [!NOTE]
> `static` means that the method belongs to the Main class and is not an object of the Main class.

To call a method in Java, write the method's name followed by two parentheses **()** and a semicolon**;**

```java
public class Main {
  static void myMethod() {
    System.out.println("I just got executed!");
  }

  public static void main(String[] args) {
    myMethod();
  }
}

// Outputs "I just got executed!"
```

### Method Paraments

skip

### Method Overloading

With method overloading, multiple methods can **have the same name with different parameters**:

> [!CAUTION]
>
> Multiple methods can have the same name as long as the number and/or type of parameters are different.

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

### Java Scope

1.Method Scope--Variables declared directly inside a **method** are available anywhere in the method following the line of code in which they were declared; 

2.Block Scope--Variables declared inside a **block of code** are only accessible by the code between the curly braces, and only after the line in which the variable was declared

3.Loop Scope--Variables declared inside a `for` loop only exist inside the loop

4.Class Scope--Variables declared inside a class but outside any method have **class scope** (also called fields). These variables can be accessed by all methods in the class

```java
public class Main {
  public static void main(String[] args) {

    // Code here CANNOT use x

    { // This is a block

      // Code here CANNOT use x

      int x = 100;

      // Code here CAN use x
      System.out.println(x);

    } // The block ends here

    // Code here CANNOT use x

  }
}
```

Maybe a kind of specialty of java

## Java Class

### Java OOP

OOP stands for **Object-Oriented Programming**. While Procedural programming is about writing procedures or methods that perform operations on the data, while object-oriented programming is about creating objects that contain both data and methods.

Object-oriented programming has several advantages over procedural programming:

- OOP is faster and easier to execute
- OOP provides a clear structure for the programs
- OOP helps to keep the Java code DRY "Don't Repeat Yourself", and makes the code easier to maintain, modify and debug
- OOP makes it possible to create full reusable applications with less code and shorter development time

### Java Class/Objects

For short, a class is a template for objects, and an object is an instance of a class.:rose:When the individual objects are created, they <font color=red>inherit</font> all the variables and methods from the class.

Everything in Java is associated with classes and objects, along with its **attributes** and **methods**.For example: in real life, a car is an object. The car has **attributes**, such as weight and color, and **methods**, such as drive and brake.

#### Create Class

To create a class, use the keyword `class`: 

```java
public class Main {
  int x = 5;
}
```

> [!TIP]
>
> A class should always start with an **uppercase first letter**, and that the name of the java file should **match the class name**.

#### Create an Object

In Java, an object is created from a class. We have already created the class named `Main`, so now we can use this to create objects.

```java
/*
 ** `public class Main {` : It is a class/class constructor function
 ** `public static void main(String[] args)` : The main function,which does not belongs to Class Main
 ** `Main myObj = new Main();` : 'Main' refers to variable type, myObj is the name of variable,'new' is keyword of creating an object,'Main()' is class constructor function
*/
public class Main {
  int x = 5;

  public static void main(String[] args) {
    Main myObj = new Main();
    System.out.println(myObj.x);
  }
}
```

#### Using Multiple Classes

You can also create an object of a class and access it in another class. This is often used for better organization of classes (one class has all the attributes and methods, while the other class holds the `main()` method (code to be executed)).

> [!NOTE]
>
> **Example**
> we have created two files ==in the same directory/folder==:
>
> - Main.java
> - Second.java
>
> //Main.java
>
> ```java
> public class Main {
>   int x = 5;
> }
> ```
> 
>//Second.java
> 
>```java
> class Second {
>   public static void main(String[] args) {
>     Main myObj = new Main();
>     System.out.println(myObj.x);
>   }
> }
> ```
> 
>When both files have been compiled, Run the Second.java file
> 
>```cmd
> C:/Users/Your Name>javac Main.java
> C:/Users/Your Name>javac Second.java
> ```
> 
>```cmd
> C:/Users/Your Name>java Second
> ```
> 

### Java Class Attributes

Access attributes by creating an object of the class, and by using the dot syntax (`.`)

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

### Java Class Methods

To call a method, write the method's name followed by two parentheses **()** and a semicolon**;**

**Static vs. Public**

```java
public class Main {
  // Static method
  static void myStaticMethod() {
    System.out.println("Static methods can be called without creating objects");
  }

  // Public method
  public void myPublicMethod() {
    System.out.println("Public methods must be called by creating objects");
  }

  // Main method
  public static void main(String[] args) {
    myStaticMethod(); // Call the static method
    // myPublicMethod(); This would compile an error

    Main myObj = new Main(); // Create an object of Main
    myObj.myPublicMethod(); // Call the public method on the object
  }
}
```

> [!IMPORTANT]
>
> A `static` method means it can be accessed without creating an object of the class. Unlike,a `public` method can only be accessed by objects.

### Java Constructors

A constructor in Java is a **special method** that is used to initialize objects.

```java
// Create a Main class
public class Main {
  int x;  // Create a class attribute

  // Create a class constructor for the Main class
  public Main() {
    x = 5;  // Set the initial value for the class attribute x
  }

  public static void main(String[] args) {
    Main myObj = new Main(); // Create an object of class Main (This will call the constructor)
    System.out.println(myObj.x); // Print the value of x
  }
}

// Outputs 5
```

> [!NOTE]
>
> 1.The constructor name must **match the class name**, and it can't have a **return type** (like `void`).
>
> 2.The constructor is called when an object of a class is created.
>
> 3.All classes have constructors by default: if you do not create a class constructor yourself, Java creates one for you. However, then you are not able to set initial values for object attributes.

**Constructor Parameters**

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

// Outputs 5
```

### 'this' keyword

The `this` keyword is often used to avoid confusion when class attributes have the same name as method or constructor parameters.

**Accessing Class Attributes**

Sometimes a constructor or method has a parameter with the same name as a class variable. When this happens, the parameter temporarily **hides** the class variable inside that method or constructor.To refer to the class variable and not the parameter, you can use the `this` keyword:

```java
public class Main {
  int x;  // Class variable x

  // Constructor with one parameter x
  public Main(int x) {
    this.x = x; // refers to the class variable x
  }

  public static void main(String[] args) {
    // Create an object of Main and pass the value 5 to the constructor
    Main myObj = new Main(5);
    System.out.println("Value of x = " + myObj.x);
  }
}
```

**Calling a Constructor from Another Constructor**

use `this()` to call another constructor in the same class.

```java
public class Main {
  int modelYear;
  String modelName;

  // Constructor with one parameter
  public Main(String modelName) {
    // Call the two-parameter constructor to reuse code and set a default year    
    this(2020, modelName);
  }

  // Constructor with two parameters
  public Main(int modelYear, String modelName) {
    // Use 'this' to assign values to the class variables
    this.modelYear = modelYear;
    this.modelName = modelName;
  }

  // Method to print car information
  public void printInfo() {
    System.out.println(modelYear + " " + modelName);
  }

  public static void main(String[] args) {
    // Create a car with only model name (uses default year)
    Main car1 = new Main("Corvette");

    // Create a car with both model year and name
    Main car2 = new Main(1969, "Mustang");

    car1.printInfo();
    car2.printInfo();
  }
}

//2020 Corvette
//1969 Mustang
```

### Java Modifiers

We divide modifiers into two groups:我们将修饰语分为两类：

- **Access Modifiers** - controls the access level访问修饰符-控制访问级别
- **Non-Access Modifiers** - do not control access level, but provides other functionality非访问修饰符——不控制访问级别，但提供其他功能

#### Access Modifiers

For **classes**, you can use either `public` or *default*:

| Modifier  | Description                                                  |
| :-------: | :----------------------------------------------------------- |
| `public`  | The class is accessible by any other class                   |
| *default* | The class is only accessible by classes in the same package. This is used when you don't specify a modifier. You will learn more about packages in the [Packages chapter](https://www.w3schools.com/java/java_packages.asp) |

For **attributes, methods and constructors**, you can use the one of the following:

|  Modifier   | Description                                                  |
| :---------: | :----------------------------------------------------------- |
|  `public`   | The code is accessible for all classes                       |
|  `private`  | The code is only accessible within the declared class        |
|  *default*  | The code is only accessible in the same package. This is used when you don't specify a modifier. You will learn more about packages in the [Packages chapter](https://www.w3schools.com/java/java_packages.asp) |
| `protected` | The code is accessible in the same package and **subclasses**. You will learn more about subclasses and superclasses in the [Inheritance chapter](https://www.w3schools.com/java/java_inheritance.asp) |

#### Non-Access modifiers

Non-access modifiers do not control visibility (like `public` or `private`), but instead add **other features** to classes, methods, and attributes.The most commonly used non-access modifiers are `final`, `static`, and `abstract`.

For **classes**, you can use either `final` or `abstract`:

| Modifier   | Description                                                  |
| :--------- | :----------------------------------------------------------- |
| `final`    | The class cannot be inherited by other classes (You will learn more about inheritance in the [Inheritance chapter](https://www.w3schools.com/java/java_inheritance.asp)) |
| `abstract` | The class cannot be used to create objects (To access an abstract class, it must be inherited from another class. You will learn more about inheritance and abstraction in the [Inheritance](https://www.w3schools.com/java/java_inheritance.asp) and [Abstraction](https://www.w3schools.com/java/java_abstract.asp) chapters) |

For **attributes and methods**, you can use the one of the following:

| Modifier       | Description                                                  |
| :------------- | :----------------------------------------------------------- |
| `final`        | Attributes and methods cannot be overridden/modified         |
| `static`       | Attributes and methods belong to the class, not to objects. This means all objects share the same `static` attribute, and `static` methods can be called without creating objects. |
| `abstract`     | Can only be used in an abstract class, and can only be used on methods. The method does not have a body, for example `abstract void run();`. The body is provided by the subclass (inherited from). You will learn more about inheritance and abstraction in the [Inheritance](https://www.w3schools.com/java/java_inheritance.asp) and [Abstraction](https://www.w3schools.com/java/java_abstract.asp) chapters |
| `transient`    | Attributes and methods are skipped when serializing the object containing them |
| `synchronized` | Methods can only be accessed by one thread at a time         |
| `volatile`     | The value of an attribute is not cached thread-locally, and is always read from the "main memory" |

### Java Encapsulation

The meaning of **Encapsulation**, is to make sure that "sensitive" data is hidden from users. To achieve this, you must:

- declare class variables/attributes as `private`  将类变量/属性声明为 `private`
- provide <u>public</u> **get** and **set** methods to access and update the value of a `private` variable   提供公共服务 并设置访问和更新a `private` 值的方法 变量

### Java Packages

**Java Packages & API**

A package in Java is used to group related classes. Packages are divided into two categories:

- Built-in Packages (packages from the Java API)  内置包（来自Java API的包）
- User-defined Packages (create your own packages)  用户定义的包（创建您自己的包）

**Built-in Packages**

The complete list can be found at Oracles website: https://docs.oracle.com/javase/8/docs/api/. It is divided into **packages** and **classes**.

**Use `import` to use a package**

```java
import package.name.Class;   // Import a single class
import package.name.*;   // Import the whole package

import java.util.Scanner; // java.util is a package, while Scanner is  a class
```



### Java inheritance

- **subclass** (child) - the class that inherits from another class  子类（子）——从另一个类继承的类
- **superclass** (parent) - the class being inherited from   超类（父类）——被继承的类

To inherit from a class, use the `extends` keyword.
If you don't want other classes to inherit from a class, use the `final` keyword:



### Java Polymorphism

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
    Animal myAnimal = new Animal();  // Create a Animal object
    Animal myPig = new Pig();  // Create a Pig object
    Animal myDog = new Dog();  // Create a Dog object
    myAnimal.animalSound();
    myPig.animalSound();
    myDog.animalSound();
  }
}
```

The same method but different class!!!



### Java super Keyword

In Java, the `super` keyword is used to refer to the **parent class** of a subclass.

<font color = red>The most common use of the `super` keyword is to <u>eliminate</u> the confusion between superclasses and subclasses that have methods with the same name.</font>

- Access attributes and methods from the parent class.
- Call the parent class constructor.



```java
class Animal {
  public void animalSound() {
    System.out.println("The animal makes a sound");
  }
}

class Dog extends Animal {
  public void animalSound() {
    super.animalSound(); // Call the parent method---
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

To better understand the polymorphism and super, it's better to write a relationship map of the class.

```java
//call constructor
class Animal {
  Animal() {
    System.out.println("Animal is created");
  }
}

class Dog extends Animal {
  Dog() {
    super(); // Call parent constructor
    System.out.println("Dog is created");
  }
}

public class Main {
  public static void main(String[] args) {
    Dog myDog = new Dog();
  }
}
```

### Java inner Classes

The purpose of nested classes is to group classes that belong together, which makes your code more readable and maintainable.

To access the inner class, create an object of the outer class, and then create an object of the inner class:

**Private Inner Class**

Unlike a "regular" class, an inner class can be `private` or `protected`. If you don't want outside objects to access the inner class, declare the class as `private`:

**Static Inner Class**

An inner class can also be `static`, which means that you can access it without creating an object of the outer class:

> [!CAUTION]
>
> Just like `static` attributes and methods, a `static` inner class does not have access to members of the outer class.

**Access Outer Class From Inner Class**

One advantage of inner classes, is that they can access attributes and methods of the outer class

### Java abstraction

Data **abstraction** is the process of hiding certain details and showing only essential information to the user.

The `abstract` keyword is a non-access modifier, used for classes and methods:

- **Abstract class:** is a restricted class that cannot be used to create objects (to access it, it must be inherited from another class).抽象类：是一个受限制的类，不能用于创建对象（要访问它，必须从另一个类继承）。
- **Abstract method:** can only be used in an abstract class, and it does not have a body. The body is provided by the subclass (inherited from).抽象方法：只能在抽象类中使用，它没有主体。主体由子类（继承自）提供。
