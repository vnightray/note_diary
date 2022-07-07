# centos7防火墙命令

```
#开启防火墙
systemctl start firewalld.service

#关闭防火墙
systemctl stop firewalld.service

#防火墙状态
systemctl status firewalld.service

#重启防火墙
firewall-cmd --reload

#查看端口的开放情况
firewall-cmd --list-all
```

```
配置防火墙使得HTTP流量、HTTPS流量能够顺利通过防火墙，并阻挡其他可疑流量
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https --permanent
firewall-cmd --add-port=80/tcp --permanent
```

