---
title: Java入门笔记（十五）
tags:
  - Java入门
  - 集合
categories:
  - Java
mathjax: true
copyright: true
abbrlink: 748a102
date: 2021-06-12 09:16:14
---

## 1.Collection子接口之二：Set接口

Set相比于List和Map来说实际用的较少。

Set 接口是 Collection 的子接口， set 接口没有提供额外的方法。

Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个Set 集合中，则添加操作失败。

Set 判断两个对象是否相同不是使用 == 运算符，而是 根据 equals() 方法。

<!--more-->

`Set`实际上相当于只存储key、不存储value的`Map`。我们经常用`Set`用于去除重复元素。

Set 接口的实现类常用的有： **HashSet、LinkedHashSet和TreeSet** 。

```Java
|----Collection接口：单列集合，用来存储一个一个的对象
*          |----Set接口：存储无序的、不可重复的数据   -->高中讲的“集合”
*              |----HashSet：作为Set接口的主要实现类；线程不安全的；可以存储null值
*                  |----LinkedHashSet：作为HashSet的子类；遍历其内部数据时，可以按照添加的顺序遍历
*                 在添加数据的同时，每个数据还维护了两个引用，记录此数据前一个数据和后一个数据。                   对于频繁的遍历操作，LinkedHashSet效率高于HashSet.
*              |----TreeSet：可以照添加对象的指定属性，进行排序。
```

HashSet最常用。实际上，`HashSet`仅仅是对`HashMap`的一个简单封装。

![image-20210612100053823](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118626.png)

以HashSet为例说明：

**Set的无序性并不等于随机性。**遍历HashSet时不按照定义顺序，但还是有特定顺序。

这里的无序性是指Set的存储不像数组那样按顺序存储，而是分散的，存储位置由数据的hash值决定。

不可重复性：保证添加的元素照equals()判断时，不能返回true.即：相同的元素只能添加一个。

**元素添加过程：(以HashSet为例)**

```Java
我们向HashSet中添加元素a,首先调用元素a所在类的hashCode()方法，计算元素a的哈希值，
此哈希值接着通过某种算法计算出在HashSet底层数组中的存放位置（即为：索引位置，判断
数组此位置上是否已经元素：
    如果此位置上没其他元素，则元素a添加成功。 --->情况1
    如果此位置上其他元素b(或以链表形式存在的多个元素，则比较元素a与元素b的hash值：
        如果hash值不相同，则元素a添加成功。--->情况2
        如果hash值相同，进而需要调用元素a所在类的equals()方法：
               equals()返回true,元素a添加失败
               equals()返回false,则元素a添加成功。--->情况3

对于添加成功的情况2和情况3而言：元素a 与已经存在指定索引位置上数据以链表的方式存储。
jdk 7 :元素a放到数组中，指向原来的元素。
jdk 8 :原来的元素在数组中，指向元素a
总结：七上八下

HashSet底层：数组+链表的结构。（前提：jdk7）hash是数据结构中的知识
```

hashCode方法就是用于计算hash值的。hash值分布越均匀说明hashCode函数设计的越好。

要求：向Set(主要指：HashSet、LinkedHashSet)中添加的数据，其所在的类一定要重写hashCode()和

equals()

要求：重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相等的散列码

*    重写两个方法的小技巧：对象中用作 equals() 方法比较的 Field，都应该用来计算 hashCode 值。

![image-20210612101425782](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118304.png)

LinkedHashSet在添加数据的同时，每个数据还维护了2个引用，记录此数据的前一个数据和后一个数据。

优点：对于频繁的遍历操作，它的效率比HashSet高。

**TreeSet:**

它的一个重要用途就是排序。按照添加对象的指定属性进行排序。

TreeSet中添加的数据要求是**相同类的对象**。

TreeSet 两种排序方法： 自然排序（实现Comparable接口）  和 定制排序（实现Comparator接口） 。默认

情况下， TreeSet 采用自然排序。

自然排序中，TreeSet比较两个对象是否相同的标准：compareTo方法返回0，不再根据equals方法。

```Java
// 自然排序
@Test
public void test5(){
    Set set = new TreeSet();
    set.add(1);
    set.add(24);
    set.add(-34);
    set.add(3);
    Iterator ite = set.iterator();
    while (ite.hasNext()){// 按照从小到大的顺序输出
        // String也是一样的
        System.out.println(ite.next());
    }

    Set s = new TreeSet();
    s.add(new Person("ad",12));
    s.add(new Person("kg",23));
    s.add(new Person("ki",6));
    s.add(new Person("bg",43));

    Iterator iterator = s.iterator();
    while (iterator.hasNext()){// 按照Person的姓名排序
        // 姓名一样只会出现第一个
        System.out.println(iterator.next());
    }
}
/*Person.java*/
import java.util.Objects;
public class Person implements Comparable {
    private String name;
    private int age;

    public Person(){

    }
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
/*...*/
    public int compareTo(Object o){
        if (o instanceof Person){
            Person p = (Person)o;
            return this.name.compareTo(p.name);
        }
        else{
            throw new RuntimeException("输入类型不匹配");
        }
    }
}
```

使用定制排序判断两个元素相等的标准是：通过 Comparator 比较两个元素返回了 0 。

```Java
// 定制排序
@Test
public void test6(){
    Comparator com = new Comparator(){
        // 按照年龄从小到大排序，两个年龄一样返回0，意味着只出现第一个
        public int compare(Object o1,Object o2){
            if (o1 instanceof Person && o2 instanceof Person ){
                Person p1 = (Person)o1;
                Person p2 = (Person)o2;
                return Integer.compare(p1.getAge(),p2.getAge());
            }else{
                throw new RuntimeException("输入的数据类型不匹配");
            }
        }
    };
    Set set = new TreeSet(com);// 传入参数com定制排序
    set.add(new Person("ad",12));
    set.add(new Person("kg",23));
    set.add(new Person("ki",6));
    set.add(new Person("bg",43));
    Iterator ite = set.iterator();
    while (ite.hasNext()){
        System.out.println(ite.next());
    }
}
```

## 2.Collection子接口之三：Map接口

![image-20210612111933894](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118572.png)

Map常用实现类的结构：

```Java
|----Map:双列数据，存储key-value对的数据   ---类似于高中的函数：y = f(x)
*       |----HashMap:作为Map的主要实现类；线程不安全的，效率高；存储null的key和value
*              |----LinkedHashMap:保证在遍历map元素时，可以照添加的顺序实现遍历。
*                    原因：在原的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。
*                    对于频繁的遍历操作，此类执行效率高于HashMap。
*       |----TreeMap:保证照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或定制排序
*                      底层使用红黑树
*       |----Hashtable:作为古老的实现类；线程安全的，效率低；不能存储null的key和value
*              |----Properties:常用来处理配置文件。key和value都是String类型
*
*
*      HashMap的底层：数组+链表  （jdk7及之前)
*                    数组+链表+红黑树 （jdk 8)
```

存储结构的理解:

```Java
>Map中的key:无序的、不可重复的，使用Set存储所的key  ---> key所在的类要重写equals()和hashCode() （以HashMap为例)
>Map中的value:无序的、可重复的，使用Collection存储所的value --->value所在的类要重写equals()
> 一个键值对：key-value构成了一个Entry对象。
>Map中的entry:无序的、不可重复的，使用Set存储所的entry
```

 **始终牢记：Map中不存在重复的key，因为放入相同的key，只会把原有的key-value对应的value给替换掉。**

 **遍历Map时，不可假设输出的key是有序的！**

我们已经知道，`HashMap`是一种以空间换时间的映射表，它的实现原理决定了内部的Key是无序的，即遍历`HashMap`的Key时，其顺序是不可预测的（但每个Key都会遍历一次且仅遍历一次）。

![image-20210612112947789](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118544.png)

HashMap和HashSet一样是无序的。

HashMap与LinkedHashMap的关系与HashSet与LinkedHashSet的关系类似，后者遍历输出顺序按照定义顺

序。

**常用方法:**

```Java
* 添加：put(Object key,Object value)
* 删除：remove(Object key)
* 修改：put(Object key,Object value)
* 查询：get(Object key)
* 长度：size()
* 遍历：keySet() / values() / entrySet()
```

![image-20210612113337366](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118722.png)

```Java
Map map = new HashMap();
map.put(null,1);
map.put(12,"asd");// 增
map.remove(12);// 删
map.put(12,"awe");// 改
System.out.println(map.get(12));// 查

Map map1 = new HashMap();
map1.put("a",12);
map1.put("b",1);
map1.put("a",11);
map.putAll(map1);
System.out.println(map);

map.clear();
System.out.println(map);

// 遍历Map,Map没有迭代器
Map map2 = new HashMap();
map2.put("a",12);
map2.put("b",1);
map2.put("c",11);

// 遍历所有的key，keySet()
Set set = map2.keySet();
Iterator iterator = set.iterator();
while (iterator.hasNext()){
    System.out.println(iterator.next());
}
// 遍历所有的value，values()
Collection values = map2.values();
Iterator iterator1 = values.iterator();
while (iterator1.hasNext()){
    System.out.println(iterator1.next());
}
// 遍历所有的key-value，entrySet(),entrySet集合中的元素都是entry
//映射关系的类型是 Map.Entry 类型，它是 Map 接口的内部接口
for (Object o : map2.entrySet()) {
    Map.Entry entry = (Map.Entry)o;
    System.out.println(entry);//a=12
    System.out.println(entry.getKey()+"-->"+entry.getValue());//a-->12
}
```

**底层原理**

HashMap在jdk7中实现原理:

```Java
HashMap map = new HashMap():
*      在实例化以后，底层创建了长度是16的一维数组Entry[] table。
*      ...可能已经执行过多次put...
*      map.put(key1,value1):
*      首先，调用key1所在类的hashCode()计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置。
*      如果此位置上的数据为空，此时的key1-value1添加成功。 ----情况1
*      如果此位置上的数据不为空，(意味着此位置上存在一个或多个数据(以链表形式存在)),比较key1和已经存在的一个或多个数据的哈希值：
*              如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功。----情况2
*              如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals(key2)方法，比较：
*                      如果equals()返回false:此时key1-value1添加成功。----情况3
*                      如果equals()返回true:使用value1替换value2。
*
*      补充：关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储。
*
*     在不断的添加过程中，会涉及到扩容问题，当超出临界值(且要存放的位置非空)时，扩容。默认的扩容方式：扩容为原来容量的2倍，并将原的数据复制过来。
```

HashMap在jdk8中相较于jdk7在底层实现方面的不同:

```Java
1. new HashMap():底层没创建一个长度为16的数组
2. jdk 8底层的数组是：Node[],而非Entry[]
3. 首次调用put()方法时，底层创建长度为16的数组
4. jdk7底层结构：数组+链表。jdk8中底层结构：数组+链表+红黑树。
4.1 形成链表时，七上八下（jdk7:新的元素指向旧的元素。jdk8：旧的元素指向新的元素）
4.2 当数组的某一个索引位置上的元素以链表形式存在的数据个数 > 8 且当前数组的长度 > 64时，此时此索引位置上的所数据改为使用红黑树存储。
```

LinkedHashMap的底层原理作为了解内容。

![image-20210612130347106](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118121.png)

**TreeMap:**

![image-20210612143329084](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232118651.png)

TreeMap用到的还是比HashMap少。

使用`TreeMap`时，放入的Key必须实现`Comparable`接口。`String`、`Integer`这些类已经实现了`Comparable`接口，因此可以直接作为Key使用。作为Value的对象则没有任何要求。

```Java
// 向TreeMap中添加key-value，要求key必须是用一个类创建的对象
// 因为要按照key进行排序：自然排序 、定制排序
// 排序类似TreeSet
TreeMap treemap = new TreeMap();
Person p1 = new Person("tom",13);
Person p2 = new Person("jordan",43);
Person p3 = new Person("jack",27);
treemap.put(p1,48);
treemap.put(p2,78);
treemap.put(p3,75);

// 自然排序
Set set = treemap.entrySet();
for (Object o:set){
    Map.Entry entry = (Map.Entry)o;
    System.out.println(entry.getKey()+"-->"+entry.getValue());
}

// 定制排序
TreeMap treeMap = new TreeMap(new Comparator() { // 匿名内部类写法
    @Override
    public int compare(Object o1, Object o2) {
        if (o1 instanceof Person && o2 instanceof Person){
            Person p1 = (Person)o1;
            Person p2 = (Person)o2;
            return Integer.compare(p1.getAge(),p2.getAge());
        }
        else{
            throw new RuntimeException("输入数据类型不匹配");
        }
    }
});

Person p4 = new Person("tom",13);
Person p5 = new Person("jordan",43);
Person p6 = new Person("jack",27);
treeMap.put(p4,48);
treeMap.put(p5,45);
treeMap.put(p6,56);

Set set1 = treeMap.keySet();
Iterator iterator = set1.iterator();
while (iterator.hasNext()){
    System.out.println(iterator.next());
}
```

## 3.Collections工具类

常用方法：均为`static`方法

```Java
reverse(List)：反转 List 中元素的顺序
shuffle(List)：对 List 集合元素进行随机排序，洗牌算法
sort(List)：根据元素的自然顺序对指定 List 集合元素升序排序
sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换，i、j为下标索引
Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
    
Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
Object min(Collection)
Object min(Collection，Comparator)
int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
void copy(List dest,List src)：将src中的内容复制到dest中
boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所旧值
eg.
List list = new ArrayList();
list.add(1);
add XXX
Collections.reverse(list);
特别说明copy方法，dest不能比src更小，标准写法如下：
List list = new ArrayList();
list.add(1);
add XXX
List dest = Arrays.asList(new Object[list.size()]);// 标准写法
Collections.copy(dest,list);
```

Collections 类中提供了多个 `synchronizedXxx ()` 方法，该方法可使将指定集合包装成线程同步的集合，从而

可以解决多线程并发访问集合时的线程安全问题。

说明：ArrayList和HashMap都是线程不安全的，如果程序要求线程安全，我们可以将ArrayList、HashMap

转换为线程的。使用`synchronizedList(List list)` 和 `synchronizedMap(Map map)`。

`List list1 = Collections.synchronizedList(list)`，返回的list1是线程安全的。