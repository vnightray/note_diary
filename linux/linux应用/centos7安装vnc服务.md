# centos7安装vnc服务

VNC需要系统安装的有桌面，如果是生产环境服务器，安装时使用的最小化安装，那么进行下面操作按章GNOME 桌面。

```
yum groupinstall -y "GNOME Desktop" 

systemctl set-default graphical.target  //设置成图形模式 

systemctl set-default multi-user.target  //设置成命令模式 
```

第一步：安装VNC服务软件，使用root用户执行以下命令（以下操作没有特别说明均在root用户）：

```
yum install tigervnc-server -y
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-1b494dcd20cae9a4e77477f27e70ae73_720w.png)

安装后可以使用如下命令来验证是否安装成功：

```
rpm -qa|grep tigervnc-server
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-d461d827c8ecf53ed62130a6ec86bbad_720w.png)

第二步：复制vnc的启动操作脚本, vncserver@:1.service中的：1表示"桌面号"，启动的端口号就是5900+桌面号，即是5901，如果再有一个就是2啦，端口号加1就是5902，以此类推：

```
cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
```

第三步：编辑 /etc/systemd/system/vncserver@:1.service

```
vim /etc/systemd/system/vncserver@\:1.service
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-d3242161180481fc1ab285d955925791_720w.png)

vnc配置文件修改前

找到其中的<USER> ，修改成自己的用户名，如果是root用户登录桌面就使用root用户，如果使用普通用户登录桌面使用普通用户，这里笔者使用用户名：cy

![img](D:\often-used materials\学习笔记\linux\pict\v2-326cf4f022b7ee0158dcf0bfd64f9600_720w.png)

vnc配置文件修改后

修改完毕后保存退出vim。

第四步：设置vnc密码，执行su cy，切换到刚配置文件设置的cy用户，执行（这一步是在cy用户下操作），输入两次密码，输入完成后会提示是否设置view-only password（*“View-only* *password”密码，*只允许查看,无控制权限。）这个可设可不设：

```
vncpasswd
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-c1fc213b502a2ed597d3e65deb2ce057_720w.png)

第五步：启动服务：

```
systemctl start vncserver@\:1.service
```

第一次输入启动服务命令可能会要求输入（从新加载配置文件，新增和配置文件发生变化时都需要执行 daemon-reload 子命令）：

```
systemctl daemon-reload
```

执行完毕之后在执行启动命令就可以了：

![img](D:\often-used materials\学习笔记\linux\pict\v2-aa5cb4e4c39f99d6dc99d7b0748c20d1_720w.png)

可以加入开机启动，下次开机就会自动启动啦：

```
systemctl enable vncserver@\:1.service
```

第六步：查看端口是否监听：

```
netstat -lnpt|grep Xvnc
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-6ec82e73faa684e27793b51e7fa7fa26_720w.png)

这里我们可以看到5901端口已经被监听

第七步：开放防火墙的5901端口：

```
firewall-cmd --zone=public --add-port=5901/tcp --permanent
```

如果防火墙没有启动需要先启动防火墙。

![img](D:\often-used materials\学习笔记\linux\pict\v2-872dac62dc4d4575eb502561342f7747_720w.png)

当然也可以狠一点，直接停止防火墙：

```
systemctl stop firewalld.service
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-fb30524253a307513825d9c48f5e446a_720w.png)

停止之后该需要禁止开机启动：

```
systemctl disable firewalld.service
```

第八步：关闭SELinux，编辑/etc/selinux/config 文件：

```
vim /etc/selinux/config
```

![img](D:\often-used materials\学习笔记\linux\pict\v2-53fb8a7e7ad6025d12e41701bde1093b_720w.png)

将selinux设置为disabled

![img](D:\often-used materials\学习笔记\linux\pict\v2-904e1b483705c6fe617838a031e1eaa2_720w.png)

到这里vnc服务已经安装完毕，下面就可使用vnc客户端来连接。

第九步：在vnc客户端（vnc viewer）输入服务器IP:桌面号（如192.168.31.100:1），输入后回车：

![img](D:\often-used materials\学习笔记\linux\pict\v2-2d51dd324cdc8b5e2d4b41ec3d6b3eb3_720w.png)

第十步：输入IP后会弹出确认，点击contiue即可：

![img](D:\often-used materials\学习笔记\linux\pict\v2-93a699d47c634dac01dfe630692eeceb_720w.png)

第十一步：输入vnc密码：

![img](D:\often-used materials\学习笔记\linux\pict\v2-897b686d1f8dbb6a82a8df696a375789_720w.png)

第十二步：登录成功，输入远程机器密码（登录成功后需要输入远程机器的用户的密码，如果没有密码就可以直接进入系统）：

![img](D:\often-used materials\学习笔记\linux\pict\v2-1eedbeb0dc745dc00a4f818ea9138cb5_720w.png)

第十三步：成功进入远程桌面：

![img](D:\often-used materials\学习笔记\linux\pict\v2-452a51f17b8c293843e07ed9a75a400e_720w.png)

至此整个CentOS7.x 的VNC服务安装完毕^_^。

小贴士：vnc服务只能在局域网使用，如果在外网，则需要有公网IP地址，VNC不仅具备内网穿透功能。