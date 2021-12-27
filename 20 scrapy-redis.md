1. python中连接Redis数据库的三种方式（以及添加密码后再连接）

   1. 连接数据库

      ```
      import redis
      
      db = redis.Redis(host=127.0.0.1,port=6379,decode_responses=False)
      db.set('foo', 'Bar')
      print(db.get('foo'))
      a = input('按任意键结束')
      ```

   2. 创建连接池

      ```
      import redis
      pool=redis.ConnectionPool(decode_response=True)
      db=redis.Redis(connection_pool=pool,host=127.0.0.1,port=6379)
      或者:
      
      pool = redis.ConnectionPool(host='207.148.120.229',port=6379)
      conn = redis.Redis(connection_pool=pool)
      conn.set('key', 'Hello World')
      print(conn.get('key'))
      a = input('按任意键结束')
      ```

      连接池则可以实现在客户端建立多个链接并且不释放，当需要使用连接的时候通过一定的算法获取已经建立的连接，使用完了以后则还给连接池，这就免去了数据库连接所占用的时间

2. redis_key = 'jingzhun:starturls' 这个从lpush 里面来的，但是我现在要给这个push的地址加上cookie 和header 才能获取数据，请问这个如何处理呢？

   1. 重写scrapy_redis的方法make_request_from_data()

      ```
      def make_request_from_data(self, data):
              """Returns a Request instance from data coming from Redis.
              By default, ``data`` is an encoded URL. You can override this method to
              provide your own message decoding.
              Parameters
              ----------
              data : bytes
                  Message from redis.
              """
              url = bytes_to_str(data, self.redis_encoding)
              return self.make_requests_from_url(url)
      ```

      单机爬虫有一个start_request方法,  scrapy_redis分布式爬虫对应的位置有make_requests_from_url方法, scrapy_redis没有start_request

3. 如何使用scrapy-redis组件来实现分布式爬虫?   

   1. 其一：基于该组件的RedisSpider类, 其二：基于该组件的RedisCrawlSpider类
   2. 两种分布式爬虫的实现流程是一致的

4. 分布式的实现流程

   1. 下载scrapy-redis组件: `pip install scrapy_redis`

   2. 创建工程

   3. 创建爬虫文件：RedisSpider RedisCrawlSpider  :  `scrapy genspider -t crawl xxx www.xxx.com`

   4. 对爬虫文件中的相关属性进行修改

      1. 导入：from scrapy_redis.spiders import RedisCrawlSpider
      2. 将爬虫类的父类修改为RedisSpider或RedisCrawlSpider。注意：原始爬虫文件基于Spider的，则父类修改成RedisSpider，原始爬虫文件是CrawlSpider，则父类修改成RedisCrawlSpider
      3. 注释或删除start_urls列表，加入redis_key属性。就是将起始的URL列表替换成redis_key = 'xxx' （调度器队列的名称）

   5. 配置文件中进行配置

      1. 用组件中封装好的可以被共享的管道类

         ```
         ITEM_PIPELINES = {
             'scrapy_redis.pipelines.RedisPipeline': 400
             }
         ```

      2. 配置调度器（使用组件中封装好的可共享的调度器）

         增加一个去重容器类的配置，作用使用Redis的set集合来存储请求的指纹数据，从而实现请求去重的持久化

         ```
         DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
         ```

         用scrapy_redis组件的调度器

         ```
         SCHEDULER = "scrapy_redis.scheduler.Scheduler"
         ```

         调度器是否要持久化, 即爬虫结束之后, 要不要清空Redis中请求队列和去重指纹的set。如果是True，则持久化存储，不清空数据，否则清空数据

         ```
         SCHEDULER_PERSIST = True
         ```

         指定存储数据中的redis

         ```
         REDIS_HOST = 'redis服务的ip地址'# 一般写127.0.0.1
         REDIS_PORT = 6379
         ```

         

   6. 在redis数据库的配置文件中修改一些数据

      1. 取消保护模式:  `protected-mode no`  #表示可以让其他ip操作redis

      2. bind绑定：将这一行注释掉 因为只有注释掉了 才能进行分布式  `#bind 127.0.0.1`  表示可以让其他ip访问redis

      3. 启动redis:

         开启redis服务端：在终端中输入 redis-server

         开启redis客户端： 在开启服务端之后 在终端输入 redis-cli

   7. 执行分布式程序

      1. 进入到项目下的spiders中 `cd spiders`   #cd 该项目.  在spiders目录下 查看你的爬虫文件存不存在  `dir`

      2. 执行此命令 :  `scrapy runspider xxx.py`   #xxx.py是你的爬虫文件名

         

   8. 向调度器队列中扔入一个url

      1. 在redis-cli执行之后，执行此命令 `lpush chouti https://dig.chouti.com/`,   # chouti是 爬虫文件中redis_key = 'chouti' #调度器队列的名称  后面是你要爬取的网站的地址
      2. 查看redis中是否有存储到数据  :  `key *`



## 20 scrapy-redis

### 6 scrapy-redis分布式组件

1. Scrapy: 通用 爬虫框架， 不支持分布式

2. Scrapy-redis : 实现Scrapy分布式爬取, 基于Redis的Scrapy组件(仅有组件)

   1. 组件: Scrapy Engine, Spiders, Scheduler, Downloader, Downloader Middlerwares, Item Pipeline, Redis, 

   2. 流程:
      1. Requests: Spiders  --> Scrapy Engine  --> Scheduler  --> Redis  --> Scheduler  --> Scrapy Engine   --> Downloader
      2. response: Downloader   -->  Scrapy Engine   -->  Spiders
      3. Items: Spiders  --> Scrapy Engine  --> Item Pipeline  --> Redis --> Item Processess.   但 Item Processess 不是 Scrapy-redis的组件

   3. `pip install scrapy-redis`

   4. scrapy-redis在scrapy的 上增加 redis, 基于redis 拓展四种组件（components）

      1. `Scheduler`
      2. `Duplication Filter`
      3. `Item Pipeline`
      4. `Base Spider`

   5. 原理:  Scrapy改造 python 的collection.deque(双向队列)形成 自己的爬取队列 Scrapy queue ([https://github.com/scrapy/queuelib/blob/master/queuelib/queue.py)](https://github.com/scrapy/queuelib/blob/master/queuelib/queue.py))，

      1. Scrapy 不支持爬虫的分布式, Scrapy 的多个spider不共享 爬取队列Scrapy queue

         1. Scrapy 用 集合 对请求request 去重. Scrapy 把已经发送的 request 指纹放入 一个集合, 把下一个 request 指纹拿到集合中比对. 如 该指纹存在于集合中，说明这个request发送过了, 如没有, 则继续操作 

         2. 这个判重的实现 

            1. ``` 
               def request_seen(self, request):
                   #创建请求request的指纹  
                   fp = self.request_fingerprint(request)
               
                   # 判重的操作，self.fingerprints 是指纹集合
                   if fp in self.fingerprints:
                   	return True  #直接返回
                   self.fingerprints.add(fp) #如果没有，就添加到指纹集合
                   if self.file:
                   	self.file.write(fp + os.linesep)
               ```
         
      2. scrapy-redis: 把 Scrapy queue换成 redis 数据库（ redis队列）

         1. 在同一个 redis-server中存放请求request， 让多个 spider从同一个数据库 读取

         2. scrapy-redis 由`Duplication Filter`组件 实现 去重 

3. 组件功能

   1. Scheduler : 提供方法 管理 队列字典

      1. 原理: 调度器 `Scheduler`  对新的 request 入列操作（加入Scrapy queue）, 取下一个要爬取的request（从Scrapy queue中取出）

         1. 把待爬队列 按 优先级建 一个字典结构，比如

            1. ```
               {
               	优先级0 : 队列0
               	优先级1 : 队列1
               	优先级2 : 队列2
               }
               ```
            
            1. 根据request中的优先级 决定 入队列， 优先级值较小 先出列。
         
      2. Scrapy原来的 Scheduler 无法 用， 用 Scrapy-redis的scheduler组件
      
   2. ` Duplication Filter`  :  用set实现 request 去重

      1. 原理: 用set 不重复的特性 实现了Duplication Filter去重
         1. scrapy-redis调度器从引擎接受request，将request的指纹存入 redis 的 set, 检查重复，并将不重复的request push推入redis的 request queue
         2. 引擎请求 request，调度器 从 redis 的 request queue 队列弹出pop 出一个request 返回给引擎，引擎将此 request 发给 spider 处理
      
   3. `Item Pipeline` : 将Item存入redis的items queue中
   
      1. 引擎将爬取到的Item给 Item Pipeline
      2. Item Pipeline 将传入 Item 存入redis的 items queue。
      3. `Item Pipeline`根据 key 从 items queue 提取item，实现 `items processes`集群
   
   4. `Base Spider` : 不再用 scrapy 原有的 Spider 类.  `RedisSpider`继承了 Spider 和 RedisMixin 两个类. RedisMixin 读取redis中的 url 类.   继承 RedisSpider 生成 爬虫时，调用 setup_redis 函数. setup_redis 函数 连接redis数据库, 设置signals(信号):
   
      1. spider空闲时的 signal, 调用 spider_idle 函数 , 这个函数调用 `schedule_next_request` 函数，保证spider 一直活着, 并且抛出DontCloseSpider异常。
      2. 抓到一个item时的 signal, 调用item_scraped函数, 这个函数 调用 `schedule_next_request` 函数，获取下一个request。

#### 6.1 源码分析参考：Connection

 1. scrapy-redis工程 : 主体还是 redis 和 scrapy 两个库，这个工程 像 胶水 一样，把两个 插件 粘结了起来. 工程本身实现的东西不是很多,  

 2. connection.py 

    https://github.com/rolando/scrapy-redis/blob/master/src/scrapy_redis/connection.py 

    1. 作用 :  根据 setting 配置 `实例化` redis. 被dupefilter和scheduler调用, 这个模块涉及到 redis 存取

       
       
    2. ```
       # ** connection.py **!
       #这里引入了redis模块，是redis-python库的接口，通过python访问redis数据库，
       # 这个文件实现连接redis数据库，这些连接接口在其他文件中经常被用到
       
       import redis
       import six
       
       from scrapy.utils.misc import load_object
       
       DEFAULT_REDIS_CLS = redis.StrictRedis
       
       #在 settings 文件中配置 套接字 的超时时间、等待时间等
       # Sane connection defaults.
       DEFAULT_PARAMS = {
           'socket_timeout': 30,
           'socket_connect_timeout': 30,
           'retry_on_timeout': True,
       }
       
       #连接到redis数据库，和其他数据库差不多，要一个ip地址、端口号、用户名密码（可选）和一个整形的数据库编号
       # Shortcut maps 'setting name' -> 'parmater name'.
       SETTINGS_PARAMS_MAP = {
           'REDIS_URL': 'url',
           'REDIS_HOST': 'host',
           'REDIS_PORT': 'port',
       }
       
       
       def get_redis_from_settings(settings):
           """Returns a redis client instance from given Scrapy settings object.
           This function uses ``get_client`` to instantiate the client and uses
           ``DEFAULT_PARAMS`` global as defaults values for the parameters. You can
           override them using the ``REDIS_PARAMS`` setting.
           Parameters
           ----------
           settings : Settings
               A scrapy settings object. See the supported settings below.
           Returns
           -------
           server
               Redis client instance.
           Other Parameters
           ----------------
           REDIS_URL : str, optional
               Server connection URL.
           REDIS_HOST : str, optional
               Server host.
           REDIS_PORT : str, optional
               Server port.
           REDIS_PARAMS : dict, optional
               Additional client parameters.
           """
           params = DEFAULT_PARAMS.copy()
           params.update(settings.getdict('REDIS_PARAMS'))
           # XXX: Deprecate REDIS_* settings.
           for source, dest in SETTINGS_PARAMS_MAP.items():
               val = settings.get(source)
               if val:
                   params[dest] = val
       
           # Allow ``redis_cls`` to be a path to a class.
           if isinstance(params.get('redis_cls'), six.string_types):
               params['redis_cls'] = load_object(params['redis_cls'])
       
           # 返回的是redis库的Redis对象，可以直接用来进行数据操作的对象
           return get_redis(**params)
       
       
       # Backwards compatible alias.
       from_settings = get_redis_from_settings
       
       
       def get_redis(**kwargs):
           """Returns a redis client instance.
           Parameters
           ----------
           redis_cls : class, optional
               Defaults to ``redis.StrictRedis``.
           url : str, optional
               If given, ``redis_cls.from_url`` is used to instantiate the class.
           **kwargs
               Extra parameters to be passed to the ``redis_cls`` class.
           Returns
           -------
           server
               Redis client instance.
           """
           redis_cls = kwargs.pop('redis_cls', DEFAULT_REDIS_CLS)
           url = kwargs.pop('url', None)
       
       
           if url:
               return redis_cls.from_url(url, **kwargs)
           else:
          return redis_cls(**kwargs)
       ```
    
       1. 接口 :  就像连接线. 

#### 6.2 源码分析参考：Dupefilter

1. dupefilter.py 作用

   1. requst的去重. 用redis的set数据结构。用queue.py模块 实现的queue。

   2. 当request不重复时，将其存入到queue中. 调度时 弹出

2. dupefilter.py 

   1. ```
      import logging
      import time
      
      from scrapy.dupefilters import BaseDupeFilter
      from scrapy.utils.request import request_fingerprint
      
      from .connection import get_redis_from_settings
      
      
      DEFAULT_DUPEFILTER_KEY = "dupefilter:%(timestamp)s"
      
      logger = logging.getLogger(__name__)
      
      
      # TODO: Rename class to RedisDupeFilter.
      class RFPDupeFilter(BaseDupeFilter):
          """Redis-based request duplicates filter.
          This class can also be used with default Scrapy's scheduler.
          """
      
          logger = logger
      
          def __init__(self, server, key, debug=False):
              """Initialize the duplicates filter.
              Parameters
              ----------
              server : redis.StrictRedis
                  The redis server instance.
              key : str
                  Redis key Where to store fingerprints.
              debug : bool, optional
                  Whether to log filtered requests.
              """
              self.server = server
              self.key = key
              self.debug = debug
              self.logdupes = True
      
          @classmethod
          def from_settings(cls, settings):
              """Returns an instance from given settings.
              This uses by default the key ``dupefilter:<timestamp>``. When using the
              ``scrapy_redis.scheduler.Scheduler`` class, this method is not used as
              it needs to pass the spider name in the key.
              Parameters
              ----------
              settings : scrapy.settings.Settings
              Returns
              -------
              RFPDupeFilter
                  A RFPDupeFilter instance.
              """
              server = get_redis_from_settings(settings)
              # XXX: This creates one-time key. needed to support to use this
              # class as standalone dupefilter with scrapy's default scheduler
              # if scrapy passes spider on open() method this wouldn't be needed
              # TODO: Use SCRAPY_JOB env as default and fallback to timestamp.
              key = DEFAULT_DUPEFILTER_KEY % {'timestamp': int(time.time())}
              debug = settings.getbool('DUPEFILTER_DEBUG')
              return cls(server, key=key, debug=debug)
      
          @classmethod
          def from_crawler(cls, crawler):
              """Returns instance from crawler.
              Parameters
              ----------
              crawler : scrapy.crawler.Crawler
              Returns
              -------
              RFPDupeFilter
                  Instance of RFPDupeFilter.
              """
              return cls.from_settings(crawler.settings)
      
          def request_seen(self, request):
              """Returns True if request was already seen.
              Parameters
              ----------
              request : scrapy.http.Request
              Returns
              -------
              bool
              """
              fp = self.request_fingerprint(request)
              # This returns the number of values added, zero if already exists.
              added = self.server.sadd(self.key, fp)
              return added == 0
      
          def request_fingerprint(self, request):
              """Returns a fingerprint for a given request.
              Parameters
              ----------
              request : scrapy.http.Request
              Returns
              -------
              str
              """
              return request_fingerprint(request)
      
          def close(self, reason=''):
              """Delete data on close. Called by Scrapy's scheduler.
              Parameters
              ----------
              reason : str, optional
              """
              self.clear()
      
          def clear(self):
              """Clears fingerprints data."""
              self.server.delete(self.key)
      
          def log(self, request, spider):
              """Logs given request.
              Parameters
              ----------
              request : scrapy.http.Request
              spider : scrapy.spiders.Spider
              """
              if self.debug:
                  msg = "Filtered duplicate request: %(request)s"
                  self.logger.debug(msg, {'request': request}, extra={'spider': spider})
              elif self.logdupes:
                  msg = ("Filtered duplicate request %(request)s"
                         " - no more duplicates will be shown"
                         " (see DUPEFILTER_DEBUG to show all duplicates)")
                  msg = "Filtered duplicate request: %(request)s"
                  self.logger.debug(msg, {'request': request}, extra={'spider': spider})
                  self.logdupes = False
      ```

      1. 重写了scrapy 的request判重功能。 

         1. scrapy单机运行， 读取内存中的request队列或持久化的request队列, 判断要发出的request url 是否已 请求过或者正在调度（本地读取）. scrapy默认的持久化 是json格式的文件，不是数据库）。
         2. scrapy分布式运行, 各个主机的 scheduler 连接同一个数据库的同一个 request 池, 判断 请求是否重复. 继承BaseDupeFilter, 重写它的方法，实现了基于 redis 的判重。
            1. scrapy-redis使用 scrapy 的 fingerprint 接收 request_fingerprint, 判断request请求是否已经出现过
            2. fingerprint 通过 hash 判断两个url是否相同. 相同的url会生成相同的hash结果. 
               1. 但 当两个url的地址相同，get型参数相同但是顺序不同时，也会生成相同的hash结果（这个真的比较神奇。。。）
      2. 原理: 这个类连接redis，使用 key 向redis的一个set中插入fingerprint
      
         1. 这个key对 同一种spider是相同的
            1. redis是 key-value 数据库， key 相同的，访问到的值 相同的
            2. 用spider名字+DupeFilter的key.  为了 属于同一种spider不同主机上的不同爬虫实例 访问到同一个set. 这个set 是 url判重池
            3. 返回值为0，set中该fingerprint已经存在（集合 没有重复值 ,返回False
            4. 返回值为1，说明添加了一个fingerprint到set中，说明这个 request 没有重复，返回True，还顺便把新 fingerprint 加入到数据库中了
         2. 在scheduler类中用到 DupeFilter 判重. 
            1. 每一个 request 进入调度之前都要进行判重
            2. 重复 的不参加调度， 舍弃. 不然 是白白浪费资源。

#### 6.3 源码分析参考：Picklecompat

 1. picklecompat.py

    1. ```
       """A pickle wrapper module with protocol=-1 by default."""
       
       try:
           import cPickle as pickle  # PY2
       except ImportError:
           import pickle
       
       
       def loads(s):
           return pickle.loads(s)
       
       
       def dumps(obj):
           return pickle.dumps(obj, protocol=-1)
       ```

         1. 实现了 loads 和 dumps 两个函数，即实现了一个序列化器。

            1. redis数据库不能存储复杂对象: 	1. key 只能是 字符串, 	2. value 只能是 字符串，字符串列表，字符串集合和hash. 	3. 存储前 要先 串行化 成文本
               
            2. pickle模块: py2和py3的 串行化 工具, 这个serializer 用于 scheduler 存储 reuqest 对象

#### 6.4 源码分析参考：Pipelines

 1. pipelines.py:

    1. 分布式处理。将Item存储在redis中 实现分布式处理. 用 from_crawler()函数 读取配置

    2. ```
       from scrapy.utils.misc import load_object
       from scrapy.utils.serialize import ScrapyJSONEncoder
       from twisted.internet.threads import deferToThread
       
       from . import connection
       
       
       default_serialize = ScrapyJSONEncoder().encode
       
       
       class RedisPipeline(object):
           """Pushes serialized item into a redis list/queue"""
       
           def __init__(self, server,
                        key='%(spider)s:items',
                        serialize_func=default_serialize):
               self.server = server
               self.key = key
               self.serialize = serialize_func
       
           @classmethod
           def from_settings(cls, settings):
               params = {
                   'server': connection.from_settings(settings),
               }
               if settings.get('REDIS_ITEMS_KEY'):
                   params['key'] = settings['REDIS_ITEMS_KEY']
               if settings.get('REDIS_ITEMS_SERIALIZER'):
                   params['serialize_func'] = load_object(
                       settings['REDIS_ITEMS_SERIALIZER']
                   )
       
               return cls(**params)
       
           @classmethod
           def from_crawler(cls, crawler):
               return cls.from_settings(crawler.settings)
       
           def process_item(self, item, spider):
               return deferToThread(self._process_item, item, spider)
       
           def _process_item(self, item, spider):
               key = self.item_key(item, spider)
               data = self.serialize(item)
               self.server.rpush(key, data)
               return item
       
           def item_key(self, item, spider):
               """Returns redis key based on given spider.
               Override this function to use a different key depending on the item
               and/or spider.
               """
               return self.key % {'spider': spider.name}
       ```
    
       1. 实现 `item pipieline` 类，
       
          2. 和scrapy的`item pipeline`是同一个对象
       
         	2. 从settings中拿到配置的 `REDIS_ITEMS_KEY` 作key， item串行化后存入 redis 数据库对应的value中. 
              
              这个 value 是list， 每个 item 是这个list中的一个结点. pipeline 存储 取出的item, 为了 延后处理数据。

#### 6.5 源码分析参考：Queue

1. queue.py

   1. ```
      from scrapy.utils.reqser import request_to_dict, request_from_dict
      
      from . import picklecompat
      
      
      class Base(object):
          """Per-spider queue/stack base class"""
      
          def __init__(self, server, spider, key, serializer=None):
              """Initialize per-spider redis queue.
              Parameters:
                  server -- redis connection
                  spider -- spider instance
                  key -- key for this queue (e.g. "%(spider)s:queue")
              """
              if serializer is None:
                  # Backward compatibility.
                  # TODO: deprecate pickle.
                  serializer = picklecompat
              if not hasattr(serializer, 'loads'):
                  raise TypeError("serializer does not implement 'loads' function: %r"
                                  % serializer)
              if not hasattr(serializer, 'dumps'):
                  raise TypeError("serializer '%s' does not implement 'dumps' function: %r"
                                  % serializer)
      
              self.server = server
              self.spider = spider
              self.key = key % {'spider': spider.name}
              self.serializer = serializer
      
          def _encode_request(self, request):
              """Encode a request object"""
              obj = request_to_dict(request, self.spider)
              return self.serializer.dumps(obj)
      
          def _decode_request(self, encoded_request):
              """Decode an request previously encoded"""
              obj = self.serializer.loads(encoded_request)
              return request_from_dict(obj, self.spider)
      
          def __len__(self):
              """Return the length of the queue"""
              raise NotImplementedError
      
          def push(self, request):
              """Push a request"""
              raise NotImplementedError
      
          def pop(self, timeout=0):
              """Pop a request"""
              raise NotImplementedError
      
          def clear(self):
              """Clear queue/stack"""
              self.server.delete(self.key)
      
      
      class SpiderQueue(Base):
          """Per-spider FIFO queue"""
      
          def __len__(self):
              """Return the length of the queue"""
              return self.server.llen(self.key)
      
          def push(self, request):
              """Push a request"""
              self.server.lpush(self.key, self._encode_request(request))
      
          def pop(self, timeout=0):
              """Pop a request"""
              if timeout > 0:
                  data = self.server.brpop(self.key, timeout)
                  if isinstance(data, tuple):
                      data = data[1]
              else:
                  data = self.server.rpop(self.key)
              if data:
                  return self._decode_request(data)
      
      
      class SpiderPriorityQueue(Base):
          """Per-spider priority queue abstraction using redis' sorted set"""
      
          def __len__(self):
              """Return the length of the queue"""
              return self.server.zcard(self.key)
      
          def push(self, request):
              """Push a request"""
              data = self._encode_request(request)
              score = -request.priority
              # We don't use zadd method as the order of arguments change depending on
              # whether the class is Redis or StrictRedis, and the option of using
              # kwargs only accepts strings, not bytes.
              self.server.execute_command('ZADD', self.key, score, data)
      
          def pop(self, timeout=0):
              """
              Pop a request
              timeout not support in this queue class
              """
              # use atomic range/remove using multi/exec
              pipe = self.server.pipeline()
              pipe.multi()
              pipe.zrange(self.key, 0, 0).zremrangebyrank(self.key, 0, 0)
              results, count = pipe.execute()
              if results:
                  return self._decode_request(results[0])
      
      
      class SpiderStack(Base):
          """Per-spider stack"""
      
          def __len__(self):
              """Return the length of the stack"""
              return self.server.llen(self.key)
      
          def push(self, request):
              """Push a request"""
              self.server.lpush(self.key, self._encode_request(request))
      
          def pop(self, timeout=0):
              """Pop a request"""
              if timeout > 0:
                  data = self.server.blpop(self.key, timeout)
                  if isinstance(data, tuple):
                      data = data[1]
              else:
                  data = self.server.lpop(self.key)
      
              if data:
                  return self._decode_request(data)
      
      
      __all__ = ['SpiderQueue', 'SpiderPriorityQueue', 'SpiderStack']
      ```

   2. 原理: 实现 几个容器类， 这些容器和redis交互， 用picklecompat定义的序列化器。	

      1. 几个容器大体相同， 一个是 队列，一个是 栈，一个是 优先级队列
         1. 三个容器 被 scheduler 对象实例化, 实现request的调度
         2. 如SpiderQueue 为调度队列 类型, request的调度 是先进先出, SpiderStack 是先进后出
      2. SpiderQueue 实现 (分析)
         1. push时, request请求先被scrapy的接口request_to_dict 变成了一个dict对象. request对象 复杂， 不好 串行化, 用picklecompat中的serializer串行化为字符串，用一个特定的key存入redis中（该key在同一种spider中是相同的）。pop时，从redis用那个特定的key 读其值（一个list）, 从list 读取最早进去的那个，于是就先进先出了
         2. 这些容器类 作为scheduler调度request的容器
            1. scheduler在每个主机上 实例化一个，并且和spider一一对应
            2. 分布式运行时, 一个spider的多个实例和一个scheduler的多个实例存在于不同的主机上. scheduler 用相同的容器， 容器 连接同一个redis服务器, 都使spider名加queue 作为key读写数据
            3. 不同主机上的不同爬虫实例 共用一个request调度池，实现了分布式爬虫 的统一调度

#### 6.6 源码分析参考：Scheduler

1. 原理: 替代scrapy 自带的scheduler（在settings的SCHEDULER变量中指出）, 实现crawler的分布式调度。 数据结构来自 queue 的数据结构

   2. scrapy-redis 两种分布式
      1. 爬虫分布式, item处理分布式 由 scheduler和 pipelines实现. 
      2. 其它模块作为 二者辅助的功能模块
   
2. scheduler.py

   1. ```
      import importlib
      import six
      
      from scrapy.utils.misc import load_object
      
      from . import connection
      
      
      # TODO: add SCRAPY_JOB support.
      class Scheduler(object):
          """Redis-based scheduler"""
      
          def __init__(self, server,
                       persist=False,
                       flush_on_start=False,
                       queue_key='%(spider)s:requests',
                       queue_cls='scrapy_redis.queue.SpiderPriorityQueue',
                       dupefilter_key='%(spider)s:dupefilter',
                       dupefilter_cls='scrapy_redis.dupefilter.RFPDupeFilter',
                       idle_before_close=0,
                       serializer=None):
              """Initialize scheduler.
              Parameters
              ----------
              server : Redis
                  The redis server instance.
              persist : bool
                  Whether to flush requests when closing. Default is False.
              flush_on_start : bool
                  Whether to flush requests on start. Default is False.
              queue_key : str
                  Requests queue key.
              queue_cls : str
                  Importable path to the queue class.
              dupefilter_key : str
                  Duplicates filter key.
              dupefilter_cls : str
                  Importable path to the dupefilter class.
              idle_before_close : int
                  Timeout before giving up.
              """
              if idle_before_close < 0:
                  raise TypeError("idle_before_close cannot be negative")
      
              self.server = server
              self.persist = persist
              self.flush_on_start = flush_on_start
              self.queue_key = queue_key
              self.queue_cls = queue_cls
              self.dupefilter_cls = dupefilter_cls
              self.dupefilter_key = dupefilter_key
              self.idle_before_close = idle_before_close
              self.serializer = serializer
              self.stats = None
      
          def __len__(self):
              return len(self.queue)
      
          @classmethod
          def from_settings(cls, settings):
              kwargs = {
                  'persist': settings.getbool('SCHEDULER_PERSIST'),
                  'flush_on_start': settings.getbool('SCHEDULER_FLUSH_ON_START'),
                  'idle_before_close': settings.getint('SCHEDULER_IDLE_BEFORE_CLOSE'),
              }
      
              # If these values are missing, it means we want to use the defaults.
              optional = {
                  # TODO: Use custom prefixes for this settings to note that are
                  # specific to scrapy-redis.
                  'queue_key': 'SCHEDULER_QUEUE_KEY',
                  'queue_cls': 'SCHEDULER_QUEUE_CLASS',
                  'dupefilter_key': 'SCHEDULER_DUPEFILTER_KEY',
                  # We use the default setting name to keep compatibility.
                  'dupefilter_cls': 'DUPEFILTER_CLASS',
                  'serializer': 'SCHEDULER_SERIALIZER',
              }
              for name, setting_name in optional.items():
                  val = settings.get(setting_name)
                  if val:
                      kwargs[name] = val
      
              # Support serializer as a path to a module.
              if isinstance(kwargs.get('serializer'), six.string_types):
                  kwargs['serializer'] = importlib.import_module(kwargs['serializer'])
      
              server = connection.from_settings(settings)
              # Ensure the connection is working.
              server.ping()
      
              return cls(server=server, **kwargs)
      
          @classmethod
          def from_crawler(cls, crawler):
              instance = cls.from_settings(crawler.settings)
              # FIXME: for now, stats are only supported from this constructor
              instance.stats = crawler.stats
              return instance
      
          def open(self, spider):
              self.spider = spider
      
              try:
                  self.queue = load_object(self.queue_cls)(
                      server=self.server,
                      spider=spider,
                      key=self.queue_key % {'spider': spider.name},
                      serializer=self.serializer,
                  )
              except TypeError as e:
                  raise ValueError("Failed to instantiate queue class '%s': %s",
                                   self.queue_cls, e)
      
              try:
                  self.df = load_object(self.dupefilter_cls)(
                      server=self.server,
                      key=self.dupefilter_key % {'spider': spider.name},
                      debug=spider.settings.getbool('DUPEFILTER_DEBUG'),
                  )
              except TypeError as e:
                  raise ValueError("Failed to instantiate dupefilter class '%s': %s",
                                   self.dupefilter_cls, e)
      
              if self.flush_on_start:
                  self.flush()
              # notice if there are requests already in the queue to resume the crawl
              if len(self.queue):
                  spider.log("Resuming crawl (%d requests scheduled)" % len(self.queue))
      
          def close(self, reason):
              if not self.persist:
                  self.flush()
      
          def flush(self):
              self.df.clear()
              self.queue.clear()
      
          def enqueue_request(self, request):
              if not request.dont_filter and self.df.request_seen(request):
                  self.df.log(request, self.spider)
                  return False
              if self.stats:
                  self.stats.inc_value('scheduler/enqueued/redis', spider=self.spider)
              self.queue.push(request)
              return True
      
          def next_request(self):
              block_pop_timeout = self.idle_before_close
              request = self.queue.pop(block_pop_timeout)
              if request and self.stats:
                  self.stats.inc_value('scheduler/dequeued/redis', spider=self.spider)
              return request
      
          def has_pending_requests(self):
              return len(self) > 0
      ```

      1. 重写了scheduler类， 代替原有的调度器 scrapy.core.scheduler 
         1. 对原有调度器的逻辑没有很大的改变
         2. 用 redis作 数据存储的媒介, 统一调度各个爬虫。 
         3. scheduler 调度各个spider的request请求. scheduler初始化时，通过settings文件读取queue和dupefilters的类型（用默认的），配置queue和dupefilters 的key
            1. 一般 是spider name加上queue或者dupefilters. 这样对 同一种spider的不同实例，使用了相同的数据块
            2. 当一个request 被调度时，enqueue_request 被调用，scheduler 用 dupefilters 判断这个url是否重复，如果不重复，就添加到queue的容器中
            3. 先进先出，先进后出, 优先级都可以，在settings中配置
            4. 当调度完成时，next_request 被调用，scheduler 通过 queue 容器的接口，取出一个request， 发送给相应的spider，让spider 爬取工作。

#### 6.7 源码分析参考：Spider

1. spider.py	: 

   1. 原理 : spider从 redis 读取要爬的url,然后 爬取. 若爬取返回更多的url，那么继续进行直至所有的request完成。之后 从redis中读取url，循环这个过程

   2. 机制
   
      1. spider 通过 connect signals.spider_idle 信号 监视crawler状态。
      2. 当idle时，返回新的请求给引擎 ( make_requests_from_url(url) )，进而交给 调度器调度
   
2. spider.py  代码

   1. ```
      from scrapy import signals
      from scrapy.exceptions import DontCloseSpider
      from scrapy.spiders import Spider, CrawlSpider
      
      from . import connection
      
      
      # Default batch size matches default concurrent requests setting.
      DEFAULT_START_URLS_BATCH_SIZE = 16
      DEFAULT_START_URLS_KEY = '%(name)s:start_urls'
      
      
      class RedisMixin(object):
          """Mixin class to implement reading urls from a redis queue."""
          # Per spider redis key, default to DEFAULT_START_URLS_KEY.
          redis_key = None
          # Fetch this amount of start urls when idle. Default to DEFAULT_START_URLS_BATCH_SIZE.
          redis_batch_size = None
          # Redis client instance.
          server = None
      
          def start_requests(self):
              """Returns a batch of start requests from redis."""
              return self.next_requests()
      
          def setup_redis(self, crawler=None):
              """Setup redis connection and idle signal.
              This should be called after the spider has set its crawler object.
              """
              if self.server is not None:
                  return
      
              if crawler is None:
                  # We allow optional crawler argument to keep backwards
                  # compatibility.
                  # XXX: Raise a deprecation warning.
                  crawler = getattr(self, 'crawler', None)
      
              if crawler is None:
                  raise ValueError("crawler is required")
      
              settings = crawler.settings
      
              if self.redis_key is None:
                  self.redis_key = settings.get(
                      'REDIS_START_URLS_KEY', DEFAULT_START_URLS_KEY,
                  )
      
              self.redis_key = self.redis_key % {'name': self.name}
      
              if not self.redis_key.strip():
                  raise ValueError("redis_key must not be empty")
      
              if self.redis_batch_size is None:
                  self.redis_batch_size = settings.getint(
                      'REDIS_START_URLS_BATCH_SIZE', DEFAULT_START_URLS_BATCH_SIZE,
                  )
      
              try:
                  self.redis_batch_size = int(self.redis_batch_size)
              except (TypeError, ValueError):
                  raise ValueError("redis_batch_size must be an integer")
      
              self.logger.info("Reading start URLs from redis key '%(redis_key)s' "
                               "(batch size: %(redis_batch_size)s)", self.__dict__)
      
              self.server = connection.from_settings(crawler.settings)
              # The idle signal is called when the spider has no requests left,
              # that's when we will schedule new requests from redis queue
              crawler.signals.connect(self.spider_idle, signal=signals.spider_idle)
      
          def next_requests(self):
              """Returns a request to be scheduled or none."""
              use_set = self.settings.getbool('REDIS_START_URLS_AS_SET')
              fetch_one = self.server.spop if use_set else self.server.lpop
              # XXX: Do we need to use a timeout here?
              found = 0
              while found < self.redis_batch_size:
                  data = fetch_one(self.redis_key)
                  if not data:
                      # Queue empty.
                      break
                  req = self.make_request_from_data(data)
                  if req:
                      yield req
                      found += 1
                  else:
                      self.logger.debug("Request not made from data: %r", data)
      
              if found:
                  self.logger.debug("Read %s requests from '%s'", found, self.redis_key)
      
          def make_request_from_data(self, data):
              # By default, data is an URL.
              if '://' in data:
                  return self.make_requests_from_url(data)
              else:
                  self.logger.error("Unexpected URL from '%s': %r", self.redis_key, data)
      
          def schedule_next_requests(self):
              """Schedules a request if available"""
              for req in self.next_requests():
                  self.crawler.engine.crawl(req, spider=self)
      
          def spider_idle(self):
              """Schedules a request if available, otherwise waits."""
              # XXX: Handle a sentinel to close the spider.
              self.schedule_next_requests()
              raise DontCloseSpider
      
      
      class RedisSpider(RedisMixin, Spider):
          """Spider that reads urls from redis queue when idle."""
      
          @classmethod
          def from_crawler(self, crawler, *args, **kwargs):
              obj = super(RedisSpider, self).from_crawler(crawler, *args, **kwargs)
              obj.setup_redis(crawler)
              return obj
      
      
      class RedisCrawlSpider(RedisMixin, CrawlSpider):
          """Spider that reads urls from redis queue when idle."""
      
          @classmethod
          def from_crawler(self, crawler, *args, **kwargs):
              obj = super(RedisCrawlSpider, self).from_crawler(crawler, *args, **kwargs)
              obj.setup_redis(crawler)
              return obj
      ```

      1. spider 改动 不大
         1. 通过connect接口，给spider绑定了spider_idle信号
         2. spider初始化时，通过 setup_redis 函数初始化和redis的连接，之后通过next_requests函数从redis中取出strat url， key是settings中REDIS_START_URLS_AS_SET定义的
            1. 注意 这里的初始化 url 池和 queue的 url 池不是一个东西
            2. queue的 池 用于调度的，而 初始化url池 存放 入口url， 都存在redis中， 使用不同的key来区分， 是不同
         3. spider 用少量的start url， 发展出很多新的url，这些url 进入scheduler 判重和调度。直到spider执行到 调度池 没有url 触发spider_idle信号，从而触发spider的 next_requests 函数，再次从redis的 start url 池 读取一些url

3. 总结: scrapy-redis的总体思路

   1. 重写scheduler和spider类, 实现了 调度、spider启动, redis的交互。实现新的dupefilter和queue类: 判重,调度容器, redis的交互
      1. 每个主机上的 爬虫访问同一个redis数据库，所以 调度 和 判重 统一管理，达到了分布式爬虫的目的
      2. spider被初始化时，同时会初始化一个对应的 scheduler 对象
         1. 这个调度器对象 读取settings，配置 自己的调度容器 queue 和判重工具 dupefilter 。
         2. 当一个spider产出一个request时, scrapy 内核 把这个reuqest 交给 spider对应的scheduler对象进行调度. scheduler对象 访问 redis, 对 request 判重. 不重复 添加进redis中的调度池。调度条件满足时，scheduler对象 从redis的调度池中取出一个request发送给spider，让他爬取。
      3. 当spider爬取 暂时可用url之后，scheduler发现这个spider对应的redis的调度池空了， 触发信号spider_idle，spider收到这个信号， 连接redis读取strart url池，拿去新的一批url入口， 再次重复上边的工作

##### 6 小结 scrapy-redis

1. scrapy-redis :  功能强大

   1. request去重. 即 已经爬过的 url 不再爬取
   2. 爬虫 持久化
   3. 轻松实现 分布式. Redis-based components for Scrapy

2. scrapy-redis 爬取流程

   1. Redis : 通过 redis 实现调度器的 队列 和 指纹集合
   2. Item Pipeline : 数据处理, 数据默认存储到 redis
   3. Spiders : 提取数据, 提取url

3. 复习 redis

   1. Redis : 开源的，内存数据库. 用作 数据库、缓存和消息中间件

      1. 支持多种类型的 数据结构，如字符串，哈希，列表，集合，有序集合等
      

4. 常用命令:

   1. redis服务器:

      1. `/etc/ init.d/redis-server stop`  # redis停止
      2. `/etc/ init.d/ redis-server start`   # 启动
      3. `/etc/ init.d/ redis-server restart`  # 重 启
      4. `redis-cli -h <hostname> -p <port>`  #客户端 远程连接redis数据库

   1. redis中:

      1. `select 1`  #切换到数据库1, 默认在数据库0
      2. `keys *`  #查看所有的redis键
      3. `type "键"`  #查看键的数据类型
      4. `flushdb`  #清空当前数据库
      5. `flushall`  #清空所有数据库

   2. 列表

      1. `LPUSH mylist "world"`  # 从左边向列表mylist添加一个值 `"world"`
      2. `LRANGE mylist 0   -1`  #返回 列表mylist中所有的值
      3. `LLEN mylis`  #返回列表mylist的长度

   3. set 

      1. `SADD myset "Hello"`  #往set中添加数据,  set的名称是 myset
      2. `SMEMBERS myset`  # 获取myset中所有的元素
      3. `SCARD myset`  #scrad 获取数量, 

   4. zset

      1. `ZADD myzset 1 "one"`  #值"one", 分数是 1.  (integer) 1

      2. `ZADD myzset 2 "two" 3 "three"`  #值"two", 分数是 2, 值 "three", 分数3. (integer)  2

      3. `ZRANGE myzset 0 -1 WITHSCORES ;`  #zrange遍历myzeset

         1. ```
               #输出
               1) "one"
               2) "1"
               3) "two"
               4) "2"
               5) "three"
               6) "3"
               ```

      4. `ZCARD myzset`  #zcard返回 zset 中元素的数量

5. 示例:

   2. settings.py,  Scrapy_redis组件1: 之 settings

      1. ```
         SPIDER_MODULES = ['example.spiders']
         NEWSPIDER_MODULE = 'example.spiders '
         USER_AGENT = 'scrapy-redis(+https://github.com/rolando/scrapy- redis)'
         DUPEFILTER_CLASS = "scrapy_edis.dupefilter.RFPDupeFilter"  #指定去重方法,给request对象去重
         SCHEDULER = "scrapy_redis.scheduler.Scheduler"  #指定scheduler队列
         SCHEDULER_PERSIST = True   #队列中的内容是否持久保存， False:关闭redis的时候清空redis
         #SCHEDULER_QUEUE_CLASS = "scrapy_redis.queue.SpiderPriorityQueue"
         #SCHEDULER_QUEUE_CLASS = "scrapy_redis.queue.SpiderQueue"
         #SCHEDULER_QUEUE_CLASS = "scrapy_redis.queue.SpiderStack"
         ITEM_PIPELINES = {
         'example.pipelines.ExamplePipeline':300,
         'scrapy_redis.pipelines.RedisPipeline':400，  #scrapy_redis实现的items保存到redis的pipeline
         }
         LOG_LEVEL = 'DEBUG '
         DOWNLOAD_DELAY = 0.3
         #指定redis服务器 IP:Port
         REDIS_URL = 'redis://192.168.207.130:6379 '  
         
         #redis的地址可以写成如下形式
         #REDIS_HOST = "192.168.207.134"  
         #REDIS_ PORT = 6379
         ```

   3. RedisPipelines.py: Scrapy_redis组件2:  之 Redispipeline

      1. ```
         #代码片段
         def process_item(self, item, spider):   #使用了process_ltem的方法，实现数据的保存
         	return deferToThread(self._process_item, item， spider)  #调用的一个异步线程 处理这个item
         	
         	
         def _process_item(self, item， spider):
         	key = self.item_key(item， spider)
         	data = self.serialize(item)
         	self.server.rpush(key，data)  #向dmoz:items中添加item
         	return item
         ```
         
         

   3. dupefilter.py. Scrapy_redis爬虫组件3:  之 RFPDupeFilter

      1. ```
         def request_seen(self, request):  #判断requests对象是否已经存在
         	#指纹
             fp = self.request_fingerprint(request)
         	added = self.server.sadd(self.key, fp)  #添加到dupefilter中
         	return added ==0  #返回0表示添加失败，即已经存在，否则表示不存在
         	
         def request_fingerprint(self, request):
         	return request_fingerprint (request)
         	
         def request_fingerprint(request, include_headers=None):
         	if include_headers:
         		include_headers = tuple(to_bytes(h.lower()) for h in sorted(include_headers))
         		
         	cache = _fingerprint_cache.setdefault(request, {})
         	if include_headers not in cache:
         		fp = hashlib.shal()  #sha1加密
         		fp.update(to_bytes(request.method))  #请求方法
         		fp.update(to_bytes(canonicalize_url(request.url))) #请求地址
         		fp.update(request.body or b'')  #请求体，post请求才会有
         		if include_headers:  #添加请求头，默认不添加请求头(因为header的cookies中含有session id, 这在不同的网站中是随机的，会给sha1的计算结果带来误差)
         			for hdr in include_headers:
         				if hdr in request.headers:
         					fp.update(hdr)
         					for V in request.headers.getlist(hdr):
         						fp.update(v)
         		cache[include_headers] = fp.hexdigest()  #返回加密之后的16进制
         	return cache[include_ headers]
         ```
         
         

   5. scheduler.py, Scrapy_redis爬虫组件4: 之 Scheduler

      1. Scheduler 代码片段

      2. ```
         #代码片段
         def close(self, reason):
         	if not self.persist: #如果在setting中设置为不持久, 那么退出的时候清空
         		self.flush()
         		
         def flush(self):
         	self.df.clear()  #指的是存放dupefilter的redis
         	self.queue.clear()  #指的是存放requests的redis
         
         def enqueue_request(self, request):
         	if not request.dont_filter and self.df.request_seen(request):  #不能加入待爬队列的条件. 当前url需要(经过allow_domain)过滤并且request不存在dp的时候, 因此:对于像百度贴吧这种页面内容会更新的网址，可以设置dont_filter为True让其能够被反复抓取
         		self.df.log(request, self.spider)
         		return False
         
         	if self.stats:
         		self.stats.inc_value('scheduler/enqueued/redis'，spider=self.spider)
         	self.queue.push(request)
         	return True
         ```
         
         

   6. myspider_redis.py, Scrapy_redis组件: 5 之  RedisSpider  

      1. ```
         class MySpider (RedisSpider):
         	name = 'myspider_redis'  #指定爬虫名
         	redis_key = 'myspider:start_urls'  #指定redis中start_urls的键,启动时向对应的键 存入url地址，不同位置的爬虫获取该url.
         	#所以启动爬虫的命令分类两个步骤:
         	#①scrapy crawl myspider_redis (或者scrapy runspider myspider_redis), 让爬虫就绪
         	#②在redis中输入lpush myspider:start.urls "http://dmoztools.net/" 让爬虫从这个url开始爬取
         	allow_doamin = ["dmoztools.net"]  #手动指定allow_domain
         	
         	def __init__ (self, *args, **kwargs):  #动态的设置allow_domain,一般不需要，直接手动指定即可
         		domain = kwargs.pop('domain','')
         		self.allowed_domains = filter(None, domain.split(','))
         		super(MySpider, self).__init__ (*args,，**kwargs)
         		
         	def parse(self, response) :
         		return{
         			'name': response.css('title::text').extract_.first(),
         			'url': response.url,
         		}
         ```
         
         

   7. mycrawl_redis.py, Scrapy_redis组件: 6 之  RedisCrawlSpider

      1. ```
         from scrapy.spiders import Rule
         from scrapy.linkextractors import LinkExtractor
         from scrapy_redis.spiders import RedisCrawlSpider
         
         class MyCrawler(RedisCrawlSpider):
         	name = 'mycrawler_redis'  #爬虫名字
         	redis_key = 'mycrawler:start_urls'  #start_url的redis的键
         	allow_domains = ["dmoztools.net"]  #手动制定allow_domains
         	rules = (  #和crawl一样，指定url的过滤规则		Rule(LinkExtractor(),callback='parse_page',follow=True)
         	)
         	
         	def __init__(self, *args, **kwargs):  #动态生成all_domain,不是必须的
         		domain = kwargs.pop('domain'，' ')
         		self.allowed_domains = filter(None, domain.split(','))
         		super(MyCrawler，self).__init__(*args, **kwargs)
         		
         	def parse_page(self, response):
         		return {
         			'name': response.css('title::text').extract_first(),
         			'url': response.url,
         		}
         ```

         

   7.  Scrapy_redis 之 domz爬虫 

      1. dmoz.py 爬虫,  Scrapy_redis组件1: 之 CrawlSpider

         1. ```
            from scrapy.linkextractors import LinkExtractor
            from scrapy.spiders import CrawlSpider, Rule
            
            class DmozSpider(CrawlSpider):
            	"""Follow categories and extract links."""
            	name='dmoz'
            	allowed_domains = ['dmoztools.net']
            	start_urls = ['http://dmoztools.net/']
            	
            	rules = [
            		Rule(LinkExtractor(  #定义了一个url的提取规则，满足的响应交给callback函数处理
            		restrict_css=('.top-cat'，'.sub-cat', '.cat-item')
            		)，callback= 'parse_directory', follow=True) ，
            	]
            	
            	def parse_directory(self, response):
            		for div in response.css('.title-and-desc' ):
            			yield {  #yield给引擎
            				'name': div.css('.site-title::text').extract_first(),
            				'description': div.css('.site-descr::text').extract_first().strip(),
            				'link': div.css('a::attr(href)').extract_first(),
            			}
            ```

            1. domz爬虫和 crawlspider没有任何区别
            2. `::`  对应标签的 text 或 attr(herf) 属性
               1. `div.css('.site-descr::text')`
               2. `a::attr(href)`
            3. response.css('css格式')   #用css 选择标签
            4. `div.css('.site-title::text')`   #div标签 有 css类选择器'site-title' 的文本.

   8. 执行 domz 爬虫, 发现 redis 多了三个键

      1. 127.0.0.1:6379> `keys *`
         1. "dmoz : requests"   
            1. Scheduler队列, 存放 待请求的 reques 对象, 获取方法是pop操作,即获取一个 去除一个
         2. "dmoz :dupefilter"
            1. 指纹集合, 存放的是已进入 scheduler 队列的 request 对象的指纹, 指纹默认由 `请求方法, url, 请求体` 组成
         3. "dmoz: items"
            1. 存放 获取到的 item 信息, 在pipeline中开启RedisPipeline才会存入
      2. settings.py 中关闭 redispipeline,  变化结果:
         1. dmoz:requests 有变化(变多或者变少或者不变), dmoz:dupefilter 变多,  dmoz:items 不变
         2. 变化结果分析
            1. `redispipeline` 仅实现了 item 数据存储到 redis
            2. 可新建一个 `pipeline` (或者修改默认的ExamplePipeline ) , 让
               数据存储到任意地方
      3. scrapy_redis如何实现这 三个键的 ??

   9. Scrapy_redis 之 dmoz 小结

      1. domz 比于之前的 spider 多了 持久化 request去重
      2. 之后的爬虫中,我们模仿 domz 的用法, 使用scrapy_ redis实现相同的功能
      3. 注意:
         1.  setting中的配置 可以自己设定的
         2.  意味着我们的可以 重写 去重和调度器的方法,包括是否 把数据存储到redis(pipeline)

6. 动手练习

   1. 抓取京东图书
      1. 项目分析:
         1. 需求: 抓取京东图书的信息
         2. 目标: 抓取京东图书包含图书的名字、封面图片地址、图书 url 地址、 作者、出版社、出版时间、价格、图书所属大分类、图书所属小的分类、分类的url地址
         3. url : https://book.jd.com/booksort.html
   2. 抓取亚马逊图书
      1. 需求: 抓取亚马逊图书的信息
      2. 目标: 抓取亚马逊图书 有图书的名字封面图片地址、图书url地址、 作者、出版社、 出版时间、价格、图书所属大分类、图书所属小的分类、分类的url地址
      3. url : https://www.amazon.cn/%E5%9B%BE%E4%B9%A6/b/ref=sd_allcat_books_I1?ie=UTF8&node=658390051

7. Crontab : 爬虫定时执行 

   1. Crontab:  安装: `apt-get install cron`  (服务器环境下默认安装)

      2. 使用: 

         1. `crontab -e`  #进入编辑页面(第一次让你选择编辑器)
         2. `crontab -l`  #查看当前的定时任务
      
      3. 编辑:

         1. <table>
                <tr><td>分</td><td>小时</td><td>日</td><td>月</td><td>星期</td><td>命令</td></tr>
                <tr><td>0-59</td><td>0-23</td><td>1-31</td><td>1-12</td><td>0-6</td><td>command</td></tr>
            </table>
      
      4. 示例

         1. <table>
                <tr><td>30</td><td>7</td><td>8</td><td>*</td><td>*</td><td>ls</td><td>指定每月8号的7: 30分执行Is命令</td></tr>
                <tr><td>*/15</td><td>*</td><td>*</td><td>*</td><td>*</td><td>ls</td><td>每15分钟执行一次ls命令 [即每个小时的第0 15 30 45 60分钟执行Is命令]</td></tr>
                <tr><td>0</td><td>*/2</td><td>*</td><td>*</td><td>*</td><td>ls</td><td>每隔两个小时执行一次ls</td></tr>
            </table>
      
      5. 注意点:
         1.星期中 `0` 表示周日
         2.每隔两个小时的时候前面的不能为`*`, 为`*`表示分钟都会执行
      
   2. 定时执行

      1. 执行python程序:

         1. 先把 python 的程序写入 `.sh` 脚本
         2. 给 `.sh` 脚本添加可执行权限, `chmod +X myspider.sh`
         3. 把 `.sh` 脚本写入crontab配置文件

      2. `myspider.sh` 的例子

         1. ```
            #!/bin/sh  #使用/bin/sh 执行下面的内容
            cd `dirname $0` || exit 1  #cd到当前目录,失败则退出. dirname上面不是引号
            python ./main.py >>run.1og 2>&1  
            ```

      3. 对应 crontab 中的编写(注意写绝对路径)

         1. ```
            #crontab
            0 6 * * * /home/ubuntu/***/myspider.sh >> /home/ubuntu/***/run.1og 2>&1  #把屏幕输出的内容从定向到run.log,同时把 标准错误 作 标准输出 输出到run.log
            ```

            

### 7 scrapy-redis实战

#### redis的 Master, Slave启动

1. Scrapy-Redis分布式策略

   1. 架构 : 四台电脑：Windows 10、Mac OS X、Ubuntu 16.04、CentOS 7.2, 都可为  Master端 或 Slaver端
      2. `Master端`(核心服务器) ：  Windows 10. 
         1. 搭建一个Redis数据库
         2. 负责: url指纹 判重、Request的 分配， 数据的存储. 不负责爬取. 
      3. `Slaver端`(爬虫程序执行端) ：  Mac OS X 、Ubuntu 16.04、CentOS 7.2
         1.  执行爬虫程序
         2.  运行过程 提交新的 Request 给 Master
      4. Master 连接 Slaver, Slaver连接Internet
   2. Scrapy-Redis默认策略(执行过程)
      1. 首先 Slaver 端从 Master 端获取任务（Request）, 抓取数据，Slaver抓取数据 时，产生新任务的 Request 提交给 Master 处理；
      2. Master端只有一个Redis数据库. 
         1. 对未处理的 Request 去重和任务分配，处理后的 Request 加入待爬队列，
         2. 存储爬取的数据。
   3. 我们的任务
      1. 继承RedisSpider、 指定redis_key就行
      2. Scrapy-Redis 已经帮我们做好 任务调度 
   4. 缺点
      1. Scrapy-Redis调度的任务是 Request 对象.  Request对象的 信息量 大（不仅包含url，还有callback函数、headers等信息）, 降低爬虫速度、 占用 Redis 存储空间
      2. 保证效率，要一定硬件水平

2. 安装Redis

   1. 安装Redis：http://redis.io/download
   2. 安装后，拷贝 Redis 安装目录下的配置文件 redis.conf 到任意目录
      1. 建议保存到：`/etc/redis/redis.conf` 
      2. Windows系统 无需变动 

3. master中修改配置文件 redis.conf

   1. 打开 redis.conf 配置文件，示例
      1. 非Windows系统: `sudo vi /etc/redis/redis.conf`
      2. Windows系统：`C:\Intel\Redis\conf\redis.conf`
   2. Master端 redis.conf 注释掉 `bind 127.0.0.1`，Slave端才能远程连接到Master端的Redis数据库。
   3. `daemonize yes` 表示Redis默认 不守护进程运行
      1. 即 运行`redis-server /etc/redis/redis.conf`时，将显示 Redis 启动提示画面；
      2. `daemonize yes` 默认后台运行，不必重新启动新的终端窗口执行其他命令，看个人喜好和实际需要。

4. Slave端远程连接Master端

   1. Master端Windows 10 的IP地址为：`192.168.199.108`

   2. Master端 按配置文件`redis.conf`  启动redis服务: `redis-server`，示例：

      1. 非Windows系统：`sudo redis-server /etc/redis/redis/conf`

      2. Windows系统：`命令行窗口`(管理员) 执行

         `redis-server C:\Intel\Redis\conf\redis.conf` 读取默认配置即可。

   3. Master端, 命令行窗口, 启动redis客户端: `redis-cli`：

      1. 客户端redis测试
      
        `ping`
        `flushdb`
        `set key1 "hello"`
        `set key2 "word"`
      
   2. 再执行 `config set requirepass 123456` 设置默认用户auth登录密码为123456
      
      1. 问题 Redis (error) NOAUTH Authentication required.解决方法
      
      2. 出现认证问题，应该是设置了认证密码，输入密码既可以啦
   
4. slave端启动redis客户端 并连接master的redis服务: `redis-cli -h 192.168.199.108`，-h 要连接的redis服务端所在的主机IP
   
   1. 一个slave端:  python用户
   
      1. ```
            #192.168.199.108是 master主机IP
         python@ubuntu:~$ sudo redis-cli -h 192.168.199.108
            192.168.199. 108:6379> keys *
            1) "key2 "
            2) "key1"
            192.168.199.108:6379> get key1
            "hello"
         ```
         
      2. 一个slave端 : 用户Power@PowerMac

         1. ```
            #192.168.199.108是 master主机IP
            Power@PowerMac:~$ sudo redis-cli -h 192.168.199.108
            192.168.199. 108:6379> keys *
            1) "key2 "
            2) "key1"
            192.168.199.108:6379> get key1
            "hello"
   
   2. 注意: Slave端无需启动`redis-server`，Master端启动即可。
   
      1. 只要 Slave 端读取到了 Master 端的 Redis 数据库，表示连接成功，可实施分布式
   
5. Redis数据库桌面管理工具

   1. 推荐 Redis Desktop Manager.  支持 Windows、Mac OS X、Linux 等平台. https://redisdesktop.com/download
   2. 流程
      1. 创建Redis连接
      2. 任意命名本连接 : Name `Scrapy-Redis-Windows`
      3. Redis数据库服务端所在的主机IP : 192.168.199.108
      4. 端口号(默认6379) : 6379
      5. 查看当前数据库
         1. `key: value`
   3. 数据库`db0`, 右键 Reload #刷新, Flush database 清空

#### 7.1 源码自带项目说明

1. 用scrapy-redis的example 修改: github上 scrapy-redis 示例

   1. 将github下载的 example-project 目录移到指定地址：
      1. `git clone https://github.com/rolando/scrapy-redis.git`  # clone github scrapy-redis源码文件
      2. `mv scrapy-redis/example-project ~/scrapyredis-project`   # 官方的项目范例，改名为自己的项目
   2. clone 的 scrapy-redis 源码 有`example-project`项目
      1. 项目有3个爬虫: dmoz爬虫, myspider_redis爬虫，mycrawler_redis爬虫
   
2. DmozSpider爬虫 (class DmozSpider(CrawlSpider))  

   1. DmozSpider继承自 CrawlSpider, 说明Redis的 持续性 

      2. 第一次运行dmoz爬虫，然后Ctrl + C停掉，再运行dmoz爬虫，之前的爬取记录 保留在Redis里的
      3. 其实是`scrapy-redis` 版 `CrawlSpider` 类
         1. 设置Rule规则，callback不能写parse()方法
      
   2. 执行 `scrapy  crawls dmoz` 创建爬虫
   
   3. dmoz.py
   
      1. ```
         from scrapy.linkextractors import LinkExtractor
         from scrapy.spiders import CrawlSpider, Rule
         
         
         class DmozSpider(CrawlSpider):
             """Follow categories and extract links."""
             name = 'dmoz'
             allowed_domains = ['dmoztools.net/']
             start_urls = ['http://dmoztools.net/']
         
             rules = [
                 Rule(LinkExtractor(
                     restrict_css=('.top-cat', '.sub-cat', '.cat-item')
                 ), callback='parse_directory', follow=True),
             ]
         
             def parse_directory(self, response):
                 for div in response.css('.title-and-desc'):
                     yield {
                         'name': div.css('.site-title::text').extract_first(),
                         'description': div.css('.site-descr::text').extract_first().strip(),
                         'link': div.css('a::attr(href)').extract_first(),
                     }
         ```
   
      
   
3. myspider_redis 爬虫 (class MySpider(RedisSpider))  

   1. 爬虫继承了 `RedisSpider`,  支持分布式，采用 `basic spider`，要写parse函数。

      2. `redis_key` 代替 `start_urls`，scrapy-redis将 key 从 Redis 里pop出来，成为请求的url地址
      
   2. myspider_redis.py
   
      1. ```
         from scrapy_redis.spiders import RedisSpider
         
         
         class MySpider(RedisSpider):
             """Spider that reads urls from redis queue (myspider:start_urls)."""
             name = 'myspider_redis'
         
             #启动爬虫的命令. 注意redis-key的格式：
             redis_key = 'myspider:start_urls'
         
             #可选：等效于allowd_domains()，__init__方法按规定格式写，使用时只需要修改super()里的类名参数即可
             def __init__(self, *args, **kwargs):
                 # Dynamically define the allowed domains list.
                 domain = kwargs.pop('domain', '')
                 self.allowed_domains = filter(None, domain.split(','))
         
                 # 修改这里的类名为当前类名
                 super(MySpider, self).__init__(*args, **kwargs)
         
             def parse(self, response):
                 return {
                     'name': response.css('title::text').extract_first(),
                     'url': response.url,
                 }
         ```
   
      2. 注意
   
         1. RedisSpider类 不要 `allowd_domains`和`start_urls`
         2. scrapy-redis 在构造方法 `__init__()` 里动态定义 爬取域，也可直接写`allowd_domains`。
         3. 必须指定 redis_key，即启动爬虫的命令，参考格式：`redis_key = 'myspider:start_urls'`
         4. 在 Master端的 redis-cli 里, lpush 把start_urls 推入 Redis数据库，RedisSpider 从数据库 获取start_urls。
   
   3. 执行方式
   
      1. runspider 执行爬虫 py 文件（也可 分次执行多条），爬虫（们） 处于等待状态
   
         `scrapy runspider myspider_redis.py`
   
      2. Master端 的 redis-cli端输入 `lpush` 指令，格式
   
         `$redis > lpush myspider:start_urls http://dmoztools.net/`
   
      3. Slaver端 爬虫获取到请求，开始爬取
   
4. mycrawler_redis爬虫 (class MyCrawler(RedisCrawlSpider)), 

   1. 爬虫继承了RedisCrawlSpider

      1. 支持分布式
      2. 采用 crawlSpider，遵守Rule规则，callback不能写parse()方法
      3. redis_key 取代 start_urls，scrapy-redis将 key 从Redis里 pop 出来，成为请求的url地址

   2. mycrawler_redis.py

      1. ```
         from scrapy.spiders import Rule
         from scrapy.linkextractors import LinkExtractor
         
         from scrapy_redis.spiders import RedisCrawlSpider
         
         
         class MyCrawler(RedisCrawlSpider):
             """Spider that reads urls from redis queue (myspider:start_urls)."""
             name = 'mycrawler_redis'
             redis_key = 'mycrawler:start_urls'
         
             rules = (
                 # follow all links
                 Rule(LinkExtractor(), callback='parse_page', follow=True),
             )
         
             # __init__方法必须按规定写，使用时只需要修改super()里的类名参数即可
             def __init__(self, *args, **kwargs):
                 # Dynamically define the allowed domains list.
                 domain = kwargs.pop('domain', '')
                 self.allowed_domains = filter(None, domain.split(','))
         
                 #只修改这里的类名为当前类名
                 super(MyCrawler, self).__init__(*args, **kwargs)
         
             def parse_page(self, response):
                 return {
                     'name': response.css('title::text').extract_first(),
                     'url': response.url,
                 }
         ```

      2. 注意

         1. RedisCrawlSpider类不写 `allowd_domains` 和 `start_urls`
         2. scrapy-redis 在构造方法 `__init__()` 里动态定义 爬取域，也可直接写`allowd_domains`。
         3. redis_key，即启动爬虫的命令，参考格式：`redis_key = 'myspider:start_urls'`
         4. `start_urls` 在 Master端的 redis-cli 里 lpush 到 Redis数据库里，RedisSpider 在数据库 获取start_urls。

      3. 启动爬虫

         1. runspider方法执行爬虫 py 文件(也可 分次执行多条), 爬虫（们） 处于等待状态

            `scrapy runspider myspider_redis.py`

         2. Master端 的 redis-cli 中输入 `lpush` 指令，格式

            `$redis > lpush myspider:start_urls http://dmoztools.net/`

         3. Slaver端 爬虫获取到 请求，开始爬取

5. 小结

   1. 只用 Redis 的去重 保存功能， 选第一种；
   2. 写分布式，则根据情况，选择第二种、第三种；
   3. 通常 选 第三种方式编写 深度聚焦爬虫。

#### 7.2 有缘网分布式爬虫项目1

1. 有缘网分布式爬虫案例

   1. `git clone https://github.com/rolando/scrapy-redis.git`  #clone github scrapy-redis源码文件
   2. `mv scrapy-redis/example-project ~/scrapy-youyuan`  #项目范例，改名为自己的项目

2. 修改settings.py

   1. 修改后的配置文件中与scrapy-redis有关的部分, middleware、proxy等内容在此就省略

   2. ```
      # -*- coding: utf-8 -*-
      
      # 指定 scrapy-redis 调度器
      SCHEDULER = "scrapy_redis.scheduler.Scheduler"
      
      # 指定 scrapy-redis 去重
      DUPEFILTER_CLASS = 'scrapy_redis.dupefilter.RFPDupeFilter'
      
      # 指定排序 爬取地址 时使用的队列，
      # 默认的 按优先级排序(Scrapy默认)，由sorted set实现的一种非FIFO、LIFO方式。
      SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.SpiderPriorityQueue'
      # 可选的 按先进先出排序（FIFO）
      # SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.SpiderQueue'
      # 可选的 按后进先出排序（LIFO）
      # SCHEDULER_QUEUE_CLASS = 'scrapy_redis.queue.SpiderStack'
      
      # redis中保存scrapy-redis用到的各个队列，从而允许暂停和暂停后恢复，也就是不清理redis queues
      SCHEDULER_PERSIST = True
      
      #SpiderQueue或者SpiderStack是有效的参数时，指定爬虫关闭的最大 间隔时间
      # SCHEDULER_IDLE_BEFORE_CLOSE = 10
      
      # 通过配置 RedisPipeline 将 item 写入key为 spider.name:items 的redis的list中，供后面的分布式处理item. 这个由 scrapy-redis 实现，不需要我们写代码
      ITEM_PIPELINES = {
          'example.pipelines.ExamplePipeline': 300,
          'scrapy_redis.pipelines.RedisPipeline': 400
      }
      
      # 指定redis数据库的连接参数
      # REDIS_PASS是 自己加上的redis连接密码（默认不做）
      REDIS_HOST = '127.0.0.1'
      REDIS_PORT = 6379
      #REDIS_PASS = 'redisP@ssw0rd'
      
      # LOG等级
      LOG_LEVEL = 'DEBUG'
      
      #默认情况下,RFPDupeFilter只记录第一个重复请求。将DUPEFILTER_DEBUG设置为True会记录所有重复的请求。
      DUPEFILTER_DEBUG =True
      
      #设置默认请求头， 自己编写Downloader Middlewares,设置代理和UserAgent
      DEFAULT_REQUEST_HEADERS = {
          'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
          'Accept-Language': 'zh-CN,zh;q=0.8',
          'Connection': 'keep-alive',
          'Accept-Encoding': 'gzip, deflate, sdch'
      }
      ```
   
3. 查看pipeline.py

   1. ```
      # -*- coding: utf-8 -*-
      
      from datetime import datetime
      
      class ExamplePipeline(object):
          def process_item(self, item, spider):
              #utcnow() 是获取UTC时间
              item["crawled"] = datetime.utcnow()
              # 爬虫名
              item["spider"] = spider.name
              return item
      ```

      

4. 修改items.py

   1. 增加 要保存的youyuanItem项. 一个非常简单的版本

   2. ```
      # -*- coding: utf-8 -*-
      
      from scrapy.item import Item, Field
      
      class youyuanItem(Item):
          # 个人头像链接
          header_url = Field()
          # 用户名
          username = Field()
          # 内心独白
          monologue = Field()
          # 相册图片链接
          pic_urls = Field()
          # 年龄
          age = Field()
      
          # 网站来源 youyuan
          source = Field()
          # 个人主页源url
          source_url = Field()
      
          # 获取UTC时间
          crawled = Field()
          # 爬虫名
          spider = Field()
      ```

5. 编写 spiders/youyuan.py

   1. spiders目录下增加youyuan.py文件编写爬虫. 运行爬虫了。简单的版本

   2. ```
      # -*- coding:utf-8 -*-
      
      from scrapy.linkextractors import LinkExtractor
      from scrapy.spiders import CrawlSpider, Rule
      # 使用redis去重
      from scrapy.dupefilters import RFPDupeFilter
      
      from example.items import youyuanItem
      import re
      
      #
      class YouyuanSpider(CrawlSpider):
          name = 'youyuan'
          allowed_domains = ['youyuan.com']
          #有缘网的列表页
          start_urls = ['http://www.youyuan.com/find/beijing/mm18-25/advance-0-0-0-0-0-0-0/p1/']
      
          #搜索页面匹配规则，根据response提取链接
          list_page_lx = LinkExtractor(allow=(r'http://www.youyuan.com/find/.+'))
      
          #北京、18~25岁、女性 的 搜索页面匹配规则，根据response提取链接
          page_lx = LinkExtractor(allow =(r'http://www.youyuan.com/find/beijing/mm18-25/advance-0-0-0-0-0-0-0/p\d+/'))
      
          # 个人主页 匹配规则，根据response提取url
          profile_page_lx = LinkExtractor(allow=(r'http://www.youyuan.com/\d+-profile/'))
      	
      	#用提取的url构造Rule请求, url是:list_page_lx,page_lx,profile_page_lx
          rules = (
              # 匹配find页面，跟进链接，跳板
              Rule(list_page_lx, follow=True),
      
              # 匹配列表页成功，跟进链接，跳板
              Rule(page_lx, follow=True),
      
              # 匹配个人主页的链接，形成request保存到redis中等待调度，一旦有响应则调用parse_profile_page()回调函数处理，不做继续跟进
              Rule(profile_page_lx, callback='parse_profile_page', follow=False),
          )
      
          # 处理个人主页信息，得到我们要的数据
          def parse_profile_page(self, response):
              item = youyuanItem()
              item['header_url'] = self.get_header_url(response)
              item['username'] = self.get_username(response)
              item['monologue'] = self.get_monologue(response)
              item['pic_urls'] = self.get_pic_urls(response)
              item['age'] = self.get_age(response)
              item['source'] = 'youyuan'
              item['source_url'] = response.url
      
              #print "Processed profile %s" % response.url
              yield item
      
      
          # 提取头像地址
          def get_header_url(self, response):
              header = response.xpath('//dl[@class=\'personal_cen\']/dt/img/@src').extract()
              if len(header) > 0:
                  header_url = header[0]
              else:
                  header_url = ""
              return header_url.strip()
      
          # 提取用户名
          def get_username(self, response):
              usernames = response.xpath("//dl[@class=\'personal_cen\']/dd/div/strong/text()").extract()
              if len(usernames) > 0:
                  username = usernames[0]
              else:
                  username = "NULL"
              return username.strip()
      
          # 提取内心独白
          def get_monologue(self, response):
              monologues = response.xpath("//ul[@class=\'requre\']/li/p/text()").extract()
              if len(monologues) > 0:
                  monologue = monologues[0]
              else:
                  monologue = "NULL"
              return monologue.strip()
      
          # 提取相册图片地址
          def get_pic_urls(self, response):
              pic_urls = []
              data_url_full = response.xpath('//li[@class=\'smallPhoto\']/@data_url_full').extract()
              if len(data_url_full) <= 1:
                  pic_urls.append("");
              else:
                  for pic_url in data_url_full:
                      pic_urls.append(pic_url)
              if len(pic_urls) <= 1:
                  return "NULL"
              # 每个url用|分隔
              return '|'.join(pic_urls)
      
          # 提取年龄
          def get_age(self, response):
              age_urls = response.xpath("//dl[@class=\'personal_cen\']/dd/p[@class=\'local\']/text()").extract()
              if len(age_urls) > 0:
                  age = age_urls[0]
              else:
                  age = "0"
              age_words = re.split(' ', age)
              if len(age_words) <= 2:
                  return "0"
              age = age_words[2][:-1]
              # 从age字符串开始匹配数字，失败返回None
              if re.compile(r'[0-9]').match(age):
                  return age
              return "0"
      ```
      
      

6. 运行程序：

   1. Master端运行 Redis服务 ： `redis-server`
   2. Slave端直接运行爬虫： `scrapy crawl youyuan`
   3. 多个Slave端运行爬虫顺序没有限制

7. 将项目修改成 RedisCrawlSpider 类的分布式爬虫，并尝试在多个Slave端运行。

##### 小结

1. 创建项目 scrapy startproject example
2. 创建爬虫 scrapy genspider -t crawl youyuan youyuan.com

#### 7.3 有缘网分布式爬虫项目2

1. 修改 spiders/youyuan.py

   1. spiders目录下增加youyuan.py文件, 编写爬虫

   2. ```
      # -*- coding:utf-8 -*-
      
      from scrapy.linkextractors import LinkExtractor
      #from scrapy.spiders import CrawlSpider, Rule
      
      # 1. 导入RedisCrawlSpider类，不使用CrawlSpider
      from scrapy_redis.spiders import RedisCrawlSpider
      from scrapy.spiders import Rule
      
      
      from scrapy.dupefilters import RFPDupeFilter
      from example.items import youyuanItem
      import re
      
      # 2. 修改父类为 RedisCrawlSpider
      # class YouyuanSpider(CrawlSpider):
      class YouyuanSpider(RedisCrawlSpider):
          name = 'youyuan'
      
      # 3. 取消 allowed_domains() 和 start_urls
      ##### allowed_domains = ['youyuan.com']
      ##### start_urls = ['http://www.youyuan.com/find/beijing/mm18-25/advance-0-0-0-0-0-0-0/p1/']
      
      # 4. 增加redis-key
          redis_key = 'youyuan:start_urls'
      	
      	#列表页
          list_page_lx = LinkExtractor(allow=(r'http://www.youyuan.com/find/.+'))
          #搜索页
          page_lx = LinkExtractor(allow =(r'http://www.youyuan.com/find/beijing/mm18-25/advance-0-0-0-0-0-0-0/p\d+/'))
          #个人页
          profile_page_lx = LinkExtractor(allow=(r'http://www.youyuan.com/\d+-profile/'))
      
          rules = (
              Rule(list_page_lx, follow=True),
              Rule(page_lx, follow=True),
              Rule(profile_page_lx, callback='parse_profile_page', follow=False),
          )
      
      # 5. 增加__init__()方法，动态获取allowed_domains()
          def __init__(self, *args, **kwargs):
              domain = kwargs.pop('domain', '')
              self.allowed_domains = filter(None, domain.split(','))
              super(youyuanSpider, self).__init__(*args, **kwargs)
      
          # 处理个人主页信息，得到我们要的数据
          def parse_profile_page(self, response):
              item = youyuanItem()
              item['header_url'] = self.get_header_url(response)
              item['username'] = self.get_username(response)
              item['monologue'] = self.get_monologue(response)
              item['pic_urls'] = self.get_pic_urls(response)
              item['age'] = self.get_age(response)
              item['source'] = 'youyuan'
              item['source_url'] = response.url
      
              yield item
      
          # 提取头像地址
          def get_header_url(self, response):
              header = response.xpath('//dl[@class=\'personal_cen\']/dt/img/@src').extract()
              if len(header) > 0:
                  header_url = header[0]
              else:
                  header_url = ""
              return header_url.strip()
      
          # 提取用户名
          def get_username(self, response):
              usernames = response.xpath("//dl[@class=\'personal_cen\']/dd/div/strong/text()").extract()
              if len(usernames) > 0:
                  username = usernames[0]
              else:
                  username = "NULL"
              return username.strip()
      
          # 提取内心独白
          def get_monologue(self, response):
              monologues = response.xpath("//ul[@class=\'requre\']/li/p/text()").extract()
              if len(monologues) > 0:
                  monologue = monologues[0]
              else:
                  monologue = "NULL"
              return monologue.strip()
      
          # 提取相册图片地址
          def get_pic_urls(self, response):
              pic_urls = []
              data_url_full = response.xpath('//li[@class=\'smallPhoto\']/@data_url_full').extract()
              if len(data_url_full) <= 1:
                  pic_urls.append("");
              else:
                  for pic_url in data_url_full:
                      pic_urls.append(pic_url)
              if len(pic_urls) <= 1:
                  return "NULL"
              return '|'.join(pic_urls)
      
          # 提取年龄
          def get_age(self, response):
              age_urls = response.xpath("//dl[@class=\'personal_cen\']/dd/p[@class=\'local\']/text()").extract()
              if len(age_urls) > 0:
                  age = age_urls[0]
              else:
                  age = "0"
              age_words = re.split(' ', age)
              if len(age_words) <= 2:
                  return "0"
              age = age_words[2][:-1]
              if re.compile(r'[0-9]').match(age):
                  return age
              return "0"
      ```

2. 分布式爬虫执行方式

   1. 在Master端启动redis服务:   `redis-server`

   2. 在Slave端分别启动爬虫，不分先后  `scrapy runspider youyuan.py`

   3. 在Master端的redis客户端: redis-cli里push一个start_urls  

      `lpush youyuan:start_url http://www.youyuan.com/find/beijing/mm18-25/advance-0-0-0-0-0-0/p1`

   4. Slave端分别启动爬虫，查看redis数据库数据

#### 7.4 处理Redis里的数据

1. 数据  保存在redis的 youyuan:items 键

   1. settings.py里 没有定制 `ITEM_PIPELINES`，而是用`RedisPipeline`
   2. `process_items.py`文件, scrapy-redis的example提供的, 从 redis 读取 item 处理的模版. 在scrapy-youyuan目录下
   3. 任务:  youyuan:items中保存的数据读出来写进MongoDB或者MySQL
      1. 自己写一个`process_youyuan_profile.py`文件，
      2. 保持后台运行 不停地将爬回来的数据写入库
   
2. 存入MongoDB

   1. 启动MongoDB服务:   `sudo mongod`

   2. 编辑 程序  process_youyuan_mongodb.py 

      1. 项目根目录下 创建  ` process_youyuan_mongodb.py`

      2. ```
         `# process_youyuan_mongodb.py
         
         `# -*- coding: utf-8 -*-
         
         import json
         import redis  #pycharm中加载 redis包, 即连接数据库的接口
         import pymongo
         
         def main():
         
             # 创建redis数据库连接, 指定Redis数据库信息
             rediscli = redis.StrictRedis(host='192.168.199.108', port=6379, db=0, password='123456')
             # 创建mongodb数据库连接,指定MongoDB数据库信息.默认是本地端口,或指定其他服务器端口
             mongocli = pymongo.MongoClient(host='localhost', port=27017)
         
             # 创建数据库名, 类似字典引用
             db = mongocli['youyuan']
             # 创建数据库的集合
             sheet = db['beijing_18_25']
         
             while True:
                 # FIFO模式为 blpop，LIFO模式为 brpop，获取键值
                 source, data = rediscli.blpop(["youyuan:items"])
         
                 item = json.loads(data)
                 sheet.insert(item)
         
                 try:
                     print ("Processing: %(name)s <%(link)s>" % item)
                 except KeyError:
                     print (u"Error procesing: %r" % item)
         
         if __name__ == '__main__':
             main()
         ```

   3. 执行程序  `python  process_youyuan_mongodb.py`

3. 存入 MySQL

   1. 启动mysql服务：`mysql.server start`（更平台不一样）

   2. 启动mysql客户端: 登录到root用户：`mysql -uroot -p`

   3. 创建数据库`youyuan`:`create database youyuan;`

   4. 切换到指定数据库：`use youyuan`

   5. 创建表`beijing_18_25`以及所有字段的 列名 和数据类型

   6. 根目录下创建 process_youyuan_mysql.py

      1. ```
         #process_youyuan_mysql.py
         
         # -*- coding: utf-8 -*-
         
         import json
         import redis
         #MySQLdb只支持到python3.4
         import MySQLdb
         
         def main():
             #创建redis连接, 指定redis数据库信息
             rediscli = redis.StrictRedis(host='192.168.199.108', port = 6379, db = 0)
             #创建mongodb连接, 指定mysql数据库
             mysqlcli = MySQLdb.connect(host='127.0.0.1', user='power', passwd='xxxxxxx', db = 'youyuan', port=3306, use_unicode=True)
         
             while True:
                 # FIFO模式为 blpop，LIFO模式为 brpop，获取键值
                 source, data = rediscli.blpop(["youyuan:items"])
                 item = json.loads(data)
         
                 try:
                     # 使用cursor()方法获取操作游标
                     cur = mysqlcli.cursor()
                     # 使用execute方法执行SQL INSERT语句
                     cur.execute("INSERT INTO beijing_18_25 (username, crawled, age, spider, header_url, source, pic_urls, monologue, source_url) VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s )", [item['username'], item['crawled'], item['age'], item['spider'], item['header_url'], item['source'], item['pic_urls'], item['monologue'], item['source_url']])
                     # 提交sql事务
                     mysqlcli.commit()
                     #关闭本次操作
                     cur.close()
                     print "inserted %s" % item['source_url']
                 except MySQLdb.Error,e:
                     print "Mysql Error %d: %s" % (e.args[0], e.args[1])
         
         if __name__ == '__main__':
             main()
         ```
         
         
   
   7. 执行程序 `python process_youyuan_mysql.py`

#### 7.5 新浪网分类资讯爬虫 - 单机版爬虫

1. 改写已有的Scrapy爬虫项目为scrapy-redis分布式爬虫: 要求：将所有对应的大类的 标题和urls、小类的 标题和urls、子链接url、文章名以及文章内容，存入Redis数据库

2. 原Scrapy爬虫项目源码 (能 跑通 )

   1. 创建过程:

      1. `scrapy startproject Sina`  #Sina 首字母大写

      2. `cd Sina`  #进入项目根目录

      3. `scrapy genspider sina sina.com.cn`  #创建爬虫 sina, 对应 allowed_domains 是 `sina.com.cn`

      4. 文件结构

         ```
         .  #工作目录
         └─── Sina  #scrapy startproject mySpider创建的项目, 根目录
             ├─── Sina  #项目的Python模块
             │	├─── __init__.py
             │	├─── items.py   
             │	├─── pipelines.py  
             │	├─── settings.py   
             │	└─── spiders  
             │		├─── __init__.py  #创建后就有的
             │       └─── sina.py #scrapy genspider sina sina.com.cn创建的
             ├─── scrapy.cfg  #配置文件
             └─── main.py #项目Sina根目录下
         ```

      5. 编辑完各个模块, 就可以执行 `scrapy crawl sina`

   2. items.py

      ```
      # -*- coding: utf-8 -*-
      
      import scrapy
      import sys
      
      #定义要爬取的数据
      class SinaItem(scrapy.Item):
          # 大类的标题 和 url
          parentTitle = scrapy.Field()
          parentUrls = scrapy.Field()
      
          # 小类的标题 和 子url
          subTitle = scrapy.Field()
          subUrls = scrapy.Field()
      
          # 小类目录存储路径
          subFilename = scrapy.Field()
      
          # 小类下的子链接
          sonUrls = scrapy.Field()
      
          # 文章标题和内容
          head = scrapy.Field()
          content = scrapy.Field()

   4. pipelines.py

      ```
      # -*- coding: utf-8 -*-
      
      from scrapy import signals
      import sys
      
      #创建类,处理scrapy engine传递过来的item
      class SinaPipeline(object):
          def process_item(self, item, spider):
              sonUrls = item['sonUrls']
      
              #指定文件名为子链接url中间部分，并将 / 替换为 _，保存为 .txt格式
              filename = sonUrls[7:-6].replace('/','_')
              filename += ".txt"
      
              fp = open(item['subFilename']+'/'+filename, 'w')
           fp.write(item['content'])
              fp.close()
      
              return item

   5. settings.py

      ```
      # -*- coding: utf-8 -*-
      
      BOT_NAME = 'Sina'
      
      SPIDER_MODULES = ['Sina.spiders']
      NEWSPIDER_MODULE = 'Sina.spiders'
      
      #设置ITEM_PIPELINES, 才能启用 pipeline.py
      ITEM_PIPELINES = {
          'Sina.pipelines.SinaPipeline': 300,
      }
      
      LOG_LEVEL = 'DEBUG'
      ```

      1. settings.py 中设置 ITEM_PIPELINES, 才能启动 pipiline.py 模块

   5. spiders/sina.py

      ```
      # -*- coding: utf-8 -*-
      
      from Sina.items import SinaItem
      import scrapy
      import os
      
      import sys
      reload(sys)
      sys.setdefaultencoding("utf-8")
      
      
      class SinaSpider(scrapy.Spider):
          name= "sina"
          allowed_domains= ["sina.com.cn"]
          
          #起始爬虫url, 默认由scrapy-engine传给start_requests(),响应response传给parse处理
          start_urls= [
             "http://news.sina.com.cn/guide/"
          ]
      
      	#解析response,用xpath()获取数据保存到items
          def parse(self, response):
              items= []
              #所有大类的url 和 标题
              parentUrls = response.xpath('//div[@id=\"tab01\"]/div/h3/a/@href').extract()
              parentTitle = response.xpath("//div[@id=\"tab01\"]/div/h3/a/text()").extract()
      
              # 所有小类的url 和 标题
              subUrls  = response.xpath('//div[@id=\"tab01\"]/div/ul/li/a/@href').extract()
              subTitle = response.xpath('//div[@id=\"tab01\"]/div/ul/li/a/text()').extract()
      
              #遍历所有大类
              for i in range(0, len(parentTitle)):
                  # 指定大类目录的路径和目录名
                  parentFilename = "./Data/" + parentTitle[i]
      
                  #如果目录不存在，则创建目录
                  if(not os.path.exists(parentFilename)):
                      os.makedirs(parentFilename)
      
                  #遍历所有小类
                  for j in range(0, len(subUrls)):
                      item = SinaItem()
      
                      # 保存大类的title和urls
                      item['parentTitle'] = parentTitle[i]
                      item['parentUrls'] = parentUrls[i]
      
                      # 检查小类的url是否以同类别大类url开头，如果是返回True (sports.sina.com.cn 和 sports.sina.com.cn/nba)
                      if_belong = subUrls[j].startswith(item['parentUrls'])
      
                      # 如果属于本大类，将存储目录放在本大类目录下
                      if(if_belong):
                          subFilename =parentFilename + '/'+ subTitle[j]
                          # 如果目录不存在，则创建目录
                          if(not os.path.exists(subFilename)):
                              os.makedirs(subFilename)
      
                          # 存储第j个小类的url、title和filename字段数据
                          item['subUrls'] = subUrls[j]
                          item['subTitle'] =subTitle[j]
                          item['subFilename'] = subFilename
      					#一个item是一个小类
                          items.append(item)
      
              #发送每一个小类url的Request请求，得到Response连同包含meta数据 一同交给回调函数 second_parse 方法处理
              for item in items:
                  yield scrapy.Request( url = item['subUrls'], meta={'meta_1': item}, callback=self.second_parse)
      
          #对每一个小类的url的response进行解析
          def second_parse(self, response):
              # 提取每次Response的meta数据
              meta_1= response.meta['meta_1']
      
              # 取出当前的一个小类的所有子链接
              sonUrls = response.xpath('//a/@href').extract()
      
              items= []
              for i in range(0, len(sonUrls)):
                  # 检查每个链接是否以大类url开头、以.shtml结尾，如果是返回True
                  if_belong = sonUrls[i].endswith('.shtml') and sonUrls[i].startswith(meta_1['parentUrls'])
      
                  # 如果属于本大类，获取字段值放在同一个item下便于传输
                  if(if_belong):
                      item = SinaItem()
                      item['parentTitle'] =meta_1['parentTitle']
                      item['parentUrls'] =meta_1['parentUrls']
                      item['subUrls'] = meta_1['subUrls']
                   	item['subTitle'] = meta_1['subTitle']
                      item['subFilename'] = meta_1['subFilename']
                   	item['sonUrls'] = sonUrls[i]
                      items.append(item)
      
              #发送每个小类的一个子链接url的Request请求，得到Response后连同包含meta数据 一同交给scrapy engine, scrapy engine转给回调函数 detail_parse 方法处理
              for item in items:
                      yield scrapy.Request(url=item['sonUrls'], meta={'meta_2':item}, callback = self.detail_parse)
      
          # 数据解析方法，获取文章标题和内容
          def detail_parse(self, response):
              item = response.meta['meta_2']
              content = ""
              head = response.xpath('//h1[@id=\"main_title\"]/text()')
              content_list = response.xpath('//div[@id=\"artibody\"]/p/text()').extract()
      
              # 将p标签里的文本内容合并到一起
              for content_one in content_list:
                  content += content_one
      
              item['head']= head
              item['content']= content
      
              yield item

   7. 执行
   
      1. `scrapy crawl sina`

#### 7.6 新浪网分类资讯 scrapy-redis分布式爬虫

1. 任务: 将已有的 新浪网分类资讯 Scrapy 单机爬虫，修改为基于 RedisSpider 类的 scrapy-redis 分布式爬虫

   2. 注意:
      1. items 数据要存于 Redis 数据库中， 由 scrapy-redis 自动实现
      2. 不用编写 pipelines.py 代码, 除非做额外处理(如直接存入本地数据库等)
   
2. 项目 模块

   1. items.py文件

      1. ```
         # items.py
         
         # -*- coding: utf-8 -*-
         
         import scrapy
         
         import sys
         
         #创建要保存的数据类(数据结构)
         class SinaItem(scrapy.Item):
             # 一个大类的标题 和 url
             parentTitle = scrapy.Field()
             parentUrls = scrapy.Field()
         
             # 一个小类的标题 和 url
             subTitle = scrapy.Field()
             subUrls = scrapy.Field()
         
             # 一个小类的文件名
             # subFilename = scrapy.Field()
         
             # 一个小类下的子链接
             sonUrls = scrapy.Field()
         
             # 文章标题和内容
             head = scrapy.Field()
             content = scrapy.Field()
         ```
         
         1. 与 单机 items.py 内容一致
   
   2. settings.py文件
   
      1. ```
         # settings.py
         
         SPIDER_MODULES = ['Sina.spiders']  #单机也有
         NEWSPIDER_MODULE = 'Sina.spiders'  #单机也有
         
         #指定UA
         USER_AGENT = 'scrapy-redis (+https://github.com/rolando/scrapy-redis)'
         
         #去重过滤
         DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
         #调度器
         SCHEDULER = "scrapy_redis.scheduler.Scheduler"
         SCHEDULER_PERSIST = True
         SCHEDULER_QUEUE_CLASS = "scrapy_redis.queue.SpiderPriorityQueue"
         #SCHEDULER_QUEUE_CLASS = "scrapy_redis.queue.SpiderQueue"
         #SCHEDULER_QUEUE_CLASS = "scrapy_redis.queue.SpiderStack"
         
         ITEM_PIPELINES = {
         #    'Sina.pipelines.SinaPipeline': 300,   #单机有
             'scrapy_redis.pipelines.RedisPipeline': 400,  #单机没有
         }
         
         LOG_LEVEL = 'DEBUG'  #单机也有
         
         # Introduce an artifical delay to make use of parallelism. to speed up the
         # crawl.
         DOWNLOAD_DELAY = 1
         
         #master主机的IP,Port
         REDIS_HOST = "192.168.13.26"
         REDIS_PORT = 6379
         ```
         
         1. redis-scrapy的 settings.py 设置 很多,  与单机版 很多不同
         
      2. 单机版 settings.py 设置很少
      
         1. ```
            # -*- coding: utf-8 -*-
            
            BOT_NAME = 'Sina'
            
            SPIDER_MODULES = ['Sina.spiders']
            NEWSPIDER_MODULE = 'Sina.spiders'
            
            ITEM_PIPELINES = {
                'Sina.pipelines.SinaPipeline': 300,
            }
            
            LOG_LEVEL = 'DEBUG'
            ```
      
            
      
   3. spiders/sina.py,  分布式爬虫
   
      1. ```
         # sina.py
         
         # -*- coding: utf-8 -*-
         
         from Sina.items import SinaItem
         from scrapy_redis.spiders import RedisSpider
         #from scrapy.spiders import Spider
         import scrapy
         
         import sys
         
         #class SinaSpider(Spider):  #单机版爬虫
         
         class SinaSpider(RedisSpider):  #分布式爬虫
             name= "sina"
             
             #启动redis_key
             redis_key = "sinaspider:start_urls"
             #allowed_domains= ["sina.com.cn"]
             #start_urls= [
             #   "http://news.sina.com.cn/guide/"
             #]#起始urls列表
         
             def __init__(self, *args, **kwargs):   #单机版没有__init__
                 domain = kwargs.pop('domain', '')
                 self.allowed_domains = filter(None, domain.split(','))
                 #分布式爬虫初始化父类
                 super(SinaSpider, self).__init__(*args, **kwargs)
         
         
             def parse(self, response):
                 items= []
         
                 # 所有大类的url 和 标题
                 parentUrls = response.xpath('//div[@id=\"tab01\"]/div/h3/a/@href').extract()
                 parentTitle = response.xpath("//div[@id=\"tab01\"]/div/h3/a/text()").extract()
         
                 # 所有小类的ur 和 标题
                 subUrls  = response.xpath('//div[@id=\"tab01\"]/div/ul/li/a/@href').extract()
                 subTitle = response.xpath('//div[@id=\"tab01\"]/div/ul/li/a/text()').extract()
         
                 #遍历所有大类
                 for i in range(0, len(parentTitle)):
         
                     # 指定大类的路径和目录名
                     #parentFilename = "./Data/" + parentTitle[i]
         
                     #如果目录不存在，则创建目录
                     #if(not os.path.exists(parentFilename)):
                     #    os.makedirs(parentFilename)
         
                     #遍历所有小类
                     for j in range(0, len(subUrls)):
                         item = SinaItem()
         
                         # 保存一个大类的title和urls
                         item['parentTitle'] = parentTitle[i]
                         item['parentUrls'] = parentUrls[i]
         
                         #检查当前小类是否与当前一个大类的小类, 用url判断，如果是返回True (如大类:sports.sina.com.cn 和 小类: sports.sina.com.cn/nba)
                         if_belong = subUrls[j].startswith(item['parentUrls'])
         
                         #如果当前小类是当前大类的一个小类
                         if(if_belong):
                             #subFilename =parentFilename + '/'+ subTitle[j]
         
                             # 如果目录不存在，则创建目录
                             #if(not os.path.exists(subFilename)):
                             #    os.makedirs(subFilename)
         
                             # 存储 小类url、title和filename字段数据
                             item['subUrls'] = subUrls[j]
                             item['subTitle'] =subTitle[j]
                             #item['subFilename'] = subFilename
         					
                             items.append(item)
         
                 #向scrapy engine发送每个小类url的Request请求，得到Response连同包含meta数据 一同交给回调函数 second_parse 方法处理
                 for item in items:
                     yield scrapy.Request( url = item['subUrls'], meta={'meta_1': item}, callback=self.second_parse)
         
             #解析每个小类的url的response
             def second_parse(self, response):
                 # 提取每次Response的meta数据, 即response的 item 赋值给 meta_1
                 meta_1= response.meta['meta_1']
         
                 # 取出小类里所有子链接
                 sonUrls = response.xpath('//a/@href').extract()
         
                 items= []
                 #遍历每个小类的提取的所有url
                 for i in range(0, len(sonUrls)):
                     # 检查每个链接是否以大类url开头、以.shtml结尾，如果是返回True
                     if_belong = sonUrls[i].endswith('.shtml') and sonUrls[i].startswith(meta_1['parentUrls'])
         
                     # 如果当前小类属于当前大类，获取字段值放在同一个item下便于传输
                     if(if_belong):
                         item = SinaItem()
                         item['parentTitle'] =meta_1['parentTitle']
                         item['parentUrls'] =meta_1['parentUrls']
                         item['subUrls'] =meta_1['subUrls']
                         item['subTitle'] =meta_1['subTitle']
                         #item['subFilename'] = meta_1['subFilename']
                         item['sonUrls'] = sonUrls[i]
                         items.append(item)
         
                 #发送每个小类下子链接url的Request请求，得到 Response 后连同 meta 数据 交给回调函数 detail_parse 方法处理
                 for item in items:
                         yield scrapy.Request(url=item['sonUrls'], meta={'meta_2':item}, callback = self.detail_parse)
         
             # 数据解析方法，获取文章标题和内容
             def detail_parse(self, response):
                 item = response.meta['meta_2']
                 content = ""
                 head = response.xpath('//h1[@id=\"main_title\"]/text()').extract()
                 content_list = response.xpath('//div[@id=\"artibody\"]/p/text()').extract()
         
                 # 将p标签里的文本内容合并到一起
                 for content_one in content_list:
                     content += content_one
         
                 item['head']= head[0] if len(head) > 0 else "NULL"
         
                 item['content']= content
         
                 yield item
         ```
         
         1. meta 参数在各个回调函数中传递 item 值
         
            1. 单机版 与 分布式版本 都是在 yield scrapy.Request(url, meta, callback)中用meta 向回调函数传递参数
         
         2. 单机版 sina.py 与 分布式版 sina.py 程序基本一致.
         
            1. 分布式的 sina.py 中多了 `__init__`函数
            2. parse, second_parse, detail_arse 函数都是一样的
         
         3. 分布式 爬虫 模块
         
            1.  items.py
            2.  settings.py
            3.  sina.py #爬虫模块
         
            
   
   4. 执行： (redis-scrapy与单机版爬虫执行方式不同)
   
      1. slave端(各个slave端都执行下面的命令)
         `scrapy runspider sina.py`
   
      2. Master端 redis-cli 命令行中(执行下面的命令)
   
         `lpush sinaspider:start_urls http://news.sina.com.cn/guide/`

##### 7.6 小结

1. redis-scrapy 与 scrapy单机版区别

   | redis-scrapy                                                 | scrapy 单机版                                              |
   | ------------------------------------------------------------ | ---------------------------------------------------------- |
   | 创建方法: `scrapy startproject 项目名`                       | 创建方法: `scrapy startproject 项目名`                     |
   | items.py                                                     | items.py , 两者一致                                        |
   | items 数据直接存储在 Redis 数据库中，这个功能由 scrapy-redis 自行实现. 不编写pipelines | pipelines.py , 设置 item 处理, 存储                        |
   | settings.py 设置复杂                                         | settings.py设置简单, 设置 ITEM_PIPELINE, 启动 pipelines.py |
   | sina.py分布式 爬虫类 有`__init__`方法, 修改单机爬虫得到的    | sina.py 爬虫类 没有 `__init__`方法, 其他一致               |
   | 分两步: 先在 Slave端启动爬虫, 再Master端 输入start_urls<br>`scrapy runspider sina.py` <br>`lpush sinaspider:start_urls http://news.sina.com.cn/guide/` | 执行: `scrapy crawl sina`                                  |
   | 总结: redis-srcapy 是在 单机 scrapy 基础上改进得到的         |                                                            |

   

#### 7.7 IT桔子分布式爬虫

  1. IT桔子: IT互联网行业的结构化的 公司数据库 和 商业信息 服务提供商，于2013年5月21日上线
     	2. 项目采集地址: `[http://www.itjuzi.com/company](http://www.itjuzi.com/company/)`
     
  2. 要求: 采集页面下所有创业公司的 公司信息，包括 不限于 items.py 中定义的各种信息


​    

#### 7.8 IT桔子分布式爬虫

1. 创建过程

   1.  scrapy startproject Itjuzi  #创建Itjuzi项目
   2.  cd Itjuzi  #进入项目根目录
   3.  scrapy genspider -t crawl itjuzi www.itjuzi.com  #创建爬虫

2. 项目中个 py 模块编写

   1. 对 scrapy 单机爬虫 修改成 scrapy-redis 分布式爬虫

    2. items.py

       ```
       #自动生成的
       # Define here the models for your scraped items
       #
       # See documentation in:
       # https://docs.scrapy.org/en/latest/topics/items.html
       
       import scrapy
       
       
       class ItjuziItem(scrapy.Item):
           # define the fields for your item here like:
           # name = scrapy.Field()
           pass
       
       
       -------------
       #修改后的
       # items.py
       
       # -*- coding: utf-8 -*-
       import scrapy
       
       class CompanyItem(scrapy.Item):
       
           # 公司id (url数字部分)
           info_id = scrapy.Field()
           # 公司名称
           company_name = scrapy.Field()
           # 公司口号
           slogan = scrapy.Field()
           # 分类
           scope = scrapy.Field()
           # 子分类
           sub_scope = scrapy.Field()
       
           # 所在城市
           city = scrapy.Field()
           # 所在区域
           area = scrapy.Field()
           # 公司主页
           home_page = scrapy.Field()
           # 公司标签
           tags = scrapy.Field()
       
           # 公司简介
           company_intro = scrapy.Field()
           # 公司全称：
           company_full_name = scrapy.Field()
           # 成立时间：
           found_time = scrapy.Field()
           # 公司规模：
           company_size = scrapy.Field()
           # 运营状态
           company_status = scrapy.Field()
       
           # 投资情况列表：包含获投时间、融资阶段、融资金额、投资公司
           tz_info = scrapy.Field()
           # 团队信息列表：包含成员姓名、成员职称、成员介绍
           tm_info = scrapy.Field()
           # 产品信息列表：包含产品名称、产品类型、产品介绍
           pdt_info = scrapy.Field()
       ```
   
       
   
   3. settings.py

      ```
      #修改前
      BOT_NAME = 'Itjuzi'
      
      #指定爬虫
      SPIDER_MODULES = ['Itjuzi.spiders']
      NEWSPIDER_MODULE = 'Itjuzi.spiders'
      #单机版本需要添加 ITEM-PIPELINE, 否则 无法启动 pipelines.py
      ITEM_PIPELINE = {
          "Itjuzi.pipelines.ItjuziPipeline": 300,
      }
      
      ------------
      
      
      #修改后
      # -*- coding: utf-8 -*-
      
      BOT_NAME = 'itjuzi'
      
      SPIDER_MODULES = ['itjuzi.spiders']
      NEWSPIDER_MODULE = 'itjuzi.spiders'
      
      # Enables scheduling storing requests queue in redis.
      SCHEDULER = "scrapy_redis.scheduler.Scheduler"
      
      # Ensure all spiders share same duplicates filter through redis.
      DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
      
      # REDIS_START_URLS_AS_SET = True
      
      COOKIES_ENABLED = False
      
      DOWNLOAD_DELAY = 1.5
      
      # 支持随机下载延迟
      RANDOMIZE_DOWNLOAD_DELAY = True
      
      # Obey robots.txt rules
      ROBOTSTXT_OBEY = False
      
      ITEM_PIPELINES = {
          'scrapy_redis.pipelines.RedisPipeline': 300
      }
      
      DOWNLOADER_MIDDLEWARES = {
          # 该中间件收集失败的页面，在爬虫完成后重新调度。（失败情况可能临时的问题，例如连接超时或者HTTP 500错误导致失败的页面）
         'scrapy.downloadermiddlewares.retry.RetryMiddleware': 80,
      
          # 该中间件提供了对request设置HTTP代理的支持。您可以在 Request 对象中设置 proxy 元数据来开启代理。
          'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware': 100,
      
          'Itjuzi.middlewares.RotateUserAgentMiddleware': 200,
      }
      
      REDIS_HOST = "192.168.199.108"  #redis在的主机的IP
      REDIS_PORT = 6379
      ```
   
      
   
      
   
   4. middlewares.py
   
      ```
      #修改后, 添加内容
      # -*- coding: utf-8 -*-
      
      from scrapy.downloadermiddlewares.useragent import UserAgentMiddleware
      import random
      
      #指定User-Agent 下载中间件
      class RotateUserAgentMiddleware(UserAgentMiddleware):
          def __init__(self, user_agent=''):
              self.user_agent = user_agent
      
          def process_request(self, request, spider):
              # 这句话用于随机选择user-agent
              ua = random.choice(self.user_agent_list)
              request.headers.setdefault('User-Agent', ua)
      
          user_agent_list = [
              "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/22.0.1207.1 Safari/537.1",
              "Mozilla/5.0 (X11; CrOS i686 2268.111.0) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.57 Safari/536.11",
              "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.6 (KHTML, like Gecko) Chrome/20.0.1092.0 Safari/536.6",
              "Mozilla/5.0 (Windows NT 6.2) AppleWebKit/536.6 (KHTML, like Gecko) Chrome/20.0.1090.0 Safari/536.6",
              "Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/19.77.34.5 Safari/537.1",
              "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/536.5 (KHTML, like Gecko) Chrome/19.0.1084.9 Safari/536.5",
              "Mozilla/5.0 (Windows NT 6.0) AppleWebKit/536.5 (KHTML, like Gecko) Chrome/19.0.1084.36 Safari/536.5",
              "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1063.0 Safari/536.3",
              "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1063.0 Safari/536.3",
              "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_0) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1063.0 Safari/536.3",
              "Mozilla/5.0 (Windows NT 6.2) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1062.0 Safari/536.3",
              "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1062.0 Safari/536.3",
              "Mozilla/5.0 (Windows NT 6.2) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1061.1 Safari/536.3",
              "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1061.1 Safari/536.3",
              "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1061.1 Safari/536.3",
              "Mozilla/5.0 (Windows NT 6.2) AppleWebKit/536.3 (KHTML, like Gecko) Chrome/19.0.1061.0 Safari/536.3",
              "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/535.24 (KHTML, like Gecko) Chrome/19.0.1055.1 Safari/535.24",
              "Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/535.24 (KHTML, like Gecko) Chrome/19.0.1055.1 Safari/535.24",
              "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/531.21.8 (KHTML, like Gecko) Version/4.0.4 Safari/531.21.10",
              "Mozilla/5.0 (Windows; U; Windows NT 5.2; en-US) AppleWebKit/533.17.8 (KHTML, like Gecko) Version/5.0.1 Safari/533.17.8",
              "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/533.19.4 (KHTML, like Gecko) Version/5.0.2 Safari/533.18.5",
              "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-GB; rv:1.9.1.17) Gecko/20110123 (like Firefox/3.x) SeaMonkey/2.0.12",
              "Mozilla/5.0 (Windows NT 5.2; rv:10.0.1) Gecko/20100101 Firefox/10.0.1 SeaMonkey/2.7.1",
              "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_5_8; en-US) AppleWebKit/532.8 (KHTML, like Gecko) Chrome/4.0.302.2 Safari/532.8",
              "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_4; en-US) AppleWebKit/534.3 (KHTML, like Gecko) Chrome/6.0.464.0 Safari/534.3",
              "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_5; en-US) AppleWebKit/534.13 (KHTML, like Gecko) Chrome/9.0.597.15 Safari/534.13",
              "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_2) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.186 Safari/535.1",
              "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/535.2 (KHTML, like Gecko) Chrome/15.0.874.54 Safari/535.2",
              "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/535.7 (KHTML, like Gecko) Chrome/16.0.912.36 Safari/535.7",
              "Mozilla/5.0 (Macintosh; U; Mac OS X Mach-O; en-US; rv:2.0a) Gecko/20040614 Firefox/3.0.0 ",
              "Mozilla/5.0 (Macintosh; U; PPC Mac OS X 10.5; en-US; rv:1.9.0.3) Gecko/2008092414 Firefox/3.0.3",
              "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1) Gecko/20090624 Firefox/3.5",
              "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.6; en-US; rv:1.9.2.14) Gecko/20110218 AlexaToolbar/alxf-2.0 Firefox/3.6.14",
              "Mozilla/5.0 (Macintosh; U; PPC Mac OS X 10.5; en-US; rv:1.9.2.15) Gecko/20110303 Firefox/3.6.15",
              "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1"
          ]
      ```
   
      
   
   5. spiders/juzi.py
   
      ```
      #自动生成的
      import scrapy
      
      
      class ItjuziSpider(scrapy.Spider):
          name = 'itjuzi'
          allowed_domains = ['www.itjuzi.com']
          start_urls = ['http://www.itjuzi.com/']
      
          def parse(self, response):
              pass
              
      ------------------------
      #修改后的
      # -*- coding: utf-8 -*-
      
      from bs4 import BeautifulSoup
      from scrapy.linkextractors import LinkExtractor
      from scrapy.spiders import CrawlSpider, Rule
      
      from scrapy_redis.spiders import RedisCrawlSpider
      from Itjuzi.items import CompanyItem
      
      
      class ITjuziSpider(RedisCrawlSpider):
      	#爬虫名字
          name = 'itjuzi'
          #爬取范围
          allowed_domains = ['www.itjuzi.com']
          # start_urls = ['http://www.itjuzi.com/company']
          
          #启动爬虫
          redis_key = 'itjuzispider:start_urls'
          rules = [
          	#上下两个Rule的关系
              # 获取每一页的链接
              Rule(link_extractor=LinkExtractor(allow=('/company\?page=\d+'))),
              # 获取每一个公司的详情
              Rule(link_extractor=LinkExtractor(allow=('/company/\d+')), callback='parse_item')
          ]
      
          def parse_item(self, response):
              soup = BeautifulSoup(response.body, 'lxml')
      
              # 开头部分： //div[@class="infoheadrow-v2 ugc-block-item"]
              cpy1 = soup.find('div', class_='infoheadrow-v2')
              if cpy1:
                  # 公司名称：//span[@class="title"]/b/text()[1]
                  company_name = cpy1.find(class_='title').b.contents[0].strip().replace('\t', '').replace('\n', '')
      
                  # 口号： //div[@class="info-line"]/p
                  slogan = cpy1.find(class_='info-line').p.get_text()
      
                  # 分类：子分类//span[@class="scope c-gray-aset"]/a[1]
                  scope_a = cpy1.find(class_='scope c-gray-aset').find_all('a')
                  # 分类：//span[@class="scope c-gray-aset"]/a[1]
                  scope = scope_a[0].get_text().strip() if len(scope_a) > 0 else ''
                  # 子分类：# //span[@class="scope c-gray-aset"]/a[2]
                  sub_scope = scope_a[1].get_text().strip() if len(scope_a) > 1 else ''
      
                  # 城市+区域：//span[@class="loca c-gray-aset"]/a
                  city_a = cpy1.find(class_='loca c-gray-aset').find_all('a')
                  # 城市：//span[@class="loca c-gray-aset"]/a[1]
                  city = city_a[0].get_text().strip() if len(city_a) > 0 else ''
                  # 区域：//span[@class="loca c-gray-aset"]/a[2]
                  area = city_a[1].get_text().strip() if len(city_a) > 1 else ''
      
                  # 主页：//a[@class="weblink marl10"]/@href
                  home_page = cpy1.find(class_='weblink marl10')['href']
                  # 标签：//div[@class="tagset dbi c-gray-aset"]/a
                  tags = cpy1.find(class_='tagset dbi c-gray-aset').get_text().strip().strip().replace('\n', ',')
      
              #基本信息：//div[@class="block-inc-info on-edit-hide"]
              cpy2 = soup.find('div', class_='block-inc-info on-edit-hide')
              if cpy2:
      
                  # 公司简介：//div[@class="block-inc-info on-edit-hide"]//div[@class="des"]
                  company_intro = cpy2.find(class_='des').get_text().strip()
      
                  # 公司全称：成立时间：公司规模：运行状态：//div[@class="des-more"]
                  cpy2_content = cpy2.find(class_='des-more').contents
      
                  # 公司全称：//div[@class="des-more"]/div[1]
                  company_full_name = cpy2_content[1].get_text().strip()[len('公司全称：'):] if cpy2_content[1] else ''
      
                  # 成立时间：//div[@class="des-more"]/div[2]/span[1]
                  found_time = cpy2_content[3].contents[1].get_text().strip()[len('成立时间：'):] if cpy2_content[3] else ''
      
                  # 公司规模：//div[@class="des-more"]/div[2]/span[2]
                  company_size = cpy2_content[3].contents[3].get_text().strip()[len('公司规模：'):] if cpy2_content[3] else ''
      
                  #运营状态：//div[@class="des-more"]/div[3]
                  company_status = cpy2_content[5].get_text().strip() if cpy2_content[5] else ''
      
              # 主体信息：
              main = soup.find('div', class_='main')
      
              # 投资情况：//table[@class="list-round-v2 need2login"]
                # 投资情况，包含获投时间、融资阶段、融资金额、投资公司
              tz = main.find('table', 'list-round-v2')
              tz_list = []
              if tz:
                  all_tr = tz.find_all('tr')
                  for tr in all_tr:
                      tz_dict = {}
                      all_td = tr.find_all('td')
                      tz_dict['tz_time'] = all_td[0].span.get_text().strip()
                      tz_dict['tz_round'] = all_td[1].get_text().strip()
                      tz_dict['tz_finades'] = all_td[2].get_text().strip()
                      tz_dict['tz_capital'] = all_td[3].get_text().strip().replace('\n', ',')
                      tz_list.append(tz_dict)
      
              # 团队信息：成员姓名、成员职称、成员介绍
              tm = main.find('ul', class_='list-prodcase limited-itemnum')
              tm_list = []
              if tm:
                  for li in tm.find_all('li'):
                      tm_dict = {}
                      tm_dict['tm_m_name'] = li.find('span', class_='c').get_text().strip()
                      tm_dict['tm_m_title'] = li.find('span', class_='c-gray').get_text().strip()
                      tm_dict['tm_m_intro'] = li.find('p', class_='mart10 person-des').get_text().strip()
                      tm_list.append(tm_dict)
      
              # 产品信息：产品名称、产品类型、产品介绍
              pdt = main.find('ul', class_='list-prod limited-itemnum')
              pdt_list = []
              if pdt:
                  for li in pdt.find_all('li'):
                      pdt_dict = {}
                      pdt_dict['pdt_name'] = li.find('h4').b.get_text().strip()
                      pdt_dict['pdt_type'] = li.find('span', class_='tag yellow').get_text().strip()
                      pdt_dict['pdt_intro'] = li.find(class_='on-edit-hide').p.get_text().strip()
                      pdt_list.append(pdt_dict)
      
              item = CompanyItem()
              item['info_id'] = response.url.split('/')[-1:][0]
              item['company_name'] = company_name
              item['slogan'] = slogan
              item['scope'] = scope
              item['sub_scope'] = sub_scope
              item['city'] = city
              item['area'] = area
              item['home_page'] = home_page
              item['tags'] = tags
              item['company_intro'] = company_intro
              item['company_full_name'] = company_full_name
              item['found_time'] = found_time
              item['company_size'] = company_size
              item['company_status'] = company_status
              item['tz_info'] = tz_list
              item['tm_info'] = tm_list
              item['pdt_info'] = pdt_list
              return item
      ```
   
      
   
   6. scrapy.cfg
   
      ```
      #内容: 自动生成的与修改后的一致
      # Automatically created by: scrapy startproject
      #
      # For more information about the [deploy] section see:
      # https://scrapyd.readthedocs.io/en/latest/deploy.html
      
      [settings]
      default = Itjuzi.settings
      
      [deploy]
      #url = http://localhost:6800/
      project = Itjuzi
      ```
   
   7. pipelines.py
   
      ```
      #单机版有pipelines.py, 分布式没有pipelines.py
      class ItjuziPipeline:
          def process_item(self, item, spider):
              return item
      ```
   
      
   
   8. 运行：
   
      1. 各个slave端: `scrapy runspider itjuzi.py`
   

      2. master端 redis-cli 命令行 : `lpush itjuzispider:start_urls http://www.itjuzi.com/company`
   

##### 7.8 小结

 1. 爬虫名与 项目名不能相同, 否则 报错. `Cannot create a spider with the same name as your project`

  2. `pip install scrapy-redis`, 或从 pycharm 的 seting 中安装