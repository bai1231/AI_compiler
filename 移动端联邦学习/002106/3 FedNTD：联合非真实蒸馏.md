**3** **FedNTD****：联合非真实蒸馏**

>FedNTD 的核心思想是只为非真实类保留全局视图。
>
>进行局部蒸馏，通过交叉熵损失函数
>
>![image-20221210095544851](C:\Users\19392\AppData\Roaming\Typora\typora-user-images\image-20221210095544851.png)
>
>![image-20221210101438437](C:\Users\19392\AppData\Roaming\Typora\typora-user-images\image-20221210101438437.png)
>
>与普通的联邦学习的唯一不同就是
>
>在每个batches更新全局模型时，所采用的目标损失函数不同。