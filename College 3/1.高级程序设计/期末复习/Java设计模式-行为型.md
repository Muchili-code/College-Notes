# 第三部分：行为型模式 (Behavioral Patterns)

## 1. 通信逻辑：对象也需要“社交”

课件第 18 页提到：

> **“这些设计模式特别关注对象之间的通信。”**

在复杂的系统中，对象很少是孤立存在的。行为型模式解决的是：谁负责做什么？谁负责通知谁？

它不像“创建型”那样关注对象怎么生出来的，也不像“结构型”那样关注类怎么拼装，它关注的是**“运行时的流程控制”**。

- **核心思想：** 解耦发送者和接收者。让对象之间通过“消息”互动，而不是直接互相写死代码。

## 2. 经典案例：观察者模式 (Observer) 深度拆解

这是面试和实际工作中出现率极高的模式（比如微信朋友圈点赞通知、群消息、股市大盘变动）。

### 课件逻辑还原 (参考课件 22-26 页)

我们将课件中的概念翻译成具体的 Java 运行逻辑。

- **Subject (被观察的主题):** 比如“天气预报站”。它手里捏着重要数据（状态）。
- **Observer (观察者):** 比如“农民”、“上班族”。他们时刻关注天气。
- **ArrayList:** 主题手里的小本本，记着谁关注了它。

### 代码运行流程图解

```Java
import java.util.ArrayList;
import java.util.List;

// 1. 观察者接口 (Observer)
// 就像所有订阅者都必须有的“手机”，用来接收通知
abstract class Observer {
    protected Subject subject;
    public abstract void update();
}

// 2. 主题类 (Subject)
// 就像“公众号”或“UP主”
class Subject {
    // 核心数据 (比如：气温)
    private int state;
    // 【关键结构】：用一个列表存所有的粉丝 (ArrayList)
    private List<Observer> observers = new ArrayList<>();

    // 获取状态
    public int getState() {
        return state;
    }

    // 改变状态 (发布新视频/天气变了)
    public void setState(int state) {
        this.state = state;
        // 【触发点】：一旦状态变了，立马通知所有人
        notifyAllObservers();
    }

    // 订阅 (关注)
    public void attach(Observer observer) {
        observers.add(observer);
    }

    // 【核心通信逻辑】：遍历列表，挨个打电话
    public void notifyAllObservers() {
        // 循环遍历 ArrayList
        for (Observer observer : observers) {
            // 调用观察者的 update() 方法
            observer.update(); 
        }
    }
}

// 3. 具体观察者 (Concrete Observer)
class BinaryObserver extends Observer {
    public BinaryObserver(Subject subject) {
        this.subject = subject;
        this.subject.attach(this); // 自动订阅
    }

    @Override
    public void update() {
        // 收到通知后的反应
        System.out.println("收到通知！Binary String: " + Integer.toBinaryString(subject.getState()));
    }
}
```

**运行逻辑总结：**

1. **注册：** `Client` 创建观察者时，观察者通过 `attach()` 把自己加到 `Subject` 的 `ArrayList` 里。
2. **变化：** `Subject` 执行 `setState(10)`，数据发生改变。
3. **通知：** `setState` 内部自动调用 `notifyAllObservers()`。
4. **遍历：** 循环 `ArrayList`，不分青红皂白，调用每一个元素的 `update()` 方法。

## 3. 生活类比：触发链 (Trigger Chain)

课件第 30 页提到了一个有趣的场景：

> **“需要在系统中创建一个触发链，A对象的行为将影响B对象，B对象的行为将影响C对象……”**

### 大学生校园生活类比：**“期末成绩发布链”**

这是一个典型的**链式反应**，虽然严格来说可能涉及“责任链模式”，但用观察者模式也能很好地解释这种**“一石激起千层浪”**的效果。

1. **源头 (Subject A - 教务系统):**
   - **动作：** 老师录入成绩，点击“发布”。
   - **观察者：** 班级群机器人 (Observer B)。
2. **中间环节 (Subject B - 班级群机器人):**
   - **动作：** 机器人收到系统通知 (update)，自动在群里 @所有人：“成绩出了！”。
   - **观察者：** 学生 (Observer C)。
3. **末端环节 (Subject C - 学生):**
   - **动作：** 学生看到群消息 (update)，去查分。
   - **分支反应：**
     - **学霸 (Observer D1):** 看到满分 -> 触发“申请奖学金”行为。
     - **学渣 (Observer D2):** 看到挂科 -> 触发“联系补考”或“通知家长”行为。

**逻辑：** 教务系统不需要认识家长的手机号，它只通知机器人。机器人只通知学生。学生自己决定下一步做什么。这就是**解耦**。

## 4. 综合总结：七大设计原则与模式映射

你现在已经学完了三类模式中最具代表性的几个。为了帮你融会贯通，我们用**七大设计原则**把它们串起来。

| **设计原则 (Design Principle)** | **一句话解释**                               | **咱们学过的模式体现 (Mapping)**                             |
| ------------------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| **1. 开-闭原则 (Open-Closed)**  | **最重要！** 对扩展开放，对修改关闭。        | **工厂模式**：加新形状只需加新类，不用改工厂逻辑（如果用抽象工厂）。 **观察者模式**：加新观察者只需实现接口，不用改 Subject 代码。 |
| **2. 里氏代换原则 (LSP)**       | 只要父类能出现的地方，子类就能出现。         | **所有模式的基础**。比如 `Shape s = new Circle()`，多态的核心。 |
| **3. 依赖倒置原则 (DIP)**       | 要依赖抽象(接口)，不要依赖具体实现。         | **工厂模式**：Client 只定义 `Shape` 变量，不定义 `Circle` 变量。 |
| **4. 单一职责原则 (SRP)**       | 一个类只干一件事。                           | **观察者模式**：Subject 只管数据变化，Observer 只管接收显示。互不干扰。 |
| **5. 接口隔离原则 (ISP)**       | 接口尽量小，别逼人实现不需要的方法。         | **适配器模式**：之所以要适配，往往就是因为原接口太大或不合适，我们想隔离出好用的部分。 |
| **6. 迪米特法则 (LoD)**         | 最少知道原则。只和朋友说话，不和陌生人说话。 | **代理模式**：Client 不直接找 RealSubject，只找 Proxy 谈。   |
| **7. 合成/聚合复用原则**        | 多用组合(Has-a)，少用继承(Is-a)。            | **适配器/代理模式**：内部都持有另一个对象的实例，而不是去继承它。 |

## 💡 初学者常见问题 (Common Pitfalls)

1. **手里拿着锤子，看什么都是钉子 (Over-design):**
   - *现象：* 写个简单的 "Hello World" 也要搞个工厂，弄个单例。
   - *建议：* 设计模式是为了解决**复杂**问题的。如果只有两个类，直接 `new` 也没关系。只有当变化频繁时，才考虑上模式。
2. **代理模式 vs 适配器模式 vs 装饰器模式 分不清:**
   - 它们结构太像了！
   - **记口诀：**
     - **适配器** = 亡羊补牢 (接口不对，改接口)。
     - **代理** = 经纪人 (接口一样，控制权限)。
     - **装饰器** = 锦上添花 (接口一样，增加功能，比如给画加个边框)。
3. **观察者模式的“内存泄漏”:**
   - *问题：* 如果 `Subject` 活着，`Observer` 就很难被垃圾回收（因为 List 里存着引用）。
   - *解决：* 记得在不需要的时候调用 `detach()` 取消订阅。

## 🎉 毕业挑战

为了验证你的学习成果，请你思考一个问题：

**如果你要设计一个“智能家居系统”：**

1. 当你用手机点击“回家模式”时 (Subject 变化)。
2. 灯光自动打开 (Observer 1)。
3. 空调自动调到 26 度 (Observer 2)。
4. 窗帘自动拉上 (Observer 3)。

你会怎么设计？

A. 在手机的代码里直接写：light.on(); ac.setTemp(26); curtain.close();

B. 让灯光、空调、窗帘都去观察“手机状态”。

(提示：选 B 就是观察者模式，选 A 就是紧耦合。)