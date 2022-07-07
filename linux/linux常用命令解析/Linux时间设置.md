# Linux时间设置

1. date

```
语法格式：date [-u][-d datestr] [-s datestr][--utc] [--universal][--date=datestr] [--set=datestr][--help] [--version][+FORMAT] [MMDDhhmm[[CC]YY][.ss]]
```

说明：可用来设置系统日期与时间。只有管理员才有设置日期与时间的权限，一般用户只能用
date 命令显示时间。若不加任何参数，data 会显示目前的日期与时间。

```
例 1：显示当前系统时间
[root@Test2 ~]# date
2010 年 06 月 17 日 星期四 00:00:04 CST
例 2：设置日期和时间为 2010 年 6 月 18 号 12:00
[root@Test2 ~]# date -s "20100618 12:00:00"
2010 年 06 月 18 日 星期五 12:00:00 CST
例 3：设置日期为 2010 年年 6 月 18 号
[root@Test2 ~]# date -s 20100618
2010 年 06 月 18 日 星期五 00:00:00 CST
例 4：设置时间为 12:00:00
[root@Test2 ~]# date 12:00:00
date: invalid date “12:00:00”
例 5：显示时区
[root@Test2 ~]# date -R
Thu, 17 Jun 2010 00:01:36 +0800
或者：
[root@Test2 ~]# cat /etc/sysconfig/clock 
# The ZONE parameter is only evaluated by system-config-date.
# The timezone of the system is defined by the contents of /etc/localtime.
ZONE="Asia/Shanghai"
UTC=true
ARC=false
```



2. hwclock/clock

```
语法格式：hwclock [--adjust][--debug][--directisa][--hctosys][--show][--systohc][--test]
[--utc][--version][--set --date=<日期与时间>]
```

```
参数：
--adjust hwclock 每次更改硬件时钟时，都会记录在/etc/adjtime 文件中。使用--adjust 参数，可使 hwclock
根据先前的记录来估算硬件时钟的偏差，并用来校正目前的硬件时钟。
--debug 显示 hwclock 执行时详细的信息。
--directisa hwclock 预设从/dev/rtc 设备来存取硬件时钟。若无法存取时，可用此参数直接以 I/O 指令
来存取硬件时钟。
--hctosys 将系统时钟调整为与目前的硬件时钟一致。
--set --date=<日期与时间> 设定硬件时钟。
--show 显示硬件时钟的时间与日期。
--systohc 将硬件时钟调整为与目前的系统时钟一致。
--test 仅测试程序，而不会实际更改硬件时钟。
--utc 若要使用格林威治时间，请加入此参数，hwclock 会执行转换的工作。
--version 显示版本信息。
例 1：查看硬件时间
# hwclock --show
或者
# clock --show
例 2：设置硬件时间
# hwclock --set --date="07/07/06 10:19" （月/日/年 时:分:秒）
或者
# clock --set --date="07/07/06 10:19" （月/日/年 时:分:秒）
例 3：硬件时间和系统时间的同步
按照前面的说法，重新启动系统，硬件时间会读取系统时间，实现同步，但是在不重新启动的时候，需
要用 hwclock 或 clock 命令实现同步。
硬件时钟与系统时钟同步：
# hwclock --hctosys（hc 代表硬件时间，sys 代表系统时间） #将硬件时间写入系统时间
或者
# clock –hctosys
例 4：系统时钟和硬件时钟同步：
# hwclock --systohc     #将系统时间写入硬件
或者
# clock –systohc
例 5：强制将系统时间写入 CMOS，使之永久生效，避免系统重启后恢复成原时间
# clock –w
或者
# hwclock -w
```



3. 时间同步

```
2.5、时间同步
例 1：同步时间
# ntpdate 210.72.145.44 （210.72.145.44 是中国国家授时中心的官方服务器） 例 2：定时同步时间
# crontab –e 添加脚本例子如下：
*/20 * * * * /usr/sbin/ntpdate 210.72.145.44 //每 20 分钟执行一次
30 5 * * * /usr/sbin/ntpdate 210.72.145.44 //每天早晨 5 点半执行
※ 前面五个*号代表五个数字，数字的取值范围和含义如下：分钟（0-59） 小時（0-23） 日期（1-31）
月份（1-12） 星期（0-6）//0 代表星期天设定完毕后，可使用# crontab –l 查看上面的设定。
```

