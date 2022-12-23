# **Preservation of the Global Knowledge byNot-True Distillation in Federated Learning**

（***\*通过联邦学习中的非真实蒸馏来保存全局知识\****）



摘要：

>问题：但全局模型的融合通常会受到<font color='blue'>数据异构性</font>的影响
>本文：
>
>发现**遗忘**可能是联邦学习的瓶颈。我们观察到全局模型忘记了前几轮的知识，而局部训练导致忘记了局部分布之外的知识。
>
>根据我们的发现，我们假设解决遗忘问题将缓解数据异构性问题
>
>提出： **Federated Not-True Distillation(FedNTD)**
>
>它为**本地可用的**  非正确类别 **的数据** 保留全局视角
>
>在实验中，FedNTD 在各种设置上展示了最先进的性能，而不会损害数据隐私或产生额外的通信成本



1、简介

>①介绍联邦学习
>
>②介绍Fedavg ,引入Non-iid 数据异构的问题
>
>③介绍数据异构性 和解决的异构性的好处
>
>
>
>![image-20221204190648206](C:\Users\bai\AppData\Roaming\Typora\typora-user-images\image-20221204190648206.png)
>
>④持续学习：由于模型在一系列任务上不断更新，会导致，为了适应新任务，而忘掉先前任务中的参数。造成严重的遗忘。
>
>⑤猜想联邦学习同样存在着类似持续学习的遗忘
>
>⑥**观察结果验证了我们的猜想：联邦学习会遗忘**。全局模型的预测在通信轮次之间高度不一致，显着降低了先前模型原本预测良好的某些类别的预测性能，对应于局部分布之外的区域的全局知识很容易被遗忘。由于仅对本地模型进行平均无法恢复它，因此全局模型难以保留先前的知识。
>
>⑦我们假设减轻遗忘问题可以缓解数据异质性
>
>FedNTD 利用全局模型对本地可用数据的预测，但仅针对不正确的类别
>
>我们证明了 FedNTD 对保存本地分布之外全局知识的影响及其对联邦学习的好处。
>
>
>
>贡献：
>
>①研究遗忘，并且遗忘与数据异构有关  （二节）
>
>②提出FedNTD (三节)（四节）
>
>③分析FedNTD 如何让联邦学习收益   （五节）



***\*1.1前期准备工作\****

>1**Federated Learning**，介绍联邦学习
>
>2***\*知识蒸馏\****：https://zhuanlan.zhihu.com/p/102038521
>
>Knowledge Distillation，简称KD，顾名思义，就是将已经训练好的模型包含的知识(”Knowledge”)，蒸馏("Distill")提取到另一个模型里面去。
>
>训练过程中使用的模型较大，不方便部署到服务中去。因此，模型压缩（在保证性能的前提下减少模型的参数量）成为了一个重要的问题。而”模型蒸馏“属于模型压缩的一种方法。
>
>### 2.2. 知识蒸馏的关键点
>
>如果回归机器学习最最基础的理论，我们可以很清楚地意识到一点(而这一点往往在我们深入研究机器学习之后被忽略): **机器学习最根本的目的**在于训练出在某个问题上泛化能力强的模型。
>
>- **泛化能力强**: 在某问题的所有数据上都能很好地反应输入和输出之间的关系，无论是训练数据，还是测试数据，还是任何属于该问题的未知数据。
>
>  而在知识蒸馏时，由于我们已经有了一个泛化能力较强的Net-T，我们在利用Net-T来蒸馏训练Net-S时，可以直接让Net-S去学习Net-T的泛化能力。
>
>  一个很直白且高效的迁移泛化能力的方法就是：使用softmax层输出的类别的概率来作为“soft target”。
>
>  ![image-20221208154405606](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208154405606.png)
>
>  ![image-20221208155101766](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208155101766.png)
>
>  训练Net-T的过程很简单，下面详细讲讲第二步:高温蒸馏的过程。高温蒸馏过程的目标函数由distill loss(对应soft target)和student loss(对应hard target)加权得到。示意图如上。![image-20221208155133355](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208155133355.png)
>
>  软目标是由Teacher模型生成的最后一个softmax层，而硬目标则是真实的标签值。
>
>  ![image-20221208155324327](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208155324327.png)
>
>  ![image-20221208155622138](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208155622138.png)
>
>  温度就是用来控制负标签的影响。
>
>  当原来的老师模型较大时，可以温度较高，学习负标签里面的正确值，
>
>  当老师也不太行时，温度较小防止负数标签的影响。



**2 Forgetting in Federated Learning**

>***\*全局模型预测一致性\****
>
>在idd的情况下，学习的服务器模型在每一轮中均匀地预测每个类
>
>在非独立同分布的情况下，先前全局模型原本预测良好的某些类的测试精度往往会显着下降。这意味着遗忘发生在联邦学习中。
>
>遗忘度量**F**：每个类别最高的准确率和最终的准确率差值的一个平均值。![image-20221208163134729](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208163134729.png)
>
>***\*本地分布之外的知识\******2.2 Knowledge Outside of Local Distribution**
>
>遗忘可能是联邦学习的绊脚石。

***\*遗忘和局部漂移\****

**2.3 Forgetting and Local Drift**

>局部更新偏离理想的全局方向已被广泛讨论为异构联邦学习中收敛缓慢且[不稳定](#bookmark73)[的](#bookmark64)[主要原因](#bookmark73)[[ 21,30,31 ]](#bookmark74)
>
>局部外分布知识保存的一个有趣特性是它可以将局部梯度修正为全局方向。
>
>梯度多样性 A 来衡量局部梯度的相异性，并说明知识保存的效果如下：
>
>梯度多样性Λ![image-20221208214803876](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208214803876.png)
>
>, Λ>=1 测量局部函数f k s w.r.t.的梯度方向与全局函数方向对其。
>
>注意，当局部函数梯度∇fks的方向变得相似时，Λ变小，例如，如果大部分 ![image-20221208215919621](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208215919621.png)是固定的，
>
>![image-20221208220003956](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208220003956.png)方向相同时，其Λ最小。

>为了了解保存知识对非局部分布![image-20221208220225567](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208220225567.png)的影响，我们通过在![image-20221208220225567](C:\Users\bai\myflie\AI_compiler\移动端联邦学习\002106\Preservation of the Global Knowledge byNot-True Distillation in Federated Learning.assets\image-20221208220225567.png)上加入带有β因子的梯度信号，
>
>，分析了局部梯度及其多样性的变化情况，得到了以下命题。



***\*FedNTD\*******\*：联合非真实蒸馏\*****3 FedNTD: Federated Not-True Distillation***

>
>
>
