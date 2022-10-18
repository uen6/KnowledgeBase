# Shell笔记总结



## 创建一个脚本

```shell
$ touch testShell.sh
$ vim testShell.sh
#! /bin/bash
echo "hello word" #:wq保存退出
~
~
~
~
$ . testShell.sh #用.运行脚本
```



## 变量

```shell
:<<!此处是多行注释（多行注释写法有很多）
有效的变量名
RUNOOB
LD_LIBRARY_PATH
_var
var2
!

# 使用变量
your_name="aaaa"
echo $your_name
your_name="bbbb"
echo ${your_name}

# 变量名外面的花括号是可选的，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：
echo "I am good at ${skill}Script"

# 只读变量
myUrl="ccc"
readonly myUrl
myUrl="ddd" #出错，变量值不能再被重新赋值

# 删除变量
myUrl="eee"
unset myUrl
echo $myUrl #出错，无法输出任何结果
```



## 数组

```shell
# 数组变量
array_name=(value0 value1 value2 value3)
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen

# 使用数组变量
valuen=${array_name[n]}
echo ${array_name[@]} # 使用@符号可以获取数组中的所有元素

# 获取数组的长度(对变量使用#号可获取数组长度)
length=${#array_name[@]} # 或者${#array_name[*]}
lengthn=${#array_name[n]} # 取得数组单个元素的长度
```



## 字符串

**单引号**

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；

- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用

**双引号**

- 双引号里可以有变量
- 双引号里可以出现转义字符

```shell
# 拼接字符串
your_name="aaa"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
# 使用单引号拼接
greeting_2='hello, '$your_name' !'

# 获取字符串长度
string="abcd"
echo ${#string} # 输出 4

# 提取子字符串
string="runoob is a great site" # 从字符串第2个字符开始截取4个字符
echo ${string:1:4} # 输出unoo

# 查找子字符串
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```



## 运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。expr 是一款表达式计算工具，使用它能完成表达式的求值操作。例如两个数相加：

```shell
val=`expr 2 + 2`
echo "两数之和为 : $val"
```

**注意：**

- 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2
- 完整的表达式要被`符号包含，注意这个字符不是常用的单引号，在 Esc 键下边

**算术运算符**（假定变量 a 为 10，变量 b 为 20）

|      | 说明                                          | 举例                        |
| :--- | :-------------------------------------------- | :-------------------------- |
| +    | 加法                                          | `expr $a + $b` 结果为 30    |
| -    | 减法                                          | `expr $a - $b` 结果为 -10   |
| *    | 乘法                                          | `expr $a \* $b` 结果为  200 |
| /    | 除法                                          | `expr $b / $a` 结果为 2     |
| %    | 取余                                          | `expr $b % $a` 结果为 0     |
| =    | 赋值                                          | a=$b 将把变量 b 的值赋给 a  |
| ==   | 相等。用于比较两个数字，相同则返回 true。     | [ \$a == \$b ] 返回 false   |
| !=   | 不相等。用于比较两个数字，不相同则返回 true。 | [ \$a != \$b ] 返回 true    |

**注意：**代码中的 [] 执行基本的算数运算，条件表达式要放在方括号之间，并且要有空格，例如: [\$a==\$b] 是错误的，必须写成[ \$a == \$b ]

**关系运算符**（假定变量 a 为 10，变量 b 为 20）

|      | 说明                                                  | 举例                       |
| :--- | :---------------------------------------------------- | :------------------------- |
| -eq  | 检测两个数是否相等，相等返回 true。                   | [ \$a -eq \$b ] 返回 false |
| -ne  | 检测两个数是否不相等，不相等返回 true。               | [ \$a -ne \$b ] 返回 true  |
| -gt  | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ \$a -gt \$b ] 返回 false |
| -lt  | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ \$a -lt \$b ] 返回 true  |
| -ge  | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ \$a -ge \$b ] 返回 false |
| -le  | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ \$a -le \$b ] 返回 true  |

**注意：**关系运算符只支持数字，不支持字符串，除非字符串的值是数字

**布尔运算符**（假定变量 a 为 10，变量 b 为 20）

|      | 说明                                              | 举例                                     |
| :--- | :------------------------------------------------ | :--------------------------------------- |
| !    | 非运算，表达式为 true 则返回 false，否则返回 true | [ ! false ] 返回 true                    |
| -o   | 或运算，有一个表达式为 true 则返回 true           | [ \$a -lt 20 -o \$b -gt 100 ] 返回 true  |
| -a   | 与运算，两个表达式都为 true 才返回 true           | [ \$a -lt 20 -a \$b -gt 100 ] 返回 false |

**逻辑运算符**（假定变量 a 为 10，变量 b 为 20）

|      | 说明       | 举例                                         |
| :--- | :--------- | :------------------------------------------- |
| &&   | 逻辑的 AND | [[ \$a -lt 100 && \$b -gt 100 ]] 返回 false  |
| \|\| | 逻辑的 OR  | [[ \$a -lt 100 \|\| \$b -gt 100 ]] 返回 true |

**字符串运算符**（假定变量 a 为 "abc"，变量 b 为 "efg"）

|      | 说明                                       | 举例                     |
| :--- | :----------------------------------------- | :----------------------- |
| =    | 检测两个字符串是否相等，相等返回 true      | [ \$a = \$b ] 返回 false |
| !=   | 检测两个字符串是否不相等，不相等返回 true  | [ \$a != \$b ] 返回 true |
| -z   | 检测字符串长度是否为0，为0返回 true        | [ -z $a ] 返回 false     |
| -n   | 检测字符串长度是否不为 0，不为 0 返回 true | [ -n "$a" ] 返回 true    |
| \$    | 检测字符串是否为空，不为空返回 true        | [ \$a ] 返回 true        |

**文件测试运算符**

|         | 说明                                                         | 举例                    |
| :------ | :----------------------------------------------------------- | :---------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true                | [ -b $file ] 返回 false |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true              | [ -c $file ] 返回 false |
| -d file | 检测文件是否是目录，如果是，则返回 true                      | [ -d $file ] 返回 false |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true | [ -f $file ] 返回 true  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true              | [ -g $file ] 返回 false |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true    | [ -k $file ] 返回 false |
| -p file | 检测文件是否是有名管道，如果是，则返回 true                  | [ -p $file ] 返回 false |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true              | [ -u $file ] 返回 false |
| -r file | 检测文件是否可读，如果是，则返回 true                        | [ -r $file ] 返回 true  |
| -w file | 检测文件是否可写，如果是，则返回 true                        | [ -w $file ] 返回 true  |
| -x file | 检测文件是否可执行，如果是，则返回 true                      | [ -x $file ] 返回 true  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true       | [ -s $file ] 返回 true  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true            | [ -e $file ] 返回 true  |
| -S      | 判断某文件是否 socket                                        |                         |
| -L      | 检测文件是否存在并且是一个符号链接                           |                         |



## 输出

```shell
echo "It is a test"
echo It is a test

# 显示转义字符
echo "\"It is a test\""

# 显示变量
read name #从标准输入中读取一行
echo "$name It is a test"

# 显示换行
echo -e "OK! \n" # -e开启转义
echo "It is a test"

# 显示不换行
echo -e "OK! \c" # -e开启转义\c不换行
echo "It is a test"

# 显示结果定向至文件
echo "It is a test" > myfile

# 原样输出字符串，不进行转义或取变量(用单引号)
echo '$name\"'

# 显示命令执行结果
echo `date` # 注意：这里使用的是反引号(`)
```

**printf命令**

```shell
# 格式：printf  format-string  [arguments...]
# format-string: 为格式控制字符串
# arguments: 为参数列表

# 例：
printf "Hello, Shell\n"
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg
```



## 流程控制

**if**

```shell
# if condition1
# then
#     command1
# elif condition2
# then
#     command2
# else
#     commandN
# fi

#例：
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

if test $[num1] -eq $[num2]
then
    echo '两个数字相等!'
else
    echo '两个数字不相等!'
fi
```

**for**

```shell
# for var in item1 item2 ... itemN
# do
#     command1
#     command2
#     ...
#     commandN
# done

for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done

for str in aa bb cc dd
do
    echo $str # 结果：aa bb cc dd
done
```

**while**

```shell
# while condition
# do
#     command
# done

int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done

# 无限循环语法格式
while : # 或者：while true 或 for (( ; ; ))
do
    command
done
```

**case ... esac**

```shell
# case 值 in
# 模式1)
#     command1
#     ;; # 两个分号;;表示break
# 模式2)
#     command2
#     ;;
# esac

read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字' # 如果无一匹配模式，使用星号*捕获该值，再执行后面的命令
    ;;
esac

```

**break\continue**

break命令允许跳出所有循环（终止执行后面的所有循环）

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环

```shell
# break
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done

# continue
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```



## 函数

1. 可以带function fun() 定义，也可以直接fun() 定义，不带任何参数

2. 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。return后跟数值n(0-255)

```shell
# [ function ] funname [()]
# {
#     action;
#     [return int;]
# }

# 一个简单的函数
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```
**注意：**所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可

```shell
# 带有return语句的函数
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```
**注意：**

- 函数返回值在调用该函数后通过\$?来获得，\$?仅对其上一条指令负责，一旦函数返回后其返回值没有立即保存入参数，那么其返回值将不再能通过\$?获得

- 和 C 语言不同，shell 语言中 0 代表 true，0 以外的值代表 false

```shell
# 使用函数参数
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !" # 大于10个参数时，需要使用花括号
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
# 运行结果：
# 第一个参数为 1 !
# 第二个参数为 2 !
# 第十个参数为 10 !
# 第十个参数为 34 !
# 第十一个参数为 73 !
# 参数总数有 11 个!
# 作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```

| 参数    | 说明                                                        |
| :------ | :---------------------------------------------------------- |
| **\$#** | 传递到脚本或函数的参数个数                                  |
| **\$*** | 以一个单字符串显示所有向脚本传递的参数                      |
| \$$     | 脚本运行的当前进程ID号                                      |
| \$!     | 后台运行的最后一个进程的ID号                                |
| \$@     | 与\$*相同，但是使用时加引号，并在引号中返回每个参数         |
| \$-     | 显示Shell使用的当前选项，与set命令功能相同                  |
| \$?     | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误 |

**注意：**\$10 不能获取第十个参数，获取第十个参数需要\${10}。当n>=10时，需要使用\${n}来获取参数



## 输入输出重定向

| 命令            | 说明                                             |
| :-------------- | :----------------------------------------------- |
| command > file  | 将输出重定向到 file                              |
| command < file  | 将输入重定向到 file                              |
| command >> file | 将输出以追加的方式重定向到 file                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file |
| n >& m          | 将输出文件 m 和 n 合并                           |
| n <& m          | 将输入文件 m 和 n 合并                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入 |

```shell
$ echo "aaaa" > users
$ cat users
aaaa
$ echo "bbb" >> users
$ cat users
bbb
aaaa

# 如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null
$ command > /dev/null
```



## 文件调用

```shell
#使用 . 号来引用test1.sh 文件
. ./test1.sh

# 或者使用以下包含文件代码
source ./test1.sh
```
