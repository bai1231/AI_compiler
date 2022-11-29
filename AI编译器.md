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

