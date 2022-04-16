# java注解

Annotation：JDK5.0引入

* 可以被其他程序读取
* 用@注释名，还可以添加参数值
* 通过反射来对元注解的访问

## 1. 内置注解

* @Override
* @Deprecated：不推荐使用，过时，存在更好的方式
* @SuppressWarnings("参数")：镇压警告

## 2. 元注解

作用：负责注解其他注解

* @Target：描述注解使用范围
* @Retention：表示需要在什么计界保存该注释（SOURCE<CLASS<RUNTIME）
* @Document：注解包括在javadoc中
* @Inheriter：说明子类可以继承父类中该注解

```java
package com.annotation;

import java.lang.annotation.*;

//测试元注解
@MyAnnotation
public class Test01 {
    @MyAnnotation
    public void test(){

    }
}


//定义一个注解
//@Target 表示注解可以用在哪里地方
//@Retention 表示注解在哪些地方有效
@Documented
@Retention(value= RetentionPolicy.RUNTIME)
@Target(value = {ElementType.METHOD, ElementType.TYPE})
@interface MyAnnotation{

}
```

## 3. 自定义注解

* @interface 来声明注解

# 反射机制

java.Reflection

正常方式：引入需要的包类的名词，通过new 实例化，获得实例化对象

反射方式 ：实例化对象，getClass()方法，得到完整的包类名词

## 1. 获取反射对象

```java
package com.reflection;

//什么叫反射
public class Test01 {
    public static void main(String[] args) throws ClassNotFoundException {
        //通过反射获取类的class对象
        Class c1 = Class.forName("com.reflection.User");
        System.out.println(c1);

        Class c2 = Class.forName("com.reflection.User");
        Class c3 = Class.forName("com.reflection.User");
        Class c4 = Class.forName("com.reflection.User");

        //相同的，因为一个类在内存中只有一个class对象
        //一个类被加载后，类的整个结构都会被封装在class对象中
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4.hashCode());
    }
}

//实体类：pojo，entity
class User{
    private String name;
    private int id;
    private int age;

    public User() {
    }

    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```

## 2. 获取Class类的实例

```java
package com.reflection;

// 测试class类的创建方式有哪些
public class Test02 {
    public static void main(String[] args) throws ClassNotFoundException {
        Person person = new Student();
        System.out.println("这个人是"+person.name);

        //1.通过对象获得Class类的实例
        Class c1 = person.getClass();
        System.out.println(c1.hashCode());

        //2.forname 获得
        Class<?> c2 = Class.forName("com.reflection.Student");
        System.out.println(c2.hashCode());

        //3. 通过类名.class获得
        Class<Student> c3 = Student.class;
        System.out.println(c3.hashCode());

        //4.基本内置类型的包装类都有Type属性
        Class<Integer> i1 = Integer.TYPE;
        System.out.println(i1.hashCode());

        //5.获得父类类型
        Class c5 = c1.getSuperclass();
        System.out.println(c5.hashCode());
    }
}


//实体类：pojo，entity
class Person{
    public String name;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "User{" +
                "name='" + name +
                '}';
    }
}

class Student extends Person{
    public Student(){
        this.name="学生";
    }
}

class Teacher extends Person{
    public Teacher(){
        this.name="老师";
    }
}
```

## 3. 所有类型的Class对象

```java
package com.reflection;

import java.lang.annotation.ElementType;

public class Test03 {
    public static void main(String[] args) {
        Class<Object> c1 = Object.class;
        Class<Comparable> c2 = Comparable.class;
        Class<String> c3 = String.class;
        Class<int[][]> c4 = int[][].class;
        Class<Override> c5 = Override.class;
        Class<ElementType> c6 = ElementType.class;
        Class<Integer> c7 = Integer.class;
        Class<Void> c8 = void.class;
        Class<Class> c9 = Class.class;
    }
}
```

## 4. 获得类的信息

```java
package com.reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

//获得类的信息
public class Test04 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, NoSuchMethodException {
        //Class c1 = Class.forName("com.reflection.User");

        Class c1 = new User().getClass();

        //获得类的名字
        System.out.println(c1.getName()); // 包名+类名
        System.out.println(c1.getSimpleName());//类名

        //获得类的属性
        Field[] fields = c1.getFields(); //只能找到public的属性

        fields=c1.getDeclaredFields(); //获得所有属性
        for (Field field : fields) {
            System.out.println(field);
        }

        Field name = c1.getDeclaredField("name");//获得指定属性
        System.out.println(name);

        //获得类的方法
        c1.getMethods();//获得本类和父类public的方法
        c1.getDeclaredMethods();//获得本类所有私有的方法

        //获得指定方法
        Method getName = c1.getMethod("getName", null);
        Method setName = c1.getMethod("setName", String.class);

        //获得指定的构造器
        c1.getConstructors();//获得public全部构造器
        c1.getDeclaredConstructors();//获得全部构造器

        //获得指定构造器
        Constructor declaredConstructor = c1.getDeclaredConstructor(String.class, int.class, int.class);

    }
}
```

## 5. 动态创建对象执行方法

```java
package com.reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

//动态创建对象，通过反射
public class Test05 {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
        // 获得Class对象
        Class c1 = Class.forName("com.reflection.User");

        //构造一个对象
        User user = (User) c1.newInstance();//本质调用无参构造器
        System.out.println(user);

        //通过构造器创建对象
        Constructor constructor = c1.getDeclaredConstructor(String.class, int.class, int.class);
        User cw = (User) constructor.newInstance("cw", 1, 18);
        System.out.println(cw);

        //通过反射调用方法
        User user3 = (User) c1.newInstance();
        //通过反射获取方法
        Method setName = c1.getDeclaredMethod("setName", String.class);
        setName.invoke(user3, "KB");
        System.out.println(user3.getName());

        //通过反射操作属性
        User user4 = (User) c1.newInstance();
        Field name = c1.getDeclaredField("name");
        name.setAccessible(true);//会变快，关闭检测
        name.set(user4, "LBJ");//不能直接操作私有属性，需要通过上一行的代码
        System.out.println(user4);
    }
}
```

## 6. 反射操作注解

```java
package com.reflection;

import java.lang.annotation.*;
import java.lang.reflect.Field;

//联系反射操作注解
public class Test06 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class c1 = Class.forName("com.reflection.Student2");

        //通过反射获得注解
        Annotation[] annotations = c1.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);//获取注解
        }

        //获取注解的值
        Tablecw tablecw = (Tablecw)c1.getAnnotation(Tablecw.class);
        System.out.println(tablecw.value());

        //获得类指定的注解
        Field f = c1.getDeclaredField("name");
        Fieldcw annotation = f.getAnnotation(Fieldcw.class);
        System.out.println(annotation.columnName());
        System.out.println(annotation.type());
        System.out.println(annotation.length());
    }
}

@Tablecw("db_student")
class Student2{
    @Fieldcw(columnName = "db_id",type = "int",length = 10)
    private int id;
    @Fieldcw(columnName = "db_age",type = "int",length = 10)
    private int age;
    @Fieldcw(columnName = "db_name",type = "String",length = 3)
    private String name;

    public Student2() {
    }

    public Student2(int id, int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student2{" +
                "id=" + id +
                ", age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

@Target(ElementType.TYPE)//作用在类上
@Retention(RetentionPolicy.RUNTIME)
@interface Tablecw{
    String value();
}

//属性的注解
@Target(ElementType.FIELD)//作用在属性上
@Retention(RetentionPolicy.RUNTIME)
@interface Fieldcw{
    String columnName();
    String type();
    int length();
}
```

