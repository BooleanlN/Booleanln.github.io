---
title: 安卓adb、aapt调用
tags:
  - python
  - 安卓
url: 170.html
id: 170
categories:
  - Python是最好的语言
date: 2018-07-19 23:04:10
---

安卓adb、aapt调用 使用subprocess库

常用adb命令：

adb devices ：列出连接设备 adb install -r "xxxx.apk" :安装app adb uninstall "Xxxx"

adb shell "xxx"：执行shell命令，常用有“ls”、“mkdir”、“rm”

adb shell screencap -p "保存截图的路径" ：截图保存

adb pull "手机路径" "电脑路径" 将文件从手机导出到电脑

#设备连接
def Has_device():
 conn = 'adb devices'
 pi = subprocess.Popen(conn, shell=True, stdout=subprocess.PIPE)
 if pi.stdout.read().splitlines().\_\_len\_\_() >= 3:
 return True
 else:
 return False

#apk安装
def Install\_apk(apk\_name):
 if apk_name!=" ":
 install\_apk = 'adb install -r ' + apk\_name
 print(install_apk)
 #print(install_apk)
 install = subprocess.Popen(install_apk, shell=True, stdout=subprocess.PIPE).stdout.read()
 str1 = str(install)
 if str1.find("Success")!=-1:
 return True
 else:
 return False
 else:
 return False

常用aapt命令： aapt dump badging "xxx.apk"：列出apk信息，包括包名等 aapt dump xmltree "xxx.xml" 解析xml文件，如解析AndroidManifest.xml

aapt l "xxx.apk" 获取资源文件列表

#获取ACTIVITIES
def getActivities(packagename,apk_Name,xmlName=' AndroidManifest.xml'):
 getMainXml = 'aapt dump xmltree '+apk_Name +xmlName
 MainXml = subprocess.Popen(getMainXml,shell=True,stdout=subprocess.PIPE).stdout.read().splitlines()
 Activitys = \[\]
 for i in range(MainXml.\_\_len\_\_()):
 str_xml = str(MainXml\[i\])
 if(str(MainXml\[i\]).find('android:name')!=-1 and str(MainXml\[i\]).find(packagename)!=-1):
 index = str_xml.split("\\"")
 Activitys.append(index\[1\])
 return Activitys
#获取app包名
def getPackageName(apkname):
 getBadging = 'aapt dump badging '+apkname
 res = bytes.decode(subprocess.Popen(getBadging,shell=True,stdout=subprocess.PIPE).stdout.read())
 return  res.splitlines()\[0\].split("\\'")\[1\]

#aapt 获取资源文件
def aapt_get(dirname,resname="res"):
 Img = \[".png", ".jpg", ".jpeg", ".gif", ".bmp"\]
 shell_ml = 'aapt l '+dirname
 ImgFolder = tempfile.mkdtemp("img","unzip",".\\\temp")
 res = subprocess.Popen(shell_ml,stdout=subprocess.PIPE,shell=True).stdout.read().splitlines()
 #print(dirname)
 dir_pi= \[\]
 for i in res:
 file = bytes.decode(os.path.splitext(i)\[1\])
 if file in Img:
 dir_pi.append(bytes.decode(i))
 dir = zipfile.ZipFile(dirname)
 tempfolder=tempfile.mkdtemp(resname+"_unzip",prefix="temp",dir='')
 for names in dir.namelist():
 try:
 dir.extract(names,tempfolder)
 except:
 continue
 dir.close()
 for item in dir_pi:
 #print(tempfolder+"/"+item)
 try:
 shutil.move(tempfolder+"/"+item, ImgFolder)
 except:
 continue
 return ImgFolder