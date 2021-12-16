#### 数据库都有客户端与服务器

1. 服务器:也称伺服器，提供计算服务的设备。服务器要响应服务请求，并进行处理，服务器应有承担服务并保障服务的能力
2. 客户端与服务端关系: 客户端向服务端存数据, 客户端从服务端取数据
3. MySQL有客户端服务端. 
   1. service mysql start 启动MySQL服务, 
   2. 用密码登录mysql账号：mysql -uroot -p 登录客户端  
4. MongoDB有客户端服务端. 
   1. mongodb服务端启动分别两种方式：a.本地测试方式的启动（只具有本地数据增删改查的功能）b.生产环境启动（具有完整的全部功能）
      1. 测试方式启动
         启动: sudo service mongodb start (sudo service mongod start)
         停止: sudo service mongodb stop
         重启: sudo service mongodb restart
      2.  生产环境正式的启动方式
         启动: `sudo mongod [--auth --dbpath=dbpath --logpath=logpath --append --fork] [-–f logfile ]`
   2. 启动mongodb的客户端：
      进入mongo shell
      启动本地客户端: mongo
      查看帮助：mongo –help
      退出：exit或者ctrl+c
   3. mongodb的简单使用: 开启mongodb server的情况下，再进入mongo shell后，就可以使用了
5. redis有客户端服务端:redis-server:启动redis的服务, redis-cli :启动redis的客户端

#### MongoDB简介

1. MongoDB 由C++语言编写，一个基于分布式文件存储的开源数据库系统。在高负载的情况下，添加更多的节点，可以保证服务器性能。MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。

2. MongoDB 的数据是一个文档，MongoDB 文档 类似于 JSON 对象。数据结构由键值(key=>value)对组成, 字段值可以是文档，数组及文档数组. 键不带引号, 字段值string类型带引号

   1. ````
      {
      	name:"sue",               //field:value
      	age:26,                   //field:value
      	status:"A",               //field:value
      	group:["news", "sports"]  //field:value
      }
      ````



#### 主要特点

1. MongoDB  一个面向文档存储的数据库
2. 可以设置任何属性的索引 (如：FirstName="Sameer",Address="8 Gandhi Road") 实现更快的排序。
3. 可通过本地或网络创建数据镜像，MongoDB有更强的扩展性。
4. 负载增加时（需要更多的存储空间和更强的处理能力） ，它可以分布在计算机网络中的其他节点上, 即所谓的分片。
5. Mongo查询指令用JSON形式的标记
6. update()命令可以实现替换完成的文档（数据）或者一些指定的数据字段 。
7. Mongodb的Map/reduce对数据进行批量处理和聚合操作。
8. Map函数用emit(key,value)遍历集合中所有的记录，将key与value传给Reduce函数处理。
9. Map函数和Reduce函数用Javascript编写的，通过db.runCommand或mapreduce命令执行MapReduce操作。
10. GridFS是MongoDB中的一个内置功能，用于存放大量小文件。
11. MongoDB允许在服务端执行脚本，用Javascript编写函数，直接在服务端执行. 把函数的定义存储在服务端，直接调用即可。
12. MongoDB支持语言:RUBY，PYTHON，JAVA，C++，PHP，C#等多种语言。
13. MongoDB安装简单。

#### MongoDB 工具

1. 监控
   1. MongoDB提供了网络和系统监控工具Munin， 作为一个插件应用于MongoDB中。
   2. Gangila是MongoDB高性能的系统监视的工具，作为一个插件应用于MongoDB中。
   3. 基于图形界面的开源工具 Cacti, 查看CPU负载, 网络带宽利用率, 提供了一个应用于监控 MongoDB 的插件
2. GUI
   1. Fang of Mongo – 网页式,由Django和jQuery所构成。
   2. Futon4Mongo – 一个CouchDB Futon web的mongodb山寨版。
   3. Mongo3 – Ruby写成。
   4. MongoHub – 适用于OSX的应用程序。
   5. Opricot – 一个基于浏览器的MongoDB控制台, 由PHP撰写而成。
   6. Database Master — Windows的mongodb管理工具
   7. RockMongo — 最好的PHP语言的MongoDB管理工具，轻量级, 支持多国语言

#### MongoDB 概念解析

1. mongodb中基本的概念是文档、集合、数据库

2. | SQL术语/概念 | MongoDB术语/概念 | 解释/说明                           |
   | :----------- | :--------------- | :---------------------------------- |
   | database     | database         | 数据库                              |
   | table        | collection       | 数据库表/集合                       |
   | row          | document         | 数据记录行/文档                     |
   | column       | field            | 数据字段/域                         |
   | index        | index            | 索引                                |
   | table joins  |                  | 表连接,MongoDB不支持                |
   | primary key  | primary key      | 主键,MongoDB自动将_id字段设置为主键 |

3. 数据库: 一个mongodb中可以建立多个数据库。MongoDB的默认数据库为"db"，该数据库存储在data目录中。MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中

   1. "show dbs"  #命令可以显示所有数据的列表
   2. **"db"** 命令可以显示当前数据库对象或集合
   3. "use local"命令，可以连接到一个指定的数据库local

4. 数据库名条件:不能是空字符串（""), 不得含有' '（空格)、.、$、/、\和\0 (空字符)。应全部小写。最多64字节。

5. 数据库服务和客户端

   1. | Mysqld/Oracle | mongod |
      | :-----------: | ------ |
      | mysql/sqlplus | mongo  |

   

#### 集合

1. 集合 是 MongoDB 文档组，类似于 RDBMS （关系数据库管理系统：Relational Database Management System)的表格. 

2. 集合存在于数据库中，集合没有固定的结构

   1. ```
      {"site":"www.baidu.com"}
      {"site":"www.google.com","name":"Google"}
      {"site":"www.runoob.com","name":"菜鸟教程","num":5}
      ```

   2. 第一个文档插入时，集合就会被创建

3. 合法的集合名

   1. 不能是空字符串""。
   2. 不能含有\0字符（空字符)，这个字符表示集合名的结尾。
   3. 不能以"system."开头，这是系统集合保留的前缀。
   4. 不能含有保留字符。千万不要在名字里出现$。

#### 元数据

1. 数据库的信息 存储在集合中。用了系统的命名空间. `dbname.system.*`
2. MongoDB数据库中名字空间 `<dbname>.system.*` 包含多种系统信息的特殊集合(Collection)

#### MongoDB 数据类型

1. | 数据类型           | 描述                                                         |
   | :----------------- | :----------------------------------------------------------- |
   | String             | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。 |
   | Integer            | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。 |
   | Boolean            | 布尔值。用于存储布尔值（真/假）。                            |
   | Double             | 双精度浮点值。用于存储浮点值。                               |
   | Min/Max keys       | 将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。 |
   | Array              | 用于将数组或列表或多个值存储为一个键。                       |
   | Timestamp          | 时间戳。记录文档修改或添加的具体时间。                       |
   | Object             | 用于内嵌文档。                                               |
   | Null               | 用于创建空值。                                               |
   | Symbol             | 符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。 |
   | Date               | 日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。 |
   | Object ID          | 对象 ID。用于创建文档的 ID。                                 |
   | Binary Data        | 二进制数据。用于存储二进制数据。                             |
   | Code               | 代码类型。用于在文档中存储 JavaScript 代码。                 |
   | Regular expression | 正则表达式类型。用于存储正则表达式。                         |

2. ObjectId : 类似唯一主键，可以很快的生成和排序，包含 12 bytes，含义是：前 4 个字节表示创建 **unix** 时间戳,格林尼治时间 **UTC** 时间，比北京时间晚了 8 个小时接下来的 3 个字节是机器标识码紧接的两个字节由进程 id 组成 PID最后三个字节是随机数

3. 字符串: BSON 字符串都是 UTF-8 编码

4. 时间戳: MongoDB 内部使用，与普通的 日期 类型不相关. 单个 mongod 实例中，时间戳值通常是唯一的

   1. 前32位是一个 time_t 值（与Unix新纪元相差的秒数）
   2. 后32位是在某秒中操作的一个递增的`序数`

#### 数据库操作

1. 创建语法 : use DATABASE_NAME
   1. `use runoob`
2. 删除语法:  db.dropDatabase()

#### 集合操作

1. 创建集合语法:  db.createCollection(name, options)

   1. 参数说明：name: 要创建的集合名称, options: 可选参数, 指定有关内存大小及索引的选项

   2. 在 test 数据库中创建 runoob 集合

      ```
      > use test
      switched to db test
      > db.createCollection("runoob")
      { "ok" : 1 }
      ```

   3. 查看已有集合: `show collections` 或` show  tables` 命令

2. 删除集合语法: db.collection.drop()

   1. ```
      >use mydb
      switched to db mydb
      >show collections
      mycol
      mycol2
      system.indexes
      runoob
      db.mycol2.drop()
      
      ```

#### 文档操作

1. MongoDB文档: 文档的数据结构和 JSON 基本一样。集合中的数据是 BSON 格式。BSON 是一种类似 JSON 的二进制形式的存储格式，是 Binary JSON 的简称

2. insert() 或 save() 向集合中插入文档, 语法: `db.COLLECTION_NAME.insert(document)` . 或`db.COLLECTION_NAME.save(document)` 该方法新版本中已废弃，可以使用 **db.collection.insertOne()** 或 **db.collection.replaceOne()** 来代替。

   1. 3.2 版本之后新增了 db.collection.insertOne()用于向集合插入一个新文档 和 db.collection.insertMany()向集合插入一个多个文档

      1. ```
         db.collection.insertOne(
            <document>,
            {
               writeConcern: <document>
            }
         )
         ```

      2. ```
         db.collection.insertMany(
            [ <document 1> , <document 2>, ... ],
            {
               writeConcern: <document>,
               ordered: <boolean>
            }
         )
         
         #参数说明：
         document：要写入的文档。
         writeConcern：写入策略，默认为 1，即要求确认写操作，0 是不要求。
         ordered：指定是否按顺序写入，默认 true，按顺序写入。
         ```

3. update() 更新文档, 语法格式

   1. ```
      db.collection.update(
         <query>,
         <update>,
         {
           upsert: <boolean>,
           multi: <boolean>,
           writeConcern: <document>
         }
      )
      参数说明：
      
      query : update的查询条件，类似sql update查询内where后面的。
      update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
      upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
      multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
      writeConcern :可选，抛出异常的级别
      ```

   2. ```
      >db.col.insert({
          title: 'MongoDB 教程', 
          description: 'MongoDB 是一个 Nosql 数据库',
          by: '菜鸟教程',
          url: 'http://www.runoob.com',
          tags: ['mongodb', 'database', 'NoSQL'],
          likes: 100
      })
      
      #update() 方法来更新标题(title)
      >db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
      WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })   # 输出信息
      > db.col.find().pretty()
      {
              "_id" : ObjectId("56064f89ade2f21f36b03136"),
              "title" : "MongoDB",
              "description" : "MongoDB 是一个 Nosql 数据库",
              "by" : "菜鸟教程",
              "url" : "http://www.runoob.com",
              "tags" : [
                      "mongodb",
                      "database",
                      "NoSQL"
              ],
              "likes" : 100
      }
      ```

4. remove() 删除文档, 语法格式

   1. ```
      MongoDB 是 2.6 版本以后的，语法格式
      
      db.collection.remove(
         <query>,
         {
           justOne: <boolean>,
           writeConcern: <document>
         }
      )
      
      参数说明：
      
      query :（可选）删除的文档的条件。
      justOne : （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有匹配条件的文档。
      writeConcern :（可选）抛出异常的级别
      ```

   2. ```
      #插入文档
      >db.col.insert({title: 'MongoDB 教程', 
          description: 'MongoDB 是一个 Nosql 数据库',
          by: '菜鸟教程',
          url: 'http://www.runoob.com',
          tags: ['mongodb', 'database', 'NoSQL'],
          likes: 100
      })
      
      #移除 title 为 'MongoDB 教程' 的文档
      >db.col.remove({'title':'MongoDB 教程'})
      WriteResult({ "nRemoved" : 2 })           # 删除了两条数据
      >db.col.find()
      ……                                        # 没有数据
      ```

5. find() 查询文档: 查询数据的语法格式

   1. ```
      db.collection.find(query, projection)
      参数:
      	query ：可选，使用查询操作符指定查询条件
      	projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
      
      #以易读的方式来读取数据，可以使用 pretty() 方法，语法格式
      db.col.find().pretty()  #以格式化的方式来显示所有文档
      ```

   2. ```
      db.col.find().pretty()
      {
              "_id" : ObjectId("56063f17ade2f21f36b03133"),
              "title" : "MongoDB 教程",
              "description" : "MongoDB 是一个 Nosql 数据库",
              "by" : "菜鸟教程",
              "url" : "http://www.runoob.com",
              "tags" : [
                      "mongodb",
                      "database",
                      "NoSQL"
              ],
              "likes" : 100
      }
      ```

   3. MongoDB 与 RDBMS Where 语句比较

      | 操作       | 格式                     | 范例                                        | RDBMS中的类似语句       |
      | :--------- | :----------------------- | :------------------------------------------ | :---------------------- |
      | 等于       | `{<key>:<value>`}        | `db.col.find({"by":"菜鸟教程"}).pretty()`   | `where by = '菜鸟教程'` |
      | 小于       | `{<key>:{$lt:<value>}}`  | `db.col.find({"likes":{$lt:50}}).pretty()`  | `where likes < 50`      |
      | 小于或等于 | `{<key>:{$lte:<value>}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50`     |
      | 大于       | `{<key>:{$gt:<value>}}`  | `db.col.find({"likes":{$gt:50}}).pretty()`  | `where likes > 50`      |
      | 大于或等于 | `{<key>:{$gte:<value>}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50`     |
      | 不等于     | `{<key>:{$ne:<value>}}`  | `db.col.find({"likes":{$ne:50}}).pretty()`  | `where likes != 50`     |

#### $type 操作符

1. MongoDB 中可以使用的类型

   1. | **类型**                | **数字** | **备注**         |
      | :---------------------- | :------- | :--------------- |
      | Double                  | 1        |                  |
      | String                  | 2        |                  |
      | Object                  | 3        |                  |
      | Array                   | 4        |                  |
      | Binary data             | 5        |                  |
      | Undefined               | 6        | 已废弃。         |
      | Object id               | 7        |                  |
      | Boolean                 | 8        |                  |
      | Date                    | 9        |                  |
      | Null                    | 10       |                  |
      | Regular Expression      | 11       |                  |
      | JavaScript              | 13       |                  |
      | Symbol                  | 14       |                  |
      | JavaScript (with scope) | 15       |                  |
      | 32-bit integer          | 16       |                  |
      | Timestamp               | 17       |                  |
      | 64-bit integer          | 18       |                  |
      | Min key                 | 255      | Query with `-1`. |
      | Max key                 | 127      |                  |

2. 数据库名称为"runoob" 我们的集合名称为"col"

   

   1. ```
      >db.col.insert({
          title: 'PHP 教程', 
          description: 'PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。',
          by: '菜鸟教程',
          url: 'http://www.runoob.com',
          tags: ['php'],
          likes: 200
      })
      ```

   2. ```
      >db.col.insert({title: 'Java 教程', 
          description: 'Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。',
          by: '菜鸟教程',
          url: 'http://www.runoob.com',
          tags: ['java'],
          likes: 150
      })
      ```

   3. ```
      >db.col.insert({title: 'MongoDB 教程', 
          description: 'MongoDB 是一个 Nosql 数据库',
          by: '菜鸟教程',
          url: 'http://www.runoob.com',
          tags: ['mongodb'],
          likes: 100
      })
      ```

   4. 获取 "col" 集合中 title 为 String 的数据

      ```
      db.col.find({"title" : {$type : 2}})
      或
      db.col.find({"title" : {$type : 'string'}})
      ```

#### mongdb简介

1. 优势: 易扩展,  大数据量， 高性能, 灵活的数据模型

2. mongodb安装

   1. 命令安装方式: `sudo apt-get install -y mongodb-org` 

   https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/ 

   2. 下载安装方式: 下载, tar解压, sudo mv移动到/usr/local/mongodb目录下, 可执行文件添加到path路径下 export PATH=/usr/local/mongodb/bin:$PATH

   

#### MongoDB操作

1. 服务端mongod启动
   1. 查看帮助：mongod –help
   2. 启动：sudo service mongod start 
   3. 停止：sudo service mongod stop 
   4. 重启：sudo service mongod restart 
   5. 查看是否启动成功：ps ajx|grep mongod 
   6. 配置文件的位置：/etc/mongod.conf，
   7. 默认端口：27017 
   8. 日志的位置：`/var/log/mongodb/mongod.log `
2. 客户端mongo
   1. 启动本地客户端:mongo 
   2. 查看帮助：mongo –help 
   3. 退出：exit或者ctrl+c

3. database的基础命令 
   1. 查看当前的数据库：db
   2. 查看所有的数据库：show dbs  /show databases 
   3. 切换数据库：use db_name 
   4. 删除当前的数据库：db.dropDatabase() 
4. 集合的基础命令
   1. 不手动创建集合： 向不存在的集合中第一次加数据时,  集合会被创建出来 
   2. 手动创建结合： 
      1. 语法: db.createCollection(name,options) 
      2. db.createCollection("stu") 
      3. db.createCollection("sub", { capped : true, size : 10 } ) 
         1. 参数capped： 默认值 false,不设置上限,值为true表示设置上限
         2. 参数size： 当capped值为true时, 需要指定此参数， 表示上限大小,当文档达到上限时， 会将之前的数据覆盖，  单位为字节 
      4. 查看集合：show collections
      5. 删除集合：db.集合名称.drop() 
5. 数据类型
   1. Object ID： 文档ID 
   2. String： 字符串， 最常用， 必须是有效的UTF-8 
   3. Boolean： 存储一个布尔值， true或false 
   4. Integer： 整数可以是32位或64位，  这取决于服务器 
   5. Double： 存储浮点值 
   6. Arrays： 数组或列表， 多个值存储到一个键 
   7. Object： 用于嵌入式的文档， 即一个值为一个文档 
   8. Null： 存储Null值 
   9. Timestamp： 时间戳， 表示从1970-1-1到现在的总秒数
   10. Date： 存储当前日期或时间的UNIX时间格式
   11. 注意点
       1. 创建日期语句: 参数的格式为YYYY-MM-DD , `new Date('2017-12-20')`
       2. 每个文档都有一个属性， 为`_id`， 保证每个文档的唯一性 
       3. 可以设置`_id`插入文档，如果没有提供, 那么MongoDB为每个文档提供了一个独特的`_id`, 类型为`objectID`
       4. `objectID`是一个12字节的十六进制数： 
          1. 前4个字节为当前时间戳 
          2. 接下来3个字节的机器ID 
          3. 接下来的2个字节中MongoDB的服务进程id 
          4. 最后3个字节是简单的增量值 

#### 插入保存查询更新删除

1. 插入:  MongoDB数据库的3个组成: 数据库, 数据库内集合, 集合内的文档, 文档是字典,字典的元素是键值对( 或文档是json对象,每个元素是键值对), json的键没有引号, 值如果是字符串有单引号.
   1. 语法: `db.集合名称.insert(document)`
   2. `db.stu.insert({name:'gj',gender:1}) `
   3. `db.stu.insert({_id:"20170101",name:'gj',gender:1})`
      1. 插入文档时, 如果不指定`_id`参数, MongoDB会为文档分配一个唯一的`ObjectId` 

2. 保存: `db.集合名称.save(document)`
   1. 如果文档的`_id`已经存在则修改，  如果文档的`_id`不存在则添加

3. 查询: `find() `

   1. find(),  `db.集合名称.find({条件文档})` 
   2. findOne(), 只返回第一个, `db.集合名称.findOne({条件文档})`
   3. pretty(), 将结果格式化, `db.集合名称.find({条件文档}).pretty()`

4. 更新: `db.集合名称.update(<query> ,<update>,{multi: <boolean>}) `

   1. 参数
      1. query:查询条件 
      2. update:更新操作符
      3. multi:可选， 默认是false，表示只更新找到的第一条记录，值为true表示把满足条件的文档全部更新 
   2. `db.stu.update({name:'hr'},{name:'mnc'})`    更新一条 
   3. `db.stu.update({name:'hr'},{$set:{name:'hys'}})`  #更新一条 
   4. `db.stu.update({},{$set:{gender:0}},{multi:true})`   #更新全部 
   5. 注意： "multi update only works with $ operators" 

5. 删除: `db.集合名称.remove(<query>,{justOne: <boolean>})`

   1. 参数
      1. query:可选，删除的文档的条件 
      2. justOne:可选, 如果设为true或1, 则只删除一条,  默
         认false， 表示删除多条 

   

#### 运算符

1. 比较运算符

   1. 等于： 默认是等于判断， 没有运算符 

   2. 小于：`$lt` （less than）

   3. 小于等于：`$lte` （less than  equal ）

   4. 大于：`$gt` （greater than）

   5. 大于等于：`$gte`

   6. 大不等于：`$ne` 

      `db.stu.find({age:{$gte:18}})`

2. 逻辑运算符 

   1. and：没有符号,  json中写多个条件即可.  查询年龄大于或等于18，  并且性别为true的学生 
      `db.stu.find({age:{$gte:18},gender:true})` 

   1.  or: `$or`, 值为数组, 数组中每个元素为json.  
      1. 查询年龄大于18,或性别为false的学生  `db.stu.find({$or:[{age:{$gt:18}},{gender:false}]}) `
      2. 查询年龄大于18或性别为男生,  并且姓名是郭靖 
         `db.stu.find({$or:[{age:{$gte:18}},{gender:true}],name:'gj'})`

3. 范围运算符: `$in, $nin` 判断是否在某个范围内. 

   1.  查询年龄为18,28的学生 `db.stu.find({age:{$in:[18,28]}})`

4. 正则表达式:  `//`或`$regex`编写正则表达式

   1. 查询姓黄的学生 : `db.stu.find({name:/^黄/}) `  或 `db.stu.find({name:{$regex:'^黄'}}) `

#### limit和skip 

1. limit(): 读取指定数量的文档 . 语法: `db.集合名称.find().limit(NUMBER)`
2. 查询2条学生信息:  `db.stu.find().limit(2)` 
3. skip():  跳过指定数量的文档. 语法: `db.集合名称.find().skip(NUMBER)`
4. db.stu.find().skip(2) 
5. 同时使用 : `db.stu.find().limit(4).skip(5)`或  
   `db.stu.find().skip(5).limit(4)` 

#### $where

$where查询后写一个函数， 返回满足条件的数据.

1. 查询年龄大于30的学生

   ```
   db.stu.find({ 
       $where:function() { 
           return this.age>30;} 
   })
   ```

#### 投影

查询结果选要的字段 

1. 语法: `db.集合名称.find({},{字段名称:1,...})`
   1. 参数: 字段与值， 值为1: 显示， 值为0: 不显示
   2. 特殊：`_id`列默认是显示的, 如不显示要明确设为0 
2. `db.stu.find({},{_id:0,name:1,gender:1})`

#### sort()

排序语法: `db.集合名称.find().sort({字段:1,...})`

1. 1. 1: 升序排列 , -1: 降序
2. 性别降序， 再年龄升序`db.stu.find().sort({gender:-1,age:1})`

#### count() 

统计结果集中文档条数

1. db.集合名称.find({条件}).count() 
2. db.集合名称.count({条件}) 
3. db.stu.find({gender:true}).count() 
4. db.stu.count({age:{$gt:20},gender:true}) 

#### distinct()

对数据去重语法: `db.集合名称.distinct('去重字段',{条件})`

1. db.stu.distinct('hometown',{age:{$gt:18}}) 

#### aggregate()

聚合(aggregate): 基于数据处理的聚合管道，每个文档通过一个由多个阶段（stage）组成的管道，对每个阶段分组、过滤等功能，然后经过一系列的处理，输出相应的结果。

1.  语法: `db.集合名称.aggregate({管道:{表达式:"$列名"}})`

   1. ```
      db.orders.aggregate([
      		{$match:{status:"A"}},
      		{$group:{_id:"$cust_id", total:{$sum:"$amount"}}}
      	])

2. 管道: `管道:{表达式:"$列名"}`,  mongodb中，文档处理完毕后，  通过管道进入下一次处理

   1. | 管道     | 含义                                                         |
      | -------- | ------------------------------------------------------------ |
      | $group   | 将集合中的文档分组，  用于统计结果                           |
      | $match   | 过滤数据， 只输出符合条件的文档                              |
      | $project | 修改输入文档的结构， 如重命名、 增加、 删除字段、 创建计算结果 |
      | $sort    | 将输入文档排序后输出                                         |
      | $limit   | 限制聚合管道返回的文档数                                     |
      | $skip    | 跳过指定数量的文档，  并返回余下的文档                       |
      | $unwind  | 将数组类型的字段拆分                                         |

4. 表达式语法：`表达式:"$列名"`, 处理输入文档并输出,

   1. | 表达式 | 含义                                   |
      | ------ | -------------------------------------- |
      | $sum       |计算总和， $sum:1 表示以一倍计数|
      | $avg   | 计算平均值                             |
      | $min   | 获取最小值                             |
      | $max   | 获取最大值                             |
      | $push  | 结果文档中插入值到一个数组中           |
      | $first | 根据资源文档的排序获取第一个文档数据   |
      | $last  | 根据资源文档的排序获取最后一个文档数据 |

#### $match

1. 将集合中的文档分组，用于统计结果

2. `_id`: 分组的依据，使用某个字段的格式为`'$字段'`

3. 例1:统计男生、女生的总人数

   ```
   db.stu.aggregate(
   	{$group:
   		{
   			_id: '$gender',
   			counter :{$sum:1}
   		}
   	}
   )

https://docs.mongodb.com/manual/reference/operator/aggregation/group/

#### Group by null 

1. 将集合中所有文档分为一组

2. 例2:求学生总人数、平均年龄

   ```
   db.stu.aggregate(
   	{$group:
   		{
   			_id:null,
   			counter:{$sum:1},
   			avgAge:{$avg:'$age'}
   		}
   	}
   )
   ```

#### $push

透视数据

1. 统计不同性别的学生姓名

   1. ```
      db.stu.aggregate(
      	{$group:
      		{
      			_id:"$gender',
      			name:{$push:'$name}
      		}
      	}
      )
      ```

2. $$ROOT将文档内容加入到结果集的数组中

   1. ```
      db.stu.aggregate(
      	{$group:
      		{
      			_id:'$gender',
      			name:{$push:'$$ROOT'}
      		}
      	}
      )
      ```

3. 动手

   1. { "country" : "china", "province" : "sh", "userid" : "a" }   
      {  "country" : "china", "province" : "sh", "userid" : "b" }   
      {  "country" : "china", "province" : "sh", "userid" : "a" }   
      {  "country" : "china", "province" : "sh", "userid" : "c" }   
      {  "country" : "china", "province" : "bj", "userid" : "da" }   
      {  "country" : "china", "province" : "bj", "userid" : "fa" }   
      需求：统计出每个country/province下的userid的数量（同一
      个userid只统计一次） 

#### $match

$match是管道命令，能将结果交给后一个管道,但是find不可以

1. 用于过滤数据，输出符合条件的文档, 使用MongoDB的标准 查询操作

2. 例1: 查询年龄大于20的学生

   ```
   db.stu.aggregate(
   	{$match: {age:{$gt:20}}
   )
   ```

3. 例2:查询年龄大于20的男生、女生人数

   ```
   db.stu.aggregate(
   	{$match: {age: {$gt:20}}},
   	{$group:{_id:'$gender', counter:{$sum:1}}}
   )
   ```

#### $project

1. 修改输入文档的结构，如重命名、增加、删除字段、创建计算结果

2. 例1:查询学生的姓名、年龄

   ```
   db.stu.aggregate(
   	{$project:{_id:0, name:1, age:1}}
   )
   ```

3. 例2:查询男生、女生人数，输出人数

   ```
   db.stu.aggregate(
   	{$group:{_id: '$gender', counter:{$sum:1}}},
   	{$project:{_id:0, counter:1}}
   )
   ```

4. 动手练习 
   { "country" : "china", "province" : "sh", "userid" : "a" }   
   {  "country" : "china", "province" : "sh", "userid" : "b" }   
   {  "country" : "china", "province" : "sh", "userid" : "a" }   
   {  "country" : "china", "province" : "sh", "userid" : "c" }   
   {  "country" : "china", "province" : "bj", "userid" : "da" }   
   {  "country" : "china", "province" : "bj", "userid" : "fa" }   
   `需求：统计出每个country/province下的userid的数量（同一个userid只
   统计一次），结果中的字段为{country:"**"，province:"**"，
   counter:"*"} `

#### $sort

1. 将输入文档排序后输出

2. 例1:查询学生信息，按年龄升序

   ```
   b.stu.aggregate({$sort:{age:1}})
   ```

3. 例2:查询男生、女生人数，按人数降序

   ```
   db.stu.aggregate(
      {$group:{_id: '$gender',counter:{$sum:1}}},
      {$sort:{counter:-1}}
   }
   ```

#### $limit

1. 限制聚合管道返回的文档数
2. 例1:查询2条学生信息
   `db.stu.aggregate({$limit:2})`

#### $skip

1. 跳过指定数量的文档，并返回余下的文档

2. 例2: 查询从第3条开始的学生信息
   `db.stu.aggregate({$skip:2})`

3. 例3:统计男生、女生人数，按人数升序，取第二条数据

   ```
   db.stu.aggregate(
   	{$group:{_id:'$gender', counter:{$sum:1}}},
   	{$sort:{counter:1}},
   	{$skip:1},
   	{$limit:1}
   )

4. 
   注意顺序:先写skip， 再写limit

#### $unwind

1. 将文档中的某一个数组类型字段拆分成多条,每条包含数组中的一个值

2. 语法: `db.集合名称.aggregate({$unwind:'$字段名称’})`

   ```
   db.t2.insert({_id:1,item:'t-shirt',size:['S','M','L'])
   db.t2.aggregate({$unwind:'$size'})
   ```

   结果如下:

   ```
   {"_id":1,"item": "t-shirt", "size": "S" }
   {"_id":1,"item": "t-shirt", "size": "M" }
   {"_id":1,"item": "t-shirt", "size": "L" }

3. 练习: 数据库中有一条数据：{"username":"Alex","tags": ['C#','Java','C++']}，如何获取该tag列表的长度？

4. 属性preserveNullAndEmptyArrays: 值为true表示保留属性值为空的文档 ,属性 值为false表示丢弃属性值为空的文档 

   用法： 

   ```
   db.inventory.aggregate({
   	$unwind:{
   		path:'$字段名称',
   		preserveNullAndEmptyArrays:<boolean> #防止数据丢失
   	}
   })
   
   ```

#### 索引

1. 索引：以提升查询速度 

2. 测试：插入10万条数据到数据库中 

   ```
   for(i=0;i<100000;i++){db.t12.insert({name:'test'+i,age:i})} 
   db.t1.find({name:'test10000'}) 
   db.t1.find({name:'test10000'}).explain('executionStats')
   ```

3. 建立索引之后对比： 
   语法：`db.集合.ensureIndex({属性:1})`，1表示升序，  -1表示降序 
   具体操作：`db.t1.ensureIndex({name:1})` 
   `db.t1.find({name:'test10000'}).explain('executionStats')`

   1.  ```
       ”executionStats":{
       	”executionSuccess": true,
       	”nReturned":1,
       	"executionTimeMillis":52，
       	“tota LKeysExamined":0,
       	"totalDocsExamined":100000,
       }
       ```

   2. ```
      ”executionStats":{
      	”executionSuccess": true,
      	”nReturned":1,
      	"executionTimeMillis":5，
      	“tota LKeysExamined":1,
      	"totalDocsExamined":1,
      }

4. 默认情况下创建的索引均不是唯一索引。 

   1. 创建唯一索引:  `db.t1.ensureIndex({"name":1},{"unique":true})`
   2. 创建唯一索引并消除重复：      
      `db.t1.ensureIndex({"name":1},"unique":true,"dropDups":true}) ` 
   3. 建立联合索引(什么时候需要联合索引): `db.t1.ensureIndex({name:1,age:1}) `

   4. 查看当前集合的所有索引： `db.t1.getIndexes() `
   5. 删除索引： `db.t1.dropIndex('索引名称')` 

#### 数据的备份

1. 备份的语法: `mongodump  -h dbhost -d dbname -o dbdirectory`

   -h： 服务器地址， 也可以指定端口号 
   -d： 需要备份的数据库名称 
   -o： 备份的数据存放位置，  此目录中存放着备份出来的数据 

2. `mongodump -h 192.168.196.128:27017 -d test1 -o ~/Desktop/test1bak` 

#### 数据的恢复

1. 恢复语法: `mongorestore -h dbhost -d dbname --dir dbdirectory` 
   	-h： 服务器地址 
   	-d： 需要恢复的数据库实例 
   	--dir： 备份数据所在位置 

   `mongorestore -h 192.168.196.128:27017 -d test2 --dir ~/Desktop/test1bak/test1`

2. 动手:  尝试将电脑中的douban.tv1中的数据恢复到自己的电脑中，具体如何
   操作？ 完成上述操作后完成以下问题： 
   1. 获取每条数据中的title，count(所有评分人数),rate(评分),country(国家)的这些字段 
   2. 获取上述结果中的不同国家电视剧的数据量
   3. 获取上述结果中分数大于8分的不同国家电视剧的数据量

#### 案例

```
from pymongo import MongoClient
class TestMonogo:
	#实例化和插入
    def __init__(self):
        client = MongoClient(host="127 .0.0.1" ,port=27017)
        self .collection = client["test'"]["t1"]  #用方括号的方式选择数据库和集合

    def test_insert(self):
        #insert接收字典，返回objectId
        ret = self.collection.insert({"name" :"test10010" , "age":33})
        print(ret)

    def test_insert_many(self):
        item_list = [{"name":"test100{}".format(i)} for i in range (10)]
        #insert_many接收一个列表，列表中为所有需要插入的字典
        t = self.collection.insert_many(item_list)
        #t.inserted_ids为所有插入的id
        for i in t.inserted_ids:
            print(i)


    #插入和更新
    def try_find_one(self):
        #find_ one查找并且返回-个结果,接收一个字典形式的条件
        t = self.collection.find_one({"name": "test10005"})
        print(t)

    def try_find_many(self):
        #find返回所有满足条件的结果，如果条件为空，则返回数据库的所有
        t = self.collection. find ({"name" :"test10005"})
        #结果是一个Cursor游标对象，是一个可迭代对象，可以类似读文件的指针,
        for i in t:
            print(i)
        for i in t: #此时t中没有内容
            print(i)

    def try_update_one(self):
        #update_one更 新一条数据
        self.collection.update_one({"name" :"test10005"},{"$set": {"name":"new_test10005"}})

    def try_update_many(self):
        #update_one更新全部数据
        self.collection.update.many ( {"name":"test10005"}, {"$set": {"name" :"new_ .test10005"}})

	#删除
    def try_delete_one(self):
        #delete_one删除一条数据
        self.collection.delete_one({"name":"test10010"})

    def try_delete_many(self):
        #delete_may删除所有满足条件的数据
        self.collection.delete_many({"name":"test10010"})

```

#### 动手

1. 动手一 
   1. 统计t1中所有的name的出现的次数 
   2. 统计t1中所有的name的出现的次数中次数大于4的name 
   3. 统计t1中所有的name的出现的次数中次数大于4的次数（只
   显示次数）
2. 动手二
  1. 使用python向集合t3中插入1000条文档，文档的属性包括`_id`, name
    1. `_id`的值为0、1、2、3..999
    2. name的值为'py0'、 'py1...
  2. 查询显示出_id为100的整倍数的文档，如100, 200,300... 并将name输出
