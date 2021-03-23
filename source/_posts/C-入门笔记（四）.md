---
title: C++入门笔记（四）
date: 2021-03-23 12:39:58
tags: [C++入门]
categories: 
	- C++
mathjax: true
copyright: true
---

## 1.静态成员

### 1.1静态数据成员

在某些情况下，我们需要在类中使用作用类似全局变量的变量，这时会用到静态数据成员。

静态数据成员类似于Java中的类变量（静态变量），随类一起加载。

<!--more-->

OOP中有着”类属性“的概念。**也就是说，如果某个属性为整个类所共有，不属于任何一个具体的对象，则采用static关键字修饰。**

静态成员在每一个类只有一个副本，有该类的所有对象共同维护和使用，从而实现同一类不同对象的数据共享。

- 如何初始化静态数据成员？

它的初始化应在类外单独进行，而且应在定义对象之前。一般在main函数之前和定义对象之后初始化。

语法：`int Student::count = 0;` --> 数据类型   类名：：静态数据成员名   =   初始值；

- 如何访问静态数据成员？

1.类名：：静态数据成员名

2.对象名.静态数据成员名

3.对象指针->静态数据成员名

注意：私有静态数据成员不能在类外直接访问，必须通过公有的（静态）成员函数访问。

**示例**

```c++
class Student{
	public:
		Student(){
			cout << "请输入学生的学号、姓名、成绩：\n";
			cin >> id >> name >> score;
			stuNum ++;
			sumScore += score;
			averScore = sumScore/stuNum;
		}
		void showIfmation(){
			cout << id +" "+name << " " << score << endl;
		}
		static void showsumScore(){
			cout << stuNum<<"个学生的总分为:"<<sumScore << endl;
		}
		static void showaverScore(){
			cout << stuNum<<"个学生的平均为:"<<averScore << endl;
		}
		~Student(){
			stuNum--;
			sumScore -= score;
			averScore = sumScore/stuNum;
		}
	private:
		string name,id;
		double score; 
		static int stuNum;
		static double sumScore,averScore;
};
// 静态数据成员初始化 
int Student::stuNum = 0;
double Student::sumScore = 0.0;
double Student::averScore = 0.0;
int main()
{
	Student s1,s2,s3;
	cout << "学生信息为：\n";
	s1.showIfmation();
	s2.showIfmation();
	s3.showIfmation();
	Student::showsumScore();
	s1.showsumScore();// 两种访问静态数据成员方式
	Student::showaverScore();
	s3.showaverScore();
	return 0;
}
/*请输入学生的学号、姓名、成绩：
101 张三 95
请输入学生的学号、姓名、成绩：
102 李四 90
请输入学生的学号、姓名、成绩：
103 王五 85
学生信息为：
101 张三 95
102 李四 90
103 王五 85
3个学生的总分为:270
3个学生的总分为:270
3个学生的平均为:90
3个学生的平均为:90*/
```

## 1.2静态成员函数

## 1.3友元函数

## 1.4类的组合

```c++
class Score{
	public:
		Score(double c = 0,double m = 0,double e = 0);
	void show();
	private:
		double chinese,math,english;
};
Score::Score(double c,double m,double e):
	chinese(c),math(m),english(e){
	}
void Score::show(){
	cout << "chinese:"<<chinese << " ";
	cout << "math:"<<math << " ";
	cout << "english:"<<english<<endl;
}

class Student{
	public:
		Student(string n,string i,double a,double b,double c);
		void show();
	private:
		string name,id;
		Score s;
};
Student::Student(string n,string i,double a,double b,double c):
	name(n),id(i),s(a,b,c){ // 注意这里的初始化列表
	}
void Student::show(){
	cout << "name:" << name << " ";
	cout << "id:" << id << " ";
	s.show();
}
int main()
{
	Student s1("小明","1001",97,78,81);
	s1.show();
	return 0;
}
```

