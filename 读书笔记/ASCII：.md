ASCII：

‘0’ ：48		‘A' : 65 	 ’a‘ : 97

```c
#include<stdio.h>
int main()
{
	int a, b, c;
	scanf_s("%d%d%*d%d", &a, &b, &c);
	printf("%d,%d,%d", a, b, c);
	return 0;
}
```

> 自动忽略第三个数据，c读到的是第四个数据！