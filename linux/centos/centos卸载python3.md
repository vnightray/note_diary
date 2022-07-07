# centos7卸载python3

###1. 查看所有的python路径：

```
whereis python
```



###2.运行以下命令删除python3.4相关文件: 

```
    # rm -rf /usr/bin/python3.4m
    # rm -rf /usr/bin/python3.4
    # rm -rf /usr/lib/python3.4
    # rm -rf /usr/lib64/python3.4
    # rm -rf /usr/include/python3.4m
```



### 3. 再次查看所有的python路径确认一下： 

```
# whereis python
```



### 4. 修改pip配置

```
# vim /usr/bin/pip
```

将第一行`#!/usr/bin/python3.4`改为 `#!/usr/bin/python3.8.10 （即改为/usr/bin/目录下你使用的python版本执行文件） 



### 5. 卸载完成



## 另一种卸载方法

### 1. 卸载python3

```
# rpm -qa|grep python3|xargs rpm -ev --allmatches --nodeps
```



### 2. 删除所有参与文件

```
# whereis python3 |xargs rm -frv
```



### 3. 卸载完成

查看现有已安装python

```
# whereis python
```

