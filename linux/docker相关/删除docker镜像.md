# docker镜像删除

然后上网需求方法，主流的方法有两种

方法一：强制删除镜像

```
docker rmi -f 8f5116cbc201
Error response from daemon: conflict: unable to delete 8f5116cbc201 (cannot be forced) - image has dependent child images
```

以失败告终。。。

```
方法二：批量删除容器，再删除镜像

# 停止所有容器
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop

# 删除所有容器
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm

# 删除所有none镜像
docker images|grep none|awk '{print $3 }'|xargs docker rmi
```

还是以失败告终。。。。。



搜了很久，发现其实是因为TAG的问题，即有其他 image FROM 了这个 image，可以使用下面的命令列出所有在指定 image 之后创建的 image 的父 image

## **方案**

**先查询依赖**

```
docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' $(docker image ls -q --filter since=XXX)    # XXX指镜像ID
```

**然后根据根据TAG删除容器**

```
docker rmi REPOSITORY:TAG
```





一次性刪除所有容器的方法：

```
#docker rm `docker ps -a -q`
## -q表示只返回容器的id，其他信息过滤。
```

