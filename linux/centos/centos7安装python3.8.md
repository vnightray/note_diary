# centos7 安装python3.8.10

### 1. 下载python3.8压缩包

使用如下命令下载python 3.8：

```
wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
```

如果提示-bash: wget: 未找到命令，需要先使用yum安装wget，命令如下：

```
yum -y install wget
```



### 2. 解压缩安装包

通过以下命令解压缩下载的文件：

```
tar https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
```



### 3. 安装前准备

因为编译 Python 源代码需要依赖于很多工具，所以得先准备一下，依次执行以下命令：

```
yum update -y

yum groupinstall -y 'Development Tools'

yum install -y gcc openssl-devel bzip2-devel libffi-devel
```



### 4. 安装

现在我们就可以安装python 3.8了，首先执行以下命令（注意，一定要在Python-3.8.10目录下执行该命令）：

```
./configure prefix=/usr/local/python3 --enable-optimizations
```

–prefix选项是配置安装的路径，如果不配置该选项，安装后可执行文件默认放在/usr/local/bin，库文件默认放在/usr/local/lib，配置文件默认放在/usr/local/etc，其它的资源文件放在/usr/local/share，比较凌乱。

如果配置--prefix，如：./configure --prefix=/usr/local/python3可以把所有资源文件放在 /usr/local/python3 的路径中，不会杂乱.用了--prefix选项的另一个好处是卸载软件或移植软件。当某个安装的软件不再需要时，只须简单的删除该安装目录,就可以把软件卸载得干干净净；移植软件只需拷贝整个目录到另外一个相同的操作系统机器即可.当然要卸载程序，也可以在原来的make 目录下用一次make uninstall，但前提是make文件指定过uninstall.

--enable-optimizations是优化选项（LTO,PGO 等）加上这个 flag 编译后，性能有 10% 左右的优化,但是这会明显的增加编译时间,老久了.

./configure命令执行完毕之后创建一个文件 Makefile, 供下面的make命令使用,执行make install之后就会把程序安装到我们指定的文件夹中去。

```
make && make install
```



### 5. 修改python2链接

我们首先查看一下 Python 可执行文件的位置：

```
which python
```

然后切换到相应的目录：

```
cd /usr/bin
```

查看相关的python信息：

```
ls -la python*
```

发现当我们执行python 命令时，系统指向python 2，然后python 2指向python 2.7，所以系统默认使用的python版本仍然是python 2。

首先将python 改名为 python.bak

```
mv python python2.bak
```

再次查看python相关信息：

```
ls -la python*
```



### 7. 修改配置文件

进入目录/usr/bin，查看有关yum的文件：

```
ls -la yum*
```

使用vi 进入文本编辑器（如果有多个yum配置文件，都要进去修改）：

```
vi yum
```

点击i进入编辑模式之后将#!/usr/bin/python 改为#!/usr/bin/python2，按ESC退出编辑模式，输入:wq!保存修改退出。

同样进入文件 /usr/libexec/urlgrabber-ext-down 做同样修改。

### 8. 配置python3软链接

进入/usr/bin目录，配置软链接。在我看来，其实软链接就像Windows操作系统里的快捷方式，比如现在桌面上有一个图标叫python，现在你双击打开它，发现它打开的是Python 2，所以我们要配置其指向python 3。

```
ln -s /usr/local/python3/bin/python3.8 /usr/bin/python
```

```
ln -s /usr/local/python3/bin/pip3.8 /usr/bin/pip
```

如果提示/usr/bin/pip已经存在，可以使用rm删除，然后再进行配置。

配置后结果：

当然，也可以配置一个pip3软链接。

```
ln -s /usr/local/python3/bin/pip3.8 /usr/bin/pip3
```

