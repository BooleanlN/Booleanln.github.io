---
title: keras_ocr代码——（densenet-ocr.ipynb）
tags:
  - keras
  - ocr
  - 机器学习，神经网络
url: 159.html
id: 159
categories:
  - keras
date: 2018-07-16 21:52:50
---

**getsession(gpu_fraction=0.6)**

*   os.environ.get('OMP\_NUM\_THREADS') 获取线程
    
*   tf.GPUOptions(per\_process\_gpu\_memory\_fraction=gpu_fraction)
    
    tf.ConfigProto(gpu\_options=gpu\_options)
    
    tf.Session(config=tf.ConfigProto(gpu\_options=gpu\_options))
    
    K.set\_session(get\_session())
    
    设置每个gpu应该拿出多少容量给进程使用
    
*   from keras import backend as K （K代表keras后端，可用于作为占位）
    

**ctc\_lambda\_func(args)**

*   K.ctc\_batch\_cost(labels, y\_pred, input\_length, label_length)在batch上运行CTC损失算法
    
    `ctc_batch_cost(y_true（真值）, y_pred（预测值）, input_length（y_pred每个batch序列长）, label_length（y_true每个序列长）)`
    
    返回值：形如(samoles，1)的tensor，包含了每个元素的CTC损失
    
    [CTC算法介绍](https://blog.csdn.net/luodongri/article/details/77005948)
    

**class random\_uniform\_num()**

*   `__init__()`
    
    初始化total、range以及顺序数字组并打乱，设置index=0
    
*   `get()`
    
    获取batchsize大小的乱序数组
    

**readtrainfile(filename)**

读取训练文件

**gen3(trainfile,batchsize=64,maxlabellength=10,imagesize=(32,280)):**

文件读取，构造输入，输出，(input,target)型迭代器：

*   常变量声明：

    #设置shape(64,32,282,1)
    #因为只用了一个通道，恐怕只能识别在灰度下，字体和底色不同的图片
    #是否可以改成tensor专用的？
    x = np.zeros((batchsize, imagesize\[0\], imagesize\[1\], 1), dtype=np.float)
    labels = np.ones(\[batchsize,maxlabellength\])*10000
    input_length = np.zeros(\[batchsize,1\])
    label_length = np.zeros(\[batchsize,1\])
    r\_n = random\_uniform\_num(len(\_imagefile))#初始化一个random\_uniform\_num类，一个随机数组
    \_imagefile = np.array(\_imagefile)

*   *      文件读取，构造输入，输出，(input,target)型迭代器

 \_imagefile = np.array(\_imagefile)
    #读取文件
    while 1:
        #读取文件
        shufimagefile = \_imagefile\[r\_n.get(batchsize)\]
        for i,j in enumerate(shufimagefile):
            #灰度处理先
            #L = R * 299/1000 + G * 587/1000+ B * 114/1000
            img1 = Image.open(j).convert('L')
            #Column-major order   获得一个img数组。全0，shape与img1相同，列主序
            #（32，280）
            img = np.array(img1,'f')/255.0-0.5
            #reshape()（32，280，1）
            x\[i\] = np.expand_dims(img,axis=2)
            #print('imag:shape',img.shape)
            #取lable
            str = image_label\[j\]
            label_length\[i\] = len(str) 
            
            if(len(str)<=0):
                print("len<0",j)
            #//取整除 \- 返回商的整数部分（向下取整）
            input_length\[i\] = imagesize\[1\]//8
            #caffe_ocr中把0作为blank，但是tf 的CTC  the last class is reserved to the blank label.
            #https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/util/ctc/ctc\_loss\_calculator.h
         #构建输入输出迭代器
            #获取label
            labels\[i,:len(str)\] =\[int(i)-1 for i in str\]
            print("label:"+labels)

        #输入
        inputs = {'the_input': x,
                'the_labels': labels,
                'input\_length': input\_length,
                'label\_length': label\_length,
                }
        outputs = {'ctc': np.zeros(\[batchsize\])} 
       # yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，
        yield (inputs,outputs)
#调用函数，真正构建
print('-----------beginfit--')
#读取文件，构建迭代器
cc1=gen3(r'E:\\C4\\keras\_ocr\\images\\train.txt',batchsize=batch\_size,maxlabellength=maxlabellength,imagesize=(img\_h,img\_w))
cc2=gen3(r'E:\\C4\\keras\_ocr\\images\\test.txt',batchsize=batch\_size,maxlabellength=maxlabellength,imagesize=(img\_h,img\_w))

**model构建**

                
#输入层，返回一个tensor，占位
input = Input(shape=(img\_h,None,1),name='the\_input')
#使用desenet获得预测结果
y\_pred= densenet.dense\_cnn(input,nclass)
#模型图构建
basemodel = Model(inputs=input,outputs=y_pred)
#打印出模型概述信息。 它是 utils.print_summary 的简捷调用。
basemodel.summary()
labels = Input(name='the_labels',shape=\[maxlabellength\],dtype='float32')
input\_length = Input(name='input\_length', shape=\[1\], dtype='int64')
label\_length = Input(name='label\_length', shape=\[1\], dtype='int64')

#Lambda用以对上一层的输出施以任何Theano/TensorFlow表达式
#keras.layers.core.Lambda(function, output_shape=None, mask=None, arguments=None)
loss\_out = Lambda(ctc\_lambda\_func, output\_shape=(1,), name='ctc')(\[y\_pred, labels, input\_length, label_length\]) 
#构建模型图
model = Model(inputs=\[input, labels, input\_length, label\_length\], outputs=loss_out)
#优化器
adam = Adam()
#模型编译
model.compile(loss={'ctc': lambda y\_true, y\_pred: y_pred}, optimizer=adam,metrics=\['accuracy'\])

**回调函数声明**

#keras.callbacks.ModelCheckpoint(filepath, monitor='val\_loss', verbose=0, save\_best\_only=False, save\_weights_only=False, mode='auto', period=1)
checkpoint = ModelCheckpoint(r'E:\\deeplearn\\OCR\\Sample\\model\\weights-densent-{epoch:02d}.hdf5',
                           save\_weights\_only=True) #检查点记录
#keras.callbacks.EarlyStopping(monitor='val_loss', patience=0, verbose=0, mode='auto')
#当监测值不再改善时，该回调函数将中止训练
earlystop = EarlyStopping(patience=10)#提前终止
#tensorboard
tensorboard = TensorBoard(r'E:\\deeplearn\\OCR\\Sample\\model\\tflog-densent',write_graph=True)

**训练**

#
res = model.fit_generator(
                    #生成器函数，或一个输入tuple
                    cc1,
                          #当生成器返回steps\_per\_epoch次数据时计一个epoch结束，执行下一个epoch
                    steps\_per\_epoch =3279601// batch_size,
                    epochs = 100,
                    validation_data =cc2 ,
                    validation\_steps = 364400// batch\_size,
    #回调函数
                    callbacks =\[earlystop,checkpoint,tensorboard\],
    #日志显示，0为不在标准输出流输出日志信息，1为输出进度条记录，2为每个epoch输出一行记录
                    verbose=1
                    )