# Ubuntu 更改apt 源

方法一：修改源列表文件
源列表备份

```
sudo cp /etc/apt/sources.list  /etc/apt/sources.list_save
```


进入源列表

```
sudo gedit /etc/apt/sources.list  
```


修改源列表，把以下阿里源复制到源列表中

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```



更新源列表

```
sudo apt update
sudo apt-get update
```


这个命令，会访问源列表里的每个网址，并读取软件列表，然后保存在本地电脑。

安装可以更新的软件

```
sudo apt upgrade
sudo apt-get upgrade
```


这个命令，会把本地已安装的软件，与刚下载的软件列表里对应软件进行对比，如果发现已安装的软件版本太低，就会提示你更新。
