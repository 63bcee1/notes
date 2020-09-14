# Linux 运维实操

## 前言

我在五个月前接触到运维这项工作，开始是抱着对Linux系统无限的崇敬去做这件事情的。由于我之前没有做过运维，对于Linux的理解也只是停留在是一个好用且神秘的操作系统之上。

五个月后，我意识到自己知识的匮乏，以及未来道路的遥远，回首望去，一路坎坷。

我发现那些经常需要用到的知识，总是记不住，每次都要查好久的资料，最后才解决，为了以后少走弯路，少浪费时间在重复的事情，我决定把之前的工作经验记录下来，以备日后只用。

总得来说，本书只会记录两样东西：

1，成功的实践

2，失败的经验



## ssh 最常用的远程登录服务

SSH提供两种身份验证方式，一种是基于口令，一种是基于密钥。

### 登录

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

### 配置文件

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

### 安装&更新

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

## telnet 备用的远程登录服务

telnet是TCP/IP协议族的一员，主要用于提供远程登录服务。

不过还有一个用处就是检测端口是否打开。

### 安装&更新

#### 检查

```shell
rpm -qa telnet-server
rpm -qa xinetd
```

这两个是一起的，后者是telnet的守护进程。

#### 安装

```shell
yum -y install telnet telnet-server xinetd
```

三个一起安装。

#### 设置开机自启动

```shell
systemctl enable xinetd.service

systemctl enable telnet.socket
```

#### 启动

```shell
systemctl start telnet.socket
systemctl start xinetd
```

#### 检查是否启动成功

```shell
# telnet默认使用23端口，通过查看端口状态方式
netstat -antupl | grep 23
# 通过控制台查看telnet进程
# systemctl status telnet.socket
```

#### 远程连接

```shell
# 不用指定端口
telnet 192.168.0.3
```

#### 测试端口

```shell
# 测试192.168.0.3 的3306端口
telnet 192.168.0.3 3306 
# 测试本地3306端口
telnet 127.0.0.1 3306
```

出现**Connected to 192.168.0.3**字样，则说的端口畅通，处于监听状态，反之则不通。

## telnet和ssh配合使用

之所以使用ssh，是因为ssh更加安全一些，毕竟名字就叫[Secure Shell](https://baike.baidu.com/item/Secure Shell)，还是要考虑安全因素。

另一个重要原因就是，telnet不安全。

所以，强烈建议，再万不得已的时候不要使用telnet。

常规的做法是，在ssh有可能不能用的时候，开启telnet，继续操作。

所以对于telnet，不要**开机自启**，需要使用的时候再开启。

我也就是再升级ssh的时候，或者相关组件，可能会影响ssh连接的时候，才会开启以作备用。

## ps 强大的进程状态查看工具

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

## top 实时显示系统信息

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

## mpstat 报告CPU统计信息

主要用于多CPU的环境下，还可以查看知道

```shell
# 查看所有CPU信息
mpstat -P ALL
# 查看第二个CPU信息
mpstat -P 1
```

## sar 性能监控好手

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

