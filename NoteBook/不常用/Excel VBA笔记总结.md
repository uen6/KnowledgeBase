# Excel VBA笔记总结



## Visual Basic内容

### 基本数据类型

```vb
'定义格式：Dim 变量名 As 变量类型
Dim b,bb As Byte 'VB中语句不需要用分号结尾
Dim i As Integer
Dim s As Single
Dim d As Double
Dim c As Char
Dim str As String
Dim bool As Boolean = True
Const cs As Integer = 1000 '用Const关键字来定义常量

'赋值
b = 100
bb = 1
i = 1000
s = 1.5
d = 3.1415926
c = "c"c '字符后面加个c表示是char型的
str = "abc"
```

**补充：**VB中没有块注释，只有单行注释，以单引号开头，直到句末。





### 类型声明符

| 数据类型                              | 类型声明字符 |
| ------------------------------------- | ------------ |
| Integer                               | %            |
| Long                                  | &            |
| Single（单精度浮点型）                | !            |
| String                                | $            |
| Double                                | #            |
| *Currency（货币型，VBA中存在的类型）* | *@*          |

**例：**

```vb
Dim str$ '$表示string类型，str为变量名
```

**注意：**如果声明时不指定变量类型，则默认为`Variant`类型（变体型），`variant`型可以在需要时变化自己的类型与要存储的数据类型匹配。





### 运算符

#### 算术运算符

| 运算符 | 用途     | 例子          |
| ------ | -------- | ------------- |
| ^      | 幂运算符 | `3 ^ 2 = 9`   |
| +      | 加法     |               |
| -      | 减法     |               |
| *      | 乘法     |               |
| /      | 小数除法 | `3 / 2 = 1.5` |
| \      | 整除     | `3 \ 2 = 1`   |
| MOD    | 取模     | `6 MOD 4 = 2` |



#### 比较运算符

| 运算符                | 用途                                                         |
| --------------------- | ------------------------------------------------------------ |
| **=**                 | **相等**                                                     |
| **<>**                | **不等**                                                     |
| <                     | 小于                                                         |
| >                     | 大于                                                         |
| <=                    | 小于等于                                                     |
| >=                    | 大于等于                                                     |
| *Is（VBA中的内容）*   | *比较两个对象是否引用了一个相同的对象（返回结果`True`或`False`）* |
| *Like（VBA中的内容）* | *比较两个字符串是否匹配（返回结果`True`或`False`）*          |

**注意：**VB的**相等**和**不等**和其他语言的可能不太一样。



#### 逻辑运算符

| 运算符               | 用途                                     |
| -------------------- | ---------------------------------------- |
| And                  | 与运算符，不短路求值                     |
| Or                   | 或运算符，不短路求值                     |
| Not                  | 非运算符                                 |
| Xor                  | 异或运算符                               |
| AndAlso              | 与运算符，短路求值                       |
| OrElse               | 或运算符，短路求值                       |
| IsTrue               | 是否为真                                 |
| IsFalse              | 是否为假                                 |
| *Eqv（VBA中的内容）* | *是否等价（检测两个表达式的值是否相同）* |





### 条件语句

#### If语句

其基本结构是`If 条件 Then 执行体 Else 执行体 End If`

```VB
Dim num As Integer = 5
If num\2 = 0 Then
	Console.WriteLine($"{num} is even")
Else
	Console.WriteLine($"{num} is odd")
End If
```

如果需要多重If语句，可以添加ElseIf语句块。

```VB
If num = 1 Then
	Console.WriteLine($"{num} is 1")
ElseIf num = 2
	Console.WriteLine($"{num} is 2")
Else
	Console.WriteLine($"{num} is other")
End If
```



#### Select语句

Select语句相当于其他语言中的switch语句，如果没有匹配项，会执行Case Else语句块。

```VB
Dim c As Char = "c"c
Select c
	Case "a"c
		Console.WriteLine("a")
	Case "b"c, "c"c
    	Console.WriteLine("b or c")
    Case Else
        Console.WriteLine("others")

End Select
```





### 循环语句

#### Do语句

`Do-While`语句，当满足循环条件的时候会继续循环，不满足条件时跳出循环。

```VB
'Do While语句
Dim i = 0
Do While i < 5
	If i = 3 Then
		Exit Do
	End If
	Console.Write(i)
	i += 1
Loop
```

`Do-Until`语句，和`Do-While`相反，在不满足条件的时候会执行循环，满足条件时跳出循环。

```VB
'Do Until语句
i = 0
Do Until i > 5
	If i < 3 Then
		i += 2
		Continue Do
	End If
		Console.Write(i)
		i += 1
Loop
```

Do循环的第二种形式就是先执行循环体，然后进行判断，同样有While和Until两种。

```VB
i = 0
Do
	Console.Write(i)
	i += 1
Loop While i < 5
Console.WriteLine
```



#### While循环

```VB
'While循环
i = 0
While i < 5
    Console.Write(i)
    i += 1
End While
```



#### For循环

下面是For循环的例子，在For循环中可以使用Step指定步长。

```VB
For counter As Integer = 1 To 9 Step 1
	Console.Write(counter)
Next '也可以写：Next [循环变量]
```



#### Foreach循环

Foreach循环用于迭代一个列表中的每一项。

```VB
Dim array() as Integer = {1, 2, 3, 4, 5, 6, 7}
For Each item As Integer In array
	Console.Write(item)
Next
```





### With语句

With语句主要用途是节省代码数量。比方说有下面这个Person类。

```VB
Public Class Person
    Public Property Name As String
    Public Property Age As Integer
End Class
```

假如有一个person对象多次出现的话，就可以使用With语句，在With语句中，点访问符默认指向的就是With语句指定的对象。

```VB
Dim person As Person = New Person
With person
	.Name = "aaa"
	.Age = 25
	Console.WriteLine($"Person(Name:{.Name}, Age:{.Age})")
End With

'不使用with语句代码应如下：
Dim person As Person = New Person
person.Name = "aaa"
person.Age = 25
```





### 跳转语句

#### Exit语句

Exit语句用于结束某个代码块，形式如下。想用Exit退出哪个代码块，就写哪个代码块的类型。

```VB
Exit { Do | For | Function | Property | Select | Sub | Try | While }
```



#### Continue语句

Continue语句用于结束当前循环，直接进行下一次循环。形式如下，后面跟要继续的代码块类型。

```VB
Continue { Do | For | While }
```



#### Goto语句

Goto语句，它会直接跳转到指定的标签处。

```VB
'Goto语句
GoTo Ending
Console.WriteLine("Print something")
Ending:
Console.WriteLine("This is end.")
```





### 数组

VB中的数组比较特殊，定义一个`Dim a1(3)`，其实是**下标0-3长度为四的一维数组**，这一点要非常注意。

```VB
'定义数组，起始索引为1，终止索引为100
Dim array(1 To 100) As Byte

'下标0-9的十个元素的数组
Dim array1(9) As Integer '等同于：Dim array1(0 To 9) As Integer，共定义了0~9，共10个数组元素

'3X3的二维数组
Dim array2(2, 2) As Integer '等同于：Dim array2(0 To 2, 0 To 2) As Interger

'定义并初始化数组
Dim array3() = {1, 2, 3, 4, 5}
ReDim array3(3) 'ReDim可重新定义数组大小，但无法改变数组类型

'锯齿数组，也就是数组的数组
Dim array4 As Integer()() = New Integer(1)() {}
array4(0) = New Integer() {1, 2}
array4(1) = New Integer() {3, 4}
```

访问数组元素需要使用**圆括号**，而不是一般语言的方括号。

```VB
'初始化一维数组
For i As Integer = 0 To 9
	array1(i) = i
Next

'初始化二维数组
For i = 0 To 2
	For j = 0 To 2
		array2(i, j) = (i + 1)*(j + 1)
	Next
Next
```

可以使用For循环迭代下标，或者用Foreach循环直接遍历元素。

```VB
'显示数组
For Each e In array1
	Console.Write(e)
Next
Console.WriteLine

For i As Integer = 0 To 2
	For j = 0 To 2
		Console.Write(array2(i, j))
	Next
	Console.WriteLine
Next

For Each e In array3
	Console.Write(e)
Next
Console.WriteLine
For i As Integer = 0 To 1
	For j = 0 To 1
		Console.Write(array4(i)(j))
	Next
	Console.WriteLine
Next
```





### 函数

#### Sub函数

没有返回值的函数，例如Main函数就是一个Sub函数。

```VB
Imports System

Module Program
    Sub Main(args As String())
        Console.WriteLine("Hello World!")
    End Sub
End Module
```

**注意：**函数可以按值传参，也可以按引用传参，VB或VBA的传参与其他语言不同，其**默认**情况下是**按引用传参**，函数里修改参数会影响到原变量的值，如果想让参数按值传参应该在参数前面加上`ByVal`，例：`Sub Main(ByVal args As String())`。同样的，想按引用传参可以加上`ByRef`。



#### Function函数

Function函数就是有返回值的函数。在函数中，如果要返回值，可以有两种办法，第一种是使用Return语句，第二种是在函数体中向函数名赋值。

```VB
'函数返回值可以用Return语句
Function Return1() As Integer
	Return 1
End Function

'也可以向函数名赋值
Function Return2() As Integer
	Return2 = 2
End Function

'Function模板
[Public|Private][Static] Function 函数名([参数列表]) [As 数据类型] '[As 数据类型]用来规定函数返回值的数据类型
    [语句块]
    [函数名=过程结果]
    [Exit Function]
End Function
```



#### 可变参数列表

可变参数列表使用`ParamArray`声明。

```vb
Function PrintIntegers(ParamArray integers As Integer())
	For Each i In integers
		Console.Write(i)
	Next
	Console.WriteLine
End Function
```







## VBA内容

### 工作表对象

```vb
'Application对象代表excel程序，是excel vba中最顶层的对象
Application.Workbooks("工作簿1") 'workbooks是工作簿集合，代表所有打开的工作簿
			.Worksheets("sheet1") 'worksheets是工作表集合，代表指定工作簿中所有的工作表
			.Range ("A2")
			.Font.Color '单元格的属性
```

对于当前活动工作表，可直接写`Range ("A2")`





### 通配符

| 符号          | 作用                                         |
| ------------- | -------------------------------------------- |
| *             | 代替任意多个字符                             |
| ？            | 代替任意单个字符                             |
| #             | 代替任意单个数字                             |
| [*charlist*]  | 代替*charlist*中的任意一个字符，如[A-Z]      |
| [!*charlist*] | 代替不在*charlist*中的任意一个字符，如[!H-J] |





### 常用对象

| 对象              | 说明                                                  |
| ----------------- | ----------------------------------------------------- |
| Application       | 代表当前使用的应用程序，是最顶层的对象                |
| Workbook          | 代表一个工作簿，如果是多个工作簿就用Workbooks         |
| Worksheet         | 代表一个工作表，如果是多个工作表就用Worksheets        |
| Range             | 代表单元格，也可以是单元格区域                        |
| WorksheetFunction | Excel中的函数对象，例如sum、count等都属于此对象底下的 |





### Application对象常用属性

| 属性           | 返回的对象                                                   |
| -------------- | ------------------------------------------------------------ |
| ActiveCell     | 当前活动单元格                                               |
| ActiveChart    | 当前活动工作簿中的活动图标                                   |
| ActiveSheet    | 当前活动工作簿中的活动工作表                                 |
| ActiveWindow   | 当前活动窗口                                                 |
| ActiveWorkbook | 当前活动工作簿                                               |
| Charts         | 当前活动工作簿中所有的图表对象                               |
| Selection      | 当前活动工作簿中所有选中的对象                               |
| Sheets         | 当前活动工作簿中所有Sheet对象，包括普通工作表、图标工作表、宏工作表、对话框工作表 |
| Worksheets     | 当前活动工作簿中的所有Worksheet对象（普通工作表）            |
| Workbooks      | 当前所有打开的工作簿                                         |





### Worksheet对象常用属性

| 属性                       | 作用                                                         |
| -------------------------- | ------------------------------------------------------------ |
| Add [before\|after\|count] | 插入一张工作表，`before`和`after`为指定的插入位置，`count`为指定的工作表数量 |
| Name                       | 更改工作表标签名称                                           |
| Delete                     | 删除工作表                                                   |
| Activate / Select          | 选中工作表，`Activate`无法选中多张工作表，但是能选中隐藏工作表；`Select`则相反，它能选中多张工作表，但无法选中隐藏工作表 |
| Copy                       | 复制工作表                                                   |
| Move                       | 移动工作表                                                   |
| Visible                    | 隐藏工作表，可选值有`False`、`xlsheetHidden`、`0`，都为隐藏的意思 |
| Count                      | 获取工作表数量                                               |
| UsedRange                  | 返回已使用的单元格围成的区域，夹在已使用的单元格中的空行空列依旧被计算在其中 |

**注意：**Worksheets与Sheets并不一样，Worksheets表示普通工作表，即常用的表格部分，Sheets还包含了图标、MS Excel 4.0宏文件、MS Excel 5.0对话框类型的工作表。





### Range对象常用属性

| 属性                                         | 作用                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| Cells(行号,列号) / Cells(索引号)             | 引用单元格，也可引用某个区域中的单元格，例：`Cells(3,"D")`。`Cells(52)`表示第52个单元格 |
| Rows(行号)                                   | 引用整行单元格，例：`Rows(3)`表示引用第三行，直接使用`Rows`表示引用所有行 |
| Columns(列号)                                | 引用整列单元格，使用方法和`Rows`相似                         |
| Union                                        | 将多个Range对象组合在一起，例：`Union(Range("A1:A10"), Range("D1:D5"))` |
| offset(上下偏移量，左右偏移量)               | 获取指定偏移量的单元格区域，例：`offset(5,3)`                |
| Resize(扩展行数，扩展列数)                   | 将指定单元格扩大或缩小指定行数或列数                         |
| CurrentRegion                                | 返回指定单元格在内的连续的矩形区域，例：`CurrentRegion.Select` |
| End(xlToLeft \| xlToRight \| xlUp \| xlDown) | 返回指定区域最尾端的单元格，效果等同于在单元格中按`End+方向键` |
| Value                                        | 给属性赋值，例如：`Range().Value="abc"`                      |
| Count                                        | 获取单元格区域中包含的单元格个数，也可与`Rows`和`Columns`同用，获取包含的行数或列数 |
| Address                                      | 获取单元格的地址                                             |
| Activate / Select                            | 选中单元格或单元格区域                                       |
| Clear                                        | 清除所有内容，包括批注、内容、格式、超链接                   |
| ClearComments                                | 清除批注                                                     |
| ClearFormats                                 | 清除格式                                                     |
| ClearHyperlinks                              | 清除超链接                                                   |
| Copy                                         | 复制单元格区域，例：`Range("A1").Copy Range("C1")`，复制到目标区域时，只需要指定一个单元即可 |
| Cut                                          |                                                              |
|                                              |                                                              |
|                                              |                                                              |
|                                              |                                                              |
|                                              |                                                              |
|                                              |                                                              |







### 使用技巧

#### VBA代码运行提速

在VBA过程程序的首尾分别加下面两句话

```vb
sub test1()
	application.ScreenUpdating=False '加在程序开始
    ...
	application.ScreenUpdating=true '加在程序结束
End sub
```

这两句话的意思分别是**关闭屏幕刷新**和**打开屏幕刷新**，是最快最有效提升Excel VBA代码运行速度的方法。



#### 程序运行时不显示警告对话框

```vb
Application.DisplayAlerts = False '设置不显示警告对话框
Application.DisplayAlerts = True '恢复警告对话框
```



#### 修改数组默认的起始索引

定义数组时如果不写起始索引，则起始索引默认为0，但可以通过`OPTION BASE 1`来修改起始索引为1。



#### 设置易失性函数

在函数开始时添加`Application.Volatile True`
设置成易失性的函数会随着工作表的更新而重新计算，保证了数据的正确性。



#### 长代码换行和短代码不换行

使用空格加下划线来分割长代码。

```vb
'例：
Application.Workbooks("book1").Worksheets("sheet1") _ '下划线前面的空格不要忽略
	.Range("A1:D100").Font.Bold = True

'等同于下面代码：
Application.Workbooks("book1").Worksheets("sheet1").Range("A1:D100").Font.Bold = True
```

使用冒号将多行代码并为一行。

```vb
'例：
Dim a%, b%, c%: a=1: b=2: c=3

'等同于下面代码：
Dim a%, b%, c%
a=1
b=2
c=3
```



#### VBA中调用Excel函数

```vb
'例如调用countif函数：
Application.WorksheetFunction.CountIf(Range("A1:B50"), ">1000") 'WorksheetFunction就是指Excel中的函数
```

**注意：**Excel的函数与VBA的函数有一定冲突性，且使用范围也有局限性，所有不建议在代码中使用Excel的函数。



#### 默认工作表数量

Excel 2013版本后，新建工作簿的默认工作表数量为**1个**，所以使用`Workbooks.Add`后，只会得到一个工作表，但是早年间使用此语句时，会得到三个工作表。所以当较老的VBA代码放到新版本的Excel下（2013版本以后）运行可能就会产生数组下表越界的报错提醒。
对于此问题可以使用`SheetsInNewWorkbook`属性来更改这个数量。或者可以修改Excel的 **文件-->选项-->常规-->包含的工作表数**，将**1**更改为**3**。
