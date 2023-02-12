# STL

## 1.STL的介绍

 	STL(Standard TemplateLibrary,标准模板库)

#### 六大容器：

​	1.容器：vector等存储容器

​	2.算法： 栈，队列，二叉树，排序算法

​	3.迭代器：扮演了容器与算法之间的胶合剂（类似于指针等）

​	4.仿函数：算法的升级进一步完善，实现更多的功能

​	5.适配器：可以完成函数参数的适配，原本传入两个，现在可以传入三个

​	6.空间适配器：编译器根据需要的空间大小，自动申请和释放，无需担心内存泄漏等问题

> ​	容器，算法，迭代器，仿函数，适配器，空间配置器
>
> ​	容器：各种数据结构
>
> ​	算法：各种常用的算法（冒泡，排序）
>
> ​	迭代器：扮演了容器与算法之间的胶合剂（类似于指针等）
>
> ​	仿函数：行为类似函数，可作为算法的某种策略
>
> ​	适配器：一种用来修饰容器或者仿函数或迭代器接口的东西
>
> ​	空间配置器：负责空间的配置与管理
>
> STL六大组件的交互关系，容器通过空间配置器取得数据存储空间，算法通过迭代器存储容器中的内容，仿函数可以协助算法完成不同的策略的变化，适配器可以修饰仿函数。

## 2.三大主件

**容器：**序列式容器和关联式容器

​	序列式容器：用户根据容器的规则进行数据的储存

​	关联式容器：编译器根据用户的规则进行数据的存储

**算法：**质变算法和非质变算法

质变算法：运算中会改变区间元素的算法（拷贝，插入，删除）

非质变算法：运算中不会改变区间元素的算法（查找）

**迭代器：**重点学习双向迭代器和随机访问迭代器

双向迭代器：++，--可以实现访问上一个元素和下一个元素

随机访问迭代器：+2 可以实现直接访问下两个元素



#### 数组容器

```
template<class T>
class MyArrey
{
public:
	typedef T* itrator;  //给T*取别名，保护数据类型
	MyArrey(int mCapacity)
	{
		mSize = 0;
		this->mCapacity = mCapacity;
		p = new T[mCapacity];
		for (int i = 0; i < mCapacity; i++)
		{
			p[i] = i + 10;
			++mSize;
		}
		
	}
	void print()
	{
		for (int i = 0; i < mSize; i++)
		{
			cout << p[i] << " ";
		}
	}
	T* begin()
	{
		return p;
	}
	T* end()
	{
		return p + mSize;
	}
	~MyArrey()
	{
		if (p != NULL)
		{
			delete p;
			p = NULL;
		}
	}
public:
	T* p;
	int mSize;
	int mCapacity;
};
template<class T>  //实现print与类的分离
void print(T* begin,T* end)
{
	for (; begin != end; begin++)
	{
		cout <<*begin<< " ";
	}
}
void test01()
{
	MyArrey<int> m1(10);
	MyArrey<int>::itrator begin = m1.begin();
	MyArrey<int>::itrator end= m1.end();
	print(begin, end);

}
```



#### vector容器

​	具体内容参考vector的使用



#### String容器

> 1.数据结构：连续的存储空间，用一个char*指向这片空间
>
> 2.迭代器:随机访问迭代器
>
> 3.常用的api：
>
> ​	1.构造
>
> ​	2.基本赋值
>
> ​	3.存取字符
>
> ​	4.拼接
>
> ​	5.查找和替换
>
> ​	6.比较
>
> ​	7.子串
>
> ​	8.插入和删除
>
> ​	9.string和const char*转换
>
> 4.常用的api中的注意:
>
> ​	1.[]和at区别：[]如果越界，不抛异常，直接挂。at会抛异常
>
> ​	2.字符串内存重新分配，[]和at获取的字符引用，再次使用时，可以能会出错
>
> ​	3.string和char *的区别：string是一个类，char *是一个指针

​	1.构造

> 构造函数
> string();//创建一个空的字符串 例如: string str;
> string(const string& str);//使用一个string对象初始化另一个string对象
> string(const char* s);//使用字符串s初始化
> string(int n, char c);//使用n个字符c初始化

```
string m;
```

​	2.基本赋值

> 基本赋值操作
> string&operator=(const char* s);//char*类型字符串 赋值给当前的字符串
> string&operator=(const string&s);//把字符串s赋给当前的字符串
> string&operator=(char c);//字符赋值给当前的字符串
> string& assign(const char *s);//把字符串s赋给当前的字符串
> string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
> string& assign(const string&s);//把字符串s赋给当前字符串
> string& assign(int n, char c);//用n个字符c赋给当前字符串
> string& assign(const string&s, int start, int n);//将s从start开始n个字符赋值给字符串,如s=hello,那么n=3,start=1，那么是hel中从e开始赋值3-1个字符

```
string m = "hello world"
string m1 = ("hello");
string m2(m1);
string m3(10,'a');
string m4(m1,2,3); 
//从m1的第二位开始，共三位赋值给m4，若长度不够则赋值到末尾结束
```

​	3.存取字符

> char&operator[](int n);//通过[]方式取字符
> char& at(int n);//通过at方法获取字符

区别：at越界会抛出异常catch(out_of_range)，m[i]不会

```
string m = "hello world";
	m[i] // 读取第i+1位  下标从零开始
	m.at(i) //读取第i+1位
```

```
void test01()
{
	string a = "hello world";
	try
	{
		//cout << a[100];
		cout << a.at(100);
	}
	catch (out_of_range& ex)
	{
		cout << "out of range" << endl;
	}
	
}
```

​	4.拼接

> 拼接操作
> string&operator+=(const string& str);//重载+=操作符
> string&operator+=(const char* str);//重载+=操作符
> string&operator+=(const char c);//重载+=操作符
> string& append(const char *s);//把字符串s连接到当前字符串结尾
> string& append(const char *s, int n);//把字符串s的前n个字符连接到当前字符串结尾
> string& append(const string&s);//同operator+=()
> string& append(const string&s, int pos, int n);//把字符串s中从pos开始的n个字符连接到当前字符串结尾
> string& append(int n, char c);//在当前字符串结尾添加n个字符c

```
	string m1 = "hello";
	string m2 = m1.append("ab");
	string m3 = m1.append(5, 'a');
	cout << m3 << endl;
```

​	5.查找和替换

(找到返回位置，未找到返回-1)

```
	string m1 = "myworld";
	int pos = m1.find("w");
```

​	6.比较

1.参考字典序比较，>时返回1，<时返回-1，=时返回0；

2.A小于a

```
string m1 = "myworld";
	int pos = m1.find("w");
	string m2 = "an";
	cout << m2.compare(m1) << endl;
```

​	7.子串

> 子串
> string substr(int pos = 0, int n = npos) const;//返回由pos开始的n个字符组成的字符串

​	8.插入和删除

​	9.string和const char*转换



## 3.Stack

栈：后进先出

```
stack <int> s1;
s1.top();
s1.pop();
s1.push();
```



## 4.Queue

队列：先进先出

```
queue <int> q1;
q1.front();
q1.back();
q1.pop();
q1.push()
```



## 5.List

#### **1.构造**

```
list<T> lstT;
list(begin,end);
list(n,elem);
list(const list &lst);
```

#### 2.数据元素的插入和排序

```
insert (pos,elem);
insert(pos,n,elem);
insert(begin,end);
push_back(elem);
push_front(elem);
pop_back();
pop_front();
clera(); //清除所有
erase(begin,end);
erase(pos);
remove(elem); //移除所有的elem元素
```

#### 3.大小操作

```
size();
empty(); //为空时为真
resize(num); //重新指定容器的长度
resize(num,elem); //重新分配长度，变长时初始化为elem
```

#### 4.赋值操作，数据的存取

```
assign(begin,end);
assign(n,elem); //将n个elem拷贝复制到本身
list&operator=(const list&lst);
swap(lst); //将lst与本身的元素交换
front();//返回第一个元素
back(); //返回最后一个元素
```

#### 5.翻转和排序

```
sort();  //排序
reverse(); //翻转
```

## 6.pair对组

1.pair对组是一个模板类,类里面有两个公有成员变量

2.适用于一个成员中有两个编号时使用，

```
pair<int,string> m1(18,"悟空");
cout<<m1.first<<" "<<m1.secend<<endl;
```

## 7.set容器和multiset容器

### set容器

1.set容器是一个关联式容器，容器自身有规则。

2.set容器中一个对象只有一个值，该值即为实值，也为键值，但是容器中不允许存在两键值相同的元素

3.数据结构：平衡二叉树

> 平衡二叉树:查找极快，从根节点来看，左边的数值都为大于根节点的数，右边都为小于根节点的数，以此类推!

4.set容器中的数据会默认的从小到大排序

```
//改变set容器的规则，变为降序（从大到小）
void test02()
{
	set<int, myfunc> s;
	s.insert(4);
	s.insert(8);
	s.insert(2);
	s.insert(10);
	s.insert(7);

	printSet2(s);
}
```

5.判断set容器中的数据是否插入成功

插入元素时，插入相同的元素不会报错，但是不会插入到容器中

```
set<int>m1;
	m1.insert(10);
	pair<set<int>::iterator, bool> set = m1.insert(10);
	if (set.second)
	{
		cout << "插入成功" << endl;
	}
	else
	{
		cout << "插入失败" << endl;
	}
	m1.insert(10);
```

6.对象在set中的应用

```
struct func
{
	bool operator()(Maker m1, Maker m2) const //需要加1const否则报错
	{
		return m1.age < m2.age;
	}
};
void printSet(set<Maker,func>& m)
{
	for (set<Maker,func>::iterator it = m.begin(); it != m.end(); it++)
	{
		cout << "name:" << (*it).name << "age:" << (*it).age << endl;
	}
}
//倒叙存入

void test02()
{
	Maker m1("悟空", 18);
	Maker m2("沙僧", 20);
	Maker m3("唐僧", 19);
	set<Maker,func> m;  //需要自行为set容器增加一个排序规则
	m.insert(m1);
	m.insert(m2);
	m.insert(m3);
	printSet(m);
}
```

## 8.map容器

1.map插入元素操作

```
map<int,string>m;
m.insert(pair<int,string>(18,"悟空"));
m.insert(make_pair(19,"唐僧"));
m.insert(map<int,string>::value_type(2,"八戒")) ;
m[4] = "沙僧"
```

2.查找

find():  查找指定值，返回迭代器

count():查找指定值的个数，multimap中可以大于一，map中只有1或0；

lower_bound(elem);  大于等于elem的最小值

upper_bound(elem);  大于elem的最小值

equal_bound(elem);  返回容器中大于等于elem的两个最小值

```
查找
find(key);//查找键key是否存在,若存在，返回该键的元素的迭代器；/若不存在，返回map.end();
count(keyElem);//返回容器中key为keyElem的对组个数。对map来说，要么是0，要么是1。对multimap来说，值可能大于1。
lower_bound(keyElem);//返回第一个key>=keyElem元素的迭代器。
upper_bound(keyElem);//返回第一个key>keyElem元素的迭代器。
equal_range(keyElem);//返回容器中key与keyElem相等的上下限的两个迭代器。
```

## 9.Stl的深浅拷贝

1.把对象放入容器中实际上是拷贝了一份

2.注：

​	1.拷贝构造要能够被调用

​	2.注意深浅拷贝问题，当传入空间时，构造函数要重新分配空间

## 10.员工案例

1.创建

```
#include<iostream>
#include<vector>
#include<map>
#include<string>
#include<ctime>
using namespace std;
//1.抽象员工
class Woker
{
public:
public:
	int Id;
	string Name;
	int Salary;
};
void Creat_Woker(multimap<int, Woker>&vWoker)
{
	string Tname = "ABCDEF";
	Woker temp;
	srand((unsigned int)time(NULL));
	for (int i = 0; i < 6; ++i)
	{
		temp.Name = "成员";
		temp.Name += Tname[i];
		temp.Id = rand() % 3 + 1;
		temp.Salary = rand() % 20000 + 20000;
		vWoker.insert(make_pair(temp.Id, temp));
	}
	
}
void printWoker(multimap<int, Woker>& vWoker,int id)
{
	multimap<int, Woker>::iterator it = vWoker.find(id);
	int num = vWoker.count(id);
	int temp = 0;
	for (; it != vWoker.end()&&temp<num; ++it,++temp)//迭代器最好是加临时变量
	{
		cout << "Name:" << it->second.Name << " Salary:" << it->second.Salary << " id:" << it->second.Id << endl;
	}
}
void test()
{
	multimap<int,Woker>vWoker;
	Creat_Woker(vWoker);
	cout << "销售部(1)如下:" << endl;
	printWoker(vWoker, 1);
	cout << "策划部(2)如下:" << endl;
	printWoker(vWoker, 2);
	cout << "财经部(1)如下:" << endl;
	printWoker(vWoker, 3);

}
int main()
{
	test();
}
```

## 11.函数对象

概念：定义一个类，类中重载了"()"，使其有了仿函数的功能。

使其调用对象函数

<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230107182513870.png" alt="image-20230107182513870" style="zoom: 50%;" />
