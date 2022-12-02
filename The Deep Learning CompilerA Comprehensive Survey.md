# The Deep Learning Compiler:A Comprehensive Survey

深度学习编译器：综合调查

## 0、摘要：

![image-20221130123126381](C:\Users\19392\AppData\Roaming\Typora\typora-user-images\image-20221130123126381.png)

```
①在不同DL硬件上部署各种深度学习模型的困难，促进了社会中DL编译器的发展和研究。一些DL编译器被工业界和学术界提出，如tensorflow XLA 和 TVM
②类似的，DL编译器将不同DL框架中描述的DL模型作为输入，并且为不同DL硬件产生优化代码作为输出。
③然后，现有的调查没有DL编译器的独特设计架构进行全面分析。
④在本文中，我们对现存在的DL编译器进行了详细剖析。重点是面向多层IR 的DL，以及前端和后端的优化。
⑤我们呈现了详细的分析在多层IR设计和举例了常用的优化的技术。
⑥最后，一些见解被突出为DL编译器的潜在研究方向，这是第一篇关于DL编译器的设计体系结构的调查文章，我们希望它能为DL编译器的未来研究铺平道路
```

## 1、介绍

![image-20221130141029808](D:\19392\Documents\The Deep Learning CompilerA Comprehensive Survey.assets\image-20221130141029808.png)

>(1)①DL的发展对各个科学领域产生了深远的影响，它不仅仅在NLP和CV等人工智能领域显示出显著的价值，而且在电子商务、智慧城市以及药品发现等广泛应用产生了巨大成功。
>
>   ②随着多功能深度学习模型的出现，如CNN，RNN，LSTM，GAN。为了实现他们的广泛采用，简化对不同DL模型的编程至关重要。
>
>（2）
>
>①随着工业和学术界的努力，一些流行的DL框架被提出，如Tensorflow,PyTorch,MXNet和CNTK，来简化DL模型的实现。尽管取决于其设计的权衡，各种框架各有优缺点，但是，当跨现存在的DL模型中支持新模型时，相互操作性对于减少工程的冗余很重要
>
>②为了提供互操作性，提出了ONNX，它定义了一种表示DL模型的统一格式，以促进不同DL框架之间的模型转换。
>
>（3）
>
>①与此同时，独特的计算特征，如矩阵成分激发了芯片的架构的性能，来设计定制的DL加速器去提高效率。
>
>②互联网巨头 (e.g., Google TPU [15], Hisilicon NPU [16], Apple Bonic [17]),处理器供应商 (e.g., NVIDIA Turing [18], Intel NNP [19])，业务提供商 (e.g.,Amazon Inferentia [20], Alibaba Hanguang [21]),甚至是初创公司如 (e.g., Cambricon [22], Graphcore [23])     正在投入大量的劳动力和资本来开发DL芯片，以提高DL模型的性能。
>
>③通常，DL硬件可分为以下类别：1)通用硬件与软件硬件共同设计,2)为DL模型定制的专用硬件。3）受生物脑科学启发的神经形态学硬件。
>
>例如，通用的硬件(e.g., CPU, GPU)已经增加了特殊的硬件组件，如AVX512向量单元和张量core来加速DL模型。而对于专用硬件，（如谷歌TPU，特定于应用程序的集成电路）如（矩阵乘法引擎和高宽带内存）已经被设计来提高性能和资源最大利用。在可预见的未来，DL硬件的设计将变得更加多样化。

<font color=blue>现存在问题</font>

<img src="C:\Users\19392\Desktop\myfile\AI_compiler\The Deep Learning CompilerA Comprehensive Survey.assets\image-20221130162400718.png" alt="image-20221130162332259" style="zoom:80%;" /><img src="C:\Users\19392\Desktop\myfile\AI_compiler\The Deep Learning CompilerA Comprehensive Survey.assets\image-20221130164520945.png" alt="image-20221130164520945" style="zoom:80%;" />

>(4)①为了适应硬件的多样性，有效的将计算映射到DL硬件上是很重要的 。
>
>在通用硬件，高度优化的线性代数库如，基本线性代数子程序（BLAS）库 (e.g., MKL and cuBLAS)作为DL模型高效计算的基础。以卷积运算为例，DL框架将卷积转化为矩阵乘法然后调用BLAS库中的GEMM函数
>
>②此外，硬件商发布了特殊的优化库专门针对DL计算。 如 (e.g., MKL-DNN and cuDNN),包括正向卷积和反向卷积、池化、规范化和激活。
>
>刚高级的工具也被发展去进一步加速DL的操作。例如TensorRT 支持图优化如(e.g., layer fusion 层融合) 
>
>以及使用大量高度优化的GPU内核来进行低位量化（low-bit quantization）
>
>③在专用DL硬件上，也提供了相似的库，然而依赖库的缺点是他们通常落后于DL模型的快速开发，因此无法有效地利用DL芯片。

<font color=blue>解决问题</font>

>（5）①为了解决DL库和工具的缺点，以及减轻在每个DL硬件上手动优化DL模型的负担，DL community 已经向特定领域的编译器求助。 很快的，几个流行的DL编译器已经被提出如TVM，Tensor Comprehensions, Glow [27], nGraph [28] and XLA 
>
>②DL编译器将DL框架中描述的模型定义 作为输入，并且生成在各个DL硬件上高效执行的代码作为输出。
>
>从‘模型定义’到‘特定代码执行’之间的转换是高度优化的，针对模型规范和硬件架构。
>
>