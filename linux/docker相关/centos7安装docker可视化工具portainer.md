# centos7安装docker可视化工具portainer

1. 安装docker
2. 查询当前有哪些portainer镜像

```
# 查询当前有哪些Portainer镜像
docker search portainer
```

3. 下载镜像

```
# 下载镜像
docker pull portainer/portainer
```

4. 下载中文包

```
git clone https://gitee.com/yos/portainer-cn.git
# 或者把本地的包放入服务器
cd portainer-cn
# 将portainer-cn重命名为public
mv portainer-cn/ public/
# 创建volume （虽然不晓得为什么）
docker volume create portainer_data
```



5. 因为仅有一个docker宿主机，使用单机版运行 选择local即可，多个docker集群可以选择其他选项

```
# -p 端口映射
docker run --name portainer -d \
--restart=always -p 9000:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /home/kimihiro/packages/docker_zh/public/:/public \
portainer/portainer
```



6. 浏览器访问ip:9000

```
http://139.224.194.160:9000/
```



7. 创建新账户，输入用户名密码
8. 登陆界面，可以综合管理docker镜像