---
layout: post
title:  "ABB-RAPID-Instruction-A"
date:   2016-09-20 22:24:05 +0800
categories: Robot ABB
---
### A.1 AccSet - 减少加速度

------

**用法**
`AccSet` 在搬用易碎品时使用。它允许较慢的加速和减速，这会产生更平滑的机器人运动。   该指令只能用于总任务 `T_ROB1` 或多移动系统的一个移动任务上。

------

**基本示例**

示例1： 

>  `Accset 50, 100;`

加速度限制为正常值的50%

示例2：

> `Accset 100,50;`

加速斜率限定在正常值的50%

示例3：

> `AccSet 100, 100 \FinePonitRamp:=50;`

在朝精细点（FinePoint）减速时，减速斜率限制为正常值的50%

------

**参数**

> `AccSet Acc Ramp [\FinePointRamp]`

`Acc`
	数据类型：num
	加速和减速的正常值。100%对应最大加速度。最大值:100%. 输入值<20%给与最大值的20%（PS：也就是说，最小是20%，低于20%的程序会自动按照20%执行）
`Ramp`
	数据类型: num
	加速和减速增长率的正常值的百分比。机械臂的抽动可以通过减少该值被限制。100%对应最大比率。最大值：100%。输入值<10%默认按照10%。

![](http://odbmi3g5l.bkt.clouddn.com/xx0500002146.PNG)

`[\FinePointRamp]`
	数据类型：num
	减速下降正常值的百分比率。该参数仅在机器人减速前往精细点是影响斜率。前往精细点的减速斜率值是 Ramp * FinePointRamp。 该参数必须于零而且在0到100之间。   
	如果没有明确设置该参数，会自动设置为缺省值，100%1.3 ActUnit - 激活一个机械单元

--------



### A.2 ActEventBuffer - 激活事件缓存区

---

**描述**

`ActEventBuffer`用来激活当前运动任务的事件缓存区的使用。

指令`ActEventBuffer`和`DeactEventBuffer`应在程序结合使用 finepoints 和一套连续应用信号由于慢的过程设备需要预先设定。   

该指令只能用于总任务 `T_ROB1` 或多移动系统的一个移动任务上。

---

**基本示例**

示例1

> `...`
>
> `DeactEventBuffer;`
>
> `! Use an application that users finepoints, such as Sporweldding`
>
> `...`
>
> `! Activate the event buffer again`
>
> `ActEventBuffer;`
>
> `! Now it is possible to use an application that needs`
>
> `! to set signals in advance, such as Dispense`
>
> `...` 

The `DeactEventBuffer`让以配置的事件缓存区失效。从 finepoint 启动的机器人更快。当通过`ActEventBuffer`激活事件缓存区。它可为带缓慢过程设备的应用设置信号提前。

----

**程序执行**

事件缓存区适用于任何类型的下一个被执行的机器人动作。直到指令`DeactEventBuffer`被执行。  

该指令保持等待，直到机器人和外部轴在事件缓存区激活之前到达停止点（当前移动指令为`ToPoint`）。因此建议程序在移动指令之前设置`ActEventBuffer`和相应的精细点。

默认值使用事件缓存区（）是自动设定的

- 在 `P-Shart`
- 当一个新程序在加载
- 从程序开头开始执行。

---

**限制**

`ActEventBuffer` 当程序连接任何特殊系统事件则无法执行：

- PowerOn 通电
- Stop 停止
- QStop 
- Restart
- Step

---



### A.3 ActUnit - 激活一个机械单元

---

**用法**

`ActUnit` 用来激活一个机械装置。   
用来确定激活哪一个单元。例如，调用普通驱动单元。    
该指令只能用于主任务T_ROB1, 或者是MultiMove系统中的移动任务。    

-----

**基本示例**

示例1：    
> `ActUnit orbit_a;`  

激活名为 “ orbit_a ” 的运动单元。

----

**参数**

> `ActUint MechUnit`

`Mechunit`机械单元 
	数据类型： meunit   
	按定义的名称激活相应的机械装置。
----

**程序执行**

当机器人和外部轴的实际路径准备好时，整个路径被清理同时指定的机械装置被启动。这意味着它（路径）被机器人控制和监视。    
如果多个机械装置共享一个普通驱动单元。其中一个机械装置被激活。同时将该单元连接到普通驱动单元（如激活焊枪）

----

**限制**

如果这个指令以一个移动指令为先导，那个移动指令必须给定一个中止点（ zonedata `fine`），不是飞行点，否则可能断电后重启。    
`ActUnit` 在某些特殊系统事件后无法执行。这些特殊系统事件包括： PowerOn，Qstop, Restart, Reset or Step .    

似乎可以在 `StorePath` 级别上使用 `ActUnit`和`DeactUnit`, 但同样的机械装置在`StorePath`下运行`RestoPath`时必须激活。这样`StorePath`的路径被清除，路径记录器和基础水平的路径将完好无损。    

**语法** 

	ActUnit    
	[ MechUnit ':='] < variable(VAR) of mecunit > ';' 

-----



###A.4 Add - 增加一个数值值

----

**用法**     

`Add` 用来从一个数值变量或持久量增加或减去一个值。

----

**基本示例**

示例1

> `Add reg1, 3;`   

将 3 加到 `reg1`上，相当于 `reg1 := reg1 + 3`



示例2

> `Add reg1, -reg2;`   

从`reg1`中减去 `reg2`的值，相当于 `reg1 := reg1- reg2`



示例3

> `VAR dnum mydnum := 5;`    
> `Add mydnum, 5000000000;`    

定义一个数值变量 `mydnum`为5 。将5000000000 加到 `mydnum`， 相当于`mydnum := mydnum + 5000000000`



示例4

`VAR dnum mydnum := 5000;`   
`VAR num mynum := 6000；`   
`Add mynum, DnumToNum (mydnum \ Integer);`   

将5000 加到 `mynum` 相当于 `mynum:=mynum+5000`。 需要用`DnumToNum` 得到一个类型为`num`值 你可以将`num`和`mynum`加在一起。

----

**参数**

> `Add Name | Dname AddValue | AddValue`

`Name`

    数据类型：num
    变量名或要被改变的持久量。

`Dname`

	数据类型：dnum
	变量名或要被改变的持久量。

`AddValue`

	数据类型：num
	被加的值

`AddDvalue`

	数据类型：dnum
	被加的值

----

**限制**

如果被加的值的数据类型是`dnum`，变量或持久量变为 `num`，运行将发生一个错误。（也就是说，dnum 和 dnum 加，`num` 和 `num` 加）解决方法见例4。

----

**语法**

	Add
	  [Name ':='] <var pers (INPUT) of num >
	  | [Dname ':='] <var or pers (INPUT) of dnum > ','
	  [AddValue ':=']<expression (IN) of num>
	  | [AddDvalue ':='] <expression (IN) of dnum> ';' 

---



###A.5 AliasIO - 使用别名定义 I/O 信号。

----

**用法**

`AliasIO` 用别名定义一个任意类型的信号，或用来在用命名将信号嵌入任务模块。   
在不同的机器人安装中，带别名的信号可预定义常规程序无需对程序进行任何修改。   
任何实际信号使用之前必须运行`AliasIO`指令。

----

**基本示例**

示例1

> `VAR signaldo alias_do;`   
> `PROC prog_start()`   
> `	AliasIO config_do, alias_do;`   
> `ENDPROC`>    

程序`prog_start` 链接到系统参数的 `START`事件。程序定义的数字输出信号`alias_do` 链接到程序开头设置的数字输出信号 `config_do`

----

**参数**

> `AliasIO FromSignal ToSignal`

`FromSignal`

数据类型：signalxx 或  string   
 **加载模块**  
信号标示符从信号描述符复制，并根据配置命名（数据类型 `signalxx`)。该信号必须在I/O 配置中定义。   
**安装的模块或加载的系统模块**   
一个相关（CONST、VAR、PERS或者它们的参数）包含信号（数据类型string字符串）的名称是从信号描述符在系统中搜索后被复制。信号必须在IO配置中定义。

`ToSignal`

数据类型:  signaixx   
信号标示符根据复制的信号描述符命名（数据类型: signaixx）。信号必须在 RAPID 程序中定义。   
FromSignal 和 ToSignal 必须使用相同的数据类型，同时数据类型必须是下列类型之一：		
- signalxx
- signalai
- signalao
- signaldi
- signaldo
- signalgi
- signalgo

----

**程序执行**
信号描述符的值 从 `FromSignal` 给出并复制到 `ToSignal` 

----

**错误处理**

下列可修复错误生成并可处理一个错误处理程序。系统变量 `ERRNO`  将被设为：

|          代码          | 含义                                       |
| :------------------: | :--------------------------------------- |
|  `ERR_ALIASIO_DEF`   | FromSignal没有在 IO 配置中定义。 ToSignal未在 RAPID 程序中声明，或 ToSignal未在 IO配置中定义。 |
|  `ERR_ALIASIO_TYPE`  | `FromSignal`和`ToSignal`两者的数据类型不一致。       |
| `ERR_NO_ALIASIO_DEF` | 信号在程序中声明是一个变量。并没有在配置中连接实际的 IO 信号。        |

----

**更多示例**

示例1

> `VAR sigaldi alias_di;`
>
> `PROC prog_start()`
>
> ​	`CONST string config_string := "config_di";`
>
> ​	`AliasIO config_string, alias_di;`
>
> `ENDPROC`



程序`prog_start`链接到系统参数中 START事件。程序定义的 数字输入信号`alias_di` 链接到在程序开头配置的数字输入信号`config_di`（通常为`config_string`)。

----

**限制**

当程序开始时，别名信号不能使用，除非指令`AliasIO`被执行。

`Alias`必须放置在：

- 事件被执行或者程序开始

- 任何程序开始后（信号使用前）之前的程序部分。

为防止错误，不推荐将`AliasIO` 动态链接到不同的物理信号。 

---

**语法**

	AliasIO
	   [FromSignal ':='] < reference(REF) of anytype> ','
	   [ToSignal ':='] <variable (VAR) of anytype> ';'

