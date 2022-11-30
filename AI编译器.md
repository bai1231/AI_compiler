# AI编译器

### 1、编译器的基本概念

①编译器和解析器

> 编译器compiler:
>
> 
>
> 解析器interpreter:
>
> ![image-20221128105138258](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128105138258.png)



②JIT和AOT区别

>程序主要有俩种运行方式：
>
>静态编译：程序执行前全部被编译为**机器码**，称为AOT （ahead of time），即提前编译
>
>动态解释：程序边编译边运行，通常将这种类型称为JIt(just in time)，即”即时编译“

![image-20221128112834244](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128112834244.png)

 ③什么时候用JIT什么时候用AOT

离线运算一般用到AOT

>![image-20221128113343963](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128113343963.png)
>
>写出神经网络源码--》AI框架会对其进行编译--》转为机器码--》机器码丢给CPU，GPU，NPU等执行，直到收敛为止，然后将神经网络模型丢出。
>
>![image-20221128113721934](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128113721934.png)
>
>将模型保存到磁盘中，当需要运算时，把模型交给GPU等进行执行运算。

④编译器的pass和IR

>pass对源程序进行扫描，然后将其变成低级语言![image-20221128114152198](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128114152198.png)
>
>
>
>IR：中间表达
>
>将源代码通过中间表达变成目标有代码
>
>![image-20221128114435588](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128114435588.png)



### 2、开源编译器简介

![image-20221128194013787](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128194013787.png)

> 编译器：将高级语言转换为二进制代码的机器语言
>
> ![image-20221128115008500](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128115008500.png)
>
> 编译器执行的过程
>
> ![image-20221128172731076](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128172731076.png)
>
> ![image-20221128172833988](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128172833988.png)
>
> ![image-20221128172857054](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128172857054.png)

GCC编译器主要基于GNU或Linux

LLVM基于苹果操作系统

微软的是基于Visual　studio的 





### 3、GCC编译过程

![image-20221128175138895](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128175138895.png)

1、GCC编译流程

>>
>
>![image-20221128175332063](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128175332063.png)
>
>①预处理：包括宏定义，文件包含，条件编译三部分。预处理过程读入源代码，检查包含预处理指令的语句和宏定义，并对其进行响应和替换。预处理过程还会删除程序中的注释和多余空白字符，最后 生成.i文件。
>
>![image-20221128181102482](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128181102482.png)
>
>进行预处理之后
>
>![image-20221128181207614](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128181207614.png)
>
>②编译
>
>通过编译器将.i文件编译后形成汇编指令。
>
>![image-20221128180517019](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128180517019.png)
>
>![image-20221128181503776](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128181503776.png)
>
>![image-20221128181611043](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128181611043.png)
>
>③汇编：
>
>汇编器：汇编器会将编译器生成的.s汇编程序汇编为机器语言或指令，也就是机器可以执行的二进制程序。会生成.o文件  二进制文件
>
>![image-20221128182450143](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128182450143.png)
>
>④链接：
>
>连接器会来链接程序运行所需要的目标文件，以及依赖的库文件，最后生成可执行的文件，以二进制的形式存在磁盘中。因为可执行的程序可能用到的不仅仅一个目标文件。
>
>![image-20221128192319481](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128192319481.png)
>
>![image-20221128193521444](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128193521444.png)

![image-20221128193852153](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128193852153.png)









### 04LLVM架构和原理

![image-20221128194233267](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128194233267.png)

![image-20221128194319995](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128194319995.png)

![image-20221128194441148](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128194441148.png)

![image-20221128194601377](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128194601377.png)

什么是LLVM

> 可以说LLVM是一个编译器
>
> ![image-20221128194621505](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221128194621505.png)
>
> 是一个编译器的前端
>
> 是一个编译器的工具集合
>
> 编译器的工具链
>
> LLVM发展成为了一个巨大的编译器相关的工具集合。



②LLVM和GCC区别

>![image-20221129193744209](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129193744209.png)
>
>

![image-20221129194144787](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129194144787.png)

![image-20221129194100087](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129194100087.png)

Clang作为了LLVM的前端，LLVM作为了优化器和后端。

![image-20221129194244380](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129194244380.png)

前端进行词法分析，语法分析，语义分析，ir生成。

![image-20221129194350109](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129194350109.png)







执行过程：

>![image-20221129194509902](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129194509902.png)
>
>①对c语言文件进行预处理，生成。i文件![image-20221129194649174](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129194649174.png)
>
>②把.i文件 导出成.bc形式的文件
>
>![image-20221129195003031](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195003031.png)
>
>![image-20221129195043409](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195043409.png)
>
>od -b 以八进制形式输出
>
>②或者：
>
>导出为我们可以识别的.ll文件。里面使用LLVM写的
>
>![image-20221129195557968](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195557968.png)
>
>![image-20221129195615619](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195615619.png)这里面LLVM的表达
>
>③将ll文件或bc文件，导出为.s
>
>![image-20221129195716859](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195716859.png)
>
>即将其转为了汇编指令
>
>⑤将其变成可执行的二进制文件
>
>![image-20221129195837338](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195837338.png)

输出整个编译过程

0是输入

1是预处理

2是编译

3是后端的优化

4是产生汇编指令

5是库的链接

6生成x86可执行的程序。![image-20221129195947858](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129195947858.png)





### 05LLVM IR 中间表达

![image-20221129200701465](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129200701465.png)

IR中间表达，在不同的地方，是不一样的。就是在不同编译阶段，它采用的不同的数据结构。

![image-20221129200824893](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129200824893.png)

![image-20221129201031379](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201031379.png)

![image-20221129201109078](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201109078.png)

![image-20221129201053375](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201053375.png)

![image-20221129201249057](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201249057.png)

![image-20221129201353829](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201353829.png)

![image-20221129201557274](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201557274.png)

![image-20221129201741875](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201741875.png)

![image-20221129201856682](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129201856682.png)

![image-20221129202006145](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129202006145.png)

![image-20221129202105475](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129202105475.png)

![image-20221129202205101](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129202205101.png)

![image-20221129202238581](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129202238581.png)

![image-20221129202327080](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129202327080.png)



### 06、LLVM架构和原理（前端优化过程和中间优化过程）

LLVM前端：把源代码c、C++、python变成中间表示，即LLVM的IR。故前端的最后一步，就是IR的生成。

代码生成之后，就是针对各个硬件

> ![image-20221129203117056](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129203117056.png)

①前端第一步：词法分析Lexical analysis

>将高级语言，去掉注释、空白、制表符等，将语言结构分解为一组单词和标记。
>
>![image-20221129203441641](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129203441641.png)

②语法分析：

>![image-20221129203822453](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129203822453.png)
>
>

③语义分析：

> 借助语言表，来检查代码是否违背语言系统，是否有语法错误。
>
> ![image-20221129204000238](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129204000238.png)

优化层：

>对程序的一次遍历，叫做一次pass
>
>![image-20221129204153987](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129204153987.png)
>
>优化层的pass
>
>分析的pass主要是进行：发掘性能和优化的机会。只做分析
>
>转换的pass则主要进行：生成必要的数据结构也称IR
>
>![image-20221129204322170](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129204322170.png)
>
>![image-20221129204544476](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129204544476.png)
>
>![image-20221129204632556](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129204632556.png)
>
>https://llvm.org/docs/Passes.html
>
>![image-20221129204939981](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129204939981.png)
>
>如这里的b,c完全没有用到，在编译的过程中就会把他完全删掉，这种就叫死代码消除。转换pass过程进行的。
>
>![image-20221129205015050](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129205015050.png)

理解pass之间的关系：

>在转换pass和分析pass之间，主要有俩种依赖。
>
>![image-20221129205456677](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129205456677.png)
>
>转换的pass在工作之前，需要用到分析，将分析的结果告诉pass,然后在进行转换。
>
>如合并这一过程，分析的pass,分析有多少重复，合并的pass将其进行合并转换。
>
>pass管理器会自动在转换pass前安排分析pass对其进行分析。
>
>![image-20221129205611613](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129205611613.png)
>
>在进行pass时是分层级，分力度的。对模型处理，有module pass ,对函数处理有函数级别的pass,对某几个语句处理，有BasicBlockpass.来方便我们对于不同域的代码进行处理。
>
>





### 07、LLVM的后端

LLVM后端由一套分析和转换Pass组成，它们的任务是代码生成，即将LLVM IR转换为目标代码（或者汇编）

代码生成

> ![image-20221129211027729](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129211027729.png)
>
> ![image-20221129211110265](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129211110265.png)

第一个Pass:

①指令选择

将LLVM的IR变成DAG节点

>把IR变成DAG，即有向无环图
>
>图就是目标机器代码节点，节点代表目标代码的指令了。而不是原来的LLVM指令（三地址指令）
>
>![image-20221129211305902](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129211305902.png)
>
>②指令调度：
>
>对DAG节点进行排序，尽可能多的发现可以并行的指令，同时把指令变成三地址表示
>
>进行寄存器的预分配。
>
>>![image-20221129211821703](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129211821703.png)

③寄存器的分配

在LLVM IR里面，寄存器假设是无限的。

这个特性到这一步就终止了，把LLVM IR里面的无限虚拟寄存器转换为实际有目标，有地址有定位的寄存器。

如果寄存器不够的时候，就会把他先塞到内存里面。

![image-20221129212528339](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129212528339.png)

④第二次指令的调度

此时可获得寄存器的实际信息。分析到刚才有些寄存器是存不下数据的，将其存到了内存中，可通过指令的调度，改变指令的执行次序。![image-20221129213055913](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129213055913.png)

⑤代码的输出：

将指令从指令调度产生的MAchineInstr转换为MCInst实例子

![image-20221129213417775](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129213417775.png)

![image-20221129213810664](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129213810664.png)

![image-20221129214047835](C:\Users\19392\Desktop\myfile\AI_compiler\AI编译器.assets\image-20221129214047835.png)
