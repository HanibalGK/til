# 基础理论

## 理论一

谈论面向对象时,主要谈论什么？

1. 什么是面向对象编程和面向对象编程语言

    OOP (Object Oriented Programming) 面向对象编程
        是一种编程范式 / 编程风格。以类或对象为组织代码的基本单元, 将封装、抽象、继承、多态四个特性作为代码设计和实现的基石。
    OOPL (Object Oriented Programming Language) 面向对象编程语言
        支持类或对象的语法机制，并有现成的语法机制，能方便地实现面向对象编程的四大特性的编程语言。

    基础元素 类(class) 对象(object)
    1960年在 Simula 编程语言中第一次使用。
    面向对象编程概念第一次被使用是在 smalltalk 编程语言中。

    1980年 C++ 出现, 带动了面向对象编程的流行。

2. 什么是面向对象分析和面向对象设计

    OOA (Object Oriented Analysis) 面向对象分析
        搞清楚做什么
    OOD (Object Oriented Design) 面向对象设计
        搞清楚怎么做
    OOP (Object Oriented Programming) 面向对象编程
        将分析和设计的结果翻译成代码的过程

    分析 -> 设计 -> 编程

    "面向对象" -> 围绕着对象来做需求分析和设计。 (程序被拆解为那些类，每个类有那些属性方法，类与类之间如何交互等)

3. 什么是 UML? 是否需要 UML

    UML (Unified Model Language) 同一建模语言
        用它来画图表达面向对象或设计模式的设计思路
        图类型:
            - 用例图
            - 顺序图
            - 活动图
            - 状态图
            - 类图
                - 泛化
                - 实现
                - 关联
                - 聚合
                - 组合
                - 依赖等
            - 组件图等

    UML 学习成本高, 沟通成本也不低

## 理论二

封装、抽象、继承、多态分别可以解决哪些编程问题?

1. 封装 (Encapsulation)
    
    信息隐藏或者数据访问保护。
    类通过暴露有限的访问接口，授权外部仅能通过类提供的方式(或者叫函数)来访问内部信息或者数据。

    需要编程语言本身提供一定的语法机制来支持。 (访问权限控制)
    Java:
        private (类本身访问)
        public (任意外部代码都可以直接访问、修改属性)
    
    意义:
        过度灵活意味着不可控, 属性可以随意被修改, 影响代码的可读性、可维护性。
        通过有限的方法暴露必要的操作:
            1. 提高类的易用性
            2. 减少使用者的用错概率

```java
public class Wallet {
 private String id;
 private long createTime;
 private BigDecimal balance;
 private long balanceLastModifiedTime;
 // ... 省略其他属性...

 public Wallet() {
 this.id = IdGenerator.getInstance().generate();
 this.createTime = System.currentTimeMillis();
 this.balance = BigDecimal.ZERO;
 this.balanceLastModifiedTime = System.currentTimeMillis();
 }

 // 注意：下面对 get 方法做了代码折叠，是为了减少代码所占文章的篇幅
 public String getId() { return this.id; }
 public long getCreateTime() { return this.createTime; }
 public BigDecimal getBalance() { return this.balance; }
 public long getBalanceLastModifiedTime() { return this.balanceLastModifiedTim}

 public void increaseBalance(BigDecimal increasedAmount) {
    if (increasedAmount.compareTo(BigDecimal.ZERO) < 0) {
        throw new InvalidAmountException("...");
    }
    this.balance.add(increasedAmount);
    this.balanceLastModifiedTime = System.currentTimeMillis();
 }

 public void decreaseBalance(BigDecimal decreasedAmount) {
    if (decreasedAmount.compareTo(BigDecimal.ZERO) < 0) {
        throw new InvalidAmountException("...");
    }
    if (decreasedAmount.compareTo(this.balance) > 0) {
        throw new InsufficientAmountException("...");
    }
    this.balance.subtract(decreasedAmount);
    this.balanceLastModifiedTime = System.currentTimeMillis();
 }
}
```

2. 抽象 (Abstraction)

    如何隐藏方法的具体实现, 让调用者只需要关心方法提供了哪些功能，并不需要知道功能如何实现的。
    
    借助编程语言提供的 接口类 或 抽象类 这两种语法机制, 实现抽象这一特性。

```java
public interface IPictureStorage {
    void savePicture(Picture picture);
    Image getPicture(String pictureId);
    void deletePicture(String pictureId);
    void modifyMetaInfo(String pictureId, PictureMetaInfo metaInfo);
 }

 public class PictureStorage implements IPictureStorage {
    // ... 省略其他属性...
    @Override
    public void savePicture(Picture picture) { ... }
    @Override
    public Image getPicture(String pictureId) { ... }
    @Override
    public void deletePicture(String pictureId) { ... }
    @Override
    public void modifyMetaInfo(String pictureId, PictureMetaInfo metaInfo) { ...
    }
```
    代码中利用 Java interface 接口语法实现抽象特性。调用者使用图片存储时,只需要了解 IPictureStorage 这个接口类暴露了哪些方法就可以了。

    类的方法时通过编程语言中的 "函数" 这一语法机制来实现的。

    意义:
        1. 关注功能点不关注实现的设计思路, 帮助大脑过滤掉许多非必要的信息。
        2. 作为非常宽泛的设计思想，在代码设计中，起到非常重要的指导作用 (非实现编程、开闭原则、代码解耦等)
        3. 定义类的方法的时候，也要有抽象思维。
            - 不在方法定义中，暴露太多的实现细节

3. 继承 (Inheritance)

    用来表示类之间的 is-a 关系
    模式:
        - 单继承: 一个子类只继承一个父类
        - 多继承: 一个子类可以继承多个父类
    
    意义:
        代码复用
    
    多用组合少用继承

4. 多态 (Polymorphsim)

    子类可以替换父类。
    方式:
        利用 "继承 + 方法重写"
        接口类语法
        duck-typing语法 (Python / JavaScript)

```java
public interface Iterator {
    String hasNext();
    String next();
    String remove();
 }

 public class Array implements Iterator {
    private String[] data;

    public String hasNext() { ... }
    public String next() { ... }
    public String remove() { ... }
    //... 省略其他方法...
 }

 public class LinkedList implements Iterator {
    private LinkedListNode head;

    public String hasNext() { ... }
    public String next() { ... }
    public String remove() { ... }
    //... 省略其他方法...
 }

 public class Demo {
    private static void print(Iterator iterator) {
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }

    public static void main(String[] args) {
        Iterator arrayIterator = new Array();
        print(arrayIterator);

        Iterator linkedListIterator = new LinkedList();
        print(linkedListIterator);
    }
 }
```

    Iterator 时一个接口类, 定义了可以遍历集合数据的迭代器。
    Array 和 LinkedList 都实现了接口类 Iterator.

    print(Iterator iterator) 函数传递对象的时候, 根据对象的实现逻辑, 调用 next()、hasNext() 的实现逻辑

```python
class Logger:
    def record(self):
        print("I write a log into file")

class DB:
    def record(self):
        print("I insert data into db")

def test(recorder):
    recorder.redocrd()

def demo():
    logger = Logger()
    db = DB()
    test(logger)
    test(db)
```

    duck-typing 既不是继承关系，也不是接口和实现的关系。
    两个类具有相同的方法，就可以实现多态，不要求两个类之间有任何关系。 (动态语言所特有的语法机制)

    意义:
        提高了代码的可扩展性
        
## 理论三

面向对象相比面向过程有哪些优势? 面向过程真的过时了吗？

1. 什么是面向过程编程与面向过程编程语言?

面向过程编程:
    以过程(方法、函数、操作) 作为代码的基本单元, 以数据(成员变量、属性) 与方法相分离为最主要的特点。
    面向过程是一种流程化的编程风格，通过拼接一组顺序执行的方法来操作数据完成一项功能。

面向过程编程语言:
    不支持类和对象两个语法概念, 不支持丰富的面向对象编程特性(比如: 封装、继承、多态), 进支持面向过程编程。

举例:

    存在一个文件 user.txt 
    内容: name&age&gender (小王&28&男)
    需求:
        1. 将 user.txt 中的数据格式化成 name\tage\tgender
        2. 按照 age 从小到大排序
        3. 重新写入另一个文本文件 formatted_users.txt

```c
struct User {
    char name[64];
    int age;
    char gender[16];
}

struct User parse_to_user(char* text) {
    // 将 text("小王&28&男") 解析成结构体 struct User
}

char* format_to_text(struct User user) {
    // 将结构体 struct User 格式化成文本 ("小王\t28\t男")
}

void sort_users_by_age(struct User users[]) {
    // 按照年龄从小到大排序 users
}

void format_user_file(char* origin_file_path, char* new_file_path) {
    // open files...
    struct User users[1024];
    int count = 0;
    while(1) {
        struct User user = parse_to_user(line);
        user[count++] = user;
    }

    sort_users_by_age(users);

    for (int i = 0; i < count; ++i) {
        char* formatted_user_text = format_to_text(users[i])
        // write to new file
    }
    // close_files...
}

int main(char** args, int argv) {
    format_user_file("/home/zheng/user.txt", "/home/zheng/formatted_users.txt");
}
```

```java
public class User {
    private String name;
    private int age;
    private String gender;

    public User(String name, int age, String gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public static User parseFrom(String userInfoText) {
       // 将 text("小王&28&男") 解析成结构体 User 
    }

    public String formatToText() {
       // 将结构体 User 格式化成文本 ("小王\t28\t男") 
    }
}

public class UserFileFormatter {
    public void format(String userFile, String formattedUserFile) {
        // open files...
        List users = new ArrayList<>();
        while(1) { // read until file is empty
        // read from file into userText
            User user = User.parseFrom(userText)
            users.add(user);
        }
        // sort users by age...
        for (int i = 0; i < users.size(); ++i) {
            String formattedUserText = user.formatToText();
            // write to new file
        }
        // close files...
    }
}

public class MainApplication {
    public static void main(String[] args) {
        UserFileFormatter userFileFormatter = new UserFileFormatter();
        userFileFormatter.format("/home/zheng/user.txt", "/home/zheng/formatted_users.txt")
    }
}
```

    基本的区别是:
        代码的组织方式不同。
        面向过程被组织成一组方法集合及其数据结构 (struct User) 方法和数据结构的定义是分开的。
        面向对象风格的代码被组织成一组类，方法和数据结构被绑定在一起，定义在类中。

2. 面向对象相比面向过程编程有哪些优势？

   2.1 OOP 更加能够应对大规模复杂程序的开发
        - 对于简单程序来说，不管使用面向过程编程，还是用面向对象编程，差别不会很大。
        - 对于大规模复杂程序开发，整个程序的处理流程错综复杂，并非只有一条主线，回事一个网状结构。
   2.2 OOP 风格的代码更易复用、易扩展、易维护
   2.3 OOP 语言更加人性化、更加高级、更加智能

## 理论四

哪些代码设计看似是面向对象，实际是面向过程的?

    常见编程风格:
        - 面向过程编程
        - 面向对象编程 (主流)
        - 函数式编程

1. 哪些代码设计看似是面向对象，实际是面向过程的？

    a. 滥用 getter、setter 方法 (违反了面向对象编程的封装特性, 相当于将面向对象编程风格退化成了面向过程编程风格)

```java
public class ShoppingCart {
    private int itemsCount;
    private double totalPrice;
    private List<ShoppingCartItem> items = new ArrayList<>();

    public int getItemsCount(){
        return this.itemsCount;
    }

    public void setItemsCount(int itemsCount) {
        this.itemsCount = itemsCount;
    }

    public double getTotalPrice() {
        return this.totalPrice;
    }

    public setTotalPrice(double totalPrice) {
        this.totalPrice = totalPrice;
    }

    public List<ShoppingCartItem> getItems() {
        return this.items;
    }

    public void addItem(ShoppingCartItem item) {
        items.add(item);
        itemsCount++;
        totalPrice += item.getPrice();
    }
    // ... 省略其他方法 ...
}
```
    虽然将内容定义为 private 私有属性, 但是提供了 public 的 getter、setter 方法, 两个属性跟定义为 public 没有什么两样了。
    且 List 方法可以在获取后, 对其中的数据进行 items.clear(), 清空, 导致最终三个字段的内容不一致, 可以调整为 UnmodifiableList<E>

    b. 滥用全局变量和全局方法
        b.1 项目中定义一个庞大的 Constants 类
        影响:
            - 影响代码的可维护性
            - 增加代码的编译时间
            - 影响代码的复用性
        改进方式:
            - 将 Constants 类拆解为功能更加单一的多个类
            - 把常量定义到使用的类中, 提高类设计的内聚性和代码的复用性
        
        b.2 Utils 类
        意义:
            - 两个类相同的功能逻辑,避免代码重复
        方式:
            - 通过继承实现代码复用
            - 定义一个新类, 实现相同的功能
        使用建议:
            - 定义 Utils 类之前, 确认是否真的需要定义一个 Utils 类
            - 是否可以将 Utils 某些方法定义到其他类中
            - 确实需要的话, 细化针对不同的功能, 设计不同的 Utils 类
    c. 定义数据和方法分离的类
        c.1 传统的 MVC 叫做贫血模式, 违背面向对象的编程风格
            - VO、BO、Entity (只会定义数据，不会定义方法，所有的操作逻辑都定义在对应的 Controller、Service、Repository 中) 为典型的面向过程的编程风格
    
    d. 为什么容易写出面向过程风格的代码
        - 面向过程编程风格复合人的流程化思维方式
        - 面向对象编程风格是一种自底向上的思考方式，将任务翻译成一个个小的模块(类), 设计类之间的交互，最后按照流程将类组装起来，完成整个任务。
        - 面向对象编程，类的设计还是需要技巧及一定设计经验。

## 理论5

接口 vs 抽象类的区别？如何用普通的类模拟抽象类和接口

    抽象类和接口时两个常用的语法概念。
    C++ 只支持抽象类，不支持接口
    Python 这类动态编程语言, 既不支持抽象类，也不支持接口


    1. 什么是抽象类和接口？区别在哪里？

定义抽象类

        模板设计模式:
            Logger 记录日志的抽象类
            FileLogger 和 MessageQueueLogger 继承 Logger
            分别实现两种不同的日志记录方式

```java
public abstract class Logger {
    private String name;
    private boolean enabled;
    private Level minPermittedLevel;

    public Logger(String name, boolean enabled, Level minPermittedLevel) {
        this.name = name;
        this.enabled = enabled;
        this.minPermittedLevel = minPermittedLevel;
    }

    public void log(Level level, String message) {
        boolean loggable = enable && (minPermittedLevel.intValue() <= level.intValue())
        if (!loggable) return;
        doLog(level, message);
    }

    protected  abstract void doLog(Level level, String message);
}

// 抽象类的子类: 输出日志到文件
public class FileLogger extends Logger {
    private Writer fileWriter;

    public FileLogger(String name, boolean enable, Level minPermittedLevel, String filepath) {
        super(name, enable, minPermittedLevel);
        this.fileWriter = new FileWriter(filepath);
    }

    @Override
    protected void doLog(Level level, String message) {
        // 格式化 level 和 message, 输出到日志文件
        fileWriter.write(...);
    }
}

// 抽象类的子类: 输出日志到消息中间件 (比如: kafka)
public class MessageQueueLogger extends Logger {
    private MessageQueueClient msgQueueClient;

    public MessageQueueLogger(String name, boolean enable, Level minPermittedLevel, MessageQueueClient msgQueueClient) {
        super(name, enable, minPermittedLevel);
        this.msgQueueClient = msgQueueClient;
    }

    @Override
    protected void doLog(Level level, String message) {
        // 格式化 level 和 message, 输出到消息中间件
        msgQueueClient.send(...);
    }
}
```

    特性:
        - 抽象类不允许被实例化，只能被继承
        - 抽象类可以包含属性和方法
        - 子类继承抽象类, 必须实现抽象类中的所有抽象方法

定义接口:

```java
// 接口
public interface Filter {
    void doFilter(RpcRequest req) throws RpcExcedption;
}

// 接口实现类: 鉴权过滤器
public class AuthencationFilter implements Filter {
    @Override
    public void doFilter(RpcRequest req) throws RpcException {
        // 鉴权逻辑
    }
}
// 接口实现类: 限流过滤器
public class RateLimitFilter implements Filter {
    @Override
    public void doFilter(RpcRequest req) thorws RpcException {
        // 限流逻辑
    }
}

// 过滤器使用 demo
public class Application {
    // filters.add(new AuthencationFilter())
    // filters.add(new RateLimitFilter())
    private List<Filter> filters = new ArrayList<>();

    public void handleRpcRequest(RpcRequest req) {
        try {
            for (Filter filter : filters) {
                filter.doFilter(req);
            }
        } catch (RpcException e) {
            // 处理过滤结果
        }
        // 省略其他处理逻辑
    }
}
```

    接口特性:
        - 接口不能包含属性
        - 接口只能生命方法，方法不能包含代码实现
        - 类实现接口的时候，必须时间接口中声明的所有方法


    抽象类是一种特殊的类, 不能被实例化为对象。

    抽象类是 is-a 的关系

    接口表示 has-a 的关系 (表示具有某些功能) 为了解决解耦问题，隔离接口和具体的实现，提高代码的扩展性。