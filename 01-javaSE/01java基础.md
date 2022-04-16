# JAVA 基础

JDK, JRE, JVM

# JAVA 开发环境搭建

1. JDK下载与安装

2. 配置环境变量

3. JDK目录介绍

4. Notepad++安装和使用

# 1. JDK下载与安装

JDK8 企业最常用，直接搜JDK8，按操作系统下载

## 1.1 卸载JDK

1. 删除java的安装目录
2. 删除java_home
3. 删除path下关于java的目录
4. java -version

## 1.2 安装JDK8

1. 搜索JDK8的对应版本，安装即可，记住安装对应路径
2. 配合环境
3. 配置路径
4. 测试 java -version

## 1.3 下载IDEA

官网下载

# 2. JAVA基础

## 2.1 注释

单行注释 //

多行注释/* */

平时写代码需要保持规范

## 2.2 标识符

略

## 2.3 数据类型

java是强类型语言：变量必须先定义后才能使用

1. 基本类型（primitive type）: 
   1. 数值类型
      1. 整数类型
         1. byte 占1个字节范围 -128-127
         2. short 占两个字节
         3. int 占4个字节
         4. long 占8个字节 30L 整数后面加个L
      2. 浮点类型
         1.  float 4个字节  3.1415F 小数后面加个F
         2. double占8个字节
   2. boolean： true false
2. 引用类型
   1. 类
   2. 接口
   3. 数组

整数扩展：

* 二进制0b开头
* 十进制
* 八进制 0开头
* 十六进制 0x开头  0~9 A~F 16

浮点数扩展：

* 银行业务：用BigDecimal类来表示
* float 有限的，离散的，存在舍入误差，不要用浮点数来比较

字符扩展：

* 所有的字符 Unicode编码 2个字节 0-65536 

转义字符：

* \t 制表符
* \n 换行
* 很多。。。。

## 2.4 类型转换

低-------------------------------->高

byte, short, char---->int---->long----->float----->double

运算中，先类型转化再运算

(int) i  //强制类型转化，高到低

 自动类型转化，低到高，不需要任何东西，直接赋值

注意：

* 不能对boolean进行转化
* 不能把对象类型转化成不相干的类型
* 高到低需要强制转化
* 转化时候容易出现内存溢出，精度问题

## 2.5 变量

type varName = value ;

修饰符：

* static 
* final： 常量 不会变

变量使用驼峰原则：首字母小写，后续字母大写

常量：全部大写

类名：首字母大写

方法名：首字母小写，驼峰原则 

## 2.6 包机制

包的本质就是文件夹

一般用公司域名的倒置作为包名：

定义包： package 路径.路径等

导入包：import 路径.路径.*

## 2.7 javaDoc

 javaDoc的命令是用来生成自己API文档的

参数信息：

* @author 
* @version
* @since  指明最早使用JDK版本
* @param 参数名
* @return
* @throws 异常抛出情况

在命令行输入 javadoc -encoding UTF-8 -charset UTF-8 文件名.java    //编译成文档 index.html 这就是文档

使用IDEA 生成javaDoc文件

## 2.8 流程控制

###  2.8.1 用户交互Scanner

java.util.Scanner 类

```java
package com.cw.scanner;

import java.util.Scanner;

public class Demo01 {
    public static void main(String[] args) {
        // 创建一个扫描器对象，用于接受键盘数据
        Scanner scanner = new Scanner(System.in);

        System.out.println("使用next方式接受：");

        // 判断用户有没有输入字符串
        if(scanner.hasNext()){
            // 用next的方式接受 不能带有空白，遇到有效字符才开始，遇到空白就结束
            String str = scanner.next();
            System.out.println("输入的内容为"+str);
        }
        scanner.close(); //一定要关掉，否则占用资源
    }
}
```

```java
package com.cw.scanner;

import java.util.Scanner;

public class Demo02 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入数据：");
        // 用nextLine的方式接受 以回车为结束符，可以获得空白
        String str = scanner.nextLine(); //程序等待用户输入完毕
        System.out.println("输入的内容为"+str);
        scanner.close();
    }
}
```

```java
package com.cw.scanner;

import java.util.Scanner;

public class Demo03 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        int i =0;
        float f = 0.0f;
        System.out.println("请输入一个数：");
        if(scanner.hasNextInt()){
            i = scanner.nextInt();
            System.out.println("你输入的数是整数："+i);
        }else {
            f = scanner.nextFloat();
            System.out.println("你输入的是小数："+f);
        }
        scanner.close();
    }
}
```

### 2.8.2 顺序结构

从上到下执行

### 2.8.3 选择结构

1. **if**(){}

2. if(){} else{}

3. if(){}else if(){}else{}

4. switch 

   ```java
   switch(expression){ //变量类型可意识 byte short int char String
       case value:  //case标签必须是字符常量或者字面量
           //语句
           break;//可选
       case value:
           //语句
           break;//可选
       default: //可选
           //语句
   }
   ```

5.  while 循环

6. do...while循环

7. for 循环

8. 增强for循环 

   ```java
   int[] numbers = {10,20,30,40}
   for(int x : numbers){
       //语句
   }
   ```

9. break continue 带标签 :  不建议使用

## 2.9 java方法

保持方法的原子性，尽量一个方法只完成一个功能，方便扩展

### 2.9.1 方法的定义  

就像c++的函数

修饰符 返回值类型 方法名（参数类型 参数名）{

​		方法体

​		return 返回值;

} 

###  2.9.2 方法的重载

一个类中相同的函数名称，但是参数列表必须不相同（个数，类型，排序顺序不同）。

只有返回型不同不行。

### 2.9.3 命令行传参

要靠传递命令给main()函数实现，传递的类似一个数组

### 2.9.4 可变参数

在指定参数类型后加一个省略号 ...

double ... numbers  本质就是数组，也可以传递进去一个数组

* 一个函数的可变参数必须是最后一个参数
* 一个函数只能存在一个可遍参数

### 2.10 数组

定义：相同数据类型的有序集合

数据类型[] 数组名; 

```java
int[] nums; //声明一个数组
nums = new int[10]; //创建一个数组
```

数据类型[] 数组名 = new 数据类型[arraySize] ;

```java
int[] nums = int[10];
int length = nums.length; //获取长度
```

### 2.10.1 初始化

```java
int[] a = {1,2,3} //静态初始化 创建+赋值
int[] b = new int[10] //动态初始化 包含默认初始化 0
```

### 2.10.2 数组的特点

* 长度是确定的，大小不可以改变
* 元素可以是任何数据类型，但必须是相同的类型
* 数组变量属于引用类型，可看成对象，每个元素相当于成员变量

### 2.10.3 数组的使用

```java
int[] arrays={1,2,3,4,5}
for(int array: arrays){
    //语句
}
```

也可以当传入的参数和返回值

### 2.10.4 多维数组

```java
int[][] array = {{1,2},{3,4},{4,5}} // 3行2列
int i = array[2][1] //拿出来用
```

### 2.10.5 Arrays类

```java
package com.cw.array;

import java.util.Arrays;

public class ArrayDemo01 {
    public static void main(String[] args) {
        int[] a = {1,3,5,2,7,9999,12,6666666,45};
        System.out.println(Arrays.toString(a));

        Arrays.sort(a);
        System.out.println(Arrays.toString(a));

        Arrays.fill(a,2,4,0);
        System.out.println(Arrays.toString(a));
    }
}
```

### 2.10.6 稀疏数组

第一行记录 多少行 多少列 多少个值

其他行 横坐标 纵坐标 值

# 3 面向对象编程 OOP

特性：封装，继承，多态

## 3.1 回顾方法

**方法的定义**

```java
package com.oop;

// Demo01 类
public class Demo01 {
    //main方法
    public static void main(String[] args) {}
    /*
    修饰符 返回值类型 方法名(){
        //方法体
        return 返回值;
    }
     */
    public String sayHello(){
        return "hello,world!";
    }
}
```

**方法的调用**

```java
package com.oop;

public class Student {
    // 静态方法 和类一起加载 可以直接被调用
    public static void a(){} 

    // 非静态方法 类实例化之后才存在
    public void say(){
        System.out.println("学生说：你好");
    }
}
```

```java
package com.oop;

public class Demo02 {
    public static void main(String[] args) {
        // 非静态方法需要实例化 new
        Student cw = new Student();
        cw.say();
    }
}
```

## 3.2 类和对象的创建

```java
package com.oop.demo02;

public class Student {
    //属性
    String name;
    int age;
    //方法
    public void study(){
        System.out.println(this.name+"are studying");
    }
}
```

```java
package com.oop.demo02;

//一个项目存在一个main 方法
public class Application {
    public static void main(String[] args) {
        // 实例化一个类
        Student xiaoming = new Student();
        xiaoming.name = "小明";
        xiaoming.study();
    }
}
```

## 3.2 构造器必须要掌握

 ```java
 package com.oop.demo02;
 
 public class Person {
 
     String name;
     int age;
     // 构造器 无参构造 默认存在构造方法 名字与类相同 没有返回值
     // 使用new，本质在调用构造器
     // 用来初始化值
     // alt+insert 自动生成构造器
     public Person(){}
 
     //有参构造 一旦有了有参构造，一定要显式定义无参构造
     public Person(String name){
         this.name=name;
     }
 
     public Person(String name, int age) {
         this.name = name;
         this.age = age;
     }
 }
 ```

## 3.3 总结

* 类是模板，对象是实力
* 方法的定义和调用
* 对象的引用：对象是通过引用来操作的
* 属性：成员变量，默认初始化定： 修饰符 属性类型 属性名=属性值；
  * 数字 0 0.0
  * char u0000
  * boolean false
  * 引用 null
* 对象的创建和使用
  * 使用new关键字 构造器
  * 对象的属性  
  * 对象的方法
* 类：
  * 属性
  * 方法

## 3.4 封装

高内聚，低耦合：类的内部数据操作细节自己完成，不允许外部干涉；仅暴露少量的方法给外部使用

封装：通常禁止直接访问一个对象中数据的实际表示，而应该通过操作接口来访问，称为信息隐蔽。

关键：属性私有 get/set

``` java
package com.oop.demo03;

// 类 private 要通过实例调用
public class Student {

    private String name;
    private int id;
    private char sex;

    //提供一些操作这些属性的方法
    //public get和set的方法
    // alt+insert
    public String getName() {
        return name;
    }

    public int getId() {
        return id;
    }

    public char getSex() {
        return sex;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setId(int id) {
        this.id = id;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }
}
```

## 3.5 继承

继承本质是对某一批类的抽象

extends 扩展，子类是父类的扩展

java只有单继承，没用多继承，私有属性无法继承

继承是类和类之间关系，还有依赖，组合，聚合。

重点：

* object类 所有的类继承object

* super 代表调用父类 ，this代表当前的，私有的属性方法无法调用。
  * new子类的时候，默认调用父类的无参构造，在子类构造器的第一行有个 super()；
  * 继承条件下，只能在子类中使用super()

* 方法重写：只有方法的重写
  * 静态方法的调用只跟定义的类型有关，不算重写
  * 非静态方法才有重写
  * 方法名必须相同，参数名必须相同，修饰符可以扩大但不能缩小
  * 抛出的异常可以被缩小，但不能扩大

## 3.6 多态

动态编译，可扩展性

* 方法的多态，属性没有多态
* 类型转化异常 ClassCastException!
* 存在条件：继承+重写
  * static 方法属于类 不能重写
  * final 是常量，不能重写
  * private 不能重写
* 父类的引用指向子类对象 father f1 = new Son(); 
* instanceof 类型转化，引用类型，判断一个对象是什么类型
  * X instanceof Y 存在父子关系编译就通过
  * X 是 Y 的父子即是true
  * 子类转父类，低转高直接转化（可能丢失方法），反之需要强制转换

```jade
package com.oop;

import com.oop.demo04.Person;
import com.oop.demo04.Student;

//一个项目存在一个main 方法
public class Application {
    public static void main(String[] args) {

        //一个对象的实际类型是确定的
        //可以指向的引用类型就不确定了：父类的引用指向了子类
        Student s1 = new Student(); //可以调用自己和父类的方法
        Person s2 = new Student();//只能调用自己的方法
        Object s3 = new Student();

        s2.run();// 子类重写了父类的方法，执行子类的方法 "run person"
        //s2.eat() 错了 对象能执行的方法看对象左边的类型
        ((Student) s2).eat();
        s1.run();
        s1.eat();
    }
}
```

## 3.7 static关键字详解

静态变量和方法可以通过类名访问

静态代码块：在构造方法前，只执行一次 static{ //代码块}

静态导入包 import java.lang.Math.random;

final 定义的类没有孩子

## 3.8 抽象类

abstract 来修饰一个类 这就是抽象类

```java
package com.oop.demo05;

//抽象类
// 不能new出来
// 只能靠子类实现
// 抽象类中能有普通方法
// 一旦有了抽象方法，这个类就一定是抽象类
public abstract class Action {

    // 抽象方法 只有方法名字 没有实现，子类必须实现其方法，除非该子类也是抽象的方法
    public abstract void doSomething();
}
```

## 3.9 接口

普通类：只有具体的实现

抽象类：具体实现和规范（抽象方法）都有！

接口：只有规范！自己无法写方法~专业的约束，约束和实现分离：面向接口编程

接口的本质是契约 （23种设计模式）

* 约束
* 定义一些方法让不同的人实现
* public static final
* 接口不能被实例化，没有构造方法
* implements 可以实现多继承
* 必须重写接口的方法

```java
package com.oop.demo06;

// 抽象的思维 架构师
// 接口都有一个实现类
public interface UserService {
    //常量 public static final
    int AGE = 99;

    //接口的所有定义都是抽象的 public abstract
    void add(String name);
    void delete(String name);
    void update(String name);
    void query(String name);
}

package com.oop.demo06;

public interface TimeService {
    void timer();
}

package com.oop.demo06;

//类可以实现接口 implements
//实现接口的类，必须重写接口里面的方法
//利用接口实现多继承
public class UserServiceImpl implements UserService, TimeService{
    @Override
    public void add(String name) {

    }

    @Override
    public void update(String name) {

    }

    @Override
    public void query(String name) {

    }

    @Override
    public void delete(String name) {

    }

    @Override
    public void timer() {

    }
}
```

## 3.10 N种内部类

一个类的内部定义一个类

* 内部类可以获得外部类的私有属性
* public static 静态内部类 访问不了
* 匿名内部类，不用将实例保存到变量中间

# 4. 异常机制

异常处理框架：

* 把异常当作对象来处理
  * 错误Error
  * 异常Exception
    * 运行时异常 RuntimeException
      * 数组下标越界
      * 空指针异常
      * 算术异常
      * 丢失资源
      * 找不到类
    * 非运行异常

检查性异常

运行时异常

错误 ERROR：错误不是异常，运行内存不足，栈溢出

## 4.1 抛出和抓取异常

try catch finally throw throws

```jade
package com.exception;

public class Test {
    public static void main(String[] args) {

        int a = 1;
        int b = 0;

        //快捷键 ctrl+alt+t
        try{//try的监控区域
            if(b==0){
                throw new ArithmeticException(); //主动抛出异常，一般在方法中使用
            }
            System.out.println(a / b);
        }catch (Exception e){ //捕获异常 括号里是想要捕获的异常类型
            System.out.println("Exception");
            e.printStackTrace();//打印错误栈信息
        }catch (Throwable e){
            System.out.println("Throwable");
        } finally {//终究会执行，善后工作
            System.out.println("finally");
        }
        // finally 不一定有 一般用来关闭资源
    }
}
```

```java
package com.exception;

public class Test {
    public static void main(String[] args) {

        try {
            new Test().test(1,0);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
        }
    }

    // 假设方法处理不了异常，方法上抛出异常
    public  void test(int a, int b) throws ArithmeticException{
        if(b==0){
            throw new ArithmeticException(); //主动抛出异常
        }
    }
}
```

## 4.2 自定义异常 

自定义异常 继承Exception

1. 创建自定义异常类
2. try catch 
3. 尽量使用finally取释放占用的资源

```java
package com.exception.demo02;

public class Test {
    //可能存在异常的方法
    static void test(int a){
        if(a>10){
            try {
                throw new MyException(a);
            } catch (MyException e) { //捕获
                e.printStackTrace();
            }
        }
        // System.out.println(a+"ok");
    }


    public static void main(String[] args) {
        new Test().test(11);
    }
}
```

# 5. java常用集合框架的使用

集合：对象的容器，实现了对象的常规操作

和数组区别：

* 数组长度固定，集合长度不固定
* 数组可以存储基本类型和引用类型，集合只能存储引用类型

位置：java.util.*

Collection（接口）体系集合：

* List：（接口）有序，有下表，元素可重复，以下是实现类
  * ArraryList
  * LinkedList
  * Vector：不用了
* Set：（接口）无序，无下标，元素不可重复，以下是实现类
  * HashSet
  * SortedSet（接口）
    * TreeSet：（类）

# 5.1 Collection 父接口

特点：代表任意类型的对象，无序，无下标，不能重复。

方法：直接看API帮助文档

* .size()

注意：通过 **[iterator](https://tool.oschina.net/uploads/apidocs/jdk-zh/java/util/AbstractCollection.html#iterator())**() 返回在此 collection 中的元素上进行迭代的迭代器。 用这个来遍历 

	*  .hasNext() 
	*  .next() 
	*  .remove() 

```java
for(Object obj: collection){
    //代码块
}

Iterator it = collection.iterator();
while(it.hasNext()){
    Object obj =it.next();// collection什么类型就用什么类型接受
    //代码块
    //迭代当作 不能用collection.remove(obj)删除元素
    it.remove(); //这样删除则可以
}
```

## 5.1 List 接口

有序的，有下标的，元素可以重复

方法：查看API帮助文档

注意：迭代器多个listIterator()

* 功能可以看帮助文档，可以往前往后删除元素

```java
List list = new ArrayList<>();//创建

ListIterator it =list.listIterator();
```

**List实现类** 

* ArrayList：数组结构实现，查询快，增加删除慢
  * 默认容量大小10，如果没有添加任何元素容量为0，添加任意元素之后容量就为10，每次扩容为原来的1.5倍
  * 存放元素 elementData
  * 实际元素个数：size
  * add() 添加元素

```java
ArrayList nums = new ArrayList<>();//创建
```

* LinkedList：双向链表实现，查询慢，增加删除快

```java
LinkedList nums = new LinkedList<>();//创建
```

## 5.2 泛型

泛型类 泛型接口 泛型方法

<T> 占位符，表示引用类型 

```java
package com.generic.demo01;
//泛型类 类名<T> T表示引用类型 占位符
public class MyGeneric<T>{
    T t; // 创建变量
    
    //作为方法的参数
    public  void  show(T t){
        // T t1=new T(); 这个不能用，因为不能保证存在无参构造
    }
    
    //作为方法的返回值
    public T getT() {
        return t;
    }
}
//测试
//public static void main(String[] args) {
//    //用泛型来创建对象 T一定是具体的类型
//    MyGeneric<String> myGeneric = new MyGeneric<String>();
//}

package com.generic.demo01;



//泛型接口 接口名<T>
//不能泛型静态常量
public interface MyInterface <T>{
    
    T server(T t);// 通过实现类 来实现，
                  // 1.类实现的时候就把类型确定了
                  // 2.用泛型类实现，创建实例的再确定类型     
}


//泛型方法
//语法 public <T> T show(T t){
	// 方法体
	// return t；
//}
```

泛型集合：强制集合元素的类型必须一致

```java
ArrayList<String> str = new ArrayList<String>();//创建泛型集合
Iterator<String> it = str.iterator();
```

## 5.3 Set集合

特点：无序 无下标 元素不可重复 继承自Collection

**Set实现类**

[HashSet](https://tool.oschina.net/uploads/apidocs/jdk-zh/java/util/HashSet.html)重点

* 底层用hash表实现，基于HashCode计算元素存放位置，哈希码相同时候，调用equals，如果相同则重复不存入，否则形成链表。

[TreeSet](https://tool.oschina.net/uploads/apidocs/jdk-zh/java/util/TreeSet.html)

* 存储结构使用红黑树，基于排序实现元素不重复
* 实现了SortedSet接口，对集合元素自动排序
* 元素对象的类型必须实现Comparable接口，指定排序规则
* 通过CompareTo确定是否为重复元素

```java
TreeSet<Person> persons = new TreeSet<>(new Comparator<Person>(){
    @Override //匿名内部类 定义TreeSet的时候就定义了比较规则
    public int compare(Person o1, Person o2){
        return n1.getAge()-n2.getAge();
    }
})
```

## 5.4 Map体系集合

Map特点：

* 键值对
* key：无序，无下标，不允许重复
* value：无序，无下标，允许重复

**HashMap** 重点

线程不安全，运行效率快，允许用null作为key和value

**Hashtable**：不怎么用了

* 其子类 Properties：`Properties` 类表示了一个持久的属性集。`Properties` 可保存在流中或从流中加载。属性列表中每个键及其对应值都是一个字符串。

**TreeMap**

* 存储结构红黑树

## 5.5 Collections工具类

[Collections](https://tool.oschina.net/uploads/apidocs/jdk-zh/java/util/Collections.html)

