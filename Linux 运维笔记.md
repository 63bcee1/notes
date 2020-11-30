# Linux 运维笔记

## 前言

**本书所有实践都是基于Centos 7**

## 常用命令

```shell
# 打印当前目录 显示出当前工作目录的绝对路径
pwd:print work directory 
# 进程状态，类似于windows的任务管理器
ps: process status
# 显示磁盘可用空间数目信息及空间结点信息
df: disk free 

du: Disk usage

rpm：RedHat Package Management

dpkg：Debian package manager

# Debian或基于Debian的发行版中提供部分Linux命令缩写
apt：Advanced package tool
# 删除目录
rmdir：Remove Directory
# 删除目录或文件
rm：Remove
# 连锁
cat: concatenate 
# 载入模块
insmod: install module
# 创建一个软链接，相当于创建一个快捷方式
ln -s : link -soft 
# 创建目录
mkdir：Make Directory

touch

man: Manual

# 切换用户
su：Swith user

cd：Change directory

ls：List files

mkfs: Make file system

fsck：File system check

uname: Unix name

lsmod: List modules

mv: Move file

rm: Remove file

cp: Copy file

ln: Link files

fg: Foreground

bg: Background

chown: Change owner

chgrp: Change group

chmod: Change mode

umount: Unmount

# 磁带档案
tar：Tape archive 

ldd：List dynamic dependencies

insmod：Install module

rmmod：Remove module

```



## 文件后缀

> 文件结尾的"rc"（如.bashrc、.xinitrc等）：Resource configuration
>
> .a（扩展名a）：Archive，static library
>
> .so（扩展名so）：Shared object，dynamically linked library
>
> .o（扩展名o）：Object file，complied result of C/C++ source file

## 文件目录

> /dev = Devices (设备)
>
> /etc = Etcetera (等等)
>
> /lib = LIBrary
>
> /proc = Processes
>
> /sbin = Superuser Binaries (超级用户的二进制文件)
>
> /tmp = Temporary (临时)
>
> /usr = Unix Shared Resources
>
> /var = Variable (变量)

## ssh 配置与使用

SSH提供两种身份验证方式，一种是基于口令，一种是基于密钥。

登录

最常用的当然是用密钥方式登录。

root登录

```shell
	ssh root@192.168.0.3 
```

默认root登录

```shell
ssh 192.168.0.3	
```

指定端口登录（2022端口）

```shell
ssh -p 2022 192.168.0.3
```

基于密钥的方式，我基本没有用过，所以暂时不记录。

配置文件

ssh 的配置文件是：**/etc/ssh/sshd_config**

具体配置项：

```shell
# 指定监听端口
Port 2022
# 允许远程root登录
PermitRootLogin yes
# 指定公钥数据库文件
AuthorsizedKeysFile .ssh/authorized_keys
# 启用密码验证
PasswordAuthentication  yes
# 启用密钥验证
PubkeyAuthentication  yes
# 最大会话数量（同时允许几个终端登录）
MaxSessions 2
# 最大尝试次数（密码输错次数）
MaxAuthTries 3
# 不使用pam模块认证
UsePAM no
```

安装&更新

linux系统一般都自带ssh，但是可能版本比较老，需要更新，要更新的话，只能采用源码编译的方式进行安装。

首先到openssh官网下载最新的源码包。

解压后，直接./configure，检查配置。

make，编译。

make install，安装。

systemctl restart sshd，重启。

中途可能出现的问题：

1，configure: error: no acceptable C compiler found in $PATH

```shell
yum install -y gcc
```

2，configure: error: *** zlib.h missing – please install first or check config.log

```shell
yum install -y zlib-devel
```

3，configure: error: *** OpenSSL headers missing – please install first or check config.log

```shell
yum install -y openssl-devel
```

以上一centos为例，其他系统大同小异。

## telnet 配置与使用

telnet是TCP/IP协议族的一员，主要用于提供远程登录服务。

不过还有一个用处就是检测端口是否打开。

安装&更新

检查

```shell
rpm -qa telnet-server
rpm -qa xinetd
```

这两个是一起的，后者是telnet的守护进程。

安装

```shell
yum -y install telnet telnet-server xinetd
```

三个一起安装。

设置开机自启动

```shell
systemctl enable xinetd.service

systemctl enable telnet.socket
```

启动

```shell
systemctl start telnet.socket
systemctl start xinetd
```

检查是否启动成功

```shell
# telnet默认使用23端口，通过查看端口状态方式
netstat -antupl | grep 23
# 通过控制台查看telnet进程
# systemctl status telnet.socket
```

远程连接

```shell
# 不用指定端口
telnet 192.168.0.3
```

测试端口

```shell
# 测试192.168.0.3 的3306端口
telnet 192.168.0.3 3306 
# 测试本地3306端口
telnet 127.0.0.1 3306
```

出现**Connected to 192.168.0.3**字样，则说的端口畅通，处于监听状态，反之则不通。

## telnet和ssh 配合使用

之所以使用ssh，是因为ssh更加安全一些，毕竟名字就叫[Secure Shell](https://baike.baidu.com/item/Secure Shell)，还是要考虑安全因素。

另一个重要原因就是，telnet不安全。

所以，强烈建议，再万不得已的时候不要使用telnet。

常规的做法是，在ssh有可能不能用的时候，开启telnet，继续操作。

所以对于telnet，不要**开机自启**，需要使用的时候再开启。

我也就是再升级ssh的时候，或者相关组件，可能会影响ssh连接的时候，才会开启以作备用。

## ps 常用命令

ps这个工具有多强大呢？从参数数量上就可以看出来。

不过常用的也就几个。

```shell
# 显示进程，连同命令行，经常和grep一起使用，筛选进程。
ps -ef
```

```shell
# 可以显示进程树，看到进程间的父子关系，我喜欢用。
ps -fax
```

```shell
# 列出当前内存中的所有程序
ps -aux
```

```shell
# 查看以root身份运行的进程
ps -u root
```

大致就是这些，ps只是当前系统的快照信息，不具有实时性。

要有实时性，当然还是**top**命令。

## top 常用命令

top可以查看系统的运行情况，如果是巡检的话，首先就是top命令一下，看看系统的运行是否正常。

**第一行，队列信息**

> 系统时间：07:27:05
>
> 运行时间：up 1:57 min,
>
> 当前登录用户： 3 user
>
> 负载均衡(uptime) load average: 0.00, 0.00, 0.00
>
>    average后面的三个数分别是1分钟、5分钟、15分钟的负载情况。
>
> load average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了

**第二行，任务信息**

> 总进程:150 total, 运行:1 running, 休眠:149 sleeping, 停止: 0 stopped, 僵尸进程: 0 zombie

**第三行，cpu状态信息**

> 0.0%us【user space】— 用户空间占用CPU的百分比。
>
> 0.3%sy【sysctl】— 内核空间占用CPU的百分比。
>
> 0.0%ni【】— 改变过优先级的进程占用CPU的百分比
>
> 99.7%id【idolt】— 空闲CPU百分比
>
> 0.0%wa【wait】— IO等待占用CPU的百分比
>
> 0.0%hi【Hardware IRQ】— 硬中断占用CPU的百分比
>
> 0.0%si【Software Interrupts】— 软中断占用CPU的百分比

**第四行,内存状态**

>  1003020k total,  234464k used,  777824k free,  24084k buffers【缓存的内存量】

**第五行，swap交换分区信息**

> 2031612k total,   536k used, 2031076k free,  505864k cached【缓冲的交换区总量】

**第七行以下：各进程（任务）的状态监控**

> PID — 进程id
> USER — 进程所有者
> PR — 进程优先级
> NI — nice值。负值表示高优先级，正值表示低优先级
> VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
> RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
> SHR — 共享内存大小，单位kb
> S —进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
> %CPU — 上次更新到现在的CPU时间占用百分比
> %MEM — 进程使用的物理内存百分比
> TIME+ — 进程使用的CPU时间总计，单位1/100秒
> COMMAND — 进程名称（命令名/命令行）

## mpstat 常用命令

主要用于多CPU的环境下，还可以查看知道

```shell
# 查看所有CPU信息
mpstat -P ALL
# 查看第二个CPU信息
mpstat -P 1
```

## sar 常用命令

```shell
# 查看平均负载
sar -q
# 查看内存使用情况
sar -r
# 查看系统swap分区统计情况
# pswpin/s    每秒从交换分区到系统的交换页面（swap page）数量
# pswpott/s   每秒从系统交换到swap的交换页面（swap page）的数量
sar -W
# 查看IO和传递速率
# tps  磁盘每秒钟的IO总数，等于iostat中的tps
# rtps  每秒钟从磁盘读取的IO总数
# wtps  每秒钟从写入到磁盘的IO总数
# bread/s  每秒钟从磁盘读取的块总数
# bwrtn/s  每秒钟此写入到磁盘的块总数
sar -b
# 查看磁盘使用情况
# DEV  磁盘设备的名称
# tps  每秒I/O的传输总数
# rd_sec/s  每秒读取的扇区的总数
# wr_sec/s  每秒写入的扇区的总数
# avgrq-sz  平均每次次磁盘I/O操作的数据大小（扇区）
# avgqu-sz  磁盘请求队列的平均长度
# await  从请求磁盘操作到系统完成处理，每次请求的平均消耗时间，包括请求队列等待时间，单位是毫秒（1秒等于1000毫秒），等于寻道时间+队列时间+服务时间
# svctm  I/O的服务处理时间，即不包括请求队列中的时间
# %util  I/O请求占用的CPU百分比，值越高，说明I/O越慢
sar -d
# 查看网络信息
# 网络接口信息
sar -n DEV 
# 查看socket连接信息
sar -n SOCK
# 查看TCP连接
sar -n TCP
```



## firewall 常用配置

linux centos 有三种防火墙，最常用的还是firewalld

复杂的概念就不说了，直接上最常用的命令。

安装

```shell
yum install firewalld firewall-config
```

开启

```shell
systemctl start firewalld
```

开机自启

```shell
systemctl enable firewalld
```

取消开机自启

```shell
systemctl disable firewalld
```

状态查看

```shell
systemctl status firewalld
```

重新加载（修改配置后，使配置生效）

```shell
firewall-cmd --reload
```

添加放行端口

```shell
# 放行2022端口，--permanent使配置在重启后不会消失
firewall-cmd --zone=public --add-port=2022/tcp --permanent
```

移除放行端口

```shell
# 移除2022端口放行，之后外网不能访问2022端口，add改成remove就可以
firewall-cmd --zone=public --remove-port=2022/tcp --permanent
```

查询放行端口列表（public空间下）

```shell
firewall-cmd --zone=public --list-ports
```

添加端口转发

```shell
# 将本地12345端口，转发到局域网192.168.0.8的27017端口，通常用于外网访问内网
firewall-cmd --add-forward-port=port=12345:proto=tcp:toport=27017:toaddr=192.168.0.8 --permanent
```

删除端口转发

```shell
# 移除本地12345到内网192.168.0.8的27017端口转发，remove与add相对应
firewall-cmd --remove-forward-port=port=12345:proto=tcp:toaddr=192.168.0.8:toport=27017 --permanent
```

指定端口访问策略

```shell
# 只允许192.168.0.0的局域网访问本机的23端口，相当于白名单
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.0.0/24" port protocol="tcp" port="23" accept"
# 拒绝来自局域网对23的访问,相当于白名单
firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.0.0/24" port protocol="tcp" port="23" reject"
```

主机访问策略

```shell
# 允许192.168.0.14的ipv4访问
firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address=192.168.0.14 accept'
# 拒绝192.168.0.14的ipv4访问
firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address=192.168.0.14 reject'
```

查看防火墙的所有区域设置（端口，策略，服务）

```shell
 firewall-cmd --list-all
```

## tar 常用命令

tar 是linux中使用最多的压缩与解压工具，当然还有其他的，不过都是小弟。

常见的压缩文件格式：

`*.Z`: compress程序压缩文件，目前使用较少。已经有gzip替换了。

`*.gz`: gzip程序压缩的文件。

`*.bz2`:bzip2程序压缩的文件，比gzip的压缩比更好。



采用gzip方式压缩

```shell
# 先指定压缩结果文件，再指定需要压缩的路径或文件
tar -zpcv -f /root/etc.tar.gz /etc
```

采用bzip2方式压缩

```shell
# 先指定压缩结果文件，再指定需要压缩的路径或文件
tar -jpcv -f /root/etc.tar.bz2 /etc
```

解压bz2文件

```shell
tar -jxv -f /root/etc.tar.bz2
```

解压到指定目录

```shell
# -C 指定tmp目录
tar -jxv -f /root/etc.tar.bz2 -C /tmp
```

zip压缩文件

```shell
# -r 递归打包，压缩文件夹的时候使用
zip -r  etc.zip /root/etc	
```

zip解压

```shell
zip etc.zip
```

tar创建压缩文件

```shell
tar -czvf etc.tar.gz /root/etc	
```

tar解压压缩文件

```shell
tar -xzvf etc.tar.gz
```

## rz&sz 常用命令

常用的工具是lrzsz

安装

```shell
yum yum install -y lrzsz
```

上传

```shell
# 上传文件，会打开一个弹窗，直接选择文件，可以多选
rz 
```

下载

```shell
# 下载文件，会打开一个弹窗，选择文件的下载路径，可以多选
sz 文件名
```

## scp 常用命令

一般是是指局域网内文件传输，同为linux，并且都运行着ssh服务才行。

本机传输**文件**到另一台服务器

```shell
# 将jdk传输到192.168.0.10的root目录下，一定要指定具体目录，否则传输失败
scp jdk-8u251-linux-x64.tar root@192.168.0.10:/root
```

本机传输**文件**到另一台服务器（指定端口）

```shell
# 因为192.168.0.10上ssh是监听2022端口，所以要指定端口
scp -P 2022 jdk-8u251-linux-x64.tar root@192.168.0.10:/root
```

复制**目录**到另一台服务器

```shell
scp -r /root/etc root@@192.168.0.10:/root/etc
```

复制**目录**远程到本地

```shell
scp -r root@192.168.0.10:/root/etc /root/etc
```

复制**文件**远程到本地

```shell
scp root@192.168.0.10:/root/etc/etc.tar.gz /root/etc
```

**如果远程ssh有特定端口，需要使用-P来指定端口。**

**如果复制文件夹，需要使用-r来做递归处理。**

## rsync 常用命令

最常用的文件同步工具，使用起来相当方便

主机间同步

```shell
#如果没有开启默认的22端口，指定ssh端口同步
# 加上/ 表示将目录下的文件同步过去
rsync -av -e 'ssh -p 2022' /root/work/backup/database/ 192.168.0.16:/root/work/temp
# 不加/ 表示同步整个目录，会多一个目录层级
rsync -av -e 'ssh -p 2022' /root/work/backup/database 192.168.0.16:/root/work/temp
```

本地同步

```shell
# 加/与不加/的效果同上
rsync -av temp/ dest/
```

## JDK 安装配置

相比windows，linux上安装配置jdk不要太简单，这也是我渐渐不喜欢windos的原因之一。

检查当前jdk是否安装及版本情况

```shell
java -version
```

```shell
rpm -qa | grep java
```

卸载已有jdk

```shell
# 后面加所有rpm -qa | grep java的结果
rpm -e --nodeps *
```

下载

**Oracle官网或者自己搜**

安装

```shell
# 假设下载下来的是jdk-8u251-linux-x64.tar.gz一个安装包
tar -xzvf jdk-8u251-linux-x64.tar.gz
# 改文件夹名字
mv jdk-8u251-linux-x64 jdk
# 改位置
mv jdk /usr/local
```

配置

```shell
编辑配置文件
vim /etc/profile
# 在文件最后加上下面几句，然后保存
export JAVA_HOME=/usr/local/jdk
export JAVA_BIN=$JAVA_HOME/bin
export JAVA_LIB=$JAVA_HOME/lib
export CLASSPATH=.:$JAVA_LIB/tools,jar:$JAVA_LIB/dt.jar
export PATH=$JAVA_BIN:$PATH
# 使配置生效
source /etc/profile
```

验证

```shell
java -version
```

输出下载的版本，说明配置成功。

## Nginx 使用

这东西是目前主流的web服务器，资料一搜一大堆，我只记录自己用的最多的。

安装配置

> 这是下载地址：
>
> http://nginx.org/en/download.html 
>
> 下载完之后，就在这样的一个安装包：
>
> nginx-1.18.0.tar.gz

直接解压

```shell
tar -xzvf nginx-1.18.0.tar.gz
```

配置

```shell
cd nginx-1.18.0
# 这个是使用默认的，什么参数也不加
./configure 
# 中途有抱什么错误，直接把错误第一行，复制粘贴到搜索框，一搜就能找到解决办法，多半是确实某个组件
```

安装

```shell
make 
make install
# 这两步可以一起进行
make && make install
```

安装完毕之后，就可以在/usr/local下面看到nginx的文件夹

> conf 目录，配置文集存放的目录
>
> sbin目录，存放可运行文件，也就一个nginx可运行文件
>
> html目录，nginx默认的存放html文件的目录
>
> logs目录，里面存着nginx的日志，通过的请求记录access,log，错误日志error.log
>
> 主要就这几个，其他的暂时用不到。

部署项目

> 一般是前端项目，也就是html页面，css文件，js文件，图片和其他的东西。
>
> 在conf目录下，编辑nginx.conf文件，nginx默认使用这个文件，在不指定配置文件的情况下。
>
> ```shell
> # 在http{}中加入一个服务，nginx认大括号，不用特意对齐，括号对应就行。
> # 简单开启一个服务，端口8088，根目录是 /usr/local/nginx/h5/，index文件是index.html
> # 本机的8088端口所有请求都会跳转到 /usr/local/nginx/h5/下
> server {
>         listen       8088;
>         server_name localhost;
>       
>         location / {
>             root   /usr/local/nginx/h5/;
>             index  index.html;
>         }
>        } 
> 
> ```

反向代理

```shell
# 代理转发
# 在http{}中开启一个服务，监听8888
# 所有访问本机8888端口的流量都会被代理到内网192.168.0.17:8888端口
server {
           listen      8888;
           server_name localhost;
          location / {
                   proxy_pass              http://192.168.0.17:8888;
           }
          }
```

根据路径代理

```shell
# 监听8088端口
# 路径中/resources请求，直接访问本地的/opt/upFiles目录
# 路径中/pass请求，直接代理带内网的8888端口
server {
        listen       8088;
        server_name  localhost;

        location / {
            root   /usr/local/nginx/h5/;
            index  index.html;
        }

        location /resources {
            root   /opt/upFiles;
                  }
        location ^~/pass {
            proxy_pass http://192.168.0.17:8888;

         }
        }
```

支持https

```shell
 # 支持https
 # 需要证书和密钥
 # 需要在端口后面加上ssl
 # 需要后面的一大堆设置，看不懂直接复制粘贴
 server {
           listen      8448 ssl;
           server_name 127.0.0.1;
           ssl_certificate "ssl.crt";
           ssl_certificate_key "ssl.key";
           ssl_session_cache shared:SSL:1m;
           ssl_session_timeout  5m;
           ssl_protocols SSLv2 SSLv3 TLSv1;
           ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
           ssl_prefer_server_ciphers on;
            location / {
            root   /usr/local/nginx/h5/;
            index  index.html;
        }
           } 
```

端口转发

```shell
# 这个stream和http是平级的关系
# 这里开启一个服务，监听8440端口，代理到cloudsocket10对应的服务器地址和端口
# 每做一个端口的转发，都需要一个server开启服务，一个upstream。
stream {
    upstream cloudsocket10 {
       hash $remote_addr consistent;
       server 192.168.0.10:3306 weight=5 max_fails=3 fail_timeout=30s;
    }
   
    server {
       listen 8440;
       proxy_connect_timeout 10s;
       proxy_timeout 300s;
       proxy_pass cloudsocket10;
  }
}
```

升级和模块支持

> https需要ssl模块的支持，因此需要对nginx添加模块支持
>
> 找到nginx源码目录
>
> 假定是nginx-1.18.0
>
> ```shell
> # 意思是加上ssl模块
> ./configure --with-http_ssl_module
> # 编译
> make
> # 编译完成之后，在nginx的objs目录下，会有一个新的nginx运行文件，直接复制到/usr/local/nginx/sbin就行了
> # 再次之前，建议对原来的nginx做个备份
> mv /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak
> 
> ```

配置文件说明

```shell
# 指定nginx的运行用户
user  root;
# 指定nginx进程数量，应该是小于等于cpu数量
worker_processse 1;
# 每个进程可接收的最大连接数量
worker_connections  1024;

```

常用命令

``` shell
# 切换到nginx安装目录
cd /usr/local/nginx/sbin
# 运行nginx 默认使用nginx.conf作为配置文件shell
./nginx
# 指定配置文件，启动nginx
./nginx -c /usr/local/nginx/conf/nginx.conf.bak
# 重新加载配置文件，默认是nginx.conf
./nginx -s reload
# 快速停止nginx进程
./nginx -s stop 
# 正常停止nginx
./nginx -s quit
# 检测配置文件，默认是nginx.conf
nginx -t
```

路径匹配规则

> ​	基础语法
>
> - =     严格匹配。如果请求匹配这个location，那么将停止搜索并立即处理此请求
> - ~     区分大小写匹配(可用正则表达式)
> - ~*    不区分大小写匹配(可用正则表达式)
> - !~    区分大小写不匹配
> - !~*   不区分大小写不匹配
> - ^~    如果把这个前缀用于一个常规字符串,那么告诉nginx 如果路径匹配那么不测试正则表达式

设置文件上传最大值

```shell
# 写在server里，表示对这个server生效
# 写在http里面，表示对http里的所有server生效
# 不用加mb，这个的意思是最大限制20MB
client_max_body_size   20m;
```

## Redis 安装配置

下载

```shell
# 下载地址，因为6.0的版本还不稳定，所以还是用5.0的版本
wget http://download.redis.io/releases/redis-5.0.4.tar.gz
```

解压

```shell
tar -xzvf redis-5.0.4.tar.gz
```

编译

```shell
cd redis-5.0.4
make
```

安装

```shell
# 这个一路点enter就可以了，就是确认配置文件的一些配置项
# 这个运行结束，出现successfully的字样，就算完了
# 安装完之后会自动运行
cd utils/
./install_server.sh
```

验证

```shell
# 有结果，
netstat -tnulp|grep redis
# 出现下面的结果，说明redis安装成功，而且已经在运行了
tcp        0      0 0.0.0.0:6379            0.0.0.0:*               LISTEN      11765/./redis-serve
```

## Mysql 安装配置

安装

```shell
# 下载rpm文件
wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
# 运行rpm文件
rpm -ivh mysql57-community-release-el7-9.noarch.rpm
# 安装
yum install mysql-server
```

这是最便捷的方法，也能避免很多问题。

启动&关闭

```shell
#启动
systemctl start mysqld
#关闭
systemctl stop mysqld		
```

配置文件

```shell
# 默认路径
/etc/my.cnf 
```

主从备份

```shell
# 至少需要两台安装有mysql的服务器
# 版本应该尽量相近
# 下面是详细步骤：
1）编辑主节点数据库配置文件my.cnf：
[mysqld]
log-bin=mysql-bin-master          #启用二进制日志
server-id=8                        #本机数据库ID 标示
2）重启主数据库：
systemc restart mysqld
3）登录数据库，创建备份用户及授权：
创建用户：CREATE USER 'username'@'192.168.0.10' IDENTIFIED BY 'password';
授权：GRANT REPLICATION SLAVE ON *.* to ‘username’@‘192.168.0.10’ identified by ‘password’;
4）查看主数据库状态：
show master status;
记录下File和Position
5）编辑从数据库配置文件my.cnf：
[mysqld]
log-bin=mysql-bin-slave          #启用二进制日志
server-id=10
6）重启从数据库：
systemc restart mysqld
7）登录从数据库，切换主数据库源：
change master to master_host='192.168.0.8',master_user='username',master_password='password',master_log_file='mysql-bin.000002',master_log_pos=263;
# master_log_file 填写之前记录的File，
# master_log_pos填写之前记录的Position
8）启动slave同步：
start slave;
9）查看同步状态：
show slave status;
如果没有出现error字段，则说明主从备份设置成功。
```

数据导出

```shell
# -u指定用户名 -p指定密码 -h指定地址，不要-h默认是127.0.0.1 --databases 指定数据库，可以同时导出多个数据库
mysqldump -uroot -p123456 -h127.0.0.1 --databases temp  >temp.sql
```

数据恢复

```shell
# 登录数据库
mysql -uroot -p123456
# 加载sql语句，使用绝对路径
resource snms_2020-07-07.sql;
```

## XtraBackup 备份与恢复

下载

```shell
# mysql 8.0 及更高版本
wget https://www.percona.com/downloads/Percona-XtraBackup-LATEST/Percona-XtraBackup-8.0.14/binary/redhat/7/x86_64/percona-xtrabackup-80-8.0.14-1.el7.x86_64.rpm
# mysql 8.0 以下版本
wget https://downloads.percona.com/downloads/Percona-XtraBackup-2.4/Percona-XtraBackup-2.4.9/binary/redhat/7/x86_64/percona-xtrabackup-24-2.4.9-1.el7.x86_64.rpm
--2020-11-30 09:00:14--  https://downloads.percona.com/downloads/Percona-XtraBackup-2.4/Percona-XtraBackup-2.4.9/binary/redhat/7/x86_64/percona-xtrabackup-24-2.4.9-1.el7.x86_64.rpm
```

安装

```
yum install percona-xtrabackup-80-8.0.14-1.el7.x86_64.rpm
```

全量备份

```
innobackupex --user=user --password=password --databases=test dir
```

**需要创建dir目录**

增量备份

```shell
innobackupex --user=user --password=password --databases=test --incremental innobackinreament/ --incremental-basedir=innoback/2020-11-17_16-31-33/
```

--**incremental 后面跟增量备份的目录 需要提前创建**

**--incremental-basedir=后面跟基于哪个备份结果进行增量**

数据恢复

```shell
 innobackupex --user=root --copy-back /data/back_data/2019-03-22_14-21-54/
```

## Vmware 扩容

1，关闭虚拟机

2，虚拟机-设置-扩展磁盘容量，选择扩容大小

3，重启虚拟机

4，查看磁盘分区

```shell
fdisk -l
```

5，分区

```shell
fdisk /dev/sda
n 新增一个分区
p 普通分区类型
分区号默认
最后w，保存退出
```

6，格式化分区

```shell
mkfs.xfs /dev/sda3
```

7，挂载分区

```shell
 mount /dev/sda3 /mnt/
```

8，查看磁盘

```shell
df -lh
# 会多出一个刚才挂在的分区
```

## Mongodb 安装配置

安装

```shell
# 添加yum源：
vim /etc/yum.repos.d/mongodb-org-4.0.repo
# 编辑内容：
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
# 使用yum安装：
yum install -y mongodb-org
```

```shell
# 下载
wget https://repo.mongodb.org/yum/redhat/7/mongodb-org/4.4/x86_64/RPMS/mongodb-org-server-4.4.1-1.el7.x86_64.rpm
# 安装
rpm -ivh mongodb-org-server-4.4.1-1.el7.x86_64.rpm
```

关闭&启动

```shell
#启动
systemctl status mongod	
#关闭
systemctl stop mongod
```

配置文件

```shell
# 默认配置文件路经
/etc/mongod.conf
```

**最新版本的mongodb采用yaml文件格式进行配置，需要注意的是每个:后面必须有一个空格，再跟上值，不然会报错，无法启动。**

数据备份

```shell
# 指定用户名 密码 数据 导出目录
mongodump --username=admin --password=123456 --db temp -o directory
```

副本集

```shell
# 配置
config = { _id:"mongoback", members:[{_id:0,host:"192.168.0.8:27017"},{_id:1,host:"192.168.0.10:27017"}]}
```

## ntp 安装使用

检查ntp是否安装

```shell
rpm -q ntp
```

安装ntp服务

```shell
yum -y install ntp
```

启动

```shell
systemctl start ntpd
```

开机启动

```shell
systemctl enable ntpd
```

查看网络中的ntp服务

```shell
ntpq -p
```

查看时间同步状态

```shell
ntpstat
```

同步时间

```shell
ntpdate -u cn.pool.ntp.org
```

```
ntpdate asia.pool.ntp.org
```

**两种写法都可以，后面跟不同的时间服务器地址**

## Zabbix 安装配置

安装lamp

```shell
yum install -y httpd  php php-mysql php-gd libjpeg* php-ldap php-odbc php-pear php-xml php-xmlrpc php-mhash
```

安装开发工具

```shell
yum groups install "Development Tools"
```

安装zabbix

```shell

rpm -ivh https://mirrors.huaweicloud.com/zabbix/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
```

安装server和agent

```shell
yum install zabbix-server-mysql zabbix-agent -y
```

启用Red Hat软件集合

```shell
yum install centos-release-scl -y
```

安装zabbix前端

```shell
yum install -y zabbix-web-mysql-scl zabbix-apache-conf-scl
```

安装mysql数据库

```shell
yum -y install mariadb-server mariadb
```

启动数据库

```shell
systemctl start mariadb&&systemctl enable mariadb
```

创建数据库和授权

```sql
create database zabbix character set utf8 collate utf8_bin;
create user zabbix@localhost identified by 'zabbix123';
grant all privileges on zabbix.* to zabbix@localhost;
```

导入初始架构和数据

```shell
zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -u zabbix -p zabbix123
```

修改时区

```shell
vim /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf
php_value[date.timezone] = Asia/Shanghai
```

修改端口

```shell
vim /etc/httpd/conf/httpd.conf
Listen 8090
```

启动服务

```shell
systemctl restart zabbix-server zabbix-agent httpd rh-php72-php-fpm
```

设置自启动

```
systemctl enable zabbix-server zabbix-agent httpd rh-php72-php-fpm
```

访问链接

```
http://IP:PORT/zabbix
```

一直点下一步就行

给要监控的主机安装agent

```shell
rpm -ivh https://mirrors.huaweicloud.com/zabbix/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
yum install zabbix-agent -y
```

编辑agent配置文件

```shell
Server=127.0.0.1	# 写server服务所在地址
ServerActive=127.0.0.1	# 写server服务所在地址
Hostname=web	# 写一个服务器的别名，前端配置的时候需要和这个名字一样
```

启动agent

```
systemctl start zabbix-agent
```

设置自启动agent

```
systemctl enable zabbix-agent
```

## Python3 安装配置

### 第一种方法：

下载（版本自选）

```shell
wget http://npm.taobao.org/mirrors/python/3.7.4/Python-3.7.4.tgz
```

解压

```shell
tar -zxvf Python-3.7.4.tgz
```

配置检查

```shell
cd Python-3.7.4
./configure
```

编译&安装

```
make&&make install
```

验证

```
python3
pip3 list
```

换源

```shell
# 找到配置文件的位置
find / -name pip.conf
# 编辑配置文件
vim /root/.pip/pip.conf
# 替换以前的源
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host = mirrors.aliyun.com
```

### 第二种方法：

安装相关依赖

```shell
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```

安装扩展源

```shell
yum -y install epel-release
```

安装pip

```shell

yum install python-pip
```

下载安装包

```
wget http://npm.taobao.org/mirrors/python/3.7.4/Python-3.7.4.tar.xz
```

解压安装

```shell
xz -d Python-3.7.4rc2.tar.xz
tar -xf Python-3.7.4rc2.tar
cd Python-3.7.4rc2
./configure prefix=/usr/local/python3
make && make install
```

**以上，一条一条运行**

遇到问题：

> ModuleNotFoundError: No module named '_ctypes' make: *** [install] 错误 1

解决办法：

> yum -y install libffi-devel

创建软连接：

```shell
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3

```



## ElasticSearch 安装

下载

```shell
wget https://mirrors.huaweicloud.com/elasticsearch/7.9.0/elasticsearch-7.9.0-x86_64.rpm
```

安装

```
yum install elasticsearch-7.9.0-x86_64.rpm
```

切换用户

```
su elsearch
```

运行

```shell
cd /opt/elasticsearch/bin
# 启动
./elasticsearch
# 后台启动
./elasticsearch -d
```

## kibana 安装

下载

```shell
wget https://mirrors.huaweicloud.com/kibana/7.8.0/kibana-7.8.0-x86_64.rpm
```

