---
title: MNIST模型搭建
tags:
  - tensorflow
  - 机器学习，神经网络
url: 107.html
id: 107
categories:
  - Tensorflow
date: 2018-07-10 00:17:02
---

##### MNIST模型搭建

1.  Mnist
    
    mnist（Mixed National Institute of Standards and Technology database ），是一系列带标记的数字图片，原始的Minst数据集的训练集是美国人口普查局的雇员的笔迹，而验证集是采集于美国中学生的笔迹。
    
    Mnist的图片一般是28*28的图片，每个像素都进行了归一化，使得其都在0-1范围内
    
2.  模型选择：
    
    入门案例中，没有选择图像识别最常用的卷积神经网络，而是采用了全连接网络+Softmax输出层
    
    对图片784个像素值进行加权求和，然后加点偏置，衡量得出的结果，与0-9作比较，用不同的权值加权10次，得到10个不同的结果，最后使用softmax输出，最后的10个输出都会有一个概率值。
    
    这里的softmax可以看成是一个激励函数或者链接函数。
    
3.  模型评估（模型训练）：
    
    这个案例中，如果使用均分误差法进行评估，归约速度太慢，所以使用“交叉熵”来进行评估
    
    y是我们预测的概率分布，y‘是实际的分布
    
4.  优化
    
    使用GradientDescentOptimizer(0.01) 步长为0.01的梯度下降算法来最小化交叉熵
    
5.  开始训练
    
6.  模型评估
    
    代码：
    
    from \_\_future\_\_ import print_function
    import tensor.down as input_data
    import tensorflow as tf
    import  numpy as np
    #数据读取
    mnist = input_data.read\_data\_sets(".", False, one_hot=True)
    #常量声明
    sess = tf.InteractiveSession()
    x = tf.placeholder("float",shape=\[None,784\])
    y_ = tf.placeholder("float",shape=\[None,10\])
    #权值、偏置变量声明
    W = tf.Variable(tf.zeros(\[784,10\]))
    b = tf.Variable(tf.zeros(\[10\]))
    #运行session，初始化变量
    sess.run(tf.initialize\_all\_variables())
    #模型搭建
    y = tf.nn.softmax(tf.matmul(x,W)+b)
    cross_entropy = -tf.reduce_sum(y_*tf.log(y)) #交叉熵
    #优化，梯度下降
    train_step = tf.train.GradientDescentOptimizer(0.01).minimize(cross_entropy)
    #随机训练
    for i in range(1000):
     batch = mnist.train.next_batch(50)
     train_step.run(feed_dict={x:batch\[0\], y_:batch\[1\]})
    #模型评估    
    correct_prediction = tf.equal(tf.argmax(y,1),tf.argmax(y_,1)) #判断位置是否一样
    accuracy = tf.reduce_mean(tf.cast(correct_prediction,"float"))#bool转换为float
    print(accuracy.eval(feed_dict={x: mnist.test.images, y_: mnist.test.labels}))
    sess.close()