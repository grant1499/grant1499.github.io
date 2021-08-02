---
title: Java入门笔记（二十）
abbrlink: 41769e7b
date: 2021-07-25 09:11:43
tags:
  - Java入门
  - 反射
categories:
  - Java
mathjax: true
copyright: true
---

## 1.Java反射机制

<!--more-->

![image-20210727172953297](Java入门笔记（二十）/image-20210727172953297.png)

红色要求重点掌握，其他作为了解内容。

![image-20210727173601290](Java入门笔记（二十）/image-20210727173601290.png)

反射示例。

```Java
// Person.java
public class Person {
    private String  name;
    public int age;

    private Person(String name) {
        this.name = name;
    }
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
    public void show(){
        System.out.println("helloworld");
    }
    private void showNation(String Nation){
        System.out.println(Nation);
    }
}
// ReflectionTest.java
public class ReflectionTest {
    // 反射之前对Person类的操作
    @Test
    public void test1(){
        // 1.创建Person类对象
        Person p1 = new Person("Tom",12);
        // 2.通过对象调用内部的属性、方法
        p1.age = 11;
        System.out.println(p1.toString());
        p1.show();
    }
    // 反射之后对Person类的操作
    @Test
    public void test2() throws Exception {
        Class clazz = Person.class;
        Constructor cons = clazz.getConstructor(String.class,int.class);
        // 1.通过反射创建Person类对象
        Object obj = cons.newInstance("Tom",12);
        Person p = (Person) obj;
        System.out.println(p.toString());
        // 2.通过反射调用对象内部的属性、方法
        // 调用属性
        Field age = clazz.getDeclaredField("age");
        age.set(p,11);
        System.out.println(p.toString());
        // 调用方法
        Method show = clazz.getDeclaredMethod("show");
        show.invoke(p);

        // 通过反射调用Person类的私有结构
        // 调用私有构造器
        Constructor cons1 = clazz.getDeclaredConstructor(String.class);
        cons1.setAccessible(true);
        Object obj1 = cons1.newInstance("Mike");
        Person p2 = (Person) obj1;
        System.out.println(p2);
        // 调用私有属性
        Field name = clazz.getDeclaredField("name");
        name.setAccessible(true);
        name.set(p2,"James");
        System.out.println(p2);
        // 调用私有方法
        Method showNation = clazz.getDeclaredMethod("showNation", String.class);
        showNation.setAccessible(true);
        String nation = (String)showNation.invoke(p2,"IceLand");
        System.out.println(nation);
    }
}
```

## 2.疑问解答

1.通过直接new的方式和反射的方式都能调用类的公共结构，开发中用哪个？

答：一般使用直接new的方式。（明确造哪个类的对象）

当不确定造哪个类的对象时，使用反射，反射具有动态性的特性。

2.反射与OOP中的封装性是否矛盾？

答：不矛盾。

## 3.Class类

Class类定义在java.lang.Class之下。

类的加载过程：程序经过javac命令编译后生成一个或多个字节码(.class)文件，接着用java命令对某个字节码文件解释执行。

解释执行的过程相当于将某个字节码文件加载到内存中，此过程称为类的加载。

加载到内存中的类称为运行时类，作为Class类的一个实例。

**Class的实例就对应着一个运行时类！**

**加载到内存中的运行时类，会缓存一定的时间。在此时间之内，我们可以通过不同的方式来获取此运行时类。**

运行时类在它的生命周期不会重复加载，只有一个。

## 4.获取Class类的实例（四种方法）

前3种方式都需要掌握，最后1种作为了解

```Java
// 获取Class的实例的方式
@Test
public void test3() throws ClassNotFoundException {
    // 前3种方式都需要掌握，最后1种作为了解
    // 方式1：调用运行时类的属性.class
    Class<Person> clazz1 = Person.class;// <Person>可以省略，加了泛型避免被强转
    System.out.println(clazz1);
    // 方式2：通过运行时类的对象，调用getClass()
    Person p1 = new Person();
    Class clazz2 = p1.getClass();
    System.out.println(clazz2);
    // 方式3：调用class的静态方法，forName(String classPath)，需要全类名
    Class clazz3 = Class.forName("Java_05.Person");
    System.out.println(clazz3);

    System.out.println(clazz1 == clazz2);//true
    System.out.println(clazz1 == clazz3);//true
    // 对应同一个运行时类

    // 方式4：使用类的加载器Classloader
    ClassLoader classloader = ReflectionTest.class.getClassLoader();
    Class clazz4 = classloader.loadClass("Java_05.Person");
    System.out.println(clazz4);
}
```

方式3用的比较多。

Class实例可以是哪些结构？

![image-20210730121121036](Java入门笔记（二十）/image-20210730121121036.png)

![image-20210730121243893](Java入门笔记（二十）/image-20210730121243893.png)

## 5.类的加载与ClassLoader的理解

了解内容。

![image-20210730121854721](Java入门笔记（二十）/image-20210730121854721.png)

类加/装载器的作用：

![image-20210730122635446](Java入门笔记（二十）/image-20210730122635446.png)

![image-20210730122814235](Java入门笔记（二十）/image-20210730122814235.png)

例子演示。

使用Classloader加载src目录下的配置文件。
		Properties：用来读取配置文件。

```Java
@Test
public void test2() throws Exception {

    Properties pros =  new Properties();
    //此时的文件默认在当前的module下。
    //读取配置文件的方式一：
    //        FileInputStream fis = new FileInputStream("jdbc.properties");
    //        FileInputStream fis = new FileInputStream("src\\jdbc1.properties");
    //        pros.load(fis);

    //读取配置文件的方式二：使用ClassLoader
    //配置文件默认识别为：当前module的src下
    ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
    InputStream is = classLoader.getResourceAsStream("jdbc1.properties");
    pros.load(is);

    String user = pros.getProperty("user");
    String password = pros.getProperty("password");
    System.out.println("user = " + user + ",password = " + password);
}
```

## 6.创建运行时类的对象（重要）

先写代码。

```Java
Class<Person> obj = Person.class;
Person p = obj.newInstance();// newInstance创建对应的运行时类的对象
// 本质上是调用了空参构造器，也可以调用有参构造器
```

说明：

newInstance():调用此方法，创建对应的运行时类的对象。内部调用了运行时类的空参的构造器。

要想此方法正常的创建运行时 在javabean中要求提供一个public的空参构造器。原因：

1. 便于通过反射，创建运行时类的对象
2. 便于子类继承此运行时类时，默认调用super()时，保证父类此构造器

通过创建运行时类的对象体会反射的动态性。

## 7.获取运行时类的完整结构

作为了解内容，不要求掌握。

我们可以通过反射，获取对应的运行时类中所有的属性、方法、构造器、父类、接口、父类的泛型、包、注解、异常等。。。。

**获取属性**

```Java
@Test
public void test1(){

    Class clazz = Person.class;

    //获取属性结构
    //getFields():获取当前运行时类及其父类中声明为public访问权限的属性
    Field[] fields = clazz.getFields();
    for(Field f : fields){
        System.out.println(f);
    }
    System.out.println();

    //getDeclaredFields():获取当前运行时类中声明的所属性。（不包含父类中声明的属性
    Field[] declaredFields = clazz.getDeclaredFields();
    for(Field f : declaredFields){
        System.out.println(f);
    }
}
```

**获取方法**

```Java
@Test
public void test1(){

    Class clazz = Person.class;

    //getMethods():获取当前运行时类及其所父类中声明为public权限的方法
    Method[] methods = clazz.getMethods();
    for(Method m : methods){
        System.out.println(m);
    }
    System.out.println();
    //getDeclaredMethods():获取当前运行时类中声明的所方法。（不包含父类中声明的方法
    Method[] declaredMethods = clazz.getDeclaredMethods();
    for(Method m : declaredMethods){
        System.out.println(m);
    }
}
```

**获取构造器**

```Java
@Test
public void test1(){

    Class clazz = Person.class;
    //getConstructors():获取当前运行时类中声明为public的构造器
    Constructor[] constructors = clazz.getConstructors();
    for(Constructor c : constructors){
        System.out.println(c);
    }

    System.out.println();
    //getDeclaredConstructors():获取当前运行时类中声明的所的构造器
    Constructor[] declaredConstructors = clazz.getDeclaredConstructors();
    for(Constructor c : declaredConstructors){
        System.out.println(c);
    }
}
```

**获取父类**

```Java
@Test
public void test2(){
    Class clazz = Person.class;

    Class superclass = clazz.getSuperclass();
    System.out.println(superclass);
}
```

**获取运行时类的带泛型的父类**

```Java
@Test
public void test3(){
    Class clazz = Person.class;

    Type genericSuperclass = clazz.getGenericSuperclass();
    System.out.println(genericSuperclass);
}
```

**获取运行时类的带泛型的父类的泛型**

```Java
@Test
public void test4(){
    Class clazz = Person.class;

    Type genericSuperclass = clazz.getGenericSuperclass();
    ParameterizedType paramType = (ParameterizedType) genericSuperclass;
    //获取泛型类型
    Type[] actualTypeArguments = paramType.getActualTypeArguments();
    //        System.out.println(actualTypeArguments[0].getTypeName());
    System.out.println(((Class)actualTypeArguments[0]).getName());
}
```

**获取运行时类实现的接口**

```Java
@Test
public void test5(){
    Class clazz = Person.class;

    Class[] interfaces = clazz.getInterfaces();
    for(Class c : interfaces){
        System.out.println(c);
    }

    System.out.println();
    //获取运行时类的父类实现的接口
    Class[] interfaces1 = clazz.getSuperclass().getInterfaces();
    for(Class c : interfaces1){
        System.out.println(c);
    }

}
```

**获取运行时类所在的包**

```Java
@Test
public void test6(){
    Class clazz = Person.class;

    Package pack = clazz.getPackage();
    System.out.println(pack);
}
```

**获取运行时类声明的注解**

```Java
@Test
public void test7(){
    Class clazz = Person.class;

    Annotation[] annotations = clazz.getAnnotations();
    for(Annotation annos : annotations){
        System.out.println(annos);
    }
}
```

