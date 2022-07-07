# YUM相关

```
YUM
YUM配置文件
创建容器，位置在/etc/yum.repos.d，扩展名必须是.repo
#cd /etc/yum.repos.d
#vim yum.repo 新建一个仓库文件，名字可以随便定义，在文件中写如下内容
[base] #代表容器名称，中括号一定要存在，里面的名字可随便取
name=base #说明这个容器的意义，随便写都可以
baseurl=ftp://192.168.0.6/pub/Server #192. 168. 0. 6 是你的 YUM 源地址，这个很重要。
enabled=1 #是否启动，=0 则不启动，不启动就无法使用该源
gpgcheck=0 #是否验证. 可不要
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release ＃验证的密钥. 可不要命令:
#yum repolist all 显示目前所使用的容器有哪些
如果查询出来的容器，status 为 disabled，要将配置文件，如上 enabled=1
```



```
/etc/yum.conf
yum.conf 这个配置文件主要是 yum 客户端使用，里面主要规定了要去用的 rpm 包的 yum 服务器的信
息。
[main] #main 开头的块用于对客户端进行配置，在 main 后也可以指定 yum 源（不推荐这样做），与
/etc/yum.repo.d 中指定 yum 源相同
cachedir=/var/cache/yum
#cachedir：yum 缓存的目录，yum 在此存储下载的 rpm 包和数据库，一般是/var/cache/yum。
keepcache=0 #0 表示不保存下载的文件，1 表示保存下载的文件，默认为不保存
debuglevel=2
#debuglevel：除错级别，0──10,默认是 2 貌似只记录安装和删除记录
logfile=/var/log/yum.log #指定 yum 的日志文件
pkgpolicy=newest #包的策略，如果配置多了 yum 源，同一软件在不同的 yum 源中有不同版本，
newest 则安装最新版本，该值为 lastest，则 yum 会将服务器上 ID 按照字母序排列，选择最后那个服务器上
的软件安装
distroverpkg=centos-release
#指定一个软件包，yum 会根据这个包判断你的发行版本，默认是 redhat-release，也可以是安装的任何
针对自己发行版的 rpm 包。
tolerant=1
#tolerent，也有 1 和 0 两个选项，表示 yum 是否容忍命令行发生与软件包有关的错误，比如你要安装
1,2,3 三个包，而其中 3 此前已经安装了，如果你设为 1,则 yum 不会出现错误信息。默认是 0。
exactarch=1
#exactarch，有两个选项 1 和 0,代表是否只升级和你安装软件包 cpu 体系一致的包，如果设为 1，则如
你安装了一个 i386 的 rpm，则 yum 不会用 i686 的包来升级。
retries=20
#retries，网络连接发生错误后的重试次数，如果设为 0，则会无限重试。
obsoletes=1
gpgcheck=1
#gpgchkeck= 有 1 和 0 两个选择，分别代表是否是否进行 gpg 校验，如果没有这一项，默认是检查的。
plugins = 1 #是否启用插件，默认 1 为允许，0 表示不允许
reposdir=/etc/yy.rm #默认是 /etc/yum.repos.d/ 低下的 xx.repo 后缀文件
#默认都会被include 进来 也就是说 /etc/yum.repos.d/xx.repo 无论配置文件有多少个 每个里面有多少
个[name]最后其实都被整合到 一个里面看就是了 重复的[name]应该是前面覆盖后面的--还是后面的覆盖前
面的呢？enabled 测试是后面覆盖前面
exclude=xxx
#exclude 排除某些软件在升级名单之外，可以用通配符，列表中各个项目要用空格隔开，这个对于安
装了诸如美化包，中文补丁的朋友特别有用。
keepcache=[1 or 0]
#设置 keepcache=1，yum 在成功安装软件包之后保留缓存的头文件 (headers) 和软件包。默认值为
keepcache=0 不保存
reposdir=[包含 .repo 文件的目录的绝对路径] #该选项用户指定 .repo 文件的绝对路径。.repo 文件包含软件仓库的信息 (作用与 /etc/yum.conf 文件
中的 [repository] 片段相同)。中
```



```
#yum安装
yum install xxx
#yum 卸载软件
yum remove xxx
```



```
1.使用 YUM 查找软件包
命令：yum search 
2.列出所有可安装的软件包
命令：yum list
3.列出所有可更新的软件包
命令：yum list updates
4.列出所有已安装的软件包
命令：yum list installed
5.列出所有已安装但不在 Yum Repository 內的软件包
命令：yum list extras
6.列出所指定的软件包
命令：yum list 
7.使用 YUM 获取软件包信息
命令：yum info 
8.列出所有软件包的信息
命令：yum info
9.列出所有可更新的软件包信息
命令：yum info updates
10.列出所有已安裝的软件包信息
命令：yum info installed
11.列出所有已安裝但不在 Yum Repository 內的软件包信息
命令：yum info extras
12.列出软件包提供哪些文件
命令：yum provides
```



```
清除YUM缓存
yum 会把下载的软件包和 header 存储在 cache 中，而不会自动删除。如果我们觉得它们占用了磁盘空间，
可以使用 yum clean 指令进行清除，更精确的用法是 yum clean headers 清除 header，yum clean packages 清
除下载的 rpm 包，yum clean all 一股脑儿端
1.清除缓存目录(/var/cache/yum)下的软件包
命令：yum clean packages
2.清除缓存目录(/var/cache/yum)下的 headers
命令：yum clean headers
3.清除缓存目录(/var/cache/yum)下旧的 headers
命令：yum clean 
Oldheaders
4.清除缓存目录(/var/cache/yum)下的软件包及旧的 headers
命令：yum clean, yum clean 
all (= yum clean packages; yum clean oldheaders)
```





