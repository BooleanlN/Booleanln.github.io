---
title: Flask+Apache2.4+mod_wsgi配置
url: 95.html
id: 95
categories:
  - Other
  - Python是最好的语言
  - Ubuntu
date: 2018-06-28 12:43:57
tags:
---

### Flask+Apache2.4+Ubuntu16.04+mod_wsgi配置

1\. 安装Flask、Apache2.4 2. 安装mod_wsgi `#apt-get install libapache2-mod-wsgi` 3\. 在Web项目文件夹中创建一个.WSGI文件 `import sys sys.path.insert(0, '/var/www/html/C4Web/route')#如果wsgi与application不在一个文件夹，需要导入路径先 from web import app as application #导入app作为application（wsgi文件只识别这个名字）` 4\. 配置Apache2.4 `cd /etc/apache2/sites-available/ sudo vim 000-default.conf WSGIPythonPath /var/www/html/C4Web/` `ServerAdmin webmaster@localhost DocumentRoot /var/www/html/C4Web/ WSGIScriptAlias / /var/www/html/C4Web/start.wsgi` `Require all granted #(apache2.4格式) #Order allow,deny #(apache2.2格式) #Allow from all WSGIScriptReloading On #支持自动重载` ErrorLog ${APACHE\_LOG\_DIR}/error.log LogLevel warn CustomLog ${APACHE\_LOG\_DIR}/access.log combined 5. web.py编写 `if __name__=='__main__': app.debug= True app.run()` 出问题可查看 `/var/log/apache2` [官方文档](http://dormousehole.readthedocs.io/en/latest/deploying/mod_wsgi.html)