# Java 进阶：静态类、静态方法与实用类库详解

本教程基于《高级程序设计-10 静态类与方法》PPT内容，涵盖了从 `static` 关键字到底层包装类、格式化以及日期处理的完整知识体系。

## 一、 静态关键字 `static` 的核心原理

### 1. 静态方法 (Static Methods)

静态方法属于类本身，而不属于类的任何特定实例。

- **调用方式**：通过类名直接调用，如 `Math.sqrt()`。
- **约束条件**：
  - 静态方法**不能**直接访问非静态（实例）变量。
  - 静态方法**不能**直接调用非静态（实例）方法。
  - 静态方法中没有 `this` 关键字。

案例：Math 类

Math 类是静态方法的典型代表，你不需要 new Math() 就能使用它。

```Java
public class MathExample {
    public static void main(String[] args) {
        // 直接通过类名调用静态方法
        double result = Math.sqrt(64); 
        System.out.println("64的平方根是: " + result);
        
        int absoluteValue = Math.abs(-10);
        System.out.println("-10的绝对值是: " + absoluteValue);
    }
}
```

### 2. 静态变量 (Static Variables)

静态变量（也称类变量）被该类的所有实例**共享**。

- **初始化时机**：在类加载时初始化，早于任何静态方法执行。
- **经典用途**：记录创建了多少个实例。

```Java
public class Duck {
    private String name;
    // 静态变量：记录所有鸭子对象的总数
    private static int duckCount = 0;

    public Duck(String name) {
        this.name = name;
        // 每创建一个对象，静态计数器加1
        duckCount++;
    }

    public static int getDuckCount() {
        // 静态方法可以访问静态变量
        return duckCount;
    }
}

// 测试类
class DuckTest {
    public static void main(String[] args) {
        new Duck("唐老鸭");
        new Duck("可达鸭");
        System.out.println("当前鸭子总数: " + Duck.getDuckCount()); // 输出: 2
    }
}
```

### 3. 静态常量 (Static Final Variables)

通常使用 `public static final` 来定义一个不可变的全局常量。

- **命名规范**：通常全大写，单词间用下划线分隔。

```Java
public class Constants {
    // 定义一个静态常量
    public static final double PI = 3.141592653589793;
}
```

## 二、 `final` 关键字的三种境界

1. **final 变量**：代表常量，一旦赋值不可更改。
2. **final 方法**：锁定方法，子类不能覆盖（Override）它。
3. **final 类**：绝育类，不能被继承（Extend），例如 `String` 类。

## 三、 包装类 (Wrapper Classes) 与 自动装箱 (Autoboxing)

更加详细的解释在ds老师: https://chat.deepseek.com/share/b6pincxzmedudeelvg

Java 为每种基本数据类型都提供了一个对应的包装类，使基本类型具有对象的特征。

**包装类的作用**：

- 让基本数据类型也能用于面向对象的场合（如集合类 `ArrayList<Integer>`）
- 提供一些实用方法（如转换、比较、常量等）
- 支持 **自动装箱与拆箱**（下面讲解）

| **基本类型** | **包装类** |
| ------------ | ---------- |
| int          | Integer    |
| char         | Character  |
| boolean      | Boolean    |
| double       | Double     |

### 1. 自动装箱与拆箱

**自动装箱** 是指 **Java编译器自动将基本数据类型转换为对应的包装类对象**。
**自动拆箱** 则相反，是 **将包装类对象自动转换为基本数据类型**

Java 5 之后，基本类型与包装类可以自动转换。

```Java
import java.util.ArrayList;

public class WrapperExample {
    public static void main(String[] args) {
        // 自动装箱：int -> Integer
        Integer x = 5; 
        
        // 自动拆箱：Integer -> int
        int y = x + 10;

        // 在集合中非常有用（集合只能存对象）
        ArrayList<Integer> list = new ArrayList<>();
        list.add(100); // 自动装箱
        int val = list.get(0); // 自动拆箱
        
        // 静态方法：String 转 基本类型
        String s = "123";
        int i = Integer.parseInt(s);
        System.out.println("转换后的整数: " + i);
    }
}
```

关于`ArrayList<>`， 编译器可以根据**变量的声明类型** `ArrayList<String>` 来推断构造器的类型参数。当你写 `new ArrayList<>()` 时，编译器看到：

- 左侧：`ArrayList<String> list`
- 右侧：`new ArrayList<>()`

它会自动推断出 `<>` 应该填入 `String`，相当于 `new ArrayList<String>()`。

包装类提供了一些非常实用的 **静态方法**，把 String 转成主数据类型，也可以反过来。例如：

```java
// String → 基本数据类型
int num = Integer.parseInt("123");
double d = Double.parseDouble("3.14");
boolean b = Boolean.parseBoolean("true");

// 基本数据类型 → String
String s1 = Integer.toString(456);
String s2 = Double.toString(2.718);

// 其他实用方法
Integer.max(10, 20);
Character.isDigit('5');
Integer.toBinaryString(10); // "1010"
```

### 注意事项

- **性能开销**：自动装箱/拆箱会创建临时对象，在大量循环中可能影响性能。

- **比较问题**：

  

```Java
Integer a = 100;
Integer b = 100;
System.out.println(a == b); // true（缓存范围内）

Integer c = 200;
Integer d = 200;
System.out.println(c == d); // false（超出缓存范围，应使用 equals 比较）
```

  应使用 `.equals()` 或 `intValue()` 进行比较。

**总结**

- **包装类**：为8种基本类型提供对象形式。
- **自动装箱/拆箱**：让基本类型与包装类之间可以无缝转换，简化代码。
- **常用方法**：`parseXxx()`、`toString()`、`valueOf()` 等。
- **应用场景**：集合、泛型、反射、支持null值等。

## 四、 数字格式化 (Number Formatting)

使用 `String.format()` 或 `System.out.printf()` 对数字进行精细化控制。

**格式化指令语法**：`% [argument_index$] [flags] [width] [.precision] conversion`

```Java
public class FormatExample {
    public static void main(String[] args) {
        double myNumber = 12345.6789;

        // %.2f 表示保留两位小数
        String s = String.format("保留两位小数: %.2f", myNumber);
        System.out.println(s);

        // %,.2f 表示带千分位分隔符并保留两位小数
        System.out.printf("带逗号格式: %,.2f\n", myNumber);
        
        // 整数补零格式化
        System.out.printf("补零输出: %05d\n", 42); // 输出: 00042
    }
}
```

## 五、 日期处理：Calendar 类

`java.util.Calendar` 是处理日期的标准方式。

```Java
import java.util.Calendar;

public class CalendarExample {
    public static void main(String[] args) {
        // 获取当前日期的实例
        Calendar cal = Calendar.getInstance();
        
        // 设置特定日期（注意：月份是从0开始的，11代表12月）
        cal.set(2025, Calendar.DECEMBER, 25);
        System.out.println("设置的时间是: " + cal.getTime());

        // 日期运算：增加1个月
        cal.add(Calendar.MONTH, 1);
        System.out.println("增加一个月后: " + cal.getTime());
        
        // 获取具体字段
        int year = cal.get(Calendar.YEAR);
        System.out.println("当前年份: " + year);
    }
}
```

## 六、 静态导入 (Static Import)

如果你极其频繁地使用某个类的静态方法，可以使用静态导入来简化代码。

```Java
// 静态导入 Math 类中的所有静态成员
import static java.lang.Math.*;

public class StaticImportExample {
    public static void main(String[] args) {
        // 不再需要写 Math.sqrt()，直接写 sqrt()
        double r = sqrt(pow(3, 2) + pow(4, 2));
        System.out.println("结果: " + r);
    }
}
```

## 总结：脑筋急转弯解答

1. **非静态方法可以读取静态变量吗？**
   - **答：可以。** 静态变量是全类共享的，实例方法（非静态）当然可以访问它。
2. **如果类已经是 final 的，再标记方法为 final 是不是多余？**
   - **答：是。** 因为类不能被继承，所以其中的方法永远不可能被重写（Override），标记为 final 在逻辑上是多余的，但不会导致错误。