[toc]

## NoSQL

即Not-OnlySQL（泛指非关系型的数据库）作为关系型数据库的补充

作用：应对基于海量用户和海量数据前提下的数据处理问题

### 特征：

* 可扩充，可伸缩
* 大数据量下高性能
* 灵活的数据模型
* 高可用

### 常用Nosql数据库：

* Redis
* memcache
* HBase
* MongoDB

应用场景

1.商品基本信息。  Mysql

* 名称
* 价格
* 厂商

2.商品附加信息（文字）MongoDB

* 描述
* 详情
* 评论

3.图片信息。  分布式文件系统

4.搜索关键字。   ES、Lucene、solr

5.热点信息。Redis、memcache、tair

* 高频
* 波段性

## Redis

概念：Redis(REmote Dictionary Server)是用C语言开发的一个开源的高性能键值对(key-value)数据库

特征：

1. 数据间没有必然的关联关系
2. 内部采用单线程机制进行工作
3. 高性能
4. 多数据类型支持
   * 字符串类型	string
   * 列表类型        list
   * 散列类型。   hash
   * 集合类型。   set
   * 有序集合类型    sorted_set
5. 持久化支持，可以进行数据灾难恢复

## Redis的应用

* 为热点数据加速查询，如：热点商品，新闻，资讯等
* 任务队列，如秒杀、抢购、购票排队等
* 即时信息查询，如排行榜，访问统计，公交到站信息等
* 时效性信息控制，如：验证码，投票
* 分布式数据共享
* 消息队列
* 分布式锁

## Redis文件结构

2个配置文件conf

3个说明文档docx

4个可执行文件

* redis-server.exe>>启动Redis服务端
* redid-check-aof.exe>>用于持久化
* redis-cli.exe>>用于使用redis客户端 
* reds-benchmark.exe>>用于性能测试

## 启动服务端之后

默认端口号port:6379

PID：随机生成

## 启动客户端之后

127.0.0.1:6379>

## 命令行模式工具使用

* 功能性指令
  * 信息添加
    * 功能：设置key,value数据
    * 命令：set key value
    * 范例：set name Song
  * 信息查询
    * 功能：根据key查询相应的value，如果不存在，返回空(nil)
    * 命令：get key
    * 范例：get name>>Song
  * 帮助
    * 功能：获取命令帮助文档，获取组中所有命令信息名称
    * 命令
      * help 命令名称
      * help @组名
    * 范例：help get
  * 退出客户端命令行模式
    * 功能：退出客户端
    * 命令：
      * quit
      * exit
      * \<ESC\>