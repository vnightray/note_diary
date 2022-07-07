# RPM(红帽软件包管理器)

|      安装软件的命令格式       | rpm -ivh filename.rpm |
| :---------------------------: | :-------------------: |
|      升级软件的命令格式       | rpm -Uvh filename.rpm |
|      卸载软件的命令格式       |  rpm -e filename.rpm  |
|  查询软件描述信息的命令格式   | rpm -qpi filename.rpm |
|  列出软件文件信息的命令格式   | rpm -qpl filename.rpm |
| 查询文件属于哪个rpm的命令格式 |   rpm -qf filename    |



# YUM软件仓库

|           命令            |             作用             |
| :-----------------------: | :--------------------------: |
|     yum repolist all      |         列出所有仓库         |
|       yum list all        |     列出仓库中所有软件包     |
|    yum info 软件包名称    |        查看软件包信息        |
|  yum install 软件包名称   |          安装软件包          |
| yum reinstall 软件包名称  |        重新安装软件包        |
|   yum update 软件包名称   |          升级软件包          |
|     yum remove 软件包     |          移除软件包          |
|       yum clean all       |       清除所有仓库缓存       |
|     yum check-update      |      检查可更新的软件包      |
|       yum grouplist       | 查看系统中已经安装的软件包组 |
| yum groupinstall 软件包组 |      安装指定的软件包组      |
| yum groupremove 软件包组  |      移除指定的软件包组      |
|  yum groupinfo 软件包组   |    查询指定的软件包组信息    |



# systemctl管理服务的启动

| System V init 命令 System V init 命令（RHEL 6 系统） | systemctl 命令（RHEL 7 系统） |              作用              |
| :--------------------------------------------------: | :---------------------------: | :----------------------------: |
|                  service foo start                   |  systemctl start foo.service  |            启动服务            |
|                 service foo restart                  | systemctl restart foo.service |            重启服务            |
|                   service foo stop                   |  systemctl stop foo.service   |            停止服务            |
|                  service foo reload                  | systemctl reload foo.service  | 重新加载配置文件（不终止服务） |
|                  service foo status                  | systemctl status foo.service  |          查看服务状态          |

| System V init 命令 （RHEL 6 系统） | systemctl 命令（RHEL 7 系统）            | 作用                               |
| ---------------------------------- | ---------------------------------------- | ---------------------------------- |
| chkconfig foo on                   | systemctl enable foo.service             | 开机自动启动                       |
| chkconfig foo off                  | systemctl disable foo.service            | 开机不自动启动                     |
| chkconfig foo                      | systemctl is-enabled foo.service         | 查看特定服务是否为                 |
| chkconfig --list                   | systemctl list-unit-files --type=service | 查看各个级别下服务的启动与禁用情况 |



### RPM软件包管理的查询功能

```
rpm {-q|--query} [select-options] [query-options]
```

1. 查询系统已安装的软件：

```
语法：rpm -q 软件名
[root@iZf8z2mq35lahjryri3vvkZ lib]# rpm -q ntpdate
ntpdate-4.2.6p5-29.el7.centos.2.x86_64
```

```
##在所有已经安装的软件包中查找某个软件。
[root@localhost RPMS]# rpm -qa |grep gaim
```



2. 查询已安装软件包都安装在何处：

```
语法：rpm -ql 软件名 或 rpm rpmquery -ql 软件名
[root@localhost RPMS]# rpm -ql lynx
[root@localhost RPMS]# rpmquery -ql lynx
```



3. 查询一个已安装软件包的信息：

```
语法格式： rpm -qi 软件名
[root@localhost RPMS]# rpm -qi lynx
```



4. 查看一下已安装软件的配置文件：

```
语法格式：rpm -qc 软件名
[root@localhost RPMS]# rpm -qc lynx
```



5. 查看一个已安装软件的文档安装位置：

```
语法格式： rpm -qd 软件名
[root@localhost RPMS]# rpm -qd lynx
```



6. 查看一个已安装软件所依赖的软件包及文件：

```
语法格式： rpm -qR 软件名
[root@localhost beinan]# rpm -qR rpm-python
```



























