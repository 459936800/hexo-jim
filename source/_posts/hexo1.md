---
title: Hexo发布到Ubuntu云服务器
date: 2020-11-10 15:04:24
tags: 
- Hexo
- 部署
- 教程
---

## 本地安装Hexo,node.js,git

请自行百度

## 服务器端

首先需要一台Ubuntu服务器，并且可以用ubuntu用户身份正常登陆.

## 安装Nginx

请自行百度

<!-- more -->

#### 配置Nginx托管文件目录
创建/var/www/hexo目录，用于Nginx托管，修改目录所有权和权限。

``` bash
sudo mkdir -p /var/www/hexo

sudo chown -R $USER:$USER /var/www/hexo
sudo chmod -R 755 /var/www/hexo
```

#### nginx.config配置

``` config
...

server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    root /var/www/hexo; # 需要修改的部分
    index index.html index.htm;
...
```
#### 重启Nginx服务

``` bash 
sudo service nginx restart
```

#### 打包程序发布到服务器
执行代码后直接把public内的文件拷贝到/var/www/hexo即可
``` bash
hexo g
```



# 如果要配置git发布请继续看下去



#### 安装Git

Git 用于版本管理和部署，Nginx 用于静态博客托管。

``` bash
sudo apt-get update
sudo apt-get install git nginx -y
```

#### 配置git仓库路径

在/var/repo/下创建名为hexo_static的裸仓库。用如下命令

``` bash
sudo mkdir /var/repo/
sudo chown -R $USER:$USER /var/repo/
sudo chmod -R 755 /var/repo/

cd /var/repo/
git init --bare hexo_static.git
```

## 创建Git钩子

在自动生成的 hooks 目录下创建一个新的钩子文件：

``` bash
vim /var/repo/hexo_static.git/hooks/post-receive
```

在该文件中添加两行代码，指定 Git 的工作树（源代码）和 Git 目录（配置文件等）。

``` bash
#!/bin/bash

git --work-tree=/var/www/hexo --git-dir=/var/repo/hexo_static.git checkout -f
```

保存并退出文件，并让该文件变为可执行文件。

``` bash
chmod +x /var/repo/hexo_static.git/hooks/post-receive
```

## 回到本地配置

#### 修改Hexo的默认配置
在站点config.yml中修改博客的地址url

``` bash
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'

url: http://server-ip # 没有绑定域名时填写服务器的实际 IP 地址。
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

#### 通过Git部署

从服务器上把hexo_static仓库克隆下来, 以此来将服务器地址添加到受信任的站点中。

``` bash
git clone ubuntu@server_ip:/var/repo/hexo_static.git
```

注意在第一次进行这一步时会提示是否继续，选yes即可。

再编辑Hexo的config.yml文件，找到Deployment, 修改为

``` bash
deploy:
  type: git
   repo: ubuntu@server_ip:/var/repo/hexo_static.git
  branch: master
```

最后记得安装Hexo部署到Git仓库的包.
``` bash
npm install hexo-deployer-git --save

hexo d
```