---
title: k-近邻算法
date: 2019-04-22 16:54:57
tags: 
- 机器学习
---

### kNN算法

k-近邻算法，也称kNN算法，该算法通过测量不同特征值之间的距离方法进行分类

优点：

- 精度高、对异常值不敏感，无数据输入假定

缺点：

- 计算复杂度高，空间复杂度高

使用数据范围：数值型和标称型

*标称型：一般在有限的数据中取，而且只存在‘是’和‘否’两种不同的结果（一般用于分类）*

*数值型：可以在无限的数据中取，而且数值比较具体化，例如4.02,6.23这种值（一般用于回归分析）*

算法思想：假设我们当前存在一个数据样本集合，且数据集当中每个数据都存在标签，即我们知道样本集中每一数据与所属分类的对应关系。当输入没有标签的新数据时，可以通过将新数据与样本集中数据特征进行比对，然后算法提取前k个最相似的数据，然后选择k个数据中分类出现次数最多的标签作为新数据的预测分类。

<!--more-->

#### 简单k-近邻算法实例

##### 电影分类

1. 创建数据作为样本数据

   ```python
   import numpy as np
   import operator
   
   def createDataSet():
       group = np.array([[1,101],[5,89],[108,5],[115,8]])
       labels = ['爱情片','爱情片','动作片','动作片']
       return group,labels
   ```

2. k-近邻算法

   ```python
   def classify0(inX,dataSet,labels,k):
       #计算距离，采用欧式距离公式
       dataSetSize = dataSet.shape[0]
       diffMat = np.tile(inX,(dataSetSize,1)) - dataSet 
       sqDiffMat = diffMat**2
       sqDistances = sqDiffMat.sum(axis=1)
       distances = sqDistances ** 0.5
       #根据样本之间的距离进行从小到大排序
       sortedDistIndicies = distances.argsort()
       classCount={}
       for i in range(k):
           voteIlabel = labels[sortedDistIndicies[i]]
           classCount[voteIlabel] = classCount.get(voteIlabel,0)+1
       #对标签根据出现次数进行从大到小排序
       sortedClassCount = sorted(classCount.items(),key=operator.itemgetter(1),reverse=True)
       return sortedClassCount[0][0]
   ```

   