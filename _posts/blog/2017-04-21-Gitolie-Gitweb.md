---
layout: post
title: gitolite、GitWeb搭建Git服务器
categories: Blog
description: 基础。
keywords: Git服务器, Linux操作
---
整理了一下这两天在Ubuntu16上搭建Git服务器的知识
 
## Gitolite安装配置

### 安装相关软件
```shell
sudo apt-get update 
sudo apt-get install git
sudo apt-get install openssh-server

```
### 创建git用户
```
sudo adduser --system --shell /bin/bash --group git
sudo adduser git ssh #特定系统需要加入ssh组才能开始ssh协议登录
passwd git #设置密码

```
### 配置gitlite
1. 生成ssh key——git需要使用ssh访问，所以需要生成一组ssh key，
    ```
    su git
    ssh-keygen
    cd .ssh
    cp id_rsa.pub authorized_keys
    
    ```
2. 安装gitolite  

  ```
    su git
    mkdir bin  #创建bin目录用于安装gitolite
  #从远端克隆gitolite
  git clone git://github.com/sitaramc/gitolite
  #执行如下命令进行安装
  ~/gitolite/install -to ~/bin
  mv ~/.ssh/authorized_keys ~/git.pub
  ~/bin/gitolite setup -pk ~/git.pub
  
  ```
成功如图：![此处输入图片的描述][1]

>  成功安装后gitolite会自动生成两个仓储，一个是testing.git用来测试，另一个gitolite-admin就是用来管理gitolite的配置仓储。 将gitolite-admin.git clone到本地，注意：还是在git用户下，因为当前只有git用户对其有读写权限。
     
### 添加git用户
使用xshell rz命令将pub文件考入gitolite-admin仓库中的keydir目录

```
rz #选择公钥文件

```
### 添加仓库

打开gitolite-admin/conf/gitolite.conf文件

```

vim /home/git/gitolite-admin/conf/gitolite.conf #修改配置文件

```

![此处输入图片的描述][2]
> 上面的repo代表是创建了一个demo仓库（创建的方式有很多种，这里我只是介绍这一种），下面的RW代表可读写，还有其他的关键字，自己搜索。等于号后面的代表是对于这个仓库的权限，多用户使用空格。

### 将配置推送到gitolite服务器

```
  git add .
  git commit -m "这个是提交信息，用于表示这次提交的解释，可以随便写"
  git push origin master 
  
```

至此gitolite搭建成功

## GitWeb安装配置

### 安装

```
  sudo apt - get install gitweb apache2 
```

### 配置
```
vim /etc/apache2/conf-available/gitweb.conf  配置apache gitweb所在目录目录
cd /var/ www/gitweb   
sudo ln -s /usr/ share/gitweb/* .    ##进行gitweb资源链接
```

```
Alias /gitweb /var/www/git
SetEnv GITWEB_CONFIG /etc/gitweb.conf
<Directory /var/www/git>
Options ExecCGI FollowSymLinks SymLinksIfOwnerMatch
AllowOverride All
order allow,deny
Allow from all
AddHandler cgi-script cgi
DirectoryIndex gitweb.cgi
</Directory>
```

```
vim /etc/gitweb.conf #修改projectroot为你repository所在目录
/etc/init.d/apache2 restart
```


[1]: http://img.blog.csdn.net/20140809155108345?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbWVuZ3hpYW5neXVl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast
[2]: http://img.blog.csdn.net/20140809160614083?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbWVuZ3hpYW5neXVl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast