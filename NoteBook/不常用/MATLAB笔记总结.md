# MATLAB笔记总结



## 基础知识

### 基本运算符

| 普通运算 | 数组运算 | 矩阵转置 |
| :------: | :------: | :------: |
|    +     |    .*    |    '     |
|    -     |    .\    |    .'    |
|    *     |    ./    |          |
|    \     |    .^    |          |
|    /     |          |          |
|    ^     |          |          |



### 常用函数

| 函数     | 作用            | 函数            | 作用              |
| -------- | --------------- | --------------- | ----------------- |
| abs(x)   | 绝对值函数      | round(x)        | 四舍五入取整      |
| sqrt(x)  | 开平方          | fix(x)          | 朝0方向取整       |
| exp(x)   | 指数函数        | floor(x)        | 朝负无穷方向取整  |
| log(x)   | 自然对数函数    | ceil(x)         | 朝正无穷方向取整  |
| log10(x) | 常用对数函数    | rats(x)         | x化为分数表示     |
| sign(x)  | 符号函数        | sin(x)/cos(x)   | 正弦/余弦函数     |
| angle(z) | 复数z的相角     | tan(x)/cot(x)   | 正切/余切函数     |
| real(z)  | 复数z的实部     | sec(x)/csc(x)   | 正割/余割函数     |
| imag(z)  | 复数z的虚部     | asin(x)/acos(x) | 反正弦/反余弦函数 |
| conj(z)  | 复数z的共轭复数 | atan(x)/acot(x) | 反正切/反余切函数 |



### 系统预定义变量

| 变量名  | 作用                        |
| ------- | --------------------------- |
| pi      | 圆周率，其值为imag(log(-1)) |
| inf/Inf | 无穷大                      |
| nan/NaN | a-number, 一个不定值，如0/0 |
| eps     | 浮点运算相对精度            |
| i/j     | 虚部单位，即 根号(-1)       |
| ans     | 特殊变量                    |



### 快捷键

| 快捷键 | 作用                                                    |
| ------ | ------------------------------------------------------- |
| ↑↓键   | 可以先输入命令的前几个字符，再按上下键头缩小范围        |
| Tab键  | 命令补全                                                |
| esc键  | 删除命令行                                              |
| 回车键 | 输出结果，若不想每次都显示输出结果只需在算式后面加 ; 号 |
| clc    | 清空命令窗口记录                                        |



### format格式

> 仅修改输出格式，不会影响变量的值

| 格式                                                        | 解释                              |
| ----------------------------------------------------------- | --------------------------------- |
| format                                                      | 短格式（缺省显示格式），同short   |
| format short                                                | 短格式（缺省显示格式），只显示5位 |
| format long                                                 | 长格式，双精度数15位，单精度数7位 |
| format short e                                              | 短格式e方式（科学计数格式）       |
| format long e                                               | 长格式e方式                       |
| format short g                                              | 短格式g方式                       |
| format long g                                               | 长格式g方式                       |
| format compact                                              | 压缩格式                          |
| format loose                                                | 自由格式                          |
| format+<br />/format bank<br />/format rat<br />/format hex | ？（未整理）                      |

**补充：**MatLab以双精度执行所有运算，结果可以在屏幕上输出，同时赋值给指定变量，若无指定变量，则自动将结果赋值给变量“`ans`”







## 使用例子

### 求极限

> `limit()`

```matlab
limit(f,x,a) %计算lim f(x) ,x->a
limit(f,a) %当默认变量趋向于a时的极限
limit(f) %计算 a=0 时的极限
limit(f,x,a,'right') %计算右极限
limit(f,x,a,'left') %计算左极限
```

**例1：**求函数$f(x)=ax^2+bx+c$ 的极限

```matlab
>>syms a b c x
>>f=a*x^2+b*x+c;
>>f1=limit(f,x,2)
```



### 求导数

> `diff()`

```matlab
diff(f) 或 diff(f,x) % d/dx f(x)
diff(f,2) 或 diff(f,x,2) % d^2/dx^2 f(x)
diff(f,n) 或 diff(f,x,n) % d^n/dx^n f(x)  f为表达式，x为符号变量，n是求导的阶数，默认值为1
diff(S,'x') %求表达式S关于x的导数
diff(S,'x',n) %求表达式S关于x的n阶导数
```

**例1：**求函数$f(x)=ax^{2}+bx+c$

```matlab
>>syms a b c x
>>f=a*x^2+b*x+c;
>>diff(f) %对x求导
>>diff(f,2) %对x求二阶导
>>diff(f,a) %对a求导
>>diff(f,a,2) %对a求二阶导
```

**例2：**求函数$f(x)=e^{-x}*\sin(x^{2})$

```matlab
>>syms x
>>f=exp(-x)*sin(x^2);
>>df=diff(f) %求一阶导
>>df=diff(f，2) %求二阶导
```



### 求微分

> `[char(diff(...)), 'dx']`

**例1：**求函数y=$sin(2x+1)$的微分

```matlab
>>syms x
>>y1=sin(2*x+1);
>>dy1=[char(diff(y1)),'dx']
```

**例2：**求函数$y=ln(1+e^{x^2})$的微分

```matlab
>>syms x
>>y2=log(1+exp(x^2));
>>dy2=[char(diff(y2)),'dx']
```

**例3：**求函数$y=e^{1-3x} cosx$的微分

```matlab
>>syms x
>>y3=exp(1-3*x)*cos(x);
>>dy3=[char(diff(y3)),'dx']
```



### 求不定积分

> `int(f, x)`

```matlab
int(f,x) %计算不定积分 ∫f(x)dx
int(f) %计算关于默认变量的不定积分
```

**例1：**计算$I=\int\frac{x+sinx}{1+cosx}\,dx$

```matlab
>>syms x; //不带常数项
>>f=(x+sin(x))/(1+cos(x));
>>I=int(f,x)

>>syms x C; //带常数项
>>f=(x+sin(x))/(1+cos(x));
>>I=int(f,x)+C;
```

**例2：**求$\int(ax{^2}+bx+c)dx$，$\int(ax{^2}+bx+c)\,da$

```matlab
>>syms a b c x C
>>f=a*x^2+b*x+c;
>>f1=int(f,x)+C
>>f2=int(f,a)+C
```



### 求定积分
```matlab
%计算定积分 F(ba) f(x)dx
int(f,x,a,b) %当f只含有一个变量时，x可省略，a表示积分下限，b表示积分上限
int(f,a,b) %计算关于默认变量的定积分
```

**例1：**求$I=\int_0^1\sqrt{ln{\frac{1}{x}}\,dx}$

```matlab
>>syms x
>>L1=int(sqrt(log(1/x)),0,1)
```

**例2：**求$I=\int_0^2f(x)\,dx$，其中$f(x)=\begin{cases} x+1&x\le1 \\ \frac{1}{2x^2}&x>1\end{cases}$

```matlab
>>syms x
>>L2=int(x+1,0,1)+int(x^2/2,1,2)
```



### 求未知量
```matlab
solve(f,var) %f代表函数式，x代表要求的未知量
solve(f,var,Name,Value)
solve(f,vars)
solve(f,vars,Name,Value)
solve(f,vars)
solve(f,vars,Name,Value)
solve(f,vars,'ReturnConditions',true)
```

**例1：**

```matlab
>>syms x
>>eqn=sin(x)==1;
>>solve(eqn,x)
```

**例2：**

```matlab
>>syms x
>>eqn=sin(x)==1;
>>[solx,params,conds]=solve(eqn,x,'ReturnConditions',true)
```

**例3：**

```matlab
syms x
solve(3*x+2,3*x+1,x) %如果返回empty，则表明解不存在。如果返empty+warning，则解可能存在，但是solve找不到
```







## 其他内容

### LaTex书写格式参照

https://blog.csdn.net/young951023/article/details/79601664

https://lanlan2017.github.io/blog/83c2e83a/





### 命令

**help**

显示当前帮助系统中所包含的所有项目，也可以通过help加载函数名来显示该函数的帮助说明

**lookfor**

只搜索出关键字匹配的结果，只对M文件的第一行进行关键字搜索，加上 `-all` 可以对M文件进行全文搜索

**Demos**

打开演示系统





### 基础知识点

**变量**

以字母开头，后面可以跟字母，数字，下划线，不能超过63个，大小写不一样



**定义变量**

```matlab
sym %建立单个符号变量 例：a=sym('表达式');
syms %建立多个符号变量 例：syms a b c;
```



**语句**

变量 = 表达式



**符号表达式的建立**

```matlab
y=sym('sin(x)+cos(x)')

x=sym('表达式')
y=sin(x)+cos(x)
```



**浮点数表示范围**

$10^{-308}$次方 ~ $10^{308}$次方，相对误差为`eps`



**复数的输入**

$z=3+4i$或$z=3+4*i$  （4与i之间不能有空格，加号两边也不能有空格）
