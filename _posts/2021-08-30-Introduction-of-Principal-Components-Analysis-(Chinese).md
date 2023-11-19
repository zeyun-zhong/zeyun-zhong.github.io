---
layout: post
toc:
  sidebar: left
title: Introduction of Principal Components Analysis (Chinese)
---

## 引入
维度是数据科学中的一个重要话题。维度是数据集的所有特征。例如，对于一个包含音乐作品的数据集而言，维度可以是流派、作品的长度、乐器的数量等等。我们可以把所有这些维度想象成不同的列。当只有两个维度时，绘图就非常方便：可以使用x轴和y轴。如果有几十个或几百个维度，那也是类似的，只是会更难将其可视化。

当有很多维度时，难免其中一些维度是相关的。例如，一首乐曲的流派会与乐曲中的乐器相关，等等。那么这些维度就是冗余的，因此实际上我们可以使用更少的维度来描述同一个对象。减少维度的一个方法是只保留部分维度而舍去其他维度，但是这样我们可能会失去一些好的信息。如果有一种方法可以减少这些维度，同时保留数据集中的重要信息，那就好了。没错，这时候PCA就登场了。

## 两个目的
主成分分析PCA是一种被广泛用于数据降维、有损数据压缩、特征提取等应用的技术。PCA为我们提供了一组新的维度，即**主成分**（PC）。每一个主成分都是数据集所有原特征的线性组合。这些主成分是有序的，第一主成分就是体现数据差异最大化的维度。此外，主成分之间是正交的（点积为0）。

PCA的目的有两个：
- **最大化正交投影后数据的方差**。也就是说想办法找到新的一组维度，使得在这些维度构成的空间上，样本间的差距最大。这样我们就能更好的分辨和描绘出每个独立的样本。
- **最小化重构损失**。我们虽然想要使用更少的维度来描述原数据，但是也不希望重构的数据与原数据相差甚远。

在分析这两个目的之前，我们来看一下重构这个概念。当数据投影到新维度空间时，投影数据的表示方式就是新维度空间的表示方式。这时如果我们想要计算重构损失，我们就要把投影数据转换成为原维度空间的表示方式。举个简单的例子，如下图所示，蓝点的坐标是(1,2)，投影到红轴上，投影点在红轴的表示方式是：$[1,2]\cdot[\frac{1}{\sqrt{2}},\frac{1}{\sqrt{2}}]^T = \frac{3}{\sqrt{2}}$。我们现在要计算重构损失，就要把橙点从红轴上再转换到原坐标系：$\frac{3}{\sqrt{2}}\cdot[\frac{1}{\sqrt{2}},\frac{1}{\sqrt{2}}]=(\frac{3}{2},\frac{3}{2})$。

![](/assets/images/Linear Algebra/proj_reconst2.png){:width=30% .align-center}
*图 1. 二维数据（蓝点）在新维度（红轴）上的投影（橙点）。*

现在我们来分析一下这两个目的。这两个目的看起来虽然很不相同，但是实质上是一样的。

我们先通过一张图来直观看待，如下图所示，图中的每一个点代表一个二维样本，可以看出这两个维度（x和y）是相关联的。我们可以通过投影这些点到一条穿过点云中心的线来重新描述这些数据。图中的黑线即我们要寻找的新的维度，黑线上的红点即原数据在新维度上的投影。这个新的维度是原维度x和y的线性组合。观看这张图我们会发现，当黑线转动到洋红线的时候：1）红点最为扩散（方差最大），2）平均重构损失（连结红点与蓝点的红线）最小。也就是说“最大方差”和“最小损失”是同时达到的。

![](/assets/images/Linear Algebra/PCA.gif){:width=80% .align-center}
*图 2. 二维数据（蓝点）在新维度（黑线）上的投影（红点）。*

现在我们用公式来证明一下这个观察。让我们用$\boldsymbol{X}\in R ^{m\times n}$来表示去中心化的数据，其中$m$表示样本的数量，$n$表示每一个样本的维度。去中心化意味着数据已被平移，新的数据中心是零向量，那么现在数据的协方差矩阵就是：

$$
\boldsymbol{\Sigma} = \frac{1}{m-1} (\boldsymbol{X}-\boldsymbol{\bar{X}})^T(\boldsymbol{X}-\boldsymbol{\bar{X}}) = \frac{1}{m-1} \boldsymbol{X}^T\boldsymbol{X}。\tag{1}
$$

我们用一个单位向量$\boldsymbol{w}\in R^{n\times 1}$来作为第一主成分（方便起见，我们暂时只考虑第一主成分），因为新维度是原所有维度的线性组合，所以长度为$n$。那么数据在这一向量上的投影就是$\boldsymbol{X}_{proj} = \boldsymbol{X}\boldsymbol{w}$，$\boldsymbol{X}_{proj} \in R^{m\times 1}$。根据目的一，我们想最大化投影后数据的方差，即：

$$
\begin{aligned}      \boldsymbol{w} = \underset{\boldsymbol{w}}{\mathrm{argmax}} \, \mathrm{Var}(\boldsymbol{X}_{proj}) &= \underset{\boldsymbol{w}}{\mathrm{argmax}} \, \frac{1}{m-1} \boldsymbol{w}^T\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w} \\      &= \underset{\boldsymbol{w}}{\mathrm{argmax}} \, \boldsymbol{w}^T\boldsymbol{\Sigma}\boldsymbol{w} \quad \text{subject to} \quad \boldsymbol{w}^T\boldsymbol{w}=1。  \end{aligned} \tag{2} 
$$

原数据的重构表示为$\boldsymbol{X}_{reconst} = \boldsymbol{X}_{proj}\boldsymbol{w}^T$，$\boldsymbol{X}_{reconst}\in R^{m\times n}$。根据目的二，我们要最小化重构损失，即：

$$
\begin{aligned}     \boldsymbol{w} &= \underset{\boldsymbol{w}}{\mathrm{argmin}} \, || \boldsymbol{X} - \boldsymbol{X}_{reconst} ||_F^2 \\& = \underset{\boldsymbol{w}}{\mathrm{argmin}} \, \mathrm{Tr}\big((\boldsymbol{X}-\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T)^T(\boldsymbol{X}-\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T) \big)\\     & = \underset{\boldsymbol{w}}{\mathrm{argmin}} \, \mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X} - \boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T-\boldsymbol{w}\boldsymbol{w}^T\boldsymbol{X}^T\boldsymbol{X}+\boldsymbol{w}\boldsymbol{w}^T\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T) \\     & = \underset{\boldsymbol{w}}{\mathrm{argmin}} \, \underbrace{\mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X})}_{常数项，可省略} - \underbrace{2\cdot \mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T)}_{迹运算}+ \underbrace{\mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T\boldsymbol{w}\boldsymbol{w}^T)}_{迹运算} \\     & = \underset{\boldsymbol{w}}{\mathrm{argmin}} \, -2\cdot \mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T)+ \underbrace{\mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T)}_{单位向量，\boldsymbol{w}^T\boldsymbol{w}=1} \\     & = \underset{\boldsymbol{w}}{\mathrm{argmax}} \,\mathrm{Tr}(\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}\boldsymbol{w}^T) = \underset{\boldsymbol{w}}{\mathrm{argmax}} \, \mathrm{Tr}(\boldsymbol{w}^T\boldsymbol{X}^T\boldsymbol{X}\boldsymbol{w}) \\& = \underset{\boldsymbol{w}}{\mathrm{argmax}} \,\boldsymbol{w}^T\boldsymbol{\Sigma}\boldsymbol{w}\quad \text{subject to} \quad \boldsymbol{w}^T\boldsymbol{w}=1。 \end{aligned} \tag{3} 
$$

其中$\|\|\boldsymbol{\cdot} \|\|_F$表示Frobenius范数，类似于向量的$L^2$范数。迹运算的部分请参考Deep Learning Book的[2.10](https://www.deeplearningbook.org/contents/linear_algebra.html)小节。可见，PCA的这两个目的最终达到的效果是一样的。

## 求解
现在有了公式就好办了，我们先用拉格朗日乘子法将有约束的优化问题转化成无约束的优化问题：

$$
\boldsymbol{w} = \underset{\boldsymbol{w}}{\mathrm{argmax}} \, L(\boldsymbol{w},\lambda) = \underset{\boldsymbol{w}}{\mathrm{argmax}} \, \boldsymbol{w}^T\boldsymbol{\Sigma}\boldsymbol{w} - \lambda(\boldsymbol{w}^T\boldsymbol{w}-1)。\tag{4} 
$$

对$\boldsymbol{w}$求导并置零来求得静态点（对矩阵求导的部分可参考[这里](https://zhuanlan.zhihu.com/p/24709748)）：

$$
\frac{\partial L}{\partial \boldsymbol{w}} = 2(\boldsymbol{\Sigma}\boldsymbol{w}-\lambda\boldsymbol{w}) = \boldsymbol{0}。\tag{5}
$$

这样我们就得到了特征值的定义等式：$\boldsymbol{\Sigma \boldsymbol{w}} = \lambda\boldsymbol{w}$，由此可知$\lambda$是协方差矩阵$\boldsymbol{\Sigma}$的特征值。将此式代入（4）可得，第一主成分即$\boldsymbol{\Sigma}$的最大特征值所对应的特征向量。值得一提的是，$\boldsymbol{\Sigma}$和$\boldsymbol{X}^T\boldsymbol{X}$的特征向量矩阵都是一样的，只不过特征值存在着系数的差别。因此，对$\boldsymbol{\Sigma}$和$\boldsymbol{X}^T\boldsymbol{X}$进行特征值分解都会获得同样的结果。

如果我们要求前$l$个主成分，我们只需要用$\boldsymbol{W}\in R ^{n\times l}$来代替式（2）和（3）中的单位向量$\boldsymbol{w}$即可。$\boldsymbol{W}$中列向量（主成分）互相正交，因此$\boldsymbol{W}^T\boldsymbol{W}=\boldsymbol{I}_l$。根据归纳法（详情请参考[这里](https://math.stackexchange.com/questions/2280047/how-to-prove-pca-using-induction)），前$l$个主成分就是$\boldsymbol{\Sigma}$的$l$个最大特征值所对应的特征向量。

## 示例分析
这里我们用一张假色红外图$\boldsymbol{I}$举个例子。这张红外图由三个通道组成，分别是NIR（近红外），R和G。这张图的分辨率是$255\times 607$，因此$\boldsymbol{X}$的大小是$154785\times 3$。在开始下面的操作之前，我们先将$\boldsymbol{X}$去中心化：$\boldsymbol{X} = \boldsymbol{X_{ori}} - \boldsymbol{\bar X}$。

![](/assets/images/Linear Algebra/houseigb.jpg){:width=50% .align-center}
*图 3. 假色红外图。*

通过计算得出，原图的协方差矩阵是：

$$
\boldsymbol{\Sigma} = \frac{1}{m-1} \boldsymbol{X}^T\boldsymbol{X} = \left[\begin{matrix}
    6324.4 & 3013.5 & 2952.5 \\
    3013.5 & 2916.9 & 2642.4 \\
    2952.5 & 2642.4 & 2595.2
\end{matrix}
\right]。
$$

根据[相关系数](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)公式：$\rho_{(A, B)} = \frac{\text{cov}(A, B)}{\sigma_A \sigma_B}$，可计算出NIR和R的相关系数是70.2%，NIR和G的系数是72.9%，R和G的系数是96.0%。可见，这三个维度是相互关联的。接下来我们应用PCA，对协方差矩阵进行特征分解，会得到三个特征值$\boldsymbol{\lambda}$（10107，1623，106）和相对应的三个特征向量$\boldsymbol{W}$（主成分）（$[0.74, 0.48, 0.46]^T，[0.67, -0.57, -0.48]^T，[-0.03, -0.67, 0.74]^T$）。将原图投影到特征空间（$\boldsymbol{X}\boldsymbol{W}$），可得到新的协方差矩阵：

$$
\boldsymbol{\Sigma}_{pca} = \left[\begin{matrix}
    10107 & 0 & 0 \\
    0 & 1623 & 0 \\
    0 & 0 & 106
\end{matrix}
\right]。
$$

这时候会有同学发现，这个新协方差矩阵不就是把上面那三个特征值塞进去了吗？

没错，确实是这样。因为：

$$
\begin{aligned}
    \boldsymbol{\Sigma}_{pca} &= \frac{1}{m-1} \boldsymbol{W}^T \boldsymbol{X}^T \boldsymbol{X} \boldsymbol{W} = \boldsymbol{W}^T\boldsymbol{\Sigma}\boldsymbol{W} \\
    & = \boldsymbol{W}^T\boldsymbol{\Sigma}[\boldsymbol{w}_1, \boldsymbol{w}_2, \boldsymbol{w}_3] = \boldsymbol{W}^T[\lambda_1\boldsymbol{w}_1, \lambda_2\boldsymbol{w}_2, \lambda_3\boldsymbol{w}_3] = \text{diag}(\boldsymbol{\lambda})。
\end{aligned}
$$

结合新的协方差矩阵和下图，我们可以得出下列结论：
- 经过投影之后的数据，维度之间不再相关联（协方差为0）。
- 在第一主成分上，数据的方差最大，且包含信息最多。在最后一个主成分上，数据的方差最小，同时也包含着最少的信息。

![](/assets/images/Linear Algebra/pca_example.png){:width=90% .align-center}
*图 4. 原图和投影之后的图。*

## 总结
主成分分析PCA是一种被广泛用于数据降维、有损数据压缩、特征提取等应用的技术。PCA会找到一组新的维度（主成分），这些维度之间互相正交。分析PCA，你会发现PCA总是会先：1）找到当前n维空间中数据差异最大的维度，2）将数据投影到与该维度正交的维度空间（n-1）。

这里再总结一下PCA的步骤：
- 对数据去中心化
- 计算协方差矩阵，注：这里除或不除样本数量m或m−1对求出的特征向量没影响
- 对协方差矩阵进行特征分解
- 选取特征值最大的几个维度进行数据映射，舍去较小特征值对应的维度，从而实现降维的目的

以前上课的对这一块儿一直一知半解，最近读Deep Learning Book看到这一块，再结合网上看的一些文章，总结出这篇文章，方便自己以后回顾，也希望能帮助一些初学者。

有错误欢迎指出，共同进步。


## 参考
- Deep Learning Book [第二章](https://www.deeplearningbook.org/contents/linear_algebra.html)
- https://math.stackexchange.com/questions/2314520/find-maximum-of-matrix-trace
- https://stats.stackexchange.com/questions/2691/making-sense-of-principal-component-analysis-eigenvectors-eigenvalues/2700#2700
- https://stats.stackexchange.com/questions/217995/what-is-an-intuitive-explanation-for-how-pca-turns-from-a-geometric-problem-wit
- https://stats.stackexchange.com/questions/32174/pca-objective-function-what-is-the-connection-between-maximizing-variance-and-m/136072#136072
- https://math.stackexchange.com/questions/2280047/how-to-prove-pca-using-induction
