---
title: Linux笔记
date: 2022-07-11
tags: Linux
categories: 笔记
description: Linux笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/14.jpg
---
# **Linux概述**

Linux是基于Unix的，是一种自由和开放源码的操作系统，存在着许多不同的Linux版本，但它们都使用了Linux内核。

**Linux系统的应用**

- 服务器系统Web应用服务器、数据库服务器、接口服务器、DNS、FTP等
- 嵌入式系统路由器、防火墙、手机、PDA、IP 分享器、交换器、家电用品的微电脑控制器等
- 高性能运算、计算密集型应用，Linux有强大的运算能力
- 桌面应用系统
- 移动手持系统

**Linux主流版本**：ubuntu、CentOS、redhat、debian等



# **Linux安装**

**VMware下载安装**

- [下载地址](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)点击Download now
- 安装完成后输入许可证即可

**创建CentOS虚拟机**

- 选择典型配置
- 稍后安装操作系统
- 自定义硬件配置，选中新CD使用[ISO映像文件](http://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)
- 右上角打开网络适配器：system eth0
- 右键打开命令行：open in Terminal
- ifconfig：获取ip地址
- 使用Finalshell远程操作虚拟机



# **Linux目录结构**

"/"：根目录

管理员登录会进入到root目录

用户登录会进入到home文件下的对应用户目录

usr：软件安装目录



# **Linux常用命令**

ifconfig：获取ip地址

pwd：获取当前目录

touch创建一个空文件：touch a.txt

clear：清屏

## **显示文件列表**

ls：获取当前目录下的所有文件(只显示名称)

ls -a：获取当前目录下的所有文件(包括隐藏文件)

ls -l / ll：获取当前目录下的所有文件(会展示详细信息)

## **切换目录**

cd 文件名(tab自动补全)：进入文件夹

cd 文件路径：进入该路径，例如：cd /usr/src

cd .. ：返回上一级

cd ~：回到当前用户目录root/home下的用户目录

cd -：返回上一次所在目录

## **创建目录和移除目录**

mkdir 文件名：创建文件夹

mkdi -p 文件名/文件名：创建多级文件夹

redir 文件名：删除空文件夹

## **浏览文件**

cat 文件：查看文件所有内容

more 文件：分屏查看文件内容，q退出查看

less 文件：和more类似，支持上下键翻页

tail -数字 文件名：查看文件最后数字行内容

tail -f 文件名：动态查看文件变化，CTRL + C结束

## **文件复制剪切删除**

cp a b：在当前目录下把a文件复制为b文件

cp a b/：把a文件复制到b文件夹中

cp a 路径：把a文件复制到该路径中

cp a b/c.：把a文件复制为c文件并保存到b文件中

cp a ../：将a文件复制到上一层目录中

mv a 路径：把a剪切到该路径中（其他和cp类似）

mv 文件名 新文件名：重命名文件

rm 文件名：删除文件，y确认删除

rm -r 文件夹名：删除文件夹，y确认删除

rm -rf：不询问直接删除文件夹

rm -f：不询问直接删除文件

rm -rf /*：自杀

## **打包和解压**

tar能够将用户所指定的文件或目录打包成一个文件，但不做压缩。一般Linux上常用的压缩方式是选用tar将许多文件打包成一个文件，再以gzip压缩命令压缩成xxx.tar.gz(或称为xxx.tgz)的文件

- -c：创建一个新tar文件
- -v：显示运行过程的信息
- -f：指定文件名
- -z：调用gzip压缩命令进行压缩
- -t：查看压缩文件的内容
- -x：解开tar文件

tar -cvf xxx.tar ./*：把当前文件夹所有文件打包到xxx中

tar -cvf xxx.tar 文件名/：把该文件打包到xxx中

tar –zcvf xxx.tar.gz ./* ：把当前文件夹所有文件打包并压缩到xxx中

tar –zxvf xxx.tar：解压到当前目录

tar -zxvf xxx.tar.gz -C ./a：解压到当前目录下的a文件中

## **查找文件和内容**

find / -name "ins*"：查找以ins开头的文件

find / -name "ins*" -ls：查找 以ins开头的文件详细信息

find / –user xxx –ls 查找用户xxx的文件

find / –user xxx –type d –ls 查找用户xxx的目录

find / -perm -777 -type d-ls：查找权限是777的文件

grep：查找文件里符合条件的字符串

grep xxx 文件名 ：在文件中查找xxx，并显示其所在行

grep xxx 文件名 –-color ：高亮显示xxx

grep xxx 文件名 -A1：显示所在行和后一行

grep xxx 文件名 -B1：显示所在行和前一行

## **VI和VIM编辑器**

在Linux下一般使用vi编辑器来编辑文件。vi既可以查看文件也可以编辑文件。

三种模式：命令行、插入、底行模式。

- 切换到命令行模式：按Esc键

- 切换到插入模式：按 i 、o、a键
    - i 在当前位置前插入
    - I 在当前行首插入
    - a 在当前位置后插入
    - A 在当前行尾插入
    - o 在当前行之后插入一行
    - O 在当前行之前插入一行
- 切换到底行模式：按 :（冒号）

打开文件：vim 文件名

退出：底行模式q

保存并退出：底行模式wq

不保存退出：底行模式q!

搜索：底行模式/+搜索内容

快捷键：

- dd – 快速删除一行
- yy - 复制当前行
- nyy - 从当前行向后复制几行
- p - 粘贴
- R – 替换

## **重定向输出**

">"：把控制台输出内容重定向覆盖输出到另一个文件例如，cat a.txt > b.txt

">>"：把控制台输出内容重定向追加输出到另一个文件例如，ifconfig >> b.txt

## **系统管理命令**

ps -ef ：查看所有进程

ps -ef | grep 进程名：查找某一进程

kill 数字：杀死该编号进程

kill -9 数字：强制杀死进程

**管道**

管道是将一个命令的输出用作另一个命令的输入

ls --help | more：分屏查看帮助信息

ps -ef | grep java：查询名字中包含Java的进程



# **Linux权限**

文件最前边十个字符表示权限x xxx xxx xxx分为四部分

1. 文件类型
    - -代表文件
    - d表示文件夹
    - l表示链接
2. 当前用户具有该文件的权限
    - r：读   		代表数字4
    - w：写          代表数字2
    - x：执行       代表数字1
3. 当前组内其他用户具有该文件的权限
    - r：读
    - w：写
    - x：执行
4. 其他组的用户具有文件的权限
    - r：读
    - w：写
    - x：执行

**文件权限管理**

chmod u=rwx,g=rx,o=r 文件名：u代表当前用户，g代表组，o代表其他组

chmod xxx 文件名：xxx为数字，例如7表示rwx，6表示wx

**切换到root用户**：su



# **Linux常用网络操作**

**主机名配置**

hostname：查看主机名

hostname xxx：修改主机名，重启后无效

修改/etc/sysconfig/network文件：永久生效

**ip地址配置**

ifconfig eth0 192.168.12.22：修改ip地址(重启后无效)

修改 /etc/sysconfig/network-scripts/ifcfg-ens33文件：永久生效

- DEVICE=eth0 #网卡名称
- BOOTPROTO=static #获取ip的方式(static/dhcp/bootp/none)
- ONBOOT=yes # 系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备。
- HWADDR=00:0C:29:B5:B2:69 #MAC地址
- IPADDR=12.168.177.129 #IP地址
- NETMASK=255.255.255.0 #子网掩码
- NETWORK=192.168.177.0 #网络地址
- BROADCAST=192.168.0.255 #广播地

**域名映射**

/etc/hosts文件用于在通过主机名进行访问时做ip地址解析用

**网络服务管理**

service 服务名 status 查看指定服务的状态

service 服务名  stop 停止指定服务

service 服务名  start 启动指定服务

service 服务名  restart 重启指定服务

service --status–all 查看系统中所有后台服务

netstat –nltp 查看系统中网络进程的端口监听情况

## **防火墙设置**

查看防火墙状态：systemctl status firewalld/firewall-cmd --state

service firewalld status 查看防火墙的状态

暂时关闭防火墙：systemctl stop firewalld

永久关闭防火墙：systemctl disable firewalld

开启防火墙：systemctl start firewalld

开放指定端口：firewall-cmd --zone=public --add-port=/tcp --permanent

关闭指定端口：firewall-cmd --zone=public --remove-port=8080/tcp --permanent

立即生效：firewall-cmd --reload

查看开放的端口：firewall-cmd --zone=public --list-ports

- systemctl是管理Linux中服务的命令，可以对服务进行启动，停止，重启，查看状态等操作
- firewall-cmd是Linux中专门用于控制防火墙的命令
- 为了保证系统安全，服务器的防火墙不建议关闭

Linux无网络链接：

- vi /etc/sysconfig/network-scripts/ifcfg-ens33

- BOOTPROTO=dhcp

  ONBOOT=yes

- win + r：services.msc

  打开VM DHCP和NAT服务

- 虚拟机终端输入：service network restart



# **Linux安装软件**

- 二进制发布包

软件已经针对具体平台编译打包发布，只要解压，修改配置即可

- RPM包

软件已经按照redhat的包管理工具规范RPM进行打包发布，需要获取到相应的软件RPM发布包，然后用RPM命令进行安装

- Yum在线安装

软件已经以RPM规范打包，但发布在了网络上的一些服务器上，可用yum在线安装服务器上的rpm软件，并且会自动解决软件安装过程中的库依赖问题

- 源码编译安装

软件以源码工程的形式发布，需要获取到源码工程后用相应开发工具进行编译打包部署。

**上传与下载工具介绍**

- FileZilla

- lrzsz

我们可以使用yum安装方式安装 yum install lrzsz

注意：必须有网络

可以在crt中设置上传与下载目录



## 安装wget

yum install wget

命令：wget  +  要下载文件的链接



## 安装xui

```
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

firewall-cmd --zone=public --add-port=9999/tcp --permanent

firewall-cmd --reload

## 安装git

yum install git

## 安装node

1.准备

进入/usr/local/node文件，没有就进行创建

```bash
mkdir  /usr/local/node
```

```bash
cd /usr/local/node
```

2.下载

```bash
wget https://npm.taobao.org/mirrors/node/v14.17.0/node-v14.17.0-linux-x64.tar.gz
```

3.解压

```bash
tar -zxvf node-v14.17.0-linux-x64.tar.gz
```

4.配置

使用ln -s创建软连接

```bash
ln -s /usr/local/node/node-v14.17.0-linux-x64/bin/npm /usr/local/bin/npm

ln -s /usr/local/node/node-v14.17.0-linux-x64/bin/node /usr/local/bin/node 
编辑文件
```

```bash
vi /etc/profile
```

在文件最后面加入下面的语句

```bash
NODE_HOME=/usr/local/node/node-v14.17.0-linux-x64
PATH=$NODE_HOME/bin:$PATH
export NODE_HOME PATH
```

让刚才的配置生效

```bash
source /etc/profile
```

5.验证

node -v

npm -v

## 安装JDK

[JDK安装地址](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)

1. 下载jdk-8u341-linux-x64.tar.gz

2. 解压：tar -zxvf jdk-8u341-linux-x64.tar.gz -C /usr/local    //解压到当前目录下的/usr/local文件中

3. 配置环境变量：vim /etc/profile

   到底部添加：JAVA_HOME=/usr/local/jdk1.8.0_341

   PATH=$JAVA_HOME/bin:$PATH //美元是英文

4. 重新加载配置：source /etc/profile

5. 验证：java -version



## 安装Tomcat

[Tomcat下载地址](https://tomcat.apache.org/download-80.cgi) 选择后缀tar.gz

1. 下载tomcat

2. 解压：tar -zxvf apache-tomcat-8.5.83.tar.gz -C /usr/local

3. 进入到tomcat的bin目录下，运行startup.sh，命令为：sh startup.sh或./startup.sh

   cd /usr/local/apache-tomcat-8.5.83/bin/

验证Tomcat是否启动成功有两种方式：

1. 查看启动日志

   more /usr/local/apache-tomcat-8.5.83/logs/catalina.out //分屏查看

2. 查看进程：ps -ef|grep tomcat

停止Tomcat服务：

- 运行Tomcat的bin目录中的停止服务脚本文件：sh shutdown sh/./shutdown.sh
- 结束Tomcat进程：查看tomcat进程，获得进程id，执行命令结束进程：kill -9 进程id

kill命令是Linux提供的用于结束进程的命令，-9表示强制结束



## 安装MYSQL

1. 检测当前系统中是否安装MYSQL数据库

   rpm -qa：查询当前系统中安装的所有软件

   rpm -qa | grep mysql：查询当前系统中安装的名称带mysql的软件

   rpm -qa | grep mariadb：查询当前系统中安装的名称带mariadb的软件

   注意：如果当前系统中已经安装MYSQL数据库，安装将失败。Centos7自带mariadb，与MYSQL数据库冲突

2. 卸载已经安装的冲突软件

   rpm -e -modeps 软件名：卸载软件

   删除不掉用rpm -e 软件名 --nodeps

   rpm -e --nodeps mariadb-libs-5.5.68-1.el7.x86_64

3. 安装wget：yum -y install wget

4. 下载MySQL源并安装

    - wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
    - rpm -ivh mysql-community-release-el7-5.noarch.rpm

5. 正式安装MYSQL：yum install mysql-server

   或者是把4和5替换为

    - wget dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
    - yum install mysql-community-release-el6-5.noarch.rpm -y
    - yum install mysql-community-server -y

6. 启动MySQL：

    - systemctl status mysqld：查看mysql服务状态
    - systemctl start mysqld：启动mysql服务
    - systemctl enable mysqld：开机启动mysql服务
    - netstat -tunlp：查看已经启动的服务
    - netstat -tunlp | grep mysql：查找mysql服务
    - ps -ef | grep mysql：查看mysql进程

7. 登录MySQL数据库并修改密码

    - mysql -uroot -p：登录数据库，首次密码为空，直接回车

    - set global validate_password_length=4;

    - set global validate_password_policy=LOW;

    - 设置密码为1234：set password = password('1234');

8. 开启用户访问权限：grant all on *.* to 'root'@'%' identified by '1234';

   刷新权限：flush privileges;

9. 开放防火墙端口



## 安装lrzsz

1. 搜索lrzsz安装包：yum list lrzsz

2. 使用yum命令在线安装：yum install lrzsz.x86_64

   yum：是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖关系，并且一次安装所有依赖的软件包。

3. 安装以后使用rz命令：上传文件



## 安装Redis

[下载地址](https://download.redis.io/releases/)

1. 将Redis安装包上传到Linux
2. 解压安装包：tar -zxvf redis-4.0.0.tar.gz -C /usr/local
3. 安装Redis的依赖环境gcc，命令：yum install gcc-c++
4. 进入/usr/local/redis-4.0.0，进行编译，命令：make
5. 进入redis的src目录，进行安装，命令：make install

**Redis服务启动与停止**

- 在redis/src中启动：./redis-server(ctrl + c停止运行)

- 复制一个shell窗口，在redis/src目录下启动客户端：./redis-cli

- 修改配置文件使得redis后台运行：

  在redis目录下：vim redis.conf

  /dae：快速查找daemonize no

  daemonize no改为daemonize yes

- ps -ef | grep redis

- 添加配置文件参数并启动：src/redis-server ./redis.conf

**设置密码**

- 在redis目录下：vim redis.conf

- /requirepass：快速查找

- 打开/requirepass的注释，后边跟的是密码

- 添加配置文件参数并启动：src/redis-server ./redis.conf

- 在redis/src目录下启动客户端：./redis-cli -h localhost -p 6379

  -h：IP地址     -p：端口号

  在127.0.0.1:6379>后输入：auth 123456(密码)

  或者./redis-cli -h localhost -p 6379 -a 123456

**远程连接**

- 在配置文件中开启远程连接：vim redis.conf

  /bind：找到bind 127.0.0.1，注释掉

- 开放6379端口：firewall-cmd --zone=public --add-port=6379/tcp --permanent

  立即生效：firewall-cmd --reload

- 在windows的redis目录打开powershell(shift + 右键)

- 输入命令：./redis-cli.exe -h 107.182.19.176 -p 6379

  接着输入：auth 123456

# 项目部署

## 手工项目部署

1. 在IDEA中把SpringBoot项目打成jar包

2. 将jar包上传到Linux服务器

   mkdir /usr/local/app：创建目录，将项目jar包放在此处

3. 运行项目：java -jar jar包名称

4. 改为后台运行，并将日志输出到日志文件

   nohup命令：用于不挂断地运行指定命令，推出终端不会影响程序运行

   语法格式：nohup Command [Arg...] [&]

   参数说明：

    - Command：要执行的命令
    - Arg：一些参数，可以指定输出文件
    - &：让命令在后台运行

   举例：nohup java -jar jar包名称 &> tt.log &：后台运行java -jar命令，并将日志输出到tt.log文件

   nohup java -Xmx1024M -Xms1024M -jar server.jar

   nohup java -jar server.jar

5. 停止项目：

   找到进程：ps -ef | grep 'java -jar'

   杀死进程：kill -9 进程号

## 通过Shell脚本自动部署项目

本地开发环境 (push）==> Git仓库(pull) ==> Linux服务器（编译，打包，启动）

1. 在Linux安装Git

    - 列出git安装包：yum list git

    - 在线安装git：yum install git

    - 使用git克隆代码：cd /usr/local

      giit clone https://github.com/New-zxp/TT-.git

2. 在Linux安装maven

   https://maven.apache.org/download.cgi

   解压tar -zxvf apache-maven-3.8.6-bin.tar.gz -C/usr/local

   vim /usr/local/etc/profile：修改配置文件加入如下内容:

   **export** MAVEN_HOME=/usr/local/apache-maven-3.8.6

   **export** PATH=$PATH:$MAVEN_HOME/bin

   source /etc/profile：重新加载配置文件

   mvn -version：验证是否修改成功

   在local文件下创建repo文件：mkdir repo

   vi /usr/local/apache-maven-3.8.6/conf/settings.xml：修改配置文件内容

   <localRepository>/user/local/repo</localRepository>

3. 编写Shell脚本(拉取代码，编译，打包，启动)

   在local文件下创建脚本文件夹：mkdir sh

   进入sh文件，创建并编辑脚本文件：vim bootstart.sh

   ```sh
   #!/bin/sh
   echo =================================
   echo  自动化部署脚本启动
   echo =================================
    
   echo 停止原来运行中的工程
   APP_NAME=TT-0.0.1-SNAPSHOT.jar
    
   tpid=`ps -ef|grep $APP_NAME|grep -v grep|grep -v kill|awk '{print $2}'`
   if [ ${tpid} ]; then
       echo 'Stop Process...'
       kill -15 $tpid
   fi
   sleep 2
   tpid=`ps -ef|grep $APP_NAME|grep -v grep|grep -v kill|awk '{print $2}'`
   if [ ${tpid} ]; then
       echo 'Kill Process!'
       kill -9 $tpid
   else
       echo 'Stop Success!'
   fi
    
   echo 准备从Git仓库拉取最新代码
   cd /usr/local/TT
    
   echo 开始从Git仓库拉取最新代码
   git pull
   echo 代码拉取完成
    
   echo 开始打包
   output=`mvn clean package -Dmaven.test.skip=true`
    
   cd target
    
   echo 启动项目
   nohup java -jar TT-0.0.1-SNAPSHOT.jar &> tt.log &
   echo 项目启动完成
   ```

4. 为用户授予执行Shell脚本的权限

   chmod 777 bootstart.sh：为所有用户赋予读，写，执行权限

5. 执行Shell脚本

   sh bootstart.sh或者./bootstart.sh

6. 设置静态ip

   修改文件/etc/sysconfig/network-scripts/ifcfg-ens33(最后名字不一定)

   BOOTPROTO="static"

   IPADDR="" //需要和虚拟网络编辑器NAT模式的子网ip一样

   NETMASK="255.255.255.0"//子网掩码

   GATEWAY=""//需要和虚拟网络编辑器NAT模式的子网ip一样

   DNS1=""//需要和虚拟网络编辑器NAT模式的子网ip一样

7. 编辑完成后重新加载：systemctl restart network



# Mc

开放指定端口：firewall-cmd --zone=public --add-port=25565/tcp --permanent

firewall-cmd --reload

Linux screen命令用于多重视窗管理程序。
有了screen，我们就可以关闭链接让服务器一直运行了。
那么先安装screen

```c
sudo yum install java-1.8.0-openjdk
yum install -y screen
```

然后创建一个叫mc的终端，（个人理解和窗口差不多。）

```c
screen -S mc
```

在这个mc窗口内，打开我们的服务端

```c
sudo java -Xmx2G -jar fabric-server-mc.1.19-loader.0.14.10-launcher.0.11.1.jar nogui
```

ps -ef | grep ' -jar'

然后然后Ctrl+a 然后按 d 退出终端
现在就算关闭会话服务器也会一直运行了。
之后在想进入终端的时候只需：screen -ls

输入screen -r xxxx.mc进入终端

```
firewall-cmd --zone=public --add-port=25565/tcp --permanent
firewall-cmd --reload
```

```
force-gamemode 是否将玩家强制设置为某一模式

allow-nether 是否可以进入下界

gamemode 游戏模式

difficulty 难度

spawn-monsters 生成怪物

op-permission-level 管理员权限等级，控制台拥有最高权限，默认为4（最高）

pvp 玩家间是否可以互相伤害

level-type 生成世界类型 default默认 flat超平坦

hardcore 极限模式（s掉了就会被服务器封禁）

enable-command-block 是否允许使用命令方块

max-players 最大玩家数

rcon.port 远程访问端口

server-port 服务器端口

server-ip 服务器ip，默认为127.0.0.1（本机）

spawn-npcs 生成村民

level-world 世界名称，也就是世界数据存储的位置

online-mode 在线模式（true，需要通过官方正版验证，false，无需通过正版验证）

level-seed 种子

motd 服务器信息
```

/function rd_skyblock_is:cmd/start启动地图。

/scoreboard players set $Delay rd_skyblock_is XXX/ 更改XXX来改变刷新时间