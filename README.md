# 3D-Point-Cloud-Semantic-Segement Overview


[TOC]

---



- [3D-Point-Cloud-Semantic-Segement Overview](#3d-point-cloud-semantic-segement-overview)
- [1.1 基于投影的方法](#11-基于投影的方法)
  - [1.1.1 多视图表示](#111-多视图表示)
  - [1.1.2 球形表示](#112-球形表示)
- [1.2 基于离散化的方法](#12-基于离散化的方法)
  - [1.2.1 密集的离散化表示](#121-密集的离散化表示)
  - [1.2.2 稀疏的离散化表示](#122-稀疏的离散化表示)

- [1.3 混合方法](#13-混合方法)
- [1.4 基于点的方法](#14-基于点的方法)
  - [1.4.1 逐点的MLP方法](#141-逐点的mlp方法)

    - [临近特征池化](#临近特征池化)
    - [基于注意力的聚合](#基于注意力的聚合)
    - [基于局部-全局特征连接](#基于局部-全局特征连接)
  - [1.4.2 点卷积方法](#142-点卷积方法)
  - [1.4.3 基于RNN的方法](#143-基于rnn的方法)
  - [1.4.4 基于图的方法](#144-基于图的方法)
  - [1.4.5 其它方法](#145-其它方法)
- [3. 常用Benchmark DataSet](#3-常用Benchmark DataSet)
- [2. 评价指标](#2-评价指标)
	- [Public Datasets](#public-datasets)
	- [Benchmark Results](#benchmark-results) 
- [4. 存在的问题](#4-存在的问题)

---

# 1. 3D点云语义分割任务

​		三维点云分割既需要了解全局几何结构，又需要了解每个点的细粒度细节。根据分割粒度的不同，三维点云分割方法可以分为三类:语义分割(场景级)、实例分割(对象级)和部分分割(部分级)。	

​		对于给定的点云，语义分割的目标是根据点的语义意义将其划分为多个子集。与三维形状分类的分类方法类似(第3节)，语义分割有四种范式:基于投影的方法、基于离散的方法、基于点的方法和混合方法。投影和离散的方法的第一步是将点云一个中间正则表示,如多视点[181],[182],球形[183],[184],[185],[166],[186],[187],permutohedral晶格[188],[189],[190]和混合表示,[191],**见图11**。然后中间分割结果被投影回原始点云。相反，**基于点的方法**直接工作在不规则的点云上。几种典型的方法**如图10**所示。

![image-20200727151620742](https://cdn.jsdelivr.net/gh/lizhangjie316/img/2020/20200727151620.png)

## 1.1 基于投影的方法

​		这些方法通常将三维点云投影到二维图像中，包括多视图和球形图像。

### 1.1.1 多视图表示

### 1.1.2 球形表示

 



## 1.2 基于离散化的方法

​		这些方法通常将点云转换为稠密/稀疏的离散表示，如体积格和稀疏透面体格。

### 1.2.1 密集的离散化表示



### 1.2.2 稀疏的离散化表示



## 1.3 混合方法



## 1.4 基于点的方法

​		直接对无序的、非结构化的点云进行操作，开山之作**PointNet[论文地址]**用**共享权重的MLP**学习每个点的特征，使用**对称池化函数**学习全局特征。以此为基提出了一系列网络。分为以下四类：逐点的MLP方法、点卷积方法、基于RNN的方法、基于图的方法。

![image-20200727182233734](https://cdn.jsdelivr.net/gh/lizhangjie316/img/2020/20200727182233.png)

### 1.4.1 逐点的MLP方法

​		为了高效获取逐点特征，这些方法通常使用共享MLP作为其网络的基本单元，但共享MLP提取的点特征不能捕获点云的局部几何形状和点之间的相互作用。为了获取每个点更广泛的上下文并学习更丰富的局部结构，我们引入了一些专用网络，包括基于**邻近特征池化**、**基于注意力的聚集**和**基于局部-全局特征连接**的方法。

- 临近特征池化

- 基于注意力的聚合
- 基于局部-全局特征连接

#### 临近特征池化

​		为了获取局部几何模式，通过对局部邻近点的信息进行聚合来获得每个点的特征。

- **Pointnet++[论文地址54]**对点进行分层分组（即球查询），逐步从更大的局部区域进行学习。**todo：添加Pointnet++的网络结构图**  针对点云的不均匀性和密度变化等问题，提出了多尺度和多分辨率的聚类方法。？？？是++么？
- **PointSIFT[论文地址141]**提出了一个PointSIFT模块来实现方向编码和尺度感知。该模块通过three-stage有序卷积将八个空间方向的信息堆叠并编码，多尺度特征被连接在一起以实现对不同尺度的自适应。
- **Engelmann等[204]**利用**K-means聚类**和**KNN**分别定义了世界空间和特征空间中的两个邻域。代替了PointNet++中的球查询。基于来自同一类的点在特征空间中更接近的这一假设，引入**pairwise distance loss（双距离损失）**和**centroid loss（质心损失）**来进一步规范特征学习。
- 为了对不同点之间的相互作用进行建模，**Zhao等[57]**提出了**PointWeb**，通过密集地构建一个局部全连接的web来**探索一个局部区域内所有对点之间的关系**。**pairs of points是什么？**提出了一种**自适应特征调整(AFA)模块**来实现信息交换和特征细化。这种聚合操作有助于网络学习一种有区别的特征表示。
- **Zhang等人[205]**提出了一种基于同心球壳统计量的**置换不变卷积**Shellconv。该方法首先查询一组多尺度的同心球体，然后在不同的shell中使用max-pooling操作来汇总统计？使用MLPs和1D卷积得到最终的卷积输出。
- **RandLA-Net[206]**是一种高效、轻量级的用于大规模点云分割的网络。利用随机点采样（Random Sampling），在内存和计算方面取得了非常高的效率。进一步提出了一种局部特征聚合模块（LFA）来捕获和保存几何特征。

#### 基于注意力的聚合

- 在点云分割中引入了**注意机制[120]**。
- **Yang等人[56]**提出了一种group shuffle attention来建模点之间的关系，并提出了一种**置换不变量**、任务不可知且可微的Gumbel子集抽样(GSS)来替代广泛使用的FPS方法。该模块对异常值不太敏感，能够选择出一个有代表性的点云子集。      **注意力机制用于采样。**
- 为了更好地捕捉点云的空间分布，**Chen等人[207]**提出了**局部空间感知(LSA)层**，基于点云的空间布局和局部结构来学习空间感知权值。
- **与CRF类似**？**Zhao等人[208]**提出了一个基于注意力的评分细化(ASR)模块，用于对网络产生的分割结果进行后处理。通过将相邻点的分数与学习到的注意权值相结合，对初始分割结果进行细化。这个模块可以很容易地集成到现有的深度网络，以提高分割性能。      <font color='red'>    **增分神器。**</font>

#### 基于局部-全局特征连接

- **Zhao等人[112]**提出了一种**置换不变的PS2-Net**来结合点云的局部结构和全局上下文。
- **Edgeconv[87]和NetVLAD[209]**被反复堆叠，以捕捉本地信息和场景级的全局特征。

### 1.4.2 点卷积方法

​		这些方法趋向于提出有效的点云卷积操作。

- **Hua等[76]**提出了一种逐点卷积操作，将相邻的点分割到核单元中，然后用**核权值**进行卷积。
- **Wang等[201]**提出了一种基于参数连续卷积层的网络**PCCN**，如图12(b)所示。该层的核函数由MLPs参数化，并张成连续向量空间。
- **Thomas等[65]**提出了一种基于核点卷积(KPConv)的核点全卷积网络(Kernel  Point full Convolutional Network, **KP-FCNN**)。KPConv的卷积权值由到核点的欧几里得距离决定，核点的数量不是固定的。将核点的位置表示为球空间中最优覆盖的优化问题。需要注意的是，使用半径邻域来保持一致的感受场，而在每一层使用网格子采样来实现不同密度点云下的高鲁棒性。
- **在[211]中，Engelmann**等提供了丰富的消融实验和可视化结果来展示**感受野对基于聚合方法性能的影响**。他们还提出了一种**扩展点卷积(DPC)操作**来聚合扩展的邻近特征，而不是K个最近邻。该操作被证明是非常有效的增加接受场，并**可以很容易地集成到现有的基于聚合的网络。**      <font color='red'>    **增分神器。**</font>

### 1.4.3 基于RNN的方法

​		为了从点云中捕获内在的上下文特征，递归神经网络(RNN)也被用于点云的语义分割。

- **Engelmann等[213]**基于PointNet[5]，首先将一个点块转换为多尺度块和网格块，获得输入级上下文。然后，将PointNet提取的块化特征依次输入到合并单元(CU)或周期性合并单元(RCU)中，获得输出级上下文。实验结果表明，结合空间上下文对提高分割性能具有重要意义。
- **Huang等[212]**提出了一种轻量级局部依赖建模模块，利用slice pooling 层将无序的点特征集转换为有序的特征向量序列。
- 如图12(c)所示，**Ye等人[202]**首先提出了**点态金字塔池(3P)模块**来捕获由**粗到细的局部结构**，然后利用双向分层RNNs进一步获取远程空间依赖关系，然后应用RNN实现端到端学习。

<font color='red'>    **然而，这些方法在用全局结构特征聚合局部邻域特征时，失去了点云丰富的几何特征和密度分布[220]。**？</font>

- 为了缓解刚性池化和静态池化操作带来的问题，Zhao等人[220]提出了一种同时考虑全局场景复杂度和局部几何特征的动态汇聚网络(DARNet)。利用自适应接收域和节点权值，动态聚合介质间特征。
- Liu等人[221]提出了**3DCNN-DQN-RNN**用于大规模点云的高效语义解析。该网络首先使用3D  CNN网络学习空间分布和颜色特征，然后使用DQN对属于特定类的对象进行定位。最后将拼接后的特征向量送入残差神经网络，得到最终的分割结果。

### 1.4.4 基于图的方法

​		使用图网络捕获3D点云形状与几何结构。

- 如图12(D)所示，**Landrieu  et  al.。[203]**将点云表示为一组相互关联的简单形状和超点，并使用属性有向图(即超点图)来捕捉结构和上下文信息。然后，将大规模点云分割问题分解为几何均匀分割、超点嵌入和上下文分割三个子问题。
- 为了进一步改进划分步骤，**Landrieu和Boussaha[214]**提出了一个监督框架来将点云过度分割？成纯超点。该问题被描述为一个由邻接图构造的深度度量学习问题。此外，还提出了一种图结构的对比损失来帮助识别物体之间的边界。
- 为了更好地捕捉高维空间中的局部几何关系，Kang等人提出了一种新的方法。[222]提出了一种基于**图嵌入模块(GEM)**和**金字塔注意力网络(PAN)**的PyramNet。GEM模块将点云表示为有向无环图，并用协方差矩阵代替欧几里德距离构造相邻相似度矩阵。PAN模块使用四种不同大小的卷积核来提取不同语义强度的特征。
- 在[215]中，图注意卷积(GAC)被提出用来从局部相邻集合中选择性地学习相关特征。该操作是通过基于不同的邻近点和特征通道的空间位置和特征差异动态地分配关注度权重来实现的。GAC可以学习获取可区分的特征进行分割，并且与常用的CRF模型？具有相似的特征。
- Ma等人。[223]提出了一种点全局上下文推理(PointGCR)模块，使用无向图表示，沿通道维度捕获全局上下文信息。PointGCR是一个即插即用的端到端可训练模块。它可以很容易地集成到现有的分段网络中，以实现性能提升。     <font color='red'>    **增分神器。**</font>



### 1.4.5 其它方法

- 弱监督下的语义分割
  - 魏等人，[224]提出了一种两阶段训练具有云下层次标签的分割网络的方法。
  - 许等人，[225]研究了几种不精确的点云语义分割监督方案。他们还提出了一种仅能用部分标记点(例如10%)进行训练的网络。



# 2. 评价指标

 OA (Overall Accuracy)：总体精度

 mIoU (mean Intersection over Unionand)：平均交并比

mAcc (mean class Accuracy)：平均类别精度

MAP(mean Average Precision) : 平均精度均值 ,长用于3D点云实例分割。



# 3. 常用Benchmark DataSet

## Public Datasets

- ScanNet (CVPR'17) [[paper]](https://arxiv.org/pdf/1702.04405) [[data]](https://github.com/ScanNet/ScanNet) [[project page]](http://www.scan-net.org/) [[results]](http://kaldir.vc.in.tum.de/scannet_benchmark/)  
- S3DIS (CVPR'17) [[paper]](http://buildingparser.stanford.edu/images/3D_Semantic_Parsing.pdf) [[data]](https://docs.google.com/forms/d/e/1FAIpQLScDimvNMCGhy_rmBA2gHfDu3naktRm6A8BPwAWWDv-Uhm6Shw/viewform?c=0&w=1) [[project page]](http://buildingparser.stanford.edu/dataset.html#Download)
- Semantic3D (ISPRS'17) [[paper]](https://www.ethz.ch/content/dam/ethz/special-interest/baug/igp/photogrammetry-remote-sensing-dam/documents/pdf/Papers/Hackel-etal-cmrt2017.pdf) [[project page]](http://www.semantic3d.net/)
  - _semantic-8_ [[data]](http://www.semantic3d.net/view_dbase.php?chl=1#download) [[results]](http://www.semantic3d.net/view_results.php?chl=1)
  - _reduced-8_ [[data]](http://www.semantic3d.net/view_dbase.php?chl=2#download) [[results]](http://www.semantic3d.net/view_results.php?chl=2)
- Paris-Lille-3D (IJRR'18) [[paper]](https://arxiv.org/pdf/1712.00032) [[data]](https://cloud.mines-paristech.fr/index.php/s/JhIxgyt0ALgRZ1O) [[project page]](http://npm3d.fr/) [[results]](http://npm3d.fr/paris-lille-3d) 
- SemanticKITTI (ICCV'19) [[paper]](https://arxiv.org/pdf/1904.01416) [[data]](http://semantic-kitti.org/dataset.html#download) [[project page]](http://semantic-kitti.org/index.html) [[results]](https://competitions.codalab.org/competitions/20331#results)
- Toronto-3D(CVPRW2020)[[paper]](https://arxiv.org/pdf/1904.01416) [[data]](http://semantic-kitti.org/dataset.html#download) [[project page]](http://semantic-kitti.org/index.html) [[results]](https://competitions.codalab.org/competitions/20331#results)
- DALES(CVPRW2020)[[paper]](https://arxiv.org/pdf/1904.01416) [[data]](http://semantic-kitti.org/dataset.html#download) [[project page]](http://semantic-kitti.org/index.html) [[results]](https://competitions.codalab.org/competitions/20331#results)

![image-20200727195450821](https://cdn.jsdelivr.net/gh/lizhangjie316/img/2020/20200727195450.png)

对于3D点云分割，这些数据集由不同类型的传感器获取，包括移动激光扫描仪(MLS)[15]、[34]、[36]、空中激光扫描仪(ALS)[33]、[38]、静态陆地激光扫描仪(TLS)[12]、RGBD相机[11]和其他3D扫描仪[10]。这些数据集可用于开发各种挑战的算法，包括相似干扰、形状不完整和类别不平衡。

## DataSet简介

- Semantic3D 











## Benchmark Results

![image-20200727201714103](https://cdn.jsdelivr.net/gh/lizhangjie316/img/2020/20200727201714.png)



# 4. 存在的问题

1.  由于规则的数据表示，基于投影的方法和基于离散化的方法都可以利用其2D图像对应的成熟的网络体系结构。然而，基于投影的方法的主要局限性在于3D-2D投影造成的信息损失，而基于离散化的方法的主要瓶颈是分辨率的提高导致计算和存储开销的成倍增加。为此，建立在索引结构上的稀疏卷积将是一个可行的解决方案，值得进一步探索。
2.  基于点的网络是研究最多的方法。然而，点表示自然没有显式的邻域信息，大多数现有的基于点的方法求助于昂贵的邻域搜索机制(例如，KNN[79]或Ball  Query[54])。这固有地限制了这些方法的效率，最近提出**的点-体素联合表示法**[256]将是一个有趣的进一步研究方向。
3.  从不平衡数据中学习仍然是点云分割中的一个具有挑战性的问题。虽然有几种方法[65]、[203]、[205]取得了显著的整体表现，但它们在少数类别上的表现仍然有限。例如，RandLA-Net[206]在Semanti3D的Reduced-8子集上实现了76.0%的整体IOU，但在Hardscape类上的IOU非常低，只有41.1%。
4.  现有的大多数方法[5]、[54]、[79]、[205]、[207]适用于小的点云(例如，具有4096个点的1m×1m)。在实际应用中，深度传感器获取的点云数据通常是巨大的、大规模的。因此，需要进一步研究大规模点云的高效分割问题。
5.  一些工作[178]、[179]、[199]已经开始从动态点云中学习时空信息。期望时空信息能够帮助提高后续任务(如3D对象识别、分割和完成)的性能。



---

# 论文引用



---

# 参考文献

1. [Deep Learning for 3D Point Clouds：A Survey_20200727版](https://github.com/lizhangjie316/3D-Point-Cloud-Semantic-Segement-Paper/blob/master/papers/Deep%20Learning%20for%203D%20Point%20Clouds%EF%BC%9AA%20Survey_20200727%E7%89%88.pdf)
2. [SoTA-Point-Cloud](https://github.com/QingyongHu/SoTA-Point-Cloud)

