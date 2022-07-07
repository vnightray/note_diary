# 基于docker镜像在centos7上搭建wordpress私人博客

最近入手了一台阿里云ECS，就寻思着搭建个个人博客，记录自己的一些技术研究，技术只有记下来才是属于你的，不记下来只是暂时属于你的。

一开始并不想把事情搞复杂，安装docker，运行wordpress官方docker镜像，一键完成部署。

```
docker run --name blog -p 80:80 -d wordpress
```

wordpress初次启动会有引导界面，通过引导界面输入数据库信息，初始化完数据库后就就完成了安装。

安装是非常方便，但问题也随之而来。

官方docker镜像没有中文版，后台管理界面是英文倒也不影响，但博客首页默认变成了英文，非常影响阅读，可以安装中文包，但需要修改`wp-config.php`文件，但由于wordpress源码打包在docker镜像里，修改起来较为复杂。因此决定放弃docker镜像，直接基于源码安装。在docker环境下基于源码安装需要以下几个镜像的配合：

- php cgi docker镜像的选择
- 负载均衡服务器的选择
- 数据库

在wordpress[中文网站](https://link.segmentfault.com/?enc=ytJ3uCf%2BiXBR%2BlbhZvi9fQ%3D%3D.CmpcS%2FeC8vT%2BJ%2BcIFtJQJvJugt5kue4RySChMKQkFuQ%3D)下载最新wordpress源码，我下载的是最新版本[WordPress 5.2.4](https://link.segmentfault.com/?enc=IVNgEORBmndnfGWdv%2BCQ3g%3D%3D.SK9dC3rid34Zh%2FbjWdV%2B8s4sXCTCbeDoMWMEqvS9CEXcgNvnDFUxv%2FL9oZlULEp%2B)，官网对WordPress 5.2.4要求PHP 7.3或者更高，MySQL 5.6或更高，在选择docker镜像的时候需要注意下，不然无法运行。

> wordpress所有的官网打开都是429错误`429 Too Many Requests`，起初我以为是我网络的问题，但无论怎么切换网络都是429，我觉得这个问题已经超出技术范围，技术解决不了政治问题，429解决的办法只能是多刷新几次，直到刷出来为止。

## PHP

PHP源码需要php cgi支持，在dockerhub上找到了[php官方镜像](https://link.segmentfault.com/?enc=qUBGfyVftcR52Wy4NWZaCw%3D%3D.0boQWhnKVQzRyQ4zp2sBLUQNZjA177TGzpCGmLoDnVg%3D)，在tag里搜索fpm，可以查看各个 php-fpm版本，PHP-FPM(PHP FastCGI Process Manager)是为了更好的管理php-fastcgi而实现的一个程序，可以接收web服务器请求处理php脚本。

我选择了`php:7.3-fpm`作为wordpress运行环境。

根据dockerhub上相关介绍，需要自行构建包含mysql模块的fpm镜像，通过工具`docker-php-ext-install`加入mysql模块，步骤也很简单，写一个Dockerfile文件即可。

```
# mkdir -p /home/kimihiro/data/fpmx
# cd /home/kimihiro/data/fpmx
# touch Dockerfile
# vim Dockerfile

```

```
### Dockerfile中写入如下命令

FROM php:7.3-fpm
RUN docker-php-ext-install mysqli pdo pdo_mysql
```

保存为`Dockerfile`，执行以下命令构建新镜像。

```
### 当前工作目录为/home/kimihiro/data/fpmx
docker build -t php:7.3-fpmx .
```

这里取了一个名字`php:7.3-fpmx`.

重新运行

```
docker run --name php -v /home/kimihiro/data/wordpress:/var/www/html -d php:7.3-fpmx
```

## nginx

解决了php运行环境的问题，就要搭建负载均衡服务器，毕竟php只是cgi，还需要一个web服务器的支持。这里选择了nginx作为web服务器。



因为本机虽然安装了nginx，但是没有docker的nginx的镜像，因此直接在dockerhub里面拉取了最新的nginx的镜像，然后可以跟着教程继续下去。

```
# docker pull nginx
# docker run --name nginx -v /home/kimihiro/data/nginx/conf.d:/etc/nginx/conf.d -v /home/kimihiro/data/wordpress:/usr/share/nginx/html --link php:php -p 8096:80 -d nginx
```



目录`/home/kimihiro/data/nginx/conf.d`下的配置文件`default.conf`如下

```
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index index.php index.html;

    }
    
    location ~ \.php$ {
        fastcgi_pass   php:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /var/www/html/$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```

- index index.php 设置根目录默认请求文件名。
- `/var/www/html/`是php-fpm容器内的路径而非文件系统中的路径。

启动nginx后就可以通过nginx进行访问。

## 数据库

之前学习mysql时，拉取过docker的mysql的镜像，也启动了一个容器（container），该容器一直正常运行，name为mysqltest。映射的端口是3307:3306



如下为初次启动mysql镜像的操作过程：

wordpress初次启动后需要输入数据库信息，如果没有可以当场搭建一个mysql数据库服务器。

```
# docker run --name mysql -p 3307:3306 -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password -d mysql

# docker exec -it mysql bash
root@ac169daba7b0:/# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 798
Server version: 8.0.18 MySQL Community Server - GPL
```



因为本机已经有一个mysqltest的mysql容器，以上步骤跳过，直接进行一下操作：



这里用的是`8.0.18`版本的mysql，可以通过docker exec命令进入mysql容器内，创建用户和数据库，脚本可以参考如下：

```
CREATE DATABASE  wordpress default charset utf8 COLLATE utf8_general_ci;

#######################################
此处的'password', 本人替换为'blog1122',可以不替换，也可以替换成自己的password
#######################################

CREATE USER 'wordpress'@'%' IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpress'@'%';

ALTER USER 'wordpress'@'%' IDENTIFIED WITH mysql_native_password;
alter user 'wordpress'@'%' identified by 'password'

FLUSH   PRIVILEGES;
```

wordpress源码提供了一个配置文件的参考代码`wp-config-sample.php`，可以直接复制并重命名为`wp-config.php`。修改数据库连接信息

```
# cd/home/kimihiro/data/wordpress
# cp wp-config-sample.php wp-config.php
# vim wp-config.php
```



```
### 
在wp-config.php中修改如下内容，其中'password'为上面设置的password，本人的是'blog1122',其他内容不做变动
###

define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpress' );
define( 'DB_PASSWORD', 'password' );
define( 'DB_HOST', 'db:3306' );
```

这里`db:3306`用别名表示数据库，方便记忆和书写，可以通过docker命令 --link设置，link可以实现docker容器内互相通信。安装完mysql后重启php：

```
docker rm -f php
docker run --name php -v /home/kimihiro/data/wordpress:/var/www/html --link mysqltest:db -d php:7.3-fpmx
```

## 主题

将主题zip包复制到源码目录`wp-content/themes`下解压后，在wordpress的后台管理界面就可以看到主题。这里推荐一款比较喜欢的主题，[coogee](https://link.segmentfault.com/?enc=2BVQrj0Tel%2BiGngfxb3JlA%3D%3D.gk4C77norYfaShaGfTRtosQB2LKU6L2vYC80c899cl4ZlTENRN6xuauiHUxGL9QC)，可以直接点击[这里](https://link.segmentfault.com/?enc=HQSNnRpFlP%2FRUGWw0fC6cg%3D%3D.nBbfBS4Iv1Pih3Jy90FJYUuyiyw2%2FmohBx%2FruJ6oX815Lmqor8saTorcDoRJSvc8UUDuYTPrgKuaXTZAJ%2FX7QA%3D%3D)进行下载。

## 插件

将插件zip包复制到源码目录`wp-content/plugins`下解压后，在wordpress的后台挂你就可以看到插件列表，对于技术博客，离不开的插件就是代码高亮，这里推荐一款代码高亮的插件[Google Syntax Highlighter](https://link.segmentfault.com/?enc=67nPiPGz9WlQQgB6l48D3Q%3D%3D.lRlCyAJ7dABbmVAUpMrfO4d72qNUvsOisv39Z4Rv2afodM3F5RT%2FeDZDUJTb9oqFKUgY169wF2eqV7nmavmF4Q%3D%3D)，点击[这里](https://link.segmentfault.com/?enc=jssPNRF3gV2agNLxgfdf1A%3D%3D.IUSpnQDwWCLyF4lmZ%2B4b6RQwBBUs3ICd39FLxhA%2BSBYxU%2FXXc%2BU%2FSeERDAHyJpvTS65yBoNAywmD64kLXP7uhyjEBSs%2FapVLLFpZoqB8rmY%3D)可以直接下载。

## 写在最后

搭建一个个人博客并不是一件容易的事，还需要有一个域名，有了域名后还需要备案，如果你有过备案的经历你就懂了，还需要解决markdown到wordpress的转换，目前用了几个插件都不是很理想，主要是代码无法兼容高亮插件，这一点比较头疼，自己有过冲动想自己写一个markdown到wordpress html的转换程序，这当然是后话了。