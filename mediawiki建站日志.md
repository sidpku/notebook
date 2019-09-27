---
title: mediawiki建站日志
date: 2019-03-05 19:00:31
update: 2019-03-15 21:39:00
tag:
- log
- linux
- 运维
- 服务器
- 安全
categories:
- log
---

开学初，发现了云服务器这个好东西，再加上一直想要建立一个wiki网站，于是就行动了起来。使用腾讯云上搭建的centos，逐渐学会了xampp等基本的操作，在这个过程中熟悉了基本概念，也温习了好多linux的基本知识，也见识到了网络空间的疯狂，但是最大的还是一步步实践中，遇到问题解决问题的能力。

我认为，我目前在做的，还是“术”的东西。希望有一天，我在做的东西，是属于“道”的范畴的。

<!-- more -->

# log

这是一个测试文件，我用来实验“将git作为工作日志”的可能性。

今天的日期是2019年2月23日，相信typora舒适的书写环境能够带来比Evernote更棒的体验。

简直太棒啦！在网上兜兜转转找了不少资料后，找到了GitHub官网的说明文档，这样就可以愉快的学习啦！

[GitHub官网的说明文档](https://help.github.com/en)

在网上找到的其他关于git的教程，不过没有阅读，感觉质量还挺高的。

[Git for Designers](https://blog.teamtreehouse.com/git-for-designers-part-1)

[GitHub: the beginners' guide](https://www.pluralsight.com/blog/software-development/github-tutorial)

## GitHub在command line下的操作

github的操作说明文档

[非常简单实用的帮助文档](https://rogerdudler.github.io/git-guide/index.zh.html)

借助上面这个文档的帮助，我可以在linux上进行修改啦~

### git基础配置

```powershell
# 设置名字（说实话，我不知道这一步是用来干嘛的）
[root@local ~]$ git config --global user.name "xxx"
[root@local ~]$ git config --global user.email "xxxx@xxxx"
# 配置sshkey
# 1. 首先查看是否已经存在密钥
[root@local ~]$ ls -al ~/.ssh/	
# 2. 创建新密钥,并添加新密钥
[root@local ~]$ ssh-keygen -t rsa -b 4096 -C "xxx@xxx"
[root@local ~]$ eval $(ssh-agent s)
[root@local ~]$ ssh-add ~/.ssh/id_rsa
# 3. 在github账号中授权密钥
# 复制 ~/.ssh/id_rsa.pub 中的内容
# 在github 账号的settings中设置ssh部分
```

### clone

[clone-官方说明文档](https://help.github.com/en/articles/cloning-a-repository)

```shell
$ cd [work directory] 
$ git clone [url]
```

### pull & add & commit & push

```
#获取最新版本
$ git pull 
# 首先将文件cp到repo目录中
$ git add .
$ git commit -m "Add existing file"
$ git push origin master
Username for 'https://github.com':xxx@xxx
Password for 'https://xxx@xxxx@github.com":xxxxxxxx
```



[相关的官方说明文档](https://help.github.com/en/articles/adding-a-file-to-a-repository-using-the-command-line)

## wiki备份

wiki的备份包括两个部分，一个是wiki的数据库，一个是文件系统。

### Database

数据库有很多方法，试用了以下几种方法：

#### Automysqlbackup

成功使用这个工具进行了备份

**安装&下载**

[下载网址](https://sourceforge.net/projects/automysqlbackup/)

```powershell
# 下载
# 将安装包解压缩
[root@local ~]$ mkdir tmp
[root@local ~]$ tar -xzvf automy* -C ./tmp
# 按照README进行安装
[root@local ~]$ sh ./tmp/intall.sh
```

**进行配置**

```powershell
# 按照README进行配置
[root@local ~]$ vim /etc/automysqlbackup/myserver.conf
```

```powershell
#设置备份进程使用的用户身份和密码
CONFIG_mysql_dump_username='xxx'
CONFIG_mysql_dump_password='xxxxx'
#设置备份存放的位置
CONFIG_backup_dir='/root/repository/mywiki'

# List of databases for Daily/Weekly Backup e.g. ( 'DB1' 'DB2' 'DB3' ... )
# set to (), i.e. empty, if you want to backup all databases
CONFIG_db_names='wikidb'
# You can use
#declare -a MDBNAMES=( "${DBNAMES[@]}" 'added entry1' 'added entry2' ... )
# INSTEAD to copy the contents of $DBNAMES and add further entries (optional).

# List of databases for Monthly Backups.
# set to (), i.e. empty, if you want to backup all databases
CONFIG_db_month_names='wikidb'
```

```powershell
#因为使用xampp，所以需要添加路径，使得automysqlbackup能够调用mysqldump
[root@local ~]$ PATH="$PATH":/opt/lampp/bin/
```

**如何运行**

```powershell
[root@local ~]$ automysqlbackup /etc/automysqlbackup/myserver.conf
```

**如何还原**

```powershell
# 注意根据自己的实际情况进行改变，并没有进行实验
[root@local ~]$ gunzip < /var/lib/automysqlbackup/weekly/my_wiki/my_wiki_week.18.2016-05-07_15h32m.sql.gz|mysql -uUSER -pPASSWORD my_wiki

```



> **tarball安装方式**
>
> 1.linux distribution自带的软件都在/usr/src中，而用户自己安装的则在/usr/local/src中，这是一种文件系统规范
> 2.一定要阅读README文档，还有将文件在/usr/local/src中解压缩的时候，记得新建一个目录，不然会把src弄的很乱
>
> **过去**：
>
> 试用这个方法没有成功（因为没有进行文件的配置）,mediawiki官网上没有automysqlbackup的配置指南。

#### Using command line

```php
#在LocalSettings.php文档中加入
$wgReadOnly = 'Dumping database, access will be restored shortly' 
#进行数据库备份,后面跟的各项参数都能够在LocalSettings.php中找到，具体见官网文档。
$ mysqldump -h localhost -u root -p --default-character-set=binary CUES > backup.sql


```

[应该使用什么参数：Mysqldump from the command line ](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)

#### PhpMyAdmin

这是最简单最方便的一种方法。

但问题在于，我不知道怎么同时让wiki的页面和xampp的页面存在，或许以后可以研究下这个文章：

[linux下浅谈xampp配置以及使用详细教程](https://my.oschina.net/logion/blog/294977)

因为前两种方法都失败了，也就意味着“除非我把automysqlbackup配置好”，现在的数据都找不回来了，看来在正式开站前还是要不断地试错练习。——2019.2.24 sid

### Filesystem

需要尝试

# log2

我决定重新来过，使用centos 7.2 跟鸟哥的版本保持一致，系统学一下服务器篇，同时回顾基础知识，加油！

## 简单基础设定

### 磁盘阵列

```powershell
[root@cloud ~]# cat /sys/block/sda/queue/scheduler
noop deadline [cfq]
[root@cloud ~]# cat /sys/block/sda/queue/read_ahead_kb
128
# 上面这个是预设值，我们想要每次开机都修改，因此可以这样做：

[root@cloud ~]# mkdir bin; cd bin 
[root@cloud bin]# vim parameters.sh 
#!/bin/bash

echo "deadline" > /sys/block/sda/queue/scheduler
echo 4096 > /sys/block/sda/queue/read_ahead_kb

[root@cloud bin]# chmod a+x /root/bin/parameters.sh 
[root@cloud bin]# vim /etc/rc.d/rc.local 
/usr/bin/bash /root/bin/parameters. sh

[root@cloud bin]# chmod a+x /etc/rc.d/rc.local 
[root@cloud bin]# sh /etc/rc.d/rc.local 
[root@cloud bin]# cat /sys/ block/sda/queue/scheduler /sys/block/sda/queue/read_ahead_kb
noop [deadline] cfq
4096

```

### 保持软件更新

```powershell
# 立即更新
[root@local ~]$ yum -y update
# 设置每天3点自动更新
[root@local ~]$ vim /etc/crontab
0 3 * * * root /bin/yum -y update

```

## 基本的安全强化

```powershell
#查看开启的网络服务
[root@VM_0_13_centos bin]# netstat -tlunp

```

- dhclient：

  自动获取IP地址的服务，不能够关闭。除了固定的端口68外，会自动调用两个随机的端口。

- ntpd：

  自动校时服务，可以关闭。（但是我保留了）

```powershell
#将服务永久关闭

[root@cloud ~]# systemctl stop rpcbind.socket     <==立刻关闭该服务 
[root@cloud ~]# systemctl disable rpcbind.socket  <==下次开机不会启用

```

### 获取root邮件

```bash
[root@VM_0_13_centos ~]# vim /etc/aliases\
# Person who should get root's mail
root:           root,zeroXY
[root@VM_0_13_centos ~]# newaliases

```

头大，mail服务不可用，真是愁人，网上好像也没有对症的啊！

1. 发现之前没有安装sendmail服务

```powershell
[root@VM_0_13_centos ~]# yum install sendmail
[root@VM_0_13_centos ~]# service sendmail start

```

2. 安装好之后发现还是不能给zeroXY发邮件，回复消息是user unknown
3. 重新创建了一个用户为zero，发现可以正常发送邮件了

### 使用Logwatch分析登陆档

1. 安装logwatch

   ```powershell
   [root@VM_0_13_centos ~]# yum intall logwatch
   [root@VM_0_13_centos ~]# sh /etc/cron.daily/0logwatch
   # ?是否每次开机都需要运行sh?
   
   ```

2. 配置

   ```powershell
   #配置文件的位置:	/usr/share/logwatch/default.conf/logwatch.conf
   #分析的Log文件所在的位置：	/var/log
   #配置选项：
   	#Mailto：
   	#range = yesterday
   	#Detail = low
   	#Service = all
   
   ```

   查看所有支持分析的服务：

   ```powershell
   [root@local ~] ls -l /usr/share/logwatch/scripts/services
   
   ```

   PS:每次都把`VM_0_13_centos`打全实在是太累了，所以以后都用`local`代替了。

   在`service = all `的配置下，如果**不希望分析某些服务**，可以使用如下设置[^1]：

   ```bash
   service = all
   service = "-[service_1]"
   service = "-[service_2]"
   ...
   
   ```

   [^1]: [在Linux上使用logwatch分析监控日志文件](http://seanlook.com/2014/08/23/linux-logwatch-usage/)

   如果想要**只分析指定的服务**，则在`logwatch.conf`中修改为：

   ```powershell
   service = [service_1]
   service = [service_2]
   ...
   
   ```

   文件目录：

   ```bash
   /usr/share/logwatch
       default.conf/     # 配置目录
           logwatch.conf   # 主配置文件，收件人，级别等
           logfiles/       # 定义待分析服务的日志文件组路径，相对于/var/log(*.conf)
           services/       # 自定义需分析日志的Service目录(*.conf)
       scripts/          # 可执行脚本
           logwatch.pl     # 启动分析的perl脚本，/usr/sbin/logwatch的源链接
           logfiles/       # 可包含多个logwatch日志文件组的子目录，对应的日志服务
           				# 运行的时候，子目录下的脚本会自动被调用
           services/       # logwatch日志服务的过滤脚本，一一对应
           shared/         # 可被多个logwatch日志服务引用的脚本
       dist.conf/
           logfiles/
           services/
       lib/
   
   ```

3. 手动使用Logwatch in command line

   略...

> Logwatch 部分参考了文章：[How To Install and Use Logwatch Log Analyzer and Reporter on a VPS](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-logwatch-log-analyzer-and-reporter-on-a-vps)

4. 自己分析的一点经验

   首先，网上很难找到“如何分析Logwatch报告”的经验。

   - new users

     - smmsp: sendmail创建的用户，sendmail邮件提交程序，具体内容可见:

       [linux – 用户smmsp在auth.log中反复找到](https://codeday.me/bug/20181110/369228.html)

     - saslauth: 同样为系统服务创建的用户

### iptables

#### 安装

```powershell
[revoluta@local ~]$ sudo yum install iptables
[revoluta@local ~]$ sudo yum install iptables-services

```

#### iptables配置

```
[root@local ~]$ mkdir ~/bin
[root@local ~]$ vim ~/bin/firewall.sh

```

```bash
#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin; export PATH

#1.清除规则
iptables -Z
iptables -F
iptables -X

#2.设定政策
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT

#制定规则
iptables -A INPUT -p icmp -j ACCEPT
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A INPUT -p tcp -m state --state NEW,ESTABLISHED  --dport 1129 -j ACCEPT

#写入防火墙规则配置文件
service iptables save

```



非常好的两个参考文献：

1. [鸟哥的私房菜中关于防火墙、iptables的讲解](http://cn.linux.vbird.org/linux_server/0250simple_firewall.php#netfilter_syntax_simple)
2. [Digital Ocean 上面关于iptables常见配置的说明，有http、ssh的针对性规则](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands)

#### fail2ban

fail2ban配合iptables防止ssh密码爆破

```powershell
[root@local ~]$ yum install fail2ban
# 创建本地配置文件
[root@local ~]$ vim /etc/fail2ban/jail.local

```

> 不能够直接复制/etc/fail2ban/jail.conf成jail.local,否则会不能启动配置。

```bash
[DEFAULT]
# Ban hosts for one hour:
findtime = 600
bantime = 3600
maxretry = 5

#set email alert
destemail = root@localhost
sendername = Fail2Ban
mta = sendmail
action = $(action_mwl)s

# Override /etc/fail2ban/jail.d/00-firewalld.conf:
banaction = iptables-multiport

[sshd]
enabled = true

```

```powershell
[root@local ~]$ service fail2ban start
[root@local ~]$ systemctl enable fail2ban.service
#查看配置情况
[root@local ~]$ fail2ban-client status
Status
|- Number of jail:	1
`- Jail list:	sshd
#若结果异常，可尝试重启服务
[root@local ~]$ service fail2ban restart

```

非常好的参考资料:[How to Protect SSH With Fail2Ban on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-centos-7)

### 禁止root通过ssh登录

```powershell
[root@local ~]$ vim /etc/ssh/sshd_config

#PermitRootLogin yes
#将上面一行修改成
PermitRootLogin no

[root@local ~]$ service sshd restart

```

测试成功



后续需要做的工作

- [x] 修改ssh端口
- [x] 禁止root通过ssh登录
- [x] 限制可以通过ssh登录的ip地址
  - [x] iptables
  - [x] fail2ban

### 修改ssh端口

```powershell
[root@local ~]$ vim /etc/ssh/ssh_config
[root@local ~]$ vim /etc/ssh/sshd_config
#修改方法：将 Port=22 去掉注释，修改成需要的端口号
#疑问：有的网站只修改了ssh_config，有的把sshd_config也修改了，有什么影响吗？

#重启ssh服务
[root@local ~]$ service sshd restart

```

我这里修改为了xxxx

> 所有修改ssh服务的操作，都只对今后建立的ssh有影响，已经建立的连接可以正常存在
>
> 重启sshd服务后，原来的22端口自动关闭
>
> 注意： 如果系统开起了iptables防火墙，那么还需要把修改之后的端口号添加到防火墙里面，不然SSH会连不上。

[参考文献](https://blog.csdn.net/tianlesoftware/article/details/6201898)

### 给普通用户添加sudo权限

注意！网上有些资料首先给`/etc/sudoers`增加w权限，然后再修改文件，这样不可取！

在`/etc/sudoers`中明确说使用`visudo`来修改这个文件，切记。

```bash
# 执行sudo命令需要以root身份
[root@local ~]$ visudo

```

找到下面一行

```bash
root	ALL=(ALL)	ALL

```

在该行下添加

```bash
xxxx	ALL=(ALL)	ALL
Defaults:zero timestamp_timeout=-1	# 进行sudo只需要输入一次密码

```

```bash
[root@local ~]$ visudo -c	# 进行拼写检查 

```

参考文献：

1. [sudo与visudo的区别](https://blog.51cto.com/chenfage/1830424)
2. [给普通用户添加sudo权限](https://blog.csdn.net/Dream_angel_Z/article/details/45841109)

## 平日的运维

### 登陆档的查看

在`基本的安全强化`中，记录了logwatch的安装和配置，但是依旧不清楚产生的分析文件所表达的含义。

可以参考鸟哥的文章:[第十八章、认识与分析登录档](http://linux.vbird.org/linux_basic/0570syslog.php) 主要讲了登陆档的产生、轮替。

## linux基本操作

本部分介绍Linux常用软件的操作，常用软件定义为普通用户同样会用到的软件

### mail

```bash
[root@local ~]$ mail -u [username] # 显示[username]的邮件箱
[root@local ~]$ mail	# 打开自己的邮件箱，会进入应用内环境
& [number]	# 打开第[number]封邮件
& u			# 显示未读邮件标题
& h			# 显示所有邮件标题
& d			# 显示已删除的邮件
& d [number1-number2]	# 删除第number1到第number2的邮件

```

> 参考：[mailx命令](https://ywnz.com/linux/mailx/)

### 开机关机重启

```bash
reboot	% 重启
```



# log3

好烦啊！Xshell总是让我提交ssh key的密码，但是我忘记了！而且在创建系统的时候设定的是使用密钥登陆，我现在用密码登陆还上不去！而且腾讯云的服务超垃圾，我换个密钥，解绑很顺利，但是绑定却出错不行！而且绑定了之后，发现根本没用，原来的那个还有效.....我真的是无语了，重装系统吧。

我已经按照这个文档中的操作，把已经会的安全强化措施都做上了，重装系统后第二次做很顺畅了，接下来就是试探iptables和fail2ban，然后再学一下防火墙的基本知识，再弄好git,再去建站，建站后要联系数据库的备份。我后悔了，我在git部分应该做好笔记的。

## Xampp

### 安装

[官网下载地址](https://www.apachefriends.org/download.html)

[官网F&Q](https://www.apachefriends.org/faq_linux.html)

```powershell
# 修改权限
[root@lcoal ~]$ chmod 755 xampp-linux-*-installer.run
# 安装
[root@local ~]$ ./xampp-linux-*-installer.run
# 修改防火墙规则,firewall.sh文件见 log2-iptables
[root@local ~]$ vim ~/bin/firewall.sh
# 添加下面两行
iptables -A INPUT -m state --state NEW -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -m state --state NEW -p tcp --dport 443 -j ACCEPT
[root@local ~]$ sh ~/bin/firewall.sh

# 设置开机启动
[root@local ~]$ ln -s /opt/lampp/lampp /etc/init.d/lampp
[root@local ~]$ chkconfig --add lampp

```

## mediawiki

### 安装

```
# 将文件解压到xampp的根目录
[root@local ~]$ tar -xvzf mediawiki-1.32.0.gz.tar
[root@local ~]$ mkdir /opt/lampp/htdocs/xampp
[root@local ~]$ move mediawiki-1.32.0/* /opt/lampp/htdocs/xampp

```



## 今后的任务表

- [x] iptables
- [x] fail2ban
- [x] 防火墙
- [x] git
- [ ] xampp
- [ ] mediawiki
- [ ] 数据库备份
- [ ] 开站
- [ ] 增加数据

## 可以拓展的部分

- [ ] [关于Fail2Ban的一些拓展设置](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-centos-7)
- [ ] [鸟哥-关于防火墙安全的更多案例与知识](http://cn.linux.vbird.org/linux_server/0250simple_firewall.php)

