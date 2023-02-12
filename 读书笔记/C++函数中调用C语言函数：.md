C++函数中调用C语言函数：

```c++
#include<stdio.h>
#ifdef  __cplusplus
extern"C"
{
#endif
	void func();
#ifdef __cplusplus
}
#endif
```

