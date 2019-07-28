---
title: Elementary OS 初体验
url: 54.html
id: 54
categories:
  - Ubuntu
date: 2018-06-04 16:00:50
tags:
---

最近因为受够了windows下开发的各种不方便，所以转战linux，版本选了Elementary OS，之前一直用的Ubuntu的KDE桌面，也用过Debian的GNome，但感觉都不是很好，发现EOS后，被它的设计所吸引，就抓紧装上了。 安装步骤： 使用UltarlSO工具将iso文件刻录至U盘，不可以使用PE，自己还用PE去尝试了下，后来一下怎么可能，文件系统都不一样... 进入安装之后，我使用的是自定义分区，给swap分区16GB，剩余空间都给了根分区，没有去区分各个文件夹。 等待安装结束之后，进入系统，先在设置lang/region里面设置了china/chinease，重启，done！ 接着下载了火狐浏览器，使用的是 Firefox Quantum： `sudo add-apt-repository ppa:mozillateam/firefox-next` sudo apt update && sudo apt upgrade 如果你已经安装了Firefox，你会发现它已经被变成了Quantum。 Firefox徽标立即更改。 如果你还没有安装Firefox，你可以使用下面的命令来安装它： `sudo apt install firefox` reference： 接着，开发怎么能少了音乐呢，网易云音乐走起 `sudo dpkg -i netease-cloud-music_1.0.0_amd64_ubuntu16.04.deb` sudo apt-get install -f 网易云第二天开的时候死活打不开，后来发现是权限问题，导致某些文件权限用不了 `sudo netease-cloud-music` sudo nohup netease-cloud-music 开发工具安装了VSCode

> sudo dpkg -i code\_1.23.1-1525968403\_amd64.deb

pycharm community

> sudo tar -zxvf pycharm-community-2018.1.4.tar.gz -C /usr/local

> sh /usr/local/pyfile/bin/pycharm.sh &

//此时pycharm已经可以运行了，但我们需要把快捷方式放到桌面

> vim /usr/share/applications/PyCharm.desktop:

> \[Desktop Entry\] Encoding=UTF-8 Name=Pycharm Comment=Pycharm IDE Exec=sh /usr/local/pycharm-community-2018.1.4/bin/pycharm.sh Icon=/usr/local/pycharm-community-2018.1.4/bin/pycharm.png Terminal=false StartupNotify=true Type=Application Categories=Application;Development;

> cp Pycharm.desktop /home/bool/

done! 同样的，安装上Phpstorm！