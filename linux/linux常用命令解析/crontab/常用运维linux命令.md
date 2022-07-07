# 常用运维linux命令

### 查找目录下所有以. zip 结尾的文件移动到指定目录

```
find . -name "*.zip" -exec mv {} ./backup/;
```

### 查找当前目录 30 天以前大于 100M 的log文件并删除。

```
find . -name "*.log" –mtime +30 –typef –size +100M | xargs rm –rf {};
```

### 批量解压当前目录下以. zip 结尾的所有文件到指定目录

```
for i  in  `find . –name "*.zip" –type f`
do
  unzip –d $i /data/www/
done
```

> 
>
> 注解:`for i in （command）;do … done` 为 for 循环的一个常用格式，其中i为变量，可以自己指定。

### 写一个脚本查找最后创建时间是 3 天前，后缀是 `*.log` 的文件并删除。

```
find . -mtime +3  -name "*.log" | xargs rm -rf {};
```

### 写一个脚本将某目录下大于 100k 的文件移动至/ tmp下

```
find . -size +100k -exec mv {} /tmp;
```

### 如何判断某个目录是否存在，不存在则新建，存在则打印信息。

```
if [ ! –d /data/backup/ ];then
   mkdir –p /data/backup/
else
   echo  "目录已存在"
fi
```

> -d 代表目录。

### 替换文件中的目录

```
sed 's:/user/local:/tmp:g' test.txt
或者
sed -i 's//usr/local//tmp/g' test.txt
```

### sed 常用命令

```
如何去掉行首的.字符: sed -i 's/^.//g' test.txt

在行首添加一个a字符: sed 's/^/a/g'    test.txt

在行尾添加一个a字符: sed 's/$/a/'     tets.txt

在特定行后添加一个z字符:sed '/rumen/az' test.txt

在行前加入一个c字符: sed '/rumenz/ic' test.txt
```

### sed 另外一个用法找到当前行，然后在修改该行后面的参数

```
sed -i '/SELINUX/s/enforcing/disabled/' /etc/selinux/config
```

> sed 冒号方式

```
sed -i 's:/tmp:/tmp/abc/:g' test.txt意思是将/tmp改成/tmp/abc/。
```

### 统计 Nginx 访问日志 访问量排在前20的ip地址

```
cat access.log |awk '{print $1}'|sort|uniq -c |sort -nr |head -20
```

> 注解：`sort` 排序、`uniq`（检查及删除文本文件中重复出现的行列 ）

### 修改文本中以ab 结尾的替换成 cd：

```
sed -e 's/ab$/cd/g' b.txt
```

### 网络抓包：tcpdump

```
#抓取 56.7 通过80请求的数据包。
tcpdump -nn host 192.168.56.7 and port 80 
```

```
#排除0.22 80端口
tcpdump -nn host 192.168.56.7 or ! host 192.168.0.22 and port 80 
```

### 统计bash_history最常用的 20 条命令

```
history | awk '{print $2}' | sort | uniq -c | sort -k1,1nr | head -10
```

### 配置防火墙脚本，只允许远程主机访问本机的 80 端口

```
iptables -F
iptables -X
iptables -A INPUT -p tcp --dport 80 -j accept
iptables -A INPUT -p tcp -j REJECT
或者
iptables -A INPUT -m state --state NEW-m tcp -p tcp --dport 80 -j ACCEPT
```

