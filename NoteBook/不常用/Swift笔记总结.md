# Swift笔记总结



## 基础部分



### 变量

```swift
let num = 1000  // let声明一个常量
var str = "string"  // var声明一个变量

var str1 = "str", num2 = 2, num3 = 3.00001  // 声明多个变量
var str1 = "str"; let num2 = 2;  // 一行内编写多个语句时，用分号可以隔开

// 声明一个变量并添加类型标注
var str2: String
str2 = "str2"

// 获取变量的最大值
var max = Int.max  // 同理，可以使用Int.min获取到最小值

// 强制类型转换
var num = int(3.14)  // 强制类型转换，转换结果为3
```

**其他内容**

```swift
/*各种进制数值的书写：
十进制，没有前缀
二进制，前缀是0b
八进制，前缀是0o
十六进，前缀是0x
*/
var binaryInteger = 0b1101  // 二进制1101
let octalInteger = 0o77  // 八进制77
let hexadecimalInteger = 0xff  // 十六进制ff

// 指数表达方式：e为十进制用，p为十六进制用
var decimalDouble = 0.31415e1  // ->3.1415
var hexadecimalDouble = 0xf.fp1 // ->ff

// 下划线：仅仅用来增强可读性，不影响数值
var num = 1_000_000
var float = 12.000_1
```

**注意：**如果你的变量名和保留字重复了，可以使用反引号(``)将变量包含起来，保证变量不会冲突（类似于sql语句中关键字的处理方式）



### 输出&注释

**输出**

```swift
print("hello word!")  // 无换行
println("hello word!")  // 有换行

var num = 10
print("number is \(num)")  // 输出含变量的内容
```

**注释**

```swift
// 这是一个单行注释

/* 这是一个多行注释 */

/*
//这是一个嵌套注释
let num = 0
/* num = 1 */
print("num is \(num)")  // -> num is 0
*/
```



### 元组

```swift
let httpError = (404, "Not Found")  // 定义一个变量给它赋值为元组类型
let (code, message) = httpError  // 给一个元组变量赋值


print("code is \(code), message is \(message)")  // 输出元组中的值->code is 404, message is Not Found
print("code is \(httpError.0), message is \(httpError.1)")  // 通过下标的方式来选择输出的变量值


let httpError = (code: 404, message: "Not Found")  // 给元组中的值命名，类似于键值对
print("code is \(httpError.code), message is \(httpError.message)")  //使用有命名的变量内容


let (code, _) = httpError  // 下划线内容为缺省，表示不使用
print("code is \(code)") 
```


