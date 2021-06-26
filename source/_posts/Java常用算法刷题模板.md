---
title: Java常用算法刷题模板
tags:
  - Java入门
categories:
  - - Java
  - - 算法
mathjax: true
copyright: true
abbrlink: d43df4eb
date: 2021-05-13 21:34:22
---

## Java常用算法刷题模板

参考1：https://www.jianshu.com/p/11158dbc7bde

参考2：https://www.acwing.com/blog/content/593/

<!--more-->

### 输入输出

#### 简单输入

```Java
import java.util.Scanner 
import java.io.BufferedInputStream;

Scanner sc1 = new Scanner(System.in);
Scanner sc2 = new Scanner(new BufferedInputStream(System.in));
// sc2用到缓冲流，读入更快
```

测试：

```Java
public static void main(String[] args) {
    long t1, t2, t3;
    Scanner sc1 = new Scanner(System.in);
    Scanner sc2 = new Scanner(new BufferedInputStream(System.in));

    t1 = System.nanoTime();
    String next = sc1.next();
    t2 = System.nanoTime();
    t3 = t2 - t1;
    System.out.println("sc1:" + t3);
    System.out.println(next);

    t1 = System.nanoTime();
    String next2 = sc2.next();
    t2 = System.nanoTime();
    t3 = t2 - t1;
    System.out.println("sc2:" + t3);
    System.out.println(next2);
}
/*abc
sc1:7968695900
abc
abc
sc2:1945031900
abc*/
```

#### 复杂输入

```undefined
如：1,2,3,4,5
```

```java
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    String str = sc.next();
    String[] split = str.split(",");
    int[] strInt = new int[split.length];
    for (int i = 0; i < split.length; i++) {
        strInt[i] = Integer.parseInt(split[i]);
    }
    System.out.println(Arrays.toString(strInt));
}
/*1,2,3,4,5
[1, 2, 3, 4, 5]*/
```

#### 文件输入

 用于大数据的读入，不用手动输入那么麻烦。BufferedInputStream缓冲流来加速，文件输入流用**绝对路径**，避免不必要的麻烦，要不就把资源文件放在src目录下。

```Java
public static void main(String[] args) throws FileNotFoundException {
    Scanner sc = new Scanner(new BufferedInputStream(new 
    FileInputStream("E:\\input.txt")));
    List<Integer> list = new ArrayList<>();
    while (sc.hasNext()) {
        list.add(sc.nextInt());
    }
    System.out.println(list);
}
```

#### 快速读入

一般我常用的数据输入方法有两种，Scanner和BufferedReader。BufferedReader可以读一行，速度比Scanner快，所以数据较多的时候使用。注意BufferedReader用完记得关。

import建议按需单独导入，最好不要`import xxx.*`全部导入，单独导入能提高编译速度和避免命名冲突。

`import java.io.BufferedReader;`

`import java.io.IOException;`

`import java.io.InputStreamReader;`

应用示例见蓝桥杯十四第一题。

**Scanner**

```Java
import java.util.*;

public class Main{
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(); // String: next(), double: nextDouble()
        int[] nums = new int[n];
        for (int i = 0; i < n; i++)
            nums[i] = scan.nextInt();
        // ...
    }
}
```

**BufferedReader**

```Java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(reader.readLine());
        int[] nums = new int[n];
        String[] strs = reader.readLine().split(" ");
        for (int i = 0; i < n; i++)
            nums[i] = Integer.parseInt(strs[i]);
        // ...
        reader.close(); // 记得关闭
    }
}
```

### 数据类型

#### 字符串

- 按照空格分割字符串

    ```Java
    String[] s=str.split(" +");
    String[] s=str.split("\\s+");
    // 以一个或多个空格分割
    ```

- `public int indexOf(String str)`：返回字符串中第一次出现str的位置；

- `public int indexOf(String str,int fromIndex)`：返回字符串从fromIndex开始第一次出现str的位置；

- `public String substring(int beginIndex)`：返回该字符串从beginIndex开始到结尾的子字符串；

- `public String substring(int beginIndex,int endIndex)`：返回该字符串从beginIndex开始到endsIndex结尾的子字符串；

- `public char[] toCharArray ()`：将此字符串转换为新的字符数组；

- `public String replace (CharSequence target, CharSequence replacement)`：将与target匹配的字符串使用replacement字符串替换；

- `String replaceAll(String regex, String replacement)`：使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。

    ```Java
    String ss=str.replaceAll(" +", ",");
    String ss=str.replaceAll("\\s+", ",");
    // 二者都能把一个或多个空格换成逗号
    ```

#### 大数

```Java
//BigDecimal类型构造方法
BigDecimal(double val)
          //将 double 转换为 BigDecimal，后者是 double 的二进制浮点值准确的十进制表示形式。
BigDecimal(int val)
          //将 int 转换为 BigDecimal。
BigDecimal(long val)
          //将 long 转换为 BigDecimal。
BigDecimal(String val)
          //将 BigDecimal 的字符串表示形式转换为 BigDecimal。
//BigInteger类型构造方法
BigInteger(int val)
          //将 int 转换为 BigInteger。
BigInteger(String val)
          //将 BigDecimal 的字符串表示形式转换为 BigInteger。
//BigInteger转换方法
BigInteger valueOf(long val)
          //返回其值等于指定 long 的值的 BigInteger。
          //如：BigInteger bi1=new BigInteger(1000);
//通用方法
BigInteger(BigDecimal).max(BigInteger(BigDecimal))
    //返回最大数
BigInteger(BigDecimal).min(BigInteger(BigDecimal))
    //返回最小数
BigInteger(BigDecimal).add(BigInteger(BigDecimal))
    //相当于：+
BigInteger(BigDecimal).subtract(BigInteger(BigDecimal))
    //相当于：-
BigInteger(BigDecimal).multiply(BigInteger(BigDecimal))
    //相当于：*
BigInteger(BigDecimal).divide(BigInteger(BigDecimal))
    //相当于：/
BigInteger(BigDecimal).mod(BigInteger(BigDecimal))
    //相当于：%
BigInteger(BigDecimal).abs()
    //返回其值是此 BigIntege(BigDecimal)r 的绝对值的 BigInteger(BigDecimal)。
```

### Calendar类

- Calendar实例化

    `Calendar calendar = Calendar.getInstance();`

- YEAR 指示年的 get 和 set 的字段数字。
- MONTH 指示月份的 get 和 set 的字段数字。
- DATE 指示日的 get 和 set 的字段数字。-
- HOUR get 和 set 的字段数字,指示当天中的某小时
- MINUTE get 和 set 的字段数字,指示当前小时中的某分钟
- SECOND get 和 set 的字段数字,指示当前分钟中的某秒
- DAY_OF_MONTH get 和 set 的字段数字, 指示以月为起点过了几天。
- DAY_OF_WEEK get 和 set 的字段数字, 指示以周为起点过了几天。
- DAY_OF_YEAR get 和 set 的字段数字, 指示以年为起点过了几天。
- DAY_OF_WEEK_IN_MONTH get 和 set 的字段数字, 指示当前月中的第几个星期。

> public static final int SUNDAY = 1;
>
> public static final int JANUARY = 0;
>
> 以上表示，一周的开始是周日，即为1，以此类推。
>
> 一年的开始是一月，即为0，所以对月的设置要 - 1，对月的读取要 + 1

   `get(int field); `// 获取给定字段的值，field即上面的字段数字

```Java
Calendar calendar = Calendar.getInstance();
// 设置日期为: 2020.1.21
calendar.set(Calendar.YEAR, 2020);
calendar.set(Calendar.MONTH, 0);
calendar.set(Calendar.DAY_OF_MONTH, 21);
// 获取2020.1.21是星期几
System.out.print(calendar.get(Calendar.DAY_OF_WEEK));

输出：
3 // 3代表星期二
```

### Calendar类与Date类的转换

#### Date类

```Java
Date date = new Date();
  System.out.println("毫秒:"+date.getTime());//输入毫秒

  //时间转字符串
  SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
  String time = sdf.format(date);
  System.out.println("时间转字符串:"+time);

  //利用字符串来转时间格式（记得抛出异常）
  String time02 = "2018-09-05";
  SimpleDateFormat  sdf2 = new SimpleDateFormat ("yyyy-MM-dd");
  Date date2 = sdf2.parse(time02);
  System.out.println("字符串转时间格式："+date2);
```

#### Calendar类

```Java
 // 使用默认时区和语言环境获得一个日历
  Calendar cal = Calendar.getInstance();
  // 赋值时年月日时分秒常用的6个值，注意月份下标从0开始，所以取月份要+1
  System.out.println("年:" + cal.get(Calendar.YEAR));
  System.out.println("月:" + (cal.get(Calendar.MONTH) + 1));
  System.out.println("日:" + cal.get(Calendar.DAY_OF_MONTH));
  System.out.println("时:" + cal.get(Calendar.HOUR_OF_DAY));
  System.out.println("分:" + cal.get(Calendar.MINUTE));
  System.out.println("秒:" + cal.get(Calendar.SECOND));

  //手动设置某个日期
  Calendar cal02 = Calendar.getInstance();
  //注意，设置时间的时候月份的下标是在0开始的
  //设置时间不一定要这6个参数3个参数也是可以的
  cal02.set(2018,9,1,12,0,0);//二零一八年十月一号十二点
  System.out.println(cal02.getTime());//getTime()返回Date类型
```

#### Calendar转换为Date

```Java
Calendar cal = Calendar.getInstance();
Date date = cal.getTime();
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
String s = simpleDateFormat.format(date);
System.out.println("时间为===="+s);
```

#### Date转换为Calendar

```Java
Date date2 = new Date();
Calendar cal2 = Calendar.getInstance();
cal2.setTime(date2);
System.out.println(cal2.get(Calendar.YEAR) +"-"+(cal2.get(Calendar.MONTH)+1)+"-"+cal2.get(Calendar.DATE));
```

例题：

从键盘输入一个日期，格式为yyyy-M-d

要求计算该日期与1949年10月1日距离多少天

例如：

用户输入了：1949-10-2 程序输出：1

用户输入了：1949-11-1 程序输出：31

```Java
public static void main(String[] args) {
        Calendar cal = Calendar.getInstance();
        Scanner sc = new Scanner(new BufferedInputStream(System.in));
        String s = sc.next();
        String[] str = s.split("-");
        cal.set(Integer.parseInt(str[0]), Integer.parseInt(str[1]) - 1, Integer.parseInt(str[2]));
        Calendar standard = Calendar.getInstance();
        standard.set(1949, 10 - 1, 1);
        long t1 = cal.getTimeInMillis();
        long t2 = standard.getTimeInMillis();
        System.out.println((t1 - t2) / (1000 * 60 * 60 * 24) + 1);
    }
```

### 双端队列

常用定义：

`Deque<Integer> q=new LinkedList<Integer>();`

- LinkedList底层是链表，ArrayDeque底层是数组实现

- `offerFirst / addFirst(Object e);` 队首插入

- `offerLast / addLast(Object e);`队尾插入

- `pollFirst();`队首删除

- `pollLast();`队尾删除

- `peekFirst();`队首获取

- `peekLast();`队尾获取

### 进制转换

#### 其他进制字符串->十进制整数

- public static Integer valueOf(int i)
- public static Integer valueOf(String s)
- public static Integer valueOf(String s, int radix) - radix为s字符串符合的进制

```Java
System.out.println(Integer.valueOf(12));//12
 
// 十进制字符串 => 十进制整数
System.out.println(Integer.valueOf("12"));//12
System.out.println(Integer.valueOf("12",10));//12
 
// 二进制字符串 => 十进制整数
System.out.println(Integer.valueOf("1100", 2));//12
 
// 8进制字符串 => 十进制整数
System.out.println(Integer.valueOf("1100", 8));//576
 
// 16进制字符串 => 十进制整数
System.out.println(Integer.valueOf("1100", 16));//4352
```

同理，用 public static int parseInt(String s)或 public static int parseInt(String s, int radix)转换为int型

```Java
// 十进制字符串 => 十进制整数
System.out.println(Integer.parseInt("12")); // 12
System.out.println(Integer.parseInt("12", 10)); // 12
 
// 二进制字符串 => 十进制整数
System.out.println(Integer.parseInt("1100", 2)); // 12
 
// 8进制字符串 => 十进制整数
System.out.println(Integer.parseInt("1100", 8)); // 576
 
// 16进制字符串 => 十进制整数
System.out.println(Integer.parseInt("1100", 16)); // 4352
 
// 32进制字符串 => 十进制整数
System.out.println(Integer.parseInt("1100", 32)); // 33792
```

#### 十进制整数->其他进制字符串

- public static String toBinaryString(int i) - 转二进制
- public static String toOctalString(int i) - 转八进制
- public static String toHexString(int i) - 转十六进制

```Java
int n = 12;
// 十进制整数 -> 十进制字符串
System.out.println(Integer.toString(n));// 12
 
// 十进制整数 -> 二进制字符串
System.out.println(Integer.toString(n, 2));// 1100
 
// 十进制整数 -> 8进制字符串
System.out.println(Integer.toString(n, 8));// 14
 
// 十进制整数 -> 16进制字符串
System.out.println(Integer.toString(n, 16));// c
 
// 十进制整数 -> 32进制字符串
System.out.println(Integer.toString(n, 32));// c
```

### 自定义排序

```Java
public static void main(String[] args) {
    String[] arr = new String[] {"3", "22", "1111"};
    // 按照长度降序
    Arrays.sort(arr, (s1, s2) -> {
        return s2.length() - s1.length();
    });
    System.out.println(Arrays.toString(arr));
}
```

### 数组求和

- Arrays工具类转stream再求和

    ```Java
    int[] array = new int[]{1,2,3};
    Arrays.stream(array).sum();
    ```

- for loop 求和，虽然代码量比上面的大一点，但是效率比上面的高很多，建议还是使用for loop求和，别搞花里胡哨的

### 其他常用模板

请参考：https://www.acwing.com/blog/content/593/

### 常用刷题API

参考1：https://www.cnblogs.com/chzhyang/p/13494554.html

参考2：https://blog.csdn.net/yubo_830/article/details/109112967

### 一个模板

```Java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.util.Arrays;

class Main {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static PrintWriter pw = new PrintWriter(System.out);
    static int N = 10010;
    static int n;
    public static void main(String[] args) throws IOException {
        n = Integer.parseInt(br.readLine());// 读入一整行
        for (int i = 0; i < n; i++) {
            String s[] = br.readLine().split(" ");
            int x1 = Integer.parseInt(s[0]);
            int y1 = Integer.parseInt(s[1]);
        }

        // ...
        pw.print(res);
        pw.flush();
        pw.close();
        br.close();
    }

class Node {
    int l, r;
    int cnt, len;

    public Node(int l, int r) {
        this.l = l;
        this.r = r;
    }
}

class Seg implements Comparable<Seg> {
    int x, y1, y2, k;

    public Seg(int x, int y1, int y2, int k) {
        this.x = x;
        this.y1 = y1;
        this.y2 = y2;
        this.k = k;
    }

    @Override
    public int compareTo(Seg seg) {
        return this.x - seg.x;
    }
}
```

