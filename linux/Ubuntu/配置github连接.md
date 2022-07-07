# ubuntu环境下配置git连接github

通过ssh连接github上的账户

## Git本地环境配置

### 1. 安装git

```
sudo apt-get install git
```

### 2. 配置用户信息

```
$ git config --global user.name "vnightray"
$ git config --global user.email "vessalius_oz@163.com"
```

### 3. 初始化本地仓库配置

```
git init
```

## 通过SSH连接Github

### 1. 安装SSH

```
sudo apt-get install ssh
```

首先 ssh-keygen 会确认密钥的存储位置和文件名（默认是 .ssh/id_rsa）,然后他会要求你输入两次密钥口令，留空即可。所以一般选用默认，全部回车即可。

### 2. 创建密钥文件

```
ssh-keygen -t rsa -C "vessalius_oz@163.com"
```

默认密钥文件路径在`~/.ssh`，`id_rsa`是私钥文件，`id_rsa.pub`是公钥文件

### 3. 将公钥添加到Github

1. 将`id_rsa.pub`文件内容全部复制
2. 登陆到GitHub上，右上角小头像->Setting->SSH and GPG keys中，点击new SSH key。

### 4. SSH测试

```
ssh -T git@github.com
```

如果结果为 “ ...You've successfully authenticated, but GitHub does not provide shell access”，则说明成功。

### 5. 设置远程仓库

```
git remote add origin git@github.com:Username/Repositories_Name.git
```

*如果手误输错，可通过git remote remove origin命令删除该远程仓库。*

### 6.最终测试

1. 在本地创建更改
2. `git add xxx`
3. `git commit -m "xxxxxx"`
4. `git push origin master`

