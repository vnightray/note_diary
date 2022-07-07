# 20个crontab示例

## 如何添加/编辑 Crontab

要在 crontab 中添加或更新作业，请使用以下命令。它将在编辑器中打开一个 crontab 文件，可以在其中添加/更新作业。

```
crontab -e
```

默认情况下，它将编辑当前登录用户的 crontab 条目。要编辑其他用户 crontab 使用命令如下

```
crontab -u username -e
```

## 如何列出 Crontab

要查看当前用户的 crontab 条目，请使用以下命令。

```
crontab -l
```

使用 -u 后跟用户名来查看指定用户的 crontab 条目。

```
crontab -u username -l
```

## 20 个有用的 Crontab 示例

以下是使用 crontab 在 Linux 系统中调度 cron 作业的示例列表。

##### 1. 安排一个 cron 在每天凌晨 2 点执行。

这对于每天安排数据库备份很有用。

```
0 2 * * * /bin/sh bashup.sh
```

- 星号 (*) 用于匹配所有记录。

##### 2. 安排一个 cron 每天执行两次。

下面的示例命令将在每天上午 5 点和下午 5 点执行。您可以通过逗号分隔指定多个时间戳。

```
0 5,17 * * * /scripts/script.sh
```

##### 3. 安排一个 cron 每分钟执行一次。

通常，我们不需要每分钟执行任何脚本，但在某些情况下，您可能需要对其进行配置。

```
* * * * * /scripts/script.sh
```

##### 4. 安排一个 cron 在每周日下午 5 点执行。

这种类型的 cron 可用于执行每周任务，例如日志轮换等。

```
0 17 * * sun /scripts/script.sh
```

##### 5. 安排一个 cron 每 10 分钟执行一次。

如果你想以 10 分钟的间隔运行你的脚本，你可以像下面这样配置。这些类型的 cron 可用于监控。

```
*/10 * * * * /scripts/monitor.sh
```

> `*/10`表示每 10 分钟运行一次。就像你想每 5 分钟执行一次一样，使用 `*/5`。

##### 6. 安排一个 cron 在选定的月份执行。

有时我们需要安排一个任务只在选定的月份执行。下面的示例脚本将在 1 月、5 月和 8 月运行。

```
* * * jan,may,aug * /script/script.sh
```

##### 7. 安排一个 cron 在选定的日期执行。

如果您需要安排任务仅在选定的日期内执行。下面的示例将在每个星期日和星期五下午 5 点运行。

```
0 17 * * sun,fri /script/script.sh
```

##### 8. 安排一个 cron 在每个月的第一个星期日执行。

无法通过时间参数安排脚本仅在第一个星期天执行脚本，但我们可以使用命令字段中的条件来执行此操作。

```
0 2 * * sun [ $(date +%d) -le 07 ] && /script/script.sh
```

##### 9. 安排一个 cron 每四个小时执行一次。

如果您想以 4 小时的间隔运行脚本。它可以像下面这样配置。

```
0 */4 * * * /scripts/script.sh
```

##### 10. 安排一个 cron 在每个星期日和星期一执行两次。

将任务安排为仅在周日和周一执行两次。使用以下设置来做到这一点。

```
0 4,17 * * sun,mon /scripts/script.sh
```

##### 11. 安排一个 cron 每 30 秒执行一次。

无法通过时间参数安排每 30 秒执行一次任务，但可以通过安排相同的 cron 两次来完成，如下所示。

```
* * * * * /scripts/script.sh
* * * * * sleep 30; /scripts/script.sh
```

##### 12. 在单个 cron 中安排多个任务。

使用单个 cron 配置多个任务，可以通过用分号 (;) 分隔任务来完成。

```
* * * * * /scripts/script.sh;/scripts/scrit2.sh
```

##### 13. 安排任务每年执行（@yearly）。

@yearly 时间戳类似于`0 0 1 1 *`。它会在每年的第一分钟执行一项任务，发送新年问候可能有用

```
@yearly /scripts/script.sh
```

##### 14. 安排任务每月执行（@monthly）。

@monthly 时间戳类似于`0 0 1 * *`。它将在每月的第一分钟执行任务。每月执行诸如支付账单和向客户开具发票等任务可能会很有用。

```
@monthly /scripts/script.sh
```

##### 15. 安排每周执行的任务 (@weekly)。

@weekly 时间戳类似于`0 0 * * mon`。它将在一周的第一分钟执行一项任务。执行每周任务（例如系统清理等）可能很有用。

```
@weekly /bin/script.sh
```

##### 16. 安排每天执行的任务 (@daily)。

@daily 时间戳类似于`0 0 * * *`。它将在每天的第一分钟执行一项任务，它可能有助于完成日常任务。

```
@daily /scripts/script.sh
```

##### 17. 安排任务每小时执行一次（@hourly）。

@hourly 时间戳类似于`0 * * * *`。它将在每小时的第一分钟执行一项任务，执行每小时任务可能很有用。

```
@hourly /scripts/script.sh
```

##### 18. 安排在系统重启时执行的任务 (@reboot)。

@reboot 对于您希望在系统启动时运行的任务很有用。它将与系统启动脚本相同。它对于在后台自动启动任务很有用。

```
@reboot /scripts/script.sh
```

##### 19. 将 Cron 结果重定向到指定的电子邮件帐户。

默认情况下，cron 将详细信息发送到调度 cron 的当前用户。如果您想将其重定向到您的其他帐户，可以通过设置 MAIL 变量来完成，如下所示

```
crontab -l
0 2 * * * /script/backup.sh
```

##### 20. 将所有 cron 备份到纯文本文件。

我建议将所有作业条目的备份保存在一个文件中。这将帮助您在意外删除的情况下恢复 cron。

**检查当前计划的 cron：**

```
crontab -l
0 2 * * * /script/backup.sh
```

**将 cron 备份到文本文件：**

```
crontab -l > cron-backup.txt
cat cron-backup.txt
0 2 * * * /script/backup.sh
```

**删除当前计划的 cron：**

```
crontab -r
crontab -l
no crontab for root
```

**从文本文件恢复 cron：**

```
crontab -l
0 2 * * * /script/backup.sh
```