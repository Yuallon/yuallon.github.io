
### 前言

第一次接触**PCA（Principal Component Analysis）**，在网上也搜索了很多资料。但是这些资料都有着很明显的特点：**数学推导没问题，不能从根本上解释PAC思想**。了解PCA的同学可能会说：PCA不就是找到关心的主成分，从而达到降维的目的。可是是否有人问过：为什么我们选出的主成分能代表大部分的数据信息？即，**下图中为什么PC1代表了大部分的数据信息？**

![主成分分析图1](/Users/huangyulong/myblog/static/主成分分析图1.jpg)

图1: 对于2D数据，主成分PC1代表了大部分的数据信息。

对于这个问题很少有文章去讨论，如果按照他们的文章内容来讲，因为有一个函数S在红线处有极值，更不严谨的可以说，因为数据点在红线处更分散。

然而，上面的回答要么是本末倒置（极值），要么是不严谨（数据点分散）。都不能使我满足。下面我尝试着用自己的理解，来从根本上解释为什么我们要找红线？如何找？

### 主成分分析

主成分分析（PCA）是最古老、最有名的多元分析技术之一。在1901年第一次被*Person*提出，随后在1933年被*Hotelling*独立发展并命名。跟其他多元分析方法一样，随着电子计算机的出现，PCA技术被广泛的使用。

**主成分分析的中心思想：通过减少数据集（数据集中包含大量的相关变量）的维度，并尽可能多的保留数据集中存在的差异。**-- 来自*Principal Component Analysis -- Second Edition, Author: I.T.Jolliffe*

那我们会问：

- **什么是数据集维度？**
- **什么是数据集中的差异？**
- **如何减少维度，并保留数据主要特征？**
- 等等

#### 数据的维度

简单来讲就是数据集包含变量的个数。

比如，对一个班级的学生我们想收集他们的**年龄**、**身高**、**体重**3个指标的信息。对于收集到的任一一个学生数据而言，里面包含了3个变量（年龄，身高，体重）。所以我们收集到的数据是3维数据。

所以，当描述一组数据是几维的时候，我们只需要看数据集中单个对象（任意一个学生）需要几个参数来描述，则就是几维数据。

#### 数据集中的差异

**数据集中的差异其实就是数据集所包含的信息**。如身高信息、体重信息等。

那我想问：什么是数据信息？如何呈现数据信息？

A学生的数据信息{年龄：18岁，身高：170cm，体重：55kg}，这是一个学生的数据信息。那该如何呈现这个学生的信息呢？很自然的想法是画在坐标系里面，坐标系0-XYZ刚好可以用来呈现每一个学生的信息。

难道这样就算完了吗？我们已经描述了数据信息，也呈现出来，还有什么可以挖掘的地方吗？我们再回头看一下主成分分析的思想，里面有一句 **尽可能多的保留数据集中的差异**，也就是说 **尽可能多的保留数据集中所包含的信息**。这句话到底是什么意思？

- **1D数据**

对于学生的数据而言，现在我们只关心 **年龄** 这个变量的信息。现在我们的数据维度为1D。所以我们可以把所有的学生信息画在一个1D的坐标系里面来呈现，可以画在一条直线上，见下图中的红线。



Figure1: 对于1D数据，尽可能多的保留信息。

对于每一个学生的年龄信息，都可以 **从原点引一条向量，模为年龄的大小，如向量$\overrightarrow{u}$ 表示学生A的年龄信息**。现在我们可以这样认为：**如果知道了向量$\overrightarrow{u}$，并且知道了其模的大小，则一定就能完全知道学生A的年龄信息**。

现在有小朋友说，我关注的向量不是$\overrightarrow{u}$，而是$\overrightarrow{v}$（不与向量$\overrightarrow{u}$平行）。那我可以得到学生A的信息吗？等到信息是否是完整的？

当然我们可以选取另一个向量$\overrightarrow{v}$，然后将向量$\overrightarrow{u}$投影到向量$\overrightarrow{v}$上为A1，则向量$\overrightarrow{OA_1}$就是我们在向量$\overrightarrow{v}$上的得到向量$\overrightarrow{u}$的信息。向量$\overrightarrow{A_1A}$ 我称之为丢失的信息。见上图中蓝色的部分。

**关于信息的解释：我们可以这样理解，如果我们知道参考点（原点），并给出数据所在的坐标系（$\overrightarrow{u}$），则在$\overrightarrow{u}$方向上，可以获得数据A的全部信息（$\overrightarrow{u}$的模=A学生的年龄大小）。如果在另一个参坐标系下（$\overrightarrow{v}$），通过投影也可以获得A点的近似值（$\overrightarrow{O_1A}$的模 < A学生的年龄大小），但是也会丢失一部分数据信息（$\overrightarrow{A_1A}$的模）**。在1D情况下 $\theta$（向量$\overrightarrow{u}$和向量$\overrightarrow{v}$的夹角），丢失的数据信息就越少。 

**这里我要强调一下**：对于1D数据我们是用不到降维操作的，但是我想通过上图来解释，我们可以通过不同的角度来看待数据，如$\overrightarrow{u}$、$\overrightarrow{v}$两个向量就代表着看待数据的方式。不同的角度去看待数据，势必存在数据信息的丢失问题，而这一点正是在高维数据中PCA的操作最基本的思想所在。

- **2D数据**

下面看一下2D情况，假设有3个学生的身高、体重信息，见下图。根据一般常识我们知道，身高较高的体重一般也比较重。所以身高和体重之间有一定的相关性。那是否可以用一个变量特征来描述身高和体重的关系，并且可以保留最多的信息呢（这也就是数据降维的思想所在）？



记住，坐标系只是我们用来分析数据的工具。如上图中的坐标系*O-xy*，在此坐标系下，我们可以得到3个学生的全部信息（$\overrightarrow{OA}$, $\overrightarrow{OB}$, $\overrightarrow{OC}$），因为通过向量在该坐标系上的正交分解，我们可以给出3个学生准确的身高、体重信息。同样，我们可以在坐标系*O'-x'y'*中来看这3个数据，在新的坐标系中，我们也可以完全给出3个学生的信息（$\overrightarrow{OA}=\overrightarrow{OO'}+\overrightarrow{O'A}$，向量$\overrightarrow{OO'}$表示两个坐标系的转换关系）。然而这依旧是2D的。如何实现降维呢？还记得前面提到的投影吗？如果我们在坐标系中可以找到一个直线，使得投影的模值最大，则我们保留的信息最多，就可以用投影点的信息来代表原始的数据点，丢失的数据信息最少。

如何找这条直线呢？

- **无偏化**

先放下如何找这条直线，我们来回顾一下《概率与数理统计》课程中，一个很基本的概念：**均值**。我这里标题采用的是 *无偏化* 其实是自己杜撰的一个词，我是由 **均值是期望值的无偏估计** 而相出的一个词。关于为什么均值是期望值的无偏估计，我不在这里累述，可以自己查阅资料或者看[马同学关于无偏估计的回答](https://www.zhihu.com/question/22983179)。

如何直观地去理解无偏估计呢？可以对比物理学中 **质心** 的概念，这让我对均值有了直观的认知。

质量中心（质心）是一个系统所有部分根据质量进行加权的平均位置。**对于密度均匀的刚性物体，质心位于其几何中心**。计算公式如下：

$r_c=\frac{m_1\overrightarrow{r_1}+m_2\overrightarrow{r_2}+...+m_n\overrightarrow{r_n}}{m_1+m_2+...+m_n}=\frac{\sum{m_i\overrightarrow{r_i}}}{M}$

如果密度均匀，则几何中心坐标为：$\overline{x}=\frac{\sum{x_i}}{n}$，$\overline{y}=\frac{\sum{y_i}}{n}$。

如果选择坐标系中心为质心位置，则$x=0, y=0, \sum{\overrightarrow{r_i}}=0$。所以有：$\overrightarrow{r_m}=-(\overrightarrow{r_1}+\overrightarrow{r_{m-1}}+\overrightarrow{r_{m+1}}+\overrightarrow{r_n})$，**这意味着如果我们以质心位置来看系统上的任意一质点，它都是无偏的，即它和余下的质点的权重和相等。**

现在我们回到数据分析上，如果获得身高、体重信息每一个的权重都一样，画在直角坐标系如下：



类比上述质心公式，这些数据的均值点为：$\overline{x}=\frac{\sum{x_i}}{n}$，$\overline{y}=\frac{\sum{y_i}}{n}$。**如果我们以均值点重新看这些数据，无论看那个数据点都是无偏的。**这也就是为啥，均值是数据期望值的无偏估计的原因。

#### 如何降维

前面说了那么多，主要阐述的想法是：如何以向量的方式理解数据，如何在不同坐标系下看待数据。有了这些概念，接下来我们变可以对数据实现降维操作，即主成分提取。

我们仍以学生的身高、体重信息为例。画在2D坐标如下：



- **坐标变换**
  我们知道，坐标系只是用来展示数据的工具。上图虽然能很好的展示获取到的学生身高、体重的准确信息，但是不利于我们对数据的分析（坐标原点不在均值点。）我们还知道，如果以均值点来看待获取到的数据，数据是无偏的。为了方便后续的数据分析，以均值点为数据的参考点。即坐标系由 **O-XY --> O'-X'Y'**。

- **数据降维**
  回顾一下前面介绍的 **向量=数据信息**。实现数据降维的主要的要求是：如何最多的保留数据信息，即 **如何找到一个向量$\overrightarrow{u}$ 使得所有数据点的向量投影之和最大？**

  **向量投影对应着数据的什么呢？**投影之和：****

- **数学实现**
  前面两步已经给出了数据降维的主要思想。下面就是如何利用数学这个工具实现而已。

  1. 找到数据均值点：$\overline{x}=\frac{\sum{x_i}}{n}$，$\overline{y}=\frac{\sum{y_i}}{n}$。并以均值点为原点，构建新的坐标系 $S'$。
  2. 在 $S'$ 坐标系下，找到一个向量$\overrightarrow{u}$，使得所有数据的投影值之和最大。

- 





