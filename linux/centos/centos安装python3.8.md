# centos7.6 安装 python3.8

### 环境信息

操作系统：CentOS Linux release 7.6.1810  Python：3.8.10

### 现状说明

当前CentOS系统自带了python2.7.5，因为yum会用到python2，所以不能删除，此次安装了python3之后就保持两个版本长期共存吧。

本次安装采用的是下载python源码再编译的方式；

### 操作步骤

以root身份登录CentOS，以下操作都在默认的~目录下：

1. yum更新：

```
yum update -y
```

1. 安装必要的软件：

```
yum -y install \
zlib-devel \
bzip2-devel \
openssl-devel \
ncurses-devel \
sqlite-devel \
readline-devel \
tk-devel \
libffi-devel \
wget \
gcc \
make
```

1. 下载python3.8.10源码：

```
wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
```

1. 解压：

```
tar -zxvf Python-3.8.10.tgz
```

1. 进入解压后的目录，执行编译前的configure操作：

```
cd Python-3.8.10 && ./configure prefix=/usr/local/python3
```

1. 编译源码，在Python-3.8.10目录执行以下命令：

```
make && make install
```

编译成功后提示如下信息，setuptools和pip都已经部署成功：

```
Collecting setuptools
Collecting pip
Installing collected packages: setuptools, pip
Successfully installed pip-19.0.3 setuptools-40.8.0
```

1. 创建python3的链接：

```
ln -s /usr/local/python3/bin/python3.8 /usr/bin/python3
```

1. 创建pip3的链接：

```
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip
```

1. pip3升级

```
pip install --upgrade pip
```

至此，安装完成，接下来简单验证一下

### 验证

1. 简单操作如下，可见python3安装成功：

```
[root@python3 ~]# python3 --version
Python 3.7.4
[root@python3 ~]# pip3 --version
pip 19.1.1 from /usr/local/python3/lib/python3.7/site-packages/pip (python 3.7)
[root@python3 ~]# python3
Python 3.7.4 (default, Jul 20 2019, 11:35:19) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-36)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("Hello world")
Hello world
```

按下Ctrl+d退出python3对话模式  2. 安装Flask：

```
pip3 install Flask
```

控制台输出以下信息，表示安装Flask成功：

```
[root@python3 ~]# pip3 install Flask
Collecting Flask
  Downloading https://files.pythonhosted.org/packages/9b/93/628509b8d5dc749656a9641f4caf13540e2cdec85276964ff8f43bbb1d3b/Flask-1.1.1-py2.py3-none-any.whl (94kB)
     |████████████████████████████████| 102kB 431kB/s 
Collecting itsdangerous>=0.24 (from Flask)
  Downloading https://files.pythonhosted.org/packages/76/ae/44b03b253d6fade317f32c24d100b3b35c2239807046a4c953c7b89fa49e/itsdangerous-1.1.0-py2.py3-none-any.whl
Collecting click>=5.1 (from Flask)
  Downloading https://files.pythonhosted.org/packages/fa/37/45185cb5abbc30d7257104c434fe0b07e5a195a6847506c074527aa599ec/Click-7.0-py2.py3-none-any.whl (81kB)
     |████████████████████████████████| 81kB 1.3MB/s 
Collecting Jinja2>=2.10.1 (from Flask)
  Downloading https://files.pythonhosted.org/packages/1d/e7/fd8b501e7a6dfe492a433deb7b9d833d39ca74916fa8bc63dd1a4947a671/Jinja2-2.10.1-py2.py3-none-any.whl (124kB)
     |████████████████████████████████| 133kB 1.8MB/s 
Collecting Werkzeug>=0.15 (from Flask)
  Downloading https://files.pythonhosted.org/packages/d1/ab/d3bed6b92042622d24decc7aadc8877badf18aeca1571045840ad4956d3f/Werkzeug-0.15.5-py2.py3-none-any.whl (328kB)
     |████████████████████████████████| 337kB 3.4MB/s 
Collecting MarkupSafe>=0.23 (from Jinja2>=2.10.1->Flask)
  Downloading https://files.pythonhosted.org/packages/98/7b/ff284bd8c80654e471b769062a9b43cc5d03e7a615048d96f4619df8d420/MarkupSafe-1.1.1-cp37-cp37m-manylinux1_x86_64.whl
Installing collected packages: itsdangerous, click, MarkupSafe, Jinja2, Werkzeug, Flask
Successfully installed Flask-1.1.1 Jinja2-2.10.1 MarkupSafe-1.1.1 Werkzeug-0.15.5 click-7.0 itsdangerous-1.1.0
```

1. 创建名为hello.py的文件，内容如下：

```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
        return 'Hello flask'

if __name__ == '__main__':
        app.run(host='0.0.0.0',port=5000,debug=True)
```

1. 运行脚本：

```
python3 hello.py
```