---
title: Tkinter简单记录
tags:
  - python
  - tkinter
url: 169.html
id: 169
categories:
  - Python是最好的语言
date: 2018-07-19 23:01:57
---

### Python Tkinter笔记

这次用了Tkinter ，特记录下

> **Tkinter：** Tkinter 模块(Tk 接口)是 Python 的标准 Tk GUI 工具包的接口 .Tk 和 Tkinter 可以在大多数的 Unix 平台下使用,同样可以应用在 Windows 和 Macintosh 系统里。Tk8.0 的后续版本可以实现本地窗口风格,并良好地运行在绝大多数平台中。

可以看到，可移植性是我选择它的前提

**布局方式：**

*   pack 包装，会从上往下分布
    
*   grid 网格，指定控件的（row，column）等参数，控制控件的分布
    
*   place 位置
    

**使用过的控件：**

*   Button 按钮
    

[http://www.runoob.com/python/python-tk-button.html](http://www.runoob.com/python/python-tk-button.html)

btn_start = Button(root, text="运行模拟器", font='微软雅黑 8 normal', fg='black', command=start_Gen).grid(row=2, sticky=W,pady=10, padx=10)
#pady距离上下，padx，距离左右，command:点击后的回调函数
btn_run = Button(root, text="模拟运行APK", font='微软雅黑 8 normal', fg='black', command=get_apk).grid(row=3, sticky=W,
 pady=10, padx=10)
btn_res = Button(root, text="获取资源文件", font='微软雅黑 8 normal', fg='black', command=getRes).grid(row=4, sticky=W,
 pady=10, padx=10)
btn_rec = Button(root, text="识别", font='微软雅黑 8 normal', fg='black', command=rec_txt).grid(row=5, sticky=W,
 pady=10, padx=10)

*   Entry 输入框
    

res_e = StringVar()
enter_res= Entry(width=50,textvariable=res_e,fg='#666666')
enter_res.grid(row=10, column=1)
enter_res\['state'\]='readonly' #三种模式：readonly,normal,disable
res_dir = './res/res.txt'
res_e.set("default:  "+res_dir)

*   Text 文本
    

state = Text(root, width=50, height=20)
state.delete(0.0,END)#设置文本框为空
state.grid(row=2, rowspan=4, column=1, padx=10, pady=10) #rowspan跨越多行，columnspan
state.insert(END,"删除完成!")#在末尾添加文本

*   Label 标签
    

label1 = Label(root, text="当前目录：", font='微软雅黑 10 normal', fg='black').grid(row=0, sticky=W, padx=10) #font 设置字体，fg设置字体颜色，sticky设置偏左，偏右，居中

*   CheckButton 复选框
    

default_btn = Checkbutton(root, text="设置",variable=default,command=default_it).grid(row=7, sticky=W, pady=5, padx=10, column=0)

*   Canvas 画布