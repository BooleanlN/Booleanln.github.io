---
title: Apache2下https的配置
tags:
  - https
  - server
  - ubuntu
url: 116.html
id: 116
categories:
  - Ubuntu
date: 2018-07-13 13:10:07
---

#### Apache2下https的配置

> [https://blog.csdn.net/positlive/article/details/54972990](https://blog.csdn.net/positlive/article/details/54972990)

1.  安装apache2
    
2.  激活SSL模块
    
    `sudo a2enmod ssl`
    
3.  重启apache2
    
4.  创建自签名SSL证书
    
    sudo mkdir /etc/apache2/ssl  
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt  
    我用的是腾讯云的证书，免费一年
    https://cloud.tencent.com/document/product/400
    
5.  编辑配置文件，修改
    
    SSL的证书路径
    
    <IfModule mod_ssl.c>
     <VirtualHost \_default\_:443>
     ServerAdmin webmaster@localhost
    ​
     DocumentRoot /var/www/html
    ​
     # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
     # error, crit, alert, emerg.
     # It is also possible to configure the loglevel for particular
     # modules, e.g.
     #LogLevel info ssl:warn
    ​
     ErrorLog ${APACHE\_LOG\_DIR}/error.log
     CustomLog ${APACHE\_LOG\_DIR}/access.log combined
    ​
     # For most configuration files from conf-available/, which are
     # enabled or disabled at a global level, it is possible to
     # include a line for only one particular virtual host. For example the
     # following line enables the CGI configuration for this host only
     # after it has been globally disabled with "a2disconf".
     #Include conf-available/serve-cgi-bin.conf
    ​
     #   SSL Engine Switch:
     #   Enable/Disable SSL for this virtual host.
     SSLEngine on
    ​
     #   A self-signed (snakeoil) certificate can be created by installing
     #   the ssl-cert package. See
     #   /usr/share/doc/apache2/README.Debian.gz for more info.
     #   If both key and certificate are stored in the same file, only the
     #   SSLCertificateFile directive is needed.
     #SSLCertificateFile     /etc/ssl/certs/ssl-cert-snakeoil.pem
     #SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
     SSLCertificateFile /etc/apache2/ssl/2_www.booleanln.cn.crt
     SSLCertificateKeyFile /etc/apache2/ssl/3_www.booleanln.cn.key
     #   Server Certificate Chain:
     #   Point SSLCertificateChainFile at a file containing the
     #   concatenation of PEM encoded CA certificates which form the
     #   certificate chain for the server certificate. Alternatively
     #   the referenced file can be the same as SSLCertificateFile
     #   when the CA certificates are directly appended to the server
     #   certificate for convinience.
     #SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt
     SSLCertificateChainFile /etc/apache2/ssl/1\_root\_bundle.crt
     #   Certificate Authority (CA):
     #   Set the CA certificate verification path where to find CA
     #   certificates for client authentication or alternatively one
     #   huge file containing all of them (file must be PEM encoded)
     #   Note: Inside SSLCACertificatePath you need hash symlinks
     #                to point to the certificate files. Use the provided
     #                Makefile to update the hash symlinks after changes.
     #SSLCACertificatePath /etc/ssl/certs/
     #SSLCACertificateFile /etc/apache2/ssl.crt/ca-bundle.crt
      
    
6.  激活SSL虚拟Host
    
    `sudo a2ensite default-ssl.conf`
    
7.  重启apache2
    
    `sudo service apache2 restart`