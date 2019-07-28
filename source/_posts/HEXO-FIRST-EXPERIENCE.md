---
title: HEXO FIRST EXPERIENCE 
date: 2019-02-05 17:02:30 
tags: hexo,github 
---
### 由wordpress向github+hexo迁移

#### hexo本地环境配置

1. Node.js与npm、cnpm安装

     **官网链接:**https://nodejs.org/en/

2. git安装

#### 使用npm下载安装Hexo

```
$ cnpm install -g hexo-cli  
$ cnpm install hexo-server --save
```

#### 初始化Hexo

```
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

在本地浏览器上查看http://localhost:4000

<!--more-->

#### 主题配置

1. 我这里采用了yilia

```
$ cd blog
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
$ cd themes/yilia
$ git pull
```

2. 修改 _config.yml ：theme:yilia

3. yilia配置（修改blog/themes/yilia/_config.yml)

 #### WordPress迁移

1. 安装 hexo-migrator-wordpress 插件

```
$ npm install hexo-migrator-wordpress --save
```

2. wordpress后台导出文章

3. 调用命令

```
$ hexo migrate wordpress <source>
```

#### github配置

1. `new Repository`

2. **Repository name**：booleanln.github.io

3. 编辑_config.yml

   ```
   deploy:
       type: git
       repo: https://github.com/your_username/your_username.github.io.git
       branch: master
   ```

#### 部署代码到Github

1. 清除缓存文件和已生成的静态文件

   `hexo clean`

2. 生成静态文件

   `hexo generate`

3. 部署

   `hexo deploy`

#### 绑定域名

添加域名解析到IP`192.30.252.153`

在blog/source目录下，新建文件CNAME，文中写出要绑定的域名：“blog.booleanln.cn”

#### 文章发布

`hexo new "my new post"`

`hexo clean`

`hexo g -d`

`<!--more-->`用于文章截断

