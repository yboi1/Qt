## vector的一些基本用法

```c++
头文件:#include<vector>

初始化:  
1.数组：
    arr[10] = {0,1,2,3}
    vector<int>m (arr,arr+4);
2.复制:
    vector<int>n(m);
3.直接赋初值:
	vector<int>x(10);//表示赋给了十个长度，但未初始化。
	vector<int>c(10,1);//赋给了十个长度，且都初始化为1。
一些基本的运用:
头文件:#include<algorithm>
1.sort(a.begin(),a.end());//对a中的元素进行排序。
2.reverse(a.begin(),a.end());//将a中的元素反向，但是不进行排序。

vector的输出：
    vector<int>m = {1,2,3,4};
	for (auto n : m)  //简单快捷！！！
	{
		cout << n << endl;
	}
	for (vector<int>::iterator it = m.begin(); it != m.end(); ++it)
	{
		cout << *it << endl;
	}
    
```

类比二维数组，我们也可以使用vector来进行二维数组的操作：

```c++
//定义
vector<vector<int> >m;
//输入时，需要定义一个新的vector来进行输入数据
//然后再向m中存储vector
```





与迭代器性质类似的函数：

### 1.begin end

```c++
int arr[] = { 1,2,3,4 };
	int* pbeg = begin(arr);  int* pend = end(arr);
	while (*pbeg != *pend)
		cout << *(pbeg++) << endl;
```

当使用end时，可以取消对arr的初始内存分配，使其自动计算，否则可能会对最后面的0进行操作。

也即：**begin-end 得到的数值为数组的最大长度而不是实际长度。**



### 2.vector的嵌套使用

```
void test02()
{
	vector<vector <int>> s;
	vector<int>s1;
	vector<int>s2;
	vector<int>s3;
	vector<int>s4;
	for (int i = 1; i < 11; i++)
	{
		s1.push_back(i + 10);
		s2.push_back(i + 20);
		s3.push_back(i + 30);
		s4.push_back(i + 40);
	}
	s.push_back(s1);
	s.push_back(s2);
	s.push_back(s3);
	s.push_back(s4);
	vector<vector<int>>::iterator sbegin = s.begin();
	vector<vector<int>>::iterator send = s.end();
	while (sbegin != send)
	{
		vector<int>::iterator begin = (*sbegin).begin();
		vector<int>::iterator end = (*sbegin).end();
		while (begin != end)
		{
			cout << *begin<<"  ";
			++begin;
		}
		++sbegin;
		cout << endl;
	}


}
```

### 3.vector 存储对象

```
关键头文件：
#include<string>
#include<vector>
class Maker
{
public:
	Maker(string name,int age)
	{
		this->name = name;
		this->age = age;
	}
public:
	string name;
	int age;
};
//重载右移运算符
ostream& operator<<(ostream &out,Maker &m)
{
	out << "name:" << m.name << " age:" << m.age << endl;
	return out;
}
//传入对象
void test01()
{
	vector<Maker> m;
	Maker m1("悟空",500);
	Maker m2("沙僧", 300);
	Maker m3("八戒", 250);
	Maker m4("唐僧", 50);
	m.push_back(m1);
	m.push_back(m2);
	m.push_back(m3);
	m.push_back(m4);
	auto begin = m.begin();
	auto end = m.end();
	while (begin != end)
	{
		cout << *begin;
		++begin;
	}

}
```

## vector的进阶用法

#### 1.数据结构

​	连续储存空间

#### 2.迭代器

​	随机迭代器

#### 3.vector的增长原理

​	vector在储存数据时，会优先开辟一块较大空间，当要存的数据多余开辟的空间时，会释放掉当前内存空间，重新申请一块更大的空间使用

#### 4.常用的api:

##### 	1.构造(相当于一个类模板)

```
vector <T> m1;
vector(v.begin(), v.end());	//将v[begin(), end())区间中的元素拷贝给本身。
vector(n, elem);			//构造函数将n个elem拷贝给本身。
vector(const vector &vec);	//拷贝构造函数。
```

##### 	2.赋值操作

```
assign(beg, end);	//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);	//将n个elem拷贝赋值给本身。
vector&operator=(const vector  &vec);//重载等号操作符
swap(vec);			// 将vec与本身的元素互换。
```

##### 	3.大小操作

//resize 开辟新空间的同时会初始化元素 即v.size（）会变化

//reverse(len)预留len的长度,但是size不会改变capacity会改变,且开辟的位置不可访问

```
大小操作
size();			 //返回容器中元素的个数
empty();		 //判断容器是否为空
resize(int num); //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
resize(int num, elem);//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长>度的元素被删除。
capacity();		 //容器的容量
reserve(int len);//容器预留len个元素长度，预留位置不初始化，元素不可访问。
```



###### swap:改变容器容量

```
//swap的使用,缩小容量
void test05()
{
	vector<int> v;
	for (int i = 0; i < 100000; i++)
	{
		v.push_back(i);
	}

	cout << "容量:" << v.capacity() << endl;  //多于100000
	cout << "大小：" << v.size() << endl; //100000

	cout << "----------------" << endl;

	v.resize(10);
	cout << "容量:" << v.capacity() << endl; //100000
	cout << "大小：" << v.size() << endl;	  //10

	cout << "----------------" << endl;

	vector<int> v2(v);//调用拷贝构造，容量缩小
	cout << "容量:" << v2.capacity() << endl; //10
	cout << "大小：" << v2.size() << endl;	   //10	
	cout << "-------------------" << endl;
	vector<int>(v).swap(v);//匿名对象和v对象交换    匿名对象先拷贝再交换
	cout << "容量:" << v.capacity() << endl;  //10
	cout << "大小：" << v.size() << endl;	   //10
```



##### 	4.数据存取

```
数据存取操作
at(int idx); //返回索引idx所指的数据，如果idx越界，抛出out_of_range异常。
operator[];//返回索引idx所指的数据，越界时，运行直接报错
front();//返回容器中第一个数据元素
back();//返回容器中最后一个数据元素
```



##### 	5.插入和删除

```
插入和删除操作
insert(const_iterator pos, int count,ele);//迭代器指向位置pos插入count个元素ele.		//count可以省略
push_back(ele); //尾部插入元素ele
pop_back();//删除最后一个元素
erase(const_iterator start, const_iterator end);//删除迭代器从start到end之间的元素
erase(const_iterator pos);//删除迭代器指向的元素
clear();//删除容器中所有元素
```

#### 5.vector的注意事项

1.resize 分配空间时，自动初始化为零

   reverse分配空间时只是预留，不会初始化，为初始化时不可访问

2.需要开辟大空间时，使用reverse提前确定空间，避免多次申请和释放空间
