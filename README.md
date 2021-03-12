<h3 align = "center">进度汇报</h3>

<h4 align = "center">朱大地</h4>

*************

#### 11.24 - 12.2 攻击NeuMF

**本周工作**

（1）跑通NeuMF源代码，理解其输入输出。

（2）将生成的假文件处理成为NeuMF模型的训练集格式，并写入到poison.txt中。

**讨论**

（1）将生成的假文件与原训练集合并后输入到推荐模型中进行训练。

（2）不要把生成的假文件写入到磁盘中，因为涉及到I/O操作，导致运行很慢。直接将数据作为变量输入到模型中即可。

（3）为什么不需要修改原用户的商品交互，只通过注入假用户&假商品交互即可改变原用户的推荐结果，这就跟推荐系统的原理机制有关了，原因之一是协同过滤，推荐系统会大部分人喜欢的商品给用户。

*********

#### 12.2 - 12.8 攻击NeuMF

**本周工作**

攻击NeuMF，数据集为ml-1m

**实验**

* 原来的NeuMF：

训练集：

#user：6040    #item：3706

评估：100个商品，其中1个为测试集的商品，通过推荐模型对这100个商品进行排序，得到长度为10个商品的ranklist，如果测试集的商品出现在ranklist中，则记为1，否则为0，最后生成一个长度为6040（即user个数）的由0、1组成的列表，即可计算命中率。

* 受到假文件注入的NeuMF

训练集：

#user：6060（20 attacker）    #item：3714 （8 target items）

评估：根据ICDE论文的做法：从全体商品中随机抽出92个商品，加上8个目标商品，组成候选集，通过推荐模型对这100个商品进行排序，得到长度为10个商品的ranklist，通过`len(set(ranklist) & set(target_items))`计算命中数。

**结果**

命中数为几十几十，并没有达到原论文中两千多的结果。。

**讨论**

老师：论文有点问题。

问题一：8个目标商品在原来的推荐系统中是不存在的。而我们做攻击，一般是让推荐系统内已有的商品发生变化。包括AUSH在内的大部分论文，都是攻击推荐系统内已有的商品，因为比较容易影响在推荐系统内已经有很多交互的商品。

问题二：这8个目标商品还只跟20个攻击者有交互，但是攻击效果却这么好，实在难以理解。可能的解释是：卖家想要让新来的商品提高曝光度。除此之外，该攻击还很容易被识别，因为假用户有大量的跟目标商品的交互。

论文中的疑似回答：*Promoting target items is a common attack objective in a real-world recommender system, after which the target items may be recommended to users more frequently than before. For example, Yang et al. [12] have successfully performed the item promotion attack on several real-world popular services like Youtube and eBay where the co-visitation recommender system [12] is deployed. It is profitable for attackers, ==and we mainly consider such an item promotion problem in this work==.*

**下周任务**

攻击PMF或者BPR，看看其效果怎么样

**********

#### 12.8 - 12.15 攻击PMF

**背景知识**

电影评分预测实际上是一个==矩阵补全==的过程，在矩阵分解的时候原来的大矩阵必然是稀疏的，即有一部分有评分，有一部分是没有评过分的，不然也就没必要预测和推荐了，所以整个预测模型的最终目的是得到两个小矩阵，通过这两个小矩阵的乘积来补全大矩阵中没有评分的位置。所以对于机器学习模型来说，问题转化成了==如何获得两个最优的小矩阵==。因为大矩阵有一部分是有评分的，那么只要保证大矩阵有评分的位置（实际值）与两个小矩阵相乘得到的相应位置的评分（预测值）之间的误差最小即可，其实就是一个均方误差损失RMSE，这便是模型的目标函数。

**实验过程中的问题**

1. attacker对item的评分应该如何设置？

   > 之前攻击GRU4Rec、NeuMF推荐系统都没有涉及到评分问题，只需将假的user-item交互注入即可，而论文中给的PMF代码在训练过程中需要通过计算预测评分和实际评分之间的误差来优化参数，所以生成的假文件就必须带有评分，而ICDE论文中生成假文件时并没有生成评分。所以我想==是否可以人为设置==？并产生以下两个问题。

   （1）attacker对推荐系统中原来的item的评分要怎么设置？

   （2）attacker对目标商品应该怎样评分？是否应该打高分？

学姐：PMF不一定要评分的，也可以是0/1，你不用去攻击评分型-PMF，你可以将其改为隐反馈类型的，只需将评分全部改成1即可。

**实验尝试**

评分设置：对假文件中每个商品都进行==随机评分==。

实验步骤：

1、模型经过训练、生成了两个“最优”的小矩阵，一个为商品矩阵，一个为用户矩阵，将商品矩阵中的某一行与用户矩阵中的某一行做点积运算即可得出该用户对于该商品的评分预测。

2、从原来的全部商品中抽出92个，与8个目标商品组成100个商品的候选集，利用前面的评分预测对其进行排序，得到Top10商品，计算Top10商品中目标商品的出现次数，进行累加得到命中数。

3、按照论文的设置，跑500个epoch，每个epoch攻击16次，推荐系统训练10轮，对16次的攻击结果hit_num取平均作为这个epoch的平均命中数。

![image-20201215093030456](C:\Users\95174\AppData\Roaming\Typora\typora-user-images\image-20201215093030456.png)

![image-20201215100236951](C:\Users\95174\AppData\Roaming\Typora\typora-user-images\image-20201215100236951.png)

实验结果：比论文中的**7050**高出了不少。。或许有点问题

![image-20201215100035283](C:\Users\95174\AppData\Roaming\Typora\typora-user-images\image-20201215100035283.png)

**实验问题**

1、随着攻击次数的增加，PMF模型的初始RMSE和结束训练时的RMSE都会缓慢升高

![image-20201215102528711](C:\Users\95174\AppData\Roaming\Typora\typora-user-images\image-20201215102528711.png)
