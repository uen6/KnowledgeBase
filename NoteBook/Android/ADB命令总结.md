# ADB命令总结



## 基础命令

| 命令                               | 作用                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| adb root / unroot                  | 获取root权限                                                 |
| adb disable-verity                 | 关闭分区检测功能，执行adb disable-verity后需要重启设备       |
| adb remount                        | 将system分区重新挂载为可读写分区                             |
| adb reboot [bootloader\|recovery]  | 重启，可增加参数<br />bootloader是重启到Fastboot模式<br />recovery是进入恢复模式 |
| adb install [-r] xxx.apk           | 安装指定apk                                                  |
| adb uninstall [-k] xxx.apk         | 卸载指定apk                                                  |
| adb pull <手机文件路径> <本机路径> | 从手机中拷贝文件                                             |
| adb push <本机文件路径> <手机路径> | 上传文件到手机中                                             |





## 设备连接

> ADB的默认端口为 5037，它通过扫描 5555 到 5585 之间（该范围供前 16 个模拟器使用）的奇数号端口查找模拟器

| 命令                    | 作用                                   |
| ----------------------- | -------------------------------------- |
| **adb devices [-l]**    | 查看连接计算机的设备，-l可显示更多信息 |
| adb -s 设备号           | 当有多个设备时，可指定连接某个设备     |
| **adb connect ip:port** | 无线adb                                |
| adb disconnect ip:port  | 断开无线adb                            |
| adb start-server        | 重启adb服务进程                        |
| **adb kill-server**     | 终止adb服务进程                        |





## am(Activity管理器)

```shell
例：am start -n com.android.mms/.ui.ConversationList #打开短信会话列表
例：am start -a android.settings.SETTINGS #打开系统设置页面
例：am start -a android.intent.action.DIAL -d tel:10086 #打开拨号页面
```

### start

| 命令                              | 作用                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| **start [options] intent**        | 启动由 intent 指定的 Activity <br />`--user user_id | current`：指定要作为哪个用户运行；如果未指定，则作为当前用户运行<br />`-D`：启用调试功能<br />`-W`：等待启动完成<br />`-R count`：重复启动 activity `count` 次。在每次重复前，将完成顶层 activity<br />`-S`：在启动 activity 前，强行停止目标应用 |
| **startservice [options] intent** | 启动由 intent 指定的 Service <br />`--user user_id | current`：指定要作为哪个用户运行；如果未指定，则作为当前用户运行 |
| **broadcast [options] intent**    | 发出广播 intent <br />`--user user_id | all | current`：指定要作为哪个用户运行；如果未指定，则作为当前用户运行 |

**intent**

| 命令                 | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| **-n \<component\>** | 指定带有软件包名称前缀的组件名称以创建显式 intent，<br />如 `com.example.app/.ExampleActivity` |
| -a \<action\>        | 指定 intent 操作，如 `android.intent.action.VIEW`。<br />只能声明一次 |
| -c \<category\>      | 指定 intent 类别，如 `android.intent.category.APP_CONTACTS`  |
| -d <data_uri>        | 指定 intent 数据 URI，如 `content://contacts/people/1`。<br />只能声明一次 |


### stop

| 命令                   | 作用                                                         |
| ---------------------- | ------------------------------------------------------------ |
| force-stop package     | 强行停止与`package`（应用的软件包名称）关联的所有进程        |
| kill [options] package | 终止与 `package`（应用的软件包名称）关联的所有进程。此命令仅终止可安全终止且不会影响用户体验的进程<br />`--user user_id | all | current`：指定要作为哪个用户运行；如果未指定，则作为当前用户运行 |
| kill-all               | 终止所有后台进程                                             |


### display

| 命令                                 | 作用                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| display-size [reset \| widthxheight] | 替换设备显示尺寸。此命令支持使用大屏设备模仿小屏幕分辨率（反之亦然）<br />示例：`am display-size 1280x800` |
| display-density dpi                  | 替换设备显示密度。此命令支持使用低密度屏幕在高密度屏幕环境上进行测试（反之亦然）<br />示例：`am display-density 480` |


### other

| 命令              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| monitor [options] | 开始监控崩溃或 ANR<br />`--gdb`：在崩溃/ANR 时，在给定的端口上启动 gdbserv |





## pm(软件包管理器)

### 应用管理

| 命令                                       | 作用                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| **list packages [options] \<stringName\>** | 输出所有软件包，或者，仅输出软件包名称包含 `stringName` 中的文本的软件包<br />`-f`：查看它们的关联文件<br />`-d`：进行过滤以仅显示已停用的软件包<br />`-e`：进行过滤以仅显示已启用的软件包<br />`-s`：进行过滤以仅显示系统软件包<br />`-3`：进行过滤以仅显示第三方软件包<br />`-i`：查看软件包的安装程序<br />`-u`：也包括已卸载的软件包<br />`--user user_id`：要查询的用户空间 |
| **install [options] path**                 | 将软件包（通过 `path` 指定）安装到系统<br /> `-r`：重新安装现有应用，并保留其数据<br />`-t`：允许安装测试 APK<br />`-i installer_package_name`：指定安装程序软件包名称<br />`--install-location location`：使用以下某个值设置安装位置：<br />    `0`：使用默认安装位置<br />    `1`：在内部设备存储上安装<br />    `2`：在外部介质上安装<br />`-f`：在内部系统内存上安装软件包<br />`-d`：允许版本代码降级<br />**`-g`：授予应用清单中列出的所有权限**<br />`--fastdeploy`：通过仅更新已更改的 APK 部分来快速更新安装的软件包 |
| **uninstall [options] package**            | 从系统中移除软件包<br />`-k`：移除软件包后保留数据和缓存目录 |
| path package                               | 输出给定包名的 APK 的路径                                    |
| clear package                              | 删除与软件包关联的所有数据                                   |
| set-install-location location              | 更改默认安装位置。位置值如下：<br />`0`：使用默认安装位置<br />`1`：在内部设备存储上安装<br />`2`：在外部介质上安装 |
| get-install-location                       | 返回当前安装位置。返回值如下：<br />`0` [auto]：让系统决定最合适的位置<br />`1` [internal]：在内部设备存储上安装<br />`2` [external]：在外部介质上安装 |

### 权限控制

| 命令                                               | 作用                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| **grant package_name permission**                  | 向应用授予权限。在搭载 Android 6.0（API 级别 23）及更高版本的设备上，该权限可以是应用清单中声明的任何权限。<br />在搭载 Android 5.1（API 级别 22）及更低版本的设备上，该权限必须是应用定义的可选权限 |
| **revoke package_name permission**                 | 从应用撤消权限。在搭载 Android 6.0（API 级别 23）及更高版本的设备上，该权限可以是应用清单中声明的任何权限。<br />在搭载 Android 5.1（API 级别 22）及更低版本的设备上，该权限必须是应用定义的可选权限 |
| list permission-groups                             | 输出所有已知的权限组                                         |
| list permissions [options] group                   | 输出所有已知的权限，或者，仅输出 `group` 中的权限<br />`-g`：按组进行整理<br />`-f`：输出所有信息<br />`-s`：简短摘要<br />`-d`：仅列出危险权限<br />`-u`：仅列出用户将看到的权限 |
| set-permission-enforced permission [true \| false] | 指定是否应强制执行指定权限                                   |

注意：对于某些权限，`grant`命令可能会无法授予，例如：`MANAGE_EXTERNAL_STORAGE`权限。此时可用`adb shell appops set --uid com.company.name MANAGE_EXTERNAL_STORAGE allow`的方式授予

### 系统信息

| 命令           | 作用                     |
| -------------- | ------------------------ |
| list libraries | 输出当前设备支持的所有库 |
| list features  | 输出系统的所有功能       |





## 多用户

### pm

| 命令                                           | 作用                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| **pm list users**                              | 输出系统中的所有用户                                         |
| pm get-max-users                               | 输出设备支持的最大用户数                                     |
| pm create-user \<user_name\>                   | 创建具有给定 `user_name` 的新用户，从而输出该用户的新用户标识符 |
| pm remove-user \<user_id\>                     | 移除具有给定 `user_id` 的用户，从而删除与该用户关联的所有数据 |
| pm disable-user [options] package_or_component | 具体选项：`--user user_id`：要停用的用户                     |
| pm enable --user \<userId\>                    |                                                              |
| pm list packages --user \<userId\>             | 可为特定用户列出软件包<br />`-e` 可列出已启用的软件包<br />`-d` 可列出已停用的软件包 |

### am

| 命令                           | 作用                    |
| ------------------------------ | ----------------------- |
| **am switch-user \<user_id\>** | 切换到指定用户          |
| **am get-current-user**        | 获取当前（前台）用户 ID |
| am start-user \<user_id\>      | 启动指定用户            |

### 多用户下的安装和卸载

```shell
adb install --user <userId>

adb uninstall --user <userId> 
```





## logcat

| 参数              | 作用                                                         |
| ----------------- | ------------------------------------------------------------ |
| **-v \<format\>** | 设置日志消息的输出格式                                       |
| **-c**            | 清空缓冲区（main、system 和 crash），如需清除所有缓冲区，请使用 `logcat -b all -c` |
| **--pid=\<pid\>** | 仅输出来自给定 PID 的日志                                    |
| -b                | 加载可供查看的备用日志缓冲区，例如 `events` 或 `radio`。默认使用 `main`、`system` 和 `crash` 缓冲区集 |
| -e \<expr\>       | 只输出日志消息与 `<expr>` 匹配的行，其中 `<expr>` 是正则表达式 |
| -m \<count\>      | 输出 `<count>` 行后退出，一般和``-e`参数共用                 |
| -d                | 将日志转储到屏幕并退出                                       |
| -D                | 输出各个日志缓冲区之间的分隔线                               |
| -f \<filename\>   | 将日志消息输出写入 `<filename>`。默认值为 `stdout`           |
| -g                | 输出指定日志缓冲区的大小并退出                               |
| -G \<size\>       | 设置日志环形缓冲区的大小。可以在结尾处添加 `K` 或 `M`，以指示单位为千字节或兆字节 |
| -t \<count\>      | 仅输出最新的行数。此选项包括 `-d` 功能                       |
| -t '\<time\>'     | 输出自指定时间以来的最新行，例：`adb logcat -t '01-26 20:52:41.820'` |
| -T \<count\>      | 输出自指定时间以来的最新行数。此选项不包括 `-d` 功能         |
| -T '\<time\>'     | 输出自指定时间以来的最新行。此选项不包括 `-d` 功能           |
| -L                | 在最后一次重新启动之前转储日志                               |

### 输出格式化

```shell
#控制日志输出格式
-v <format>

#注意：-v 只能指定一种格式化，但可以指定多个格式化修饰符
例：adb logcat -v thread
例：adb logcat -v color
```

| 格式化     | 含义                                                         |
| :--------- | :----------------------------------------------------------- |
| **time**   | 显示日期、调用时间、优先级、标记以及发出消息的进程的 PID     |
| threadtime | 默认：显示日期、调用时间、优先级、标记、PID 以及发出消息的线程的 TID |
| brief      | 显示优先级、标记以及发出消息的进程的 PID                     |
| long       | 显示所有元数据字段，并使用空白行分隔消息                     |
| process    | 仅显示 PID                                                   |
| raw        | 显示不包含其他元数据字段的原始日志消息                       |
| tag        | 仅显示优先级和标记                                           |
| thread     | 旧版格式，显示优先级、PID 以及发出消息的线程的 TID           |

| 格式化修饰符 | 含义                                                   |
| ------------ | ------------------------------------------------------ |
| color        | 使用不同的颜色来显示每个优先级                         |
| uid          | 如果访问控制允许，则显示 UID 或记录的进程的 Android ID |
| usec         | 显示精确到微秒的时间                                   |
| UTC          | 显示 UTC 时间                                          |
| year         | 将年份添加到显示的时间                                 |
| zone         | 将本地时区添加到显示的时间                             |

### 调整缓冲区

```shell
#加载一个可使用的日志缓冲区供查看
-b <Buffer>

例：logcat -b radio
例：logcat -b main -b radio -b events
例：logcat -b main,radio,events
```

| 缓冲区    | 含义                                               |
| :-------- | :------------------------------------------------- |
| **all**   | 查看所有缓冲区                                     |
| **crash** | 查看崩溃日志缓冲区（默认）                         |
| radio     | 查看包含无线装置/电话相关消息的缓冲区              |
| events    | 查看已经过解译的二进制系统事件缓冲区消息           |
| main      | 查看主日志缓冲区（默认），不包含系统和崩溃日志消息 |
| system    | 查看系统日志缓冲区（默认）                         |
| default   | 报告 `main`、`system` 和 `crash` 缓冲区            |

### 调整级别

```shell
#将所有标记的优先级设为“静默”
adb logcat "*:W"

#抑制除标记为“ActivityManager”、优先级不低于“Info”的日志消息，以及标记为“MyApp”、优先级不低于“Debug”的日志消息以外的所有其他日志消息
adb logcat ActivityManager:I MyApp:D *:S
```

| 级别 | 含义                                                   |
| :--- | :----------------------------------------------------- |
| :V   | 过滤只显示 Verbose 及以上级别(优先级最低)              |
| :D   | 过滤只显示 Debug 及以上级别                            |
| :I   | 过滤只显示 Info 及以上级别                             |
| :W   | 过滤只显示 Warning 及以上级别                          |
| :E   | 过滤只显示 Error 及以上级别                            |
| :F   | 过滤只显示 Fatal 及以上级别                            |
| :S   | 过滤只显示 Silent 及以上级别(优先级最高，什么也不输出) |





## input

### 硬按键模拟

```bash
input keyevent [--longpress] [key值]
```

| key值 | 对应按键 |
| ----- | -------- |
|**3**|**HOME 键**|
|**4**|**返回键**|
|5|打开拨号应用|
|6|挂断电话|
|24|增加音量|
|25|降低音量|
|**26**|**电源键**|
|27|拍照（需要在相机应用里）|
|64|打开浏览器|
|82|菜单键|
|85|播放/暂停|
|86|停止播放|
|87|播放下一首|
|88|播放上一首|
|122|移动光标到行首或列表顶部|
|123|移动光标到行末或列表底部|
|126|恢复播放|
|127|暂停播放|
|164|静音|
|176|打开系统设置|
|187|切换应用|
|207|打开联系人|
|208|打开日历|
|209|打开音乐|
|210|打开计算器|
|220|降低屏幕亮度|
|221|提高屏幕亮度|
|223|系统休眠|
|224|点亮屏幕|
|231|打开语音助手|
|276|如果没有 wakelock 则让系统休眠|

### 文本输入

在焦点处于某文本框时，可以通过 `adb shell input text hello`，来输入一串文本

### 手势模拟

```
input [tap | swipe | draganddrop | press | roll]
```


如果需要实现滑动动作，可以设置：`adb shell input swipe 起始点x坐标 起始点y坐标 结束点x坐标 结束点y坐标`





## monkey

```shell
例：adb shell monkey -p your.package.name -v 500 #启动您的应用并向其发送500个伪随机事件
```

### 常规

| 命令                        | 作用                                                         |
| --------------------------- | ------------------------------------------------------------ |
| -v                          | 命令行上的每一个-v都将增加反馈信息的详细级别（最多使用3个-v）<br />级别 0（默认值）只提供启动通知、测试完成和最终结果<br />级别 1 提供有关测试在运行时的更多详细信息，例如发送到您的 Activity 的各个事件<br />级别 2 提供更详细的设置信息，例如已选择或未选择用于测试的 Activity<br />用法：`adb shell "monkey -v -v -v"` |
| -s \<seed\>                 | 伪随机数生成器的种子值<br />如果您使用相同的种子值重新运行 Monkey，它将会生成相同的事件序列 |
| --throttle \<milliseconds\> | 在事件之间插入固定的延迟时间。您可以使用此选项减慢 Monkey 速度<br />如果未指定，则不延迟，系统会尽快地生成事件 |

### 调整触摸事件

| 命令                        | 作用                                                         |
| --------------------------- | ------------------------------------------------------------ |
| --pct-touch \<percent\>     | 调整轻触事件所占百分比                                       |
| --pct-motion \<percent\>    | 调整动作事件所占百分比（动作事件包括屏幕上某个位置<br />的按下事件，一系列伪随机动作和一个释放事件） |
| --pct-trackball \<percent\> | 调整轨迹球事件所占百分比（轨迹球事件包括<br />一个或多个随机动作，有时后跟点击） |
| --pct-nav \<percent\>       | 调整“基本”导航事件所占百分比（导航事件包括<br />向上/向下/向左/向右，作为方向输入设备的输入） |
| --pct-majornav \<percent\>  | 调整“主要”导航事件所占百分比（这些导航事件通常会导致<br />界面中的操作，例如 5 方向键的中间按钮、返回键或菜单键） |
| --pct-syskeys \<percent\>   | 调整“系统”按键事件所占百分比（这些按键通常预留供系统使用，<br />例如“主屏幕”、“返回”、“发起通话”、“结束通话”或“音量控件”） |
| --pct-appswitch \<percent\> | 调整 Activity 启动次数所占百分比，Monkey 会以随机间隔发起<br />startActivity() 调用，以最大限度地覆盖软件包中的所有 Activity |
| --pct-anyevent \<percent\>  | 调整其他类型事件所占百分比。这包括所有其他类型的事件，<br />例如按键、设备上的其他不太常用的按钮等等 |

### 调试

| 命令                         | 作用                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| --ignore-crashes             | 通常，当应用崩溃或遇到任何类型的未处理异常时，Monkey 将会停止。<br />如果指定此选项，Monkey 会继续向系统发送事件，直到计数完成为止。 |
| --ignore-timeouts            | 通常，当应用遇到任何类型的超时错误（例如“应用无响应”对话框）时，Monkey 将会停止。<br />如果指定此选项，Monkey 会继续向系统发送事件，直到计数完成为止。 |
| --ignore-security-exceptions | 通常，当应用遇到任何类型的权限错误（例如，如果它尝试启动需要<br />特定权限的 Activity）时，Monkey 将会停止。如果指定此选项，<br />Monkey 会继续向系统发送事件，直到计数完成为止。 |
| --kill-process-after-error   | 通常，当 Monkey 因出错而停止运行时，出现故障的应用将保持运行状态。<br />设置此选项后，它将会指示系统停止发生错误的进程。注意，在正常（成功）<br />完成情况下，已启动的进程不会停止，并且设备仅会处于最终事件之后的最后状态 |

### 指定文件

| 命令                               | 作用                                   |
| ---------------------------------- | -------------------------------------- |
| 1> /sdcard/info.txt                | 表示一般信息输出到/sdcard/info.txt     |
| 2> /sdcard/error.txt               | 表示错误信息输出到/sdcard/error.txt    |
| --pkg-whitelist-file \<file_name\> | 指定白名单（白名单：只测试名单内的）   |
| --pkg-blacklist-file \<file_name\> | 指定黑名单（黑名单：不测试该名单内的） |

### monkey结果分析

在monkey运行结束后，会输出测试结果。其中最后几行如下，代表了运行过程中的一些信息。

```tex
//显示Monkey 执行失败
** Monkey aborted due to error.

//执行的事件数量，如果该数字小于你的预定值，说明提前终止了
Events injected: 6258

//旋转的角度为0
:Sending rotation degree=0, persist=false

//丢失的事件数量，按键0，提示4，轨迹球0，翻转21，旋转0
:Dropped: keys=0 pointers=4 trackballs=0 flips=21 rotations=0

//网络状态，移动网络 0ms ，Wi-Fi 0ms ，无网 144ms
## Network stats: elapsed time=465222ms (0ms mobile, 0ms wifi, 465222ms not connected)

//提示在执行到第8 个事件时出现Crash ，以及所使用的随机种子值
** System appears to have crashed at event 8 of 10 using seed 1454215444564
```





## dumpsys

```
adb shell dumpsys [options]
```

| 参数         | 作用                           |
| ------------ | ------------------------------ |
| **meminfo**  | **内存**                       |
| **activity** | **显示所有的activities的信息** |
| cpuinfo      | CPU                            |
| gfxinfo      | 帧率                           |
| display      | 显示                           |
| power        | 电源                           |
| battery      | 电池                           |
| batterystats | 电池状态                       |
| location     | 位置                           |
| alarm        | 闹钟                           |
| account      | accounts                       |
| window       | 显示键盘，窗口和它们的关系     |
| wifi         | 显示wifi信息                   |

```shell
# 查看指定应用的相关信息
dumpsys package com.example.XXX

# 查看顶部activity
dumpsys activity top | grep ACTIVITY

# 查看正在运行的Services
dumpsys activity services [<packagename>]

# 获取过去三小时内应用的内存占用情况统计信息（采用简单易懂的格式）
dumpsys procstats --hours 3
```





## prop(系统属性)

**注意：**此命令运行需要root环境

| 命令                                | 作用                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| **getprop** \<prop-name\>           | 获取系统属性                                                 |
| **setprop** \<prop-name\> \<value\> | 修改系统属性                                                 |
| watchprops                          | 监听系统属性的变化（在此期间，如果系统的属性发生变化则将变化的值显示出来） |

```shell
# 获取系统时区
getprop persist.sys.timezone

# 设置系统时区
setprop persist.sys.timezone Pacific/Midway
```





## WM(显示相关)

### 分辨率修改

| 命令             | 作用                    |
| ---------------- | ----------------------- |
| wm size 1422x720 | 将分辨率修改为 1422x720 |
| wm size reset    | 恢复原分辨率            |

### 屏幕密度修改

| 命令             | 作用                  |
| ---------------- | --------------------- |
| wm density 160   | 屏幕密度修改为 160dpi |
| wm density reset | 恢复原屏幕密度        |

### 显示区域修改

| 命令                  | 作用                                                         |
| --------------------- | :----------------------------------------------------------- |
| wm overscan 0,0,0,100 | 修改显示区域，四个数字分别表示距离左、上、右、下边缘的留白像素<br />以上命令表示将屏幕底部 100px留白 |
| wm overscan reset     | 恢复显示区域                                                 |





## 截图、录屏

**截图**

```shell
adb shell screencap <filename>

#例：
adb shell screencap /sdcard/screen.png
adb pull /sdcard/screen.png
```
**录屏**

```shell
screenrecord [options] filename

#例1：
adb shell screenrecord /sdcard/demo.mp4
adb pull /sdcard/demo.mp4

#例2：
screenrecord --bit-rate 6000000 /sdcard/demo.mp4
```

| 参数                | 作用                                                         |
| ------------------- | ------------------------------------------------------------ |
| --size widthxheight | 默认值为设备的本机显示屏分辨率；如果不支持，则为 1280x720    |
| --bit-rate rate     | 设置视频的视频比特率（以 MB/秒为单位）。默认值为4Mbps        |
| --time-limit time   | 设置最大录制时长（以秒为单位）。默认值和最大值均为180（3分钟） |
| --rotate            | 将输出旋转 90 度。此功能处于实验阶段                         |
| --verbose           | 在命令行屏幕显示日志信息                                     |

需要停止时按`Ctrl-C`，默认情况下，该实用程序以本机显示分辨率和屏幕方向进行录制，时长不超过三分钟

局限性：

-   音频不与视频文件一起录制
-   无法在搭载 Wear OS 的设备上录制视频
-   某些设备可能无法以它们的本机显示分辨率进行录制。如果在录制屏幕时出现问题，请尝试使用较低的屏幕分辨率
-   不支持在录制时旋转屏幕。如果在录制期间屏幕发生了旋转，则部分屏幕内容在录制时将被切断
