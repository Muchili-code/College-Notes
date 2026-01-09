# Java 关键字

## this 和 super 

- **直接引用**
- **重名区分**
- **引用构造函数**

### this

```java
//test1.java
class Person {
    private int age = 10;
    public Person(){
    System.out.println("初始化年龄："+age);
}
    public int GetAge(int age){
        this.age = age; //指代成员参数age而不是GetAge的形参age
        return this.age;
    }
}
 
public class test1 {
    public static void main(String[] args) {
        Person Harry = new Person();
        System.out.println("Harry's age is "+Harry.GetAge(12));
    }
}

//output：
//初始化年龄：10
//Harry's age is 12
```

### super

```Java
class Country {
    String name;
    void value() {
       name = "China";
    }
}
  
class City extends Country {
    String name;
    void value() {
    name = "Shanghai";
    super.value();      //调用父类的方法
    System.out.println(name);
    System.out.println(super.name);
    }
  
    public static void main(String[] args) {
       City c=new City();
       c.value();
       }
}
```

**引用构造函数**

```java
class Person { 
    public static void prt(String s) { 
       System.out.println(s); 
    } 
   
    Person() { 
       prt("父类·无参数构造方法： "+"A Person."); 
    }//构造方法(1) 
    
    Person(String name) { 
       prt("父类·含一个参数的构造方法： "+"A person's name is " + name); 
    }//构造方法(2) 
} 
    
public class Chinese extends Person { 
    Chinese() { 
       super(); // 调用父类构造方法（1） 
       prt("子类·调用父类\"无参数构造方法\"： "+"A chinese coder."); 
    } 
    
    Chinese(String name) { 
       super(name);// 调用父类具有相同形参的构造方法（2） 
       prt("子类·调用父类\"含一个参数的构造方法\"： "+"his name is " + name); 
    } 
    
    Chinese(String name, int age) { 
       this(name);// 调用具有相同形参的构造方法（3） 
       prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
    
    public static void main(String[] args) { 
       Chinese cn = new Chinese(); 
       System.out.println("----------"); 
       cn = new Chinese("codersai"); 
       System.out.println("----------"); 
       cn = new Chinese("codersai", 18); 
       System.out.println("----------"); 
    } 
}
```

```
output:
父类·无参数构造方法： A Person.
子类·调用父类”无参数构造方法“： A chinese coder.
--------
父类·含一个参数的构造方法： A person's name is codersai
子类·调用父类”含一个参数的构造方法“： his name is codersai
--------
父类·含一个参数的构造方法： A person's name is codersai
子类·调用父类”含一个参数的构造方法“： his name is codersai
子类：调用子类具有相同形参的构造方法：his age is 18
```

`super` 关键字是一个“编译时”的特殊指令，它<span style="color:#6666FF; text-decoration:underline;">只能出现在 **子类的内部**的非静态方法或构造函数中</span>，用来指明“我要跳过自己的实现，去用我爸爸的实现”。`this` 类似









