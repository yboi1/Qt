## try语句 与 throw语句的使用：

头文件：#include<stdexcept>

throw 可以实现，当条件不满足预期时，使程序报错。

而try语句中的catch 则恰好是接住throw 报出的错误，然后针对其错误进行相应的语句和操作的。有点类似于switch的用法

下面这个程序是判断分母是否为零

其中运用了goto语句!

```c++
#include<iostream>
#include<stdexcept>
using namespace std;
int main()
{
	int m, n;

	begin: cin >> m >> n;
	try 
    {
		if (n == 0)
		{
			throw runtime_error("the two numbers can't be zero");
		}
	}
	catch (runtime_error)
	{
		cout << "the two numbers can't be zero" << endl;
		cout << "inout a new number again? Y or N" << endl;
		char c;
		c = getchar();
		c = getchar();
		if (c == 'N' || !c)
		{
			return 0;
		}
		else if (c == 'Y')
		{
			/*cin >> m >> n;
			cout << m / n << endl;*/
			goto begin;

		}
		cout << m / n << endl;
	}
		cout << m / n << endl;
	return 0;
}

```

