---
title: Python解包APK文件
url: 80.html
id: 80
categories:
  - Python是最好的语言
date: 2018-06-18 16:19:19
tags:
---

#### **解包APK文件**

`import zipfile import os def un_zip(filename): zip_file = zipfile.ZipFile(filename) if os.path.isdir(filename + "_files"): pass else: os.mkdir(filename+"_files") for names in zip_file.namelist(): zip_file.extract(names,filename + "_files/") zip_file.close() un_zip("xxxx.apk")`