# Smali语法总结

> Android代码一般是用java编写的，执行java程序一般需要用到java虚拟机，在Android平台上也不例外，但是出于性能上的考虑，并没有使用标准的JVM，而是使用专门的Android虚拟机（5.0以下为Dalvik，5.0以上为ART）。Android虚拟机的可执行文件并不是普通的class文件，而是再重新整合打包后生成的dex文件。dex文件反编译之后就是Smali代码，所以说，Smali语言是Android虚拟机的反汇编语言



## 数据类型

| Smali          | Java     | 备注                                                         |
| -------------- | -------- | ------------------------------------------------------------ |
| v              | void     | 只能用于返回值类型                                           |
| Z              | boolean  |                                                              |
| B              | byte     |                                                              |
| S              | short    |                                                              |
| C              | char     |                                                              |
| I              | int      |                                                              |
| J              | long     |                                                              |
| F              | float    |                                                              |
| D              | double   |                                                              |
| Lpackage/name; | 对象类型 | `L`表示这是一个对象类型<br />`package`表示该对象所在的包<br />`;`表示对象名称的结束 |
| [类型          | 数组     | `[I`表示一个int型数据<br />`[Ljava/lang/String`表示一个String的对象数组 |



## 语法关键词

| 关键词          | 说明                                 |
| --------------- | ------------------------------------ |
| .class          | 定义java类名                         |
| .super          | 定义父类名                           |
| .source         | 定义Java源文件名                     |
| **.filed**      | **定义字段(定义变量)**               |
| .end filed      | 定义字段(定义变量)结束               |
| **.method**     | **定义方法开始**                     |
| .end method     | 定义方法结束                         |
| **.annotation** | **定义注解开始**                     |
| .end annotation | 定义注解结束                         |
| .implements     | 定义接口指令                         |
| **.local**      | **指定了方法内局部变量的个数**       |
| **.registers**  | **指定方法内使用寄存器的总数**       |
| .prologue       | 表示方法中代码的开始处               |
| **.line**       | **表示java源文件中指定行**           |
| **.paramter**   | **指定了方法的参数**                 |
| .param          | 和.paramter含义一致,但是表达格式不同 |



## 寄存器

> Java中变量都是存放在内存中的，Android为了提高性能，变量都是存放在寄存器中的，寄存器为32位，可以支持任何类型

寄存器分为如下两类：

1. 本地寄存器：用v开头数字结尾的符号来表示`v0, v1, v2,...`

2. 参数寄存器：用p开头数字结尾的符号来表示`p0,p1,p2,...`

**注意：**在非`static`方法中，`p0`代指`this`，`p1`为方法的第一个参数。在`static`方法中，`p0`为方法的第一个参数。

```java
//把值0x1存到v0本地寄存器
const/4 v0, 0x1

//把v0中的值赋给com.aaa.IsRegistered，p0代表this，相当于this.Isregistered=true
iput-boolean v0,p0,Lcom/aaa;->IsRegisterd:Z
```



## 成员变量

成员变量定义格式为：

```java
.field public/private [static][final] varName:<类型>
```

获取指令：`iget, sget, iget-boolean, sget-boolean, iget-object, sget-object`

操作指令：`iput, sput, iput-boolean, sput-boolean, iput-object, sput-object`

**提示：**`array`的操作是`aget`和`aput`

举例：

```java
//获取ID这个String类型的成员变量并放到v0这个寄存器中
sget-object v0,Lcom/aaa;->ID:Ljava/lang/String;

//iget-object比sget-object多一个参数p0，这个参数代表变量所在类的实例。这里p0就是this
iget-object v0,p0,Lcom/aaa;->view:Lcom/aaa/view;
```

代码示例：

```java
//相当于java代码：this.timer = null;
const/4 v3, 0x0
sput-object v3, Lcom/aaa;->timer:Lcom/aaa/timer;

//相当于java代码：args.what = 18;其中args为Message的实例
.local v0, args:Landroid/os/Message;
const/4 v1, 0x12
iput v1,v0,Landroid/os/Message;->what:I
```



## 函数

函数定义格式为：

```java
.method public/private [static][final] methodName()<类型> 
......
.end method
```

代码示例：

```java
.method private ifRegistered()Z
    .locals 2            // 本地寄存器的个数
    .prologue
    const/4 v0, 0x1      //v0赋值为1
    if-eqz v0, :cond_0   //判断v0是否等于0，等于0则跳到cond_0执行
    const/4 v1, 0x1      //符合条件分支
    :goto_0              //标签
    return v1            //返回v1的值
    :cond_0              //标签
    const/4 v1, 0x0      //cond_0分支
    goto :goto_0         //跳到goto_0执行
.end method
```

函数分为两类：`direct method`和`virtual method`，`direct method`就是`private`方法，`virtual method`就是指其余的方法

```java
//调用格式：
invoke-指令类型 {参数1, 参数2,...}, L类名;->方法名

//调用指令：
invoke-direct
invoke-virtual
invoke-static
invoke-super
invoke-interface

//返回结果
move-result
move-result-object
```

**注意：**如果不是是静态方法，参数1代表调用该方法的实例

代码示例：

```rust
//相当于java代码：System.loadLibrary("NDKLIB")
const-string v0, "NDKLIB"
invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V

//表示将方法t返回的String对象保存到v2中
const-string v0, "Eric"
invoke-static {v0}, Lcmb/pbi;->t(Ljava/lang/String;)Ljava/lang/String;
move-result-object v2
```
