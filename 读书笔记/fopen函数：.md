### fopen函数：

> r:只读                         如果文件不存在，会报错
>
> w:只写						如果文件不存在，会重新创建一个文件
>
> r+:可读可写
>
> w+:可读可写
>
> a：加写

### fclose函数：

fclose(FILE* fp); 

功能：关闭文件

输入函数流：传入之前打开的文件

### fputc：输入文件！

fputc('a',fp);

```c++
FILE* fp = fopen("./a.txt", "w");
	if (NULL == fp)
	{
		perror("open");
		return 0;
	}
	char buf[] = " hello ";
	int i = 0;
	while (buf[i] != 0)
	{
		fputc(buf[i], fp);
		i++;
	}
	fclose(fp);
```

### fgetc函数：读取文件

```c++
FILE* fp = fopen("./a.txt", "r");
	char arr[] = "hello world";
	int i = 0,m=0;
	while (!feof(fp))
	{
		printf("%c\n", fgetc(fp));
	}
```

### 随机函数：

```c++
srand(time(NULL));
	int a = rand()%10;
```

