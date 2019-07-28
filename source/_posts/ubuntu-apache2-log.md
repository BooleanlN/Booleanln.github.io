---
title: Ubuntu_apache2 log查看
url: 131.html
id: 131
categories:
  - Ubuntu
date: 2018-07-13 14:16:58
tags:
---

**apache2日志查看**

*   查看apache2.conf

  ErrorLog ${APACHE\_LOG\_DIR}/error.log

*    找到日志位置，以变量形式存放

 export APACHE\_LOG\_DIR=/var/log/apache2$SUFFIX	
  cat /etc/apache2/envvars