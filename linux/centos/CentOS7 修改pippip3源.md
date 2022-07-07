# CentOS7 修改pip/pip3源

###一、国内镜像列表

国内镜像列表：

```
http://pypi.douban.com/simple/ 豆瓣
http://mirrors.aliyun.com/pypi/simple/ 阿里
http://pypi.hustunique.com/simple/ 华中理工大学
http://pypi.sdutlinux.org/simple/ 山东理工大学
http://pypi.mirrors.ustc.edu.cn/simple/ 中国科学技术大学
https://pypi.tuna.tsinghua.edu.cn/simple 清华
```




###二、Centos配置过程

有两个方法，方法一是一次性的，方法二是永久性的。

####方法一：

后面的–trusted-host 是指设置为受信源，否则在安全性较高的连接下是连接不上的

以安装pandas为例

```
pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host pypi.tuna.tsinghua.edu.cn
```



####方法二：

进入根目录

```
cd ~
```

创建文件夹

```
mkdir .pip
```

进入目录

```
cd .pip
```

编辑文件pip.conf(会自动创建)

```
vim pip.conf
```

把下边的复制进去(我用的清华的源，如果想配置其他源只需要改下边的内容即可)

```
[global]
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

