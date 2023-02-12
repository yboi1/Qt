**KMP算法：**

Next数组的求取：

```c++
typedef struct SqString
{
	char data[MAXSIZE];
	int length;
};
void GetNext(SqString t,int Next[])//Next数组中第i项的数表示的是在该数之前的最大相同前后缀
{
	int k = -1, j = 0;
	Next[0] = -1;   //将第0项设为-1，是为了防止到达第一项时变成死循环。
	while (j < t.length - 1)
	{
		if (k == -1 || t.data[k] == t.data[j])
		{
			k++, j++;
			Next[j] = k;
		}
		else
		{
			k = Next[k];
		}
	}
}
```









下面是KMP算法的完全代码：

```c++
#include<iostream>
#define MAXSIZE 100
using namespace std;
typedef struct SqString
{
	char data[MAXSIZE];
	int length;
};
void GetNext(SqString t,int Next[])
{
	int k = -1, j = 0;
	Next[0] = -1;
	while (j < t.length - 1)
	{
		if (k == -1 || t.data[k] == t.data[j])
		{
			k++, j++;
			Next[j] = k;
		}
		else
		{
			k = Next[k];
		}
	}
}
int KMPindex(SqString t,SqString p,int Next[]) //与Next数组的初始化极其相似！！
{
	int i = 0, j = 0;
	GetNext(p, Next);
	while (i < t.length && j < p.length)
	{
		if (j==-1||t.data[i] == p.data[j])//j ==-1 此处是关键！！
		{
			i++, j++;
		}
		else
		{
			j = Next[j];
		}
	}
	if (j >= p.length)
	{
		return i - p.length+1;
	}
	else
	{
		return -1;
	}
}
void Init(SqString& t)
{
	int i = 0;
	char c = getchar();
	while (c != '\n')
	{
		t.data[i++] = c;
		//cout << c << endl;
		c = getchar();
	}
	t.length = i;
}
int main()
{
	SqString p,t;
	Init(t);
	Init(p);
	int Next[MAXSIZE];
	GetNext(p, Next);
	cout<< KMPindex(t, p, Next)<<endl;
	
}
```

