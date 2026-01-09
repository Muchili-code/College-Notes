# Java 包

Java 的**包**(packages) 是对程序的一种组织。

- `import` 关键字用于引入整个库或该库的一个成员

  ```
  import java.util.*;
  import java.util.Vector;
  ```

- 包提供了一种管理“**命名空间**(name space)”的机制

- 静态导入

  ```Java
  // 一般导入
  import java.lang.Math;
  double r = Math.cos(Math.PI * theta);
  
  // 静态导入
  import static java.lang.Math.PI;
  import static java.lang.Math.*;
  double r = cos(PI * theta);
  ```

**和 C/C++, Python 比较**

| 编程语言 | 语法                        | 实现                                                         |
| :------- | :-------------------------- | :----------------------------------------------------------- |
| C/C++    | `#include <stdio.h>`        | 文本插入，编译时只看原型，链接时需要编译后的二进制代码       |
| Java     | `import java.util.Scanner;` | 装载类，用 RTTI 了解类，编译和运行时均需要编译后的二进制代码，会自动编译 |
| Python   | `import Pandas`             | 装载运行 Pandas.py 文件，需要源码可见                        |
