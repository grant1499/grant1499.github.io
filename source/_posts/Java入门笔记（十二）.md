---
title: Java入门笔记（十二）
tags:
  - Java入门
categories:
  - Java
mathjax: true
copyright: true
abbrlink: 92e5de72
date: 2021-05-02 16:29:45
---

## 1.JDK8中新日期时间API

日期时间API的迭代

第一代：jdk 1.0 Date类

第二代：jdk 1.1 Calendar类，一定程度上替换Date类

第三代：jdk 1.8 提出了新的一套API

前两代存在的问题举例

可变性：像日期和时间这样的类应该是不可变的。

偏移性：Date中的年份是从1900开始的，而月份都从0开始。

格式化：格式化只对Date用，Calendar则不行。

此外，它们也不是线程安全的；不能处理闰秒等。

<!--more-->

![image-20210502163607157](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111707.png)

![image-20210502164631141](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111812.png)

```Java
public void APITest() {
        // LocalDate、LocalTime、LocalDateTime的使用
        LocalDate localdate = LocalDate.now();//根基当前时间创建对象
        LocalTime localtime = LocalTime.now();
        //LocalDateTime用的较多
        LocalDateTime localdatetime = LocalDateTime.now();
        System.out.println(localdate);
        System.out.println(localtime);
        System.out.println(localdatetime);
        //根据指定时间创建对象
        LocalDateTime localDateTime = LocalDateTime.of(2021,3,31,15,25,45);
        System.out.println(localDateTime);
        //getXxx()获取对象信息
        System.out.println(localDateTime.getDayOfMonth());
        System.out.println(localDateTime.getDayOfWeek());
        System.out.println(localDateTime.getMonth());
        System.out.println(localDateTime.getMonthValue());
        //withXxx()修改信息并返回新的对象，体现不可变性
        LocalDate localDate = localdate.withDayOfMonth(4);
        System.out.println(localDate);
        System.out.println(localdate);
        //plusXxx(),minusXxx()对当前对象加减相应时间，体现不可变性
        LocalDateTime localDateTime1 = localDateTime.plusDays(12);
        System.out.println(localDateTime);
        System.out.println(localDateTime1);
    }
```

![image-20210502193652086](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111362.png)

Instant类，时间戳。

![image-20210502193732034](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111636.png)

```Java
public void APITest() {
        //now()获取本初子午线对应的标准时间
        Instant instant = Instant.now();
        System.out.println(instant);
        //2021-05-02T11:41:52.974Z
        //添加时间偏移量
        OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(5));
        System.out.println(offsetDateTime);
        //2021-05-02T16:41:52.974+05:00
        //获取自1970年对应的毫秒数
        long milli = instant.toEpochMilli();
        System.out.println(milli);
        //返回毫秒数对应的Instant对象
        Instant instant1 = Instant.ofEpochMilli(1619956303369L);
        System.out.println(instant1);
    }
```

![image-20210502195424494](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111952.png)

## 2.Java比较器（重要）

利用比较器对多个对象进行排序。

Java 实现对象排序的方式有两种：

- 自然排序： java.lang.Comparable

- 定制排序： java.util.Comparator

![image-20210502201039171](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111117.png)

重写compareTo()方法的**一般规则**如上图蓝色字体。

例子：

```Java
public class Goods implements Comparable{
    String name;
    int price;
    public Goods() {
    }
    public Goods(String name, int price) {
        this.name = name;
        this.price = price;
    }
    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void setPrice(int price) {
        this.price = price;
    }
    @Override
    public String toString() {
        return "Goods{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }
    //指明排序方,价格从低到高排序
    @Override
    public int compareTo(Object o) {
        if (o instanceof Goods){
            Goods goods = (Goods)o;
            //方法1
            if (this.price > goods.price){
                return 1;
            }
            else if (this.price < goods.price){
                return -1;
            }else{
                return 0;
            }
            //涉及到String类和包装类的比较建议就直接用实现好的方法
            //方法2，包装类中的compare
            //return Integer.compare(this.price,goods.price);
        }
        throw new RuntimeException("传入的数据类型不一致！");
    }
}
```

```Java
public class CompareTest {
    public static void main(String[] args) {
        //Comparable接口的使用举例
        //像String、包装类等实现了Comparable接口，重写了compareTo()方法
        //给出了比较两个对象大小的方式
        String[] arr = new String[]{"As","cd","sd1","12"};
        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
        //自定义类实现Comparable接口
        //在compareTo(obj)指明如何排序
        Goods[] goods = new Goods[4];
        goods[0] = new Goods("xiaomi",19);
        goods[1] = new Goods("lenovo",25);
        goods[2] = new Goods("apple",199);
        goods[3] = new Goods("dell",35);
        Arrays.sort(goods);
        System.out.println(Arrays.toString(goods));
    }
}
/*[12, As, cd, sd1]
[Goods{name='xiaomi', price=19}, Goods{name='lenovo', price=25}, Goods{name='dell', price=35}, Goods{name='apple', price=199}]*/
```

![image-20210502204316839](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232111527.png)

接上面例子：

```Java
public class CompareTest {
    public static void main(String[] args) {
        String[] arr = new String[]{"As","cd","sd1","12"};
        //从大到小排序
        Arrays.sort(arr,new Comparator()){
            public int compare(Object o1,Object o2){
                if(o1 instanceof String && o2 instanceof String){
                    String s1 = (String)o1;
                    String s2 = (String)o2;
                    return -s1.compareTo(s2);
                }
                throw new RuntimeException("传入的数据类型不一致！");
            }
        });
        System.out.println(Arrays.toString(arr));

        Goods[] goods = new Goods[4];
        goods[0] = new Goods("xiaomi",19);
        goods[1] = new Goods("lenovo",25);
        goods[2] = new Goods("xiaomi",199);
        goods[3] = new Goods("dell",35);
        //先按名称从低到高，再按价格从高到低排序
        Arrays.sort(goods,new Comparator(){
            public int compare(Object o1,Object o2){
                if(o1 instanceof Goods && o2 instanceof Goods){
                    Goods g1 = (Goods)o1;
                    Goods g2 = (Goods)o2;
                    if (g1.getName().equals(g2.getName())){
                        return -Integer.compare(g1.getPrice(),g2.getPrice());
                    }else{
                        return g1.getName().compareTo(g2.getName());
                    }
                }
                throw new RuntimeException("传入的数据类型不一致！");
            }
        });
        System.out.println(Arrays.toString(goods));
    }
}
/*[sd1, cd, As, 12]
[Goods{name='dell', price=35}, Goods{name='lenovo', price=25}, Goods{name='xiaomi', price=199}, Goods{name='xiaomi', price=19}]*/
```

比较

- Comparable接口的方式一旦一定，保证Comparable接口实现类的对象在任何位置都可以比较大小。
- Comparator接口属于临时性的比较。