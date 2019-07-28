---
title: Tkinter窗口使用
tags:
  - python
  - tkinter
  - ui
url: 165.html
id: 165
categories:
  - Python是最好的语言
date: 2018-07-19 00:17:50
---

#coding=utf-8
from tkinter import *
from tkinter import filedialog
from Emulator import *
import os
#apk按钮事件
def get_apk():
    in_file = filedialog.askopenfilename()
    if Has_device():
        if Install\_apk(in\_file):
            pkname = getPackageName(in_file)
            Activities = getActivities(packagename=pkname, apk\_Name=in\_file)
            text.insert(END, "packagename：" + pkname + "\\n")
            pwd = os.getcwd()
            screen\_dir = pwd + "\\screen\_shot"
            tmp\_dir = screen\_shot(screen\_dir, apk\_name=in_file,Activities=Activities,packagename=pkname)
            text.insert(END,"检索Activity："+"\\n")
            for activity in Activities:
                text.insert(END, activity+"\\n")
            text.insert(END, "获取运行图片...")
        else:
            text.insert(END, "安装失败！")
    else:
        text.insert(END,"无设备连接！")
#运行模拟器事件
def start_Gen():
    os.system('D:\\Applications\\Genymotion2\\genymotion.exe')
#初始化界面
def init(root):
    #滑轮
    S = Scrollbar(root)
    #grid布局
    label1 = Label(root,text="当前目录：",font='微软雅黑 10 normal',fg='black').grid(row=0,sticky=W,padx=10)
    pwd = Label(root,text=os.getcwd(),font='微软雅黑 8 normal',fg='#666666').grid(row=0,column=1,sticky=W)
    
    label2 = Label(root,text="菜单",font='微软雅黑 10 normal',fg='black').grid(row=1,sticky=W,pady=5,padx=10)
    
    btn_start = Button(root,text="运行模拟器",font='微软雅黑 8 normal',fg='black').grid(row=2,sticky=W,pady=10,padx=10)
    btn_run = Button(root,text="模拟运行APK",font='微软雅黑 8 normal',fg='black').grid(row=3,sticky=W,pady=10,padx=10)
    btn_res = Button(root, text="获取资源文件", font='微软雅黑 8 normal', fg='black').grid(row=4, sticky=W, pady=10, padx=10)
   
    state = Text(root,width=50,height=20)
    state.grid(row=2,rowspan=3,column=1,padx=10,pady=10)
    state.config(yscrollcommand=S.set)
    
    label3= Label(root, text="参数设置", font='微软雅黑 10 normal', fg='black').grid(row=5, sticky=W, pady=5, padx=10)
    default_btn = Checkbutton(root,text="默认").grid(row=6 ,sticky=W, pady=5, padx=10,column=0)
    set_btn = Checkbutton(root,text="设置").grid(row=6 ,sticky=W, pady=5, padx=10,column=1)
    
    prefix = Label(root, text="资源文件保存位置：", font='微软雅黑 8 normal', fg='black').grid(row=7, sticky=W, pady=5, padx=10,column=0)
    enter_file = Entry(width=50).grid(row=7,column=1)
    
    log = Label(root, text="日志文件保存位置：", font='微软雅黑 8 normal', fg='black').grid(row=8, sticky=W, pady=5, padx=10,column=0)
    enter_log = Entry(width=50).grid(row=8, column=1)
    
    res = Label(root, text="结果文件保存位置：", font='微软雅黑 8 normal', fg='black').grid(row=9, sticky=W, pady=5, padx=10,column=0)
    enter_log = Entry(width=50).grid(row=9, column=1)
    
    confirm = Button(root,text="保存设置").grid(row=10, pady=5, padx=10, sticky=W)
def main():
    #设置主窗体属性
    root = Tk()
    root.geometry('500x600')
    root.title("Android应用资源文件检测程序")
    init(root)
    mainloop()
if \_\_name\_\_ == '\_\_main\_\_':
    main()

![](http://www.booleanln.cn/blog/wp-content/uploads/2018/07/TIM图片20180719001631.png)