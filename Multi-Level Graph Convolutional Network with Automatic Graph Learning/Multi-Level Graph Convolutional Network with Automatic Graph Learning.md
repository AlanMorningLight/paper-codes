# 具有自动图学习的多尺度图卷积网络用于高光谱图像分类 #
## 摘要 ##
- 缺点：1、目前基于GCN的方法将图形构建和图像分类视为两个独立的任务，这往往导致性能不理想。2、主要侧重于对图节点之间局部成对重要性的建模，而缺乏捕捉HSI全局上下文信息的能力。
- MGCN-AGL:它可以在局部和全局两级自动学习图形信息。1、通过使用注意机制来表征空间相邻区域之间的重要性，可以自适应地将最相关的信息合并到决策中，从而对空间上下文进行编码，形成局部层次的图形信息。2、利用多路径进行局部图卷积，以充分利用HSI不同空间背景的优点，并提升所产生表示的表现力。3、为了重建全局上下文关系，我们的MGCN-AGL基于在局部产生的表达对图像区域之间的长程依赖进行编码。然后沿着连接遥远区域的重构图边缘进行推理。4、对多尺度信息进行自适应融合，生成网络输出。这样，图学习和图像分类就可以集成到一个统一的框架中，并且可以互相促进。
## 引言 ##
- CNN缺点：1、CNN无法感知不同目标区域之间的几何变化，因为它的卷积核被设计成只在规则平方区域中执行。2、卷积核的权值在卷积所有HSI块时保持不变，这必然会导致类边界信息的大量丢失，从而降低特征的代表性。因此，CNN中使用的形状和权重固定的卷积核不能很好地适应HSI中的不规则结构。
- GCN优点：与CNN不同，GCN可以对图结构数据进行操作，包括社会网络数据和基于图的分子表示，并且能够跨图节点传递、转换和聚合特征信息。这样，GCN可以应用于非欧几里德数据，从而灵活地保留了不同区域的类边界。
- 早期GCN缺点：1、HSI中本来就没有图形信息，直接获取图形信息的方法是根据成对欧几里德距离事先人工构造图。然而，欧几里德距离对于揭示图数据之间的关系可能不是最佳的，因此构造的图可能是有噪声的，或者具有与标签一致性不符的边，这将最终削弱所生成表示的表达能力。2、上述方法主要集中在对局部区域之间的成对重要性进行编码，而忽略了长程相关性，因此未能考虑全局上下文。
- 提出了一种多级图卷积网络自动图学习（MGCN-AGL）方法，在统一的框架内自动学习图节点之间的局部空间重要性和全局上下文信息。为了精确地利用局部区域之间的关系，该模型在训练过程中自适应地描述具有可学习尺度系数的成对重要性。该网络可以自动学习局部空间层次上的图形信息，减少不精确的预计算图形的负面影响。因此，该模型能够集中于每个区域最相关的空间信息来进行决策。此外，通过不同空间层次的图形卷积，可以在多个空间层次上综合捕捉上下文信息。结果表明，该方法能够更好地表达具有不同物体外观的区域，从而提高特征表示的表达能力。
## RELATED WORK ##
### A. Deep-Learning-based Hyperspectral Image Classification
- 基于CNN的分类方法在HSI分类中取得了很好的效果，但它们只是将固定的卷积核应用于不同的图像区域，在复杂的情况下不可避免地会造成信息丢失，从而导致分类结果不理想。
### B. Graph Convolutional Network 
## PROPOSED METHOD ##
- 图1。算法框架。（a） 是输入的高光谱数据。（b） 表示通过over-segmenting分割原始HSI获得的区域。（c） 表示在多个空间层次上的图卷积，利用注意机制自动学习区域间的成对重要性。这里，ReLU[45]被用作激活函数。（d） 显示了全局的图卷积，其中拓扑图信息根据在局部级生成的表示自动重构。在（e）中，分类结果是通过对多层次输出的自适应积分得到的。
### A. Region-based Segmentation
- 采用了一种称为SLIC的分割技术将原始HSI分割成一组紧凑的均匀图像区域，每个区域由少量具有强烈光谱空间相关性的像素组成。具体地说，SLIC算法通过使用k-means算法迭代地增长局部簇来执行分割。分割完成后，将每个图像区域视为一个图节点，从而大大减少了图的节点数，从而加速了后续的图卷积。在这里，区域特征可以通过计算所涉及像素的平均光谱特征来获得。
### B. Automatic Graph Learning 
- 获取图的一种常用方法是预先计算图节点（即图像区域）之间的成对欧几里德距离。然而，HSI中不同类型噪声的存在会降低生成图的质量。同时，由于模型训练和图的构造是孤立的步骤，因此得到的图可能不适合后续的分类任务。为了改善这一问题，提出了从局部和全局两个层次自动地学习图形信息，并将其自然地集成到分类模型中。
- (1)
- (2)
- (3)
- 图2。局部空间水平上的图形信息学习。两个节点中的每一个都充当另一个节点的空间邻居.图2说明如何学习两个节点之间的成对重要性。通过利用注意机制，我们提出的模型可以自动地从局部空间邻域中聚集重要的特征信息。因此，与文献[19]和[13]相比，该图学习模型对高光谱数据中包含的噪声不太敏感。
### C. Integration of Multi-Level Contextual Information

## EXPERIMENTAL RESULTS