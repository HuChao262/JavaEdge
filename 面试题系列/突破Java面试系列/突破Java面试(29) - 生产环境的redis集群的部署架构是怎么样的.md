# 1 面试题
生产环境中的redis是怎么部署的？

# 2 考点分析
看看你了解不了解你们公司的redis生产集群的部署架构，如果你不了解，那么确实你就很失职了，你的redis是主从架构？集群架构？用了哪种集群方案？有没有做高可用保证？有没有开启持久化机制确保可以进行数据恢复？线上redis给几个G的内存？设置了哪些参数？压测后你们redis集群承载多少QPS？

兄弟，这些你必须是门儿清的，否则你确实是没好好思考过

# 3 面试详解
## 3.1 redis cluster
10台机器，5台机器部署了redis主节点，另外5台机器部署了redis的从节点
每个主节点挂了一个从节点，5个节点对外提供读写服务，每个节点的读写高峰QPS可能可以达到每秒5万，5台机器最多是25万个QPS.

## 3.2 机器配置
32G内存+8核CPU+1T磁盘，但是分配给redis进程的是10g内存，一般线上生产环境，redis的内存尽量不要超过10g，超过10g可能会有问题。

5台机器对外提供读写，一共有50g内存。

因为每个主实例都挂了一个从实例，所以是高可用的，任何一个主实例宕机，都会自动故障迁移，redis从实例会自动变成主实例继续提供读写服务

## 3.3 你往内存里写的是什么数据？每条数据的大小是多少？
商品数据，每条数据是10kb。100条数据是1mb，10万条数据是1g。常驻内存的是200万条商品数据，占用内存是20g，仅仅不到总内存的50%。

目前高峰期每秒就是3500左右的请求量

# X 交流学习
![](https://img-blog.csdnimg.cn/20190504005601174.jpg)
## [Java交流群](https://jq.qq.com/?_wv=1027&k=5UB4P1T)
![](https://img-blog.csdnimg.cn/20190502142519844.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTg5NTEw,size_16,color_FFFFFF,t_70)

## [博客](http://www.shishusheng.com)

![](https://img-blog.csdnimg.cn/20190502142541289.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTg5NTEw,size_16,color_FFFFFF,t_70)

## [Github](https://github.com/Wasabi1234)
