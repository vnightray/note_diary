# 服务器上创建mysql的docker镜像容器，并远程连接访问

1. 使用docker pull mysql 来拉取docker仓库中的mysql镜像

```
# docker pull mysql;
```

2. 使用mysql镜像创建容器（注意端口映射）

```
# docker run -p 3307:3306 --name mysqltest -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest --character-set-server=utf8mb4
```

其中，-p 宿主端口：镜像端口   实现端口映射，或使用 -P 随机端口映射，

--name 容器的名字

-e MYSQL_ROOT_PASSWORD=123456   官方推荐必须使用密码 ，为了方便安全使用，给新建的mysql容器设置初始密码（用户默认root）

-d  打印完整的镜像sha值

mysql:latest   镜像的名称：版本，或者使用镜像id

--character-set-server=utf8mb4   设置编码方式为utf8



3. 该镜像默认只能localhost连接，因此需要进入容器中修改配置

```
# docker exec -it 9c747980d6e1 bash
```

9c747980d6e1为容器的id

进入mysql容器中，更改mysql中user表中的默认设置

```
# mysql -uroot -p123456
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '123456';
mysql> alter user 'root'@'%' identified with mysql_native_password by '123456';
mysql> flush privileges;
```

配置更改后可以尝试重启容器，但是不重启也已经可以远程连接了。