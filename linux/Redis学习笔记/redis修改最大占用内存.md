Redis占用内存大小

我们知道Redis是基于内存的key-value数据库，因为系统的内存大小有限，所以我们在使用Redis的时候可以配置Redis能使用的最大的内存大小。

1、通过配置文件配置

通过在Redis安装目录下面的redis.conf配置文件中添加以下配置设置内存大小。

//设置Redis最大占用内存大小为100M
　　maxmemory 100mb
　　redis的配置文件不一定使用的是安装目录下面的redis.conf文件，启动redis服务的时候是可以传一个参数指定redis的配置文件的。伊人在线：www.sheonline.cn

2、通过命令修改

Redis支持运行时通过命令动态修改内存大小。

//设置Redis最大占用内存大小为100M
　　127.0.0.1:6379> config set maxmemory 100mb
　　//获取设置的Redis能使用的最大内存大小
　　127.0.0.1:6379> config get maxmemory
　　如果不设置最大内存大小或者设置最大内存大小为0，在64位操作系统下不限制内存大小，在32位操作系统下最多使用3GB内存
　　Redis的内存淘汰

既然可以设置Redis最大占用内存大小，那么配置的内存就有用完的时候。那在内存用完的时候，还继续往Redis里面添加数据不就没内存可用了吗？

实际上Redis定义了几种策略用来处理这种情况：

noeviction(默认策略)：对于写请求不再提供服务，直接返回错误（DEL请求和部分特殊请求除外）。

allkeys-lru：从所有key中使用LRU算法进行淘汰。

volatile-lru：从设置了过期时间的key中使用LRU算法进行淘汰。

allkeys-random：从所有key中随机淘汰数据。

volatile-random：从设置了过期时间的key中随机淘汰。

volatile-ttl：在设置了过期时间的key中，根据key的过期时间进行淘汰，越早过期的越优先被淘汰。

当使用volatile-lru、volatile-random、volatile-ttl这三种策略时，如果没有key可以被淘汰，则和noeviction一样返回错误。

如何获取及设置内存淘汰策略

获取当前内存淘汰策略：

127.0.0.1:6379> config get maxmemory-policy
　　通过配置文件设置淘汰策略（修改redis.conf文件）：

maxmemory-policy allkeys-lru
　　通过命令修改淘汰策略：

127.0.0.1:6379> config set maxmemory-policy allkeys-lru
　　LRU算法
————————————————
版权声明：本文为CSDN博主「asade12345」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/asade12345/article/details/103215904