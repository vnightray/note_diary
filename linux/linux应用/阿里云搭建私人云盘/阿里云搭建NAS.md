# 阿里云搭建NAS

## 步骤一：安装OwnCloud

OwnCloud是一款开源的云存储软件，基于PHP的自建网盘。基本上是个人使用，没有用户注册功能，但是有用户添加功能，你可以无限制地添加用户，OwnCloud支持多个平台（Windows，MAC，Android，IOS，Linux）。

1.执行以下命令，添加一个新的软件源。

```
rpm --import https://download.owncloud.org/download/repositories/10.0/CentOS_7/repodata/repomd.xml.key
wget http://download.owncloud.org/download/repositories/10.0/CentOS_7/ce:10.0.repo -O /etc/yum.repos.d/ce:10.0.repo
```

2.执行以下命令安装OwnCloud-files。

```
yum -y install owncloud-files
```

3.执行以下命令查看安装是否成功。

```
ll /var/www/html
```



## 步骤二：安装Apache服务

Apache HTTP Server（简称Apache）是Apache软件基金会的一个开放源代码的网页服务器软件，可以在大多数电脑操作系统中运行。由于其跨平台和安全性，被广泛使用，是最流行的Web服务器软件之一。

1.执行以下命令安装Apache服务。

```
yum -y install httpd
```

2.执行以下命令启动Apache服务。

```
systemctl start httpd
```

3.打开浏览器输入体验平台创建的ECS的弹性公网IP。如果出现如下图内容表示Apache安装成功。


4.添加OwnCloud配置：
a.执行以下命令打开Apache配置文件。

```
vim /etc/httpd/conf/httpd.conf
```

b.按i键进入文件编辑模式，然后在内容后添加以下内容。

```
# owncloud config
Alias /owncloud "/var/www/html/owncloud/"
<Directory /var/www/html/owncloud/>
    Options +FollowSymlinks
    AllowOverride All
    <IfModule mod_dav.c>
        Dav off
    </IfModule>
    SetEnv HOME /var/www/html/owncloud
    SetEnv HTTP_HOME /var/www/html/owncloud
</Directory>
```



c.按esc键退出编辑模式，然后输入:wq保存并退出配置文件。

## 步骤三：安装并配置PHP

由于OwnCloud是基于PHP开发的云存储软件，需要PHP运行环境，请根据以下步骤完成OwnCloud工作环境的配置。

1.执行以下命令手动更新rpm源。

```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm   
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm   
```

2.执行以下命令安装PHP 7.2版本。
说明: OwnCloud只支持PHP 5.6+。

```
yum -y install php72w
yum -y install php72w-cli php72w-common php72w-devel php72w-mysql php72w-xml php72w-odbc php72w-gd php72w-intl php72w-mbstring
```

3.执行以下命令检测PHP是否安装成功。

```
php -v
```

4.将PHP配置到Apache中：
a.执行以下命令，找到php.ini文件目录。

```
find / -name php.ini
```

b.执行以下命令打开httpd.conf文件。

```
vim /etc/httpd/conf/httpd.conf
```

c.按i键进入文件编辑模式，然后在文件最后添加以下内容。

```
PHPIniDir /etc/php.ini
```

d.按esc键退出编辑模式，然后输入:wq保存并退出配置文件。
e.执行以下命令，重启Apache服务。

```
systemctl restart httpd
```

## 步骤四：配置OwnCloud

完成上述配置后，您就可以登录OwnCloud创建个人网盘了。

1.打开浏览器，输入ECS弹性IP/owncloud，例如1.1.1.1/owncloud。
2.创建管理员账号和密码，然后单击**存储&数据库**，最后单击**安装完成。**


3.输入已创建的用户名和密码登录Owncloud。


登录成功界面如下：



## 步骤五：挂载NAS服务

完成OwnCloud初始化之后就可以将NAS存储包挂载到您的网盘服务器上。

1.在体验平台左侧资源栏，单击**一键复制登录url**，然后在浏览器中粘贴已复制的url。


2.输入体验平台提供的子用户名和密码，登录阿里云控制台。在产品列表，搜索NAS，然后单击文件存储NAS。


3.选择**文件系统**>**文件系统列表**，然后单击文件系统 ID进入文件系统详情页。


4.选择**挂载使用**，然后单击**添加挂载点**选择专有网络，最后单击确定。


5.在命令行终端，执行以下命令安装NFS客户端。

```
yum install nfs-utils
```

6.在控制台，单击**挂载文件系统到ECS**查看挂载命令。


7.在打开的挂载文件系统到ECS页面复制挂载命令。


a.点击挂载命令复制按钮。
c.运行。


8.将复制好的挂载命令粘贴到记事本中，然后把命令最后的/mnt替换为：/var/www/html/owncloud/data/<OwnCloud登录名>。
例如：

```
sudo mount -t nfs -o vers=3,nolock,proto=tcp,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 3ad894afd4-uon67.cn-shanghai.nas.aliyuncs.com:/ /var/www/html/owncloud/data/admin
```

9.在命令窗口执行上一步骤的挂载命令。


10.执行以下命令查看挂载是否成功。

```
df -h | grep aliyun
```



*注意:NAS挂载成功后，OwnCloud网盘中的默认目录和文件不可读写，请在网盘中新建目录上传。*