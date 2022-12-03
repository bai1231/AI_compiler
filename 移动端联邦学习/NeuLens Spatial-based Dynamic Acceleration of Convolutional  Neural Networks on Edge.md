# **NeuLens: Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge**

基于空间的边缘卷积神经网络的动态加速

### 0、摘要：

><font color='blue'>问题:</font>
>
>最先进的CNN加速的难点： 在一般计算平台上限的实际延迟加速  和 延迟加速有着严重的精度损失
>
><font color='blue'>本文：</font>
>
>我们提出了一个基于空间的动态CNN加速框架Neulens 适用于移动平台和边缘平台
>
>①设计了新的动态推理机制，组装具有区域感知的卷积 超级网
>
>基于空间冗余和信道分割  它剥离了CNN模型内的冗余操作。
>
>在ARAC超网中，CNN推理流被划分为多个独立的微流 ，并且每个的计算成本都可以根据 其平铺输入内容和应用程序需求 进行自动调整。
>
>这些微流可以作为单一模型加载到gpu等硬件中。因此它的操作减少，可以被理解为延迟加速，它是与硬件级的加速相兼容。
>
>推理的精确值可以被很好的维持通过   识别图像上的关键区域   和 在原分辨率使用大量微流来处理他们
>
>NeuLens优于其他方法，在相同的精度下减少高达58%的延迟 以及 准确度提高了高达67.9% 在相同的延迟/内存约束下。
>
>**CCS CONCEPTS**
>
>**Computing methodologies** → *Neural networks*; • **Human**
>
>**centered computing** → *Ubiquitous and mobile computing*.
>
>

1、介绍

>与计算机视觉相关的任务通常需要大量的计算资源,许多研究集中于降低CNN推理的计算成本。一些工作提出了轻量级的网络架构，如MobileNets， CondenseNet , ShuffleNets [51, 93], and EfficientNet [71].其他的研究通过剪枝[42,44,49,50]或量化来压缩现有的网络
>
>最近的工作提出了各种方法，允许动态计算成本调整CNN推理。受人类视觉启发，只有有限的视觉场景可以被视觉系统处理。
>
>最近的工作深入研究了基于**输入空间信息**来降低计算成本的前景，通过提出专门的网络架构 or通过设计与一般CNN架构兼容的计算流
>
>在视频流媒体和分析学，感兴趣区域（RoIs）交叉框架跟踪(Edge-Assisted [47] and Elf [92]) 或者通过低分辨率检测(DDS [9])
>
>通过基于RoI的编码，框架传输数据的大小显著降低了。

<img src=".\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221201163112673.png" alt="image-20221201163112673" style="zoom: 67%;" />

(2)本文：

>在本文中，我们提出了一个自适应的框架,NeuLens,用于在移动设备和边缘设备上的动态CNN推理加速。首先，我们设计了一种新的动态机构,assemble聚集区域感知卷积（ARAC）超级网,这有效地降低了推理成本，且精度损失较小。ARAC 超网是一个空间分割网络集成。它根据图像与最终预测的相关性，自适应地选择不同大小的子网络。
>
>此外，我们设计了一个轻量级的在线控制器，DEMUX (§5),在实际应用中，基于服务水平目标（SLOs），动态调整每个平铺的子网络选择和超级网络的配置
>
>最后，我们综合评估了 ARAC supernet在不同的移动/边缘平台和各种应用 applications中
>
>根据我们的评估，ARAC supernet在相同的延迟/内存约束下，与最先进的（SOTA）动态推理方法相比，实现了高达67.9%的精度提高。在具有相同的推理延迟，比SOTA模型压缩技术的精度高出1.23×
>
>除此之外，将ARAC超网应用于连续目标检测系统中，比SOTA技术提高7.7×



(3)我们总结了本文的贡献如下：

>①开发用于移动/边缘计算平台的  新型CNN加速机制
>
>通过利用图像上空间和深度冗余，在CNN中，我们提出了一种加速机制，ARAC supernet。这有效地减少了计算资源的消耗，但精度略有降低。
>
>与现有的加速工作相比，ARAC supernet，在准确性-延迟权衡上实现了帕累托最优。我们强调以下高级技术 在 ARAC supernet中：
>
>- 构建ARAC supernet ，这通常适用于CNN的架构。通过将输入图像分割成块， supernet 利用不同压缩级别的子网络进行分析它们。超级网的输出是连接起来的，输入到CNN模型的剩余层中，去计算最后的结果。这种结构允许supernet减少计算中的空间和深度冗余，不影响原网络的整体工作方案。
>
>- 在ARAC supernet中，每一块内容感知调整计算成本。在ARAC supernet中 设计压缩导向门（A compression guiding gate）用于有效分析每块中的内容，以及分配一个具有适当压缩级别的子网去分析。提出一种标签规则来自动生成压缩导向门的训练集
>
>- 从操作冗余减少到设备上延迟加速的有效转换。 In ARAC supernet，将计算流分为多个独立的微流。基于其输入的内容（一个块）。每个微流在独立分析输入时调整操作量（压缩水平）。当每个微流被加载到设备的计算单元中时（如GPU）就像一个单独的神经网络，其操作减少直接转换为延迟加速。
>
>  
>
>②设计一种针对移动/边缘设备的轻量级slo感知控制器，以适应有限的计算预算
>
>我们设计了一个在线轻量级控制器，DEMUX，基于用户的SLO，使用可忽略的在手机或移动设备上的开销来调整调整ARAC supernet.
>
>在一个ARAC supernet 参数中，给了专门的操作， DEMUX自适应地选择最优的参数集，并在用户的SLOs范围内保持较高的精度。
>
>
>
>③实现了ARAC supernet 并 在不同的移动/边缘计算平台和各种视觉应用程序上执行了性能评估
>
>我们从几个方面对ARAC超网的性能进行了综合评价。并证明了其在提高移动/边缘设备上的cnn相关应用程序的整体性能方面的有效性。我们突出我们的评估结果如下：
>
>- 在手机霍边缘设备上，动态预测和模型压缩比SOTA 技术分别高出67.9%（7.2）和1.23×（7.4）
>- 将SOTA连续目标检测系统的整体性能提高7.7×
>- 在针对混合现实设备的SOTA 3D异议检测系统上，端到端延迟减少了近50%

### 2、背景和动机

#### 2.1空间相关卷积

>正如[15,83]所示，在图像中可能有相当数量的冗余像素,它们是与准确识别无关的。一些工作集中于减少冗余像素的卷积操作。大多工作提出了空间神经结构。紧凑型网络被设计用于减少基于空间冗余的操作。序列网络的设计具有多尺度分辨率。
>
>CBAM [80]设计了一个注意模块，它可以插入CNNs中。其他最近的工作提出，基于空间 冗余的计算流修改，这可以普遍适用于CNN 框架，而不用重新设计一个新的。
>
>GFNet [77， 30] 动态处理序列图像上的裁剪，直到具有足够置信度的预测。
>
>DRNet [95] 使用分辨率预测器预测每个输入图像的最佳分辨率
>
>SAR [20]设计了一种双分支网络架构，其中一分支分析低分辨率输入特征，并在每层中为另一层选择高分辨率细化区域。
>
>与这些研究相比，我们提出了一种新的计算流，ARAC supernet,用来处理空间冗余。通过将输入的图片分块，我们基于他们的内容，在supernet中选择不同的子网络。 ARAC supernet 通常适用于流行的CNN架构。
>
>值得注意的是，SAR，CGNet和ASC是不完全支持深度学习平台的实际加速的，消炎药特殊的硬件和框架支持。
>
>相反，我们的工作可以有效在SOTA是的学习平台上实现，并且实现延迟加速。

#### **2.2 Dynamic Inference**动态推断

>我们将动态推理的SOTA研究根据它们是否为平台和SLO自适应研究 分为两种类型。
>
>**与平台和 SLO 无关**：一些工作主要集中在用动态机制来设计或修改单一的网络。早期存在的拓扑被提出用于CNN（[3]，MSDNet [31]和ZTW [79]）。
>
>CNMM [60] 和 RNP [45] 设计了可以动态修剪的网络
>
>Skipnet [76] 和 BlockDrop [81] 根据输入跳过 ResNet 中的块。
>
>ConvNet-AIG [73] 根据估计的相关性确定是否跳过每一层，
>
>CoDiNet [75]基于交叉图像相似度优化层跳过
>
>LCCL [8] 在特征图中避免计算0，通过预测他们的位置。
>
>其他的工作还开发了用于动态推理的网络集成。
>
> Russian Doll Network[35]通过将较小的子网嵌入较大的子网中来构建嵌套网络
>
>HNE [61]设计了一个层次化的神经集成，允许对分支数进行调整。
>
>Slimmable networks 根据不同输入，调整层中的过滤器数量
>
>CoE [94]汇集了由数据集中互斥子集训练的网络集合。
>
>CondConv [86] 构建具有专家混合的子网络，并为每个输入选择它们的组合。
>
>整体而言，这些工作的重点是训练优化和结构修改。他们的设计是手动的，没有考虑到对平台和SLOs的适应性
>
>
>
>**平台和slo相适应的**：相比之下，一些工作集中于在SLOs的移动设备和边缘设备上的，系统级性能的优化。一些工作为一般的cnn设计自适应框架。
>
> NestDNN在一个紧凑的多容量模型 中动态实现资源精度权衡
>
>ReForm [85]提出了一种基于ADMM算法的资源感知DNN重构框架
>
>DMS [38]通过自适应剪枝来控制推理的资源需求。
>
>PatDNN [53]设计了一个基于核模式剪枝的高效DNN框架。
>
>LegoDNN [19]通过切换重新训练的后代块来动态缩放DNNs
>
>其他工作通过  处理视频分析等特定应用程序中的特性   来开发自适应框架 和 实时（视频）目标检测 (AdaVP [48], ApproxDet [84], Remix [37], and [22]).
>
> 
>
>我们：
>
>与他们的工作相比，我们ARAC supernet 实现了一个动态机制 ， 基于空间冗余 通过 将输入分块，并使用不同的压缩子网络来处理它们 。
>
>此外,通过设计用于在线设备推理的sloo自适应在线控制器，我们将ARAC supernet集成到移动系统和边缘系统中
>
>换句话说，我们的工作还解决了对平台和slo的适应性问题。



#### **2.3 Observations** 观察结果

>ARAC supernet和SOTA动态模型的  精度-延迟权衡  如图1所示。
>
>基于空间的工作（MSGFNet [30]，DRNet [95]，MSDNet [31]，CGNet [27,28]，和SAR [20]）和与空间无关的工作（LegoDNN [19]，DS-Net [43]，HNE [61]）都包括在比较中。
>
>在相同的推理延迟，ARAC supernet 的准确率比SOTA方法高出1.22%到2.07%。在具有相同精度，ARAC supernet  减少了19.4%到47.9%的时间在每次推理中。
>
>基于空间冗余的大部分SOTA方法并没有实现实际的加速。其根本原因是 他们在冗余部分上的分割操作，是与图层之间的中间映射不兼容。
>
>例如，SAR[20]（即SOTA像素级动态网络）从每个层的输入特征中选择一组提取的图案，并只对它们运行操作。这些图案是不规则的，每层都会生成不同的patterns ，使得在GPU上计算效率不太好
>
>相比之下, ARAC supernet利用了空间相关的操作减少和动态推理，通过在supernet中构建多个不同大小的子网络，并将CNN推理流划分为空间维度。supernet通过在内部选择适当的子网，可以自适应地调整每个分离流的计算。每个分裂流没有相互依赖，作为一个独立的微网络执行。
>
>数据流从一个层到另一个层，仅发生在每个微型网络内部。因此每一个微型网络可以作为一个CNN模型被加载到GPU内部，以及他们的节省操作可以有效的被转化为延迟加速。
>
>注意，尽管GFNet [30,77]设计了基于空间的CNN工作流，其操作减少实现了真正的加速，但由于对一系列裁剪图像的连续执行，它的加速速度受到了限制。对MSDNet同样如此。



>我们在图2中演示了两种基本的CNN加速技术的性能。适用于实际加速的SOTA研究都是建立在这其中之一的基础上的。
>
>第一种加速技术是将输入图像调整到不同的尺度(Fig. 2 (a)).随着更详细的信息丢失，分辨率96的准确率低于50%。即使是SOTA在动态分辨率上进行，DRNet,保存分辨率在96上。因此，单独调整分辨率不能在保持精度的同时产生大范围的延迟。
>
>第二种加速技术是在层中剪枝通道，被LEgoDNN 和DS-Net采用。注意，通道剪枝是和权重剪枝不同的，权重剪枝需要特殊硬件支持来实现加速。
>
>如图2 (b)所示，尽管，在一些通道被剪枝后，准确度不会急速下降，但是，也没有显著的延迟减少。当更多的通道被剪枝后，延迟急剧下降，但是精确的也显著下降了。



#### **2.4 Challenges**

>一种方法同时利用输入降低尺度和通道剪枝 在fig 3中展示。
>
>我们准备了四个不同大小的子网络（在ResNet-50的前三个块上压缩，带有四个通道剪枝级别）。我们将原始输入图像分割成3×3个块（78×78像素）。我们将每一块与子网络大小匹配，并且这一块作为输入到子网络中。
>
>所有的3x3块的输出是连接的，并且联合输入到ResNet-50中的最后一个块，以生成整个图像的最终结果。
>
>在初步研究中，我们详细的搜集了所有四种图像的可能配对。并且我们在图中标记了每个图像的块和模型大小的最优配对，在fig 3.如图3所示，在不影响预测正确性的情况下，显著减少了操作的数量和延迟。
>
>这种分裂CNN流的成功源于俩个因素： 首先，没有调整原始图像分辨率的大小。当数次缩小尺寸的时候，保持分辨率的分裂块保留了详细的信息。其次，该方法遵循人类视觉流的本能，对于重要的部分进行高强度分析，对不重要的部分可进行低强度分析。
>
>一个图像的所有子网络块可以被当作一个微型模型独立加载，并且他们输出被整合来产生图像最后的预测。子网络为不同大小的块实现了各种所需的计算。



>然而，经过上述的最初研究展示了分块输入图像的好处，在实施的过程中也存在着很多的挑战，如一个有益的推理流：首先，如何设计和准备不同大小的高效且有效的子网络。
>
>我们设计了一个具有信道切片的子网络，称作ARAC supernet .较小的子网络可以使用信道切片直接从大的网络中提取。我们通过一个集成促进的方案来同时训练所有的子网络。
>
>第二，在现实中，不可能穷尽所有的可能配对来寻找最优的一个。我们提出了一个轻量级的引导梦来自主为子网络选择块。
>
>第三：如何将提出的推理流应用于实际的应用中，尤其是满足服务水平的目标。
>
>我们提出了一个用于在线设备推理的轻量级的在线控制器，
>
>
>





### **3 SYSTEM OVERVIEW**

>提出了针对移动和边缘计算平台的slo-自适应深度学习加速框架,NeuLens,如图4所示。NeuLens包含一个一次性的离线阶段和一个轻量级的在线阶段。
>
>**离线阶段**,一个CNN模型被修改为一个ARAC supernet（supernet 生成图4中）
>
>一个压缩引导门被设计来为图像的每个块选择子网络（CGG设计在Fig4）
>
>内存预测器，延迟预测器，和精度比较器 是基于supernet 的分析数据进行训练的（图4）
>
>.**在线阶段**一个轻量级在线控制器 DEMUX，被设计来寻找最优参数。如，块大小，层数以及 ARAC supernet的每块压缩级别。
>
>DEMUX根据输入图像的块的内容和服务水平目标（SLOs） 自适应地选择这些参数，
>
>基于输入图像的内容和应用程序的slo，NeuLens，将一个CNN推理计算流划分为多个微流，并且独立地控制每个微流的计算代价。通过这种方式，NeuLens 成功放大了 SOTA CNN加速技术的好处，如，基于空间冗余的计算-操作减少和通道剪枝。
>
>随着ARAC supernet 技术的提出，在相同的延迟/内存约束下，NeuLens 能比最先进的（SOTA）动态推理方法达到高达67.9%的精度提高。在具有相同的推理延迟 ，NeuLens 能比SOTA模型压缩技术的精度可高出1.23×





### **4 DESIGN OF ARAC SUPERNET**

#### **4.1 Workflow of ARAC Supernet**

>如图5所示，输入的图像被分割成𝑘×𝑘块。这些块被联合进入ARAC supernet,  并且 他们是独立并行处理的。
>
>基于他们的内容，他们有supernet 中不同的子网络处理。每一块的输出是连接的，并且进一步联合到模型中后面的层。有许多不同的方式，可以用ARAC supernet 形成这样的模型。（如*,* neural architecture search）
>
>在本文中，我们修改先存在的CNN模型结构，如(*e.g.,* MobileNet [26], ResNet-50 [21],Inception [69])　成为使用　ARAC supernet 的结构。



>给定一个使用N 层的 存在的CNN 模型，我们修改第P层，到ARAC supernet 保持剩余的层不变。在这个supernet中，我们使用通道修剪[42,44,49,50]等技术来压缩前p-1层使用不同的压缩级别，以及我们设置了R不同压缩级别C = {*𝑐*1*, ..., 𝑐**𝑅*} (0 ≤ *𝑐*1 *<* *...* *<* *𝑐**𝑅* ≤ 1)，它表示在前（𝑃−1）层中被修剪枝的输出通道的不同比例。
>
>因此，给定一个压缩级别Ci,前（𝑃−1）中每一层的输出通道数为：
>
>![image-20221202143608984](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202143608984.png)
>
>![image-20221202144014886](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202144014886.png)原始的输出通道个数，
>
>![image-20221202144051178](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202144051178.png)压缩后的通道个数，
>
>ci=0 d=D 代表没有剪枝,ci=1 d=0 代表输出被全部剪枝了
>
>高的压缩级别，代表前p-1层中，大部分被剪枝，反之亦然
>
>换句话来讲，级别越高的子网络计算成本越低。基于每个块中的内容和延迟需求，系统为他们挑选不同级别的压缩。
>
>注意，尽管ARAC supernet 将一个图片分成不同块，分块不会影响原始图像中多个块的特征提取。例如，跨块给定一个目标的关键特征，supernet将会用较高的计算量处理相应的块。
>
>supernet之后，不同的块中提取的特征被输入到最后一层的共享层中，并且最后的几个是基于来自所有块的信息产生的。
>
>此外，ARAC supernet 结构并没有打破原始模型的输出。ARAC supernet 通常适用于各种基于cnn的视觉任务。



#### **4.2 Intermediate Dimensions in Supernet**(supernet 中的中间尺寸)

>对于ARAC中的子网络，（𝑙+1）层的输入尺寸等于第𝑙层输出的尺寸。
>
>来自某一层的输出深度（即输出通道的数量）由压缩级别决定，如Eq. 1.所示
>
>对于图层𝑙，输出和输入的空间维度之间的关系（高度和宽度）是由图层的配置决定的
>
>![image-20221202152517006](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202152517006.png)
>
>
>
>b:是二进制的,是否这个块在原始图像的边缘，
>
>layer l:卷积层   b=1 否则b=0
>
>Sl :步长
>
>Fl:卷积核大小
>
>Al: padding
>
>当我们将输入图像分割成𝑘xk块时，并，使用supernet内部不同的子网络独立地处理它们，来自supernet的𝑘×𝑘输出需要连接在一起。
>
>为了连接它们，每个输出的空间大小被设置为![image-20221202160440974](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202160440974.png)
>
>![image-20221202160457166](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202160457166.png)是（𝑃+1)层输入的空间尺寸
>
>给定![image-20221202160915023](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202160915023.png)，supernet中输入和输出的中间空间维度可有Eq.2决定。
>
>因此，我们可以得到每个块的空间尺寸，标记为![image-20221202161120161](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202161120161.png)。进一步，我们可以确定原始输入图像每一个块的坐标 𝑥1,: = 0, 𝑥2,: = 𝑤𝑖𝑛 − Δ𝑥𝑖𝑛, 𝑥𝑖,: = 𝑥𝑖−1,: + 𝑤𝑖𝑛 − Δ𝑥𝑖𝑛 (3 ≤ 𝑖 ≤ 𝑘); 𝑦:,1 = 0, 𝑦:,2 = ℎ𝑖𝑛 − Δ𝑦𝑖𝑛, 𝑦:,𝑗 = 𝑦:,𝑗−1 + ℎ𝑖𝑛 − Δ𝑦𝑖𝑛 (3 ≤ 𝑗 ≤ 𝑘);
>
>xi为第𝑖行块的𝑥坐标，𝑦：，𝑗为第𝑗列上块的𝑦坐标。
>
>Δ*𝑥**𝑖𝑛* = (*𝑘* · *𝑤**𝑖𝑛* −*𝑊**𝑖𝑛*)/(*𝑘* − 1), and
>
>Δ*𝑦**𝑖𝑛* = (*𝑘* ·*ℎ**𝑖𝑛* −*𝐻**𝑖𝑛*)/(*𝑘* −1). 
>
>注意：原点位于原始输入数据的左上角。



>在ARAC supernet中，每个块通过一个压缩级别独立处理。我们将（i,j）块的压缩级别记作ci,j
>
>对于supernet 中的卷积层，操作次数（乘积）是<img src=".\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202162245453.png" alt="image-20221202162245453" style="zoom:80%;" />。
>
>对于supernet中的最大池化层，操作（比较）数为<img src=".\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202162512376.png" alt="image-20221202162512376" style="zoom:80%;" />
>
>因此，使用{𝑐𝑖，𝑗}的supernet的操作总数可描述为：![image-20221202162636619](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202162636619.png)
>
>

#### **4.3 Training of Supernet**

>对有N层的CNN模型，我们离线训练R压缩模型使用压缩等级Ci.一种简单的方法是单独训练𝑅压缩模型。然而，以这种方式获得的模型不共享权重，并且在边缘设备上消耗了大量的内存。
>
>因此，我们通过通道切片形成了一个𝑅模型的集合。我们将有C1压缩等级模型的每一层的权重即为Wl;对于在这个集合中拥有其他压缩等级Ci(2<i<N)的模型，它们在第𝑙层中的权重是
>
>![image-20221202163452612](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202163452612.png)![image-20221202163505539](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202163505539.png).这样，个𝑅模型集合中的权重数等于压缩级为𝑐1的模型，并且其他的模型则可以通过对通道维数上的权值进行切片来直接得到。
>
>为了训练𝑅（压缩）模型的集成，我们使用了一个类似于IEB的集成促进方案。我们与IEB之间的区别在于，不是训练随机选择模型，而是，我们训练压缩级别范围从 C2 到 cR−1 的模型，来预测由压缩级别为 C1 的模型生成的 soft label ![image-20221202164417313](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202164417313.png)
>
>具有C1压缩级别的模型来预测真实的标签。![image-20221202164728367](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202164728367.png)
>
>具有CR压缩等级的模型，被训练来预测所有其他模型的概率积累，![image-20221202164951517](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202164951517.png)
>
>
>
>类似于训练一个缩减的神经网络，我们在每次训练迭代中一起训练𝑅模型的集合：
>
>我们分别计算上述为所有模型定义的损失，并将它们的反向传播梯度累积在一起；然后，我们更新了集合中的权重。
>
>注意：训练过程中这些模型的输入是 训练数据集中的图像没有被分割，
>
>给定训练好的𝑅模型集合，拥有P层的supernet可以通过取集合中每个模型的前𝑃层得到。
>
>后面的（𝑁−𝑃）层与原始CNN模型相同。

#### **4.4 Compression Guiding Gate**

>在ARACsupernet 的入口处，我们设计了一个压缩导向门 (CGG)。一个CGG为每个图像块选择压缩级别。特别是，CGG将分割后的图像块作为输入，并为它们生成压缩级别。
>
>CGG可以看作是一个分类模型，它将分割后的图像块分为不同的压缩级别。但是，在训练集中的图像被标记为原始的应用程序，而且我们没有针对分割图像的压缩级标签。
>
>因此，我们提出了标签生成规则来生成压缩等级的标签，用来分割训练集的图像。
>
>通过生成的压缩级标签，我们可以用监督学习技术来训练一个CGG



>4.4.1标签规则：
>我们将原始图像的标签称为A标签，将分割图像压缩等级的标签作为C-标签。
>
>给定压缩等级![image-20221202172540474](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202172540474.png)，我们在训练集中用C标签来标记所有的分割图像。
>
>对于一个图像的分割图像，标签规则是：
>
>首先，所有的分割图像都由压缩等级为c1的子网络处理。如果他的最终输出没有和A标签匹配，然后所有的分割图形被标记为C1，否则我们分摊提升每个分割图像的压缩等级为C2;
>
>如果最后的输出没有和A标签匹配，将相应的图像标记为c1，否则，我们提升他的压缩等级为C3.我们重复*match-label-raise procedure until we reach compression level CR
>
>图6显示了给定四个压缩级别的标记过程的一个例子。



>4.4.2 CGG体系结构
>给定一个输入图像X,CGG生成𝑘×𝑘one-hot 𝑅长度向量{v}𝑘×k. 向量中的一个元素代表了一个压缩级别Ci(1 ≤ *𝑖* ≤ *𝑅*)。CGG的最终输出为![image-20221202222054848](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202222054848.png)CGG在一个batch中处理X个kxk的分割图像。将一个分割的图像记为x,我们有一个编码器![image-20221202222347851](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202222347851.png),以及一个特征映射函数![image-20221202222423251](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202222423251.png)，对于编码器![image-20221202222448028](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202222448028.png)。我们利用一个卷积层和一个最大池化层来整合空间信息。我们利用一个全连通的层来进行特征映射函数![image-20221202222616599](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202222616599.png)，
>
>压缩级别的选择是通过将arg max应用于映射函数![image-20221202222616599](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202222616599.png)的输出向量



>*4.4.3 CGG Training:*
>
>我们使用标记规则对训练数据集中的分割图像进行标记。我们将压缩级损失函数定义为：![image-20221202223318175](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202223318175.png)其中𝑥是一个分割图像。𝐶𝐺𝐺（𝑥)是𝑥的softmax输出。𝑐𝑔𝑡（𝑥)是一个独热编码的压缩级标签x.图像识别损失为
>
>![image-20221202223548584](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202223548584.png)
>
>![image-20221202223606862](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202223606862.png)是来自整个模型的 softmax output 
>
>![image-20221202223702691](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202223702691.png)真实标签的独热码。
>
>我们通过图像识别损失和压缩级损失来联合优化CGG：![image-20221202223843156](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202223843156.png)
>
>其中，𝛼控制了这两个损失的影响
>
>![image-20221202224011035](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202224011035.png)的反向传播给CGG的参数是通过gumbel softmax technique来实现的。
>
>带有损失函数，CGG被训练来分析分割图像的内容 并且为它选择一个压缩级别。
>
>CGG的训练确保了整个模型的正确输出(the first part in Eq. 4) ，同时匹配压缩级别的标签。
>
>拥有一个更高的a，CGG倾向于分配具有更高压缩水平的块，以更显著地降低计算成本。
>
>拥有一个更低的a,CGG 倾向于分配具有更低压缩水平的块，来获取更高的准确率。本文设置a=0.7



### 5 CNN INFERENCE W/ SUPERNET(CNN推理)

>在ARCR supernet中一个压缩导向门CGG选择合适的压缩级别 为图像的块。
>
>从CGG中选择压缩级别保证了以最小的操作次数来实现可忽略的精度损失。
>
>然而，CGG是SLO无关的，如，它选择压缩级别，而不考虑延迟和内存限制。因此，我们设计了另一个SLO自适应组件。压缩级齿轮（5.2），以在线调整所选的压缩级，以满足SLOs
>
>给定候选的可控参数，一个轻量级的在线控制器，DEMUX，被设计来寻找可控参数的最优集。



#### **5.1 Problem Definition**

> 如§4所展示的，一个ARAC supernet 包含了就有R压缩级别的前P层CNN模型，一个输入图像被分为了kxk块，并且，本supernet中的微型网络独立处理。因此，对于具有ARAC supernet 的CNN推理，我们可以动态控制：
>
> （1）supernet中的层数，![image-20221202234337279](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234337279.png)
>
> （2)块数![image-20221202234353981](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234353981.png)
>
> （3)块的压缩级别![image-20221202234427075](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234427075.png)。我们将kxk块矩阵的压缩级别记为![image-20221202234532961](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234532961.png)以及C是
>
> ![image-20221202234610250](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234610250.png)的候选值。
>
> 一般来说，CNN推理的优化集中于三种类型的性能：：(1)精度（𝐴𝑐𝑐）、(2)延迟（𝑇）和(3)内存消耗（𝑀)。在本文中，我们关注于在给定的延迟和内存约束下![image-20221202234759538](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234759538.png)![image-20221202234810142](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234810142.png)最大化精度
>
> ![image-20221202234831341](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202234831341.png)



>尽管，更高的压缩级别会降低精度 (Eq. 5),但是，获得更低的延迟/内存消耗；在调整压缩级别中存在权衡，因为高压缩级别会导致低精度，尽管满足了延迟和内存约束。
>
>对于输入图像的块，ARAC supernet 利用不同的压缩级别来处理他们，换句话说，给定输入图像C压缩级别和kxk块，对于图像中所有的块来说由![image-20221202235343707](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221202235343707.png)的组合的压缩选择。这可能是一个非常大的数字。因此，一个有效的等式解决Eq5to7是必要的，而不是详尽的搜索。



#### **5.2 Compression-Level Gear**

>对于一个图像的𝑘×𝑘块，CGG（4.4）为它们选择压缩级别。我们将CGG选择的𝑘×𝑘块的压缩级别表示为![image-20221203000903099](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203000903099.png)
>
>推理延迟![image-20221203000940522](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203000940522.png)和和内存消耗![image-20221203001001366](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203001001366.png)是是由延迟和内存消耗预测器来估计的。
>
>当延迟/内存约束时(Eq。6和7)被违反了，我们使用压缩级别齿轮(CLG)调整𝑘×𝑘块的压缩级别，通过自适应地提高𝑘×𝑘块的压缩级别。
>
>我们描述了我们根据经验发现有效的方法（图7.3中的12(a)），这被称为基于置信度的步进（CS）方法。由于CGG是以一种有监督的方式进行训练的（4.4），它的置信度表示与压缩级别被正确识别的概率。
>
>因此，我们设计基于置信的CLG到![image-20221203001715680](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203001715680.png)
>
>具体来说，我们设置了一个置信度阈值𝜃𝑓和一个窗口长度Δ𝑤。
>
>置信度低于𝜃𝑓的块按置信度从低到高进行排序，而Δ𝑤块则是在其中循环选择的。
>
>CLG以循环运行，直到延迟和内存限制满意。
>
>在每个循环中，CLG将Δ𝑤块的压缩级别提高1



#### 5.3 DEMUX

>我们设计了DEMUX来寻找![image-20221203002127538](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203002127538.png),DEMUX由五个轻量级组件组成CGG (§4.4), CLG (§5.2),latency predictor, memory predictor, and accuracy comparator.
>
>DEMUX的工作流程如图7所示。.给定𝑃∈P和𝑘∈K，CGG和CLG寻找矩阵的压缩级别![image-20221203002324663](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203002324663.png)满足![image-20221203002342808](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203002342808.png)在找到所有矩阵的压缩级别之后![image-20221203002428843](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203002428843.png)
>
>精度比较器选择![image-20221203002505081](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203002505081.png)
>
>我们根据supernet中子网的压缩级别分析计算延迟和内存消耗。我们训练轻量级线性回归模型来预测一个supernet中的延迟和内存，以所有supernet中子网络的的压缩水平作为输入。
>
>注意，线性回归变量的训练是一次性的工作，并产生较小的离线开销，如，在只有CPU的电脑（Intel i7-8700K）上，训练时间不到5分钟。
>
>对于supernet中后面的其他层，我们可以简单地记录它们相对于层数的计算延迟和内存消耗。
>
>总的延迟/内存消耗是通过结合这两部分来获得的。如图8 (a)所示，预测的延迟和内存消耗与测量的结果相匹配，误差小于5%。
>
>从CLG的（𝑃，𝑘，Ω）集合中选择最优集，我们定义退化分数作为CGG选择的压缩级别和CLG调整的压缩级别之间的𝑘×𝑘块的平均差异。
>
>如图8 (b)所示，在准确性损失和退化分数之间有很强的相关性。
>
>因此，我们可以离线分析每对(*𝑃,𝑘*)以及一个回归系数![image-20221203003559510](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203003559510.png)。
>
>每对![image-20221203003645332](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203003645332.png)精确度的损失可以通过![image-20221203003708531](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203003708531.png)评估，以及我们选择![image-20221203003735411](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203003735411.png)这有最低的精确度损失。
>
>注意，对于每对![image-20221203003833096](.\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203003833096.png)，我们观察到可忽略的精度损失when使用CGG选择的压缩级别来处理块。
>
>





### **6 IMPLEMENTATION**

>ARAC 的实现如下：
>
>测试台：用于设备上推理评估，我们选择了三种异构的移动/边缘设备：Jetson TX2; Xiaomi 6 Plus;Alienware 17 R3。
>
>第一台设备运行Linux Ubuntu 18.04 LTS，第二款设备运行安卓10.0系统，第三台设备运行Windows10。
>
>基础CNN模型、数据集和框架：
>
>我们主要评估在移动系统和边缘系统上的两个最重要的应用：(1)图像分类：我们基于三种流行的CNN模型 (ResNet50 [21], MobileNetV3 [25], and Inception-V3 [69])构建了ARAC supernet.我们使用ImageNet数据集[7]；
>
>(2)目标检测：我们基于最常用的模型(YOLOv3 [58])构建了ARAC supernet.我们使用COCO数据集
>
>使用pytorch 框架
>
>我们还在7.7节中评估了其他9个视觉应用程序



**Baselines:**

>我们选择了三种性能优于图1中其他方法的SOTA方法：
>
>(i) DS-Net [43]通过针对不同输入进行通道切片来调整层中过滤器的数量。ARAC supernet 和DS-Net关键的不同是：我们将输入图像分成更小的块，并且使用压缩子网络来处理他们，但是DS-Net是处理整个图像。
>
>MS-GFNet [30]动态地处理图像上的一系物品，直到预测有足够的置信度。虽然MS-FGnet设计了基于空间的CNN工作流，其操作减少导致了真正的加速，但由于顺序执行，其加速受到了限制
>
>iii）LegoDNN [19]通过切换重新训练的后代块来动态缩放DNNs。它通过过滤器剪枝离线生成块集。
>
>它将一个图像作为一个整体，并使用自适应选择的块尺度对其进行处理。
>
>注意：DS-Net和LegoDNN是用于动态推理的SOTA方法，没有基于空间冗余的加速度。
>
>MS-GFNet是一种基于空间冗余加速的SOTA方法。
>
>我们还比较的ARAC supernet 和SOTA模型压缩技术  in §7.4.

**Candidate controllable parameters:**

>我们设置了3个块大小，通过将图像分离成了![image-20221203100735259](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203100735259.png)
>
>我们 设置了5个压缩等级，使用压缩率![image-20221203101012833](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203101012833.png)注意压缩率代表这通道剪枝的比率，如拥有所有的通道代表使用压缩率1进行剪枝。
>
>对于ResNet50,我们设置了3个层数选择，：2个块(11层）、3个块(23层）和4个块(41层）
>
>For MobileNet-V3，,我们设置了2个层数选择：8层，13层
>
> For Inception-V3,我们设置了3个层数选择，七层，在第一个初始模块之后和第二个初始模块之后
>
>For YOLO-V3,我们在supernet中设置了三个层数选项：在第一个、第二个和第三个残差块之后。

**Hyper-parameters in CLG**

>我们研究了CLG（5.2）中这两个超参数的影响。
>
>根据经验，最佳设置是：
>
>![image-20221203101838856](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\NeuLens Spatial-based Dynamic Acceleration of Convolutional  Neural Networks on Edge.assets\image-20221203101838856.png)

**Compression guiding gate:**

>在CGG中，编码器的卷积层
>
>
