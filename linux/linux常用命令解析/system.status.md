# 系统状态检测命令

###ifconfig

ifconfig 命令用于获取网卡配置与网络状态等信息，格式为“ifconfig [网络设备][参数]”。
使用 ifconfig 命令来查看本机当前的网卡配置与网络状态等信息时，其实主要查看的就
是网卡名称、inet 参数后面的 IP 地址、ether 参数后面的网卡物理地址（又称为 MAC 地址），
以及 RX、TX 的接收数据包与发送数据包的个数及累计流量（即下面加粗的信息内容）：

```
kimihiro@kimihiro-vmachine:~/桌面$ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:26:d7:b4:cd  txqueuelen 0  (以太网)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.11.129  netmask 255.255.255.0  broadcast 192.168.11.255
        inet6 fe80::bb31:d177:a6ec:8193  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:cc:9e:91  txqueuelen 1000  (以太网)
        RX packets 53714  bytes 57039226 (57.0 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 165620  bytes 40449219 (40.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (本地环回)
        RX packets 2393406  bytes 3610881288 (3.6 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2393406  bytes 3610881288 (3.6 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

kimihiro@kimihiro-vmachine:~/桌面$ 

```



###uname 命令



uname 命令用于查看系统内核与系统版本等信息，格式为“uname [-a]”。

在使用 uname 命令时，一般会固定搭配上-a 参数来完整地查看当前系统的内核名称、主
机名、内核发行版本、节点名、系统时间、硬件名称、硬件平台、处理器类型以及操作系统
名称等信息。

```
[root@linuxprobe ~]# uname -a 
Linux linuxprobe.com 3.10.0-123.el7.x86_64 #1 SMP Mon May 5 11:16:57 EDT 2017 
x86_64 x86_64 x86_64 GNU/Linux
```



###uptime命令

uptime 用于查看系统的负载信息，格式为 uptime。
uptime 命令真的很棒，它可以显示当前系统时间、系统已运行时间、启用终端数量以
及平均负载值等信息。平均负载值指的是系统在最近 1 分钟、5 分钟、15 分钟内的压力情
况（下面加粗的信息部分）；负载值越低越好，尽量不要长期超过 1，在生产环境中不要
超过 5。

```
[root@linuxprobe ~]# uptime 
22:49:55 up 10 min, 2 users, load average: 0.01, 0.19, 0.18
```



###free命令

free 用于显示当前系统中内存的使用量信息，格式为“free [-h]”。
为了保证 Linux 系统不会因资源耗尽而突然宕机，运维人员需要时刻关注内存的使用量。
在使用 free 命令时，可以结合使用-h 参数以更人性化的方式输出当前内存的实时使用量信息。

```
[root@linuxprobe ~]# free -h
```



###who命令

who 用于查看当前登入主机的用户终端信息，格式为“who [参数]”。
这三个简单的字母可以快速显示出所有正在登录本机的用户的名称以及他们正在开启的
终端信息

```
[root@linuxprobe ~]# who
```



###last命令

last 命令用于查看所有系统的登录记录，格式为“last [参数]”。



###history命令

history 命令用于显示历史执行过的命令，格式为“history [-c]”。
history 命令应该是作者最喜欢的命令。执行 history 命令能显示出当前用户在本地计算机
中执行过的最近 1000 条命令记录。如果觉得 1000 不够用，还可以自定义/etc/profile 文件中的
HISTSIZE 变量值。在使用 history 命令时，如果使用-c 参数则会清空所有的命令历史记录。
还可以使用“!编码数字”的方式来重复执行某一次的命令。

```
[root@linuxprobe ~]# history 
1 tar xzvf VMwareTools-9.9.0-2304977.tar.gz 
2 cd vmware-tools-distrib/ 
3 ls 
4 ./vmware-install.pl -d 
5 reboot 
6 df -h 
7 cd /run/media/ 
8 ls 
9 cd root/ 
10 ls 
11 cd VMware\ Tools/ 
12 ls 
13 cp VMwareTools-9.9.0-2304977.tar.gz /home 
14 cd /home 
15 ls
16 tar xzvf VMwareTools-9.9.0-2304977.tar.gz
17 cd vmware-tools-distrib/ 
18 ls 
19 ./vmware-install.pl -d 
20 reboot 
21 history 
[root@linuxprobe ~]# !15 
anaconda-ks.cfg Documents initial-setup-ks.cfg Pictures Templates 
Desktop Downloads Music Public Videos
```



历史命令会被保存到用户家目录中的.bash_history 文件中。Linux 系统中以点（.）开
头的文件均代表隐藏文件，这些文件大多数为系统服务文件，可以用 cat 命令查看其文件
内容。

```
[root@linuxprobe ~]# cat ~/.bash_history
```

要清空当前用户在本机上执行的 Linux 命令历史记录信息，可执行如下命令：

```
[root@linuxprobe ~]# history -c
```

