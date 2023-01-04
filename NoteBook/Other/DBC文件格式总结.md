# DBC文件格式总结

> DBC是vector公司定义的can网络通信文件格式，文件描述了单个网络节点的通信信息，这些信息可以监测分析网络以及模拟网络节点。相较于Excel、Word版的CAN协议描述文件，DBC文件格式比较固定、不会产生歧义和理解误差，便于交流。

## 样例

```
VERSION ""

NS_ : 
	NS_DESC_
	CM_
	BA_DEF_
	BA_
	VAL_
	CAT_DEF_
	CAT_
	FILTER
	BA_DEF_DEF_
	EV_DATA_
	ENVVAR_DATA_
	SGTYPE_
	SGTYPE_VAL_
	BA_DEF_SGTYPE_
	BA_SGTYPE_
	SIG_TYPE_REF_
	VAL_TABLE_
	SIG_GROUP_
	SIG_VALTYPE_
	SIGTYPE_VALTYPE_
	BO_TX_BU_
	BA_DEF_REL_
	BA_REL_
	BA_DEF_DEF_REL_
	BU_SG_REL_
	BU_EV_REL_
	BU_BO_REL_
	SG_MUL_VAL_

BS_:

BU_: BMS VCU MCU

VAL_TABLE_ Enable 1 "开启" 0 "关闭" ;
VAL_TABLE_ ErrLevel 0 "无故障" 1 "一级故障" 2 "二级故障" ;
VAL_TABLE_ State 1 "正转" 2 "反转" ;

BO_ 384 VCU_Msg: 8 VCU
 SG_ VCU_Sig6 : 56|8@1+ (1,0) [0|1] ""  BMS,MCU
 SG_ VCU_Sig5 : 48|8@1+ (1,0) [0|1] ""  BMS,MCU
 SG_ VCU_Sig4 : 40|8@1+ (1,1985) [1985|2235] "year"  BMS,MCU
 SG_ VCU_Sig3 : 32|8@1+ (0.25,0) [0|63.75] "day"  BMS,MCU
 SG_ VCU_Sig2 : 16|8@1+ (1,0) [0|23] "h"  BMS,MCU
 SG_ VCU_Sig1 : 8|8@1+ (1,0) [0|59] "min"  BMS,MCU
 SG_ VCU_Sig0 : 0|8@1+ (0.25,0) [0|63.75] "s"  BMS,MCU

BO_ 2566848513 BMS_Msg: 8 BMS
 SG_ BMS_Sig5 : 62|2@1+ (1,0) [0|3] "V"  VCU
 SG_ BMS_Sig4 : 56|6@1+ (1,0) [0|63] "V"  VCU
 SG_ BMS_Sig4 : 48|8@1+ (1,0) [0|255] "V"  VCU
 SG_ BMS_Sig3 : 45|3@1+ (1,0) [0|7] "V"  VCU
 SG_ BMS_Sig3 : 40|2@1+ (1,0) [0|3] ""  VCU
 SG_ BMS_Sig2 : 32|8@1+ (0.4,0) [0|100] ""  VCU
 SG_ BMS_Sig1 : 16|16@1+ (0.02,-600) [-600|600] "V"  VCU
 SG_ BMS_Sig0 : 0|16@1+ (0.1,0) [0|700] "V"  VCU

BO_ 2365521921 MCU_Msg: 8 MCU
 SG_ MCU_Sig5 : 57|2@0+ (1,0) [0|3] ""  VCU
 SG_ MCU_Sig4 : 55|8@0+ (1,-40) [-40|150] ""  VCU
 SG_ MCU_Sig0 : 3|4@0+ (1,0) [0|15] ""  VCU
 SG_ MCU_Sig1 : 15|16@0+ (1,0) [0|3000] ""  VCU
 SG_ MCU_Sig2 : 31|16@0+ (0.125,0) [0|8000] ""  VCU
 SG_ MCU_Sig3 : 47|8@0+ (1,-40) [-40|150] ""  VCU

CM_ BU_ BMS "BMS节点";
CM_ SG_ 384 VCU_Sig1 "VCU节点的报文发送的信号";
CM_ BO_ 2365521921 "MCU节点发送的报文";
BA_DEF_ SG_  "GenSigStartValue" FLOAT 0 0;
BA_DEF_ SG_  "SignalType" STRING ;
BA_DEF_ BO_  "GenMsgCycleTime" INT 0 10000;
BA_DEF_DEF_  "GenSigStartValue" 0;
BA_DEF_DEF_  "SignalType" "";
BA_DEF_DEF_  "GenMsgCycleTime" 0;
BA_ "GenMsgCycleTime" BO_ 384 50;
BA_ "GenMsgCycleTime" BO_ 2566848513 100;
BA_ "GenMsgCycleTime" BO_ 2365521921 10;
BA_ "GenSigStartValue" SG_ 384 VCU_Sig1 40;
BA_ "GenSigStartValue" SG_ 2566848513 BMS_Sig1 30000;
BA_ "SignalType" SG_ 2365521921 MCU_Sig0 "ENM";
BA_ "GenSigStartValue" SG_ 2365521921 MCU_Sig1 100;
BA_ "SignalType" SG_ 2365521921 MCU_Sig2 "ENM";
BA_ "SignalType" SG_ 2365521921 MCU_Sig3 "ENM";
VAL_ 384 VCU_Sig0 1 "开启" 0 "关闭" ;
VAL_ 2566848513 BMS_Sig0 0 "无故障" 1 "一级故障" 2 "二级故障" ;
VAL_ 2365521921 MCU_Sig0 1 "正转" 2 "反转" ;
```



## 关键字

| 关键字         | 名称                  | 解释                                                         |
| -------------- | --------------------- | ------------------------------------------------------------ |
| VERSION        | version               | **版本**：一般为空或者是Vector CANdb++ Editor使用的字符串    |
| NS\_           | new_symbols           | **新符号**：DBC文件中可能出现的符号                          |
| BS\_           | bit_timing            | （过时但需要）                                               |
| BU\_           | nodes                 | **节点**：CAN网络中所有节点                                  |
| VAL\_TABLE\_   | value_tables          | **值表**：值表部分定义全局值表，描述了信号的原始值取值列表   |
| BO\_   SG\_    | messages              | **报文**：定义帧的名称、属性以及帧上传输的信号等信息。为"BO\_"符号后面的内容<br />其中的每个信号的定义为"SG\_"符号后面的内容 |
| CM\_           | comments              | **注释**：注释是对DBC对象的说明                              |
| BA\_           | attribute_values      | **属性值**：为具有用户自定义属性的对象指定该属性的值         |
| BA\_DEF\_      | attribute_definitions | **属性定义**：用户自定义的属性是扩展对象属性的一种方法。<br />这些附加属性必须有默认值。每个具有该属性的对象，<br />都必须指定该属性的值，如未指定则使用该属性的默认值 |
| BA\_DEF\_DEF\_ | attribute_defaults    | **属性默认值**：用户自定义属性时指定的默认值                 |
| VAL\_          | value_descriptions    | **值描述**：对信号原始值或环境变量值的描述                   |

**不常用关键字**

> 在一般的DBC文件中不会出现的结构为：signal_types、sigtype_attr_list、category_definitions、categories、filter、signal_type_refs、signal_extended_value_type_list共7个部分


| 关键字         | 名称                            | 解释                                                         |
| -------------- | ------------------------------- | ------------------------------------------------------------ |
| BO_TX_BU\_     | message_transmitters            | **报文多节点发送**：定义报文的多个发送节点，即多个节点都会发送该帧报文 |
| EV\_           | environment_variables           | **环境变量**：定义用于系统仿真和半物理总线仿真工具的环境变量 |
| ENVVAR_DATA\_  | environment_variables_data      | **环境变量数据**：定义环境变量的数据类型，这种数据类型的环境变量可以存储给定长度的任意二进制数据，长度以字节为单位给出。为"ENVVAR\_DATA\_"符号后面的内容 |
| SIG_GROUP\_    | signal_groups                   | **信号组**：信号组用于定义消息中的信号为一组，对同组的信号执行同一操作。例如，定义一个组的信号同时进行更新。为"SIG\_GROUP\_"符号后面的内容 |
| SGTYPE\_       | signal_types                    | **信号类型**：信号类型用于定义多个信号的公共属性。通常不在DBC文件中使用。为"SGTYPE\_"符号后面的内容 |
| /              | sigtype_attr_list               | **信号类型属性列表**                                         |
| /              | category_definitions            | **类别定义**                                                 |
| /              | categories                      | **类别**                                                     |
| /              | filter                          | **过滤**                                                     |
| /              | signal_type_refs                | **信号类型引用**                                             |
| SIG\_VALTYPE\_ | signal_extended_value_type_list | **信号扩展值类型列表**：值类型为"float"或"double"的信号需要在此列表中添加额外说明项。通常不在DBC文件中使用。为"SIG\_VALTYPE\_"符号后面的内容 |



## 详细语法

### 节点

**语法**：`BU_: 节点名称1 节点名称2 节点名称3 …`

```
BU_: BMS VCU MCU
```

BU_开头的行定义节点，每个节点之间用空格分隔。示例中定义了BMS、VCU、MCU三个节点。

### 值表
**语法**：`VAL_TABLE_ 值表名称  值1 “值1描述”  值2 “值2描述”  值3 “值3描述” … ;`

```
VAL_TABLE_ Enable 1 "开启" 0 "关闭" ;
VAL_TABLE_ ErrLevel 0 "无故障" 1 "一级故障" 2 "二级故障" ;
VAL_TABLE_ State 1 "正转" 2 "反转" ;
```

VAL_TABLE_开头的行定义值表，值和值的描述成对出现。

### 报文
**语法**：`BO_ 报文ID 报文名称: 报文数据长度 发送节点`

```
BO_ 384 VCU_Msg: 8 VCU
BO_ 2566848513 BMS_Msg: 8 BMS
```

BO_开头的行定义报文。报文ID为十进制数，标准帧ID为数值本身，如384为0x180，扩展帧ID为数值本身减去0x80000000，如2566848513为0x18FF0001。

报文数据长度一般为8个字节。

如未指定报文的发送节点，则发送节点必须为Vector__XXX，不能留空。

### 信号
**语法**：`SG_ 信号名称: 起始位|位长@字节顺序 符号类型 (增益,偏置) [最小值|最大值] “单位” 接收节点`

```
SG_ VCU_Sig0 : 0|8@1+ (0.25,0) [0|63.75] "s"  BMS,MCU
SG_ MCU_Sig3 : 47|8@0+ (1,-40) [-40|150] ""  VCU
```

SG_开头的行定义信号。

- 字节顺序：0表示Motorola格式（高字节在前，正常的读写形式），1表示Intel格式（低字节在前，正常读写形式的前后颠倒版）

- 符号类型：+表示无符号，-表示有符号。

- 信号的增益和偏置用于信号原始值`raw_value`和物理值`physical_value`之间的转换计算，计算公式为：

     ```
     physical_value = raw_value * factor + offset
     raw_value = (physical_value – offset) / factor
     ```


     信号的最小值和最大值为物理值的下限值和上限值。接收信号的节点可以为一个或多个，有多个时中间用英文逗号分隔。如未指定信号的接收节点，则接收节点必须为Vector__XXX，不能留空。

### 注释
**语法**：`CM_ 对象类型 对象名称 “注释”;`

```
CM_ BU_ BMS "BMS节点";
CM_ SG_ 384 VCU_Sig1 "VCU节点的报文发送的信号";
CM_ BO_ 2365521921 "MCU节点发送的报文";
```

CM_开头的行定义对象的注释。对象类型可以为节点、报文、信号、环境变量。对于报文类型的对象，对象名称使用报文ID。对于信号类型的对象，信号名称之前需要添加报文ID以确定信号的具体位置。

### 属性定义
**语法**：`BA_DEF_ 对象类型 “属性名称” 属性值类型 最小值 最大值;`

```
BA_DEF_ SG_  "GenSigStartValue" FLOAT 0 0;
BA_DEF_ SG_  "SignalType" STRING ;
BA_DEF_ BO_  "GenMsgCycleTime" INT 0 10000;
```


BA_DEF_开头的行定义属性。对象类型可以为节点、报文、信号、环境变量，表示该属性用于该类型的对象。

### 属性默认值
**语法**：`BA_DEF_DEF_ “属性名称” 属性默认值;`

```
BA_DEF_DEF_  "GenSigStartValue" 0;
BA_DEF_DEF_  "SignalType" "";
BA_DEF_DEF_  "GenMsgCycleTime" 0;
```


BA_DEF_DEF_开头的行定义属性的默认值，定义属性必须指定属性默认值。

### 属性值
**语法**：`BA_ “属性名称” 对象类型 对象名称 属性值;`

```
//表示报文0x0CFF0001的报文周期为10ms
BA_ "GenMsgCycleTime" BO_ 2365521921 10;

//表示报文0x0CFF0001的信号MCU_Sig0的类型为枚举类型
BA_ "SignalType" SG_ 2365521921 MCU_Sig0 "ENM";

//表示报文0x18FF0001的信号BMS_Sig1的初始原始值为30000
//初始物理值通过增益和偏置计算为0（0 = 30000 * 0.02 - 600）
BA_ "GenSigStartValue" SG_ 2566848513 BMS_Sig1 30000;
```

BA_开头的行定义对象的属性值，如果对象的属性值刚好等于属性的默认值，则不会在DBC文件中出现该对象的属性值的定义。对象类型可以为节点、报文、信号、环境变量。对于报文类型的对象，对象名称为报文ID。

DBC文件中记录的信号初始值都为信号原始值，而Vector CANdb++ Editor中显示的为信号的物理初始值。

### 值描述
**语法**：`VAL_ 信号或环境变量名称 值1 “值1描述” 值2 “值2描述” 值3 “值3描述” … ;`

```
VAL_ 384 VCU_Sig0 1 "开启" 0 "关闭" ;
VAL_ 2566848513 BMS_Sig0 0 "无故障" 1 "一级故障" 2 "二级故障" ;
VAL_ 2365521921 MCU_Sig0 1 "正转" 2 "反转" ;
```

VAL_开头的行定义信号或环境变量的值描述，值和描述成对出现。对于信号类型的对象，信号名称之前需要添加报文ID以确定信号的具体位置。



## 符号

| 符号   | 作用                                                   |
| ------ | ------------------------------------------------------ |
| =      | **等于号**：使左侧的名称=右侧的语法定义                |
| ；     | **分号**：终止一个定义                                 |
| \|     | **竖线**：表示其左右两边任选一项，相当于或的意思       |
| []     | **方括号**：方括号内的定义为可选项（零次或一次出现）   |
| {}     | **大括号**：大括号内的定义为可重复项（零次或多次出现） |
| ()     | **圆括号**：圆括号定义分组元素                         |
| '  '   | **连字符**                                             |
| (*  *) | **注释符**                                             |
