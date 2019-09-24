---
layout: post
title: linux常用命令
categories: Blog
description: linux常用命令
keywords: linux
---
# linux常用命令

标签（空格分隔）：linux 

cat /proc/cpuinfo 显示CPU info的信息 
ls
ll 文件显示
tree 显示文件和目录由根目录开始的树形结构
lstree 显示文件和目录由根目录开始的树形结构

##文件搜索
find / -name file1 从 '/' 开始进入根文件系统搜索文件和目录 
find / -user user1 搜索属于用户 'user1' 的文件和目录 
find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件 
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件 
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件 
find / -name \*.rpm -exec chmod 755 '{}' \; 搜索以 '.rpm' 结尾的文件并定义其权限 
find / -xdev -name \*.rpm 搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备 

## tail
tail命令，常用于边发请求边查看
tail -f filename 显示文件尾部10行，动态刷新显示在屏幕上
tail -n 20 filename 显示文件尾部20行
tail -r - n 20 filename 逆序显示 

## cat
一次显示整个文件，cat filename
将几个文件合并为一个文件，cat file1 file2 file3 > file

## sed
相比在UE上，使用sed、awk命令可以在处理大量数据方面有很大的优势
sed可以将数据进行替换、删除、新增、选取特定行等功能
删除1.txt中以null开头的行：sed -i ‘/^null/d’ 1.txt（-i直接修改1.txt）
取出字符数为65个的行写入a.txt：sed -ne '/^.\{66\}$/p' P_txnMedmID.dat > a.txt
取出800100.txt响应时间里第三列数cos<时间>，再取出里面的时间，写入到a.txt文件里

cat 800100.txt | awk ‘{print $3}’ | cut -d ‘<’ -f 2 | cut -d ‘>’ -f 1 > a.txt


## awk
获取1.txt文件中第一行的长度：awk ‘NR==1 {print length($0)}’ 1.txt
## scp 
scp命令用于在不同的Linux系统之间来回copy文件

scp  app_temp0224.tar epsvc@128.196.40.117:/home/ap/epsvc/ #将本地的一个tar文件复制到远程机器117上的/home/ap/epsvc/目录下，如果有多个文件用空格隔开，会提示输入密码

## crontab命令
crontab -l 查看定时任务
crontab -e 编辑定时任务

##nohup 
如果正在运行一个进程，而在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在退出帐户/关闭终端之后继续运行相应的进程。
nohup command &（注：&符号表示将命令丢到后台中执行）
例子：ps -ef|grep dahong或 netstat -pan|grep 8090
      nohup ./start.sh &

##netstat 
用于显示各种网络相关信息，如网络连接、路由表、接口状态
查看端口是否已经打开
netstat -pan|grep 8101  
其中27747/java为PID/进程名称

根据PID找到是被哪个应用程序使用
ps -ef|grep 27747

##环境变量 
通过修改.bash_profile文件:

添加export PATH=/usr/local/mongodb/bin:$PATH

生效方法：source .bash_profile

有效期限：永久有效

lsb_release -a 命令
