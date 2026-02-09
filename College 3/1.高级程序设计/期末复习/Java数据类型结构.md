# Java 数据类型 / 结构

## Java 数组

## 声明数组变量

首先必须声明数组变量，才能在程序中使用数组。下面是声明数组变量的语法：

```Java
dataType[] arrayRefVar;   // 首选的方法  
//或  
dataType arrayRefVar[];  // 效果相同，但不是首选方法
```

### 创建数组

<span style="color:#FF9933; font-weight:bold;">Java的数组需要创建，与C语言不一样！</span>Java 语言使用 new 操作符来创建数组，语法如下：

```Java
arrayRefVar = new dataType[arraySize];
//e.g.
double[] myList = new double[10];
```

另一种方式和 C 语言很像。

```Java
dataType[] arrayRefVar = {value0, value1, ..., valuek};
```

Java 数组的索引也是从 0 开始

### **数组的操作**

基本操作、函数参数、函数返回值都和 C 语言一致

[java 内置的数组操作方法](https://www.runoob.com/java/java-array.html#:~:text=java.util.Arrays%20%E7%B1%BB%E8%83%BD%E6%96%B9%E4%BE%BF%E5%9C%B0%E6%93%8D%E4%BD%9C%E6%95%B0%E7%BB%84%EF%BC%8C%E5%AE%83%E6%8F%90%E4%BE%9B%E7%9A%84%E6%89%80%E6%9C%89%E6%96%B9%E6%B3%95%E9%83%BD%E6%98%AF%E9%9D%99%E6%80%81%E7%9A%84%E3%80%82)

## 字符串/String 类

### 创建字符串

String 类 **有 11 种构造方法**，这些方法提供不同的参数来初始化字符串

```Java
//e.g.1
String str = "Runoob";
//e.g.2
String str2=new String("Runoob");
//e.g.3
char[] helloArray = { 'r', 'u', 'n', 'o', 'o', 'b'};
String helloString = new String(helloArray)
```

#### 创建格式化字符串

String 类使用静态方法 format() 返回一个 String 对象而不是 PrintStream 对象。

String 类的静态方法 format() 能用来创建可复用的格式化字符串，而不仅仅是用于一次打印输出。

```java
System.out.printf("浮点型变量的值为 " +
                  "%f, 整型变量的值为 " +
                  " %d, 字符串变量的值为 " +
                  "is %s", floatVar, intVar, stringVar);
// or
String fs;
fs = String.format("浮点型变量的值为 " +
                   "%f, 整型变量的值为 " +
                   " %d, 字符串变量的值为 " +
                   " %s", floatVar, intVar, stringVar);
```

根据你提供的资料，我为你整理了 Java “字符串方法”的笔记内容。这部分内容涵盖了查找、提取、修改、比较以及分割等核心操作，并附带了代码示例与预期输出。

------

### 字符串常用方法

#### 1. 查找子字符串

Java 提供了两种主要方法来定位子字符串在原字符串中的位置。如果未检索到目标，均返回 `-1`。

- `indexOf(String s)`: 返回参数字符串 `s` 在指定字符串中 **首次** 出现的索引位置。
- `lastIndexOf(String s)`: 返回参数字符串 `s` 在指定字符串中 **最后一次** 出现的索引位置。



```Java
String str = "We are students";
int firstA = str.indexOf("a");      // 结果: 3
int lastE = str.lastIndexOf("e");   // 结果: 12
int emptyPos = str.lastIndexOf(""); // 结果: 15 (等同于字符串长度)

System.out.println("首次出现'a'的位置: " + firstA);
System.out.println("最后出现'e'的位置: " + lastE);
```

#### 2. 获取指定位置字符与子串

- `charAt(int index)`: 返回指定索引处的字符。
- `substring(int beginIndex)`: 从指定位置截取到字符串末尾。
- `substring(int beginIndex, int endIndex)`: 截取从 `beginIndex` 到 `endIndex` 之间的内容（包含头，不包含尾）`[beginIndex,endIndex)`



```Java
String str = "hello word";
char mychar = str.charAt(5);           // 结果: ' ' (空格)
String sub1 = str.substring(3);        // 结果: "lo word"
String sub2 = str.substring(0, 3);     // 结果: "hel"

System.out.println("索引5的字符: [" + mychar + "]");
System.out.println("子串1: " + sub1);
System.out.println("子串2: " + sub2);
```

#### 3. 去除空格与替换内容

- `trim()`: 返回字符串的副本，忽略前导空格和尾部空格。不忽略中间空格
- `replace(oldChar, newChar)`: 将指定的字符或字符串全部替换为新的内容（注意大小写敏感）。



```Java
String str = "  address  ";
String str = "192.168.1.1 = 162.24.56";
String trimmed = str.trim();               // 结果: "address"
String str1 = str.trim();				//结果：""192.168.1.1 = 162.24.56""
String replaced = trimmed.replace("a", "A"); // 结果: "Address"

System.out.println("去除空格: [" + trimmed + "]");
System.out.println("替换后: " + replaced);
```

#### 4. 判断开始、结尾与相等

- `startsWith(String prefix)` / `endsWith(String suffix)`: 判断字符串是否以指定内容开始或结束。
- `equals(String otherstr)`: 比较内容是否完全相同（区分大小写）。
- `equalsIgnoreCase(String otherstr)`: 比较内容是否相同（忽略大小写）。



```Java
String str = "JavaProject";
boolean start = str.startsWith("Java");     // 结果: true
boolean end = str.endsWith("txt");          // 结果: false
boolean isEqual = str.equals("javaproject"); // 结果: false

System.out.println("是以Java开始吗? " + start);
System.out.println("相等吗(忽略大小写)? " + str.equalsIgnoreCase("javaproject"));
```

#### 5. 字典顺序比较

- `compareTo(String otherstr)`: 基于 Unicode 值按字典顺序比较。
  - 返回负整数：当前字符串位于参数之前。
  - 返回正整数：当前字符串位于参数之后。
  - 返回 0：两个字符串相等。



```Java
String str1 = "a";
String str2 = "b";
System.out.println(str1.compareTo(str2)); // 输出: -1 (a在b之前)
```

#### 6. 大小写转换

- `toLowerCase()`: 全部转为小写。
- `toUpperCase()`: 全部转为大写。


```Java
String str = "Java";
System.out.println(str.toLowerCase()); // 输出: java
System.out.println(str.toUpperCase()); // 输出: JAVA
```

#### 7. 字符串分割

- `split(String sign)`: 根据分隔符拆分字符串。如果想定义多个分割符，可使用符号 `|`。例如，`,|=` 表示分割符分别为 `,` 和 `=`。
- `split(String sign, int limit)`: 限制拆分的分数。可以使用 `|` 定义多个分隔符。



```Java
String str = "192.168.1.1";
String[] parts = str.split("\\."); // 使用转义或正则分割

for(String p : parts) {
    System.out.println("片段: " + p);
}
// 预期输出:
// 片段: 192
// 片段: 168
// 片段: 1
// 片段: 1
```

------

>  [!NOTE]
>
> 由于 `String` 类是不可变的，以上所有修改操作（如 `replace`、`trim`）都会返回一个新的字符串对象，而不会改变原字符串的内容。

> [!TIP]
>
> String 类确实是不可变的。如果需要频繁变换 string 值，建议用 [StringBuffer 和 StringBuilder 类](https://www.runoob.com/java/java-stringbuffer.html)

---

