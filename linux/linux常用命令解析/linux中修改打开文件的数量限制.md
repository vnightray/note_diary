# linux中修改打开文件的数量限制



> 在 Linux中你可以更改打开文件的最大数量。你可以使用`ulimit`命令。它使你能够控制可用于 shell 或由它启动的进程的资源。

### 查找 Linux 打开文件限制

```
# cat /proc/sys/fs/file-max

365004
```

> 该值表示每次登录会话可以打开的文件数。不同系统结果可能会有所不同。

> 例如在一个 `CentOS` 我的服务器，限制设置为 `365004`

#### 在 Linux 中检查硬限制

```
# ulimit -Hn

65535
```

#### 检查 Linux 中的软限制

```
# ulimit -Sn

65535
```

> 要查看不同用户的硬值和软值，你可以`su`切换用户查看比较。

> 例如：

```
# su rumenz
$ ulimit -Sn
1024
```

```
$ ulimit -Hn
1024
```

### 如何在 Linux 中检查系统范围的文件描述符限制

> 如果你正在运行服务器，你的某些应用程序可能需要更高的打开文件描述符限制。一个很好的例子是`MySQL/MariaDB` 服务或 `Apache` 网络服务器。

> 你可以通过编辑内核指令来增加 Linux 中打开文件的限制 `fs.file-max`。`sysctl`用于在运行时配置内核参数。

> 例如，将打开文件限制增加到 `500000`，你可以以 root 身份使用以下命令：

```
# sysctl -w fs.file-max=500000
```

> 你可以使用以下命令检查打开文件的当前值：

```
$ cat /proc/sys/fs/file-max
```

> 使用上述命令，你所做的更改将仅在下次重新启动之前保持活动状态。如果你希望永久应用它们，则必须编辑以下文件：

```
# vi /etc/sysctl.conf
```

> 添加以下行：

fs.file-max=500000

> 也可以根据需要更改数量。

```
# cat /proc/sys/fs/file-max
```

> 用户需要注销并重新登录才能使更改生效。如果要立即应用限制，可以使用以下命令：

```
# sysctl -p
```

### 在 Linux 中设置用户级别打开文件限制

> 上面的示例展示了如何设置全局限制，但你可能希望对每个用户应用限制。root 用户需要编辑以下文件：

```
# vi /etc/security/limits.conf
```

> 格式：

```
<domain>        <type>  <item>  <value>
```

> 这是为用户设置软限制和硬限制的示例 `rumenz`用户：

```
## Example hard limit for max opened files
rumenz        hard nofile 4096
## Example soft limit for max opened files
rumenz        soft nofile 1024
```

