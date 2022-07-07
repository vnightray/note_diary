# ps相关命令

```
ps 的参数说明：
ps 提供了很多的选项参数，常用的有以下几个；
l 长格式输出；
u 按用户名和启动时间的顺序来显示进程；
j 用任务格式来显示进程；
f 用树形格式来显示进程；
a 显示所有用户的所有进程（包括其它用户）；
x 显示无控制终端的进程；
r 显示运行中的进程；
ww 避免详细参数被截断；
```



我们常用的选项是组合是 aux 或 lax，还有参数 f 的应用；ps aux 或 lax 输出的解释；

USER 表示启动进程用户。PID 表示进程标志号。%CPU 表示运行该进程占用 CPU 的时间与该进程总的运行时间的比例。%MEM 表示该进程占用内存和总内存的比例。VSZ 表示占用的虚拟内存大小，以 KB 为单位。RSS 为进程占用的物理内存值，以 KB 为单位。TTY 表示该进程建立时所对应的终端，"?"表示该进程不占用终端。STAT 表示进程的运行状态，包括以下几种代码：D，不可中断的睡眠；R，就绪（在可运行队列中）；S，睡眠；T，被跟踪或停止；Z，终止（僵死）的进程，Z 不存在，但暂时无法消除；W，没有足够的内存分页可分配；<高优先序的进程；N，低优先序的进程；L，有内存分页分配并锁在内存体内（实时系统或 I/O）。START 为进程开始时间。TIME 为执行的时间。COMMAND 是对应的命令名。



```
应用实例：
实例一：ps aux 最常用
[root@localhost ~]# ps -aux |more
可以用 | 管道和 more 连接起来分页查看；
[root@localhost ~]# ps aux > ps001.txt
[root@localhost ~]# more ps001.txt 
这里是把所有进程显示出来，并输出到 ps001.txt 文件，然后再通过 more 来分页查看；
实例二：和 grep 结合，提取指定程序的进程；
[root@localhost ~]# ps aux |grep httpd
root 4187 0.0 1.3 24236 10272 ? Ss 11:55 0:00 /usr/sbin/httpd
apache 4189 0.0 0.6 24368 4940 ? S 11:55 0:00 /usr/sbin/httpd
apache 4190 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
apache 4191 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
apache 4192 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
apache 4193 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
apache 4194 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
apache 4195 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
apache 4196 0.0 0.6 24368 4932 ? S 11:55 0:00 /usr/sbin/httpd
root 4480 0.0 0.0 5160 708 pts/3 R+ 12:20 0:00 grep httpd
实例二：父进和子进程的关系友好判断的例子
[root@localhost ~]# ps auxf |grep httpd
root 4484 0.0 0.0 5160 704 pts/3 S+ 12:21 0:00 \_ grep 
httpd
root 4187 0.0 1.3 24236 10272 ? Ss 11:55 0:00 /usr/sbin/httpd
apache 4189 0.0 0.6 24368 4940 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4190 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4191 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4192 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4193 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4194 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4195 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
apache 4196 0.0 0.6 24368 4932 ? S 11:55 0:00 \_ /usr/sbin/httpd
这里用到了 f 参数；父与子关系一目了然；
例三：找出消耗内存最多的前 10 名进程
# ps -auxf | sort -nr -k 4 | head -10 
例四：找出使用 CPU 最多的前 10 名进程
# ps -auxf | sort -nr -k 3 | head -10
```



```
kill所有python的后台进程：
ps -ef | grep python | awk '{ print $2 }' | sudo xargs kill -9
```

