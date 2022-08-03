# C语言笔记总结



## 头文件

```c
#include <stdio.h> //基本输入输出库函数
#include <string.h> //字符串处理的库函数
#include <stdlib.h> //文件的库函数
#include <time.h> //时间库函数
#include <conio.h>
```





## 宏定义 和 结构体

### define 和 undef

```c
#define N 10 //宏定义，将代码中的变量N都替换为10
#undef N //#define 的作用效果到这里结束
```



### ifdef 和 endif

```c
#ifdef XX //如果#define定义了XX，就执行#ifdef到#endif之间的语句
#endif
```



### typedef

类型自定义名称，类似于`define`但是可以参与类型检查（`define`的作用只是简单的字符替换）

```c
typedef unsigned char BYTE; //定义
BYTE X, Y[10], *Z; //使用
```

可以和结构体一起使用，从而省略结构体的标签

```c
typedef struct {double x; double y;} rect; //定义
rect x; //使用
```

**提示：**`typedef struct`和`struct`之间的不同请查看`struct`定义新变量的代码



### struct

结构体，格式和例子如下：

```C
/*
struct 结构名
{
	类型 变量名;
	类型 变量名;
	...
} 结构变量; 
*/

struct persons { //定义
 	char name[8];
	int age;
	char sex[2];
	char depart[20];
	float wage1, wage2, wage3, wage4, wage5;
} person;

struct persons x; //使用
```





## 主函数

```c
int main(void) 
{
	return 0;
}
```





## 变量类型

| 整型      | 字符型 | 浮点型      |
| --------- | ------ | ----------- |
| int       | char   | float       |
| long      |        | long double |
| short     |        | double      |
| long long |        |             |

**局部变量和全局变量**

| 类型     | 作用                                       |
| -------- | ------------------------------------------ |
| auto     | 自动变量                                   |
| static   | 静态局部变量                               |
| register | 寄存器变量，再次调用函数中的变量时数据还在 |
| extern   | 扩展全局变量范围                           |





## 控制符

### 输入（scanf）的控制符

| 符号 | 作用           | 符号 | 作用            |
| ---- | -------------- | ---- | --------------- |
| i或d | int            | g    | float           |
| u    | unsigned int   | G    | float           |
| o    | 八进制         | a或A | 十六进制        |
| x    | 十六进制       | c    | char            |
| X    | 大写的十六进制 | s    | 字符串          |
| f或F | float，6       | p    | 指针            |
| e或E | 指针           | n    | 读入/写出的个数 |



### 输出（printf）的控制符

| 符号    | 作用                         | 符号 | 作用         |
| ------- | ---------------------------- | ---- | ------------ |
| d       | int                          | s    | 字符串(单词) |
| i       | 整数，可以为十六进制或八进制 | u    | unsigned int |
| o       | 八进制                       | x    | 十六进制     |
| a,e,f,g | float                        | c    | char         |
| l       | long,double                  | ll   | long long    |
| h       | short                        | hh   | char         |
| L       | long double                  | p    | 指针         |
| 数字    | 最大字符数                   | *    | 跳过         |
| [...]   | 所允许的字符                 |      |              |



### 文件操作（fopen）的控制符

| 符号 | 作用                                               |
| ---- | -------------------------------------------------- |
| r    | 打开只读                                           |
| r+   | 打开读写，从文件头开始                             |
| w    | 打开只写，如果不存在则新建，如果存在则清空         |
| w+   | 打开读写，如果不存在则新建，如果存在则清空         |
| a    | 打开追加，如果不存在则新建，如果存在则从文件尾开始 |
| ..x  | 只新建，如果文件已存在则不能打开                   |





## 常用语句

```c
a[ ][ ]={ } //二维数组
```

```c
scanf("%",& );
printf("%", );
```

```c
for ( ; ; ){ ;}
if ( ){ ;}
```

```c
break; //退出循环
continue; //跳过本次循环
```

```c
default; //switch语句中使用
```

```c
/**/ //多行注释
// //单行注释
```





## 函数

### 字符串函数
| 函数               | 作用                                   |
| ------------------ | -------------------------------------- |
| strcat(str1, str2) | 字符串连接                             |
| strcpy(str1, str2) | 字符串复制（会覆盖前一个字符串的内容） |
| strcmp(str1, str2) | 字符串比较                             |
| strlen(str1)       | 测量字符串长度                         |
| strlwr(str1)       | 转换为小写                             |
| strupr(str1)       | 转换为大写                             |



### 文件操作函数
| 函数                            | 作用                   |
| ------------------------------- | ---------------------- |
| fgetc(fp)                       | 从fp中读出一个字符     |
| fputc(ch, fp)                   | 把ch写到fp中           |
| fgets(str, n, fp)               | 从fp中读出n个放到str中 |
| fputs(str, fp)                  | 把str写到fp中          |
| fread(buffer, size, count, fp)  | 从fp中读出             |
| fwrite(buffer, size, count, fp) | 写到fp中               |
| fseek(fp, n, 0)                 | 0开头 1当前 2结尾      |





## 代码块

### const

可以用来定义一个常量，例：`const int i=0;`类似于`#define`，但比`#define`功能更强



### 生成随机数

```c
srand((unsigned)time(NULL)); //利用当前时间产生随机数种子
rand()%90+10; //产生随机数（产生[m,n]之间的随机数的公式：rand()%(n-m+1)+m）
```



### goto

```c
I:
goto I //返回执行I所指的行
```



### 暂停程序

```c
getch(); //暂停，直到有按键
```



### 清屏

```c
system("cls");//清除屏幕，之前的输出会被清空
```



### 获取时间

```c
printf("%f", (double)clock()/CLOCKS_PER_SEC); //查看程序运行时间

例：
clock_t start, end;
start = clock(); //记录当前时间
end = clock(); //记录当前时间
printf("%f", (double)(end-strat)/CLOCKS_PER_SEC); //查看程序运行时间
//注意：要用后面的时间减去前面的时间，否则出来的值将会是负数
```



### 让编译器“假死”

```c
#include <con>
```



### 控制输出长度

```c
printf( "%*d", 5,i);
```

`*`号表示占位符，可以控制输出的长度，例句中输出的长度就为`5`，好处就是可以随意控制输出的长度。



### 调用快速排序

```c
//括号中分别对应的是：[数组名] [元素个数] [单个元素的大小] [排序方法]
qsort(a, n, sizeof(), cmp) 


//排序方法（适用于int型一维数组）
int cmp(const void *a, const void *b) {
    return *(int*)a - *(int*)b;
}
```

**提示：**快排，头函数为`#include <stdlib.h>`



### 创建链表

```c
#include <stdio.h>
#include <stdlib.h>

/*定义节点*/
typedef struct _node {
	int num;
	struct _node *next;
} node;
typedef node *linkList;

/*主函数*/
int main(void) {
	int i=0;
	linkList flag = (linkList)malloc(sizeof(node));
    flag->num = i++;
    flag->next = NULL;
	linkList head = flag; //保存头节点位置
    
    //创建节点内容
    while(i<10) {
        linkList p = (linkList)malloc(sizeof(node)); //创建临时节点
        p->num = i;
        p->next = NULL;
        flag->next = p; //将临时节点链接到flag后
        flag = flag->next;
        i++;
    }

    //输出节点
    while(head) {
        printf("%d ", head->num);
        head = head->next;
    }

	return 0;
}
```

