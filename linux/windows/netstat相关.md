# 通过netstat查看端口占用情况

查找所有运行的端口

```
netstat -ano
```



查看被占用端口对应的PID

```
netstat -aon|findstr "8081"
```



查看指定PID的进程

```
tasklist|findstr "9088"
```



结束进程

强制（/F参数）杀死 pid 为 9088 的所有进程包括子进程（/T参数）： 

```
taskkill /T /F /PID 9088 
```

