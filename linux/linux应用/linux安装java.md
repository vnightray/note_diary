# linux安装jdk_1.8.0_152

1. 在oracle官网下载jdk-8u152-linux-x64.tar.gz

<https://www.oracle.com/technetwork/java/javase/archive-139210.html> 

2. 将文件放置在 /usr/local/java/下
3. 解压jdk文件

```
tar -zxvf jdk-8u152-linux-x64.tar.gz
```

4. 

```
[root@iZf8z2mq35lahjryri3vvkZ packages]# cd /usr/local/java/
[root@iZf8z2mq35lahjryri3vvkZ java]# ls
jdk1.8.0_152
[root@iZf8z2mq35lahjryri3vvkZ java]# cd jdk1.8.0_152/
[root@iZf8z2mq35lahjryri3vvkZ jdk1.8.0_152]# ls
bin        db       javafx-src.zip  lib      man          release  THIRDPARTYLICENSEREADME-JAVAFX.txt
COPYRIGHT  include  jre             LICENSE  README.html  src.zip  THIRDPARTYLICENSEREADME.txt
[root@iZf8z2mq35lahjryri3vvkZ jdk1.8.0_152]# pwd
/usr/local/java/jdk1.8.0_152
[root@iZf8z2mq35lahjryri3vvkZ jdk1.8.0_152]# ls
bin        db       javafx-src.zip  lib      man          release  THIRDPARTYLICENSEREADME-JAVAFX.txt
COPYRIGHT  include  jre             LICENSE  README.html  src.zip  THIRDPARTYLICENSEREADME.txt
[root@iZf8z2mq35lahjryri3vvkZ jdk1.8.0_152]# cd jre/
[root@iZf8z2mq35lahjryri3vvkZ jre]# pwd
/usr/local/java/jdk1.8.0_152/jre
[root@iZf8z2mq35lahjryri3vvkZ jre]# vim /etc/profile
[root@iZf8z2mq35lahjryri3vvkZ jre]# 

```



5. 修改/etc/profile文件配置

```
[root@iZf8z2mq35lahjryri3vvkZ ~]# vim /etc/profile
```

将以下内容粘贴至profile文件最后，vim转至最后一行的快捷键是shift+G

```
# ---Set Java Environment---
JAVA_HOME=/usr/local/java/jdk1.8.0_152
JRE_HOME=/usr/local/java/jdk1.8.0_152/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
# ---Set Java Environment---
```

保存退出



6. 刷新环境变量

```
source /etc/profile
```



7. 检测java安装效果

```
java -version
```

