#### 总结:

1. win7中启动redis步骤: 
   1. 启动redis服务: 解压目录中,打开命令窗口, 输入redis-server.exe redis.windows.conf --maxmemory 200M . 此语句启动redis服务
   2. 再打开一个命令窗口, 输入`redis-cli.exe`, 
2. Redis的4条关键使用规范
   1. redis的键命名特点:, 
      1. key名称中不能有特殊字符，如空格、换行、单双引号、逗号、转义字符
      2. 用冒号":"分隔域，用"."作为单词间的连接，比如业务名:表名:id, ugc:video:1 
      3. key名称中加单引号或双引号也不出错, 但不合理.
   2. Redis Value规范
      1. 拒绝bigkey(防止网卡流量、慢查询)，string类型控制在10KB以内，hash、list、set、zset元素个数不要超过5000
   3. Redis 存储规范
      1. Redis是缓存而非存储，所有Redis存储的内容，用其他方式做[持久化存储](https://www.zhihu.com/search?q=持久化存储&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A161677697})（丢失后可以从持久化存储中恢复）
   4. Redis 连接规范
      1. 非特殊情况，连接Redis必须通过[中间件](https://www.zhihu.com/search?q=中间件&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A161677697})方式
   5. hash 的键名, 字段名 不加单双引号, 加也不出错, 但不建议加.
3. 大小写: 命令 大小都可, key,value区分大小
   1. 

#### 网站:

https://try.redis.io/   Redis在线练习

## 13 Redis

https://www.runoob.com/redis/redis-install.html

##### 备注:

1. 使用配置文件启动redis服务, 如 `redis-server 7000.conf`
2. Redis的Key值:大小写敏感，使用字符串做键值时注意大小写
3. 键有无 `引号`都可以, 
4. ???值引号 有无????
4. 没有 `;`结束符

#### nosql介绍

1. NoSQL

   1. not only sql , 新出现的数据库
   2. 特点: 不支持SQL
      2. nosql 存储的 `KV`形式 数据,  不同于 关系型数据库 关系表
      3. NoSQL 没有 通用语言，每种nosql数据库 有自己的api和语法, 擅长的业务场景
   3. NoSQL中的产品种类 多
      1. Mongodb
      2. Redis
      3. Hadoop( Hbase hadoop, Cassandra hadoop)
   
2. NoSQL, SQL数据库 比较

   1. 场景不同：
      1. sql数据库适合 关系数据查询场景, 
      2. nosql反之
   2. “事务”特性
      1. sql 支持 事务
      2. nosql 基本不支持事务
   3. 两者 取长补短，呈 融合趋势

3. Redis简介

   1. 简介

      1. 开源的,  ANSI C编写, 支持网络, 基于 内存 或 持久化的日志型、Key-Value数据库
      2. 提供多种语言的API
      3. NoSQL 一员，有多种 键值类型 存储. 
      4.  可作为 如缓存、队列. (借助高层级接口,)

   2. 特点

      1. 支持持久化
         1. 内存的数据可保存到 磁盘，重启时可 再次加载 使用。
      2. 支持 数据结构:  key-value类型, list，set，zset，hash
      3. 支持数据的备份 : `master-slave` 模式的数据备份
      
   3. 优势
   
      1. 性能 高 
         1. Redis能读 速度 110000次/s,写 速度 81000次/s 。
      2. 数据类型丰富 
         1. 支持二进制编码的数据类型 5种:  Strings, Lists, Hashes, Sets,  Ordered Sets(有序集合)
         1. 多种类型的数据结构，如 [字符串（strings）](http://redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://redis.cn/commands/geoadd.html) 索引半径查询
      3. 原子性
         1. Redis的所有操作 原子性的, 还支持几个操作 合并 后的原子性执行。
      4. 丰富的特性
         1. Redis 支持 publish/subscribe, 通知, key过期

   4. redis应用场景

      1. 做缓存(ehcache/memcached)

         1. redis的 数据 放在内存中 （内存数据库）

      2. 替代传统数据库—— 特定场景 如社交类的应用

      3. 系统中实现特定的功能：session共享、购物车

         

#### 1 安装

1. ubuntu虚拟机 已 安装redis
2. 下载/安装
   1. 下载: `wget http://download.redis.io/releases/redis-3.2.8.tar.gz`
      1. wget命令, 后面是url连接
   2. 解压: `tar -zxvf redis-3.2.8.tar.gz`
   3. 复制 到usr/local/redis/录下: `sudo mv ./redis-3.2.8 /usr/local/redis/`
   4. 进redis目录 usr/local/redis/ : `cd /usr/local/redis`
   5. 生成: `sudo make`
   6. 测试: `sudo make test`
   7. 安装, redis的命令 自动安装到 /usr/local/bin/ 
      1. `sudo make install`
   8. 安装完 查看, 进入目录 /usr/local/bin 
      1. `cd /usr/local/bin`
      2. `ls -all`
      3. 命令:
         1. redis-server : redis服务器
         2. redis-cli  :  redis命令行客户端
         3. redis-benchmark  :  redis性能测试工具
         4. redis-check-aof  : AOF文件修复工具
         5. redis-check-rdb  :  RDB文件检索工具
   9. 配置文件`redis.conf` 移动到 `/etc/redis/`下
      1. `sudo cp /usr/local/redis/redis.conf /etc/redis/`

#### 2 配置

1. Redis配置信息 `/etc/redis/redis.conf`
   1. 查看  `sudo vi /etc/redis/redis.conf`
2. 核心配置选项
   1. 绑定ip ,  `bind 127.0.0.1`
      1. 如 要远程访问，可将此 注释，或绑定真实ip
   2. 端口 , 默认 6379,  `port 6379`
   3. 守护进程,  `daemonize`精灵进程
      1. 系统服务进程（守护进程）不受用户登录和注销的影响，一直运行。这种进程叫做**守护进程**
      2. 运行在后台的一种特殊进程。独立于控制终端, 周期性的执行某种任务或等待处理某些发生的事件.  Linux的大多数服务器就是用守护进程实现的。比如ftp服务器，ssh服务器，Web服务器等
      3. daemonize yes 守护进程，daemonize no 不守护进程
      4. 推荐设置为yes
   4. 本地数据库文件名  `dbfilename dump.rdb`
   5. 本地数据库存放目录  `dir /var/lib/redis`
   6. 日志记录目录   ` logfile /var/log/redis/redis-server.log`
   7. 数据库的数量，默认有16个,  `database 16`
   8. 主从复制，类似于双机备份,   `slaveof`
      1. 本机A为 slave 服务时，设置 master 服务的 IP 地址及端口，在 Redis 启动时，它会自动从 master 进行数据同步

#### 菜鸟redis 配置

1. `https://www.runoob.com/redis/redis-conf.html`  

   1. 通过 **CONFIG** 命令查看或设置配置项

   2. **Redis CONFIG 命令格式**

      redis 127.0.0.1:6379> `CONFIG GET CONFIG_SETTING_NAME`

   3. 实例: redis 127.0.0.1:6379> `CONFIG GET loglevel`

   4. 使用 ***** 号获取所有配置项 :  redis 127.0.0.1:6379> `CONFIG GET *`

2. 修改 redis.conf 文件或使用 **CONFIG set** 命令来修改配置

   1. CONFIG SET 命令基本语法 : redis 127.0.0.1:6379> `CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE`
   2. 实例:  redis 127.0.0.1:6379> `CONFIG SET loglevel "notice"`

   redis 127.0.0.1:6379> `CONFIG GET loglevel`

   

#### 3 服务端和客户端命令

1. 启动redis服务器端方法
   1. window中启动: 打开一个终端并输入命令`redis-server`，该命令启动本地的 redis 服务
   
      1. 如输入  `redis-server --help`, 查看帮助文档
   
      1. 推荐 服务的 方式管理redis服务
   
   2. linux中以服务的方式启动停止服务器端
   
      1. 启动   `sudo service redis start`
      2. 停止  `sudo service redis stop`
      3. 重启  ` sudo service redis restart`
   
   3. 个人习惯
      1. ps -ef|grep redis   #查看redis服务器进程
      2. sudo kill -9 pid   #杀死redis服务器
      3. sudo redis-server /etc/redis/redis.conf  #目录指定加载的配置文件
   
2. 启动redis客户端步骤: 
   1. 先启动 redis 服务器端
      
   1. 然后再打开一个终端并输入命令 `redis-cli`，该命令会连接本地的 redis 服务
      
      1. 如输入  `redis-cli --help`, 查看帮助文档
      
   3. 在远程 redis 服务上执行命令`redis-cli -h host -p port -a password`
   
      1. 实例演示: 连接到主机为 127.0.0.1，端口为 6379 ，密码为 mypass 的 redis 服务上
   
         `$redis-cli -h 127.0.0.1 -p 6379 -a "密码mypass"`
   
   4. 运行测试命令  `ping`
   
   5. 切换数据库  `select n`,  n取 0-15中一个数
      1. 数据库没有名称，默认 16个，用`0-15`标识，redis默认选择 0标识数据库

#### 4 数据操作

1. 数据结构

   1. key-value对，每条一个`键值对`

      1. 键类型 `字符串`
      2. 键不能重复
      
   2. NoSQL  KV对存储形式  
   
      数据结构服务器
   
      ```
      ┌─────────────────────────────────────────────────┐
      |                   ┌────────────────────────────┐│
      │ byte[]            │   value                    ││
      |┌──────────┐       │                            ││
      ││ key      │<----> │   String                   ││
      │└──────────┘       └────────────────────────────┘|    
      |                                                 │
      |                   ┌────────────────────────────┐│
      │                   │value                       ││
      |┌──────────┐       │List{"xiaozhang","xiaowang"}││
      ││ key      │<----> │                            ││
      │└──────────┘       └────────────────────────────┘|
      |                                                 │
      |                   ┌────────────────────────────┐│
      │                   │value                       ││
      |┌──────────┐       │     ┌──────────┐           ││
      ││ key      │<----> │hash │ key|value|           ││
      │└──────────┘       |     │──────────│           ││
      |                   │     │    │     │           ││
      │                   │     └──────────┘           ││
      |                   └────────────────────────────┘│           
      |                                                 │
      |                   ┌────────────────────────────┐│
      │                   │value                       ││
      |┌──────────┐       │set{"xiaozhang","xiaowang", ││
      ││ key      │<----> │   "xiaoli"}                ││
      │└──────────┘       └────────────────────────────┘|
      └─────────────────────────────────────────────────┘
      ```
   
   2. 值 类型 五种
      1. 字符串string
      2. 哈希hash
      3. 列表list
      4. 集合set
      5. 有序集合zset
   
2. 数据操作: 4种

   1. 增加(增)
   2. 删除(删)
   3. 修改(改)
   4. 查找(查)

3. http://redis.cn/commands.html  官方文档

##### 4.1 string值

* string 的值
  * 字符串类型在Redis中是 二进制编码, 安全的. 接受 任何格式 的数据，如JPEG图像 或Json对象 
  * Redis中 字符串类型 的Value 最大长度是512M

1. set 增加 : 1. 键 不存在则 添加， 2. 键已 存在则 修改

   1. 设置`键-值`  语法: `set key value`

      1. `set name itcast` , 设置 键为name值为itcast   
2. 设置键值及过期时间, `秒` 单位. 格式:  setex key seconds value 
   
   1. `setex ka 3 va`, 设置键为ka 过期时间为3秒 值为va的数据
3. 设置多个键值对,  mset key1 value1 key2 value2 ...
   
   1. `mset a1 python a2 java a3 c`, 设置 键为'a1'值为'python'、键为'a2'值为'java'、键为'a3'值为'c'
      2. 中间没有分隔符

   4. 追加值,  append key value

      1. `append  a1  'haha'`, 向键为a1中追加值' haha'
2. get 获取

   1. 根据键获取值， 键不存在 返回nil,   get key

      1. `get name`, 获取键name的值

   2. 根据多个键获取多个值,  mget key1 key2 ...

      1. `mget a1 a2 a3`, 获取键a1、a2、a3'的值, 不存在返回`<nil>`


##### 4.2 keys命令

1. `keys`命令, 参数 是 正则表达式, 语法:   keys pattern

   1. 查看所有键,  `keys *`
   2. 查看名称中 含a的键,   `keys  a* `   #带引号

2. 判断键 存在， 存在返回1，不存在返回0,  格式:  exists key1

   1. `exists a1`, 判断键a1 存在

3. 查看键的value的类型,   type key

   1. `type a1`, 查看键a1的值类型，redis 的五种类型中的一种

4. 删除键及值,  del key1 key2.  删除键时 将删除值

   1. `del a2 a3`, 删除键a2、a3

5. 设置过期时间,  秒 单位. 没指定过期时间则一直存在，直到DEL移除, 格式 :  expire key seconds

   1. `expire a1 3` , 设置键'a1'的过期时间为3秒

6. 查看剩余有效时间，秒单位,  ttl key

   1. `ttl  bb`    , 查看键'bb'的有效时间,

      

##### 4.3 hash 值

* hash类型 
  * 数据结构仍是 键值对
  * 值是对象，结构为`字段-值` ,即一个键值对, 该键值对的值的类型为string

1. hset 增加 

   1. 增加单个字段值,  hset key field value

      1. `hset user name itheima`, 设置键 user的字段name 值itheima
         1. 问题: Redis快照不能持久化
            1. Redis 的配置: 保存数据库快照, 不能持久化到硬盘。
            2. 修改集合数据的 命令 不能用
         3. 解决方案
            1. 运行`config set stop-writes-on-bgsave-error no`
            2. 关闭配置项stop-writes-on-bgsave-error解决之
   2. 增加多个hash,  hmset key field1 value1 field2 value2 ...
   
      1. `hmset u2 name itcast age 11`, 设置键u2的字段name为itcast、字段age为11
2. hkeys 查找

   1. 一次获取键所有的字段,   hkeys key

      1. `hkeys u2`, 获取键u2的所有 字段
2. 一次获取所有字段的值,  hvals key
      1. `hvals u2`, 获取键 u2 的所有字段的值
2. 获取指定key field的值,  hget key field
   1. `hget u2  name ` , 获取键u2 字段 name 的值
4. 获取多个field的值,  hmget key field1 field2 ...
   
   1. `hmget u2 name age`获取键u2 字段 name 、 age的值
3. del 删除

   1. del命令删除整个hash`键及值`， del key, 如: `del u2`

   2. 删除字段， 对应的值 一起删除.  hdel key field1 field2 ...

      1. `hdel u2 age`, 删除键'u2'的字段'age'

##### 4.4 list

* list类型
  * 结构: 仍是键值对, 值是列表, 列表的元素类型为 string, 按插入顺序 排序
  * 索引定位列表的元素, 第一个元素索引是0, 负数索引，表示从尾部计数，如-1表示最后一个元素

1. lpush增加. l指list:

   1. 左侧插入数据,  lpush key value1 value2 ...

      1. `lpush a1 a b c` , 键为'a1'的列表左侧加入数据a 、 b 、c

         `lrange a1 0 3`  #查找索引[0-3)对应的值,  不包括3

   2. 右侧插入数据,  rpush key value1 value2 ...

      1. `rpush a1 0 1`键为'a1'的列表右侧加入数据0 1

   3. 指定元素的前/后插入新元素,  `linsert key before或after 现有元素 新元素`,  l指list

      1. `linsert a1 before b 3`, 键为 a1 的列表中元素 b 前加入 3

2. lrem 删除

   1. `lrem key count value`    #删除指定个数的指定元素

      1. 解析
         1. 将列表中前`count`次出现的 值value 的元素移除
         2. count > 0: 从头往尾移
         3. count < 0: 从尾往头移除
         4. count = 0: 移除所有

   2. `lrem a2 -2 b`  ,  键a2的列表从头到尾删除2个 b

      1. ```
         lpush a2 a b a b a b
         lrange a2 0 -1  //查找所有的元素
         lrem a2 -2 b  //键a2的列表从头到尾删除2个 b
         ```

3. lset 修改指定索引位置的元素值, 语法: `lset key index value`

   1. `lset a1 1 z`,  键为`a1`的列表 下标1 的元素值改为 z , 或增加z

4. lrange 查找(显示)

   1. `lrange key start stop`返回列表 指定范围内的元素

      1. 解析
         1. start、stop 元素 下标索引, 不包括stop
         2. 第一个元素索引为0
         3. 负数索引, 从尾部开始计数，-1 最后一个元素下标

   2. `lrange a1 0 -1`, 获取键 a1 的列表的所有元素

       `lrange a1 -5 -3`

##### 4.5 set

* set 类型
  * 无序集合, 元素为 `string`类型, 元素有唯一性，不重复
  * 说明：集合没有 修改

1. sadd 增加: sadd key member1 member2 ...   #添加元素

   1.  `sadd a3 zhangsan sili wangwu`, 向键 a3 的集合中添加元'zhangsan'、'lisi'、'wangwu'
2. smembers 获取: smembers key  #返回所有的元素

   1.  `smembers a3`, 获取键 a3 的集合中所有元素
3. srem 删除: srem key  #删除指定元素

   1. `srem a3 wangwu`, 删除键 a3 的集合的元素'wangwu'

##### 4.6 zset

* zset类型
  * sorted set，有序集合, 元素为`string`类型, 元素有唯一性，不重复
  * 每个元素 关联 double类型的score权重，根据权重将元素从小到大排序
  * 说明：没有修改操作

1. zadd 增加: zadd key score1 member1 score2 member2 ...  #添加

   1. `zadd a4 4 lisi 5 wangwu 6 zhaoliu 3 zhangsan`, 向键 a4 的集合中添加元素'lisi', 'wangwu', 'zhaoliu', 'zhangsan', 权重分别为4、5、6、3

2. zrange 查找: zrange key start stop   #返回指定范围内的元素

   1. 解析

      1. start、stop 元素的下标索引. 第一个元素索引为0, 负数索引从尾部计数，如-1最后一个元素下标
   2. `zrange a4 0 -1`, 查找键 a4 的集合中所有元素
   
   2. zrangebyscore key min max  #返回 权重score 在min和max之间的元素
   
      1. `zrangebyscore a4 5 6`, 获取键 a4 的集合中score权限值在5和6之间的元素
   
   3. zscore key member   #返回 元素member的score值, 也是string格式
   
      1. `zscore a4 zhangsan`, 获取键 a4 的集合中元素'zhangsan'的权重
3. zrem 删除: zrem key member1 member2 ...   #删除指定元素

   1. `zrem a4 zhangsan`, 删除键 a4 的集合 中元素'zhangsan'

   2. zremrangebyscore key min max  #删除权重在指定范围的元素

      1. `zremrangebyscore a4 5 6`, 删除键 a4 的集合中权限在5、6之间的元素

         

#### 5 与python交互

1. 安装redis

   1. 虚拟环境中redis安装, 3种方式   https://github.com/andymccurdy/redis-py

      1. 第一种：进入虚拟环境 py_django ，联接 安装包redis

         `pip install redis`

      2. 第二种：进入虚拟环境py_django，联接 安装包redis

         `easy_install redis`

      3. 第三种：到中文官网--客户端下载redis包的源码，用 源码安装.  一步步执行

         1. `wget https://github.com/andymccurdy/redis-py/archive/master.zip`   #wget命令, 后面是url
         2. unzip master.zip
            cd redis-py-master
            sudo python setup.py install

2. 引入模块: `from redis from *`

   1. 模块提供了`StrictRedis`类(Strict严格)，用于连接redis服务器，并按照不同类型提供 了不同方法，进行交互操作

##### 5.1 方法

1. 创建StrictRedis对象

   1. 创建对象: `sr = StrictRedis(host='localhost', port=6379, db=0)`

      1. 参数 解析
         1. host 指定的服务器, host默认 localhost
         2. port 指定的服务器 端口, port默认 6379
         3. db 即数据库默认为0
      2. 简写  `sr=StrictRedis()`
      
   2. 根据不同的类型，调用不同的实例方法

      1.  方法与 redis命令对应
      2. 方法的参数与命令的参数一致
   
2. string 类型方法

   1. set
   2. setex
   3. mset
   4. append
   5. get
   6. mget
   7. key

3. keys  类型方法

   1. exists
   2. type
   3. delete
   4. expire
   5. getrange
   6. ttl

4. hash  类型方法

   1. hset
   2. hmset
   3. hkeys
   4. hget
   5. hmget
   6. hvals
   7. hdel

5. list  类型方法

   1. lpush
   2. rpush
   3. linsert
   4. lrange
   5. lset
   6. lrem

6. set  类型方法

   1. sadd
   2. smembers
   3. srem

7. zset  类型方法

   1. zadd
   2. zrange
   3. zrangebyscore
   4. zscore
   5. zrem
   6. zremrangebyscore

##### 5.2 string

1. 准备

   1. 创建 `redis` 目录

   2. pycharm打开 redis目录

   3. 创建 `redis_string.py` 文件

      ```
      from redis import *
      if __name__=="__main__":
          try:
              #创建StrictRedis对象sr，sr与redis服务器连接
              sr=StrictRedis()
      
          except Exception as e:
              print(e)
      ```

      

2. string-增加

   1. 方法`set('key', 'value')`, 增加 键-值对, 返回 True/ False, 

      1. 参数: 键和值都是string类型, 加单/双引号

   2. 编写代码

      ```
      from redis import *
      if __name__=="__main__":
          try:
              #创建StrictRedis对象，与redis服务器建立连接
              sr=StrictRedis()
              #添加键name，值为itheima, 
              result=sr.set('name','itheima')
              
              #结果, 成功返回True，否则回False
              print(result)
          except Exception as e:
              print(e)
      ```

      

3. string-查找

   1. 方法`get('key')`，获取键对应的值, 键存在 返回对应的值, 键不存在 返回None

   2. 编写代码

      ```
      from redis import *
      if __name__=="__main__":
          try:
              #创建StrictRedis对象，与redis服务器建 连接
              sr=StrictRedis()
              #获取键name的值
              result = sr.get('name')
              
              #值，键不存在返回None, 值以二进制编码的形式存在
              print(result)
          except Exception as e:
              print(e)
      ```

      

4. string-修改

   1. 方法`set('key', 'value')`， 键存在则 修改， 不存在则 添加

   2. 编写代码

      ```
      from redis import *
      if __name__=="__main__":
          try:
              #创建StrictRedis对象，与redis服务器建⽴连接
              sr=StrictRedis()
              #设置键name的值，如果键已经存在则修改，如果键不存在则添加
              result = sr.set('name','itcast')
              
              #结果，如果操作成功则返回True，否则返回False
              print(result)
          except Exception as e:
              print(e)
      ```

      

5. string-删除

   1. 方法`delete('key')`，删除键-值, 返回受影响的键数 或0

   2. 编写代码

      ```
      from redis import *
      if __name__=="__main__":
          try:
              #创建StrictRedis对象sr，与redis服务器建 连接
              sr=StrictRedis()
              #删除键'name'及对应值，返回受影响的个数
              result = sr.delete('name')
              
              #结果，删除成功返回受影响的键数，否则返回0
              print(result)
          except Exception as e:
              print(e)
      ```

      

6. 获取键

   1. 方法`keys`, 根据 `正则表达式` 获取键

   2. 编写代码

      ```
      from redis import *
      if __name__=="__main__":
          try:
              #创建StrictRedis对象，与redis服务器建 连接
              sr=StrictRedis()
              #获取所有的键
              result=sr.keys()
              #结果，所有键构成一个列表，如 没有键则返回空列表
              print(result)
          except Exception as e:
              print(e)
      ```

##### 5.3 django

1. django存储session

   1. django的session
      1. 默认存储在 数据库
      2. 也可存储在 redis

2. 准备工作

   1. 创建 test5 项目和 booktest 应用
   2. 配置 url

3. session用redis存储配置

   1. 安装包

      `pip install django-redis-sessions==0.5.6`

   2. 修改settings文件，增加项

      ```
      SESSION_ENGINE = 'redis_sessions.session' #引擎
      SESSION_REDIS_HOST = 'localhost'  #Redis服务器主机IP
      SESSION_REDIS_PORT = 6379  #服务器port
      SESSION_REDIS_DB = 2  #数据库个数
      SESSION_REDIS_PASSWORD = ''  #redis密码
      SESSION_REDIS_PREFIX = 'session'  #字符串session
      ```

      

4. 测试

   1. 打开booktest/views.py, 创建`session_set`和`session_get`视图

      ```
        def session_set(request):
            request.session['name']='itheima'
            return HttpResponse('ok')
      
      
        def session_get(request):
            name=request.session['name']
            return HttpResponse(name)
      ```

      1. 视图 就是个 函数封装键值对

   2. 打开booktest/urls.py, 配置url

      ```
        url(r'^session_set/$',views.session_set),
        url(r'^session_get/$', views.session_get),
      ```

   3. 用`redis-cli` 客户端查看

      ```
      select 2
      keys *
      ```

      输出结果:

      ```
      int7yli en56ewxus 5xnrohsxbw3tu9rdOGMWOTQ3ZTFmN jAzMjNhZGY1NzFtZjA3YzhkNjUwYzQ1NTljYTY3Nzp7 Im5hbWULOtJpdGh l aW1hIn0
      ```

      

   4. Base64在线解码http://base64.xpcha.com/

      1. 步骤3中的输出结果, 在线解码得

         `8c0947e1 f60323adf571bf07c8d650c4559ca677 " name": "itheima"}`

      2. 结果与程序中设定一致

      

#### 6 搭配主从

 1. 主从概念

     1. 多级服务器集群架构

         	1. 一个master有多个slave，一个slave也有多个slave
         	2. 如此下去，形成 多级服务器集群架构

     2. 各节点的作用: master 写数据，slave 读数据. 网站 读写比 `10:1`

        ```
        					┌─────────┐
                            │  Master │   写
                            └─────────┘
                                 │
                      ┌────────────────────┐    
             	┌─────────┐           ┌─────────┐
                │  Slave  │           │  Slave  │  读
                └─────────┘           └─────────┘
                     │                     │
                ┌──────────┐          ┌──────────┐ 
        ┌──────────┐  ┌────────┐ ┌────────┐ ┌────────┐ 
        │  Slave   │  │  Slave │ │  Slave │ │  Slave │
        └──────────┘  └────────┘ └────────┘ └────────┘
        读:写  10:1
        ```

     3. 服务器主从配置实现 `读写分离`

         1. master和slave都是redis实例(redis服务) . master是 一个redis实例, slave 也是一个redis实例

 2. 主从配置(服务器)案例:

     1. 配置主服务器(主机A) 

         1. 查看当前主机的ip地址: `ifconfig`  #命令行中

            2. 修改 etc/redis/redis.conf 文件

                  `sudo vi redis.conf`

                  `bind 192.168.26.128`

         3. 重启redis服务:    `sudo service redis stop`  #关闭,    `redis-server redis.conf`  #重启

     2. 配置从服务器(主机B)

         1. 复制 etc/redis/redis.conf 文件 : 	`sudo cp redis.conf ./slave.conf`

         2. 编辑redis/slave.conf文件:    `sudo vi slave.conf`

               `bind 192.168.26.128`

               `slaveof 192.168.26.128 6379`  #从服务器IP port

               `port 6378`

         3. 启动redis服务:    `sudo redis-server slave.conf`,  指定配置文件

         4. 查看主从关系:    `redis-cli -h 192.168.26.128 info Replication`

 3. 数据操作

     1. master(主机A)和slave(主机B)分别执行 `info` 命令,查看输出信息 ,
     2. 启动主服务器(主机A)的客户端: `redis-cli -h 192.168.26.128 -p 6379`
     3. 启动从服务器(主机B)的客户端 `redis-cli -h 192.168.26.128 -p 6378`

     2. 在master(主机A)终端写数据(主客户端)  `set aa aa`
     3. 在slave(主机B)终端读数据 (从的客户端)  `get aa`

#### 7 搭建集群

1. 为什么要有集群

   1. 问题: 一主多从, 主服务挂掉，数据服务就挂掉
   2. 大公司都会有很多的服务器

2. 集群的概念

   1. 集群

      1. 相互独立的用高速网络 联的计算机 构成组，以`单一系统模式`管理。
      2. 客户与集群相互作用时，集群像 一个独立的服务器。
      3. 集群配置 提高 可用性和可缩放性

   2. 流程: 请求--发给-->负载均衡服务器- -转发--> 一台服务器

      ```
                           b servers with identical,
                              static content
                                        ┌──────────┐
                                     ┌──│ 服务器1   │
                                     │  └──────────┘
                                     │        
      ┌──────────┐    ┌───────────┐  │  ┌──────────┐
      │Internet  │────│负载均衡服务器│──│  │ 服务器... │
      └──────────┘    └───────────┘  │  └──────────┘
                   Network-request   │
                   load balancer     │  ┌──────────┐
                                     └──│ 服务器n   │
                                        └──────────┘
      ```

      

3. redis集群

   1. 分类:软件层面(逻辑), 硬件层面(物理)
   2. 软件层面(逻辑上): 集群像一台 电脑, 这一台电脑上启动了多个 redis服务
   3. 硬件层面(物理上): 实际有多台电脑, 每台电脑都启动了一个或多个redis服务
   
4. 搭建集群案例: 

   1. 两台主机  (主机A)172.16.179.130、(主机B)172.16.179.131

5. 参考阅读

   1. redis集群搭建http://www.cnblogs.com/wuxl360/p/5920330.html
   2. [Python]搭建redis集群http://blog.5ibc.net/p/51020.html

##### 7.1 配置主机A

1. 172.16.179.130 主机A: 进入`Desktop`目录，创建`conf`目录. 172.16.179.130是当前ubuntu机器的ip

2. 在conf目录下创建3个文件 `7000.conf`, `7001.conf`, `7002.conf`

   1. `7000.conf`，编辑内容

      ```
      port 7000
      bind 172.16.179.130
      daemonize yes
      pidfile 7000.pid
      cluster-enabled yes
      cluster-config-file 7000_node.conf
      cluster-node-timeout 15000
      appendonly yes
      ```

   2. `7001.conf`，编辑内容

      ```
      port 7001
      bind 172.16.179.130
      daemonize yes
      pidfile 7001.pid
      cluster-enabled yes
      cluster-config-file 7001_node.conf
      cluster-node-timeout 15000
      appendonly yes
      ```

   3. `7002.conf`，编辑内容

      ```
      port 7002
      bind 172.16.179.130
      daemonize yes
      pidfile 7002.pid
      cluster-enabled yes
      cluster-config-file 7002_node.conf
      cluster-node-timeout 15000
      appendonly yes
      ```

      1. 总结：三个文件的配置区别在port、pidfile、cluster-config-file三项

3. 使用配置文件启动主机A中的redis服务

   ```
   redis-server 7000.conf  #启动第一个redis服务
   redis-server 7001.conf  #启动第二个redis服务
   redis-server 7002.conf  #启动第三个redis服务
   ```

4. 查看进程 `ps -ef|grep redis`

##### 7.2 配置主机B

1. 172.16.179.131 主机B: 进入`Desktop`目录，创建`conf`目录. 172.16.179.131是当前ubuntu机器的ip

2. 在conf目录下创建3个文件 `7003.conf`, `7004.conf`, `7005.conf`

   1. `7003.conf`，编辑内容

      ```
      port 7003
      bind 172.16.179.131
      daemonize yes
      pidfile 7003.pid
      cluster-enabled yes
      cluster-config-file 7003_node.conf
      cluster-node-timeout 15000
      appendonly yes
      ```

   2. `7004.conf`，编辑内容

      ```
      port 7004
      bind 172.16.179.131
      daemonize yes
      pidfile 7004.pid
      cluster-enabled yes
      cluster-config-file 7004_node.conf
      cluster-node-timeout 15000
      appendonly yes
      ```

   3. `7005.conf`，编辑内容

      ```
      port 7005
      bind 172.16.179.131
      daemonize yes
      pidfile 7005.pid
      cluster-enabled yes
      cluster-config-file 7005_node.conf
      cluster-node-timeout 15000
      appendonly yes
      ```

      1. 总结：三个文件的配置区别在port、pidfile、cluster-config-file三项

3. 使用 配置文件 启动主机B中的redis服务

   ```
   redis-server 7003.conf  #启动第一个redis服务, 指定的配置文件
   redis-server 7004.conf  #启动第二个redis服务
   redis-server 7005.conf
   ```

4. 查看进程 `ps -ef|grep redis`

##### 7.3 创建集群

1. 创建集群 案例步骤: 

   1. `172.16.179.130`机器A:  复制命令(任何目录下调用此命令)

      `sudo cp /usr/share/doc/redis-tools/examples/redis-trib.rb /usr/local/bin/`  #redis-trib.rb复制到bin目录下, 

      1. redis 安装包中有`redis-trib.rb`，用于 创建集群

   2. 安装ruby环境.  redis-trib.rb是 `ruby`开发的

      `sudo apt-get install ruby`

      1. 在提示信息处输 y，然后回车 继续安装

   3. 运行命令 创建集群

      `redis-trib.rb create --replicas 1 172.16.179.130:7000 172.16.179.130:7001 172.16.179.130:7002 172.16.179.131:7003 172.16.179.131:7004 172.16.179.131:7005`

       1. 执行上面这个指令, 某些机器 可能会报错, 原因是 安装的 ruby 不是最 新版本

       1. 解决方法:

           1. 天朝的防火墙导致无法下载最新版本. 要设置 gem 的源

               ```
          #先查看ruby的 gem 源是什么地址
               gem source -l -- #如果是https://rubygems.org/ 就需要更换
               #更换指令为
               gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
               #通过 gem 安装 redis 的相关依赖
               sudo gem install redis
               ```
           
        2. 然后重新执行指令: `redis-trib.rb create --replicas 1 172.16.179.130:7000 172.16.179.130:7001 172.16.179.130:7002 172.16.179.131:7003 172.16.179.131:7004 172.16.179.131:7005`
           
   3. 提示如下主从信息，输入yes后回车
           
   4. 提示完成，集群搭建成功	

2. 数据验证

   1. 集群结构:  

      1. 主服务器: 7000、7001、7003, 对应的从服务器: 7004、7005、7002
      3. 主从服务器 是 ruby 自己分配的
      
   2. 在172.16.179.131主机B上连接7002，参数-c表示连接到集群
   
      `redis-cli -h 172.16.179.131 -c -p 7002`
   
      1. 写入数据 `set name itheima`
   
      2. 自动跳到 `7003`服务器，并写入数据成功
   
         `->Redirected to slot [5798] located at 192 .168. 26.130: 7003  OK`
   
      3. 在7003 获取数据，若写入数据又重定向到7000(负载均衡)
   
         `get name`
   
         `set age 20`
   
         `->Redirected to slot [741] located at 192 .168. 26.128: 7000 OK`
   
3. 在哪个服务器上写数据：CRC16算法

   1. redis cluster 去中心化，去中间件
      1. 集群中 的每个节点 平等的 对等的，每个节点都保存各自的数据和整个集 群的状态。
      2. 每个节点都和其他所有节点连接，这些连接保持活跃，保证了只需连接集群中的任意一个节点，就可获取到其他节点的数据
   2. Redis集群用 哈希槽 (hash slot)分配。
      1. redis cluster 默认分配了 16384 个slot，当 set一个key 时，用CRC16算法 取得slot，然后将 key 分到哈希槽区间的节点上
         1. 具体算法就是：CRC16(key) % 16384。
      2. 所以测试  看到 set 和 get 时，直接跳转到了7000端口的节点
   3. Redis 集群 把数据存于一个 master 节点，然后 master 和其对应的salve 数据同步.
      1. 当读取数据时, 根据`一致性哈希算法`到对应的 master 节 点获取数据
      2. 当一个master 挂掉,  会启动对应的 salve 节点，充 当 master
   4. 必须有3个或以上的主节点，否则在创建集群时会失败，并且当存 活的主节点数等于总节点数的一半时，整个集群就无法提供服务了

##### 7.4  集群与python交互

1. 安装包

   1. `pip install redis-py-cluster`

   2. redis-py-cluster源码地址: `https://github.com/Grokzen/redis-py-cluster`

2. 创建文件redis_cluster.py，示例码

   ```
   from rediscluster import *
   if __name__ == '__main__':
     try:
         # 构建所有的节点，Redis用CRC16算法，将键-值写到某个节点上
         startup_nodes = [
             {'host': '192.168.26.128', 'port': '7000'},
             {'host': '192.168.26.130', 'port': '7003'},
             {'host': '192.168.26.128', 'port': '7001'},
         ]
         # 构建StrictRedisCluster的对象
         src=StrictRedisCluster(startup_nodes=startup_nodes,decode_responses=True)
         # 设置键为name、值为itheima的数据
         result=src.set('name','itheima')
         print(result)
         # 获取键为name
         name = src.get('name')
         print(name)
     except Exception as e:
         print(e)
   ```

   

#### 8 总结与作业

---

#### Session

1. 什么是Session
   1. 当用户通过浏览器，访问某个网站时，服务器会在服务器的内存为该浏览器分配个空间，该空间被这个浏览器独占。这个空间就是session空间，该空间中的数据默认存在时间为30min,也可修改
      1. A : 服务器分配给A客户端的session空间
      2. B : 服务器分配给B客户端的session空间
      3. C : 服务器分配给C客户端的session空间
2. session用了做什么
   1. 网上商城中的购物车
   2. 保存登录用户的信息
   3. 将某些数据放入到Session中，供同一用户的各个页面使用
   4. 防止用户非法登录到某个页面
3. Session的样子
   1. session看作一张表. 这张表有两列, 有多少行，理论上没有限制，
   2. 每一行是session的一个属性。每个属性有两部分, 属性名字(String) 和它的值(Object)
4. 如何用Session
   1. 得到session:  `HttpSession hs=request.getSession(tue);`
   2. 向session添加属性:  `hs.s etAttribute(String name,Object val);`
   3. 从session得到某个属性:  `Sting namne =hs.getAttribute(String narne);`
   4. 从session删除某个属性:  `hs.remnoveAttribute(String naime);`

#### 网页间传数据

1. request、session、application 获取数据, 作用域request < session < application
2. page、request、session和application有什么区别
   1. page指当前页面。在一个jsp页面里有效
   2. request : 从http请求到服务器处理结束，返回响应的整个过程。这个过程用forward方式跳转多个jsp。这些页面里都可以用这个变量。 有点像HttpServletRequest，在所相关的网页都可以获取到请求网页的相关信息一样
   3. Session: 有效范围当前会话，从浏览器打开到浏览器关闭这个过程。 每个用户都有唯一的一个
   4. application: 有效范围是整个应用。 作用域里的变量，它们的存活时间是最长的，如果不进行手工删除，它们就一直可以使用 
   5. https://blog.csdn.net/sinat_19650093/article/details/50175303?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163854307516780357221194%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163854307516780357221194&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-50175303.first_rank_v2_pc_rank_v29&utm_term=request%2Csession%2Capplication%E7%9A%84%E5%8C%BA%E5%88%AB&spm=1018.2226.3001.4187
