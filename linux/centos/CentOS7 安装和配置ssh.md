# CentOS7 安装和配置ssh

- 安装 openssh-server

> ```
> # 输入指令 yum install -y openssl openssh-server  
> ```

- 修改配置文件

> ```
> # 输入指令 vim /etc/ssh/sshd_config 
> # 将 PermitRootLogin, RSAAuthentication, PubkeyAuthentication 设置为 yes  
> ```

- 启动 ssh 的服务

> ```
> # 输入指令 systemctl start sshd.service  
> ```

- 设置开机自动启动 ssh 服务

> ```
> # 输入指令 systemctl enable sshd.service  
> ```

- 设置文件夹 ~/.ssh 的访问权限

> ```
> # 依次输入指令 cd ~ chmod 700 .ssh chmod 600 .ssh/* ls -la .ssh  
> ```

- CentOS7 安装和配置 ssh 成功
- 服务器免密码登录

> ```
> cd .ssh vim authorized_keys 将公钥粘贴其中 esc 后输入 :wq 保存退出 # 完成服务器免密码登录配置
> ```