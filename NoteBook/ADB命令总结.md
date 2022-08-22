# ADB命令总结



## 基础命令

| 命令                               | 作用                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| `adb remount`                      | 将system分区重新挂载为可读写分区                             |
| `adb reboot [bootloader|recovery]` | 重启，可增加参数<br />bootloader是重启到Fastboot模式<br />recovery是进入恢复模式 |
| `adb root`                         |                                                              |
| `adb unroot`                       |                                                              |
| `adb disable-verity`               |                                                              |
|                                    |                                                              |





## 设备连接

| 命令                     | 作用                                   |
| ------------------------ | -------------------------------------- |
| `adb devices [-l]`       | 查看连接计算机的设备，-l可显示更多信息 |
| `adb -s 设备号`          | 当有多个设备时，可指定连接某个设备     |
| `adb connect ip:port`    | 无线adb                                |
| `adb disconnect ip:port` | 断开无线adb                            |
| `adb start-server`       | 重启adb服务进程                        |
| `adb kill-server`        | 终止adb服务进程                        |





## 应用管理

| 命令                                 | 作用             |
| ------------------------------------ | ---------------- |
| `adb install [-r] xxx.apk`           | 安装指定apk      |
| `adb uninstall [-k] xxx.apk`         | 卸载指定apk      |
| `adb pull <手机文件路径> <本机路径>` | 从手机中拷贝文件 |
| `adb push <本机文件路径> <手机路径>` | 上传文件到手机中 |
|                                      |                  |
|                                      |                  |
|                                      |                  |





## wm

### 分辨率修改

| 命令               | 作用                    |
| ------------------ | ----------------------- |
| `wm size 1422x720` | 将分辨率修改为 1422x720 |
| `wm size reset`    | 恢复原分辨率            |

### 屏幕密度修改

| 命令               | 作用                  |
| ------------------ | --------------------- |
| `wm density 160`   | 屏幕密度修改为 160dpi |
| `wm density reset` | 恢复原屏幕密度        |

### 显示区域修改

| 命令                    | 作用                                                         |
| ----------------------- | :----------------------------------------------------------- |
| `wm overscan 0,0,0,100` | 修改显示区域，四个数字分别表示距离左、上、右、下边缘的留白像素<br />以上命令表示将屏幕底部 100px留白 |
| `wm overscan reset`     | 恢复显示区域                                                 |





## input

### 硬按键模拟

```
input keyevent [key值]
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





## am(Activity管理器)

```
例：am start -n com.android.mms/.ui.ConversationList #打开短信会话列表
例：am start -a android.settings.SETTINGS #打开系统设置页面
例：am start -a android.intent.action.DIAL -d tel:10086 #打开拨号页面
```

| 命令                                | 作用                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| **`start [options] intent`**        | 启动由`intent `指定的`Activity`                              |
| **`startservice [options] intent`** | 启动由`intent `指定的`Service`                               |
| **`broadcast [options] intent`**    | 发出广播 intent                                              |
| `to-uri intent`                     | 以 URI 的形式输出给定的 intent 规范                          |
| `to-intent-uri intent`              | 以 `intent:` URI 的形式输出给定的 intent 规范                |
| `kill [options] package`            | 终止与 `package`（应用的软件包名称）关联的所有进程。此命令仅终止可安全终止且不会影响用户体验的进程 |
| `force-stop package`                | 行停止与`package`（应用的软件包名称）关联的所有进程          |
| `kill-all`                          | 终止所有后台进程                                             |
| `profile start process file`        | 启动 `process` 的性能剖析器，将结果写入 `file`               |
| `profile stop process`              | 停止 `process` 的性能剖析器                                  |
| `clear-debug-app`                   | 清除之前使用 `set-debug-app` 设置的待调试软件包              |
| `monitor [options]`                 | 开始监控崩溃或 ANR<br />`--gdb`：在崩溃/ANR 时，在给定的端口上启动 gdbserv |

### options

| 命令                      | 作用                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `-D`                      | 启用调试功能                                                 |
| `-W`                      | 等待启动完成                                                 |
| `--start-profiler <file>` | 启动性能剖析器并将结果发送至 file                            |
| `-P file`                 | 类似于 `--start-profiler`，但当应用进入空闲状态时剖析停止    |
| `-R count`                | 重复启动 Activity `count` 次。在每次重复前，将完成顶层 Activity |
| `-S`                      | 在启动 Activity 前，强行停止目标应用                         |
| `--opengl-trace`          | 启用 OpenGL 函数的跟踪                                       |
| `--user user_id|current`  | 指定要作为哪个用户运行；如果未指定，则作为当前用户运行       |

### intent

| 命令                 | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| **`-n <component>`** | 指定带有软件包名称前缀的组件名称以创建显式 intent，<br />如 `com.example.app/.ExampleActivity` |
| `-a <action>`        | 指定 intent 操作，如 `android.intent.action.VIEW`。<br />只能声明一次 |
| `-c <category>`      | 指定 intent 类别，如 `android.intent.category.APP_CONTACTS`  |
| `-d <data_uri>`      | 指定 intent 数据 URI，如 `content://contacts/people/1`。<br />只能声明一次 |





## pm(软件包管理器)

### 应用

| 命令 | 作用 |
| ---- | ---- |
| **`install [options] path`**                  | 将软件包（通过 `path` 指定）安装到系统<br /> `-r`：重新安装现有应用，并保留其数据<br />`-t`：允许安装测试 APK。仅当您运行或调试了应用或者使用了 Android Studio 的 **Build > Build APK** 命令时，Gradle 才会生成测试 APK<br />`-i installer_package_name`：指定安装程序软件包名称<br />`--install-location location`：使用以下某个值设置安装位置：<br />    `0`：使用默认安装位置<br />    `1`：在内部设备存储上安装<br />    `2`：在外部介质上安装<br />`-f`：在内部系统内存上安装软件包<br />`-d`：允许版本代码降级<br />`-g`：授予应用清单中列出的所有权限<br />`--fastdeploy`：通过仅更新已更改的 APK 部分来快速更新安装的软件包<br />`--incremental`：仅安装 APK 中启动应用所需的部分，同时在后台流式传输剩余数据。如要使用此功能，您必须为 APK 签名，创建一个 APK 签名方案 v4 文件，并将此文件放在 APK 所在的目录中。只有部分设备支持此功能。此选项会强制 adb 使用该功能，如果该功能不受支持，则会失败（并提供有关失败原因的详细信息）。附加 --wait 选项，可等到 APK 完全安装完毕后再授予对 APK 的访问权限<br />`--no-incremental` 可阻止 adb 使用此功能 |
| **`uninstall [options] package`**             | 从系统中移除软件包。具体选项：<br />`-k`：移除软件包后保留数据和缓存目录 |
| **`list packages [options] <stringName>`**    | 输出所有软件包，或者，仅输出软件包名称包含 `stringName` 中的文本的软件包<br />`-f`：查看它们的关联文件<br />`-d`：进行过滤以仅显示已停用的软件包<br />`-e`：进行过滤以仅显示已启用的软件包<br />`-s`：进行过滤以仅显示系统软件包<br />`-3`：进行过滤以仅显示第三方软件包<br />`-i`：查看软件包的安装程序<br /> `-u`：也包括已卸载的软件包<br />`--user user_id`：要查询的用户空间 |
| `clear package`                               | 删除与软件包关联的所有数据                                   |
| `path com.android.xxx`                        | 输出给定包名的 APK 的路径                                    |


### 用户

| 命令                                          | 作用                                                         |
| --------------------------------------------- | ------------------------------------------------------------ |
| `list users`                                  | 输出系统中的所有用户                                         |
| `disable-user [options] package_or_component` | 具体选项：`--user user_id`：要停用的用户                     |
| `create-user user_name`                       | 创建具有给定 `user_name` 的新用户，从而输出该用户的新用户标识符 |
| `remove-user user_id`                         | 移除具有给定 `user_id` 的用户，从而删除与该用户关联的所有数据 |
| `get-max-users`                               | 输出设备支持的最大用户数                                     |

### 权限

| 命令                                                | 作用                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| `grant package_name permission`                     | 向应用授予权限。在搭载 Android 6.0（API 级别 23）及更高版本的设备上，该权限可以是应用清单中声明的任何权限。在搭载 Android 5.1（API 级别 22）及更低版本的设备上，该权限必须是应用定义的可选权限 |
| `revoke package_name permission`                    | 从应用撤消权限。在搭载 Android 6.0（API 级别 23）及更高版本的设备上，该权限可以是应用清单中声明的任何权限。在搭载 Android 5.1（API 级别 22）及更低版本的设备上，该权限必须是应用定义的可选权限 |
| `list permission-groups`                            | 输出所有已知的权限组                                         |
| `list permissions [options] group`                  | 输出所有已知的权限，或者，仅输出 `group` 中的权限<br />`-g`：按组进行整理<br />`-f`：输出所有信息<br />`-s`：简短摘要<br />`-d`：仅列出危险权限<br />`-u`：仅列出用户将看到的权限 |
| `set-permission-enforced permission [true | false]` | 指定是否应强制执行指定权限                                   |

### 其他

| 命令                             | 作用                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| `list libraries`                 | 输出当前设备支持的所有库                                     |
| `list features`                  | 输出系统的所有功能                                           |
| `enable package_or_component`    | 启用给定的软件包或组件（写为“package/class”）                |
| `disable package_or_component`   | 停用给定的软件包或组件（写为“package/class”）                |
| `set-install-location location`  | 更改默认安装位置。位置值如下：<br />`0`：自动：让系统决定最合适的位置<br />`1`：内部：在内部设备存储上安装<br />`2`：外部：在外部介质上安装 |
| `get-install-location`           | 返回当前安装位置。返回值如下：<br />`0 [auto]`：让系统决定最合适的位置<br />`1 [internal]`：在内部设备存储上安装<br />`2 [external]`：在外部介质上安装 |
| `trim-caches desired_free_space` | 减少缓存文件以达到给定的可用空间                             |





## logcat

```
-c				清空缓冲区（main、system 和 crash），如需清除所有缓冲区，请使用 -b all -c
-d				将日志转储到屏幕并退出
-f <filename>	将日志消息输出写入 <filename>。默认值为 stdout
--pid=<pid>		仅输出来自给定 PID 的日志
-g				打印缓冲区的大小
```

### 调整级别

```
#例：adb logcat "*:W"
```

| 级别 | 含义                                                   |
| :--- | :----------------------------------------------------- |
| *:V  | 过滤只显示 Verbose 及以上级别(优先级最低)              |
| *:D  | 过滤只显示 Debug 及以上级别                            |
| *:I  | 过滤只显示 Info 及以上级别                             |
| *:W  | 过滤只显示 Warning 及以上级别                          |
| *:E  | 过滤只显示 Error 及以上级别                            |
| *:F  | 过滤只显示 Fatal 及以上级别                            |
| *:S  | 过滤只显示 Silent 及以上级别(优先级最高，什么也不输出) |

### 输出格式化

```
-v <format>		控制日志输出格式
注意：-v 只能指定一种格式化，但可以指定多个格式化修饰符

例：adb logcat -v thread
例：adb logcat -v color
```

| 格式化     | 含义                                                         |
| :--------- | :----------------------------------------------------------- |
| brief      | 显示优先级、标记以及发出消息的进程的 PID                     |
| long       | 显示所有元数据字段，并使用空白行分隔消息                     |
| process    | 仅显示 PID                                                   |
| raw        | 显示不包含其他元数据字段的原始日志消息                       |
| tag        | 仅显示优先级和标记                                           |
| thread     | 旧版格式，显示优先级、PID 以及发出消息的线程的 TID           |
| threadtime | 默认：显示日期、调用时间、优先级、标记、PID 以及发出消息的线程的 TID |
| time       | 显示日期、调用时间、优先级、标记以及发出消息的进程的 PID     |

| 格式修饰符 | 含义                                                   |
| ---------- | ------------------------------------------------------ |
| color      | 使用不同的颜色来显示每个优先级                         |
| uid        | 如果访问控制允许，则显示 UID 或记录的进程的 Android ID |
| usec       | 显示精确到微秒的时间                                   |
| UTC        | 显示 UTC 时间                                          |
| year       | 将年份添加到显示的时间                                 |
| zone       | 将本地时区添加到显示的时间                             |

### 缓冲区相关

```
-b <Buffer>		加载一个可使用的日志缓冲区供查看

例：logcat -b radio
例：logcat -b main -b radio -b events
例：logcat -b main,radio,events
```

| 缓冲区  | 含义                                               |
| :------ | :------------------------------------------------- |
| radio   | 查看包含无线装置/电话相关消息的缓冲区              |
| events  | 查看已经过解译的二进制系统事件缓冲区消息           |
| main    | 查看主日志缓冲区（默认），不包含系统和崩溃日志消息 |
| system  | 查看系统日志缓冲区（默认）                         |
| crash   | 查看崩溃日志缓冲区（默认）                         |
| all     | 查看所有缓冲区                                     |
| default | 报告 `main`、`system` 和 `crash` 缓冲区            |





## monkey

```
例：monkey -p your.package.name -v 500 #启动您的应用并向其发送500个伪随机事件
```

### 常规

| 命令                        | 作用                                                         |
| --------------------------- | ------------------------------------------------------------ |
| -v                          | 0（默认值）只提供启动通知、测试完成和最终结果<br />级别 1 提供有关测试在运行时的更多详细信息，例如发送到您的 Activity 的各个事件<br />级别 2 提供更详细的设置信息，例如已选择或未选择用于测试的 Activity |
| `-s <seed>`                 | 伪随机数生成器的种子值。如果您使用相同的种子值重新运行 Monkey，<br />它将会生成相同的事件序列 |
| `--throttle <milliseconds>` | 在事件之间插入固定的延迟时间。您可以使用此选项减慢 Monkey 速度。<br />如果未指定，则不延迟，系统会尽快地生成事件 |
| `-p <allowed-package-name>` | 如果您通过这种方式指定一个或多个软件包，Monkey 将仅允许系统访问这些软件包内的 Activity。如果应用需要访问其他软件包中的 Activity（例如选择联系人），您还需要指定这些软件包。如果未指定任何软件包，Monkey 将允许系统启动所有软件包中的 Activity。要指定多个软件包，请多次使用 -p 选项，每个软件包对应一个 -p 选项 |

### 触摸事件

| 命令                        | 作用                                                         |
| --------------------------- | ------------------------------------------------------------ |
| `--pct-touch <percent>`     | 调整轻触事件所占百分比                                       |
| `--pct-motion <percent>`    | 调整动作事件所占百分比（动作事件包括屏幕上某个位置<br />的按下事件，一系列伪随机动作和一个释放事件） |
| `--pct-trackball <percent>` | 调整轨迹球事件所占百分比（轨迹球事件包括<br />一个或多个随机动作，有时后跟点击） |
| `--pct-nav <percent>`       | 调整“基本”导航事件所占百分比（导航事件包括<br />向上/向下/向左/向右，作为方向输入设备的输入） |
| `--pct-majornav <percent>`  | 调整“主要”导航事件所占百分比（这些导航事件通常会导致<br />界面中的操作，例如 5 方向键的中间按钮、返回键或菜单键） |
| `--pct-syskeys <percent>`   | 调整“系统”按键事件所占百分比（这些按键通常预留供系统使用，<br />例如“主屏幕”、“返回”、“发起通话”、“结束通话”或“音量控件”） |
| `--pct-appswitch <percent>` | 调整 Activity 启动次数所占百分比，Monkey 会以随机间隔发起<br />startActivity() 调用，以最大限度地覆盖软件包中的所有 Activity |
| `--pct-anyevent <percent>`  | 调整其他类型事件所占百分比。这包括所有其他类型的事件，<br />例如按键、设备上的其他不太常用的按钮等等 |

### 调试

| 命令                           | 作用                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `--ignore-crashes`             | 通常，当应用崩溃或遇到任何类型的未处理异常时，Monkey 将会停止。<br />如果指定此选项，Monkey 会继续向系统发送事件，直到计数完成为止。 |
| `--ignore-timeouts`            | 通常，当应用遇到任何类型的超时错误（例如“应用无响应”对话框）时，Monkey 将会停止。<br />如果指定此选项，Monkey 会继续向系统发送事件，直到计数完成为止。 |
| `--ignore-security-exceptions` | 通常，当应用遇到任何类型的权限错误（例如，如果它尝试启动需要<br />特定权限的 Activity）时，Monkey 将会停止。如果指定此选项，<br />Monkey 会继续向系统发送事件，直到计数完成为止。 |
| `--kill-process-after-error`   | 通常，当 Monkey 因出错而停止运行时，出现故障的应用将保持运行状态。<br />设置此选项后，它将会指示系统停止发生错误的进程。注意，在正常（成功）<br />完成情况下，已启动的进程不会停止，并且设备仅会处于最终事件之后的最后状态 |





## dumpsys

```
# 查看顶部activity
dumpsys activity top | grep ACTIVITY

# 查看正在运行的Services
dumpsys activity services [<packagename>]
```





## 截图、录屏

**截图**

```
screencap <filename>

例：adb shell screencap /sdcard/screen.png
   adb pull /sdcard/screen.png
```
**录屏**

默认情况下，该实用程序以本机显示分辨率和屏幕方向进行录制，时长不超过三分钟

局限性：

-   音频不与视频文件一起录制
-   无法在搭载 Wear OS 的设备上录制视频
-   某些设备可能无法以它们的本机显示分辨率进行录制。如果在录制屏幕时出现问题，请尝试使用较低的屏幕分辨率
-   不支持在录制时旋转屏幕。如果在录制期间屏幕发生了旋转，则部分屏幕内容在录制时将被切断

```
screenrecord [options] filename
参数：
--size widthxheight	默认值为设备的本机显示屏分辨率；如果不支持，则为 1280x720
--bit-rate rate		设置视频的视频比特率（以 MB/秒为单位）。默认值为4Mbps
--time-limit time	设置最大录制时长（以秒为单位）。默认值和最大值均为180（3分钟）
--rotate			将输出旋转 90 度。此功能处于实验阶段
--verbose			在命令行屏幕显示日志信息。如果您不设置此选项，则该实用程序在运行时不会显示任何信息

例1：adb shell screenrecord /sdcard/demo.mp4
    adb pull /sdcard/demo.mp4
例2：screenrecord --bit-rate 6000000 /sdcard/demo.mp4
```
