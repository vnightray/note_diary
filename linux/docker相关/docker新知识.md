# docker新知识（零散）

-it：-i和-t的结合，感觉就是如下图，直接进入容器的命令行模式。

–name：自定义容器名称，不用的话会自动分配一个名称。

-v： 将本地文件夹~/PycharmProjects/text_similar与容器文件夹/root/text_similar共享。

python:3.7.4：要运行的镜像名+TAG

bash：进入容器命令行。

```
docker run -it --name pytest -v ~/PycharmProjects/text_similar:/root/text_similar python:3.7.4 bash
```

执行上面命令后，就像进入了一个linux系统了，这里的所有操作都是在当前docker容器中。当然如果用rm -rf删除共享文件夹里边内容，那本地电脑里边的也就真删了。