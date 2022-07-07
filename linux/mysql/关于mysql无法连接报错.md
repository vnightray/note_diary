#关于安装好mysql之后无法连接报错问题

MySql错误 1251 - Client does not support authentication protocol requested by server 解决方案

命令行登陆mysql之后，输入如下命令：

```
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '密码';
mysql> flush privileges;
#其中， 密码是自己的数据库root对应的密码
```

