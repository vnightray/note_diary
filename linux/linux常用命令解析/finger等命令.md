finger等命令查看用户信息

finger / w / who

finger 工具：用来查询用户信息，侧重用户家目录、登录 SHELL 等

finger [参数选项] [用户名］

-l 采用长格式（默认），显示由-s 选项所包含的所有信息，以及主目录、办公地址、办公电话、登录 SHELL、邮件状态、.plan、.project 和.forward； -m 禁止对用户真实名字进行匹配；

-p 把.plan 和.project 文件中的内容省略；

-s 显示短格式，用户名（也被称为登录名 Login）、真实名字（NAME）、在哪个终端登录（Tty）、写状态、空闲时间（Idle）、登录时间（Login Time）、办公地点、办公电话等；

    kimihiro@kimihiro-vmachine:~$ finger
    Login     Name       Tty      Idle  Login Time   Office     Office Phone
    kimihiro  kimihiro   pts/0          Oct 20 17:10 (192.168.11.52)
    kimihiro@kimihiro-vmachine:~$ finger -l
    Login: kimihiro       			Name: kimihiro
    Directory: /home/kimihiro           	Shell: /bin/bash
    On since Wed Oct 20 17:10 (CST) on pts/0 from 192.168.11.52
       2 seconds idle
    No mail.
    No Plan.

    kimihiro@kimihiro-vmachine:~$ w
     17:14:08 up 4 min,  1 user,  load average: 0.33, 0.76, 0.41
    USER     TTY      来自           LOGIN@   IDLE   JCPU   PCPU WHAT
    kimihiro pts/0    192.168.11.52    17:10    0.00s  0.07s  0.00s w
    kimihiro@kimihiro-vmachine:~$ who
    kimihiro pts/0        2021-10-20 17:10 (192.168.11.52)
    



groups查询用户所归属的用户组

语法格式： groups 用户名

    [beinan@localhost ~]$ groups beinan 注：查询 beinan 所归属的用户组；
    beinan : beinan 注：beinan 是 beinan 用户组下的成员；
    [beinan@localhost ~]$ groups linuxsir 注：查询 linuxsir 用户所归属的用户组；
    linuxsir : linuxsir root beinan 注：linuxsir 用户是 linuxsir 用户组、beinan 用户组、root 用户组成员；



su 命令

命令作用

su 的作用是变更为其它使用者的身份，超级用户除外，需要键入该使用者的密码。

使用方式

su -fmp -s shell --version [USER [ARG]]

参数说明

-f ， –fast：不必读启动文件（如 csh.cshrc 等），仅用于 csh 或 tcsh 两种 Shell。 

-l ， –login：加了这个参数之后，就好像是重新登陆一样，大部分环境变量(例如 HOME、SHELL和 USER 等)都是以该使用者(USER)为主，并且工作目录也会改变。如果没有指定 USER，缺省情况是 root。

 -m， -p ，–preserve-environment：执行 su 时不改变环境变数。

-c command：变更账号为 USER 的使用者，并执行指令（command）后再变回原来使用者。

–help 显示说明文件

–version 显示版本资讯

USER：欲变更的使用者账号，

ARG: 传入新的 Shell 参数。



sudo 命令

/etc/sudoers 谁能作什么的一个列表，Sudo 能用需要在这个文件中定义

visudo 增加如下，加%代表用户组，ALL=(ALL)表示登录者的来源主机名，最后的 ALL 代表可执行

的命令。NOPASSWD 代表不需要密码直接可运行 Sudo,限制多命令一定要写绝对路径，用逗号分开，多行用

‘\’，用！代表不能执行



chmod 命令

	文件或者目录的用户能够使用 chmod 命令修改文件的权限。Chmod 命令有两种方式：一种是字符方

式，使用字符来修改文件的权限；另外一种是数字方式，使用 3 个数字的组合来修改文件的权限。

使用方式 : chmod -cfvR [--version] mode file... 

说明 : Linux/Unix 的档案调用权限分为三级 : 档案拥有者、群组、其他。利用 chmod 可以借以控制档案如何被他人所调用。

参数 : 

mode : 权限设定字串，格式如下 : ugoa...rwxX]...，其中

u 表示该档案的拥有者，g 表示与该档案的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表

示这三者皆是。

+表示增加权限、- 表示取消权限、= 表示唯一设定权限。

r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该档案是个子目录或者该档案已经被设定过为可执行。

-c : 若该档案权限确实已经更改，才显示其更改动作

-f : 若该档案权限无法被更改也不要显示错误讯息

-v : 显示权限变更的详细资料

-R : 对目前目录下的所有档案与子目录进行相同的权限变更(即以递回的方式逐个变更) 

--help : 显示辅助说明

--version : 显示版本

范例 :

将档案 file1.txt 设为所有人皆可读取 : 

    chmod ugo+r file1.txt 

将档案 file1.txt 设为所有人皆可读取 : 

    chmod a+r file1.txt

将档案 file1.txt 与 file2.txt 设为该档案拥有者，与其所属同一个群体者可写入，但其他以外的人则不

可写入 : chmod ug+w,o-w file1.txt file2.txt 

将 ex1.py 设定为只有该档案拥有者可以执行 : chmod u+x ex1.py 

将目前目录下的所有档案与子目录皆设为任何人可读取 : chmod -R a+r * 

 数字方式的基本语法是：chmod nnn 文件

其中第 1、2、3 个 n 分别表示用户、组成员和所有其它用户。各个位置上的 n 要么是一个 0，或者是一

个由赋予权限的相关值相加得到的单个阿拉伯数字之和。这些数字的意义如表 1 所示。

值 表示的意义

4 表示文件或者目录的读权限

2 表示文件或者目录的写权限

1 表示文件或者目录的执行权限

很显然，当使用数字方式时，这 3 个数字必须为 0 至 7 中的一个。

若要 rwx 属性则 4+2+1=7；

若要 rw-属性则 4+2=6；

若要 r-x 属性则 4+1=7。

范例：

chmod a=rwx file 和 chmod 777 file 效果相同

chmod ug=rwx,o=x file 和 chmod 771 file 效果相同

若用 chmod 4755 filename 可使此程序具有 root 的权限








