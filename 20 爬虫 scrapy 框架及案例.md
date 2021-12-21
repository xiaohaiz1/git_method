网站: https://www.w3school.com.cn/xpath/xpath_nodes.asp

##### 总结: 

1. 爬虫获取到的response是网页的源码，爬取前要先确认源码和element中的元素和值是否一致，只有一致了才可以直接用element中的元素和值。
1. meta  #实现不同的解析函数之间 `传递数据`. meta默认 携带部分信息, 如下载延迟, 请求深度等
1. item  #scrapy engine与各个组件之间传递数据



### 20 爬虫 scrapy 框架及案例

--------

### 4 Scrapy框架

 1. Scrapy 框架:纯Python实现 的应用框架
    	1. 爬取网站数据,   提取结构性数据, 
     	2. 用Twisted`['twɪstɪd] `异步网络框架 处理网络通讯， 加快下载速度
         	1. 不用自己 实现异步框架
         	2. 异步网络框架 Twisted`['twɪstɪd]` 主要对手是Tornado
    
 2. Scrapy架构图(绿线是数据流向)

    1. 构成: Scrapy Engine(引擎), Scheduler(调度器), Downloader（下载器）, Spider（爬虫）, Item Pipeline(管道), Downloader Middlewares（下载中间件）, Spider Middlewares（Spider中间件）
    2. 流程: Spiders -- Requests --> Scrapy Engine -- Requests --> Scheduler-- Requests -->Downloader,  Downloader -- response -->Scrapy Engine-- response -->Spiders,  Spiders -- Items --> Scrapy Engine -- Items --> Item Pipeline     
       1. `Scrapy Engine(引擎)`: 负责`Spider`、`Scheduler` 、`Downloader`、`ItemPipeline`、 通讯， 数据传递
       2. `Scheduler(调度器)`:  接受`引擎`发来的Request请求，并排序入队，当`引擎`需要时，发给`引擎`。
       3. `Downloader（下载器）`： 下载`引擎`发来的Requests请求，并将Responses发给`引擎`，由`引擎`发给`Spider`
       4. `Spider（爬虫）`： 从Responses中分析提取数据，获取Item字段需要的数据，将要跟进的URL发给`引擎`，再次进入`Scheduler(调度器)`，
       5. `Item Pipeline(管道)`：处理 从`Spider` 获取的Item. 即 后期处理（详细分析、过滤、存储等）的地方.
       6. `Downloader Middlewares（下载中间件）`：自定义扩展 下载组件
       7. `Spider Middlewares（Spider中间件）`：自定义 扩展和操作, `引擎`和`Spider`间`通信`组件（比如进入`Spider`的Responses;和从`Spider`出去的Requests）

 3. Scrapy的运作流程: 代码写好，程序运行...

    1. `引擎`：Hi！`Spiders`, 你要处理哪一个网站？
2. `Spider`：老大 要我处理xxxx.com。
    3. `引擎`：把要处理 第一个URL给我。
    4. `Spider`：给你，第一个URL xxxxxxx.com。
    5. `引擎`：Hi！`调度器`，一个request请求你 排序入队 
    6. `调度器`：好的，正处理。
    7. `引擎`：Hi！`调度器`，把 处理好的 request 请求给我。
    8. `调度器`：给你， 处理好的request
    9. `引擎`：Hi！下载器，请按照老大的`下载中间件`的设置 下载这个request请求
    10. `下载器`：好的！给你。（如失败：sorry，这个request下载失败了。`引擎`告诉`调度器`，这个request下载失败了，你记录一下，待会儿再下载）
    11. `引擎`：Hi！`Spider`，这是按老大的`下载中间件`下载的东西, 你处理一下（注意！这儿responses默认交给`parse()` 函数处理的）
    12. `Spider`：（处理完毕数据之后, 要跟进的URL），Hi！`引擎`，这是要跟进的URL，这是获取到Item数据。
    13. `引擎`：Hi ！`管道` , 这是item 请处理一下！`调度器`！这是要跟进URL请处理下。然后从第七步开始循环，直到获取完老大需要全部信息。
    14. `管道,调度器`：好的，现在就做！
    
    注意！ `调度器`中不存在任何request了，整个程序 停止，（下载失败的URL，Scrapy 重新下载。）

 4. Scrapy 爬虫  要4步：

    1. 新建项目 : `scrapy startproject xxx`  #新建一个新的爬虫项目
    2. 明确目标 :  `items.py`   #指明要抓取的目标
    3. 制作爬虫:  `spiders/xxspider.py`  #制作爬虫 
    4. 存储内容: `pipelines.py`  #设计管道, 存储爬取内容

#### 4.1 配置安装

1. Scrapy的安装介绍: Scrapy框架 官方网址：http://doc.scrapy.org/en/latest , Scrapy中文 维护站点：http://scrapy-chs.readthedocs.io/zh_CN/latest/index.html

2. Windows 安装方式: Python 2 / 3

   2. 升级pip版本：`pip install --upgrade pip`
   3. 通过pip 安装 Scrapy 框架`pip install Scrapy`
   
3. Ubuntu 需要9.10或以上版本安装方式: Python 2 / 3

   2. 安装非Python的依赖 `sudo apt-get install python-dev python-pip libxml2-dev libxslt1-dev zlib1g-dev libffi-dev libssl-dev`
   3. 通过pip 安装 Scrapy 框架 `sudo pip install scrapy`
   
4. anaconda安装 scrapy框架: `conda install scrapy`即可

5. 安装成功: 命令终端输入` scrapy` ，提示类似结果，已经安装成功

6. Scrapy安装流程: http://doc.scrapy.org/en/latest/intro/install.html#intro-install-platform-notes 里面有各个平台的安装方法

   

#### 4.2 入门案例

* 学习目标:
  * 创建`Scrapy项目`
  * 定义 要提取的 结构化数据(Item)
  * 编写 Spider `爬, 取` 结构化数据(Item)
  * 编写 Item的 Pipelines 存储 Item(即结构化数据)



1. 先 建项目(scrapy startproject)

   1. pycharm中 选 Terminal , `cd` 进入存放项目的目录webframe，运行 命令

      `scrapy startproject mySpider`  #mySpider项目名

   2. 项目目录

      ```
      webframe  #工作目录
      └─── mySpider  #scrapy startproject mySpider命令创建的项目目录
          ├─── mySpider  #项目的Python包
          │	├─── __init__.py
          │	├─── items.py   #目标文件
          │	├─── pipelines.py  #管道文件
          │	├─── settings.py  #设置文件
          │	└─── spiders  #爬虫代码文件夹, 内放很多爬虫
          │		└─── __init__.py  #创建后就有的
          └─── scrapy.cfg  #配置文件
          
      #第一个 mySpider后面是 mySpider, scrapy.cfg,在第一mySpider内部
      #第一个 mySpider是项目目录
      #第二个 mySpider是 项目的Python包
      ```

      1. scrapy.cfg ：项目的配置文件
      2. mySpider/ ：项目的Python包，将从这里引用代码
      3. mySpider/items.py ：项目的目标文件
      4. mySpider/pipelines.py ：项目的管道文件
      5. mySpider/settings.py ：项目的设置文件
      6. mySpider/spiders/ ：存储爬虫代码目录

2. 明确目标(mySpider/items.py)

   1. 项目: 抓取：http://www.itcast.cn/channel/teacher.shtml 网站里的所有讲师的姓名、职称和个人信息

      1. 打开 mySpider/items.py, 创建 scrapy.Item 类的子类，定义 scrapy.Field 类型的字段, 此即子类的类属性, 也是爬虫要保存的

         1. items.py中定义的 结构化字段, 保存 爬取的数据, 像Python的dict，但提供了保护 减少错误。
         
      2. 创建ItcastItem 类继承自 scrapy.Item类，构建item模型（model）
      
      3. items.py 代码
      
         ```
         #自动创建的代码
         import scrapy
         class MyspiderItem(scrapy.Item):
             # define the fields for your item here like:
             # name = scrapy.Field()
             pass
         
         #修改为下面代码
         import scrapy
         
         class ItcastItem(scrapy.Item):
             name = scrapy.Field()
             level = scrapy.Field()
             info = scrapy.Field()
         ```
         

3. 制作爬虫 （spiders/itcast.py）,爬虫 分两步 : 爬数据, 取数据

   1. 爬数据: 创建 爬虫itcast, 自动生成itcast.py文件

      1. pycharm中, `cd mySpider`进入项目 mySpider 中, 

      2. pycharm的Terminal 输入命令: `scrapy genspider itcast "itcast.cn"`, 提示: Created spider 'itcast' using template 'basic' in module:  mySpider.spiders.itcast.

      3.  在`mySpider/spider`目录下创建 名为`itcast.py`的爬虫, `"itcast.cn"`  指定爬取范围. 目录结构
   
         3. ```
            #执行命令前目录结构
            webframe  
            └─── mySpider   #cd 进入mySpider目录下
                ├─── mySpider #在这个目录中执行, 结果一样 
                │	└─── spiders   
                │		├─── __init__.py  #原来有的
             
            #执行命令后目录结构
            webframe  
            └─── mySpider   
             ├─── mySpider  
                │	└─── spiders  
             │		├─── __init__.py  #原来有的
                │		└─── itcast.py  #执行命令新建的文件
         ```
            
   
         1. 在 `\mySpider\mySpider`目录下执行 `scrapy genspider itcast "itcast.cn"` 与 在`\mySpider` 目录中 执行 `scrapy genspider itcast "itcast.cn"` 的结果是一样的, 目录结构也一样
         2. 新创建的`itcast.py` 文件 自动 加入到 `spiders` 文件夹中
      
      4. 编辑 爬虫 mySpider/spider/itcast.py
      
         1. ```
            #原文件内容
            import scrapy
            
            
      
         class MyspiderItem(scrapy.Item):
                # define the fields for your item here like:
             # name = scrapy.Field()
                pass
      
         #修改文件内容
            import scrapy
      
            class ItcastSpider(scrapy.Spider):
             name = "itcast"
                allowed_domains = ["itcast.cn"]
                #start_urls = (
                    'http://www.itcast.cn/',
                )
                #将start_urls的值 改为 要爬取的第一个url
                start_urls = ("http://www.itcast.cn/channel/teacher.shtml",)
      
                def parse(self, response):
                    with open("teacher.html", "w") as f:
                    	f.write(response.text)
            ```
         
            1. 创建 `scrapy.Spider` 类的子类ItcastSpider: 三个类属性 和 一个对象方法
            1. `name = ""` ： 爬虫名称, 唯一的
               2. `allow_domains = []`  搜索的域名, 爬虫的约束区域, 不存在的URL 被忽略。
               3. `start_urls = ()`  爬取的URL(元组或列表)。
                  1. 爬虫从这里开始抓取数据, 第一次下载的数据 从这些urls开始
                  2. 其他子URL 从这个起始URL中继承 生成。
               4. `parse(self, response)` ：解析方法
                  1. 每个初始URL完成下载后, 自动调用该方法,  传入该URL返回的Response对象做实参数
                  2. 主要作用: 解析返回的 网页数据(response.body)，提取结构化数据(生成item), 生成 下一页的URL请求。
            2. 备注: 用自行创建itcast.py.   免去 写固定代码的麻烦
      
      5. 执行 爬虫.  Terminal中,在webframe/mySpider目录下执行
      
         `scrapy crawl itcast`
      
         1. itcast 爬虫名, 是 ItcastSpider 类的 name 属性, 也是 `scrapy genspider`命令 生成的爬虫名
      
      2. Scrapy爬虫项目里有多个爬虫, 各个爬虫 执行时，按照 name 属性 区分。
      
      3. 运行后，打印的日志出现 `[scrapy] INFO: Spider closed (finished)`，执行完成。 当前文件夹出现了 teacher.html 文件，里面就是我们刚刚要爬取的网页的全部源代码信息
      
      4. 目录结构变成:
      
         ```
            webframe  #工作目录
            └─── mySpider  #scrapy startproject mySpider创建的项目目录
                ├─── mySpider  #项目的Python模块
                │	├─── __init__.py
                │	├─── items.py   
             │	├─── pipelines.py  
                │	├─── settings.py   
             │	└─── spiders  
                │		├─── __init__.py  #创建后就有的
             │       └─── itcast.py #scrapy genspider itcast "itcast.cn"创建的
                ├─── scrapy.cfg  #配置文件
             └─── teacher.html #scrapy crawl itcast 创建的
         ```
   
2. 取数据, 任务:观察页面 源码
   
   1. chrome浏览器中, elements
   
      ```
         <div class="li_txt">
          <h3>  xxx  </h3>
             <h4> xxxxx </h4>
          <p> xxxxxxxx </p>
      ```
      
      2. XPath方式提取数据
      
         ```
         def parse(self, response):
             #open("teacher.html","wb").write(response.body).close()
             # 存放信息的集合
             items = []
         
             for each in response.xpath("//div[@class='li_txt']"):
                 # 将我们得到的数据封装到一个 `ItcastItem` 对象 
                 #from mySpider.items import ItcastItem
                 item = ItcastItem()
              #extract()方法返回的都是字符串
                 name = each.xpath("h3/text()").extract()
              title = each.xpath("h4/text()").extract()
                 info = each.xpath("p/text()").extract()
         
                 #xpath返回的是包含一个元素的列表
              item['name'] = name[0]
                 item['title'] = title[0]
              item['info'] = info[0]
         
                 items.append(item)
         
             # 直接返回最后数据
             return items
         ```

4. 保存数据: scrapy保存信息的格式有四种，-o 指定输出格式的文件

   1. 1. `scrapy crawl itcast -o teachers.json`  # json格式，默认为Unicode编码
      2. `scrapy crawl itcast -o teachers.jsonl`  # json lines格式，默认为Unicode编码
      3. `scrapy crawl itcast -o teachers.csv`  #csv 逗号表达式，可用Excel打开
      4. `scrapy crawl itcast -o teachers.xml`  # xml格式
   
5. 思考: yield 在这里的作用

   1. ```
      def parse(self, response):
          #open("teacher.html","wb").write(response.body).close()
          # 存放老师信息的集合
          #items = []
          for each in response.xpath("//div[@class='li_txt']"):
              # 将我们得到的数据封装到一个 `ItcastItem` 对象
              #from mySpider.items import ItcastItem
              item = ItcastItem()
              #extract()方法返回的都是字符串
              name = each.xpath("h3/text()").extract_first()
              title = each.xpath("h4/text()").extract_first()
              info = each.xpath("p/text()").extract_first()
              #items.append(item)
              #将获取的数据交给pipelines
              yield item
      ```
      
      

#### 4.3 Scrapy Shell

1. Scrapy shell: Scrapy的交互终端

   1. 未启动spider时调试代码， 用XPath或CSS表达式提取网页中的数据
   2. 若安装了 IPython ，Scrapy终端 用 IPython (替代标准Python终端)
   
2. 启动Scrapy Shell

   1. terminal中进入项目 `根目录`，执行`scrapy shell ......`命令启动shell

      `scrapy shell "http://www.itcast.cn/channel/teacher.shtml"`, 默认返回对象`response`

   2. Scrapy Shell根据下载的页面自动创建对象，例如 response 对象，以及 `Selector 对象 (对HTML及XML内容)`

   3. shell载入后, 得到本地 response 变量, 它包含response数据. 获取数据

      1. 输入 `response.body`, 输出response的包体
      2. 输入 `response.headers` , 输出response的包头。
      3. 输入 `response.selector` 时，  得到 用response 初始化的 类 Selector 的对象, 用 `response.selector.xpath()`或`response.selector.css()` 对 response 查询。
      4. Scrapy快捷方式: `response.xpath()`或`response.css()`同样可以生效（如之前的案例）

   4. exit()  退出 scrapy shell

      

3. Selectors选择器: Scrapy Selectors 内置 `xpath` 和 CSS Selector 表达式机制

   2. Selector 四个方法， 常用的是xpath
      1. xpath(): 传入参数: `xpath表达式`，返回 对应节点的selector list列表
      2. extract(): 序列化该节点为 字符串 并返回 list
      3. css(): 传入CSS表达式，返回 对应节点的selector list列表，语法同 BeautifulSoup4
      4. re(): 传入 正则表达式，返回字符串list列表
   3. xpath()的参数: xpath表达式的含义
      1. `'/html/head/title'`: 选择`<HTML>`节点中 `<head>` 节点内的 `<title>` 节点
      2. `'/html/head/title/text()'`: 选择上面提到的 `<title>` 节点的文字
      3. `'//td'`: 选择所有的 `<td>` 节点
      4. `'//div[@class="mine"]'`: 选择带 `class="mine"` 属性的 `div` 节点
   
4. scrapy shell中使用Selector (选择器)案例

   1. 任务:  腾讯社招的网站http://hr.tencent.com/position.php?&start=0#a

   2. 步骤:

      1. 启动: `scrapy shell "http://hr.tencent.com/position.php?&start=0#a"`,   返回默认对象`response`

      2. `response.xpath('//title')`     # 返回<class 'scrapy.selector.unified.SelectorList'>类型, 返回值:  `[<Selector xpath='//title' data='<title>职位搜索 | 社会招聘 | Tencent 腾讯招聘</title'>]`

      3. `response.xpath('//title').extract()`  , #提取data, 返回类型<class 'list'>, 返回值: `['<title>职位搜索 | 社会招聘 | Tencent 腾讯招聘</title>']`

      4. `response.xpath('//title').extract_first()`  , #返回类型 <class 'str'>, 返回值: `'<title>职位搜索 | 社会招聘 | Tencent 腾讯招聘</title>'`,  打印列表第一个元素, 没有返回None 

   3. contains的用法，or的用法，last()的含义

      1. `response.xpath('//*[contains(@class,"odd") or contains(@class,"even")]/td[last()]/text()').extract()`
         输出: 
   
         ```
         ['2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02',
          '2017-06-02']
         ```
   
         
   
      2. `response.xpath('//a[contains(@href,"position_detail.php?")]/text()').extract()`
         输出
   
         ```
          ['19407-移动游戏平台合作（上海）',
           '19407-手游商业化与本地化策划（上海）',
           'OMG236-腾讯视频平台高级产品经理（深圳）',
           'OMG096-科技频道记者（北京）',
           '18402-项目管理',
           'IEG-招聘经理（深圳）',
           'OMG097-视觉设计师（北京）',
           'OMG097-策略产品经理/产品运营（北京）',
           'OMG097-策略产品经理/产品运营（北京）',
           'OMG097-数据产品经理(北京）']
         ```
   
         
   
      3. `response.xpath('//*[contains(@class,"odd") or contains(@class,"even")]/td[last()-1]/text()').extract()`
   
         输出:
   
         ```
         ['上海', '上海', '深圳', '北京', '深圳', '深圳', '北京', '北京', '北京', '北京']
         ```

      4. 小结:

         1. `contains(@属性, 属性值)`   #属性有多个值, 选其中一个
         2. `[contains(@属性1, 属性值1) or contains(@属性, 属性值2)]`
   
5. 小结

   1. 数据提取 时 ，可先在Scrapy Shell中测试，测通过后再应用到代码中
   2. 官方文档：http://scrapy-chs.readthedocs.io/zh_CN/latest/topics/shell.html

#### 4.4 Item Pipeline

1. Item pipeline: 即pipelines.py模块, 处理 Item. 每个Item Pipeline 都是实现了简单方法的 Python类.  item pipeline 应用: 验证 爬取的数据(检查item包含某些字段，比如说name字段), 查重 (并丢弃), 将 爬取结果 保存 到文件或者数据库中

   1. 编写 Item pipeline 很简单, Item pipeline 组件是独立的 Python类, process_item()方法 必须实现

   2. pipelines.py

      ```
      import something
      
      class SomethingPipeline(object):
          def __init__(self):    
              # 可选实现，做参数初始化等
              # doing something
      	#process_item() 必须实现
          def process_item(self, item, spider):
              # item (Item 对象) – 被爬取的item
              # spider (Spider 对象) – 爬取该item的爬虫spider
              # 这个方法必须实现，每个item pipeline组件都需要调用该方法，
              # 这个方法必须返回一个 Item 对象，被丢弃的item将不会被之后的pipeline组件所处理。
              return item
      
          def open_spider(self, spider):
              # spider (Spider 对象) – 被开启的spider
              # 可选实现，当spider被开启时，这个方法被调用。
      
          def close_spider(self, spider):
              # spider (Spider 对象) – 被关闭的spider
              # 可选实现，当spider被关闭时，这个方法被调用
      ```

      

2. 完善pipelines.py  :  pipeline将 item 存储到 items.json 文件，每行是 一个序列化为'JSON'格式的'item'。

   1. pipelines.py

      ```
      #改写前,自动生成的内容
      class MyspiderPipeline:
          def process_item(self, item, spider):
              return item
              
      #改写后的内容
      import json
      
      class ItcastJsonPipeline(object):
      
          def __init__(self):
              self.file = open('teacher.json', 'wb')
      
          def process_item(self, item, spider):
              content = json.dumps(dict(item), ensure_ascii=False) + "\n"
              self.file.write(content)
              return item
      
          def close_spider(self, spider):
              self.file.close()
      ```

      1. 编辑后没配置ITEM_PIPELINES, 故不能启动 Item Pipeline组件

   2. 启用一个Item Pipeline组件: 将Item Pipeline的类ItcastJsonPipeline 添加到 `settings.py` 文件中, 配置`ITEM_PIPELINES` 

      1. ```
         `# Configure item pipelines
         `# See http://scrapy.readthedocs.org/en/latest/topics/item-pipeline.html
         
         ITEM_PIPELINES = {
             #'mySpider.pipelines.SomePipeline': 300,
             "mySpider.pipelines.ItcastJsonPipeline":300
         }
         ```
         
         1. item按数字从低到高的顺序 通过pipeline. 数字在0-1000范围内, ，数值越低，组件的优先级越高
         3. settings.py中设置完ITEM_PIPELINES后, pipelines.py才起作用
         

3. 重新启动爬虫: `scrapy crawl itcast`

#### 4.5 Spider

1. 原理: Spider类: 定义爬取的动作, 分析 网页的地方

   1. `class scrapy.Spider`是基类, 所有爬虫必须继承这个类. 类的函数及调用顺序:
      1. `__init__(self)` : 初始化 爬虫名, start_urls列表
      2. `start_requests(self)`,  调用 `make_requests_from_url()`:生成 Requests对象 交给 Scrapy下载并返回 response
      3. `parse(self)`  : 解析response，返回Item或Requests（需指定回调函数）
         2.  Item传给Item pipline持久化 
         3.  Requests交由Scrapy下载，由指定的回调函数处理(默认parse()), 一直循环，直到处理完所有的数据为止
   2. scrapy是包

2. scrapy.Spider源码

   1. ```
      #所有爬虫的基类，用户定义的爬虫必须从这个类继承
      class Spider(object_ref):
      
          #定义spider名字 字符串(string)。spider的名字定义了Scrapy如何定位(初始化)spider，必须是唯一的。
          #name是spider最重要的属性，必须的。
          #一般做法:用 网站(domain)(加或不加 后缀 ) 命名spider.例如 spider爬取 mywebsite.com, 该spider通常会被命名为 mywebsite
          name = None
      	
          #初始化，提取爬虫名字，start_ruls
          def __init__(self, name=None, **kwargs):
              if name is not None:
                  self.name = name
              # 如果爬虫没有名字，中断后续操作则报错
              elif not getattr(self, 'name', None):
                  raise ValueError("%s must have a name" % type(self).__name__)
      
              # python 对象或类型 用 内置成员__dict__ 存储成员
              self.__dict__.update(kwargs)
      
              #URL列表。当没指定的URL时，spider从该列表开始爬取。第一个被获取到的页面的URL将是该列表之一。 后续的URL 从获取到的数据中提取。
              if not hasattr(self, 'start_urls'):
                  self.start_urls = []
      
          # 打印Scrapy执行后的log信息
          def log(self, message, level=log.DEBUG, **kw):
              log.msg(message, spider=self, level=level, **kw)
      
          # 判断对象object的属性是否存在，不存在 做断言处理
          def set_crawler(self, crawler):
              assert not hasattr(self, '_crawler'), "Spider already bounded to %s" % crawler
              self._crawler = crawler
      
          @property
          def crawler(self):
              assert hasattr(self, '_crawler'), "Spider not bounded to any crawler"
              return self._crawler
      
          @property
          def settings(self):
              return self.crawler.settings
      
          #读取start_urls内的地址, 每个地址生成一个Request对象，交给Scrapy下载并返回Response
          
          #该方法仅调用一次
          def start_requests(self):
              for url in self.start_urls:
                  yield self.make_requests_from_url(url)
      
          #被 start_requests() 调用，生成Request
          #Request对象默认的回调函数为parse()，提交的方式为get
          def make_requests_from_url(self, url):
              return Request(url, dont_filter=True)
      
          #默认的Request对象回调函数，处理返回的response。
          #生成Item或者Request对象。用户必须实现这个类
          def parse(self, response):
              raise NotImplementedError
      
          @classmethod
          def handles_request(cls, request):
              return url_is_from_spider(request.url, cls)
      
          def __str__(self):
              return "<%s %r at 0x%0x>" % (type(self).__name__, self.name, id(self))
      
          __repr__ = __str__
      ```
      
   2. 属性和方法

      1. name: spider名字,字符串类型. 如，spider取 mywebsite.com ，该spider通常命名为 mywebsite
      2. allowed_domains: spider 爬取的域名(domain) 列表，可选
      3. start_urls : 初始URL元祖/列表。当没有特定的URL时，spider将从该列表中开始 爬取。
      4. start_requests(self) : 返回一个可迭代对象(iterable)
         2. 该对象 含 第一个Requests  （默认是 start_urls 的url）
         3. spider启动爬取 且未指定start_urls时，调用该方法。
      5. parse(self, response) : 请求url返回网页没有指定回调函数, 默认的Request对象的回调函数。处理返回的response， 生成Item或Request对象。 
      6. log(self, message[, level, component])  : scrapy.log.msg() 方法记录(log)message。 更多数据请参见 [logging](4.7.html)

3. 案例：腾讯招聘网自动翻页采集

   1. 创建 爬虫 (pycharm的Terminal)

      1. `scrapy genspider tencent "tencent.com"`

   2. 编写 items.py, 获取 职位名称、详细信息

      2. ```
         class MyspiderItem(scrapy.Item):
             # define the fields for your item here like:
             # name = scrapy.Field()
             pass
         
         
         #定义tencent.py爬虫的Item类
         class TencentItem(scrapy.Item):
             name = scrapy.Field()
             detailLink = scrapy.Field()
             positionInfo = scrapy.Field()
             peopleNumber = scrapy.Field()
             workLocation = scrapy.Field()
             publishTime = scrapy.Field()
         ```

   3. 编写 tencent.py 爬虫模块

      1. ```
         #自动创建的爬虫文件
         import scrapy
         
         
         class TencentSpider(scrapy.Spider):
             name = 'tencent'
             allowed_domains = ['tencent.com']
             #start_urls中的url直接被提交, 不要手动构造Request实例
             start_urls = ['http://tencent.com/']
         
             def parse(self, response):
                 pass
         
         #改写的tencent爬虫文件:
         #from mySpider.items import TencentItem
         import scrapy
         import re
         
         class TencentSpider(scrapy.Spider):
             name = "tencent"
             allowed_domains = ["hr.tencent.com"]
             start_urls = [
                 "http://hr.tencent.com/position.php?&start=0#a"
             ]
         
             def parse(self, response):
                 items = response.xpath('//*[contains(@class,"odd") or contains(@class,"even")]')
                 
                 for item in items:
                     temp = dict(
                     	#给TencentItem类定义的字段赋值
                         name=item.xpath("./td[1]/a/text()").extract()[0],
                         detailLink="http://hr.tencent.com/"+item.xpath("./td[1]/a/@href").extract()[0],
                         positionInfo=item.xpath('./td[2]/text()').extract()[0] if len(item.xpath('./td[2]/text()').extract())>0 else None,
                         peopleNumber=item.xpath('./td[3]/text()').extract()[0],
                         workLocation=item.xpath('./td[4]/text()').extract()[0],
                         publishTime=item.xpath('./td[5]/text()').extract()[0]
                     )
                     
                     #向Scrapy engine发提取的数据
                     yield temp
         
                 now_page = int(re.search(r"\d+", response.url).group(0))
                 print("*" * 100)
                 if now_page < 216:
                     url = re.sub(r"\d+", str(now_page + 10), response.url)
                     print("this is next page url:", url)
                     print("*" * 100)
                     
                     #向Scrapy engine发提取的url请求, url返回的response由回调函数parse处理
                     yield scrapy.Request(url, callback=self.parse)
         ```
         
         1. 爬虫提取值赋给 Item的各个属性
         2. 爬虫提取新的url并构造新的请求 scrapy.Request()类对象
            1.  scrapy.Request()的构造参数
                1. url: 新url
                2. callback: 回调函数 self.parse, 即: url请求页面 返回的response的解析函数
         3. 爬虫向Scrapy engine发送 Item构成的 dict对象, 爬虫向Scrapy engine发送新的请求 scrapy.Request()

   4. pipeline.py, 处理 Item对象, 即处理爬虫爬取的数据

      1. ```
         import json
         
         #class ItcastJsonPipeline(object):
         	def process_item(self, item, spider):
                 return item
                 
         #定义tencent.py爬虫的数据处理管线        
         class TencentJsonPipeline(object):
         
             def __init__(self):
                 #self.file = open('teacher.json', 'wb')
                 self.file = open('tencent.json', 'wb')
         
             def process_item(self, item, spider):
                 content = json.dumps(dict(item), ensure_ascii=False) + "\n"
                 self.file.write(content)
                 return item
         
             def close_spider(self, spider):
                 self.file.close()
         ```
         
      2. setting.py, 设置ITEM_PIPELINES, 才能启用 pipeline.py

         1. ```
            ITEM_PIPELINES = {
                #'mySpider.pipelines.SomePipeline': 300,
                #"mySpider.pipelines.ItcastJsonPipeline":300
                "mySpider.pipelines.TencentJsonPipeline":300
            }
            ```

   5. 执行爬虫

      1. `scrapy crawl tencent`

   6. 思考  :   parse()方法的工作机制

      1. 用yield，不是return. parse函数 作生成器 用。scrapy 逐一获取parse()生成的结果，并判断结果是什么类型；
      2. 如果是 Request 则加入爬取队列，如果是Item类型则用pipeline.py处理，其他类型则返回错误信息。
         1. scrapy取第一部分的 request 放到队列里，接着从生成器里获取, 不会立马发送这个request
         2. 取尽第一部分的request，然后再获取第二部分的item，取到item，放到对应的pipeline里处理；
      3. parse() : 作回调函数(callback)赋给 Request，指定parse()处理请求 scrapy.Request(url, callback=self.parse)返回的response.
         2. Request对象请求网页返回 scrapy.http.response 响应对象， 送回给parse()方法，直到调度器中没有Request（递归的思路）
         3. 取尽后，parse()工作结束，引擎再根据队列和pipelines中的内容去执行相应的操作；
      4. 程序在取得各个页面的items前，先处理之前所有的request队列里的请求，然后再提取items。
      5. Scrapy引擎和调度器将负责到底。

4. 常见bug: [scrapy.spidermiddlewares.offsite] DEBUG: Filtered offsite request to 'hr.tencent.com':

   1. 解决方式： domain错误 修改domain为：hr.tencent.com

##### 小结: 

1. 每一个爬虫都有一个 `爬虫名.py` 的模块  
2. items.py, pipelines.py, settings.py, middlewares.py 则多个爬虫共有. 在各个文件中或定义独立的类, 或进行设置.



#### 4.6 CrawlSpider

1. 用 `CrawlSpider`模板创建爬虫的命令: 

   1. `scrapy genspider -t crawl tencent tencent.com`
   2. `class scrapy.spiders.CrawlSpider`, Spider的子类.  
      1. Spider类的 设计原则: 只爬取start_url列表中的网页
      2. CrawlSpider类 定义了一些 规则(rule): 跟进link 机制，从爬取的网页中获取link并继续爬取

2. scrapy.spiders.CrawlSpider类源码

   1. ```
      class CrawlSpider(Spider):
          rules = ()
          def __init__(self, *a, **kw):
              super(CrawlSpider, self).__init__(*a, **kw)
              self._compile_rules()
      
          #首先调用parse()处理start_urls返回的response对象
          #parse()将response对象传递给了_parse_response()函数处理，并设置回调函数为parse_start_url()
          #设置了跟进标志位True
          #parse将返回item和跟进了的Request对象    
          def parse(self, response):
              return self._parse_response(response, self.parse_start_url, cb_kwargs={}, follow=True)
      
          #处理start_url请求返回的response， 要重写
          def parse_start_url(self, response):
              return []
      
          def process_results(self, response, results):
              return results
      
          #从response中抽取符合任一用户定义'规则'的链接，并构造Resquest对象返回
          def _requests_to_follow(self, response):
              if not isinstance(response, HtmlResponse):
                  return
              seen = set()
              #抽取之内的所有链接，只要符合任意一个'规则'，即合法
              for n, rule in enumerate(self._rules):
                  links = [ l for l in rule.link_extractor.extract_links(response) if l not in seen]
                  
                  #使用用户指定的process_links处理每个连接
                  if links and rule.process_links:
                      links = rule.process_links(links)
                      
                  #将链接加入seen集合，为每个链接生成Request对象，并设置回调函数为_repsonse_downloaded()
                  for link in links:
                      seen.add(link)
                      #构造Request对象，并将Rule规则中定义的回调函数作为这个Request对象的回调函数
                      r = Request(url=link.url, callback=self._response_downloaded)
                      r.meta.update(rule=n, link_text=link.text)
                      #对每个Request调用process_request()函数。该函数默认为indentify，即不做任何处理，直接返回该Request.
                      yield rule.process_request(r)
      
          #处理通过rule提取出的连接，返回item及request
          def _response_downloaded(self, response):
              rule = self._rules[response.meta['rule']]
              return self._parse_response(response, rule.callback, rule.cb_kwargs, rule.follow)
      
          #用callback解析response对象，返回request或Item对象
          def _parse_response(self, response, callback, cb_kwargs, follow=True):
              #首先判断是否设置了回调函数。（该回调函数可能是rule中的解析函数，也可能是 parse_start_url函数）
              #如果设置了回调函数（parse_start_url()），那么首先用parse_start_url()处理response对象，
              #然后再交给process_results处理。返回cb_res的一个列表
              if callback:
                  #如果是parse调用的，则解析成Request对象
                  #如果是rule callback，则解析成Item
                  cb_res = callback(response, **cb_kwargs) or ()
                  cb_res = self.process_results(response, cb_res)
                  for requests_or_item in iterate_spider_output(cb_res):
                      yield requests_or_item
      
              #如果要跟进，那么用定义的Rule规则提取并返回这些Request对象
              if follow and self._follow_links:
                  #返回每个Request对象
                  for request_or_item in self._requests_to_follow(response):
                      yield request_or_item
      
          def _compile_rules(self):
              def get_method(method):
                  if callable(method):
                      return method
                  elif isinstance(method, basestring):
                      return getattr(self, method, None)
      
              self._rules = [copy.copy(r) for r in self.rules]
              for rule in self._rules:
                  rule.callback = get_method(rule.callback)
                  rule.process_links = get_method(rule.process_links)
                  rule.process_request = get_method(rule.process_request)
      
          def set_crawler(self, crawler):
              super(CrawlSpider, self).set_crawler(crawler)
              self._follow_links = crawler.settings.getbool('CRAWLSPIDER_FOLLOW_LINKS', True)
      ```

      1. CrawlSpider继承于Spider类, 继承 属性 name、allow_domains, 又定义新的属性和方法:
      
         

3. CrawlSpider用rules 定义 爬取规则，匹配的url请求提交给引擎。CrawlSpider不需要手动返回请求

   1. rules 包含一个或多个Rule对象. 每个Rule定义了爬取网站 的特定操作

      2. 如 提取当前内容的链接，是否对提取的链接 跟进爬取，对提交的请求设置回调函数等。
         1. 多个rule匹配了相同的链接, 根据规则 第一个会被使用。

   2. ```
      class scrapy.spiders.Rule(
              link_extractor,
              callback = None,
              cb_kwargs = None,
              follow = None,
              process_links = None,
              process_request = None
      )
      ```

      1. `link_extractor`： 一个Link Extractor对象， 要提取的链接。
      2. `callback`：从link_extractor中获取链接时, 指定 回调函数. 回调函数接受一个response作为 第一个参数。
         1. 注意: 编写爬虫规则，避免用 parse() 作回调函数。因为 CrawlSpider 用 parse() 实现其逻辑, 如果覆盖了 parse()，crawl spider 运行失败。
      3. `follow`：布尔(boolean)值. 是否跟进从response中提取链接. callback为None，follow 默认为True . 否则为False。
      4. `process_links`：指定spider中被调用的函数. 从link_extractor中取到链接列表时 调用该函数。该方法 用来过滤。
      5. `process_request`：指定 spider中被调用的函数, 规则提取的每个request 都调用该函数。 (过滤request)

4. `class scrapy.linkextractors.LinkExtractor`: 提取链接

   1. extract_links(): 每个LinkExtractor 唯一的公共方法  ,  接收 Response 对象，返回 scrapy.link.Link 对象。
      1. LinkExtractors 要实例化一次. 根据不同的 response,  多次调用extract_links() 提取链接｡
      
   2. ```
      class scrapy.linkextractors.LinkExtractor(
          allow = (),
          deny = (),
          allow_domains = (),
          deny_domains = (),
          deny_extensions = None,
          restrict_xpaths = (),
          tags = ('a','area'),
          attrs = ('href'),
          canonicalize = True,
          unique = True,
          process_value = None
      )
      ```

      1. 主要参数
         1. `allow`： “正则表达式”,   提取满足正则的URL.  空， 全部匹配。
         2. `deny`： “正则表达式”,  不提取 满足正则的URL.  优先级高于allow
         3. `allow_domains`： 提取链接的 domains 
         4. `deny_domains`： 不提取链接的domains
         5. `restrict_xpaths`：xpath表达式, 和allow共同过滤链接

5. 提取规则(Crawling rules) 示例

   1. 任务:  腾讯招聘为例， 使用CrawlSpider 配合rule

   2. 步骤:(scrapy shell环境中)

      1. 先运行scrapy shell: `scrapy shell "https://careers.tencent.com/m/search.html?&start=0#a"`,   返回 `response` 名称的对象

      2. 导入LinkExtractor类，创建 LinkExtractor 实例

         1. ```
            from scrapy.linkextractors import LinkExtractor
            
            #allow的值是response中的url的re表达式
            page_lx = LinkExtractor(allow=('position.php?&start=\d+'))
            ```
      
      3. 调用 LinkExtractor实例 的extract_links() 查询匹配结果 `page_lx.extract_links(response)`
      
         1. 没有查到, 返回 `[]`
      
      4. 修改allow的值,即修改匹配规则, 重新抽取url
      
         1. ```
            #allow的值是response中的url的re表达式
            page_lx = LinkExtractor(allow=('position\.php\?&start=\d+'))
            
            page_lx.extract_links(response)
            ```
      
            
      

6. 用CrawlSpider模板创建 tencent.py 爬虫

   1.  scrapy shell交互环境中: 测试代码

      1. ```
         #提取匹配 'http://hr.tencent.com/position.php?&start=\d+'的链接
         page_lx = LinkExtractor(allow = ('start=\d+'))
         
         #下面写法不对
         rules = [
             #提取匹配,并用spider的parse方法解析;并跟进链接(没有callback意味着follow默认为True). 这样写不对,  callback的值不能是 parse
             Rule(page_lx, callback = 'parse', follow = True)
         ]
         ```

         

   2. CrawlSpider模板方式 创建爬虫

      1. Terminal 中: `scrapy startproject mySpider` 创建的项目. 目录结构

         ```
         webframe  #工作目录
         └─── mySpider  #scrapy startproject mySpider命令创建的项目目录
             ├─── mySpider  #项目的Python模块
             │	├─── __init__.py
             │	├─── items.py   #目标文件
             │	├─── pipelines.py  #管道文件
             │	├─── settings.py  #设置文件
             │	└─── spiders  #爬虫代码文件夹, 内放很多爬虫
             │		├─── __init__.py  #创建后就有的
             └─── scrapy.cfg  #配置文件
         ```

      2. Terminal : `cd mySpider`进入mySpider项目, `scrapy genspider -t crawl tencent tencent.com` . 此时目录结构

         ```
         webframe  #工作目录
         └─── mySpider      #项目目录
             ├─── mySpider  #项目的Python模块
             │	├─── __init__.py
             │	├─── items.py   #所有爬虫的 Item 放此文件中
             │	├─── pipelines.py  
             │	├─── settings.py   
             │	└─── spiders  
             │		├─── __init__.py  
             │		└─── tencent.py  #用CrawlSpider模板创建的爬虫
             └─── scrapy.cfg  
         ```

      3. items.py中添加 `class TencentItem(scrapy.Item)`类

         ```
         class TencentItem(scrapy.Item):
             name = scrapy.Field()
             detailLink = scrapy.Field()
             positionInfo = scrapy.Field()
             peopleNumber = scrapy.Field()
             workLocation = scrapy.Field()
             publishTime = scrapy.Field()
      
      4. 修改 tencent.py 代码
      
         ```
         #自动生成的爬虫模块 tencent.py, scrapy genspider -t crawl tencent tencent.com
         import scrapy
         from scrapy.linkextractors import LinkExtractor
         from scrapy.spiders import CrawlSpider, Rule
         
         
         class TecentSpider(CrawlSpider):
             name = 'tencent'
             allowed_domains = ['tencent.com']
             start_urls = ['http://tencent.com/']
         	
         	rules = (
                 Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
             )
         	
         	#解析url返回的response对象的方法
             def parse_item(self, response):
                 item = {}
                 #item['domain_id'] = response.xpath('//input[@id="sid"]/@value').get()
                 #item['name'] = response.xpath('//div[@id="name"]').get()
                 #item['description'] = response.xpath('//div[@id="description"]').get()
                 return item
         
         
         
         #修改scrapy genspider -t crawl tencent tencent.com创建的tencent.py文件:
         import scrapy
         from scrapy.linkextractors import LinkExtractor
         from scrapy.spiders import CrawlSpider, Rule
         
         
         class TecentSpider(CrawlSpider):
             name = 'tencent'
             allowed_domains = ['hr.tencent.com']
             
             #指定起始页url
             start_urls = ['http://hr.tencent.com/position.php?&start=0']
             
             #起始页url的响应交 LinkExtractor() 处理,提取新的url
             page_lx = LinkExtractor(allow=r'start=\d+')
             #position.php?&start=10#a
             
             #callback 不能调用 parse(). 因为CrawlSpider类 用parse()实现其它逻辑
             #新提取的url封装成新的请求
             rules = (
                 Rule(page_lx, callback='parse_item', follow=True),
             )
         	
         	#Rule链接返回的response对象的方法
             def parse_item(self, response):
                 items = response.xpath('//*[contains(@class,"odd") or contains(@class,"even")]')
                 for item in items:
                     temp = dict(
                         position=item.xpath("./td[1]/a/text()").extract()[0],
                         detailLink="http://hr.tencent.com/" + item.xpath("./td[1]/a/@href").extract()[0],
                         type=item.xpath('./td[2]/text()').extract()[0] if len(
                             item.xpath('./td[2]/text()').extract()) > 0 else None,
                         need_num=item.xpath('./td[3]/text()').extract()[0],
                         location=item.xpath('./td[4]/text()').extract()[0],
                         publish_time=item.xpath('./td[5]/text()').extract()[0]
                     )
                     print(temp)
                     yield temp
         
             # parse() 方法不能重写, 因为CrawlSpider类 用parse()实现其它逻辑   
             # def parse(self, response):                                              
             #     pass
         ```
      
      5. 运行tencent.py模块对应的爬虫: `scrapy crawl tencent`

7. Logging日志模块: Scrapy提供 log功能

   1. logging 模块使用:配置文件settings.py

      1. ```
         LOG_FILE = "TencentSpider.log"
         LOG_LEVEL = "INFO"
         ```
         
      2. setting.py中 配置logging

         1. `LOG_ENABLED` 默认: True，启用logging
         2. `LOG_ENCODING` 默认: 'utf-8'，logging使用的编码
         3. `LOG_FILE` 默认: None，在当前目录里创建logging输出文件的文件名
         4. `LOG_LEVEL` 默认: 'DEBUG'，log的最低级别
         5. `LOG_STDOUT` 默认: False 如果为 True，进程所有的标准输出(及错误)将会被重定向到log中。例如，执行 print "hello" ，其将会在Scrapy log中显示

   2. Scrapy的logging提供5级Log levels: 
      1. CRITICAL - 严重错误(critical)
      2. ERROR - 一般错误(regular errors)
      3. WARNING - 警告信息(warning messages)
      4. INFO - 一般信息(informational messages)
      5. DEBUG - 调试信息(debugging messages)
      
      

#### 4.7 Request/Response

请求/响应

1. Request类 部分源码

   1. ```
      # 部分代码
      class Request(object_ref):
      
          def __init__(self, url, callback=None, method='GET', headers=None, body=None, 
                       cookies=None, meta=None, encoding='utf-8', priority=0,
                       dont_filter=False, errback=None):
      
              self._encoding = encoding  # this one has to be set first
              self.method = str(method).upper()
              self._set_url(url)
              self._set_body(body)
              assert isinstance(priority, int), "Request priority not an integer: %r" % priority
              self.priority = priority
      
              assert callback or not errback, "Cannot use errback without a callback"
              self.callback = callback
              self.errback = errback
      
              self.cookies = cookies or {}
              self.headers = Headers(headers or {}, encoding=encoding)
              self.dont_filter = dont_filter
      
              self._meta = dict(meta) if meta else None
      
          @property
          def meta(self):
              if self._meta is None:
                  self._meta = {}
              return self._meta
      ```

      1. 参数

         1. url:  要请求的url

         2. callback:  回调函数, 处理 url请求返回的Response

         3. method:  一般不需要指定, 默认GET方法，可设为"GET", "POST", "PUT"等， 字符串 大写

         4.  headers: 请求头. 一般不需要。内容一般如下

            1. ```
               #请求头
               Host: media.readthedocs.org
               User-Agent: Mozilla/5.0 (Windows NT 6.2; WOW64; rv:33.0) Gecko/20100101 Firefox/33.0
               Accept: text/css,*/*;q=0.1
               Accept-Language: zh-cn,zh;q=0.8,en-us;q=0.5,en;q=0.3
               Accept-Encoding: gzip, deflate
               Referer: http://scrapy-chs.readthedocs.org/zh_CN/0.24/
               Cookie: _ga=GA1.2.1612165614.1415584110;
               Connection: keep-alive
               If-Modified-Since: Mon, 25 Aug 2014 21:59:35 GMT
               Cache-Control: max-age=0
               ```
            
         5. meta: 常用，在不同的请求之间传递数据。dict字典.类型
         
            1. ```
               request_with_cookies = Request(
               url="http://www.example.com",
               cookies={'currency': 'USD', 'country': 'UY'},
               meta={'dont_merge_cookies': True}
               )
               ```
         
         6. encoding:  默认  'utf-8'
         
         7. dont_filter:  请求是否要调度器过滤。多次执行相同的请求,忽略重复的过滤器, 默认为False。
         
      8. errback: 指定错误处理函数
   
2. Response类 部分源码

   1. ```
      class Response(object_ref):
          def __init__(self, url, status=200, headers=None, body='', flags=None, request=None):
              self.headers = Headers(headers or {})
              self.status = int(status)
              self._set_body(body)
              self._set_url(url)
              self.request = request
              self.flags = [] if flags is None else list(flags)
      
          @property
          def meta(self):
              try:
                  return self.request.meta
              except AttributeError:
                  raise AttributeError("Response.meta not available, this response " \
                      "is not tied to any request")
      ```
      
      1. 大部分参数和Request的一致
         1. status: 响应码
         2. _set_body(body)： 响应体
         3. _set_url(url)：响应url
         4. self.request = request
   
3. POST请求: `yield scrapy.FormRequest(url, formdata, callback)`  

   1. 程序开始发送POST请求: 重写Spider类的`start_requests(self)` 方法，并且不 调用start_urls里的url

      2. ```
         #自定义mySpider爬虫对应的mySpider.py模块
         class mySpider(scrapy.Spider):
             # start_urls = ["http://www.example.com/"]
         
             def start_requests(self):
                 url = 'http://www.renren.com/PLogin.do'
         
                 # FormRequest 是Scrapy发送POST请求的方法
                 yield scrapy.FormRequest(
                     url = url,
                     formdata = {"email" : "mr_mao_hacker@163.com", "password" : "axxxxxxxe"},
                     callback = self.parse_page
                 )
             def parse_page(self, response):
                 # do something
         ```
         
         
   
4. 模拟登陆

   1. FormRequest.from_response() 模拟用户登录: 预填充或重写 用户名、用户密码等表单字段

   2. ```
      import scrapy
      
      #自定义LoginSpider爬虫对应的LoginSpider.py模块
      #继承自scrapy.Spider
      class LoginSpider(scrapy.Spider):
          name = 'example.com'
          start_urls = ['http://www.example.com/users/login.php']
      	
      	#用FormRequest.from_response()填充或重写 用户名、用户密码
          def parse(self, response):
              return scrapy.FormRequest.from_response(
                  response,
                  formdata={'username': 'john', 'password': 'secret'},
                  callback=self.after_login
              )
      
          def after_login(self, response):
              # check login succeed before going on
              if "authentication failed" in response.body:
                  self.log("Login failed", level=log.ERROR)
                  return
      
              # continue scraping with authenticated session...
      ```
   
      

5. 知乎爬虫案例参考

   1. 用 CrawlSpider 模板 创建 Zhihu.py模块的爬虫zhihu: Terminal中, 在webframe/mySpider目录下 运行`scrapy genspider -t crawl zhihu www.zhihu.com` 

      2. ```
         webframe  #工作目录
         └─── mySpider  #scrapy startproject mySpider命令创建的项目目录
             ├─── mySpider   
             │	├─── __init__.py
             │	├─── items.py    
             │	├─── pipelines.py   
             │	├─── settings.py   
             │	└─── spiders   
             │		├─── __init__.py  
             │		└─── zhihu.py  #新创建的爬虫zhihu
             └─── scrapy.cfg  #配置文件
         ```
         
         
      
   2. 在 item.py中定义 ZhihuItem类,继承父类Item
   
      1. ```
         from scrapy.item import Item, Field
         
         class ZhihuItem(Item):
             # define the fields for your item here like:
             # name = scrapy.Field()
             url = Field()  #保存抓取问题的url
             title = Field()  #抓取问题的标题
             description = Field()  #抓取问题的描述
             answer = Field()  #抓取问题的答案
             name = Field()  #个人用户的名称
         ```
   
      
   
   2. 编写 zhihu.py爬虫代码
   
      1. ```
         #!/usr/bin/env python
         # -*- coding:utf-8 -*-
         from scrapy.spiders import CrawlSpider, Rule
         from scrapy.selector import Selector
         from scrapy.linkextractors import LinkExtractor
         from scrapy import Request, FormRequest
         # from items import ZhihuItem
         
         class ZhihuSpider(CrawlSpider):
             name = 'zhihu'
             allowed_domains = ['www.zhihu.com']
             start_urls = ['http://www.zhihu.com/']
         
             rules = (
                 Rule(LinkExtractor(allow = ('/question/\d+#.*?', )), callback = 'parse_page', follow = True),
                 Rule(LinkExtractor(allow = ('/question/\d+', )), callback = 'parse_page', follow = True),
             )
         
             headers = {
                 "Accept": "*/*",
                 "Accept-Language": "en-US,en;q=0.8,zh-TW;q=0.6,zh;q=0.4",
                 "Connection": "keep-alive",
                 "Content-Type":" application/x-www-form-urlencoded; charset=UTF-8",
                 "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2125.111 Safari/537.36",
                 "Referer": "http://www.zhihu.com/"
             }
         
             #重写start_requests, 实现自定义请求, 调用自定义的callback回调函数
             def start_requests(self):
                 return [Request("https://www.zhihu.com/login", meta = {'cookiejar' : 1}, callback = self.post_login)]
         
             def post_login(self, response):
                 print('Preparing login')
                 # 下面这句话用于抓取请求网页后返回网页中的_xsrf字段的文字, 用于成功提交表单
                 xsrf = response.xpath('//input[@name="_xsrf"]/@value').extract()[0]
                 print(xsrf)
                 # FormRequeset.from_response()是Scrapy提供的一个函数, 用于post表单
                 # 登陆成功后, 会调用after_login回调函数
                 return [FormRequest.from_response(response,  # "http://www.zhihu.com/login",
                                                   meta={'cookiejar': response.meta['cookiejar']},
                                                   headers=self.headers,  # 注意此处的headers
                                                   formdata={
                                                       '_xsrf': xsrf,
                                                       'email': '123456@qq.com',
                                                       'password': '123456'
                                                   },
                                                   callback=self.after_login,
                                                   dont_filter=True
                                                   )]
         
             def after_login(self, response):
                 for url in self.start_urls:
                     yield self.make_requests_from_url(url)
         
             def parse_page(self, response):
                 problem = Selector(response)
                 item = ZhihuItem()
                 item['url'] = response.url
                 item['name'] = problem.xpath('//span[@class="name"]/text()').extract()
                 print(item['name'])
                 item['title'] = problem.xpath('//h2[@class="zm-item-title zm-editable-content"]/text()').extract()
                 item['description'] = problem.xpath('//div[@class="zm-editable-content"]/text()').extract()
                 item['answer'] = problem.xpath('//div[@class=" zm-editable-content clearfix"]/text()').extract()
                 return item
         ```
   
         
   
   3. setting.py 设置
   
      1. ```
         BOT_NAME = 'zhihu'
         
         SPIDER_MODULES = ['zhihu.spiders']
         NEWSPIDER_MODULE = 'zhihu.spiders'
         DOWNLOAD_DELAY = 0.25   #设置下载间隔为250ms
         ```
      
   5. Terminal中运行 `scrapy crawl zhihu`

#### 4.8 Downloader 与 Middlewares

 1. 反爬虫相关机制: 网站 用 复杂性规则防范爬虫. 绕过规则是困难和复杂的,  有时要特殊的基础设施

 2. 爬虫的Scrapy框架的几个策略

      1. 动态设置User-Agent.  1. 随机切换 User-Agent, 2. 模拟不同用户的浏览器信息
      2. 禁用Cookies:  有些网站用cookie 发现爬虫. 不开启cookies middleware, 不向Server发送cookies, `COOKIES_ENABLED`  设置 CookiesMiddleware 开启或关闭
      3. 延迟下载: 1 .  防止访问频繁, 2. 设置为 2秒 或更高
      4. Google Cache 和 Baidu Cache:  如可能，用谷歌/百度等搜索引擎服务器的 `页面缓存` 获取数据。
      5. IP地址池：VPN和代理IP.    大部分网站 根据IP 反爬
      6. 使用 [Crawlera](https://scrapinghub.com/crawlera)（专用 爬虫的代理组件）, 项目所有的request都 通过crawlera发出。

 3. 下载中间件（Downloader Middlewares）:  引擎(crawler.engine)和下载器(crawler.engine.download())之间的组件. 可以有多个下载中间件被加载运行

     1. 作用 :  引擎 传递请求 给下载器，下载中间件处理请求(如增加http header信息，增加proxy信息等）;下载器 传递响应 给引擎, 下载中间件处理响应(如 gzip 解压等）

     2. settings.py中设置激活下载器中间件

         	DOWNLOADER_MIDDLEWARES = {
         	    'mySpider.middlewares.MyDownloaderMiddleware': 543,
         	}
			
         	  1. 字典类型:  键 为 中间件类的路径, 值 为 中间件的顺序(order)

     3. 编写下载器中间件.  中间件 是 一个Python类.  十分简单, 有一个或多个方法

           ````
           class scrapy.contrib.downloadermiddleware.DownloaderMiddleware
           ````

           ```
           #每个request通过下载中间件时，该方法被调用
           process_request(self, request, spider)
           参数: 
           1.request: (Request的实例)要处理的request, 
           2.spider: (Spider的实例) request对应的spider
           
           返回值：或None ,或一个 Response实例, 或一个 Request实例, 或 raise IgnoreRequest
            	1. 返回 None: Scrapy 继续处理该request，执行其他的中间件的相应方法，直到合适的下载器处理函数(download handler)被调用， 该request被执行(其response被下载)。
           	2. 返回 Response实例: Scrapy 不调用任何其他的 process_request() 或 rocess_exception() 方法，或相应地下载函数；其返回该response； 已安装的中间件的 process_response() 方法 在每个response返回时被调用。
           	3. 返回 Request实例: Scrapy 停止调用 process_request方法并重新调度返回的request, 当新返回的request被执行后, 相应地中间件链 根据下载的response被调用。
           	4. raise一个 IgnoreRequest 异常: 安装的下载中间件的 process_exception() 方法会被调用,如 没有方法处理该异常， 则request的errback(Request.errback)方法 被调用, 如果没有代码处理抛出的异常， 则该异常被忽略且不记录(不同于其他异常那样)
           ```

           ```
           下载器完成http请求，响应传给引擎时 调用
           process_response(self, request, response, spider)
           
           参数
            	1. request (Request实例) – response所对应的request
           	2. response (Response实例)– 被处理的response
           	3. spider (Spider实例)– response所对应的spider
           
           返回值: 之一:  一个 Response 对象, 一个 Request 对象或raise一个 IgnoreRequest 异常
           1. 返回一个 Response 
           	1. 可与传入的response相同，也可以是全新的对象
           	2. 该response 被 链中的其他中间件的 process_response() 处理。
           2. 返回一个 Request 对象
           	1. 中间件链停止， 返回的request 被重新调度下载
           	2. 处理类似于 process_request() 返回request 那样。
           3. 抛出一个 IgnoreRequest 异常
           	1. 调用request的errback(Request.errback)
           	2. 如 没有代码处理抛出的异常，则该异常被忽略且不记录(不同于其他异常那样)
           ```

 4. 案例:  修改`middlewares.py`文件.  Scrapy的代理IP、Uesr-Agent  由`settings.py`中 `DOWNLOADER_MIDDLEWARES` 控制

     1. ````
          # middlewares.py, 包装所有请求
          
          #!/usr/bin/env python
          # -*- coding:utf-8 -*-
          
          import random
          import base64
          
          from settings import USER_AGENTS
          from settings import PROXIES
          
          # 随机的User-Agent
          class RandomUserAgent(object):
              def process_request(self, request, spider):
                  useragent = random.choice(USER_AGENTS)
          
                  request.headers.setdefault("User-Agent", useragent)
          
          class RandomProxy(object):
              def process_request(self, request, spider):
                  proxy = random.choice(PROXIES)
          
                  if proxy['user_passwd'] is None:
                      # 没有代理账户验证的代理 使用方式
                      request.meta['proxy'] = "http://" + proxy['ip_port']
                  else:
                      # 账户密码 base64编码转换
                      base64_userpasswd = base64.b64encode(proxy['user_passwd'])
                      # 对应到代理服务器的信令格式里
                      request.headers['Proxy-Authorization'] = 'Basic ' + base64_userpasswd
                      request.meta['proxy'] = "http://" + proxy['ip_port']
          ````

          1. 为什么HTTP代理 用base64编码: HTTP代理的原理很简单. `客户端` 通过HTTP协议与`代理服务器`建立连接，协议信令中包含 连接到的`远程主机`的 IP和端口号, 要身份验证时 加上授权信息，服务器收到信令后 先 身份验证，通过后便与远程主机建立连接. 连接成功返回给客户端200，表示验证通过

          2. 信令格式

             ```
             CONNECT 59.64.128.198:21 HTTP/1.1
             Host: 59.64.128.198:21
             Proxy-Authorization: Basic bGV2I1TU5OTIz
             User-Agent: OpenFetion
             ```

             1. `Proxy-Authorization` 身份验证信息: Basic后面的字符串是`用户名和密码`组合的base64编码, 即username:password的base64编码
             2. `HTTP/1.0 200 Connection established`,  客户端收到此信令 表示成功建立连接, 接下来 发送给远程主机的数据 可以发送给代理服务器了.  代理服务器 根据 IP地址和端口号 把对应的连接放入缓存，收到信令后 根据IP地址和端口号从缓存中找到对应的连接，将数据通过该连接转发出去。

     2. 修改settings.py, 配置USER_AGENTS, PROXIES, 禁止Cookies, 延时, 添加下载中间件类

         1. ```
            #添加USER_AGENTS
            USER_AGENTS = [
            "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 2.0.50727; Media Center PC 6.0)",
            "Mozilla/5.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0; WOW64; Trident/4.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 1.0.3705; .NET CLR 1.1.4322)",
            "Mozilla/4.0 (compatible; MSIE 7.0b; Windows NT 5.2; .NET CLR 1.1.4322; .NET CLR 2.0.50727; InfoPath.2; .NET CLR 3.0.04506.30)",
            "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN) AppleWebKit/523.15 (KHTML, like Gecko, Safari/419.3) Arora/0.3 (Change: 287 c9dfb30)",
            "Mozilla/5.0 (X11; U; Linux; en-US) AppleWebKit/527+ (KHTML, like Gecko, Safari/419.3) Arora/0.6",
            "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.8.1.2pre) Gecko/20070215 K-Ninja/2.1.1",
            "Mozilla/5.0 (Windows; U; Windows NT 5.1; zh-CN; rv:1.9) Gecko/20080705 Firefox/3.0 Kapiko/3.0",
            "Mozilla/5.0 (X11; Linux i686; U;) Gecko/20070322 Kazehakase/0.4.5"
            ]
            
            #添加代理IP设置PROXIES, 免费代理IP可 网上搜索，或 付费购买 可用的私密代理IP
            PROXIES = [
                {'ip_port': '111.8.60.9:8123', 'user_passwd': 'user1:pass1'},
                {'ip_port': '101.71.27.120:80', 'user_passwd': 'user2:pass2'},
                {'ip_port': '122.96.59.104:80', 'user_passwd': 'user3:pass3'},
                {'ip_port': '122.224.249.122:8088', 'user_passwd': 'user4:pass4'},
            ]
         	
            #禁用cookies，防止某些网站根据Cookie 封锁爬虫.  除非特殊需要
            COOKIES_ENABLED = False
            
            #下载延迟
            DOWNLOAD_DELAY = 3
            
            #代理IP、Uesr-Agent 由 DOWNLOADER_MIDDLEWARES 控制
            #添加自定义的下载中间件类
            DOWNLOADER_MIDDLEWARES = {
            	#'mySpider.middlewares.MyCustomDownloaderMiddleware': 543,
            	'mySpider.middlewares.RandomUserAgent': 1,
            	'mySpider.middlewares.ProxyMiddleware': 100
            }
            ```


#### 4.9 Settings

1. settings.py : 定制Scrapy组件.  1. 控制 `核心`(core)，`插件`(extension)，`pipeline`及`spider`组件; 2. 设置Json Pipeliine、LOG_LEVEL等

2. settings.py设置参考

   1. `BOT_NAME`: 默认: 'scrapybot', 用 startproject 命令创建项目时 自动赋值。

   2. `CONCURRENT_ITEMS`  :  默认: 100, Item Processor(即 Item Pipeline) 同时处理(每个response的)item的最大值。
   
   3. `CONCURRENT_REQUESTS` : 默认: 16, Scrapy downloader 并发请求(concurrent requests)的最大值。
   
   4. `DEFAULT_REQUEST_HEADERS` : Scrapy HTTP Request使用的默认headers
   
      ```
      {
      'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
      'Accept-Language': 'en',
      }
      ```

   5. `DEPTH_LIMIT`  :  默认: 0,  爬取网站的最大深度(depth)值。 0， 没有限制。

   6. `DOWNLOAD_DELAY`  :  默认: 0 , 下载同一个网站下一个页面前 等待时间。该选项 限制爬取速度， 减轻服务器压力。 支持小数: `DOWNLOAD_DELAY = 0.25 # 250 ms of delay`,  默认情况下，Scrapy在两个请求间不等待固定的值， 而是用0.5到1.5之间的 `随机值` * DOWNLOAD_DELAY 作为等待间隔。

   7. `DOWNLOAD_TIMEOUT`  :  默认: 180,  下载器超时时间(单位: 秒)。
   
   8. `ITEM_PIPELINES`  :  默认: {},  设置项目中使用的pipeline及其顺序。  默认为空，值(value)任意, 在0-1000内，越小优先级越高。 
   
      ```
      ITEM_PIPELINES = {
      'mySpider.pipelines.SomethingPipeline': 300,
      'mySpider.pipelines.ItcastJsonPipeline': 800,
      }
      ```

   9. `LOG_ENABLED`  :  默认: True,  是否启用logging。
   
   10. `LOG_ENCODING`   :  默认: 'utf-8',  logging 用的编码。
   
   11. `LOG_LEVEL`  :  默认: 'DEBUG',  最低级别。可选的级别有: CRITICAL、 ERROR、WARNING、INFO、DEBUG 。
   
   12. `USER_AGENT`   :   默认: "Scrapy/VERSION (+http://scrapy.org)",  默认的User-Agent。

   13. `PROXIES`： 代理设置

       ```
       PROXIES = [
         {'ip_port': '111.11.228.75:80', 'password': ''},
         {'ip_port': '120.198.243.22:80', 'password': ''},
         {'ip_port': '111.8.60.9:8123', 'password': ''},
         {'ip_port': '101.71.27.120:80', 'password': ''},
         {'ip_port': '122.96.59.104:80', 'password': ''},
         {'ip_port': '122.224.249.122:8088', 'password':''},
       ]
       ```

   14. `COOKIES_ENABLED = False`   #禁用Cookies

##### 4 小结

1. 模块 与 框架:  很多模块组成框架

2. 同步与异步:  阻塞 是 同步过程,  非阻塞 是异步过程.  非阻塞, 指 调用不能立即得到结果时, 调用不会阻塞当前线程

3. 数据重复, 用 deepcopy() 处理

4. JS 生成的url: 找规律, 响应中 应该有 页面数 和 总页码数

5. CrawlSpider的使用命令: `scrapy genspider -t crawl 爬虫名 域名`,  爬虫模块中`start_url`   指定起始url, 和rules 参数.   

6. CrawlSpider使用条件 : url 能用 re 或 xpath 得到, 页面有需要的数据, 若没有, 在 callback 中手动构造请求

7. CrawlSpider 使用注意点 : `parse()`  不能重写, CrawlSpider用它实现其他逻辑

8. 下载中间件的方法:

   1. process_request(), 用于处理请求,  return request
      2. 添加随机 UA, `request.headers["user-agent"] = " "`
      3. 添加代理 IP, `request.meta["proxy"]="IP + PORT"`
   2. process_response(), 用于处理响应,  return response

9. 爬虫模块中的请求:

   1. 创建Request实例带 cookies 参数 : `scrapy.Request(url, callback, cookies={})`  

      2. `cookies` 不能放 headers 中, headers 中放请求头 `cookie: cookie值`
      
   2. 创建form 表单的FormRequest实例, `scrapy.FormRequest(url, formdata={}, callback=)`

   3. 设置 formdata，填写并提交表单，实现模拟登入: `scrapy.FormRequest.from_response(response, formdata={}, callback)`

   4. ```
      import urllib
      a = "http://.../m?kw=a"
      b = "m?kz=c"
      url.parse.urllib(a.b)
      ```

10. scrapy: 让 爬虫 更快 更强, requests + selenium 爬取 90% 需求

   1. scrapy: 爬取网站数据, 提取数据的 应用框架, 使用了 Twisted['twɪstɪd] 异步网络框架

   2. scrapy 爬虫流程:  url队列 --> 发送请求 --> 内容提取(提取数据, 提取url) --> 数据队列 --> 保存数据

   3. 组件: Scrapy Engine, Scheduler, Downloader, Downloader Middleware, Spider,  Spider Middlewares, Item Pipeline

         1. <table>
                <tr><td colspan=3> Scrapy 组件 功能 实现</td></tr>
                <tr><td>组件名</td><td>功能</td><td>实现方式</td></tr>
                <tr><td>Scrapy Engine(引擎)</td><td>总指挥:负责数据和信号的在不同模块间的传递</td><td>scrapy已经实现</td></tr>
                <tr><td>Scheduler(调度器)</td><td>一个队列，存放引擎发过来的request请求</td><td>scrapy已经实现</td></tr>
                <tr><td>Downloader (下载器)</td><td>下载 引擎发过来的requests请求，并返回给引擎</td><td>scrapy已经实现</td></tr>
                <tr><td>Spider 爬虫</td><td>py模块, 处理引擎发来的response, 提取数据， 提取ur, 并交给引擎</td><td>需要手写</td></tr>
                <tr><td>Item Pipeline(管道)</td><td>items.py模块, 处理引擎传过来的数据，比如存储</td><td>需要手写</td></tr>
                <tr><td>Downloader Middlewares(下载中间件)</td><td>可以自定义的下载扩展，比如设置代理</td><td>一般不用手写</td></tr>
                <tr><td>Spider MiddlewaresSpider(中间件)</td><td>可以自定义requests请求和进行response过滤</td><td>一般不用手写</td></tr>
            </table>

         2. 各模块的功能

            1. items . py # 定义要爬取的数据字段
            2. middlewares.py  #自定义 中间件 的文件
            3. pipelines. py  #管道，处理item,保存item
            4. settings.py  #设置文件,  UA, 启动管道
            5. spiders.py  #自己定义的spider的文件
            6. scrapy.cfg  #项目的配置文件

   4. Scrapy使用步骤: 

      1. 创建一 个scrapy项目 `scrapy startproject mySpider`
      2. 生成一个爬虫 `scrapy genspider itcast "itcast.cn"`
      3. 提取数据 : 用xpath等方法
      4. 保存数据 `pipeline`中保存数据

   5. Scrapy 之 scrapy shell : 交互终端, 调试代码 用. 使用方法:

         1. 命令行窗口: `scrapy shell zhibo8.cc`,  返回 `response` 对象
         2. response 的属性
            1. response.url  #当前响应的 url 地址
            2. response.headers  #响应头
            3. response. body  #响应体, 即`html`代码,默认是byte类型, `.deode()` 转为 str
            4. response.meta
            5. response.request.url  #当前响应对应的 请求的 url 地址
            6. response.request.headers   #当前响应的 请求的 头
            7. response.request.meta
            8. request.meta

   6. Item 之 items.py : 定义解析后的 数据结构

         1. ```
            class MyspiderItem(scrapy.Item):  # .Item 是一个字典
            	#define the fields for your item here like:
            	name = scrapy.Field()  # scrapy.Field() 是一个字典
            	pass
            ```

            1. `Myspiderltem` , 是一个类, 理解为一个字典
            2. scrapy 为什么 定义一个字典 ?   用不同的 Item 存放提取的不同的数据, 数据交给 `pipeline` 时, 用 `isinstance(item, Myspiderltem)` 判断数据属于哪个item, 进行不同的数据处理

         2. Item示例:  在 yg.py 爬虫中

            1. items.py 中 定义

               1. ```
                  class YgItem(scrapy.Item):
                      title = scrapy.Field()
                      href = scrapy.Field()
                      number = scrapy. Field()
                      hanle_ state = scrapy.Field()
                      update_ time = scrapy.Field()
                      involved_ department = scrapy. Field()
                      content = scrapy. Field()  
                  ```

            2. 爬虫 yg.py 中, `meta` 指定 在不同的解析函数之间传递的 数据 `item` 

               1. ```
                  from Yg.items import YgItem
                  def parse(self, response):
                  	tr_list = response.xpath(' //div[@class="greyframe"]/table//table/tr')
                  	for i in tr_list:
                  		item = YgItem()  #实例化自定义的item, item 和字典的操作一样
                  		......
                  		......
                  		#yield item
                  		#指定请求的解析函数, meta向该函数传达的数据
                  		yield scrapy.Request(item["href"], callback=self.get_content,  meta={"item":item})
                  
                  #meta: 解析函数parse()向解析函数get_content()传达数据
                  def get_content(self, response):
                  传递数子店
                  	item = response.meta['item'] #meta['item']中的item是键, 从parse中过来的
                  	item["content"] = response.xpath("//div[@class='content text14_2']//text()").extract()
                  	yield item  #数据统一传递给 pipeline
                  ```

                  1.  item["title"], item["href"], item["number"]

   7. Scrapy 之 pipeline: pipelines.py

         1. ````
            import json
            class JsonWriterPipeline(object):  #
            	
            	def open_spider(self, spider):  #爬虫开启时开启，仅执行一次
            		self.file =open(spider.settings.get("SAVE_FILE", "./temp.json"), 'w')
            
            	def close_spider(self, spider): #爬虫关闭时开启，仅执行一次
            		self. file.close()
                
                #处理scrapy engine传递过来的item
                def process_item(self, item, spider): 
                	line =json.dumps(dict(item)) + "\n"
            		self.file.write(line )
            		return item   #若不return的情况, 另一个权重较低的pipeline 不会获取到该item
            ````

            1. 方法名 pen_spider, close_spider, process_item 是固定的, 不能随意更改

         2. 示例: pipelines.py

            1. ```
               import json
               
               #定义mySpider爬虫对应的管道类MyspiderPipeline
               class MyspiderPipeline (object):
               	
               	#存储爬虫mySpider爬取的数据item
               	#process_item名字是固定的格式, 不能改为其他的名字
               	def process_item(self, item， spider): 
               		with open( 'temp.txt','a') as f:
               		json.dump(item, f, ensure_ascii=False, indent=2)
               ```

         3. 在 `setting.py` 中设置开启 pipelines

            1. ```
               ITEM_ PIPELINES = {
               #  myspider 项目名, pipelines是项目的 pipelines.py
               'myspider.pipelines.MyspiderPipeline': 300, #权重小, 优先级高
               }
               ```

            2. 为什么要多个pipeline: 

               1. 有多个 spider, 不同的 pipeline 处理不同的 item 的内容
               2. 一个 spider 的 可以做多个不同的操作, 如存入不同的数据库中

   8. Scrapy 之 logging

         1. ```
            import logging
            logging.warning(item)
            logging.debug(item)
            loggging.info(itme)
            ```

         2. 流程

            1. `settings.py` 中设置 LOG_LEVEL 日志级别, 日志存储位置

               1. `LOG_LEVEL  = "WARNING"`  #日志级别, 全部大写
               2. `LOG.FILE = "./log.log"`   #设置日志存储位置

            2. pipelines.py 中 添加 `logging.warning()`

               1. 不显示log位置

                  1. ```
                     import logging
                     def process_item(self, item, spider):
                     	logging.warning(item)
                     	
                     #输出 2017-07-12 10:47:08 [root] WARNING: { 'name' :......
                     ```

               2. 显示log位置

                  1. ```
                     import logging
                     logger = logging.getLogger(__name__)
                     
                     def process_item(self,item,spider):
                     	logger.warning(item)
                     	
                     #输出  2017-07-12 11 :06:20 [myspider.pipelines] WARNING:  #myspider.pipelines是 log 位置 
                     ```

         3. `logging` 在普通文件中使用

            1. log_a.py

               1. ```
                  import logging
                  logging.basicConfig()  #设置日志样式, 百度搜之
                  
                  #显示 log 位置
                  logger = logging.getLogger(__name__)  #logger对象
                  if __name__ == "__main__":
                  	logger.info("this is a info")
                  ```

            2. log_b.py 中调用 log_a.py

               1. ```
                  from log_a import logger
                  
                  if __name__ == "__main__":
                  	logger.warning("this is b")
                  ```

   9. Scrapy 之 settings.py 文件

         1. 配置文件 settings.py

            1. 存放 公共的变量(如数据库的地址, 账号密码等), 方便自己和别人修改
            2. 全大写字母命名变量名 , 如 SQL_HOST = `192.168.0.1', 

         2. 在spider中，setting 通过 `self.settings` 方式

            1. ```
               class MySpider(scrapy.Spider):
               	name = ' myspider '
               	start_ urls = ['http://example. com']
               	
               	def parse(self， response):
               		print("Existing settings: %S" % self.settings.attributes.keys())
               ```

               1. `settings.attributes.keys()`  #查看索引属性的键
               2. `settings.attributes.values()`  #查看所有键对应的值
               3. `settings.attributes.items()`  #查看所有项
               4. `settings.attributes["USER_AGENT"] `   # `<SettingsAttribute value='Scrapy/2.4.1 (+https://scrapy.org)' priority=0>`
               5. `settings["USER_AGENT"] `  #`'Scrapy/2.4.1 (+https://scrapy.org)'`

         3. 在 `pipelines.py` 中用 `settings.py` 设置的内容

            1. ```
               class YgPipeline(object) :
               	def open_spider (self, spider):
               		print (spider.settings.get("BOT_NAME", None))
               ```

         4. 使用 settings.py 中 属性方法的3中方式

            1. 方式一: `from Yg.settings import MONGO_HOST`
            2. 方式二:  `spider对象.属性`,  `self.settings["MONG_HOST"]`
            3. 方式三: `spider对象.方法()`, `self.settings.get("MONG_HOST")` 
               1. 方式二与方式三的值 相同

   10. itcast爬虫示例: itcast.py

         1. ```

     class ItcastSpider (scrapy.Spider): #自定义spider类，继承自scrapy.spider
            	name = 'itcast' #爬虫名字<爬虫启动时用: scrapy crawl itcast>
            	allowed_domains = ['itcast.cn'] #允许爬取的范围，防止爬虫爬到了别的网站
            	start_urls = ['http://www.itcast.cn/channel/teacher.shtml'] #进始爬取的地址
            	#解析响应response, 提取数据
            	#def parse(self, response): #数据提取方法，接收下载中间件传过来的response
            		#names = response.xpath("//div[@class='tea_con']//li/div/h3/text()")
            		#print (names) #返回包含选择器的列表
            		
            	#解析函数 parse() 修改为: 
                def parse(self, response):
                    #xpath提取数据
                    teachers=response.xpath("//diV[@class='tea_com']//li")  
            
                    for t in teachers:
                        #当前节点下提取数据
                        name = t.xpath("./div/h3/text()").extract_first()
                        position = t.xpath("./div/h4/text()").extract_first()
                        profile = t.xpath("./div/p/text()").extract_first()
                        item = dict(
                            name = name,
                            position = position,
                            profile = profile
                        )
                        yield item  #yield就可以了
            ```
            
            1. parse() 提取start_urls的response中想要的内容,  parse方法名不能修改. 要爬取的 url 地址必须属于 allow_domain 下的连接. respone.xpath()  #返回一个列表, 包含 selector对象 
              2. 为什么要使用 yield ?  yield让函数变成一个生成器，generator(生成器)每次遍历时挨个读到内存中，不会导致内存的占用量变高
            
         2. 选择器 提取数据的方法
       
            1. extract()  #返回一个列表, 包含 字符串数据
     2. extract_first()  #返回列表中的 第一个 字符串
       
               3. python3 中 range 和 python2 中的 xrange 同理


​         

11. 翻页 实现

       1. requests 模块 发送翻页的请求, 找到下一页地址, 之后调用 requests.get(url)
          1. 思路:  找到下一页 地址, 构造 下一页 url 地址的 request 请求传递给调度器

       2. 实现翻页

          1. ```
             #爬虫.py文件中, 用url,回调函数等构建请求对象
             next_page_url = response.xpath("//a[text()='下一页']/@href").extract()
             while len(next_page_url)>0:
             	yield scrapy.Request(next_page_url, callback=self.parse)  #scrapy.Request 能构建一个 Request 对象, 同时指定 提取数据的 callback 函数

       3. `scrapy.Request(url [,callback, method='GET', headers, body, cookies, meta, dont_filter=False])`,  方括号中的参数表示 可选参数

          1. 参数

             1. callback  #指定 url 的响应的 解析函数
             2. meta  #实现不同的解析函数之间 `传递数据`. meta默认 携带部分信息, 如下载延迟, 请求深度等
             3. dont_filter #让scrapy的去重不过滤当前url. scrapy默认有url去重功能, 对需要重复请求的url有重要用途 !

       

12. Mongodb 回顾

    1. 程序中Mongodb使用: 代码 pipelines.py

       1. ```
          from pymongo import MongoClient #客户端
          
          class SnbookPipeline(object):
          	def open_spider(self,spider) :
          		#创建数据库连接
          		con = MongoClient(spider.settings.get("HOST")，spider.settings.get("PORT"))  #创建mongoclient实例
          		db = con[spider.settings.get("DB")]  #连接到数据库
          		self.collection = db[spider.settings.get("COLLECTION")]  #连接到集合
          	
          	def process_item(self，item， spider):
          		self.collection.insert(dict(item))  #插入字典形式的item
          ```

    2. `mongodb shell` 命令回顾

       1. show dbs  #查看数据库
       2. use books  #使用数据库
       3. db.suning.find()  #无条件查找
       4. db.suning.find().pretty()   #查找并美化打印
       5. db.suning.find().count()   #查找并统计
       6. db.suning.remove({'catetory':'文学小说'})  # 删除所有 `category` 为 `文学小说` 的数据
       7. db.suning.drop()  #删除集合(表)

23. Scrapy 中 CrawlSpider模板: 

       1. 爬虫关键寻找下一页的 url, 思路

          1. 步1: `response` 中提取 ur 地址
          2. 步2: 自动构造`requests` 请求, 发给引擎
          3. `CrawlSpider` 可自动实现上面 两步

       3. 模板 `crawlspider` 创建爬虫: `scrapy genspider -t crawl csdn 'csdn.cn'`

       4. CrawlSpider创建的爬虫代码: csdn.py

          1. ```
             from scrapy.linkextractors import linkExtractor #连接提取器类
             from scrapy.spiders import CrawlSpider, Rule
              
             class CsdnspiderSpider (CrawlSpider):  #父类:spiders的CrawlSpider类
             	name = 'csdnspider'
             	allowed_domains = ['blog.csdn.net']
             	start_urls = ['http://blog.csdn.net/peoplelist.html?channelid=0&page=1']  #第一次 请求的url,如对这个url有特殊处理，则重写父类的parse_start_url()处理这个url的响应
             
             	#元组类型, 元素是 url提取器
             	rules = (
             		#allow指定要匹配url的正则表达式, follow是否跟进链接
             		Rule(LinkExtractor(allow=r'http://blog.csdn.net/\w+$')，follow=True), #找到所有作者的博客地址并且请求,$符号表示以 \w 结尾，否则会匹配 \w+'/abc/def'等内容
             		Rule(LinkExtractor(allow=r'peoplelist.html?channelid=\d+&page=\d+$'), follow=True),  #找到所有的翻页地址并且请求，$符号同理
             		Rule(LinkExtractor(allow=r'/article/details/\d+$'),callback="parse_article", follow=True), #找到所有的文章的 url 地址，并且请求，调用parase_article函数处理response
             		Rule(LinkExtractor(allow=r'/article/list/\d+$')，follow=True)  #找到所有的博客文章列表的翻页地址并且请求
             		)
             ```
             
          2. 注意点
          
             1. 用 `crawlspider` 模板创建一个爬虫:  `scrapy genspider -t crawl <爬虫名字> <all. _domain>`, 也可手动创建
             2. `CrawlSpider` 中不能有以 `parse` 命名的数据提取方法, `CrawlSpider`用 `parse ` 提取基础 url 
             3. Rule类很多参数
                1. LinkExtractor 对象,  `url` 的正则规则 筛选的 url的 `LinkExtractor` 对象
                2. callback(url的解析函数)
                3. follow , (response中提取的链接是否 要跟进)
             4. 不指定 callback 函数时，若 `follow` 为True, 满足该 rules 的 url 继续被请求
             5. 若多个Rule都满足某一个url, 会从rules中取第一个满足的进行操作
          
          3. `LinkExtractor` 连接提取器, 提取匹配正则表达式的 url 地址
          
             1. `LinkExtractor` 参数: 
                1. `allow`  #提取满足 r“正则表达式” 的URL. 空, 全部匹配。
                2. `deny`  #不提取满足 r“正则表达式”的URL, 优先级高于allow
                3. `allow_domains`   #被提取的链接的 domains
                4. `deny_domains`  #不被提取链接的domains。
                5. `restrict_xpaths`  #`xpath` 表达式，和allow共同过滤链接，提取 xpath 范围内的 url 地址
          
          4. 元组 `Rules` 的 元素是 `Rule` 类的实例
          
             1. `spiders.Rule`参数
                1. link_extractor   #一个LinkExtractor类的实例对象， 定义提取的链接
                2. callback  #指定回调函数
                3. follow  #布尔(oolean)值，从 response 提取的链接是否要跟进。如果 `callback` 为None, follow 默认设置为True，否则默认为False。
                4. process_links  #指定调用 spider 中哪个的函数, 从link_extractor中获取到链接列表时将会调用该函数, 过滤url。
                5. process_request  #指定调用 spider 中哪个的函数,  提取到每个request时 调用该函数, 过滤request

24. `scrapy` 模拟登陆 为了获取 `cookie`,  获取了cookie 才能爬取登陆后的页面

       1. requests 模拟登陆方法:

          1. 方法1, 直接携带 cookies 请求页面
          2. 方法2, 找接口发送post请求存储cookie

       2. selenium 模拟登陆: 找到 `input` 标签, 输入文字, 点击登录

       3. scrapy 模拟登陆 两个方法

          1. 方法1, 直接携带 `cookie`
          2. 方法2, 找到发送 post 请求的 url , 带上信息,发送请求

       4. scrapy模拟登陆 之 携带cookie, 应用场景

          1. `cookie` 过期时间很长，常见于一些不规范的网站

          2. cookie过期之前 搜到数据

          3. 配合其他程序. 如, 首先  `selenium` 把登陆之后的 `cookie` 获取到保存到本地，之后 scrapy 读取本地 cookie 后再发送请求

          4. 携带cookie登录之 前: scrapy. Spider部分源码

             1. ```
                from scrapy.http import Request
                class Spider(object_ref):
                    """Base class for scrapy spiders. All spiders must inherit from this
                    class.
                    """
                	......
                	
                	def start_requests(self):   #自动调用
                		cls = self.__class__
                		if method_is_overridden(cls, Spider, 'make_requests_from_url'):
                			warnings.warn(
                				"Spider.make_requests_from_url method is deprecated;it
                				"won't be called in future Scrapy releases. Please "
                				"override Spider.start_requests method instead (see %S .%s)."% (
                     				cls.__ module__, cls.__name__
                                ),
                            )
                            for url in self.start_urls:
                            	yield self.make_requests_from_url(url)
                        else:
                        	for url in self.start_urls:
                        		yield Request(url, dont_filter=True)
             	def make_requests_from_url(self, url):
                		"""This method is deprecated."""
                		return Request(url, dont_filter=True)
                ```
                
             2. spider 的 `start_urls= []` 的响应response交给 parse()

             3. 与 CrawlSpider 的 start_urls 交给 parse_start_url() 处理区别

          5. scrapy模拟登陆之 携带cookie : renren.py 爬虫

             1. ```
                class RenrenspiderSpider(scrapy.Spider):
                	name = 'renrenspider'
                	allowed_domains = ['renren.com'] 
                	cookies = dict(  #自定义的cookies，字典形式
                		anonymid= "j3jxk555-nrn0wh"，
                		......
                		wp_fold=0
                	)
                	
                	def start_requests(self):  #重写start_requests函数， 指定start_urls的处理方式
                		start_urls = 'http://www.renren.com/'
                		yield scrapy.Request(start_urls, callback=self.parse, cookies=self.cookies)  #指定callback函数，同时携带cookie
                		
                	def parse(self, response):
                		print(re.findall(r'毛**'，response.body.decode()))
                		my_profile_url = "http://www.renren.com/327550029/profile"
                		yield scrapy.Request(my_profile_url, callback=self.parse_profile)
                        
                    def parse_profile(self, response):
                		print('*' * 100)
             		print(re.findall(r'毛**'，response.body.decode()))
             
          2. cookie 在不同的解析函数之间 传递, 在settings.py 中设置

             1. ```
                   # Disable cookies(enabled by default)
                   #COOKIES_ENABLED = False  #cookie在 setting 中默认是开启的
                   COOKIES_DEBUG = True  #cookie在不同的解析函数中传递，前提也是COOKIES_ENABLED为True
                   ```
                
             1. 终端效果如下: `[scrapy . downloadermi ddLewares . cookies] DEBUG: Sending cookies to: GET http: / /zhi bo. renren. com/top> Cookie: anonymid=j3j xk555-nrn0wh;. r01_ =1; wp=0;`

25. scrapy模拟登陆之 发送post请求: Github.py 爬虫

       1.     ```	
              class GithubSpider(scrapy.Spider):
                 	name = 'github' 
                  allowed_domains = ['github.com']
              	start_urls = ['https://github. com/login']  #请求首页， 为了获取登录参数
               	headers = {
               		"Accept":"*/*",
               		"Accept-Language":"en-US, en;q=0.8, zh- TW;q=0. 6, zh;q=0.4",
               		
               	def parse(self, response):
               		print(response.url)
               		utf8 = response.xpath("//form[@action='/session']/div[1]/input[1]/@value").extract.first()
               		authenticity_token = response.xpath("//form[@action='/session']/div[1]/input[2]/@value").extract_first()
                		return scrapy.FormRequest("https://github.com/session", 
               				headers=self.headers #可以在spider中定义,也可以在setting中定义
               				formdata=dict(
               					commit="Sign in",
                                   utf8=utf8,
                                   authenticity_token=authenticity_token,
                                   login="user_name",
                                   password="password"
                               ),
                               callback=self.after_login  #登录之后的回调函数
                               )
                               
                   def after_login(self, response):
                   	print(response.url, "*"*100， response.status)
            ```
            
            1. scrapy.Request 发送的get请求,  scrapy.FormRequest 发送post请求
            3. 注意: github的部分页面只允许一处登录 ,比如https://github.com/settings/security

26. scrapy模拟登陆之 自动登录:  Renren.py 爬虫

       1.     ```
              class RenrenspiderSpider(scrapy.Spider):
                  name = 'renrenspider'
                  allowed_domains = ["renren.com"]
                  start_urls = [
                      "http: / /www. renren. com/",
                  ]
              
                  def parse(self, response):
                      yield scrapy.FormRequest.from_response(
                          response, # 自动从该响应中找到 form 表单进行登录
                          formdata = {"email": "user. name", "password": "password"},
                          callback = self.parse_page
                      )
              
                  def parse_page(self, response):
                      print(response.url, "*" * 100, response.status )
                      print('*' * 100)
                      print(re.findall(r'user_name', response.body.decode()))  # response.body是bytes类型，需要decode为str
              
       2. from_ response的意思就是从响应中找到 form 表单进行登录    



##### 下载中间件:

1.  middlewares.py的类  MyspiderDownloaderMiddleware 和 MyspiderSpiderMiddleware

   1. 要在 `settings` 设置 开启. 和 pipeline一样

   2. class MyspiderDownloaderMiddleware默认方法

      1. `process_request(self, request, spider):`	#每个request 通过下载中间件时, 调用此方法
      2. `process_response(self, response, spider):`  #响应传给引擎时 调用此方法

   3. 示例

      1. ```
         class RandomUserAgent(object):
         	def process_request(self, request， spider):
         		useragent = random.choice(USER_AGENTS)
         		#添加指定的UA，给request的headers赋值即可
         		request.headers["User-Agent"] = useragent  
         
         class ProxyMiddleware( obj ect):
         	def process_request(self, request, spider):
         		#添加指定的代理
         		reqeust.meta["proxy"]="http://124.115.126.76:808"  #要在request的meta信息中添加proxy字段. 代理形式: 协议+ip地址+端口
         ```

      

##### scrapy起始请求url的两种写法

1. 一种简写: 常量start_urls，加parse(). 另一种: 直接定义 start_requests()

2. ```
   import scrapy
   class simpleUrl(scrapy.Spider):
       name = "simpleUrl"
       
       #简写方法:
       start_urls = [  #一种写法，无需定义start_requests方法
           'http://lab.scrapyd.cn/page/1/',
           'http://lab.scrapyd.cn/page/2/',
       ]
       
       #简写初始url，解析函数名必须为 parse
       def parse(self, response):
           page = response.url.split("/")[-2]
           filename = 'mingyan-%s.html' % page
           with open(filename, 'wb') as f:
               f.write(response.body)
           self.log('保存文件: %s' % filename)
   
       # 另外一种初始链接写法
       # def start_requests(self):
       #     urls = [ #爬取的链接由此方法通过下面链接爬取页面
       #         'http://lab.scrapyd.cn/page/1/',
       #         'http://lab.scrapyd.cn/page/2/',
       #     ]
       #     for url in urls:
       #         yield scrapy.Request(url=url, callback=self.parse)
       
       
       #def parse(self, response):
       #    page = response.url.split("/")[-2]
       #    filename = 'mingyan-%s.html' % page
       #    with open(filename, 'wb') as f:
       #        f.write(response.body)
       #    self.log('保存文件: %s' % filename)   
   
   ```



### 5 Scrapy实战项目

##### 图片管道教程1

1. ImagesPipeline是Scrapy 的一个pipe管道组件的一个插件模块，封装了对图片爬取的处理. 图片链接丢给这个管道，其他的不用我们管，包括图片的命名和后缀名处理，图片存储地址（需要修改默认值），图片长宽处理等。

2. ImagesPipeline的源码中两个函数

   1. get_media_requests(): 下载图片，调用item中图片的链接（图片链接存在item中），调用Request函数下载，默认的函数写死了，要进行一些处理才可以拿到图片链接

      ```
      def get_media_requests(self, item, info):
          return [Request(x) for x in item.get(self.images_urls_field, [])]
      ```

   2. file_path,定义图片的存储路径， 把图片存储到我们想存储的地方

      ```
      def file_path(self, request, response=None, info=None):
          image_guid = hashlib.sha1(to_bytes(request.url)).hexdigest()
          return 'full/%s.jpg' % (image_guid)
      ```

##### 案例:

案例:百度图片爬取

1. URL分析:  打开百度图片：https://image.baidu.com/ ,  随便搜一点东西

   打开检查，Network，ctrl+R刷新，向下翻几页，可以看到百度是通过ajax的xhr来传递异步数据的. 

   name栏下选一个xhr，复制Request URL. 请求参数蛮多的 . 浏览器输入一段url，分析后得出图片的url为: "thumbURL":"https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=48630087,4029563252&fm=26&gp=0.jpg".  ctrl+F搜索thumbURL，得出一个xhr 提供30个图片的链接.  分析URL的请求参数，明显的看出queryWord：星际就是我们搜索的内容. 接着分析，word也有一个搜索内容，然后是pn：30，这个pn就是页码数，经过观察可以发现第一页为30，第二页为60，依次+30。http协议不允许传递中文

2. 程序设计: 打开一个终端，创建scrapy项目(Linux打开Terminor,window打开cmd), 输入:

   ```
   scrapy startproject baiduimage
   
   cd baiduimage
   
   scrapy genspider image image.baidu.com
   ```

   使用PyChram编辑器打开项目, 项目结构显示

   

3. :几个坑点

   images.py:   

   转义（http协议不支持中文），调用urllib提供的parse.quote即可转义

   ```
   word_origin = input("请输入搜索关键字：")
   word = parse.quote(word_origin)
   ```

   正则匹配，拿到thumbURL图片链接

   ```
   regex = '"thumbURL":"(.*?)"'
   pattern = re.compile(regex, re.S)
   links = pattern.findall(response.text)
   ```

   ````
   # -*- coding: utf-8 -*-
   import scrapy
   import json
   import re
   from urllib import parse
   from ..items import BaiduimageItem
    
    
   class ImagesSpider(scrapy.Spider):
       name = 'images'
       allowed_domains = ['image.baidu.com']
       word_origin = input("请输入搜索关键字：")
       word = parse.quote(word_origin)
       url = "https://image.baidu.com/search/acjson?tn=resultjson_com&ipn=rj&is=&fp=result&queryWord=" + word + "&cl=2&lm=-1&ie=utf-8&oe=utf-8&adpicid=&st=-1&z=&ic=0&hd=&latest=&copyright=&word=" + word + "&s=&se=&tab=&width=&height=&face=0&istype=2&qc=&nc=1&fr=&expermode=&force=&pn={}&rn=30&gsm=1e&1586660816262="
    
       def start_requests(self):
           #页数可调整，开始页30,每次递增30
           for pn in range(30,151,30):
               url=self.url.format(pn)
               yield scrapy.Request(url=url,callback=self.parse)
    
       def parse(self, response):
           #正则匹配符合条件的thumbURL
           regex = '"thumbURL":"(.*?)"'
           pattern = re.compile(regex, re.S)
           links = pattern.findall(response.text)
           item=BaiduimageItem()
           #将搜索内容赋值给item,创建文件夹会用到
           item["word"]=self.word_origin
           for i in links:
               item["link"]=i
    
               yield item
   ````

4. pipelines.py

   class BaiduimagePipeline继承了scrapy提供的ImagesPipeline图片处理管道

   将item中的word参数复制给了一个全局变量，目的是传递给file_path。也可以使用scrapy.Request(meta={})去传递

   ```
   # -*- coding: utf-8 -*-
    
   # Define your item pipelines here
   #
   # Don't forget to add your pipeline to the ITEM_PIPELINES setting
   # See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html
    
   from scrapy.pipelines.images import ImagesPipeline
   import scrapy
   import hashlib
   from scrapy.utils.python import to_bytes
    
   class BaiduimagePipeline(ImagesPipeline):
    
       word=""
       
       # 重写ImagesPipeline里的get_media_requests函数
       # 原函数的请求不适应与本程序
       def get_media_requests(self, item, info):
           self.word=item['word']
           yield scrapy.Request(url=item['link'])
       
       # 重写ImagesPipeline里的file_path函数
       # 原函数return 'full/%s.jpg' % (image_guid) 
       # 我们将其改为自己想存放的路径地址
       def file_path(self, request, response=None, info=None):
           image_guid = hashlib.sha1(to_bytes(request.url)).hexdigest()
           return self.word + '/%s.jpg' % (image_guid)
   ```

5. settings.py

   在settings.py里IMAGES_STORE是图片存储的路径

   ```
   BOT_NAME = 'baiduimage'
    
   SPIDER_MODULES = ['baiduimage.spiders']
   NEWSPIDER_MODULE = 'baiduimage.spiders'
    
   ROBOTSTXT_OBEY = False
   DOWNLOAD_DELAY = 0.5
    
   DEFAULT_REQUEST_HEADERS = {
     'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
     'Accept-Language': 'en',
     'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.116 Safari/537.36'
   }
    
   ITEM_PIPELINES = {
      'baiduimage.pipelines.BaiduimagePipeline': 300,
   }
    
    
   IMAGES_STORE= 'D:\\图片\\'
   ```

   ImagesPipeline中的from_setting会调用 IMAGES_STORE，store_uri=settings['IMAGES_STORE']

6. items.py   两个参数，一个图片链接，一个搜索内容

   ```
   # -*- coding: utf-8 -*-
    
   # Define here the models for your scraped items
   #
   # See documentation in:
   # https://docs.scrapy.org/en/latest/topics/items.html
    
   import scrapy
    
    
   class BaiduimageItem(scrapy.Item):
       # define the fields for your item here like:
       # name = scrapy.Field()
       link=scrapy.Field()
       word=scrapy.Field()
       pass

7. 执行程序与效果预览

   ```
   from scrapy import cmdline
    
   cmdline.execute('scrapy crawl images'.split()) 
   ```

   

##### 图片管道教程2

1. ImagesPipeline:  将img的src属性值从原网页中解析出，封装到item、然后提交给管道，管道就会自动对图片的src发送请求、获取图片的二进制数据，并进行持久化存储

2. ImagesPipeline工作流程: 

   1. 爬虫文件中解析出图片的src,封装到item中，把item提交给管道
   2. pipelines.py中重新定义一个管道类（原来的要删掉），该类继承自ImagesPipeline. 新定义的管道类中要重写三个函数：
      1. get_media_requests(self, item, info) 用于对src发送请求
      2. file_path(self,requst,response=None,info=None) 返回图片的名称（名称用于图片存储路径）
      3. item_completed(self, results, item, info) 将item传递给下一个即将被执行的管道

3. settings.py中添加 IMAGES_STORE = 'XXX',  如上一步返回的名字是wyb.jpg, 那么这张照片在本地的存储位置是xxx/wyb.jpg.  如果第二步中自定义的管道类与之前的管道类的名字不一样，那么还需要在settings.py中把管道的名字改成自定义的那个

4. 代码实现

   1. 爬虫代码, 提取爬取的内容

      ```
      import scrapy
      from imgsPro.items import ImgsproItem
      
      class ImgSpiderSpider(scrapy.Spider):
          name = 'img_spider'
          start_urls = ['http://sc.chinaz.com/tupian/']
      
          def parse(self, response):
              info_nodes = response.xpath('//*[@id="container"]/div')
              for node in info_nodes:
                  item = ImgsproItem()
                  #解析得到图片的下载地址
                  src = 'http:'+node.xpath('./div/a/img/@src2').extract_first()
                  #封装到item中
                  item['src'] = src
                  yield item

   2. item文件中的代码, 定义要存储的内容

      ```
      import scrapy
      class ImgsproItem(scrapy.Item):
          src = scrapy.Field()
      ```

   3. pipelines文件中的代码

      ```
      import  scrapy
      from  scrapy.pipelines.images import ImagesPipeline
      class imgsPipeLine(ImagesPipeline):
          #向scrapy engine传递图片的url发送请求
          def get_media_requests(self, item, info):
              yield scrapy.Request(item['src'])
      
          #指定图片存储的名字
          def file_path(self,requst,response=None,info=None):
              #这里的request就是get_media_requsts里的scrapy.Request
              imgName = requst.url.split('/')[-1]
              return imgName
      
          def item_completed(self, results, item, info):
              return item #返回给下一个即将被执行的管道类
      ```

   4. settings.py文件中

      ```
      #修改ITEM_PIPELINES
      ITEM_PIPELINES = {
         'imgsPro.pipelines.imgsPipeLine': 300,
      }
      #增加IMAGE_STORE
      IMAGES_STORE = './imgs'

   

5. 实现方式

   1. 方式1: 自定义pipeline，优势:可重写ImagePipeline类的实现方法；
   2. 方式2: 直接用ImagePipeline类，简单但灵活；

6. 方式1:

   1. item.py文件：定义item字段

      ```
      import scrapy 
       
      class PicPhotoPeopleItem(scrapy.Item):
          # define the fields for your item here like:
          # name = scrapy.Field()
          # 存放url的下载地址
          image_urls = scrapy.Field()
          # 图片下载路径、url和校验码等信息（图片全部下载完成后将信息保存在images中）
          images = scrapy.Field()
          # 图片的本地保存地址
          image_paths = scrapy.Field()
      ```

   2. spider.py文件：编写爬虫文件，解析源码，得到图片的url下载路径

      ```
      # -*- coding: utf-8 -*-
      import scrapy
      from ..items import PicPhotoPeopleItem
       
       
      class PeopleSpider(scrapy.Spider):
          name = 'people'
          allowed_domains = ['699pic.com']
          start_urls = ['http://699pic.com/people.html']
          base_url = 'http://699pic.com/'
       
          def parse(self, response):
              # 获取全部的图片地址
              list_imgs = response.xpath('//div[@class="swipeboxEx"]/div[@class="list"]//img/@data-original').extract()
              if list_imgs:
                  item = PicPhotoPeopleItem()
                  item['image_urls'] = list_imgs  # 保存到item的image_urls里
                  yield item  # 返回item给pipeline文件处理
       
              # 获取下一页url
              next_url = response.xpath('//a[@class="downPage"]/@href').extract()[0] if len(response.xpath('//a[@class="downPage"]/@href')) else None
              if next_url:
                  yield scrapy.Request(url=self.base_url+ next_url, callback=self.parse)
      ```

   3. pipelines.py文件：处理item，调用调度器和下载器下载图片。重写ImagesPileline类的方法

      ```
      from scrapy.pipelines.images import ImagesPipeline
      from scrapy.exceptions import DropItem
      from scrapy.http import Request
       
       
      class PicPhotoPeoplePipeline(ImagesPipeline):
          def file_path(self, request, response=None, info=None):
              """
              重写ImagesPipeline类的file_path方法
              实现：下载的图片以校验码命名，实现保持原有图片命名
              :return: 图片路径
              """
              #取原url的图片命名
              image_guid = request.url.split('/')[-1]   
              return 'full/%s' % (image_guid)
       
          def get_media_requests(self, item, info):
              """
              遍历image_urls里的每一个url，调用调度器和下载器，下载图片
              :return: Request对象
              图片下载完毕后，图片的处理结果以二元组的方式返回给item_completed()函数
              """
              for image_url in item['image_urls']:
                  yield Request(image_url)
       
          def item_completed(self, results, item, info):
              """
              将图片的本地路径赋值给item['image_paths']
              results:下载结果，二元组定义：(success, image_info_or_failure)。
              success:图片是否下载成功；第二个元素是一个字典。
              如果success=true，image_info_or_error词典包含以下键值对: url：原始URL, path：本地存储路径, checksum：校验码
              :item:
              :info:
              失败则包含一些出错信息。
              :return:
              """
              image_paths = [x['path'] for ok, x in results if ok]
              if not image_paths:
                  raise DropItem("Item contains no images")   # 如果没有路径则抛出异常
              item['image_paths'] = image_paths
              return item
      ```

   4. settings.py文件：配置setting文件。

      ```
      import os
      # 配置数据保存路径，为当前工程目录下的 images 目录中
      project_dir = os.path.abspath(os.path.dirname(__file__))
      IMAGES_STORE = os.path.join(project_dir, 'images')
      # 过期天数
      IMAGES_EXPIRES = 90  # 90天内抓取的都不会被重抓
       
      # IMAGES_MIN_HEIGHT = 100  # 图片的最小高度
      # IMAGES_MIN_WIDTH = 100   # 图片的最小宽度
      ITEM_PIPELINES={
       'pic_photo_people.pipelines.PicPhotoPeoplePipeline': 300,
      }
      ```

7. 方式2

   1. item.py和spider.py同上；pipeline.py此处不需要自己编写，直接用现成的ImagePileline；

   2. 修改settings.py文件即可，代码如下

      ```
      import os
      IMAGES_URLS_FIELD = 'image_urls' # 要保存的字段，即在 Item 类中的字段名为 image_urls
       
      # 配置数据保存路径，为当前工程目录下的 images 目录中
      project_dir = os.path.abspath(os.path.dirname(__file__))
      IMAGES_STORE = os.path.join(project_dir, 'images')
      # 过期天数
      IMAGES_EXPIRES = 90  # 90天内抓取的都不会被重抓
       
      # IMAGES_MIN_HEIGHT = 100  # 图片的最小高度
      # IMAGES_MIN_WIDTH = 100   # 图片的最小宽度
       
      ITEM_PIPELINES = {
         'scrapy.pipelines.images.ImagesPipeline': 1,
      }

#### 5.1 (案例一)手机App抓包爬虫

1. items.py: 项目分析, 确定项目目标(想要的数据)

   1. ```
       class DouyuspiderItem(scrapy.Item):
         name = scrapy.Field()# 存储照片的名字
         imagesUrls = scrapy.Field()# 照片的url路径
         imagesPath = scrapy.Field()# 照片保存在本地的路径

2. scrapy genspider douyu "douyu.com"  创建 spiders/douyu.py

   1. ```
      #自动创建的文件
      import scrapy
      
      class DouyuSpider(scrapy.Spider):
          name = 'douyu'
          allowed_domains = ['douyu.com']
          start_urls = ['http://douyu.com/']
      
          def parse(self, response):
              pass
      
      
      #修改后的douyu爬虫内容:
      import scrapy
      import json
      from douyuSpider.items import DouyuspiderItem
      
      class DouyuSpider(scrapy.Spider):
          name = "douyu"
          allowd_domains = ["http://douyu.com"]
      
          offset = 0
          url = "https://www.douyu.com/directory/all"
          #url = "http://douyu.com/api/v1/getVerticalRoom?limit=20&offset="
          start_urls = [url + str(offset)]
      	
      	#解析图片的url,向scrapy engine返回Item和 下一页请求
          def parse(self, response):
              #从response中提取data段数据集合.  response.text是json格式
              data = json.loads(response.text)["data"]
          
              for each in data:
              	#创建要保存的数据类
                  item = DouyuspiderItem()
                  item["name"] = each["nickname"]
                  item["imagesUrls"] = each["vertical_src"]
          		
          		#向scrapy engine返回Item
                  yield item
          
              self.offset += 20
              #向scrapy engine 下一页请求
              yield scrapy.Request(self.url + str(self.offset), callback = self.parse)
      ```

      

3. 设置setting.py

   1. ```
      ITEM_PIPELINES = {'douyuSpider.pipelines.ImagesPipeline': 1}
      
      # Images 存放位置， 在pipelines.py里调用
      IMAGES_STORE = "./Images"
      
      # user-agent
      USER_AGENT = 'DYZB/2.290 (iPhone; iOS 9.3.4; Scale/2.00)'
      ```

4. pipelines.py,   处理item数据

   1. ```
      import scrapy
      import os
      from scrapy.pipelines.images import ImagesPipeline
      
      #导入方法get_project_settings(),返回settings.py模块
      from scrapy.utils.project import get_project_settings
      
      class ImagesPipeline(ImagesPipeline):
          IMAGES_STORE = get_project_settings().get("IMAGES_STORE")
      
          def get_media_requests(self, item, info):
          	'''
          	get_media_requests 为每一个图片url生成一个Request对象，
          	这个方法的输出到item_completed()的参数results中，results是一个元组，
          	每个元组包括(success, imageinfor_failure)。
          	如果success=true，imageinfor_failure是一个字典，包括url/path/checksum三个key
          	'''
              image_url = item["imagesUrls"]
              #向scrapy engine返回图片请求
              yield scrapy.Request(image_url)
      
          def item_completed(self, results, item, info):
              '''
              固定写法，获取图片路径，同时判断这个路径是否正确，如果正确，就放到imagespath里
              '''
              
              image_path = [x["path"] for ok, x in results if ok]
      
              os.rename(self.IMAGES_STORE + "/" + image_path[0], self.IMAGES_STORE + "/" + item["name"] + ".jpg")
              item["imagesPath"] = self.IMAGES_STORE + "/" + item["name"]
      
              return item
      ```
      
   2. 分析: 继承父类ImagePipeline，定义自己的pipeline

      重写父类的两个方法: 一个 get_midia_requests(), 构建request请求，然后交由下载器下载，第二个: item_completed(), 图片下载完毕后，默认调用之， 只注意第二个results参数，一个二元元祖，第一个值是一个布尔类型的值，判断图片是否下载成功，第二个值是图片的下载信息

      ```
      from scrapy.pipelines.images import ImagesPipeline
      from scrapy.exceptions import DropItem
      from scrapy import Request
      
      class DownLoadImgPipeline(ImagesPipeline):
      
          def get_media_requests(self, item, info):
              for url in item['image_urls']:
              	#向scrapy engine传递生成的图片请求
                  yield Request(url)
      
      
          def item_completed(self, results, item, info):
              item['images'] = [x for ok, x in results if ok]
              return item
      ```
      

5. 最后设置settings.py

   IMAGES_STORE 指定图片的下载路径，通过os模块指定为当前项目下。item中两个字段是ImagesPipeline中默认设置的字段，不需要通过IMAGES_URLS_FIELD，IMAGES_RESULTS_FIELD字段指定，在ITEM_PIPELINE中配置pipeline

   ```
   project_dir = os.path.abspath(os.path.dirname(__file__))
   IMAGES_STORE = os.path.join(project_dir, 'images')
   ITEM_PIPELINES = {
      # 'MeizhiTu.pipelines.MeizhituPipeline': 300,
      'MeizhiTu.pipelines.DownLoadImgPipeline': 300,
   }

6. 在项目根目录下新建main.py文件,用于调试

   1. ```
      from scrapy import cmdline
      cmdline.execute('scrapy crawl douyu'.split())
      ```

7. 执行程序, pycharm的terminal中输入`python main.py`

8. 小结:  5, 6步骤 相当于 `scrapy crawl douyu`

   


#### 5.2 (案例二)阳光热线问政平台爬虫

1. Spider版本
   1. 项目分析: 爬取投诉帖子的编号、帖子的url、帖子的标题，和帖子里的内容

      1. https://wz.sun0769.com/political/index/politicsNewest?id=1&page=4

   2. 搭建项目框架

      1. `scrapy startproject mySpider`  #创建项目
      2. cd mySpider  #进入项目根目录
      3. scrapy genspider sun wz.sun0769.com  #创建爬虫

   3. items.py: 爬取数据类Item 设置

      2. ```
         import scrapy
         
         class DongguanItem(scrapy.Item):
             # 每个帖子的标题
             title = scrapy.Field()
             # 每个帖子的编号
             number = scrapy.Field()
             # 每个帖子的文字内容
             content = scrapy.Field()
             # 每个帖子的url
             url = scrapy.Field()
         ```
      
   4. spiders/sun.py: 编辑爬虫

      2. ```
         import scrapy
         # import items.DongguanItem
         
         class SunSpider(scrapy.Spider):
             name = 'sun'
             allowed_domains = ['wz.sun0769.com']
         
             url = 'https://wz.sun0769.com/political/index/politicsNewest?id=1&page='
             offset = 0
         
             start_urls = [url + str(offset)]
         	#start_urls自动传给父类start_request处理, start_urls的响应response交parse处理
         	
             def parse(self, response):
                 # 取出每个页面里帖子链接列表        	 #response.xpath("//div[@class='greyframe']/table//td/a[@class='news14']/@href").extract()
         
                 links = response.xpath("//div[@class='width-12']//ul[@class='title-state-ul']/li[@='clear']//a[@class='color-hover']/@href").extract()
                 # 迭代
                 for link in links:
                 	#向scrapy engine发送每个帖子的请求，回调函数parse_item
                     yield scrapy.Request(link, callback = self.parse_item)
                 # 设置页码终止条件，并且每次发送新的页面请求调用parse方法处理
                 if self.offset <= 71130:
                     self.offset += 30
                     #向scrapy engine发送每个帖子的请求，回调函数parse
                     yield scrapy.Request(self.url + str(self.offset), callback = self.parse)
         
             # 处理每个帖子里
             def parse_item(self, response):
                 item = DongguanItem()
                 # 标题
                 #/html/body/div[3]/div[2]/div[2]/p
                 item['title'] = response.xpath('//div[contains(@class, "width-12 mr-three")]//p/text()').extract()[0]
         
                 # 编号  /html/body/div[3]/div[2]/div[2]/div[1]/span[4]
                 item['number'] = response.xpath('//div[contains(@class, "width-12 mr-three")]//span[4]/text()').extract()[0].split(' ')[-1].split(":")[1]
                 # item['number'] = item['title'].split(' ')[-1].split(":")[-1]
         
                 # 文字内容，默认先取出有图片情况下的文字内容列表
                 #/html/body/div[3]/div[2]/div[2]/div[4]/div/pre
                 content = response.xpath('/html/body/div[3]/div[2]/div[2]/div[4]/div/pre/text()').extract()
                 # 如果没有内容，则取出没有图片情况下的文字内容列表
                 if len(content) == 0:
                     content = response.xpath('/html/body/div[3]/div[2]/div[2]/div[4]/div/pre/text()').extract()
                     # content为列表，通过join方法拼接为字符串，并去除首尾空格
                     item['content'] = "".join(content).strip()
                 else:
                     item['content'] = "".join(content).strip()
         ```
      
   5. 设置 pipelines.py  #如CrawlSpider 版本中的 pipelines.py
   
   6. 设置 settings.py  #如CrawlSpider 版本中的 settings.py
   
   7. 运行程序, pycharm的 Terminal中, `scrapy crawl sun`
   
2. CrawlSpider 版本 (这个程序 跑通 了)

   1. cd mySpider, 进入项目根目录 mySpider. 创建 sun.py爬虫: `scrapy genspider -t crawl sun wz.sun0769.com`

   2. 爬虫: sun.py

      1. ```
         #自动生成的爬虫爬虫文件
         import scrapy
         from scrapy.linkextractors import LinkExtractor
         from scrapy.spiders import CrawlSpider, Rule
         
         
         class SunSpider(CrawlSpider):
             name = 'sun'
             allowed_domains = ['wz.sun0769.com']
             start_urls = ['http://wz.sun0769.com/']
         
             rules = (
                 Rule(LinkExtractor(allow=r'Items/'), callback='parse_item', follow=True),
             )
         
             #解析response, 返回要提取的数据item
             def parse_item(self, response):
                 item = {}
                 #item['domain_id'] = response.xpath('//input[@id="sid"]/@value').get()
                 #item['name'] = response.xpath('//div[@id="name"]').get()
                 #item['description'] = response.xpath('//div[@id="description"]').get()
                 return item
          
         ###----------------------------------------------------
          
         #手动修改的爬虫文件
         rom scrapy.linkextractors import LinkExtractor
         from scrapy.spiders import CrawlSpider, Rule
         import time
         
         class SunSpider(CrawlSpider):
             name = 'sun'
             allowed_domains = ['wz.sun0769.com']
         
             #start_urls = ['http://wz.sun0769.com/index.php/question/questionType?type=4&page=']
         
             start_urls = ['https://wz.sun0769.com/political/index/politicsNewest?id=1&page=']
             #start_urls交给父类的start_request处理
         
             # 每一页的匹配规则
             #start_urls中的response交提取器提取新的url
             
             pagelink = LinkExtractor(allow=('id=1'))
             contentlink = LinkExtractor(allow=r'/political/politics/index?id=\d+')
         
             rules = [
                 #pagelink为特殊情况，调用deal_links()处理每个页面里的链接
                 Rule(pagelink, process_links="deal_links", follow=True),
                 
                 #生成contentlink的请求, 指定回调函数parse_item
                 Rule(contentlink, callback='parse_item')
             ]
         
             #要重新处理每个页面里的链接，将链接里的‘Type&type=4?page=xxx’替换为‘Type?type=4&page=xxx’（或者是Type&page=xxx?type=4’替换为‘Type?page=xxx&type=4’），否则无法发送这个链接
             def deal_links(self, links):
                 for link in links:
                     link.url = link.url.replace("?", "&").replace("Type&", "Type?")
                     print (link.url)
                 return links
         
             def parse_item(self, response):
                 print(response.url)
                 item = DongguanItem()
                 # 标题
                 item['title'] = response.xpath('//div[contains(@class, "pagecenter p3")]//strong/text()').extract()[0]
         
                 # 编号
                 item['number'] = item['title'].split(' ')[-1].split(":")[-1]
         
                 # 文字内容，默认先取出有图片情况下的文字内容列表
                 content = response.xpath('//div[@class="contentext"]/text()').extract()
                 # 如果没有内容，则取出没有图片情况下的文字内容列表
                 if len(content) == 0:
                     content = response.xpath('//div[@class="c1 text14_2"]/text()').extract()
                     # content为列表，通过join方法拼接为字符串，并去除首尾空格
                     item['content'] = "".join(content).strip()
                 else:
                     item['content'] = "".join(content).strip()
         
                 # 链接
                 item['url'] = response.url
         
                 yield item
         ```
   
   4. pipelines.py
   
         1. ```
               # -*- coding: utf-8 -*-
               
               # 文件处理类库，可以指定编码格式
               import codecs
               import json
               
               class JsonWriterPipeline(object):
               
                   def __init__(self):
                       # 创建一个只写文件，指定文本编码格式为utf-8
                       self.filename = codecs.open('sun.json', 'w', encoding='utf-8')
               	
               	#处理scrapy engine传递过来的数据item, 并保存数据到指定文件
                   def process_item(self, item, spider):
                       content = json.dumps(dict(item), ensure_ascii=False) + "\n"
                       self.filename.write(content)
                       return item
               
                   def spider_closed(self, spider):
                       self.file.close()
               ```
   
   5. settings.py
   
         1. ```
               ITEM_PIPELINES = {
                   'mySpider.pipelines.JsonWriterPipeline': 300,  #项目根名, pipelines.py名,JsonWriterPipeline 类名 
               }
               
               # 日志文件名和处理等级
               LOG_FILE = "dg.log"
               LOG_LEVEL = "DEBUG"
   
   6. 项目根目录下新建main.py文件,用于调试

         1. ```
               from scrapy import cmdline
               cmdline.execute('scrapy crawl sun'.split())  #输入的字符串split用空格
   
   7. 执行程序: pycharm的Termina中, cd 进入项目根目录, `python main.py`  执行程序


#### 5.3 (案例三)新浪网分类资讯爬虫 

1. 项目分析

   1. 任务: 要提取的数据
   2. 创建项目:  `scrapy startproject Sina`
   3. `cd Sina`  进入项目根目录
   4. 创建爬虫:  `scrapy genspider sina sina.com.cn`
   
2. item.py,  指定要提取的数据的字段

   1. ```
      #自动创建的 items.py
      import scrapy
      
      class SinaItem(scrapy.Item):
          # define the fields for your item here like:
          # name = scrapy.Field()
          pass
          
      #---------------------
      修改 items.py
      import scrapy
      import sys
      reload(sys)
      sys.setdefaultencoding("utf-8")
      
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
      ```

3. spiders/sina.py

   1. ```
      #自动产生的sina.py
      import scrapy
      
      
      class SinaSpider(scrapy.Spider):
          name = 'sina'
          allowed_domains = ['sina.com.cn']
          start_urls = ['http://sina.com.cn/']
      
          def parse(self, response):
              pass
              
      #--------------------------        
              
      #修改后的 sina.py
      # -*- coding: utf-8 -*-
      
      from Sina.items import SinaItem
      import scrapy
      import os
      
      import sys
      #reload(sys)   #python3 中这两个语句已经没有了
      #sys.setdefaultencoding("utf-8")
      
      
      class SinaSpider(scrapy.Spider):
          name= "sina"
          allowed_domains= ["sina.com.cn"]
          start_urls= [
             "http://news.sina.com.cn/guide/"
          ]
      
          def parse(self, response):
              items= []
              # 所有大类的url 和 标题  , list类型
              parentUrls = response.xpath('//div[@id=\"tab01\"]/div/h3/a/@href').extract()
              parentTitle = response.xpath("//div[@id=\"tab01\"]/div/h3/a/text()").extract()
      
              # 所有小类的ur 和 标题
              subUrls  = response.xpath('//div[@id=\"tab01\"]/div/ul/li/a/@href').extract()
              subTitle = response.xpath('//div[@id=\"tab01\"]/div/ul/li/a/text()').extract()
      
              #爬取所有大类
              for i in range(0, len(parentTitle)):
                  # 指定大类目录的路径和目录名
                  parentFilename = "./Data/" + parentTitle[i]
      
                  #如果目录不存在，则创建目录
                  if(not os.path.exists(parentFilename)):
                      os.makedirs(parentFilename)
      
                  # 爬取所有小类
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
      
                          # 存储 小类url、title和filename字段数据
                          item['subUrls'] = subUrls[j]
                          item['subTitle'] =subTitle[j]
                          item['subFilename'] = subFilename
      
                          items.append(item)
      
              #发送每个小类url的Request请求，得到Response连同包含meta数据 一同交给回调函数 second_parse 方法处理
              for item in items:
                  yield scrapy.Request( url = item['subUrls'], meta={'meta_1': item}, callback=self.second_parse) 
                  
      	
          #对于返回的小类的url，再进行递归请求
          def second_parse(self, response):
              ##meta在不同解析函数之间传递参数, 用response.meta提取上一个解析函数传来的数据
              meta_1= response.meta['meta_1']
      
              #取出小类里所有子链接
              sonUrls = response.xpath('//a/@href').extract()
      
              items= []
              for i in range(0, len(sonUrls)):
              
                  #检查每个链接是否以大类url开头、以.shtml结尾，如果是返回True
                  #sonUrls[i]是字符串, endswith()方法
                  if_belong = sonUrls[i].endswith('.shtml') and sonUrls[i].startswith(meta_1['parentUrls'])
      
                  #如果属于本大类，获取字段值放在同一个item下便于传输
                  if(if_belong):
                      item = SinaItem()
                      item['parentTitle'] =meta_1['parentTitle']
                      item['parentUrls'] =meta_1['parentUrls']
                      item['subUrls'] = meta_1['subUrls']
                      item['subTitle'] = meta_1['subTitle']
                      item['subFilename'] = meta_1['subFilename']
                      item['sonUrls'] = sonUrls[i]
                      items.append(item)
      
              #发送每个小类下子链接url的Request请求，得到Response后连同包含meta数据 一同交给回调函数 detail_parse 方法处理
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
      ```

      

4. pipelines.py

   1. ```
      #自动产生 pipelines.py
      # Define your item pipelines here
      #
      # Don't forget to add your pipeline to the ITEM_PIPELINES setting
      # See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html
      
      
      # useful for handling different item types with a single interface
      from itemadapter import ItemAdapter
      
      
      class SinaPipeline:
          def process_item(self, item, spider):
              return item
      
      #修改后 pipelines.py
      from scrapy import signals
      import sys
      #reload(sys)  #python3 中这两个语句已经没有了
      #sys.setdefaultencoding("utf-8")
      
      class SinaPipeline(object):
          def process_item(self, item, spider):
              sonUrls = item['sonUrls']
      
              # 文件名为子链接url中间部分，并将 / 替换为 _，保存为 .txt格式
              filename = sonUrls[7:-6].replace('/','_')
              filename += ".txt"
      
              fp = open(item['subFilename']+'/'+filename, 'w')
              fp.write(item['content'])
              fp.close()
      
              return item
      ```

5. settings.py

   1. ```
      #自动产生的 settings.py
      BOT_NAME = 'Sina'
      
      SPIDER_MODULES = ['Sina.spiders']
      NEWSPIDER_MODULE = 'Sina.spiders'
      ROBOTSTXT_OBEY = True
      
      
      #修改后的 settings.py
      BOT_NAME = 'Sina'
      
      SPIDER_MODULES = ['Sina.spiders']
      NEWSPIDER_MODULE = 'Sina.spiders'
      
      ITEM_PIPELINES = {
          'Sina.pipelines.SinaPipeline': 300,   #Sina项目根目录
      }
      
      LOG_LEVEL = 'DEBUG'
      ```

      

5. 项目根目录下新建main.py文件,用于调试

   1. ```
      from scrapy import cmdline
      cmdline.execute('scrapy crawl sina'.split())
      ```

6. 执行程序

   1. `python main.py`

   2. 执行后 文件结构

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

      

7. 小结: NameError: name 'reload' is not defined

   1. Python 2.X

      1. ```
         import sys
         reload(sys)
         sys.setdefaultencoding("utf-8")
         ```
   
   2. <= Python 3.3：

      1. ```
         import imp
         imp.reload(sys)
         ```
   
   3. 注意：
      1. Python 3 与 Python 2 有很大的区别，其中Python 3 系统默认使用的就是utf-8编码。
      2. 所以，对于使用的是Python 3 的情况，就不需要sys.setdefaultencoding("utf-8")这段代码。
      3. 最重要的是，Python 3 的 sys 库里面已经没有 setdefaultencoding() 函数了。




#### 5.4 (案例四)Cosplay图片下载器爬虫

备注: 没通

1. 创建工程:  `scrapy startproject Cosplay`

   2. cd Cosplay

   3. `scrapy genspider coser bcy.net`

   4. 目录结构

      ```
      .  #工作目录
      └─── Sosplay  #scrapy startproject mySpider创建的项目目录
          ├─── Sosplay  #项目的Python模块
          │	├─── __init__.py
          │	├─── items.py   
          │	├─── pipelines.py  
          │	├─── settings.py   
          │	└─── spiders  
          │		├─── __init__.py  #创建后就有的
          │       └─── coser.py #scrapy genspider coser bcy.net创建的,"bcy.net"加引号也行
          ├─── scrapy.cfg  #配置文件
          └─── main.py #项目Sina根目录下
      ```

2. items.py

   1. 修改后items.py

      ```
      import scrapy
      
      class CoserItem(scrapy.Item):
          url = scrapy.Field()
          name = scrapy.Field()
          info = scrapy.Field()
          image_urls = scrapy.Field()
          images = scrapy.Field()
      ```

      

3. spiders/coser.py

   1. ```
      #自动生成的爬虫模块
      import scrapy
      
      class CoserSpider(scrapy.Spider):
          name = 'coser'
          allowed_domains = ['bcy.net']
          start_urls = ['http://bcy.net/']
      
          def parse(self, response):
              pass
              
              
      #修改后的爬虫模块
      from scrapy.selector import Selector
      import scrapy
      from ..items import CoserItem
      
      
      class CoserSpider(scrapy.Spider):
          name = "coser"
          allowed_domains = ["bcy.net"]
          start_urls = (
              'http://bcy.net/cn125101',
              'http://bcy.net/cn126487',
              'http://bcy.net/cn126173'
          )
      
          def parse(self, response):
              sel = Selector(response)
      
              for link in sel.xpath("//ul[@class='js-articles l-works']/li[@class='l-work--big']/article[@class='work work--second-created']/h2[@class='work__title']/a/@href").extract():
                  link = 'http://bcy.net%s' % link
                  request = scrapy.Request(link, callback=self.parse_item)
                  yield request
      
          def parse_item(self, response):
              item = ItemLoader(item=CoserItem(), response=response)
              item.add_xpath('name', "//h1[@class='js-post-title']/text()")
              item.add_xpath('info', "//div[@class='post__info']/div[@class='post__type post__info-group']/span/text()")
              urls = item.get_xpath('//img[@class="detail_std detail_clickable"]/@src')
              urls = [url.replace('/w650', '') for url in urls]
              item.add_value('image_urls', urls)
              item.add_value('url', response.url)
      
              return item.load_item()
      ```

      

4. pipelines.py

   1. ```
      #修改后 pipelines.py
      import requests
      from . import settings
      import os
      
      
      class ImageDownloadPipeline(object):
          def process_item(self, item, spider):
              if 'image_urls' in item:
                  images = []
                  dir_path = '%s/%s' % (settings.IMAGES_STORE, spider.name)
      
                  if not os.path.exists(dir_path):
                      os.makedirs(dir_path)
                  for image_url in item['image_urls']:
                      us = image_url.split('/')[3:]
                      image_file_name = '_'.join(us)
                      file_path = '%s/%s' % (dir_path, image_file_name)
                      images.append(file_path)
                      if os.path.exists(file_path):
                          continue
      
                      with open(file_path, 'wb') as handle:
                          response = requests.get(image_url, stream=True)
                          for block in response.iter_content(1024):
                              if not block:
                                  break
      
                              handle.write(block)
      
                  item['images'] = images
              return item
      ```
      

5. settings.py

   1. ```
      ITEM_PIPELINES = {'Cosplay.pipelines.ImageDownloadPipeline': 1}
      
      IMAGES_STORE = '../Images'
      
      DOWNLOAD_DELAY = 0.25    # 250 ms of delay
      ```

      

6. 项目根目录下新建main.py文件,用于调试

   1. ```
      from scrapy import cmdline
      cmdline.execute('scrapy crawl coser'.split())
      ```
      
   2. ````
      .  #工作目录
      └─── Sosplay  #scrapy startproject mySpider创建的项目目录
          ├─── Sosplay  #项目的Python模块
          │	├─── __init__.py
          │	├─── items.py   
          │	├─── pipelines.py  
          │	├─── settings.py   
          │	└─── spiders  
          │		├─── __init__.py  #创建后就有的
          │       └─── coser.py #scrapy genspider coser bcy.net创建的,"bcy.net"加引号也行
          ├─── scrapy.cfg  #配置文件
          └─── main.py #项目Sina根目录下

   

7. 执行程序

   1. `python main.py`

8. [scrapy] DEBUG: Forbidden by robots.txt

   1. 修改方法：
      将setting改变ROBOTSTXT_OBEY为False


#### 5.5 (案例五)将数据保存在MongoDB中

备注: 能跑通

1. 新建项目

   1. 任务分析 : 爬取豆瓣电影top250[movie.douban.com/top250](movie.douban.com/top250)的电影数据, 保存在MongoDB中
   2. 创建工程 : `scrapy startproject Douban`
      2. cd Douban
   3. 创建爬虫 : `scrapy genspider douban movie.douban.com `
   
2. items.py

   1. ```
      import scrapy
      
      class DoubanspiderItem(scrapy.Item):
          # 电影标题
          title = scrapy.Field()
          # 电影评分
          score = scrapy.Field()
          # 电影信息
          content = scrapy.Field()
          # 简介
          info = scrapy.Field()
      ```

3. spiders/douban.py

   1. ```
      mport scrapy
      
      from Douban.items import DoubanspiderItem
      
      
      class DoubanSpider(scrapy.Spider):
          name = "douban"
          allowed_domains = ["movie.douban.com"]
          start = 0
          url = 'https://movie.douban.com/top250?start='
          end = '&filter='
          start_urls = [url + str(start) + end]
      
          def parse(self, response):
      
              item = DoubanspiderItem()
      
              movies = response.xpath("//div[@class=\'info\']")
      
              for each in movies:
                  title = each.xpath('div[@class="hd"]/a/span[@class="title"]/text()').extract()
                  content = each.xpath('div[@class="bd"]/p/text()').extract()
                  score = each.xpath('div[@class="bd"]/div[@class="star"]/span[@class="rating_num"]/text()').extract()
                  info = each.xpath('div[@class="bd"]/p[@class="quote"]/span/text()').extract()
      
                  item['title'] = title[0]
                  # 以;作为分隔，将content列表里所有元素合并成一个新的字符串
                  item['content'] = ';'.join(content)
                  item['score'] = score[0]
                  item['info'] = info[0]
                  # 提交item
      
                  yield item
      
              if self.start <= 225:
                  self.start += 25
                  yield scrapy.Request(self.url + str(self.start) + self.end, callback=self.parse)
      ```

4. pipelines.py

   1. ```
      # from scrapy.conf import settings #此句报错
      import pymongo
      
      class DoubanspiderPipeline(object):
          def __init__(self):
              # 获取setting主机名、端口号和数据库名
              from Douban import settings
              host = settings['MONGODB_HOST']
              port = settings['MONGODB_PORT']
              dbname = settings['MONGODB_DBNAME']
      
              # pymongo.MongoClient(host, port) 创建MongoDB的客户端对象
              client = pymongo.MongoClient(host=host,port=port)
      
              #指定的数据库名
              mdb = client[dbname]
              #指定数据库的表名
              self.post = mdb[settings['MONGODB_DOCNAME']]
      
      
          def process_item(self, item, spider):
              data = dict(item)
              
              # 向指定的表里添加数据
              self.post.insert(data)
              return item
              # print(item)
              # return item
      ```

5. settings.py

   1. ```
      BOT_NAME = 'douban'
      
      SPIDER_MODULES = ['Douban.spiders']
      NEWSPIDER_MODULE = 'Douban.spiders'
      
      ITEM_PIPELINES = {
              'douban.pipelines.DoubanspiderPipeline' : 300
              }
      
      # Crawl responsibly by identifying yourself (and your website) on the user-agent
      USER_AGENT = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.116 Safari/537.36'
      
      # MONGODB 主机环回地址127.0.0.1
      MONGODB_HOST = '127.0.0.1'
      # 端口号，默认是27017
      MONGODB_PORT = 27017
      # 设置数据库名称
      MONGODB_DBNAME = 'DouBan'
      # 存放本次数据的表名称
      MONGODB_DOCNAME = 'DouBanMovies'
      ```

6. 运行: `scrapy crawl douban`

7. 数据库 MongoDB

   1. ```
      启动MongoDB数据库需要两个命令：
      
      mongod：mongoDB数据库服务
      mongo：命令行shell客户端
      
      
      sudo mongod # 首先启动数据库服务，再执行Scrapy
      sudo mongo # 启动数据库shell
      
      在mongo shell下使用命令:
      
      # 查看当前数据库
      > db
      
      # 列出所有的数据库
      > show dbs
      
      # 连接DouBan数据库
      > use DouBan
      
      # 列出所有表
      > show collections
      
      # 查看表里的数据
      > db.DouBanMoives.find()
      ```

      

#### 5.6 (案例六)三种scrapy模拟登陆策略

1. 模拟登陆, 必须保证settings.py里  `COOKIES_ENABLED` (Cookies中间件)  开启状态
   1. `COOKIES_ENABLED = True` 或 `# COOKIES_ENABLED = False`
   
2. 策略

   1. 策略一：直接POST数据（ 如 登陆的账户信息)

      1. 提交post数据, 可 用这种方法。 示例 : post 数据是账户密码

      2. ```
         # -*- coding: utf-8 -*-
         import scrapy
         
         
         class Renren1Spider(scrapy.Spider):
             name = "renren1"
             allowed_domains = ["renren.com"]
         
             def start_requests(self):
                 url = 'http://www.renren.com/PLogin.do'
                 # FormRequest() : Scrapy发送POST请求的方法
                 yield scrapy.FormRequest(
                         url = url,
                         formdata = {"email" : "mr_mao_hacker@163.com", "password" : "axxxxxxxe"},
                         callback = self.parse_page)
         
             def parse_page(self, response):
                 with open("mao2.html", "w") as filename:
                     filename.write(response.body)
         ```

   2. 策略二： 模拟登陆步骤

      1. 模拟登录

         1. 先发送登录页面的get请求，获取到页面里的登录必须的参数（比如说zhihu登陆界面的 _xsrf）
         2. 然后和账户密码一起 post 到服务器，登录成功

      2. ```
         # -*- coding: utf-8 -*-
         import scrapy
         
         
         #定义爬虫模块
         class Renren2Spider(scrapy.Spider):
             name = "renren2"
             allowed_domains = ["renren.com"]
             start_urls = (
                 "http://www.renren.com/PLogin.do",
             )
         
             # 处理start_urls里的登录url的响应内容，提取登陆需要的参数（如果需要的话)
             def parse(self, response):
                 # 提取登陆需要的参数
                 #_xsrf = response.xpath("//_xsrf").extract()[0]
         
                 # 发送请求参数，并调用指定回调函数处理
                 yield scrapy.FormRequest.from_response(
                         response,
                         formdata = {"email" : "mr_mao_hacker@163.com", "password" : "axxxxxxxe"},#, "_xsrf" = _xsrf},
                         callback = self.parse_page
                     )
         
             # 获取登录成功状态，访问需要登录后才能访问的页面
             def parse_page(self, response):
                 url = "http://www.renren.com/422167102/profile"
                 yield #scrapy.Request(url, callback = self.parse_newpage)
         
             # 处理响应内容
             def parse_newpage(self, response):
                 with open("xiao.html", "w") as filename:
                     filename.write(response.body)
         ```

         

   3. 策略三：直接 用 Cookie模拟登陆.  因为 Cookie 保存登陆状态的

      1. ```
         # -*- coding: utf-8 -*-
         import scrapy
         
         class RenrenSpider(scrapy.Spider):
             name = "renren"
             allowed_domains = ["renren.com"]
             start_urls = (
                 'http://www.renren.com/111111',
                 'http://www.renren.com/222222',
                 'http://www.renren.com/333333',
             )
         
             cookies = {
             "anonymid" : "ixrna3fysufnwv",
             "_r01_" : "1",
             "ap" : "327550029",
             "JSESSIONID" : "abciwg61A_RvtaRS3GjOv",
             "depovince" : "GW",
             "springskin" : "set",
             "jebe_key" : "f6fb270b-d06d-42e6-8b53-e67c3156aa7e%7Cc13c37f53bca9e1e7132d4b58ce00fa3%7C1484060607478%7C1%7C1486198628950",
             "t" : "691808127750a83d33704a565d8340ae9",
             "societyguester" : "691808127750a83d33704a565d8340ae9",
             "id" : "327550029",
             "xnsid" : "f42b25cf",
             "loginfrom" : "syshome"
             }
         
             #重写Spider类的start_requests方法，附带Cookie值，发送POST请求
             def start_requests(self):
                 for url in self.start_urls:
                     yield scrapy.FormRequest(url, cookies = self.cookies, callback = self.parse_page)
         
             #处理响应内容
             def parse_page(self, response):
                 print "===========" + response.url
                 with open("deng.html", "w") as filename:
                     filename.write(response.body)
         ```

         

#### 5.7 附：通过Fiddler进行手机抓包方法

1. Fiddler抓包工具， 抓取手机的网络通信
   1. 前提: 手机和电脑处于同一局域网内（WI-FI或热点）
2. Fiddler对Android应用 抓包(设置流程)
   1. 电脑端 打开Fiddler设置: 菜单 Tools --> Telerik Fiddler Options
   2. 弹出窗口中选 `Connections`
      1. 选 allow remote computers to connect  #设置允许连接远程计算机
      2. 确认后重新启动Fiddler
   3. 命令行窗口,  输入`ipconfig`查看本机IP
      1. IP是 `10.73.59.166`
   4. 打开 Android手机 的“设置”->“WLAN”, 选择手机连接的网络
      1. 找到 要连接的网络， 长按，然后选择“修改网络”
      2. 弹出 网络设置 对话框， 勾选“显示高级选项”
      3. “代理”后面 输入框选择“手动”
      4. 在“代理服务器主机名”后面 输入框输入电脑的ip地址`10.73.59.166`，
      5. 在“代理服务器端口”后面 输入框输入`8888`，
      6. 然后点击“保存”按钮
   5. 启动 Android手机 中的浏览器, 可在电脑端的Fiddler中看到请求和响应数据
3. Fiddler对iPhone手机应用 抓包
   1. 基本流程 差不多，只是 手机设置 不一样
   2. iPhone手机：点击设置 > 无线局域网 > 无线网络 > HTTP代理 > 手动
      1. 代理地址(电脑IP)：192.168.xx.xxx
      2. 端口号：8888

