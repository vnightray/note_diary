# 常用命令

1. echo

echo 命令用于在终端输出字符串或变量提取后的值，格式为“echo [字符串 | $变量]” 

```
[root@linuxprobe ~]# echo Linuxprobe.Com
Linuxprobe.Com
```

```
[root@linuxprobe ~]# echo $SHELL
/bin/bash
```



2. date

date 命令用于显示及设置系统的时间或日期，格式为“date [选项][指定的格式] ”。

| 参数 |      作用      |
| :--: | :------------: |
|  %t  |  跳格[Tab 键]  |
|  %H  | 小时（00～23） |
|  %I  | 小时（00～12） |
|  %M  | 分钟（00～59） |
|  %S  |  秒（00～59）  |
|  %j  | 今年中的第几天 |

```
[root@linuxprobe ~]# date 
Mon Aug 24 16:11:23 CST 2017
[root@linuxprobe ~]# date "+%Y-%m-%d %H:%M:%S" 
2017-08-24 16:29:12
[root@linuxprobe ~]# date -s "20170901 8:30:00" 
Fri Sep 1 08:30:00 CST 2017
[root@linuxprobe ~]# date "+%j" 
244
```





3. reboot 

reboot 命令用于重启系统

```
[root@linuxprobe ~]# reboot
```



4. poweroff

poweroff 命令用于关闭系统

```
[root@linuxprobe ~]# poweroff
```



5. wget 命令 

wget 命令用于在终端中下载网络文件，格式为“wget [参数] 下载地址”。



![1633786586787](D:\often-used materials\学习笔记\linux\pict\1633786586787.png)



6. ps命令

ps 命令用于查看系统中的进程状态，格式为“ps [参数]”。

![1633786659452](D:\often-used materials\学习笔记\linux\pict\1633786659452.png)



7. top命令

top 命令用于动态地监视进程活动与系统负载等信息，其格式为 top。



8. pidof命令

pidof 命令用于查询某个指定服务进程的 PID 值，格式为“pidof [参数][服务名称]”。

```
[root@linuxprobe ~]# pidof sshd 
2156
```



9. kill命令

kill 命令用于终止某个指定 PID 的服务进程，格式为“kill [参数][进程 PID]”。

```
[root@linuxprobe ~]# kill 2156
```



10. killall命令

killall 命令用于终止某个指定名称的服务所对应的全部进程，格式为：“killall [参数][进程名称]”。

```
[root@linuxprobe ~]# pidof httpd 
13581 13580 13579 13578 13577 13576 
[root@linuxprobe ~]# killall httpd 
[root@linuxprobe ~]# pidof httpd 
[root@linuxprobe ~]#
```



