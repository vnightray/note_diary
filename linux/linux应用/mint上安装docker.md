# Mint上安装docker

## 在[Linux Mint 20](https://www.yundongfang.com/Yuntag/linux-mint-20) Ulyana上[安装Docker](https://www.yundongfang.com/Yuntag/%e5%ae%89%e8%a3%85docker)

步骤1.在运行以下教程之前，重要的是通过`apt`在终端中运行以下命令来确保系统是最新的：

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

步骤2.在[Linux Mint 20](https://www.yundongfang.com/Yuntag/linux-mint-20)上[安装Docker](https://www.yundongfang.com/Yuntag/%e5%ae%89%e8%a3%85docker)。

- **使用Snap安装Docker：**

运行以下命令以安装Snap软件包和Docker：

```
sudo rm /etc/apt/preferences.d/nosnap.pref
sudo apt update
sudo apt install snapd
```

要[安装Docker](https://www.yundongfang.com/Yuntag/%e5%ae%89%e8%a3%85docker)，只需使用以下命令：

```
sudo snap install docker
```

- **从官方来源安装Docker：**

现在我们将Docker官方密钥添加到您的Linux Mint系统中：

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

接下来，将Docker存储库添加到Linux Mint：

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(. /etc/os-release; echo "$UBUNTU_CODENAME") stable"
```

然后，使用以下命令[安装Docker](https://www.yundongfang.com/Yuntag/%e5%ae%89%e8%a3%85docker) CE：

```
sudo apt update
sudo apt install docker-ce
```

安装后，将创建一个docker组。将您的用户添加到将要运行docker命令的组中：

```
sudo usermod -aG docker $USER
```

之后，使用以下命令验证Docker安装：

```
sudo docker --version
```

Docker的语法如下所示：

```
$ docker help
```

