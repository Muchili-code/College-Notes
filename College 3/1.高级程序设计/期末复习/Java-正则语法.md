#  Java 中的正则表达式

[TOC]

> [!TIP]
>
> 正则基本，见[正则](./new_regex.html)

Java 在 `java.util.regex` 包中提供支持。

## 4.1 String 类

- `boolean matches(String regex)`: 告诉这个字符串是否匹配给定的正则表达式。它要求**整个字符串**完全匹配正则，而不是包含正则。

```java
String phone = "18868111057 ";
boolean flag = phone.matches("1[0-9]{7}")
```

> [!TIP] 
>
> **代码避坑**：在 Java 字符串里写正则，反斜杠 `\` 需要转义。例如匹配数字 `\d` 要写成 `"\\d"`。
>
> 这是因为在 Java 中，一个字符串字面量要经历**两次**解析过程才能变成正则引擎能识别的指令。
>
> 1. **第一次解析（Java 编译器）**：Java 编译器先读取你的源代码字符串。在 Java 字符串中，`\` 是转义符（例如 `\n` 是换行）。如果你只写 `"\d"`，Java 编译器会报错：“非法的转义字符”，因为它不认识 `\d`。
> 2. **第二次解析（正则引擎）**：正则引擎需要看到 `\d` 才能识别为“数字”。
>
> **推导公式：** 为了让正则引擎看到 `\d` -> Java 字符串必须存 `\d` 这两个字符 -> 源代码里必须写 `"\\d"`（第一个 `\` 转义了第二个 `\`，使其变成普通字符）。

**最极端的例子：匹配一个真实的“反斜杠”字符 `\`**

- **正则逻辑**：在正则里，匹配特殊字符 `\` 需要转义，所以正则应该写成 `\\`。
- **Java 逻辑**：为了让正则引擎收到 `\\`，Java 源代码里每一个 `\` 都需要用另一个 `\` 来保护。
- **结果**：你需要写 **`"\\\\"`**。
  - 第 1、2 个 `\\` → Java 解析为第一个 `\`。
  - 第 3、4 个 `\\` → Java 解析为第二个 `\`。
  - 正则引擎收到 `\\` → 匹配到一个 `\`

## 4.2 核心类

> [!NOTE]
>
> 在 Java 中，正则表达式的操作被严格拆分为两个阶段：**编译（Compile）** 和 **匹配（Match）**。这分别对应 `java.util.regex` 包下的两个核心类

1. **Pattern 类**:
   - 正则表达式的**编译后**表示形式。
   - 无公共构造函数(没办法**new**)，通过 `Pattern.compile(String regex)` 创建。
   - **创建方法**：`Pattern p = Pattern.compile("正则表达式");`
   - 当你调用 `Pattern.compile()` 时，JVM 会解析正则语法，并将其构建成一个由内部 `Node` 对象组成的树状结构（一个非确定性有限自动机 - NFA）。
   - **生命周期**：通常作为静态常量（`static final`）存在。它是**线程安全**且**不可变的**。
   - **JVM 视角**：编译正则是一个耗 CPU 的过程，涉及语法分析和内存分配。
2. **Matcher 类**:
   - 执行匹配操作的引擎。
   - 通过 `pattern.matcher(CharSequence input)` 获得。
   - **创建方法**：`Matcher m = p.matcher("目标字符串");`
   - 是**有状态（Stateful）**的，非线程安全。它记录了当前匹配到了字符串的哪个位置（Index）。
3. **PatternSyntaxException**:
   - 非强制异常 (Unchecked Exception)，指示正则语法错误。

光看着很抽象，下面的代码看一下

```java
// Java 中使用正则的标准起手式（“三步走”战略）
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexDemo {
    public static void main(String[] args) {
        // 1. 定义源数据
        String text = "Java17, Java8, Java21";

        // 2. 第一步：编译 (Compile)
        // 将字符串形式的正则，编译成 Pattern 对象 (这是 NFA 图结构)
        // 注意：Java 字符串中 \d 需要写成 \\d (下章细讲)
        Pattern pattern = Pattern.compile("Java\\d+");

        // 3. 第二步：生成匹配器 (Matcher)
        // 将 Pattern 绑定到具体的文本上，生成一个有状态的 Matcher
        Matcher matcher = pattern.matcher(text);

        // 4. 第三步：循环匹配 (Execution)
        // find() 是一个“步进”动作，它会记住上一次匹配结束的位置
        while (matcher.find()) {
            System.out.println("找到匹配: " + matcher.group());
            // start() 和 end() 返回当前匹配子串的索引位置
            System.out.println("位置范围: " + matcher.start() + " - " + matcher.end());
        }
    }
}
```

**深度原理：为什么 Java 强制“先编译再匹配”？**

很多初学者会问：*“为什么不能像 String 类那样直接给结果，非要搞这么麻烦？”*。其实，`String.matches("regex", "input")` 内部确实也可以直接用，但它的代价很高。

```java
// 效率低下：循环 100 万次，编译 100 万次
for (String phone : phoneList) {
    if (phone.matches("1\\d{10}")) { ... } 
}

// 效率高：只编译 1 次，重复使用 100 万次
Pattern p = Pattern.compile("1\\d{10}");
for (String phone : phoneList) {
    if (p.matcher(phone).matches()) { ... }
}
```

**底层原理：正则表达式的生命周期**

当你写下一个正则字符串 `"a*b"` 时，计算机会经历以下步骤：

1. **解析（Parsing）**：检查语法，构建语法树。
2. **编译（Compilation）**：将语法树转化为**非确定性有限自动机（NFA, Nondeterministic Finite Automaton）**。这是一个复杂的图结构节点网络。
3. **执行（Execution）**：输入字符串在 NFA 图中游走，寻找路径。

**关键点：** 步骤 1 和 2 是非常消耗 CPU 资源的。

- **如果你使用 `String.matches(...)`：** 每次调用，Java 都会在内部重新编译一次 NFA，用完即扔。如果你在一个 100 万次的循环里调用它，CPU 就在做 100 万次重复的编译工作，性能极其低下。
- **如果你使用 `Pattern.compile(...)`：** 你只编译一次 NFA，生成一个 `Pattern` 对象。然后你可以生成无数个 `Matcher` 去复用这个 NFA。**这是典型的“空间换时间”策略。**

**进阶知识：Java 特有的旗标（Flags）**

在 Java 中，你不仅可以在正则字符串里写 `(?i)` 来忽略大小写，还可以通过 `Pattern.compile` 的第二个参数来控制。这是考试中的**加分项**。

- **`Pattern.CASE_INSENSITIVE`**：忽略大小写（等同于正则开头的 `(?i)`）。
- **`Pattern.DOTALL`**：让 `.` 号能匹配换行符（等同于 `(?s)`）。**默认情况下 `.` 是不匹配换行符的！**
- **`Pattern.MULTILINE`**：让 `^` 和 `$` 匹配每一行的开头结尾，而不是整个字符串的开头结尾（等同于 `(?m)`）。

```Java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class SyntaxDemo {
    public static void main(String[] args) {
        // 场景：匹配 Windows 路径 "C:\Windows\System32" 中的反斜杠
        // 或者是忽略大小写匹配 "java"
        
        String path = "C:\\Windows\\System32"; // Java 字符串本身也要转义 \
        
        // 1. 匹配字面量反斜杠（考试必考点）
        // 目标：匹配 "\" -> 正则 "\\" -> Java 源码 "\\\\"
        Pattern pathPattern = Pattern.compile("\\\\"); 
        Matcher m1 = pathPattern.matcher(path);
        System.out.println("路径中反斜杠数量: " + m1.results().count()); 
        // Java 9+ 写法，如果你用老版本可以用 while(m.find()) count++

        // 2. 使用 Flag 匹配多行文本
        String log = "Error: 404\nException: NullPointer";
        
        // 目标：匹配以 Error 开头的行。
        // 如果不加 MULTILINE，^ 只匹配字符串最开头，加上后匹配每一行开头
        Pattern logPattern = Pattern.compile("^Error", Pattern.MULTILINE | Pattern.CASE_INSENSITIVE);
        Matcher m2 = logPattern.matcher(log);
        
        if (m2.find()) {
            System.out.println("找到了错误日志头！");
        }
    }
}
```



## 4.3 Matcher 常用方法分类 

- **索引方法 (Index Methods)**: `start()`, `end()` - 查看匹配位置。
- **查找方法 (Search Methods)**: `lookingAt()`, `find()`, `matches()` - 检查是否存在匹配。
- **替换方法 (Replacement Methods)**: `replaceAll()`, `replaceFirst()`, `appendReplacement()`, `appendTail()` - 替换文本。

------

## 3. Matcher 常用方法详解

这是该 MD 文件中最精华的部分，决定了你如何处理匹配到的结果。

`Matcher` 内部维护了一个**游标（Cursor）**，记录当前扫描到的位置。

| **方法**          | **行为描述**                                                 | **对应正则逻辑** | **场景**                       |
| ----------------- | ------------------------------------------------------------ | ---------------- | ------------------------------ |
| **`matches()`**   | **全卷检查**。尝试将**整个**输入字符串与模式匹配。如果不完全匹配，返回 `false`。 | 隐式加上 `^...$` | 表单验证（如校验邮箱、手机号） |
| **`find()`**      | **扫描器**。在输入字符串中查找**下一个**能匹配的子串。每次调用都会推动游标向后移。 | `...`            | 数据提取（爬虫、日志分析）     |
| **`lookingAt()`** | **前缀检查**。从字符串**开头**开始尝试匹配，只要开头符合即可，不要求匹配完整个字符串。 | 隐式加上 `^...`  | 检查协议头（如 `http://`）     |

> [!IMPORTANT] 
>
> `matches()` 和 `lookingAt()` 只有一次机会；而 `find()` 可以在 `while` 循环中多次调用。

**捕获组（Groups）**

通过括号 `()` 将正则分组，可以在匹配成功后提取特定内容。

- **`group(0)`**：永远代表**整个**被匹配到的子串（<span style="color:#FF0000; font-weight:bold;">考试陷阱：不要以为它是第一个括号</span>）。
- **`group(1)`**：第一个左括号 `(` 及其内部内容。
- **`group(n)`**：第 n 个左括号 `(` 及其内部内容。

### A. 查找方法 (Search Methods)

这是最常用的功能：

- **`matches()`**: 尝试将**整个**区域与模式匹配。
- **`find()`**: 扫描输入序列以查找与模式匹配的**下一个**子序列。可以在 `while` 循环中使用它来提取多个结果。
- **`lookingAt()`**: 仅尝试从输入序列的**开头**开始匹配。

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class ApiDemo {
    public static void main(String[] args) {
        String input = "2026-05-20: Error 404";

        // 定义两个正则
        Pattern datePattern = Pattern.compile("\\d{4}-\\d{2}-\\d{2}"); // 仅日期
        Pattern fullLogPattern = Pattern.compile("^(\\d{4}-\\d{2}-\\d{2}): (.*)$"); // 完整格式

        Matcher m1 = datePattern.matcher(input);
        
        // 1. 演示 matches() 失败
        // 因为 input 包含后续内容，而正则只定义了日期，matches 要求全部匹配，所以返回 false
        System.out.println("matches(): " + m1.matches()); 

        // 2. 演示 lookingAt() 成功
        // 重置一下 matcher (因为上面 matches 动了游标)
        m1.reset(); 
        System.out.println("lookingAt(): " + m1.lookingAt()); // true，因为开头是日期

        // 3. 演示 find() 与 group() 提取
        Matcher m2 = fullLogPattern.matcher(input);
        if (m2.find()) {
            System.out.println("完整匹配 group(0): " + m2.group(0)); // 2026-05-20: Error 404
            System.out.println("第一组 group(1): " + m2.group(1));   // 2026-05-20
            System.out.println("第二组 group(2): " + m2.group(2));   // Error 404
        }
    }
}
```



### B. 索引方法 (Index Methods)

当你用 `find()` 找到内容后，可以用这些方法获取位置：

- **`start()`**: 返回上一次匹配成功的起始索引。
- **`end()`**: 返回上一次匹配成功的最后一个字符之后的偏移量。

### C. 替换方法 (Replacement Methods)

- **`replaceAll(String replacement)`**: 替换所有匹配到的子字符串。
- **`replaceFirst(String replacement)`**: 只替换第一个匹配到的子字符串。

| **序号** | **方法签名**        | **功能核心**                                                 | **适用场景**                                               |
| -------- | ------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- |
| **1**    | `appendReplacement` | **渐进式替换**。将当前匹配点之前的文字以及替换后的文字追加到 `StringBuffer`。 | 需要在替换过程中执行逻辑判断（如动态计算替换值）。         |
| **2**    | `appendTail`        | **收尾工作**。将输入序列中最后一次匹配之后的剩余部分追加到 `StringBuffer`。 | 与 `appendReplacement` 配合使用，确保字符串完整。          |
| **3**    | `replaceAll`        | **全部替换**。将所有匹配到的子序列替换为指定的字符串。       | 简单的全量更新，不需中间逻辑。                             |
| **4**    | `replaceFirst`      | **首个替换**。仅替换第一个匹配到的子序列。                   | 只想修改开头的匹配项。                                     |
| **5**    | `quoteReplacement`  | **转义处理**。返回指定字符串的字面值（对 `$` 和 `\` 进行转义）。 | 当替换内容本身包含正则特殊字符，且不希望它们被解析时使用。 |

```Java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexDemo {
    public static void main(String[] args) {
        String input = "苹果价格是 5 元，西瓜价格是 10 元。";
        String regex = "\\d+"; // 匹配数字
        
        Pattern p = Pattern.compile(regex);
        Matcher m = p.matcher(input);
        StringBuffer sb = new StringBuffer();

        System.out.println("原始文本: " + input);

        // 逐个寻找匹配的数字
        while (m.find()) {
            // 获取匹配到的数字并翻倍
            int number = Integer.parseInt(m.group());
            int doubled = number * 2;
            
            // 【关键点】将匹配前的文字 + 替换后的数字(doubled) 放入 sb
            // appendReplacement 会处理匹配位置之前的 "苹果价格是 " 等字符
            m.appendReplacement(sb, String.valueOf(doubled));
        }

        // 【关键点】此时 sb 只有 "... 20"，还缺最后的 " 元。"
        // appendTail 将最后一次匹配后的剩余部分追加上去
        m.appendTail(sb);

        System.out.println("处理后文本: " + sb.toString());
    }
}
```



------

## 异常与边界

Java 正则最容易让程序崩掉的两个地方：一个是**没匹配就取值**，另一个是**取不存在的组**。

#### 1. 深度原理：状态机未就绪与越界

`Matcher` 是一个状态机。刚创建时，它处于“初始状态”，并没有指向任何匹配结果。

- **`IllegalStateException`（非法状态异常）**：
  - **触发原因**：在调用 `find()`, `matches()`, 或 `lookingAt()` **之前**，或者在这些方法返回 `false` **之后**，直接调用了 `group()`。
  - **原理**：状态机说：“我还没干活呢（或者干活失败了），你凭什么问我要结果？”
  - **在 Java 正则中，`group()` 能不能调，完全取决于最近一次 `find()/matches()` 是否返回了 `true`。** 只要是 `false`，碰都不要碰 `group()`。
- **`IndexOutOfBoundsException`（索引越界异常）**：
  - **触发原因**：你调用了 `group(3)`，但正则里总共只有 2 对括号（只有 group 0, 1, 2）。
  - **原理**：和数组越界一样，找不到对应的存储槽位。

#### 2.标准代码模板：防御性编程

```Java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class SafetyDemo {
    public static void main(String[] args) {
        String text = "Order ID: 12345";
        Pattern p = Pattern.compile("Order ID: (\\d+)");
        Matcher m = p.matcher(text);

        // 错误示范（考试常见错误项）：
        // System.out.println(m.group(1)); // ❌ 抛出 IllegalStateException，因为还没 find()

        // 正确示范：
        if (m.find()) {
            // 只有在 find 返回 true 的代码块里，调用 group 才是安全的
            System.out.println("ID: " + m.group(1));
            
            // System.out.println(m.group(2)); // ❌ 抛出 IndexOutOfBoundsException，没有第2组
        } else {
            System.out.println("未找到匹配项");
        }
    }
}
```

## 5.性能与安全（灾难性回溯）

你可能听说过，一个简短的正则表达式就能让 CPU 飙升到 100% 甚至搞挂整个服务器。这就是**灾难性回溯（Catastrophic Backtracking）**。

#### 1. 深度原理：贪婪与固执的代价

Java 的正则引擎是基于 **NFA（非确定性有限自动机）** 的。它的特点是“不到黄河心不死”。 当它遇到无法匹配的情况时，它会不断地**回退（Backtrack）**，尝试其他的匹配路径。

**灾难发生的公式**： `嵌套的量词` + `贪婪匹配` + `几乎匹配但在最后失败的字符串`

例如：`regex = "(x+x+)+y"` 输入：`xxxxxxxxxxxxxxxxxxxx` (没有 y)

1. 引擎吃掉所有的 `x`。
2. 发现最后没有 `y`。
3. **回溯开始**：它吐出一个 `x`，尝试另一种组合方式（比如前一个 `x+` 少吃点，后一个 `x+` 多吃点）。
4. 随着 `x` 长度增加，组合方式呈指数级爆炸（2^n）。只要字符串长度达到 30 左右，就需要计算数亿次，CPU 直接卡死。

#### 2. 解决方案：占有优先量词（Possessive Quantifiers）

Java 提供了一种手段告诉引擎：“吃进去了就别吐出来”。这就是**占有优先量词**，写法是在量词后面再加一个 `+`。

- **贪婪（默认）**：`\d+` —— 尽可能多吃，但为了匹配成功可以吐出来。
- **占有（优化）**：`\d++` —— 尽可能多吃，**死也不吐**。如果后面不匹配，直接放弃，不回溯

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class PerformanceDemo {
    public static void main(String[] args) {
        // 模拟一个恶意的长字符串
        String badInput = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa";

        // 1. 危险写法 (嵌套量词)
        // 含义：任意个a，被括号包裹，然后再来任意次
        // 如果你解开这个注释运行，可能会发现程序卡住几秒甚至更久
        // Pattern bad = Pattern.compile("(a+)+b"); 
        
        // 2. 安全写法 (占有优先量词)
        // a++ 表示匹配多个a，且一旦占有绝不回溯。
        // 当它发现后面没有 'b' 时，因为不能回退吐出字符，立即宣告匹配失败，速度极快。
        long start = System.currentTimeMillis();
        Pattern good = Pattern.compile("(a++)+b"); 
        Matcher m = good.matcher(badInput);
        System.out.println("匹配结果: " + m.find());
        System.out.println("耗时: " + (System.currentTimeMillis() - start) + "ms");
    }
}
```

再看一个贪婪的例子

```Java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexMatches {

    public static void main(String args[]) {

        // 要扫描以查找模式的字符串
        String line = "This order was placed for QT3000! OK?";
        String pattern = "(.*)(\\d+)(.*)";

        // 创建 Pattern 对象
        Pattern r = Pattern.compile(pattern);

        // 现在创建 matcher 对象
        Matcher m = r.matcher(line);
        if (m.find()) {
            // group(0) 总是代表整个匹配到的字符串
            System.out.println("Found value: " + m.group(0));
            // group(1) 匹配第一个括号 (.*)--贪婪This order was placed for QT300，留下最后一个0给\d+
            System.out.println("Found value: " + m.group(1));
            // group(2) 匹配第二个括号 (\\d+)
            System.out.println("Found value: " + m.group(2));
            // 注意：原图中只写到了 group(2)，如果你想看最后一部分，可以增加 m.group(3)
        } else {
            System.out.println("NO MATCH");
        }
    }
}
```



---

## 6. 综合实战演示

为了让你更清晰地理解，这里展示一个提取字符串中所有数字的例子：

```java
import java.util.regex.*;

public class RegexDemo {
    public static void main(String[] args) {
        String text = "我的订单号是: 12345，快递单号是: 67890";
        String regex = "\\d+"; // 匹配一个或多个数字

        // 1. 编译正则
        Pattern p = Pattern.compile(regex);
        // 2. 获取匹配器
        Matcher m = p.matcher(text);

        // 3. 循环查找并打印位置
        while (m.find()) {
            System.out.println("发现数字: " + m.group()); // group() 获取匹配到的文本
            System.out.println("起始位置: " + m.start() + ", 结束位置: " + m.end());
        }
        
        // 4. 替换
        String hiddenText = m.replaceAll("****");
        System.out.println("脱敏后: " + hiddenText);
    }
}
```



---



| **知识模块** | **核心口诀/关键点**                                          | **典型代码**                       |
| ------------ | ------------------------------------------------------------ | ---------------------------------- |
| **1. 架构**  | **先编译，后复用**。拒绝在循环里用 `String.matches`。        | `Pattern p = Pattern.compile(...)` |
| **2. 语法**  | **Java 双反斜杠**。正则想要 `\`，Java 源码必须写 `\\\\`。    | `Pattern.compile("\\\\d")`         |
| **3. API**   | **find 找子串，matches 拼全力**。group 编号看左括号。        | `while(m.find()) m.group(1)`       |
| **4. 异常**  | **先问 true/false，再取 group**。否则抛 `IllegalStateException`。 | `if(m.find()) { ... }`             |
| **5. 性能**  | **拒绝嵌套量词**。遇到回溯风险，用独占量词 `++`。            | `Pattern.compile("(a++)+b")`       |

