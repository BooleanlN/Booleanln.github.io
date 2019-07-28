---
title: MNIST卷积神经网络结构
tags:
  - tensorflow
  - 机器学习，神经网络
url: 111.html
id: 111
categories:
  - Tensorflow
date: 2018-07-10 17:32:17
---

##### 卷积神经网络结构

1.  **卷积(Convolution):**
    
    一个函数（如：单位响应）在另一个函数（如：输入信号）上的**加权叠加。**
    
    [https://www.zhihu.com/question/22298352](https://www.zhihu.com/question/22298352)
    
    *   卷积函数：
        
        *   tf.nn.conv2d(input, filter, strides, padding, use\_cudnn\_on\_gpu=None, data\_format=None, name=None)
            
            input:输入图像tensor，并且shape为（batch,height,width,channels）(**训练时一个batch图像的数量，图像高度，图像宽度， 图像通道数** )
            
            filter:卷积核，使用Tensor表示，shape为（**filter\_height, filter\_width, in\_channels, out\_channels** ）**\[卷积核高度，卷积核宽度，图像通道数，卷积核个数\]**
            
            strides:步长，平滑移动的大小，长度为4的一位向量
            
            padding:"SAME"或“VALID”两种卷积方式
            
            *   SAME:外围补一圈，然后将每个像素作为卷积核中心进行卷积，最后会得到一个和原图大小相同的特征图
                
            *   VALID:只考虑卷积核中心走过的位置，最终生成的特征图会小一圈
                
            
            use\_cudnn\_on_gpu:是否使用GPU进行运算加速，默认为TRUE
            
            data_format :设置input的tensor格式
            
            name:别名
            
            返回值：生成的特征图
            
2.  **激活函数**
    
    激活函数是在神经网络的神经元上运行的函数，负责奖神经元的输入映射到输出端，常见的映射函数有Sigmoid、TanHyperbolic、Relu、softplus及softmax.
    
    **加入激活函数是用来加入非线性因素的，解决线性模型所不能解决的问题。**
    
    *   ReLU
        
    *   Sigmoid
        
    
3.  **池化（pooling）**
    
    扫描小型grid中的图像，用一个包含给定grid中最高值的单个单元替换每个grid
    
    *   池化函数
        
        *   tf.nn.max_pool(value,ksize,strides,padding,name=None):特征值取最大
            
            value：池化的输入，一般是由卷积操作后得到的特征图
            
            ksize：池化窗口的大小，
            
            strides：步长设置
            
            padding：VALID或“SAME”
            
            返回值：池化后的tensor，同input的shape
            
        *   tf.nn.mean_pooling
            
        *   tf.nn.Stochastic-pooling
            
4.  **密集连接层**
    
    用于将前边提取的特征综合起来，由于其全相连的特性，一般全链接层的参数也是最多的，方便交给最后的分类器或者回归。
    
    *   reshape()
        
5.  **Dropout**
    
    为了减少过拟合，在输出层前加入dropout，Dropout即在训练过程中随机扔掉一部分神经元
    
    *   tf.nn.dropout(x, keep\_prob, noise\_shape=None, seed=None,name=None)
        
        x：输入
        
        keep_prob：设置神经元被选中的概率，是一个占位符（0，1\]
        
        noise_shape：干扰shape
        
        seed
        
        返回值：tensor
        
        训练过程中启用droout，测试过程中，关闭dropout
        
6.  输出层
    
7.  训练和评估
    
    *   交叉熵评估，真实值及实际值的偏差
        
    
    *   优化器：ADAM优化器，最小化交叉熵
        
    *   accuracy.eval()：f.Tensor.eval(feed_dict=None, session=None)
        
        feed_dict:参数字典
        
8.  代码：
    
    from \_\_future\_\_ import print_function
    import tensor.down as input_data
    import tensorflow as tf
    mnist = input\_data.read\_data\_sets(".", False, one\_hot=True)
    sess = tf.InteractiveSession()
    def weight_variable(shape):
    initial = tf.truncated_normal(shape,stddev=0.1)
    return tf.Variable(initial_value=initial)
    def bias_variable(shape):
    initial = tf.constant(0.1,shape=shape)
    return tf.Variable(initial_value=initial)
    def conv2d(x,W):
    return tf.nn.conv2d(x,W,strides=\[1,1,1,1\],padding='SAME')
    def max\_pool\_2x2(x):
    return tf.nn.max_pool(x, ksize=\[1,2,2,1\],
    strides=\[1,2,2,1\],padding='SAME')
    #数据
    x = tf.placeholder("float", \[None, 784\])
    y_ = tf.placeholder("float", \[None, 10\])
    #变量
    W = tf.Variable(tf.zeros(\[784,10\]))
    b = tf.Variable(tf.zeros(\[10\]))
    
    y = tf.nn.softmax(tf.matmul(x,W)+b)
    #第一层卷积
    w\_conv1 = weight\_variable(\[5,5,1,32\]) #卷积核为5x5，在每个patch上算出32个特征，，前两个维度是patch的大小，然后是通道数目，最后是输出的通道数目
    b\_conv1 = bias\_variable(\[32\])#偏置量
    
    x_image = tf.reshape(x, \[-1,28,28,1\]) #使用卷积层需要将转换为一个4维向量，第一位是批次数，2，3维对应图片宽高，最后一个对应颜色通道数
    #tf.nn.conv2d(input, filter, strides, padding, use\_cudnn\_on\_gpu=None,data\_format=None, name=None)
    h\_conv1 = tf.nn.relu(conv2d(x\_image, w\_conv1)+b\_conv1) #卷积relu函数激活
    h\_pool1 = max\_pool\_2x2(h\_conv1)
    
    #第二层卷积
    w\_conv2 = weight\_variable(\[5,5,32,64\])
    b\_conv2 = weight\_variable(\[64\])
    
    h\_conv2 = tf.nn.relu(conv2d(h\_pool1, w\_conv2)+b\_conv2)
    h\_pool2 = max\_pool\_2x2(h\_conv2)
    
    #密集链接层
    #用于将前边提取的特征综合起来，由于其全相连的特性，一般全链接层的参数也是最多的，方便交给最后的分类器或者回归。
    w\_fc1 = weight\_variable(\[7\*7\*64, 1024\]) #7*7的尺寸，64个特征的图片，使用一个1024个神经元的全连接层
    b\_fc1 = bias\_variable(\[1024\])#偏置
    
    h\_pool2\_flat = tf.reshape(h_pool2, \[-1,7\*7\*64\])#由原来的7x7x64x1的四维向量转换为列为7\*7\*64的tensor
    h\_fc1 = tf.nn.relu(tf.matmul(h\_pool2\_flat, w\_fc1)+ b_fc1) #reLU进行运算
    #减少过拟合，执行dropout
    keep_prob = tf.placeholder("float")
    h\_fc1\_drop = tf.nn.dropout(h\_fc1,keep\_prob=keep_prob)
    #输出层softmax
    W\_fc2 = weight\_variable(\[1024, 10\])
    b\_fc2 = bias\_variable(\[10\])
    
    y\_conv = tf.nn.softmax(tf.matmul(h\_fc1\_drop, W\_fc2) + b_fc2)
    #训练评估
    cross\_entropy = -tf.reduce\_sum(y_*tf.log(y_conv)) #交叉熵
    train\_step = tf.train.AdamOptimizer(1e-4).minimize(cross\_entropy)#ADAM优化器，调整模型
    correct\_prediction = tf.equal(tf.argmax(y\_conv,1), tf.argmax(y_,1))
    accuracy = tf.reduce\_mean(tf.cast(correct\_prediction, "float"))
    sess.run(tf.global\_variables\_initializer())
    for i in range(20000):
    batch = mnist.train.next_batch(50)
    if i%100 == 0:
    train\_accuracy = accuracy.eval(feed\_dict={
    x: batch\[0\], y_: batch\[1\], keep_prob: 1.0}) #测试不使用dropout
    print("step %d, training accuracy %g" % (i, train_accuracy))
    train\_step.run(feed\_dict={x: batch\[0\], y_: batch\[1\], keep_prob: 0.5})#训练
    
    print("test accuracy %g" % accuracy.eval(feed_dict={
    x: mnist.test.images, y_: mnist.test.labels, keep_prob: 1.0}))