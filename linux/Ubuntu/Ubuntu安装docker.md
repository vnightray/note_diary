# Ubuntu安装docker

## **第1步 - 安装Docker**

官方Ubuntu存储库中提供的Docker安装包，但是可能不是最新的版本。为了确保我们获得最新版本，我们将从官方Docker存储库安装Docker。为此，我们将添加一个新的资源包，从Docker添加GPG密钥以确保下载有效，然后安装该包。

首先，更新现有的包列表：

```
sudo apt update
```

接下来，使用`apt`安装一些允许通过HTTPS才能使用的软件包：

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

然后将官方Docker存储库的GPG密钥添加到您的系统：

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

将Docker存储库添加到APT源：

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

接下来，使用新添加的repo源中的Docker包更新包数据库：

```
sudo apt update
```

确保您要从Docker repo安装而不是默认的Ubuntu repo：

```
apt-cache policy docker-ce
```

虽然Docker的版本号可能不同，但您还是会看到这样的输出：

```
docker-ce:
  Installed: (none)
  Candidate: 18.03.1~ce~3-0~ubuntu
  Version table:
     18.03.1~ce~3-0~ubuntu 500
        500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
```

现在`docker-ce`还没有安装，用上面这个命令我们能看到安装源来自的Docker官方存储库。

最后，安装Docker：

```
sudo apt install docker-ce
```

现在应该安装好Docker了，检查它是否正在运行：

```
sudo systemctl status docker
```

输出应类似于以下内容，表明该服务处于工作状态：

```
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2018-07-05 15:08:39 UTC; 2min 55s ago
     Docs: https://docs.docker.com
 Main PID: 10096 (dockerd)
    Tasks: 16
   CGroup: /system.slice/docker.service
           ├─10096 /usr/bin/dockerd -H fd://
           └─10113 docker-containerd --config /var/run/docker/containerd/containerd.toml
```

Docker不仅可以为您提供Docker服务，还可以为您提供docker[命令行工具](https://cloud.tencent.com/product/cli?from=10680)或Docker客户端。我们将在本教程后面探讨如何使用docker命令。