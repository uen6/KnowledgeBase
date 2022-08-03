# JAVA笔记总结



## 基础内容
### 输出 & 输入

**输出**

```java
system.out.println("aaa"+i); //输出一行（自带换行符）
system.out.print("bbb"+i); //输出一句（不自带换行符）
system.out.printf("%.2f",i); //按条件格式输出，相当于C的 printf( );
```

**输入**

```java
scanner in=new scanner(system.in); //初始化输入（相当于告诉系统我这个程序后面会输入东西）
system.out.println( in.nextline( ) ); //直接将输入的东西输出

in.next(); //读入一个单词，空格，Tab，换行为结束标志
int.nextLine(); //读入一整行
```



### 类型

**整型和浮点型**

```java
int i = in.nextint();
float i = in.nextfloat();
```

**布尔型**

```java
boolean i = true/false; //逻辑类型，只有 true 和 false 两种值
```

**字符串**

```java
String s = new String("XXX"); //创建一个字符串，并初始化为 XXX
String s = "abc"; //给字符串赋值
```

**常量**

```java
final int/char = 3; //定义常量，作用类似于C中的 #define
```



### 数组

```java
int[] a = {1, 2, 3, 4, 5, 6};
int[][] a = { {1, 2, 3}, {4, 5, 6}, };
int[] b = a;
int[] c = new int[10];
int[][] c = new int[10][10];

boolean e = new boolean[10]; //逻辑类型数组
```



### 包裹类型
| 普通类型 | 对应的包裹类型 |
| -------- | -------------- |
| boolean  | Boolean        |
| char     | Character      |
| int      | Integer        |
| double   | Double         |

**获取包裹类型最大值**

```java
Integer.MIN_VALUE //获得该类型最大或最小值，类似于sizeof()
Integer.MAX_VALUE
```



### 访问修饰符作用范围

> 静态初始化`static{...}` > 普通初始化`{...}` > 构造方法的初始化`public abc(){...}`

| 修饰符          | 权限                   |
| --------------- | ---------------------- |
| private         | 本类                   |
| default（默认） | 本类、同包             |
| protected       | 本类、同包、子类       |
| public          | 本类、同包、子类、其他 |



### 转义字符
| 符号 | 作用                     | 符号 | 作用       |
| ---- | ------------------------ | ---- | ---------- |
| `\b`   | 回退一格，类似于光标前移 | `\*`   | 输出星号   |
| `\t`   | 到下一个制表位           | `\'` | 输出逗号   |
| `\n`   | 换行                     | `\\`   | 输出反斜杠 |
| `\r`   | 回车，光标移到该行最前面 |      |            |



### format()

>将输出格式化成字符串，用法类似于 printf(" ", );

| 转换符 | 说明                                        | 示例         |
| ------ | ------------------------------------------- | ------------ |
| %s     | 字符串类型                                  | "mingrisoft" |
| %c     | 字符类型                                    | 'm'          |
| %b     | 布尔类型                                    | true         |
| %d     | 整数类型(十进制)                            | 99           |
| %x     | 整数类型(十六进制)                          | FF           |
| %o     | 整数类型(八进制)                            | 77           |
| %f     | 浮点类型                                    | 99.99        |
| %a     | 十六进制浮点类型                            | FF.35AE      |
| %e     | 指数类型                                    | 9.38e+5      |
| %g     | 通用浮点类型(f和e类型中较短的)              |              |
| %h     | 散列码                                      |              |
| %%     | 百分比类型                                  | ％           |
| %n     | 换行符                                      |              |
| %tx    | 日期与时间类型(x代表不同的日期与时间转换符) |              |



### String类常见操作
| 语句                                                       | 作用                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| `int indexOf(int ch)`                                      | 返回指定字符在此字符串中第一次出现处的索引                   |
| `int lastIndexOf(int ch)`                                  | 返回指定字符再此字符串中最后一次出现处的索引                 |
| `int incdeOf(String str)`                                  | 返回指定子字符串再此字符中第一次出现处的索引                 |
| `char charAt(in index)`                                    | 返回指定子字符串在此字符串中最后一次出现处的索引             |
| `boolean endsWith(String suffix)`                          | 返回字符串中index位置上的字符，其中，index的取值范围是0~（字符串长度-1） |
| `int length()`                                             | 判断此字符串是否以指定的字符串结尾                           |
| `boolean equals(Objext anObject)`                          | 返回此字符串的长度                                           |
| `boolean isEmpty()`                                        | 将此字符串与指定的字符串比较                                 |
| `boolean startsWith(String prefix)`                        | 当且仅当字符串长度为0时返回true                              |
| `boolean contains(CharSwquence cs)`                        | 判断此字符串是否以指定的字符串开始                           |
| `String toLowerCase()`                                     | 判断此字符串中是否包含指定的字符序列                         |
| `String toUpperCase()`                                     | 使用默认语言环境的规则将String中的所有字符都转换为小写       |
| `static String valueOf(int i)`                             | 使用默认语言环境的规则将String中的所有字符都转换为大写       |
| `char[] toCharArray()`                                     | 返回int参数的字符串表示形式                                  |
| `String replace(CharSequence oldstr, CharSequence newstr)` | 将此字符串转换为一个字符数组                                 |
| `String[] split(String regex)`                             | 根据参数regex将原来的字符串分割为若干个子字符串              |
| `String substring(int beginIndex)`                         | 返回一个新字符串，它包含从指定的beginIndex处开始直到此字符串末尾的所有字符串 |
| `String substring(int beginIndex, int endIndex)`           | 返回一个新字符串，它包含从指定的beginIndex处开始直到索引endIndex-1处的所有字符 |
| `String trim()`                                            | 返回一个新字符串，它去除了原字符串首尾的空格                 |

**比较两个字符串**

```java
s1 == str2 //比较两个是否共同控制一个字符串
s1.equals("aaa") //比较内容是否相同
s1.compareTo(s2) //比较两个字符串，相等为0，s1大为正，s2大为负
"abc".compareTo(s2)
s1.compareToIgnoreCasa //不区分大小写来比较
```

**访问String里的字符**

```java
s.charAt(i) //i的范围：0到length()-1，不能使用for-each循环

//例：
for(int i; i<s1.length(); i++)
{system.out.println(s.charAt(i));} //输出字符串s中的每一项
```

**得到子串内容**

```java
s.substring(n) //得到从n到末尾的全部内容
s.substring(a,b) //得到从a到b之前的内容

//例：
System.out.println( s1.substring(1,5));
```



### StringBuffer类常见操作
```java
StringBuffer append(char c)
StringBuffer insert(int offset, String str)
StringBuffer deleteCharAt(int index)
StringBuffer delete(int start, int end)
StringBuffer replace(int start, int end, String s)
void setCharAt()
```





## 进阶内容

### 构造对象

**无参的对象（类似于结构体）**

```java
public class ABC{
    //类（类似于结构体中的变量，默认值为0）
    int a;
    char b;
    float c;
    //方法（类似于自定义函数）
    public void abc() {
	System.out.println("abc");
    }
}

ABC ab=new ABC(); //新建对象，供后面程序调用

ab.b=12.1; //给对象中的类赋值
ab.c='A';

ab.abc(); //调用对象中的自定义函数
```

**有参的对象**

```java
public ABC(int newA, char newB, float newC) {
    int a = newA;
    char b = newB;
    float c = newC;
    public void abc() {
	System.out.println("");
    }
}

ABC ab=new ABC(2, 'A', 17.4f); //调用的时候可以赋值
```

**对参数进行判断**

```java
public ABC(int newA) {
   if(newA>10) System.out.println("NO!");
   else int a=newA;
   ...
}
```



### 封装
**修改属性的可见性**

```java
private int number; //在定义变量的前面加：private
```

**输出有private的变量**

```java
public int getNumber(){
	return number;
}
```

**输入有private的变量**

```java
public void setNumber(int newNumber){
	number = newNumber;
}
```



### 异常
**捕捉异常**

```java
try {
    //可能产生异常的代码
    ...
} catch(type1 id1) {
    //处理type1异常的代码
    ...
} catch(type2 id2) {
    //处理type2异常的代码
    ...
} catch(type3 id3) {
    //处理type3异常的代码
    ...
}
//如果出现异常，异常代码会给catch捕捉到，并执行相应的catch中的语句
//如果没有出现异常，就不会执行catch中的语句。当try中出现异常后，异
//常语句后面的正常语句将不会被执行，系统直接跳到catch处开始执行
```



### java.util包
**导入此包**

在文档的最前面（`public class XXX`）输入`import java.util.XXX`

**Arrays.sort(数组名)**

排序，由小到大。

```java
//例：
int[] a={2,3,5,1};
Arrays.sort(a);
```
**Arrays.toString(数组名)**

将数组转成字符串，多个元素之间用逗号和空格隔开。

```java
//例：
int[] a={2,3,5,1};
System.out.println( Arrays.toString(a) );
```

