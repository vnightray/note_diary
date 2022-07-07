# Ubuntu vmware tools安装与彻底卸载

参考资料：https://blog.csdn.net/weixin_43445661/article/details/109497763



一、安装

参考文章：https://blog.csdn.net/love20165104027/article/details/83377758

点击虚拟机——安装VMware Tools

这时候Ubuntu里边会显示出DVD的图标，我理解的是相当于VMware在你的Ubuntu里边插了一个光盘


将VMwareTools-10.3.21.14772444.tar.gz压缩包拷贝到根目录的opt文件夹里


拷贝到opt文件夹需要root权限，我是先把他拷贝到别的文件夹，在再命令行移到opt文件夹里的

$ sudo mv  源路径  /opt(目标路径)

进入opt目录，解压缩

$ cd /opt
$ tar -xzvf VMwareTools-10.3.21.14772444.tar.gz


解压缩之后


安装

$ cd /opt
$ cd vmware-tools-distrib
$ sudo ./vmware-install.pl



之后一路按Enter选择默认就好啦

二、彻底卸载
参考文章：https://blog.csdn.net/zerolity/article/details/81206476

vmtools在重装之前要完全卸载，不然又会出现各种奇奇怪怪的bug
步骤如下：（具体见上参考文章，我是无脑都运行了一遍然后就卸干净了）

sudo vmware-uninstall-tools.pl
sudo apt-get remove open-vm-tools
sudo apt-get remove --auto-remove open-vm-tools
sudo apt-get purge open-vm-tools
sudo apt-get purge --auto-remove open-vm-tools
