#  Java 中的正则表达式

Java 在 `java.util.regex` 包中提供支持。

## 4.1 String 类

- `boolean matches(String regex)`: 告诉这个字符串是否匹配给定的正则表达式。

```java
String phone = "18868111057 ";
boolean flag = phone.matches("1[0-9]{7}")
```



## 4.2 核心类

1. **Pattern 类**:
   - 正则表达式的编译表示。
   - 无公共构造函数，通过 `Pattern.compile(String regex)` 创建。
2. **Matcher 类**:
   - 执行匹配操作的引擎。
   - 通过 `pattern.matcher(CharSequence input)` 获得。
3. **PatternSyntaxException**:
   - 非强制异常 (Unchecked Exception)，指示正则语法错误。

## 4.3 Matcher 常用方法分类 (PPT 提及)

- **索引方法 (Index Methods)**: `start()`, `end()` - 查看匹配位置。
- **查找方法 (Search Methods)**: `lookingAt()`, `find()`, `matches()` - 检查是否存在匹配。
- **替换方法 (Replacement Methods)**: `replaceAll()`, `replaceFirst()`, `appendReplacement()`, `appendTail()` - 替换文本。