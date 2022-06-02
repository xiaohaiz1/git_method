没有勾心斗角不叫中国人

----



### 8 正则表达式

#### 8.1 正则表达式概述

##### 8.1 总结 01-正则表达式简介 

1. 正则表达式是**通用**的, 
   1. python, HTML前端等都可以
2. import re

#### 8.2 re模块操作

0. Python, 模块 re

1. re模块的使用过程

   1. ```
      `#coding=utf-8
      
      `# 导入re模块
      import re
      
      `# 使用match方法进行匹配操作
      result = re.match(正则表达式,要匹配的字符串)
      
      `# group方法来提取 上一步匹配的数据的话
      result.group()
      ```

   2. ```
      `# coding=utf-8`
      
      import re
      
      result = re.match("itcast", "itcast.cn")
      
      print(result.group())  #itcast
      ```

   3. re.match()  匹配以 xxx **开头**的字符串

#### 8.3 匹配单个字符

1.  字符

   1. | 字符 | 功能                                  |
      | :--: | :------------------------------------ |
      |  .   | 任意1个字符（除了\n）                 |
      | [ ]  | 一个, [ ]中列举的字符, 任一个         |
      |  \d  | 一个, 数字，即 0-9                    |
      |  \D  | 一个,  非数字，即不是数字             |
      |  \s  | 一个,  空白，即 空格，tab键, 回车`\n` |
      |  \S  | 一个,  非空白                         |
      |  \w  | 一个, 字符，即a-z、A-Z、0-9、_        |
      |  \W  | 一个, 非字符                          |

2. 示例

   1. ```
      `#coding=utf-8`
      
      import re
      
      ret = re.match(".","M")
      print(ret.group())  # M
      
      ret = re.match("t.o","too")
      print(ret.group())  # too
      
      ret = re.match("t.o","two")
      print(ret.group())  # two
      ```

   2. 区分 大小写

   3. ```python
      `# 下面这个正则不能够匹配到数字4，因此ret为None`
      ret = re.match("[0-35-9]Hello Python","4Hello Python")
      `# print(ret.group())`
      ```

   4. ```
      ret=re.match(r"sdy[1-9]","sdy5") # r开头,r 也可省略,[] 一位
      
      ret.group() #选出匹配的部分
      ```

      

#### 8.4 匹配多个字符

1. {}  个数, **紧挨**着的那**位**字符

2. 数量符

   1. | 字符  | 功能                              |
      | :---: | :-------------------------------- |
      |   *   | 前一个字符, 0次-无限次，可有可无  |
      |   +   | 前一个字符, 1次-无限次，至少有1次 |
      |   ?   | 前一个字符, 1次-0次，1次，没有    |
      |  {m}  | 前一个字符, m次                   |
      | {m,n} | 前一个字符, 从m-n次               |

   

3. `re.S` , 参数,  包括 "**\n**"  换行符

4. 示例

   1. ```
      ret = re.match("[A-Z][a-z]*","MnnM")  # *
      print(ret.group()) # Mnn
      ```

   2. ```
      ret = re.match("[a-zA-Z_]+[\w]*",name) # +
      print("变量名 %s 符合要求" % ret.group())
      ```

   3. ```
      ret = re.match("[1-9]?[0-9]","7")  # ?
      print(ret.group())
      ```

   4. ```
      `# {m, n} m到n个字符`
      ret = re.match("[a-zA-Z0-9_]{8,20}","1ad12f23s34455ff66")  # 8位到20位字符
      print(ret.group())
      ```



#### 8.5 开头结尾

1. | 字符 | 功能                                |
   | :--: | :---------------------------------- |
   |  ^   | 匹配字符串开头, 一个指定的字符 开头 |
   |  $   | 匹配字符串结尾, 一个指定的字符 结尾 |

2. 比较

   1. re.match()  #自动含开头
   2. re.match(r"^  $","")  # ^ 开头, `$ `结尾, 一位

3. 匹配163.com的邮箱地址

   1. ```
      `#coding=utf-8`
      
      import re
      
      email_list = ["xiaoWang@163.com", "xiaoWang@163.comheihei", ".com.xiaowang@qq.com"]
      
      for email in email_list:
          ret = re.match("[\w]{4,20}@163\.com", email)
          if ret:  // 非空
              print("%s符合规定,结果%s" % (email, ret.group()))
          else:   // 空
              print("%s 不符合要求" % email)
      ```

4. 转义
   
   1. 特殊符号 `? .` , 反斜杠`\`, 转义

#### 8.6 匹配分组  07-分组等

1. |     字符     | 功能                                                         |
   | :----------: | :----------------------------------------------------------- |
   |     `|`      | 一分两半, `|`前后各为一个整体,   "ab\|cd"  ab或cd, 不是 abc,acd, 匹配左右任意一个表达式 |
   |     (ab)     | 括号中一个整体                                               |
   |    `\num`    | 分组序号,引用`\num` 对应的字符串, `\1`第一个括号, `\2` 第二个括号 |
   | `(?P<name>)` | 分组别名`命名`为 name,       ?P<name>  的引用 ?P=name  , 大写 P |
   |  (?P=name)   | 引用 name别名 对应的字符串, 大写 P                           |

   1. group() , 所有
   
   2. group(1)  , 第一个括号
   
   3. `<?P<p1>\w>.*</(?P=p1)`
   
   4. ```
      test_label = "<html>hh</html>"
      ret = re.match(r"<([a-zA-Z]*)>\w*</\1>", test_label)
      print(ret.group(1))  # html
      print(ret.group())   # <html>hh</html>
      ```
   
      
   
2. 示例

   1. ```
      #coding=utf-8`
      import re
      ret = re.match("[1-9]?\d","8333")
      print(ret.group())  # 83
      ```

   2. ```
      #`|`前后各为一个整体
      ret = re.match("\w{4,20}@(163|126|qq)\.com", "test@126.com")
      print(ret.group())  # test@126.com
      ```

   3. ```
      #不是以4、7结尾的手机号码(11位)
      tels = ["13100001234", "18912344321", "10086", "18800007777"]
      
      for tel in tels:
          ret = re.match("1\d{9}[0-35-68-9]", tel)
      ```

   4. ```
      `# 提取区号和电话号码`
      ret = re.match("([^-]*)-(\d+)","010-12345678")
      ret.group()  # '010-12345678'
      ```

   5. ```
      `# 匹配出<html>hh</html>`
      ```
   
3. 分组

   1. group()

   2. ```
      `# 引用 分组 中匹配到的数据，元字符串，类似 r""这种格式`
      ret = re.match(r"<([a-zA-Z]*)>\w*</\1>", "<html>hh</html>")
      print(ret.group())
      ```

   3. `\num`   如 `\1`,  `\2`,  `\3`,  从1开始

      ```
      `# 匹配出<html><h1>www.itcast.cn</h1></html>`
      #coding=utf-8
      
      import re
      
      labels = ["<html><h1>www.itcast.cn</h1></html>", "<html><h1>www.itcast.cn</h2></html>"]
      
      for label in labels:
          ret = re.match(r"<(\w*)><(\w*)>.*</\2></\1>", label)
          if ret:
              print("%s 是符合要求的标签" % ret.group())
          else:
              print("%s 不符合要求" % label)
      ```

   4. 分组的别名的`命名, 引用`

      1. 命名`(?P<name>)`,  引用` (?P=name)`,  大写P, 对称性

      2. ```
         import re
         
         ret = re.match(r"<(?P<name1>\w*)><(?P<name2>\w*)>.*</(?P=name2)></(?P=name1)>", "<html><h1>www.itcast.cn</h1></html>")
         ret.group()
         ```

4. 空格 是 符号, 注意 match中的`空格`

#### 8.7 re模块的高级用法

1. search

   1. ```
      `#匹配出文章阅读的次数
      `#coding=utf-8
      import re
      
      ret = re.search(r"\d+", "阅读次数为 9999")
      ret.group()  # '9999'
      ```
      
   2. 对比

      1. | search                                 | match            |
         | -------------------------------------- | ---------------- |
         | 不从头开始                             | 从头开始         |
         | 返回 一个 匹配串                       | 返回 一个 匹配串 |
         | 有`group()`                            | 有`group()`      |
         | 加 `^`, 从头匹配,  返回一个, 与match同 |                  |
         
         

2. findall(r"","")  # 返回list, 元素是str类型, 无group

   1. ```
      统计出python、c、c++相应文章阅读的次数
      import re
      
      ret = re.findall(r"\d+", "python = 9999, c = 7890, c++ = 12345")
      print(ret) # ['9999', '7890', '12345']
      ```

3. sub 匹配替换

   1. sub(r"A", "B", "C")  # 用A匹配C, 匹配的用B替换,  无group()

   2. ```
      ret = re.sub(r"\d+", '998', "python = 997")
      print(ret)   # python = 998, str类型
      ```

   3. ```
      def add(temp):
          strNum = temp.group()
          num = int(strNum) + 1
          return str(num)
      
      ret = re.sub(r"\d+", add, "python = 997")
      print(ret)  # python = 998
      ```

4. re.split(r"A|B", "C")  # 用A或B匹配C, 切割, 去掉A或B 返回list, 无group()

   1. ```
      ret = re.split(r":| ","info:xiaoZhang 33 shandong")
      print(ret) # ['info', 'xiaoZhang', '33', 'shandong']
      ```

      
      

#### 8.8 贪婪和非贪婪

1. `数量词` 默认`贪婪的`

   1. 匹配尽可能 多 的字符 

2. 非贪婪 相反，总是 匹配尽可能 少 的字符

   1. "*","?","+","{m,n}"后 加上？，贪婪变成非贪婪

   2. ```python
      re.match(r"aa(\d+)","aa2343ddd").group(1) # '2343'
      ```

      ```
      re.match(r"aa(\d+?)","aa2343ddd").group(1) # '2'
      ```

#### 8.9 r的作用

1. 正则字符串前 加` r `表示 原生字符串

   1. ```
      ret = re.match("c:\\\\a",mm).group()
      print(ret)  #  c:\\a
      ```

   2. ```
      ret = re.match(r"c:\\a",mm).group()
      print(ret)  #  c:\\a
      ```

   3. 正则表达式`"\"`作为转义字符, 加 r 解决问题了

2. `原生字符串`, 不用担心 漏写 反斜杠`\`

----

### 9 http协议, web服务器-并发服务器1

#### 9.1 http协议

1. 使用谷歌/火狐浏览器分析
   1. 服务器把 网页 传给浏览器，实际是把 `网页HTML代码` 传给浏览器
   
   2. 浏览器和服务器 间的传输协议 `HTTP协议`
   
   3. HTTP
      1. 定义网页的 `文本`.  
         1. 会HTML, 就会编 网页；
      2. HTTP是 网络协议， 浏览器和服务器的 通信
      
   4. Chrome 开发者工具
      1.  Chrome的 菜单中选 “视图”，“开发者”，“开发者工具”
      2. Elements : 网页的结构
      3. Network  浏览器和服务器的通信
         1. 点Network，确保第一个小红灯亮 ，Chrome 会记录 浏览器和服务器 间的通信
      
   5. chrome 开发者工具
   
      1. 开发者工具面板: Elements、Console、Sources、Network、Performance、Memory、Application、Security、Audits
   
      2. | 面板            | 功能 / 使用                                                  |
         | --------------- | ------------------------------------------------------------ |
         | Elements        | 功能: 查看和编辑当前页面中的 HTML 和 CSS 元素<br>实时编辑DOM节点和CSS样式，修改的效果会立即显示在浏览器中，极大地方便了前端的调试。<br>1. 双击DOM树视图的节点，实时编辑标签属性，修改的效果立刻反应在浏览器里面<br>2. 点击右侧Style面板，实时修改CSS的属性值，所有样式Name和Value都可以编辑; 每个属性后面单击可添加新的样式.<br>3. 点击右侧Computed面板， 编辑左侧选中的盒子模型参数，所有的值都是可以修改的; 点击不同的位置(top、bottom、left、right) 就可以修改元素的padding、border、margin属性值。<br>4. 查看网页的本地修改历史<br/>5. 点击Styles面板中修改过属性的文件名，会跳转到Source面板<br>6. 文件位置右击选择Local modifications, 查看本地的所有修改记录<br>7. 点击指定的时间点, 看到粉红背景的删除内容和绿色背景的添加内容 |
         | Console         | 功能: 显示脚本输出信息，或测试脚本等<br>控制台输出日志信息<br>js命令行`console.log()`、`console.error()`输出日志 |
         | Network         | 功能: 查看 HTTP 请求的详细信息(如请求头、响应头)和返回内容<br><br>Header：列出资源的HTTP头信息，包括请求url、HTTP方法、响应状态码、请求头和响应头及它们各自的值、请求参数等等。<br/><br/>Preview：预览面板，根据选择的资源类型（JSON、图片、文本）显示相应的预览。<br/><br/>Response：显示HTTP的Response信息，`响应信息面板`的资源还未进行格式处理的内容。<br/><br/>cookie:显示资源HTTP的Request和Response过程中的Cookies信息。<br/><br/>Timing：资源请求的详细信息花费时间。<br/> |
         | Sources资源面板 | 功能: 查看和调试当前页面所加载的脚本的`源文件`<br>调试JS代码，也可以在工作区打开本地文件. 打开Sources进行js断点调试,几乎能解决8成的代码问题<br>分四个部分,四个部分互相关联:监控`js`在执行期的活动<br>区域1:  功能类似于Resources面板, 显示网页加载的脚本文件: 如css, js(不包含cookie,img等静态资源文件)<br>区域1的导航条上有三个tab切换选项,他们都存有不同域名和环境下的js和css文件,<br>Sources(资源)选项的作用:<br>Sources包含该项目的静态资源文件。`双击`选中文件,  内容在区域2中显示,如选中js文件, 在区域2中单击行号 断点调试, js执行到了标记行,会停止并且等待你的命令.<br>区域2 : 显示区域1选中的内容. 区域2中修改, 可验证代码线上是否可行<br>区域3: Breakpoints记录信息会变高亮. <br>区域4: Scope 选项中列出了断点处私有和公有的变量信息<br>按`F10`跟着js一步一步执行, 遇到一个函数包含着另外一个函数, 按`F11`进入函数中观察. 也可以通过点击`区域1`底部的各个图标对js代码进行跟踪. ,<br>区域4: 切换到`Watch Expressions` 选项, 为目前断点添加表达式,使得每次断点往下走一步都会执行你写下的js代码。 注意: 这个功能谨慎使用, 这可能导致监控代码段会不断地被执行. 为避免调试代码重复执行,调试时直接在console控制台上一次性地输出当前断点处的信息(推荐这样做)。为了验证在console面板中拥有的是当前断点环境, 可以对比断点执行前后的this值变化<br>----------------<br>区域1,Content script 选项: 包含第三方插件或浏览器自身的js代码, 它被忽略的, 作用很少。<br>Snippets选项: 在Sinppets中, 可编辑(重写)js代码片段。这些片段相当于js文件, 不同的是本地的js文件在编辑器里编辑, 此处, 在浏览器中编写。 代码片段在浏览器不会消失,也不会执行,除非手动执行它。作用: 使编写一些项目的测试代码时提供`便捷` .  编辑器上编写这些代码, 发布时必须为它们添加注释符号或手动删除, 在浏览器上编写不需要这样繁琐。<br>Snippets选项: 空白处右键弹出`new`选项, 建立一个新的文件, 在区域2种编辑它。 Snippets 非常功能强大, 如: 调试片段, 单元测试, 少量的功能代码编写<br>------------<br>区域3: 监控事件的功能 |
         | Performance     | 功能: 记录与分析 程序运行过程中所产生的活动.  用在性能优化方面<br>四个窗格:<br>1. controls。开始记录，停止记录和配置记录期间捕获的信息<br/>2. overview。页面性能的高级汇总<br/>3. 火焰图。 CPU 堆叠追踪的可视化<br/>4. 统计汇总。以图表的形式汇总数据 |
         | Memory          | 功能: 录制一段当前页面一些页面操作，时间自己掌握<br>堆栈快照、JavaScript函数内存分配、隔离内存泄漏 |
         | Application     | `记录`网站加载的所有资源信息，包括存储数据（Local Storage、Session Storage、IndexedDB、Web SQL、Cookies）、缓存数据、字体、图片、脚本、样式表等 |
         | Security        | 功能: 判断当前网页是否安全<br>调试网页安全和认证，确保网站上实现HTTPS（判断网页安全性） |
         | Audits          | 对当前网页进行网络利用情况、网页性能方面的诊断，并给出一些优化建议。比如列出所有没有用到的CSS文件等 |
   
      3. 总结:
      
         1. | "查看网页源代码”                                             | 审查元素Elements                                          |
            | ------------------------------------------------------------ | --------------------------------------------------------- |
            | 代码内容是服务器发送到浏览器的原封不动的源代码(未渲染的代码)，不包括页面动态渲染的内容 | 源代码+js动态渲染的内容，即最终展示的html内容(渲染的代码) |
      
         2. elements与networks 分析 1
            1. `Elements`标签
               1. 右图点击出现两个部分，中间图显示整个网页的HTML信息，单击选中某行，右图部分styles标签会显示当前点击选中内容的CSS样式，并可对元素的CSS进行查看与编辑修改
               2. 鼠标放到中间图的某行，此行会“高亮”，同时左图相应的部分也“高亮”，表明自动显示并选中该元素在HTML里的位置。
            2. `Network`标签
               1. 记录页面上的网络请求的详情信息，从发起网页页面请求Request后分析HTTP请求后得到的各个请求资源信息（包括状态、资源类型、大小、所用时间、Request和Response等）， 根据这个进行网络性能优化。
               2. 点击资源的Name可以查看该资源的详细信息, 资源类型不同显示的信息也不太一样，可能Tab信息：
                  1. Headers 该资源的HTTP头信息。
                  2. Preview 资源类型（JSON、图片、文本）显示相应的预览。
                  3. Response 显示HTTP的Response信息。
                  4. Cookies 显示资源HTTP的Request和Response过程中的Cookies信息。
                  5. Timing 显示资源在整个请求生命周期过程中各部分花费的时间。
   
2.  http协议的分析
   1. network 操作方法(概述): 
      1. 地址栏输入`www.sina.com`,  右键 > 检查
      2. `Network`面板，左侧 找到`www.sina.com`记录，点击之，右侧 Headers 显示Request Headers，点击右侧的 view source，看到浏览器发给新浪服务器的`请求`
   2. 浏览器请求 ---- Request Headers
      1. Request Headers，点击右侧 `view source`，  浏览器发给新浪服务器的`请求`:
         2.  第一行
            1.  `GET / HTTP/1.1`
               1. `GET` 请求方法， 从服务器获得网页数据
               2. `/`  URL的路径，URL总以`/`开头，`/` 表示首页
               3. `HTTP/1.1`   采用 HTTP协议, 版本是1.1
                  1. 目前HTTP协议的版本 是1.1
                  2. 大部分服务器也支持1.0版本
                  3. 1.1版本允许 多个HTTP请求 复用一个TCP连接，以加快传输
         2. 从第二行开始，每行格式: `Xxx: abcdefg`
            1. `Host: www.sina.com`
               1. 请求的域名`www.sina.com`
               2. 一台服务器有多个网站，服务器用`Host`区分浏览器请求网站
         3. 备注:服务器 域名 网站什么关系
            1. 开设网站，要在服务器部署网站程序, 域名绑定网站地址
            2. 服务器: 指24小时运行的电脑主机
            3. 域名: 容易记住的名字，方便访问网站
            4. 可以IP直接访问网站
   3. 服务器响应 ---- Response Headers
      1. 点击 `view source`， 服务器返回的`原始响应数据`
         1. HTTP响应分 `Header`, `Body`两部分（Body是可选项）
      2. 响应Header
         1. `HTTP/1.1 200 OK`
            1. `200` 一个成功的响应， `OK`是说明
            2. `404 Not Found`：网页不存在
            3. `500 Internal Server Error`：服务器内部出错
         2. `Content-Type: text/html`
            1. Content-Type 响应内容类型，这里是text/html, HTML网页
               1. 响应内容类型: 网页, 图片, 视频, 音乐
            2. 浏览器 靠Content-Type 判断响应的内容: 网页,图片,视频,音乐
               1. 浏览器 不靠URL 判断响应的内容
               2. URL是`http://www.baidu.com/meimei.jpg`，不一定是图片
      3. 响应Body
         1. HTTP响应的Body 是HTML源码
            1. 菜单栏选择“视图”，“开发者”，“查看网页源码”
            2. 或, 右键 > 查看网页源代码
   4. 浏览器解析过程
      1. 浏览器获得新浪首页的 HTML源码， 解析HTML，显示 `页面`
      2. 然后，根据HTML中的各 链接，再发送 HTTP请求 给新浪服务器，获得 图片、视频、Flash、JavaScript脚本、CSS等各种资源，最终显示`完整页面`
   
3. 总结

   1. HTTP请求 流程

      1. 步骤1, 浏览器 先 向服务器发送`HTTP请求`, 包括

         1. 方法：GET / POST，GET仅请求资源，POST会附带用户数据；

         2. 路径：/full/url/path；

         3. 域名：`Host`指定：Host: www.sina.com

         4. 其他Header；

         5. POST，请求包括一个Body，含用户数据

         6. ```
            GET / HTTP/1.1
            Host: www.qq.com
            ......
            ```

            

      2. 步骤2, 服务器 向 浏览器返回`HTTP响应`，包括

         1. 协议/版本 响应代码 OK, 如:   `HTTP/1.1 200 OK` 
            1. 响应代码：200 成功, 3xx 重定向, 4xx 客户端请求错误, 5xx 服务器端 错误；
         2. 响应类型： Content-Type指定；
         3. 其他相关Header；
         4. 服务器 HTTP响应 携带 响应内容Body
            1. 包含响应的内容
         2. HTML源码就在Body中
   
   3. 步骤3,  浏览器 再次向服务器请求其他资源. 如图片，要再发 HTTP请求，重复步骤1、2
   
         1. Web的HTTP协议 : 请求-响应模式
         2. 编写页面
            1. 只要 发送HTTP请求，不考虑附带图片、视频等请求
               2. 浏览器要 图片和视频，它会发送另一个HTTP请求
            3. 一个HTTP请求只处理一个资源(TCP协议是短连接，每个链接只获取一个资源.  如需要多个就需要建立多个链接)
         3. HTTP协议: 扩展性. 
            1. HTML中可 链入其他服务器的资源.
            2. 将请求压力分散到各个服务器上
               1. 为了分散服务器压力, 一个网站会有多台服务器
               2. 如 一台放图片, 一台放 视频
         3. 一个站点可 链接到其他站点，无数 站点互相链接,  形成了World Wide Web，简称WWW。
   
2. HTTP格式
   
   1. HTTP请求和响应 的格式，
   
         1. HTTP 有 Header和Body两部分， Body是可选的
      2. HTTP协议 是 文本协议,  简单
   
   2. HTTP GET请求的格式
   
      1. ```
         GET /path HTTP/1.1
         Host: quote.eastmoney.com
         Header1: Value1
         Header2: Value2
      Header3: Value3
         ```
      ```
         
      2. 每个`Header`一行，换行符是`\r\n`
      ```
   
   3. HTTP POST请求的格式
      
         1. ```
            POST /path HTTP/1.1
            Header1: Value1
            Header2: Value2
            Header3: Value3
            
            ```
         
   
   body data goes here...
      ```
   
      2. `header 连续两个\r\n body`
      
         1. 连续两个`\r\n`，Header 结束，后面的数据是Body
      
      4. HTTP响应的格式
      
         1. ```
            200 OK
            Header1: Value1
            Header2: Value2
         Header3: Value3
         
            body data goes here...
      ```
   
         2. HTTP响应若 含body，也用`\r\n\r\n`分隔
         3. Body 数据类型由`Content-Type: 类型` 头 确定
            1. 若 网页，Body 是 `文本`
            2. 若 图片，Body 是图片的`二进制数据`
         4. 有Content-Encoding时，Body数据 压缩的，最常见的压缩方式 gzip
            1. 若 Content-Encoding: gzip ，要将Body数据 解压 ,得到数据
            2. 压缩的目的: 减小Body, 加快传输

----------

---

#### 9.2 Web静态服务器-1-显示固定的页面

1. ```
   `# 服务器端 编程 web静态服务器
   #coding=utf-8
   import socket
   
   
   def handle_client(client_socket):
       "为一个客户端进行服务"
       recv_data = client_socket.recv(1024).decode("utf-8")
       request_header_lines = recv_data.splitlines()
       for line in request_header_lines:
           print(line)
   
       #组织 头信息(header)
       response_headers = "HTTP/1.1 200 OK\r\n"  # 200表示找到这个资源
       response_headers += "\r\n"  # 用一个空的行与body进行隔开
       #组织 内容(body)
       response_body = "hello world"
   	#连接"头信息+内容"
       response = response_headers + response_body
       
       client_socket.send(response.encode("utf-8"))
       client_socket.close()
   
   
   def main():
       "作为程序的主控制入口"
   
       server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
       #设置 服务器先close 即服务器端4次挥手之后资源能够立即释放，这样就保证了，下次运行程序时 可以立即绑定7788端口
       server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
       server_socket.bind(("", 7788))
       server_socket.listen(128)
       while True:
           client_socket, client_addr = server_socket.accept()
           handle_client(client_socket)
   
   
   if __name__ == "__main__":
       main()
   ```
   
   1. setsockopt()  获取或设置与某个套接字关联的选项
      1. int setsockopt(int sock, int level, int optname, const void *optval, socklen_t optlen);
         1. 参数：
            1. sock：要被设置或者获取选项的套接字。
            2. level：选项所在的协议层。
            3. optname：需要访问的选项名。
            4. optval：对于getsockopt()，指向返回选项值的缓冲。对于setsockopt()，指向包含新选项值的缓冲。
            5. optlen：对于getsockopt()，作为入口参数时，选项值的最大长度。作为出口参数时，选项值的实际长度。对于setsockopt()，现选项的长度。
   2. splitlines()  #按行(’\r’, ‘\r\n’, \n’)分隔，返回一个包含各行作为元素的列表

#### 9.3  Web静态服务器-2-显示要的页面

1. ```
   `# web 静态服务器, 显示页面
   
   #coding=utf-8
   import socket
   import re
   
   
   def handle_client(client_socket):
       "为一个客户端进行服务"
       recv_data = client_socket.recv(1024).decode('utf-8', errors="ignore")
       request_header_lines = recv_data.splitlines()
       for line in request_header_lines:
           print(line)
   
       http_request_line = request_header_lines[0]
       get_file_name = re.match("[^/]+(/[^ ]*)", http_request_line).group(1)
       print("file name is ===>%s" % get_file_name)  # for test
   
       # 如果没有指定访问哪个页面。例如index.html
       # GET / HTTP/1.1
       if get_file_name == "/":
       	#/html/index.html
           get_file_name = DOCUMENTS_ROOT + "/index.html"
       else:
           get_file_name = DOCUMENTS_ROOT + get_file_name
   
       print("file name is ===2>%s" % get_file_name) #for test
   
       try:
           f = open(get_file_name, "rb")
       except IOError:
           # 404表示没有这个页面
           response_headers = "HTTP/1.1 404 not found\r\n"
           response_headers += "\r\n"
           response_body = "====sorry ,file not found===="
       else:
           response_headers = "HTTP/1.1 200 OK\r\n"
           response_headers += "\r\n"
           response_body = f.read()
           f.close()
       finally:
           #头信息 按照字符串组织的，不能与以二进制打开文件读取的数据合并，因此分开发送
           # 先发送response的头信息
           client_socket.send(response_headers.encode('utf-8'))
           # 再发送body
           client_socket.send(response_body)
           client_socket.close()
   
   
   def main():
       ""作为程序的主控制入口""
       server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
       server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
       server_socket.bind(("", 7788))
       server_socket.listen(128) #主动转被动
       while True:
           client_socket, clien_cAddr = server_socket.accept()
           handle_client(client_socket)
   
   
   #配置服务器
   DOCUMENTS_ROOT = "./html"
   
   if __name__ == "__main__":
       main()
   ```

#### 9.3 Web静态服务器-3-多进程

1. ```
   `# Web静态服务器端程序-3-多进程
   #子进程复制了主进程的所有资源
   #coding=utf-8
   import socket
   import re
   import multiprocessing  
   
   
   class WSGIServer(object):
   
       def __init__(self, server_address):
           #创建一个tcp套接字
           self.listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           #允许立即使用上次绑定的port
           self.listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           #绑定
           self.listen_socket.bind(server_address)
           #变为被动，并指定队列的长度
           self.listen_socket.listen(128)
   
       def serve_forever(self):
           "循环运行web服务器，等待客户端的链接并为客户端服务"
           #循环
           while True:
               # 等待新客户端到来
               client_socket, client_address = self.listen_socket.accept()
               print(client_address)  # for test
               #创建进程对象
               new_process = multiprocessing.Process(target=self.handleRequest, args=(client_socket,))
               #启动进程
               new_process.start()
   
               #子进程复制了父进程的套接字等资源,父进程.close()不会关闭对应的链接
               #父进程关闭
               client_socket.close()
   
       def handleRequest(self, client_socket):
           """用一个新的进程，为一个客户端进行服务"""
           recv_data = client_socket.recv(1024).decode('utf-8')
           print(recv_data)
           requestHeaderLines = recv_data.splitlines()
           for line in requestHeaderLines:
               print(line)
   		
   		#获取request的第一行(获取客户端要请求的路径)
           request_line = requestHeaderLines[0]
           get_file_name = re.match("[^/]+(/[^ ]*)", request_line).group(1)
           print("file name is ===>%s" % get_file_name) # for test
   
           if get_file_name == "/":
           	# ./html/index.html, 路径+文件
               get_file_name = DOCUMENTS_ROOT + "/index.html"
           else:
               get_file_name = DOCUMENTS_ROOT + get_file_name
   
           print("file name is ===2>%s" % get_file_name) # for test
   
           try:
               f = open(get_file_name, "rb")
           except IOError:
               response_header = "HTTP/1.1 404 not found\r\n"
               response_header += "\r\n"
               response_body = "====sorry ,file not found===="
           else:
               response_header = "HTTP/1.1 200 OK\r\n"
               response_header += "\r\n"
               response_body = f.read()
               f.close()
           finally:
               client_socket.send(response_header.encode('utf-8'))
               client_socket.send(response_body)
               client_socket.close()
   
   
   # 设定服务器的端口
   SERVER_ADDR = (HOST, PORT) = "", 8888
   # 设置服务器服务静态资源时的路径
   DOCUMENTS_ROOT = "./html"
   
   
   def main():
       httpd = WSGIServer(SERVER_ADDR)
       print("web Server: Serving HTTP on port %d ...\n" % PORT)
       httpd.serve_forever()
   
   if __name__ == "__main__":
       main()
   ```

#### 9.4 Web静态服务器-4-多线程

1. ```
   `#Web静态服务器-4-多线程
   #子线程与主线程共用一个socket
   #coding=utf-8
   import socket
   import re
   import threading
   
   
   class WSGIServer(object):
   
       def __init__(self, server_address):
           #创建一个tcp套接字
           self.listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           #设置套索字选项, 允许立即使用上次绑定的port
           self.listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           #绑定(iP,port)
           self.listen_socket.bind(server_address)
           #变为被动，指定队列的长度
           self.listen_socket.listen(128)
   
       def serve_forever(self):
           "循环运行web服务器，等待客户端的链接并为客户端服务"
           while True:
               #等待新客户端到来
               client_socket, client_address = self.listen_socket.accept()
               print(client_address)
               #创建子线程
               new_process = threading.Thread(target=self.handleRequest, args=(client_socket,))
               #启动线程
               new_process.start()
   
               #线程共享一个套接字，主线程不能关闭，否则子线程就不能再使用这个套接字了
               # client_socket.close()  #主线程不能关闭
   	
   	#
       def handleRequest(self, client_socket):
           """用一个新的子进程，为一个客户端进行服务"""
           recv_data = client_socket.recv(1024).decode('utf-8')
           print(recv_data)
           requestHeaderLines = recv_data.splitlines()
           for line in requestHeaderLines:
               print(line)
   
           request_line = requestHeaderLines[0]
           get_file_name = re.match("[^/]+(/[^ ]*)", request_line).group(1)
           print("file name is ===>%s" % get_file_name) # for test
   
           if get_file_name == "/":
               get_file_name = DOCUMENTS_ROOT + "/index.html"
           else:
               get_file_name = DOCUMENTS_ROOT + get_file_name
   
           print("file name is ===2>%s" % get_file_name) # for test
   
           try:
               f = open(get_file_name, "rb")
           except IOError:
               response_header = "HTTP/1.1 404 not found\r\n"
               response_header += "\r\n"
               response_body = "====sorry ,file not found===="
           else:
               response_header = "HTTP/1.1 200 OK\r\n"
               response_header += "\r\n"
               response_body = f.read()
               f.close()
           finally:
               client_socket.send(response_header.encode('utf-8'))
               client_socket.send(response_body)
               client_socket.close()
   
   
   # 设定服务器的端口
   SERVER_ADDR = (HOST, PORT) = "", 8888
   # 设置服务器服务静态资源时的路径
   DOCUMENTS_ROOT = "./html"
   
   
   def main():
       httpd = WSGIServer(SERVER_ADDR)
       print("web Server: Serving HTTP on port %d ...\n" % PORT)
       httpd.serve_forever()
   
   if __name__ == "__main__":
       main()
   ```

---------

视频

#### 2 http协议   

1. 备注  1的内容对应 "8 正则表达式"

##### 2.1-此阶段知识的介绍

##### 2.2-HTTP协议的通俗讲解

1. Request Header 浏览器(客户端)请求头内容

   | 请求头内容                                  | 内容解析                     |
   | ------------------------------------------- | ---------------------------- |
   | GET / HTTP/1.1                              | 请求方式 路径 协议/版本号    |
   | Host: 127.0.0.1:8080                        | 服务器主机: 目标IP: Port     |
   | Connection: keep-alive                      | 连接方式                     |
   | Accept: text/html, application/xhtml+xml,.. | 浏览器接收内容格式           |
   | Upgrade-Insecure-Requests:  1               |                              |
   | User-Agent: Mozilla/5.0                     | 客户端浏览器版本             |
   | Accept-Encoding: gzip, deflate, br          | 客户端浏览器接收压缩格式     |
   | Accept-Language:zh-CN,zh;q=0.9              | 客户端浏览器语言             |
   | Cookie                                      | 客户端浏览器Cookie, 标记客户 |

2. Response Header, 服务器应答头(部分)

   1. | 服务器响应头    | 说明                              |
      | --------------- | --------------------------------- |
      | HTTP/1.1 200 OK | 协议/版本号 响应代码 OK, 必须有的 |
   | Set-Cookie      | 服务器标记客户端的Cookie          |
   
   2. 浏览器端 接收服务响应的数据后的渲染:
   
      1. ```
         浏览器端 数据:
         
         HTTP/1.1 200 OK
      
         <h1>hahahah</h1>
         ```
   ```
      
   2. 格式说明
      
         1. header  #Response Headers, 浏览器要设置的内容
         2. 空行   #空行分割两者之间
         3. body # 浏览器要显示的内容
   ```
   
   

-------------

##### 2.3-通过网络调试助手充当http服务器来验证http协议

----------

#### 03 简单web`服务器`实现(视频)

##### 3.1-案例：返回固定页面的http服务器

1. ```
   import socket
   
   
   def service_client(new_socket):  # 参数 套接字对象
   	'''服务器端程序: 为一个客户端返回数据'''
   	# 1. 接收浏览器发送过来的请求, 即http请求
   	# GET / HTTP/1.1
   	# ...
   	request = new_socket.recv(1024)
   	print(request)
   	
   	# 2. 服务器端构造数据: 返回给浏览器的http格式的数据, 
   	# 2.1 准备发送给浏览器的 -- header
   	response = "HTTP:/1.1 200 OK\r\n"
   	response += "\r\n"      # 空行
   	# 2.2 准备发送给浏览器的 -- body
   	response += "hahahaha"
   	# 2.3 套接字对象 发送数据 (服务器向客户端发送数据)
   	new_socket.send(response.encode("utf-8"))
   	
   	# 关闭套接字
   	new_socket.close()
   
   def main():
   	"""服务器端主程序: 用来完成整体的控制"""
   	# 1. 创建 服务器端的 套接字
   	tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   	
   	# 2. 服务器套接字 绑定 IP 端口
   	tcp_server_socket.bind(("", 7890))  
   	
   	# 3. 服务器套接字 变成 监听套接字 (主动->被动)
   	tcp_server_socket.listen(128)
   	
   	while True:
   		# 4. 等待 新客户端的连接, 创建服务套接字
   		new_socket, client_addr = tcp_server_socket.accept()
   	
   		# 5. 监听套接字 为客户端服务
   		service_client(new_socket)
   	
   	# 关闭 服务器端 套接字
   	tcp_server_socket.close()
   	
   if __name__ =='__main__':
   	main()
   ```

   1. 调试过程

      1. 配置
      
         1. 服务器端: pycharm中黏贴上面程序, 启动, 作 tcp 服务器端	
            1. pycharm所在主机做远程主机(服务器)  , IP: 192.168.199.146, port: 7890
         2. 客户端: NetAssist.exe 启动, 做 tcp客户端
            1. 网络设置
               1.  协议类型 TCP Client
               2. 远程主机地址 192.168.199.146(服务器的IP)
                  1. 即 pycharm 所在的电脑的IP, 
                  2. 本题pycharm(服务器)与 NetAssist.exe(客户端) 在同一台电脑
                  3. cmd打开命令窗口, ipconfig, 得到 192.168.199.146
               3. 远程主机端口 7890(服务器的port)
         3. 连接, 成功
      
      2. 测试数据
      
         1. NetAssist(客户端)中
      
            1. 点击"发送" 按钮, 向 服务器端 发出请求
      
            2. NetAssist结束 服务器端响应. 数据日志区 显示
      
                  ```
                  HTTP:/1.1 200 OK
                             # 空行
                  hahahaha
               ```
      
               
   
2. HTTP 通信本质:  tcp套接字通信

3. 服务器 发送给 浏览器 的数据类型: **字符串**: 
   1. 字符串格式: "response header+ 空行+ response body",  字符串类型
      1. response header 告诉 浏览器设置成什么样子
      2. response body 浏览器 显示内容
   2. 发送内容前: `数据.encode()` 编码, 格式 utf-8, 二进制的 utf-8

4. 服务器程序 必须 bind() 服务器自身的(IP,Port) , 元组格式

   1. IP 可空, 字符串 ""
   2. Port, 必须填写,  数值类型,  如  7890

5. request和response格式: 

   1. ```
      header
      一个空行
      body
      ```

      
##### 3.2 -  tcp3次握手、通信, 4次挥手

1. tcp通信过程:
   1. 3次握手过程:  (C(客户端) S(服务器端), 准备资源)
      1. C 先发syn 11 给S               # SYN包, syn请求标记  ack 应答标记, C 客户端, S 服务器端
      2. S 发ack 12, syn 44 给 C    #SYN-ACK包, 12 =11 + 1,  S端的ack与syn合并发送
      3. C 发ack 45 给 S                 #ACK包, 45 = 44 + 1
   2. 传递一次`请求应答`数据包. 先3次握手成功(即建立连接), 再客户端与服务器端 通信, 
   3. 4次挥手过程: ( C 与 S 各自 释放资源)  # 因为是全双工
      1. C 先调用 close() 后 向 S 发数据包, C关闭 发送链接.  (然后C端等待)
      2. S 收到后,  向 C 发数据包 确认, S关闭 接收链接
      3. S 调用 close()后, 向 C 发数据包, S 关闭 发送链接
      4. C 收到后, 向 S 发数据包确认, C 关闭 接收链接
2. 为什么4次挥手: socket是全双工的
3. TCP包的类型 (SYN, FIN, ACK, PSH, RST, URG)
   1. `TCP层`，有个`FLAGS`字段，这个字段几个标识：SYN, FIN, ACK, PSH, RST, URG.
   2. 分析有用的五个字段:
      1. SYN: 建立连接，
      2. FIN: 关闭连接，
      3. ACK: 响应，
         1. ACK可能与SYN，FIN等同时使用的，如SYN和ACK同时为1，表示建立连接后的响应
      4. PSH: 有 DATA数据传输，
      5. RST: 连接重置。

##### 3.3 - tcp3次握手、4次挥手-强调

1.  客户端先请求关闭(然后客户端 等2分钟). 
   
1. 原因: 谁 先 调用close , 谁 先 等待2分钟
   
   2. 原因: 服务器 端口 是固定的,  要及时释放 端口资源
   
      1. ```
         #允许立即使用上次绑定的port
         self.listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
         ```
         
        2. 该语句 及时释放服务器端口(资源)



##### 3.4-案例：返回浏览器需要的页面http服务器 - 介绍

##### 3.5-案例：返回浏览器需要的页面http服务器 - 代码实现

1. service_client(new_socket)   重新编写,  其他不变

   ```
   import socket
   import re
   
   
   def service_client(new_socket):  # 参数 套索字
   	'''为一个客户端返回 数据 '''
   	# 1. 接收 客户端浏览器 发送过来的请求, 即http请求
   	# 客户端请求: GET /index.html  HTTP/1.1
   	# ...
   	request = new_socket.recv(1024).decode("utf-8")
   	
   	# splitlines() 按行('\r', '\r\n', \n')分隔
   	request_lines = request.splitlines()
   	
   	#客户端发送的请求: GET /index.html  HTTP/1.1
   	# 方式: get post put del
   	# [^/]+ 非/字符若干个
   	ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
   	if ret:
   		file_name = ret.group(1)
   		if file_name == "/":
   			file_name = "/index.html"
   			
   	# 2. 返回http格式的数据, 给浏览器
   	try:
   		# 当前文件夹 . 下html文件夹 下index.html文件 './html/index.html'
   		f = open("./html" + file_name, "rb")   
   	except:
   		# 若./html/index.html 文件不存在
           # 2.1 准备发送给浏览器的数据 --response之headers
   		response = "HTTP:/1.1 404 NOT FOUND\r\n"
   		# 准备发送给浏览器的数据 -- response之 空行 \r\n
   		response += "\r\n"      # 空行
   		# 2.2 准备发送给浏览器的数据 -- response之 body
   		response += "---file not found---"
   		
   		# 服务器之服务套接字 发生 响应, 按utf-8规则进行 二进制编码
   		new_socket.send(response.encode("utf-8"))		
   	else:
   		html_content = f.read()
   		f.close()
   		# 2.1 准备发送给浏览器的数据 --response之 header
   		response = "HTTP:/1.1 200 OK\r\n"
   		# 准备发送给浏览器的数据 --response之 空行
   		response += "\r\n"      # 空行
   		# 2.2 准备发送给浏览器的数据 --response之 body 部分1
   		response += "hahhahaha"
   		
   		# 发送内容分为两步: header, body
   		# 将response之 header发送给浏览器
   		new_socket.send(response.encode("utf-8"))
   		# 将response值 body 部分2发送给浏览器
   		new_socket.send(html_content)
   	# 关闭套接字
   	new_socket.close()
   
   def main():
   	"""用来完成整体的控制"""
   	"""与原来代码一致"""
   	......
   	
   if __name__ =='__main__':
   	main()
   ```
   
   1. splitlines() 按行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表
   
      ```
      str.splitlines([keepends])
      ```

-----

### 10 web服务器- 并发服务器2

#### 10.1  Web静态服务器-5-非堵塞模式

​	示例代码:  服务器是静态的,  且只有一个线程,  各个 套索字socket 非堵塞模式

​	非堵塞模式 : accept()时会产生异常

1. 单进程 非堵塞 模型

   ```
   `#coding=utf-8
   from socket import *
   import time
   
   `# 用来存储所有的新链接socket(为每个客户服务的套接字)
   g_socket_list = list()
   
   def main():
   	'''服务器端程序'''
       server_socket = socket(AF_INET, SOCK_STREAM)
       server_socket.setsockopt(SOL_SOCKET, SO_REUSEADDR  , 1)
       server_socket.bind(('', 7890))
       server_socket.listen(128)
       
       # 将套接字设置为 非堵塞. 
       # 非堵塞，如accept时，没有客户端connect(链接)，服务器accept 产生异常，
       # 异常用 try-except 处理异常
       
       #服务器套接字设置 非阻塞模式
       server_socket.setblocking(False)
   
       while True:
   
           # 用来测试
           time.sleep(0.5)
   
           try:
           	# newClientInfo 是 list类型, 2个元素
               newClientInfo = server_socket.accept()
           except Exception as result:
               pass
           else:
               print("一个新的客户端到来:%s" % str(newClientInfo))
               
               # newClientInfo[0] 服务客户套接字, 
               newClientInfo[0].setblocking(False)  #客户连接套接字设置为非堵塞
               
               #服务器端客户连接(套接字地址)存储于list中
               g_socket_list.append(newClientInfo)
   		
   		# for轮询
           for client_socket, client_addr in g_socket_list:
               try:
               	# 服务客户套接字接收数据
                   recvData = client_socket.recv(1024)
                   
                   # 若 接收到数据
                   if recvData:
                       print('recv[%s]:%s' % (str(client_addr), recvData))
                   # 如 没收到数据
                   else:
                       print('[%s]客户端已经关闭' % str(client_addr))
                       client_socket.close()
                       
                       # 没有收到数据, 把(对应服务客户套接字, IP)移除
                       g_socket_list.remove((client_socket,client_addr))
               except Exception as result:
                   pass
   
           print(g_socket_list)  # for test
   
   if __name__ == '__main__':
       main()
   ```

   

2.  web静态服务器-单进程非堵塞

   ```
   import time
   import socket
   import sys
   import re
   
   
   class WSGIServer(object):
       """服务器端运行的程序: 定义一个WSGI服务器的类"""
   	
       def __init__(self, port, documents_root):
   		""""__init__初始化化类""
           # 1. 创建 服务器端 套接字
           self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           
           # 2. 绑定服务器端本地信息
           self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           self.server_socket.bind(("", port))
           
           # 3. 变为监听套接字
           self.server_socket.listen(128)
           
   		# 服务器端套接字设为 非阻塞模式
           self.server_socket.setblocking(False)
           
           #客户套接字列表, list类型, 存储 客户服务套接字
           self.client_socket_list = list()
   		
   		# 文件根目录
           self.documents_root = documents_root
   
       def run_forever(self):
           """运行服务器"""
   
           # 等待对方链接
           while True:
   
               # time.sleep(0.5)  # for test
   
               try:
               	# 为客户服务套接字, 客户端IP Port
                   new_socket, new_addr = self.server_socket.accept()
               except Exception as ret:
                   print("-----1----", ret)  # for test
               else:
               	
               	#客户服务套接字设为 非阻塞模式
                   new_socket.setblocking(False)
                   self.client_socket_list.append(new_socket)
   
               for client_socket in self.client_socket_list:
                   try:
                   
                   	# 为客户服务套接字 接收客户端request
                       request = client_socket.recv(1024).decode('utf-8')
                   except Exception as ret:
                       print("------2----", ret)  # for test
                   else:
                       if request:
                           self.deal_with_request(request, client_socket)
                       else:
                           client_socket.close()
                           self.client_socket_list.remove(client_socket)
   
               print(self.client_socket_list)
   
   
       def deal_with_request(self, request, client_socket):
           """为这个浏览器服务器"""
           if not request:
               return
   		
   		# 按 换行符 划分request数据 , 并返回list类型对象
           request_lines = request.splitlines()
           for i, line in enumerate(request_lines):
               print(i, line)
   
           `# 提取请求的 文件名称(index.html)
          `# GET /a/b/c/d/e/index.html HTTP/1.1
           ret = re.match(r"([^/]*)([^ ]+)", request_lines[0])
           if ret:
               print("正则提取数据:", ret.group(1))
               print("正则提取数据:", ret.group(2))
               file_name = ret.group(2)
               if file_name == "/":
                   file_name = "/index.html"
   
   
           `#服务器端: 读取 要请求的文件的 数据
           try:
               f = open(self.documents_root+file_name, "rb")
           except:
               response_body = "file not found, 请输入正确的url"
               response_header = "HTTP/1.1 404 not found\r\n"
               response_header += "Content-Type: text/html; charset=utf-8\r\n"
               response_header += "Content-Length: %d\r\n" % (len(response_body))
               response_header += "\r\n"
   
               #服务器端: 将header返回给浏览器
               client_socket.send(response_header.encode('utf-8'))
   
               #服务器端: 将body返回给浏览器
               client_socket.send(response_body.encode("utf-8"))
           else:
           	#服务器端: 读取文件f
               content = f.read()
               f.close()
   
               response_body = content
               response_header = "HTTP/1.1 200 OK\r\n"
               response_header += "Content-Length: %d\r\n" % (len(response_body))
               response_header += "\r\n"
   
               # 将header返回给浏览器
               client_socket.send( response_header.encode('utf-8') + response_body)
   
   
   `# 设置服务器服务静态资源时的路径
   DOCUMENTS_ROOT = "./html"
   
   
   def main():
       """服务器端: 控制web服务器整体"""
       # python3 xxxx.py 7890, argv=['xxxx.py', '7890']
       if len(sys.argv) == 2:
           port = sys.argv[1]
           if port.isdigit():
               port = int(port)
       else:
           print("运行方式如: python3 xxx.py 7890")
           return
   
       print("http服务器使用的port:%s" % port)
       http_server = WSGIServer(port, DOCUMENTS_ROOT)
       http_server.run_forever()
   
   if __name__ == "__main__":
          main()
   ```

#### 10.2 Web静态服务器-6-epoll

1. I/O复用和(非)阻塞机制
   
   1. I/O复用:
   
      1. 计算机阻塞在一组数据流上，直到某个数据流到达唤醒阻塞的进程
      2. 此时I/O信道通过一组数据流, 即复用
   
   2. [阻塞和非阻塞](https://www.quora.com/What-is-the-difference-between-blocking-and-non-blocking)：
   
      1. I/O为例子
      2. 阻塞模型，程序等到有数据才向下执行，否则一直等待
      3. 非阻塞模型，有数据，读取数据向下执行，没有数据也向下执行
   
   3. `select`、`poll`和`epoll` 三个函数: Linux系统中**I/O复用**的**系统调用**函数
   
      1. 即 linux的 I/O 是阻塞模型
   
         1. I/O复用,  三个函数同时监听多个文件描述符()(File Descriptor, FD)，
         2. 每个文件描述符相当于一个需要 I/O的“文件”
         3. socket共用一个端口
   
      2. 三个函数的本身是阻塞, 故服务器 默认是串行的
   
      3. select
   
         1. 三者中最底层的，它的事件的轮训机制 基于比特位。每次查询要遍历整个事件列表。
   
         2. select要处理一个`fd_set`结构
            1. `fd_set` 是长度1024的比特位
               1. 每个比特位表示一个需要处理的FD
               2. 如果是1，这个FD有要处理的I/O事件，否则没有。
               3. Linux简化位操作，定义了一组宏函数处理这个比特位数组。
            2. select处理fd最大的数量是1024，由fd_set的容量决定的
            
         3. select的调用方式
            1. `int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);`
               1. 参数:
                  1. nfds：表示表示文件描述符最大的数目+1，这个数目是指读事件和写事件中数目最大的，+1是为了全面检查
                  2. readfds：表示需要监视的会发生读事件的fd，没有设置为NULL
                  3. writefds：表示需要监视的会发生写事件的fd，没有设置为NULL
                  4. exceptfds：表示异常处理的，暂时没用到。。。
                  5. timeout：表示阻塞的时间，如果是0表示非阻塞模型，NULL表示永远阻塞，直到有数据来
               2. 返回值:
                  1. 三个类型的返回值
                  2. 正数： 表示`readfds`和`writefds`就绪事件的总数
                  3. 0：超时返回0
                  4. -1：出现错误
            
         4. 通用模型
   
            1. ```
               int main() {
               
               
                 fd_set read_fs, write_fs;
                 struct timeval timeout;
                 int max_sd = 0;  // 用于记录最大的fd，在轮询中时刻更新即可
                 
                 /*
                  * 这里进行一些初始化的设置，
                  * 包括socket建立，地址的设置等,
                  * 同时记得初始化max_sd
                  */
               
                 // 初始化比特位
                 FD_ZERO(&read_fs);
                 FD_ZERO(&write_fs);
               
                 int rc = 0;
                 int desc_ready = 0; // 记录就绪的事件，可以减少遍历的次数
                 while (1) {
                   // 这里进行阻塞
                   rc = select(max_sd + 1, &read_fd, &write_fd, NULL, &timeout);
                   if (rc < 0) {
                     // 这里进行错误处理机制
                   }
                   if (rc == 0) {
                     // 这里进行超时处理机制
                   }
               
                   desc_ready = rc;
                   // 遍历所有的比特位，轮询事件
                   for (int i = 0; i <= max_sd && desc_ready; ++i) {
                     if (FD_ISSET(i, &read_fd)) {
                       --desc_ready;
                       // 这里处理read事件，别忘了更新max_sd
                     }
                     if (FD_ISSET(i, &write_fd)) {
                       // 这里处理write事件，别忘了更新max_sd
                     }
                   }
                 }
               }
               #https://blog.csdn.net/qq_35976351/article/details/85228002
               ```
   
      4. poll
   
         1. 增强版本的`select`
   
         2. poll`底层操作的数据结构`pollfd`
   
            1. ```
               struct pollfd {
               	int fd;          // 需要监视的文件描述符
               	short events;    // 需要内核监视的事件
               	short revents;   // 实际发生的事件
               };
               ```
   
            2. 使用该结构时, 对事件本身操作, 不用对比特位的操作
   
            3. 初始化
   
               1. 默认初始化都是0
               2. 通过bzero或者memset统一初始化即可
               3. 之后在events上注册感兴趣的事件，监听的时候在revents上监听即可。
   
            4. 注册查询
   
               1. 注册事件用`|`操作，
               2. 查询事件用`&`操作。
               3. 示例:
                  1. 注册POLLIN数据到来的事件，要`pfd.events |= POLLIN`，
                     1. 注册多个事件多次`|`操作即可。
                  2. 取消事件 `~`操作，如 `pfd.events ~= POLLIN`。
                  3. 查询事件：pfd.revents & POLLIN。
   
         3. `poll`函数
   
            1. ```
               #include <poll.h>
               int poll(struct pollfd* fds, nfds_t nfds, int timeout);
               ```
   
               1. 参数:
   
                  1. `fds`：一个`pollfd`队列的队头指针，先把需要监视的文件描述符和上面的事件放到这个队列中
                  2. `nfds`：队列的长度
                  3. `timeout`：事件操作，设置指定正数的阻塞事件
                     1. 0表示非阻塞模式，-1表示永久阻塞。
   
               2. 时间的数据结构：
   
                  1. ```
                     struct timespec {
                     	long    tv_sec;         /* seconds */
                         long    tv_nsec;        /* nanoseconds */
                     };
                     ```
   
         4. poll 常用模型
   
            1. ```
            // 先宏定义长度
               #define MAX_POLLFD_LEN 200  
            
               int main() {
                 /*
                  * 在这里进行一些初始化的操作，
                  * 比如初始化数据和socket等。
               */
               
               ```
            
              int rc = 0;
                 pollfd fds[MAX_POLL_LEN];
                 memset(fds, 0, sizeof(fds));
                 int ndfs  = 1;  // 队列的实际长度，是一个随时更新的，也可以自定义其他的
                 int timeout = 0;
                 /*
                  * 在这里进行一些感兴趣事件的注册，
                  * 每个pollfd可以注册多个类型的事件，
                  * 使用 | 操作即可，就行博文提到的那样。
                  * 根据需要设置阻塞时间
                  */
            
                 int current_size = ndfs;
                 int compress_array = 0;  // 压缩队列的标记
              while (1) {
                   rc = poll(fds, nfds, timeout);
                if (rc < 0) {
                   // 这里进行错误处理
                   }
                if (rc == 0) {
                   // 这里进行超时处理
                }
            
                   for (int i = 0; i < current_size; ++i) {
                  if (fds[i].revents == 0){  // 没有事件可以处理
                       continue;
                     }
                     if (fds[i].revents & POLLIN) {  // 简单的例子，比如处理写事件
                     
                     }
                     /*
                      * current_size 是为了降低复杂度的，可以随时进行更新
                      * ndfs如果要更新，应该是最后统一进行
                      */
                   }
                   
                   if (compress_array) {  // 如果需要压缩队列
                     compress_array = 0;
                     for (int i = 0; i < ndfs; ++i) {
                       for (int j = i; j < ndfs; ++j) {
                         fds[i].fd = fds[j + i].fd;
                       }
                       --i;
                       --ndfs;
                     }
                   }
                 }
               }
               ————————————————
               https://blog.csdn.net/qq_35976351/article/details/85228002
            
               ```
            
               ```
         
      5. epoll函数
   
         1. 更加高级的操作
   
            1. 对比
   
               1. | 函数          | 特点                                                         |
                  | ------------- | ------------------------------------------------------------ |
                  | epoll         | 直接申请一个`epollfd`的文件. 对这些进行统一的管理. 有了面向对象的思维模式 |
                  | select / poll | 要轮询队列逐一判断是否有事件, 事件队列是直接暴露给调用者的，比如上面`select`的`write_fd`和`poll`的`fds`. 复杂度高，而且容易误操作 |
   
         2. epoll的数据结构
   
            1. ```
               typedef union epoll_data {
               	void *ptr;
               	int fd;
               	uint32_t u32;
               	uint64_t u64;
               } epoll_data_t;
               
               struct epoll_event {
               	uint32_t events;
               	epoll_data_t data;
               };
               ```
   
            2. `epoll_data`是一个`union`类型。
   
               1. fd 是文件描述符；
                  1. 文件描述符: 本质是内核中资源地址的下标描述，可用`ptr`指针代替
   
            3. `epoll_event`，
   
               1. `data` 表示`fd`
               2. `events`表示注册的事件。
   
         3. `epoll`通过一组函数进行
   
            1. 创建`epollfd`, 2个
   
               1. ```
                  #include <sys/epoll.h>
                  int epoll_create(int size);
                  ```
   
               2. ```
                  #include <sys/epoll.h>
                  int epoll_create1(int flag);
                  ```
   
            2. 处理事件
   
               1. ```
                  #include <sys.epoll.h>
                  int epoll_ctl(int epfd, int op, int fd, struct epoll_event* event);
                  ```
   
            3. 等待事件
   
               1. `int epoll_wait(int epfd, struct epoll_event* evlist, int maxevents, int timeout);`
   
         4. epoll的工作模式: ET和LT
   
            1. ET模式是默认模式
               1. 也是select和poll的模式.
               2. 只要有事件发生，就会被epoll_wait捕获，
               3. 如果一次读写没有完成，会在下一次epoll_wait调用时接着被捕获；
               4. ET边沿触发模式是读写没完成，下次不会被捕获，之后新的数据到达时才会触发。
   
         5. EPOLLONESHOT事件
   
            1. epoll特有的事件
            2. 操作系统上最多触发文件描述符上注册的一个可读、可写或者异常事件，只能触发一次
               1. 除非使用epoll_ctl重置该描述符
            3. 多线程编程时常用到，处理完毕后需要重新复原。
   
         6. epoll的一个模板
   
            1. ```
               #define MAX_EVENTS 10
               int main() {
               	struct epoll_event ev, events[MAX_EVENTS];
                   int listen_sock, conn_sock, nfds, epollfd;
               
                   /* Code to set up listening socket, 'listen_sock',
                    (socket(), bind(), listen()) omitted */
               
               	epollfd = epoll_create1(0);
               	if (epollfd == -1) {
               		perror("epoll_create1");
                     	exit(EXIT_FAILURE);
               	}
               
               	ev.events = EPOLLIN;
               	ev.data.fd = listen_sock;
               	if (epoll_ctl(epollfd, EPOLL_CTL_ADD, listen_sock, &ev) == -1) {
               		perror("epoll_ctl: listen_sock");
               		exit(EXIT_FAILURE);
               	}
               
               	for (;;) {
               	    // 永久阻塞，直到有事件
               		nfds = epoll_wait(epollfd, events, MAX_EVENTS, -1);
               		if (nfds == -1) {  // 处理错误
               			perror("epoll_wait");
               			exit(EXIT_FAILURE);
               		}
               
               		for (n = 0; n < nfds; ++n) {
               			if (events[n].data.fd == listen_sock) {
               				conn_sock = accept(listen_sock, (struct sockaddr *) &addr, &addrlen);
               				if (conn_sock == -1) {
               					perror("accept");
               					exit(EXIT_FAILURE);
               				}
               				setnonblocking(conn_sock);
               				ev.events = EPOLLIN | EPOLLET;
               				ev.data.fd = conn_sock;
               				if (epoll_ctl(epollfd, EPOLL_CTL_ADD, conn_sock, &ev) == -1) {
               					perror("epoll_ctl: conn_sock");
               					exit(EXIT_FAILURE);
               				}
               			} else {
               				do_use_fd(events[n].data.fd);
               			}
               		}
               	}
               	return 0;
               }
               ————————————————
               https://blog.csdn.net/qq_35976351/article/details/85228002
               ```
   
   1. select()，poll()，epoll()的IO方式为event driven IO
   2. select/epoll的好处:  一个`process` 同时处理多个网络连接的IO
   3. 基本原理:
      1. select，poll，epoll 函数不断的轮询所负责的socket
      2. 当某个socket有数据到达，通知用户进程。
   
2. epoll简单模型

   1. ```
      import socket
      import select
      
      `# 创建套接字
      s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
      
      `# 设置可以重复使用绑定的信息
      s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR,1)
      
      `# 绑定本机信息
      s.bind(("",7788))
      
      `# 变为被动
      s.listen(10)
      
      `# 创建一个epoll对象
      epoll = select.epoll()
      
      `# 测试，用来打印套接字对应的文件描述符
      `# print(s.fileno())
      `# print(select.EPOLLIN|select.EPOLLET)
      
      `# 注册事件到epoll中
      `# epoll.register(fd[, eventmask])
      `# 注意，如果fd已经注册过，则会发生异常
      `# 将创建的套接字添加到epoll的事件监听中
      epoll.register(s.fileno(), select.EPOLLIN|select.EPOLLET)
      
      connections = {}
      addresses = {}
      
      `# 循环等待客户端的到来或对方发送数据
      while True:
      
          #epoll 进行 fd 扫描的地方 -- 未指定超时时间则为阻塞等待
          epoll_list = epoll.poll()
      
          # 对事件进行判断
          for fd, events in epoll_list:
      
              # print fd
              # print events
      
              #如果是socket创建的套接字,要激活
              if fd == s.fileno():
                  new_socket, new_addr = s.accept()
      
                  print('有新的客户端到来%s' % str(new_addr))
      
                  # 将 conn 和 addr 信息分别保存起来
                  connections[new_socket.fileno()] = new_socket
                  addresses[new_socket.fileno()] = new_addr
      
                  #向 epoll 中注册 新socket 的 可读 事件
                  epoll.register(new_socket.fileno(), select.EPOLLIN|select.EPOLLET)
      
              # 如果是客户端发送数据
              elif events == select.EPOLLIN:
                  #从激活 fd 上接收
                  recvData = connections[fd].recv(1024).decode("utf-8")
      
                  if recvData:
                      print('recv:%s' % recvData)
                  else:
                      # 从 epoll 中移除该 连接 fd
                      epoll.unregister(fd)
      
                      # server 侧主动关闭该 连接 fd
                      connections[fd].close()
                      print("%s---offline---" % str(addresses[fd]))
                      del connections[fd]
                      del addresses[fd]  
      ```

      1. 事件类型
         1. EPOLLIN （可读）
         2. EPOLLOUT （可写）
      3. EPOLLET （ET模式）
      
   2. epoll对 文件描述符 的操作 两种模式

      1. LT（level trigger）电平触发, LT模式是默认模式
      2. ET（edge trigger）边沿触发。ET模式在`不可读写`变化到`可读写`时，才触发

   3. LT模式与ET模式的区别

      1. LT模式：
         1. epoll检测到`描述符`的事件发生, 将事件通知应用程序
         2. 应用程序 可以**不**立即处理该事件
         3. 如果不处理, 下次调用epoll时，会再次提醒应用程序 通知此事件。
      2. ET模式
         1. epoll检测到描述符事件发生, 将此事件通知应用程序
         2. 应用程序必须**立即**处理该事件
         3. 如果不处理，下次**调用epoll**时，不 会再次提醒应用程序 通知此事件。

3. web静态服务器-epool

   1. 用 epoll解决` I/O 口复用`

   2. 代码.  http的`长连接`，用`Content-Length`

      ```
      import socket
      import time
      import sys
      import re
      import select
      
      
      class WSGIServer(object):
          """服务器上的程序: 定义一个WSGI服务器的类"""
      
          def __init__(self, port, documents_root):
      
              # 1. 创建套接字
              self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
              # 2. 绑定本地信息, 设置可 重复使用绑定的信息
              self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
              self.server_socket.bind(("", port))
              # 3. 变为监听套接字, 变为被动
              self.server_socket.listen(128)
      		
      		#文件目录
              self.documents_root = documents_root
      
              # 创建epoll对象
              self.epoll = select.epoll()
              
              # 将tcp服务器套接字加入到epoll中进行监听
              self.epoll.register(self.server_socket.fileno(), select.EPOLLIN|select.EPOLLET)
      
              # 创建 添加的fd对应的套接字的数据结构: 字典
              self.fd_socket = dict()
      
          def run_forever(self):
              """运行服务器"""
      
              # 等待对方链接
              while True:
                  # epoll 进行fd扫描的地方 -- 未指定超时时间则为阻塞等待
                  epoll_list = self.epoll.poll()
      
                  # 对事件进行判断
                  for fd, event in epoll_list:
                      # 如果服务器套接字可以收数据，意味着可以进行accept
                      if fd == self.server_socket.fileno():
                          new_socket, new_addr = self.server_socket.accept()
                          #epoll中注册 连接socket 的 可读事件
                          self.epoll.register(new_socket.fileno(), select.EPOLLIN | select.EPOLLET)
                          # 记录这个信息
                          self.fd_socket[new_socket.fileno()] = new_socket
                      # 接收到数据
                      elif event == select.EPOLLIN:
                          request = self.fd_socket[fd].recv(1024).decode("utf-8")
                          if request:
                              self.deal_with_request(request, self.fd_socket[fd])
                          else:
                              #epoll中注销客户端的信息
                              self.epoll.unregister(fd)
                              # 关闭客户端的文件句柄
                              self.fd_socket[fd].close()
                              #字典中删除与已关闭客户端相关的信息
                              del self.fd_socket[fd]
      
          def deal_with_request(self, request, client_socket):
              """服务器端程序: 为这个浏览器服务"""
      
              if not request:
                  return
      
              request_lines = request.splitlines()
              for i, line in enumerate(request_lines):
                  print(i, line)
      
              # 提取请求的文件(index.html)
              # GET /a/b/c/d/e/index.html HTTP/1.1
              ret = re.match(r"([^/]*)([^ ]+)", request_lines[0])
              if ret:
                  print("正则提取数据:", ret.group(1))
                  print("正则提取数据:", ret.group(2))
                  file_name = ret.group(2)
                  if file_name == "/":
                      file_name = "/index.html"
      
      
              # 读取文件数据
              try:
                  f = open(self.documents_root+file_name, "rb")
              except:
              	#服务器端构造响应信息
                  response_body = "file not found, 请输入正确的url"
      
                  response_header = "HTTP/1.1 404 not found\r\n"
                  response_header += "Content-Type: text/html; charset=utf-8\r\n"
                  response_header += "Content-Length: %d\r\n" % len(response_body)
                  response_header += "\r\n"
      
                  # 将header返回给浏览器
                  client_socket.send(response_header.encode('utf-8'))
      
                  # 将body返回给浏览器
                  client_socket.send(response_body.encode("utf-8"))
              else:
                  content = f.read()
                  f.close()
      
                  response_body = content
      
                  response_header = "HTTP/1.1 200 OK\r\n"
                  response_header += "Content-Length: %d\r\n" % len(response_body)
                  response_header += "\r\n"
      
                  # 将数据返回给浏览器
                  client_socket.send(response_header.encode("utf-8")+response_body)
      
      
      # 设置服务器服务静态资源时的路径
      DOCUMENTS_ROOT = "./html"
      
      
      def main():
          """控制web服务器整体"""
          # python3 xxxx.py 7890
          if len(sys.argv) == 2:
              port = sys.argv[1]
              if port.isdigit():
                  port = int(port)
          else:
              print("运行方式如: python3 xxx.py 7890")
              return
      
          print("http服务器使用的port:%s" % port)
          http_server = WSGIServer(port, DOCUMENTS_ROOT)
          http_server.run_forever()
      
      
      if __name__ == "__main__":
          main()
      ```

4. 小结

   1. I/O 多路复用
      1. select , poll , 或 epoll 三种解决方案
      2. 一种机制: 
         1. 一个进程能同时等待多个文件描述符
         2. 任意一个文件描述符（套接字描述符）进入读就绪状态，epoll()函数就可以返回。
      3. IO多路复用，本质上没有并发，因为任何时候 只有一个进程或线程工作
      4. IO复用提高效率原理. 
         1. select\epoll 把socket放到他们的 '监视' 列表，任一socket有可读可写数据立马处理
         2. select\epoll 同时检测着很多socket， 一有动静马上返回给进程处理，比一个一个socket过来,阻塞等待,处理高效S率。

#### 10.3 Web静态服务器-7-gevent版

1. gevent版本

   1. ```
      from gevent import monkey
      import gevent
      import socket
      import sys
      import re
      
      monkey.patch_all()
      
      
      class WSGIServer(object):
          """服务器端的程序: 定义一个WSGI服务器的类"""
      
          def __init__(self, port, documents_root):
      
              # 1. 创建套接字
              self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
              # 2. 绑定本地信息, 设置可 重复使用绑定的信息
              self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
              self.server_socket.bind(("", port))
              # 3. 变为监听套接字, 变为被动
              self.server_socket.listen(128)
      		#文档路径
              self.documents_root = documents_root
      
          def run_forever(self):
              """服务器端: 运行服务器"""
      
              # 等待对方链接
              while True:
                  new_socket, new_addr = self.server_socket.accept()
                  
                  # (处理为客户服务的套索字程序的引用, 为客户服务的套索字)
                  gevent.spawn(self.deal_with_request, new_socket)  #创建协程运行它
      
          def deal_with_request(self, client_socket):
              """服务器端程序: 为浏览器服务"""
              while True:
                  #服务器端接收客户端发来的数据
                  request = client_socket.recv(1024).decode('utf-8')
                  # print(gevent.getcurrent())
                  # print(request)
      
                  #客户端浏览器接收完数据，自动调用close关闭. 当其关闭后，服务器端也要关闭对应套接字
                  if not request:
                      client_socket.close()
                      break
      
                  request_lines = request.splitlines()
                  for i, line in enumerate(request_lines):
                      print(i, line)
      
                  # 提取请求的文件(index.html)
                  # GET /a/b/c/d/e/index.html HTTP/1.1
                  ret = re.match(r"([^/]*)([^ ]+)", request_lines[0])
                  if ret:
                      print("正则提取数据:", ret.group(1))
                      print("正则提取数据:", ret.group(2))
                      file_name = ret.group(2)
                      if file_name == "/":
                          file_name = "/index.html"
      
                  file_path_name = self.documents_root + file_name
                  try:
                      f = open(file_path_name, "rb")
                  except:
                      # 如果不能打开这个文件，那么意味着没有这个资源，没有资源 那么也得需要告诉浏览器 一些数据才行
                      # 404
                      response_body = "没有你需要的文件......".encode("utf-8")
      
                      response_headers = "HTTP/1.1 404 not found\r\n"
                      response_headers += "Content-Type:text/html;charset=utf-8\r\n"
                      
                      # Content_Length, 长连接http
                      response_headers += "Content-Length:%d\r\n" % len(response_body)
                      response_headers += "\r\n"
      
                      send_data = response_headers.encode("utf-8") + response_body
      
                      client_socket.send(send_data)
      
                  else:
                      content = f.read()
                      f.close()
      
                      # 响应的body信息
                      response_body = content
                      # 响应头信息
                      response_headers = "HTTP/1.1 200 OK\r\n"
                      response_headers += "Content-Type:text/html;charset=utf-8\r\n"
                      response_headers += "Content-Length:%d\r\n" % len(response_body)
                      response_headers += "\r\n"
                      send_data = response_headers.encode("utf-8") + response_body
                      client_socket.send(send_data)
      
      #服务器服务静态资源的路径
      DOCUMENTS_ROOT = "./html"
      
      def main():
          """控制web服务器整体"""
          # python3 xxxx.py 7890
          if len(sys.argv) == 2:
              port = sys.argv[1]
              if port.isdigit():
                  port = int(port)
          else:
              print("运行方式如: python3 xxx.py 7890")
              return
      
          print("http服务器使用的port:%s" % port)
          http_server = WSGIServer(port, DOCUMENTS_ROOT")
          http_server.run_forever()
      
      
      if __name__ == "__main__":
          main()
      ```



#### 10.4  知识扩展-C10K问题

1. 《单台服务器并发TCP连接数到底可以有多少》 http://www.52im.net/thread-561-1-1.html
2. 《上一个10年，著名的C10K并发连接问题》 http://www.52im.net/thread-566-1-1.html





----



视频

#### 04-并发web服务器 实现 

##### 4.1-http协议复习

##### 4.2-多进程、多线程实现http服务器

1. 多进程实现.  进程特点:  全局变量, 局部变量都复制. 使用资源多

   1. 文件描述符,   文件名与文件硬链接

   2. ```
      #多进程实现并发web服务器
      #服务器端程序
      import multiprocessing
      
      def main():
      	"""用来完成整体的控制"""
      	# 1. 服务器端 创建套接字 对象
      	tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
      	
      	# 2. 绑定 服务器端 IP, Port
      	tcp_server_socket.bind(("", 7890))  
      	
      	# 3. 变成 监听套接字
      	tcp_server_socket.listen(128)
      	
      	while True:
      		# 4. 等待新客户端的连接, 客户服务套接字new_socket
      		new_socket, client_addr = tcp_server_socket.accept()
      	
      		# 5.创建线程: 为这个客户端服务
      		p = multiprocessing.Process(target=service_client,args=(new_socket))
      		p.start()
      		
      		# 6. 关闭主进程中new_socket, 子进程中已经复制了一份new_socket
      		new_socket.close()
      	# 关闭监听套接字
      	tcp_server_socket.close()
      ```
      
      1. 创建进程:  multiprocessing.Process()
   
2. 线程实现. 

   1.  线程特点: 资源共享, 不复制, 故不能在main中关闭 new_socket.close()

   2. ```
      import threading
      
      def main():
      	"""用来完成整体的控制"""
      	# 1. 创建套接字
      	tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
      	
      	# 2. 绑定
      	tcp_server_socket.bind(("", 7890))  
      	
      	# 3. 变成监听套接字
      	tcp_server_socket.listen(128)
      	
      	while True:
      		# 4. 等待新客户端的连接, 客户服务套接字new_socket
      		new_socket, client_addr = tcp_server_socket.accept()
      	
      		# 5. 为这个客户端服务
      		t = threading.Thread(target=service_client,args=(new_socket))
      		t.start()
      		
      		# 6. 线程共用资源new_socket, new_socket不能关闭
      		# new_socket.close()
      	# 关闭监听套接字
      	tcp_server_socket.close()
      ```

      1. 线程 : threading.Thread()

3. linux中vim命令

   1.   esc :  # 进入编辑模式 
   2.   %s/multiprocessing/threading/g    #multiprocessing 用 threading替换,

##### 4.3-多进程、线程实现http服务器-补充

1. 用户增多, 进程, 线程资源限制了

##### 4.4-gevent实现http服务器, 

1. ```
   import gevent
   from gevent import monkey
   
   monkey.patch_all()   # 时间延迟,自动转为gevent代码
   
   def main():
   	"""用来完成整体的控制"""
   	# 1. 创建套接字
   	tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   	
   	# 2. 绑定
   	tcp_server_socket.bind(("", 7890))  
   	
   	# 3. 变成监听套接字
   	tcp_server_socket.listen(128)
   	
   	while True:
   		# 4. 等待新客户端的连接
   		new_socket, client_addr = tcp_server_socket.accept()
   	
   		# 5. 为这个客户端服务
   		gevent.spawn(service_client,new_socket)
   		
   		# 6. 线程共用资源,new_socket不能关闭
   		# new_socket.close()
   	# 关闭监听套接字
   	tcp_server_socket.close()
   ```

   1.  协程更快, 一个单进程, 一个单线程, 多个函数一起跑
   2.  协程 :  gevent.spawn()

##### 4.5-单进程、单线程  非堵塞方式 实现并发的原理

1. 服务器端只有一个单进程或 只有一个单线程, 非堵塞实现并发 . 没用协程实现 

2. ```
   tcp_socket_tcp = socket()
   
   tcp_server_tcp.setblocking(False)  # 设置套接字为非阻塞的方式
   client_socket_list = list()
   
   while True:
   	try:
   		new_socket, new_addr = tcp_server_tcp.accept()  # 接收客户端套接字
   	except Exception as ret:
   		print("--没有客户端--")
       else:
       	print("--没有产生异常, 有一个新的客户端--")
       	new_socket.setblocking(False)  # 设置套接字为非阻塞的方式
       	client_socket_list.append(new_socket)
       for client_socket in client_socket_list:
       	try:
       		client_socket.recv()  #接收客户端数据
       	except Exception as ret:
       		print("--客户端没有发送数据--")
       	else:
       		print("--客户端发送数据--")
   ```

   1. new_socket.setblocking(False)  # 设置套接字为 非阻塞方式
   
      

##### 4.6-单进程、线程、非堵塞实现并发的验证-1

1. 服务器端只有一个单进程或 只有一个单线程, 非堵塞实现并发.  没用协程实现 

   ```
   tcp_socket_tcp = socket(socket.AF_INET, socket.SOCK_STREAM)
   tcp_socket_tcp.bind(("", 7890))
   tcp_socket_tcp.listen(128)
   tcp_server_tcp.setblocking(False)  # 设置套接字为非阻塞的方式
   
   client_socket_list = list()
   
   while True:
   	try:
   		# 为客户服务套接字 new_socket
   		new_socket, new_addr = tcp_server_tcp.accept()  # 接收客户端套接字
   	except Exception as ret:
   		print("--没有客户端--")
       else:
       	print("--没有产生异常, 有一个新的客户端--")
       	new_socket.setblocking(False)  # 设置客户套接字为非阻塞的方式
       	client_socket_list.append(new_socket)
       for client_socket in client_socket_list:
       	try:
       		# 为客户服务套接字 client_socket(即: new_socket)
       		recv_data = client_socket.recv(1024)  #接收客户端数据
       	except Exception as ret:
       		print("--客户端没有发送数据--")
       	else:
       		if recv_data:
       			# 对方发送过来数据
       			print("--客户端发送过来数据--")
               else:
               	# 对方调用close 导致 recv空
               	client_socket_list.remove(client_socket)
            	client_socket.close()
   ```
   
   1. new_socket.setblocking(False)  # 设置 为客户服务套接字 为非阻塞的方式
   2. 原理:
      1. 用 列表 存储各客户套接字, 设置服务器端套接字, 为各客户服务套接字为 非阻塞模式, 
      2. 当 服务器进程 能处理一个新套接字时, 立即处理一个客户套接字

##### 4.7-单进程、线程、非堵塞实现并发的验证-2、debug的思想

1.  网络读取数据流程
    1.  网络传过来 数据 存于 电脑缓存区
    2.  应用程序每过 一段时间 来缓存区读取一次

##### 4.8-长连接、短连接

1. 短连接: 
   1. 每 一次 取数据都经过 ` 三次握手, 取数据, 四次挥手断开`
   2. HTTP/1.0
   3. 每 一次 都新创建`一个`套索字
2. 长连接:
   1. 取所有数据 : 只有一遍 `三次握手, 四次挥手`
   2. HTTP/1.1
   3. 同`一个`套接字
   4. 减少资源浪费

##### 4.9-单进程、或单线程、非堵塞、长连接的http服务器

1. ```
   import socket
   import re
   
   
   def service_client(new_socket, request):
       """服务器端程序: 为这个客户端返回数据"""
   
       # 1. 接收浏览器发送过来的请求 ，即 http请求  
       # GET / HTTP/1.1
   	
   	# 以\r\n, \r,\n等换行符 划分,返回list
       request_lines = request.splitlines()
   
       # GET /index.html HTTP/1.1
       # get post put del
       # 数据处理, 获取 请求文件名
       file_name = ""
       ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
       if ret:
           file_name = ret.group(1)
           if file_name == "/":
               file_name = "/index.html"
   
       # 2. 返回http格式的数据，给浏览器    
       try:
           f = open("./html" + file_name, "rb")
       except:
       	# 没有 找到文件
           response = "HTTP/1.1 404 NOT FOUND\r\n"
           response += "\r\n"
           response += "------file not found-----"
           new_socket.send(response.encode("utf-8"))
       else:
       	# 找到文件
           html_content = f.read()  # 从服务器端 磁盘 读入内存
           f.close()
   		
   		# 文件内容content
           response_body = html_content
   
           response_header = "HTTP/1.1 200 OK\r\n"
           #设置长连接. 浏览器依此要求设置长连接
           response_header += "Content-Length:%d\r\n" % len(response_body)
           response_header += "\r\n"
   
           response = response_header.encode("utf-8") + response_body
   
           new_socket.send(response)
   
   
   def main():
       """用来完成整体的控制"""
       # 1. 创建套接字
       tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
       tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
   
       # 2. 绑定
       tcp_server_socket.bind(("", 7890))
   
       # 3. 变为监听套接字
       tcp_server_socket.listen(128)
       tcp_server_socket.setblocking(False)  # 将服务器端套接字变为非堵塞
   
       client_socket_list = list()
       while True:
           # 4. 等待新客户端的链接
           try:
           	# 生成为 客户服务套接字
               new_socket, client_addr = tcp_server_socket.accept()
           except Exception as ret:
               pass
           else:
           	# 为客户服务套接字 设为非阻塞
               new_socket.setblocking(False)
               client_socket_list.append(new_socket)
   
   
           for client_socket in client_socket_list:
               try:
                   recv_data = client_socket.recv(1024).decode("utf-8")
               except Exception as ret:
                   pass
               else:
                   if recv_data:
                   	# 调用函数, 响应客户端请求 向客户端发送数据
                       service_client(client_socket, recv_data)
                   else:
                       client_socket.close()
                       client_socket_list.remove(client_socket)
   
       # 关闭监听套接字
    tcp_server_socket.close()
   
   
   if __name__ == "__main__":
       main()
   ```
   
      1. 长连接策略: response header中放入`Content-Length:文件字节长度` ,  
      2.  encode('utf-8')  文件按utf-8解析成二进制,  decode("utf-8")  内容按utf-8从二进制解析为字符
   
2. linux命令

   1.  `vim 文件名 +93`  # 打开"文件名"并跳到93行

----------

https://blog.csdn.net/armlinuxww/article/details/92803381?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163523885416780357277615%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163523885416780357277615&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-92803381.first_rank_v2_pc_rank_v29&utm_term=epoll%E7%9A%84%E5%8E%9F%E7%90%86&spm=1018.2226.3001.4187

##### 4.10-（重要）epoll的原理过程讲解

1.   linux 服务器 采样方式: epoll .  没有采样多进程 或 多线程.
2. epoll服务器特定:

   1.  epoll 采用 `内存映射(mmap)` 技术
       1.  监听 套接字socket 特有的`内存空间`,  
       2.  `应用程序` 与 `Kernel内核` 共有 该内存空间
   2.  epoll `事件通知` 方式 访问, 效率高
        1.  事件通知 不是 for 轮询.
   3.  基于上述 `2 特点`, 决定了效率高
   4.  epoll 操作系统 快
3.   select/epoll
     1.   select/epoll好处: 单个process(进程)  同时处理 多个网络IO。
     2.   select/epoll基本原理: select，poll，epoll这3个function会不断的`轮询`所负责 socket，当某个socket有数据到达, 就通知`用户进程`。
4.   epoll模式: EPOLLIN （可读）, EPOLLOUT （可写）, EPOLLET （ET模式）
5.   epoll对文件描述符的操作: 
   1.   两种模式：
      1.   LT（level trigger）
           1.   LT模式是默认模式
      2.   ET（edge trigger）
   2.   LT模式与ET模式的区别如下：
      1. LT : 不立即通知
      2. ET : 立即通知
6.   操作系统的内存空间是独立的

----------

##### 4.11-epoll版的http服务器

1. ```
   import socket
   import re
   import select
   
   def service_client(new_socket, request):
       """为这个客户端返回数据"""
       """构造客户端请求的文件对应的 response"""
       
       
   def main():
       """用来完成整体的控制"""
       # 1. 创建套接字
       tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
       tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
   
       # 2. 绑定
       tcp_server_socket.bind(("", 7890))
   
       # 3. 变为监听套接字
       tcp_server_socket.listen(128)
       tcp_server_socket.setblocking(False)  # 将套接字变为非堵塞
   
       # 创建一个 epoll对象
       epl = select.epoll()
   	
   	#fd : 套接字对应的文件描述符
       #监听套接字对应的 fd 注册到epoll中, register(fd, select.EPOLLIN)
       epl.register(tcp_server_socket.fileno(), select.EPOLLIN)
   	
   	# 文件描述符_事件_字典: {fd: new_socket_1,....}
       fd_event_dict = dict()
   
       while True:
   
           fd_event_list = epl.poll()  # 默认堵塞，直到 os监测到数据到来, 通过 事件通知 方式告诉这个程序，此时才 解堵塞
   
           # [(fd, event), (套接字对应的文件描述符, 这个文件描述符到底是什么事件 如 可以调用recv接收等)]
           for fd, event in fd_event_list:
               # 等待新客户端的链接
               if fd == tcp_server_socket.fileno():
                   new_socket, client_addr = tcp_server_socket.accept()
                   
                   # 服务客户套接字对应的 fd 注册到epoll中. register(fd, fd对应事件)
                   epl.register(new_socket.fileno(), select.EPOLLIN)
                   fd_event_dict[new_socket.fileno()] = new_socket
               elif event==select.EPOLLIN:
                   # 判断 链接的客户端是否 有数据 发送过来
                   recv_data = fd_event_dict[fd].recv(1024).decode("utf-8")
                   if recv_data: #接收到客户端浏览器请求
                       service_client(fd_event_dict[fd], recv_data)
                   else:  #没有收到客户端浏览器请求
                       fd_event_dict[fd].close()
                       epl.unregister(fd)
                       del fd_event_dict[fd]
   
   
       # 关闭监听套接字
       tcp_server_socket.close()
   
   
   if __name__ == "__main__":
       main()
   ```

   1. select 模块中导入epoll类

      2. epl = select.epoll()  # 创建epoll对象,  存储套接字的专用内存空间

      3. epl.register(tcp_server_socket.fileno(), select.EPOLLIN)  # 将监听套接字对应的fd注册到epoll中

         1. fd 文件描述符.  linux中每个文件都有 文件描述符
         2. 监听套接字注册 用 fd注册 实现,  tcp_server_socket.fileno() 套接字文件描述符
         3.  select.EPOLLIN  # 判断 fd对应的套接字是否有 输入

      4. fd_event_list = epl.poll()  # epoll对象默认 堵塞，直到 os监测到数据, 通过 `事件通知` 方式告知程序，解除堵塞

         1. fd_event_list 是 list类型, # [(fd, event), (套接字对应的文件描述符, 文件描述符的事件 如 recv接收等)]

      5. fd == tcp_server_socket.fileno()  #监听套接字有输入

         1. new_socket, client_addr = tcp_server_socket.accept()  # 接收并生成 客户套接字, 客户addr

         2. epl.register(new_socket.fileno(), select.EPOLLIN)  # 在 epoll中 注册 客户套接字

            fd_event_dict[new_socket.fileno()] = new_socket   # 客户套接字添加到字典, 事件通知时用

      6. event==select.EPOLLIN  # 客户端发来数据

         recv_data = fd_event_dict[fd].recv(1024).decode("utf-8")    # 接收数据

      7. epl.unregister(fd)  # epl对象删除 fd注册

-----

### 11 网络通信过程

#### 11.1  网络通信过程

1. 2台电脑的网络
   
   1. 两台电脑 `网线直连` 直接通信
   2. 提前设置好ip地址, 网络掩码
      1. ip地址 在同一网段内, 才可通信
      
      2. 如 一台 `192.168.1.1`另一台 `192.168.1.2`
      
         ```
         ┌───────────────────┐                  ┌───────────────────┐
         │  PC1: 192.168.1.1 │──────────────────│  PC2: 192.168.1.3 │ 
         └───────────────────┘                  └───────────────────┘
         ```
      
         
   
2. 集线器(Hub)组成一个网络

   1. ```ascii
                                                                                                       ┌────────────────────────────┐
                                                                                 ┌─────────│  PC-PT  PC1: 192.168.1.1   │
                                                                                 │         └────────────────────────────┘
      ┌───────────────┐────────┘         ┌────────────────────────────┐
      │  Hub-PT Hub0  │──────────────────│  PC-PT  PC2: 192.168.1.2   │
      └───────────────┘────────┐         └────────────────────────────┘
                               │         ┌────────────────────────────┐
                               └─────────│  PC-PT  PC3: 192.168.1.3   │
                                         └────────────────────────────┘
      ```

   2. 缺点:

      1.  集线器的接口 少
      2. 集线器: 广播的方式 发送数据
         1. Hub 收到 A电脑的数据 转发给B电脑
         2. Hub同时连接电脑C、D
         3. Hub向每台电脑发送一份数据，会导致网络拥堵

3. `交换机` 组成一个网络

   1.     
              ┌─────────────────────────┐
              │ PC-PT  PC0: 192.168.1.1 │──────┐     
              └─────────────────────────┘      │        
                                            ┌────────┐    ┌─────────────────────────┐
                                            │ Switch │────│ PC-PT  PC2: 192.168.1.2 │
                                            └────────┘    └─────────────────────────┘
              ┌─────────────────────────┐      │    
              │ PC-PT  PC2: 192.168.1.3 │──────┘
              └─────────────────────────┘           
   
   2. 特点:
      1. 克服了集线器以广播发送数据的缺点
      2. 已经替代 集线器
      3. 企业 交换机组网
   
4. `路由器`连接多个网络

   1. ```
      ┌──────────┐                                          ┌──────────┐
      │PC-PT PC0 │──────┐                          ┌────────│PC-PT PC3 │
      └──────────┘      │                          │        └──────────┘
      ┌──────────┐   ┌───────┐    ┌───────┐    ┌───────┐    ┌──────────┐
      │PC-PT PC1 │   │Switch0│────│Router0│────│Switch1│    │PC-PT PC4 │
      └──────────┘   └───────┘    └───────┘    └───────┘    └──────────┘
      ┌──────────┐      │                          │        ┌──────────┐
      │PC-PT PC2 │──────┘                          └──────  │PC-PT PC5 │ 
      └──────────┘                                          └──────────┘
      ```

5. 云通信（复杂）

   1. ```
      多台电脑---交换机1---路由器0---云1---...---云2---路由器2---交换机2---多台DNS服务器
                                        │───云3---路由器2---交换机2---多台Http服务器
      ```

   2. 说明

      1. 浏览器 输入 域名，解析出ip地址  (DNS服务器)
      2. 客户端收到ip地址，浏览器以 tcp方式 3次握手 链接Http服务器
      3. 浏览器以 tcp方式 发送 http协议 的请求数据 给 Http服务器
      4. Http服务器以 tcp方式 回应 http协议 的应答数据 给浏览器

6. 总结

   1. MAC地址：网卡的序列号, 唯一的, 设备与设备 数据通信时 标记收发双方
   2. IP地址：（相当于电脑的序列号）, 逻辑上标记 电脑，指定数据包的收发方向
   3. 网络掩码： 区分 ip地址 的 网络号和主机号, 又称 子网掩码
   4. 默认网关：通信目的ip不在同一个网段 时，先发送给默认的一台电脑，这台电脑成为网关
   5. 集线器(hub)：已过时,连接多态电脑，缺点：每次收发数据都 广播，网络拥堵
   6. 交换机：集线器的升级版. 有学习功能知道数据发送给哪台设备，根据需要进行单播、广播
   7. 路由器：连接多个不同的网段，让他们之间进行收发数据，每次收到数据后，ip不变，但是MAC地址会变化
   8. DNS：解析出IP（类似电话簿）
   9. http服务器：提供浏览器访问到的数据

#### 11.2 NAT(网络地址转换器)

1. net address transition(网络地址转换)
2. 只有公网ip地址才能上网
3. 光猫:  电脑 <---->路由器 <---->光猫 <---->万维网 <--->....
4. 电脑（192.168.1.2）上网，先通过DNS协议解析 域名 对应的ip

   1. 发送数据: 经 路由器( 192.168.1.1)  转换 电脑ip为`公网ip` 及路由器分配的 `临时端口`
   1. 192.168.1.2:6789-->192.168.1.1 路由器 116.226.52.212:6539-->猫-->万维网
   2. 接收数据:经 路由器 转换为路由器之前 记录的ip及port
      1. 万维网-->猫-->116.226.52.212:6539 路由器( 192.168.1.1) -->192.168.1.2:6789

--------------

视频:

#### 05-网络通信

##### 5.1-tcp ip协议

1. 100多种协议, 

2.  网络分成四层:  (Http--(TCP, UDP)--IP--mac)
   1.  应用层,  组成:  `内容+协议`,  如HTTP
      1. http是 浏览器 协议,   UDP端口80 与TCP端口80 不冲突
   2.  传输层 : 加上 `端口`(源,目的端口), 校验
      1. `TCP` , `UDP`
   3.  网际层(IP层):  加上 `IP`
      1. 网络控制协议 `IP`, ` ICMP`
      2. IP 相当于电脑的序列号, 
   4.  网络接口层: 加上 帧头(帧尾) 即 加上 `mac地址`
      1. 网卡序列号, 即设备的Id
3. 组包解包:  发送方组包, 接收方解包

4. 两种划分
   1. OSI     :   <u>应用层   表示层  会话层</u>, 传输层, 网络层, <u>数据链路层  物理层</u>
   2. TCP/IP:             应用层                  , 运输层, 网际层,  网络接口层

1.  原生套接字 可以 直接传入IP

##### 5.2-wireshark抓包工具-安装

1. 安装windows. 但 linux, max用不了
   1. 打钩, 否则只安了外壳

##### 5.3-wireshark抓包工具-使用

1. 192.168.33.255  广播地址
2. ip.dst == 192.168.33.45 and upd
3. udp.port  == 2225

##### 5.4-2台电脑通信、网络掩码

1. 2电脑: 
   1. 网线 直连, 
   2. ip地址 在同一网段内
2. 子网掩码: 
   1.  确定 IP中的 网络号和主机IP号,  
      1. 0 主机号
      2. 不是0的是网络号
   2. 按位与操作
3. Notepad++ , sublime编程工具

##### 5.5-集线器、交换器组网、arp获取mac地址等

1. 网络传输的是 电压信号, 不是电流
2. hub, 集线器, 淘汰了
   1. 广播发送
   2. 信号容易冲突
3. 交换机 
   1. 通信过程:
      1. 广播: 得到 mac地址
         1. `arp -a` 广播
            1. `arp -a` 或者 `arp -e` #列出所有arp
         2. 所有网卡通用地址 FF:FF:FF:FF:FF:FF 通用mac地址
      2. 单播
         1. 向目标电脑单播response的
   2. 实际地址: mac地址(网卡地址)
   3. ARP协议:  
      1. `arp -a`   #列出所有arp
      2. arp攻击, 即 中间人攻击
   4. 处于同一个网络

##### 5.6-路由器 链接多个网络、默认网关

1. 作用: 连接 不同的网络

   1. 网关 是 路由器
      1.  转发数据 功能设备
      2. 一般指 路由器

2. 路由器 有2个网卡

   1. 网卡1: 连接网络1

      1. 请求头

         目标主机IP : 192.168.2.1

         目标主机mac: 路由器左侧的mac地址

         源IP : 192. 168. 1. 1

         源mac: 192.168.1.1电脑的mac地址

   2. 网卡2 连接网络2

      1. 请求头

         目标主机 IP : 192.168.2.1

         目标主机 mac: 192.168.2.1电脑的mac地址

         源IP : 192. 168. 1. 1

         源mac: 路由器右侧的mac地址

   3. IP不变, mac变

##### 5.7-浏览器 访问服务器的过程

1. 购买域名` www.ttoo.com`

   1. 买域名
      1. 阿里云 > 产品 > 域名与网站 > 域名注册
   2. 买服务器
      1. 阿里云 > 产品 > 弹性计算 > 云服务器 ECS >

2. DNS服务器

   1. 类似电话本,  域名 对应 IP
   2. 公众都知道的电脑

   ​       IP 有区域范围

3. 通信过程:
   1.  浏览器 发送`arp` 获取 路由器mac
   2.  浏览器 发生域名解析请求, 通过路由 网关 互联网发给 DNS服务器, DNS解析获得IP后发给浏览器
   3. 浏览器 根据IP, 通过路由 网关 互联网 找到 http服务器, 浏览器向服务器发送tcp 3次握手, 建立连接
   4. 浏览器发送 http请求, 服务器应答请求
   5. 浏览器向http服务器发送tcp的4次挥手
   
4. ARP协议

   1. ARP协议 是“Address Resolution Protocol”（地址解析协议） 缩写。 作用:  网络中，数据传输依靠`MAC`地址而非IP地址, ARP协议 将IP地址转换为MAC地址
   2. 静态映射
      1. 手动创建一张`ARP表`， 逻辑（IP）地址和物理地址关联
         1. ARP表储存在网络中的每一台机器上。
      2. 知道机器的 IP地址 但不知道其 物理地址,  通过`ARP表`找物理地址。
   3. 动态映射
      1. 机器A只要知道机器B的逻辑（IP）地址， 根据`协议`找出对应的物理地址。
      2. 动态映射协议有`ARP` `RARP`两种。
         1. ARP把逻辑（IP）地址映射为物理地址。
         2. RARP把物理地址映射为逻辑（IP）地址

5. ARP原理及流程

   主机A的 IP数据报文发送给主机B，它要知道接B的逻辑（IP）地址。IP地址必须封装成帧才能通过物理网络。这意味着发送方A必须有接收方B的物理（MAC）地址，因此要完成 逻辑地址 到 物理地址 的映射。ARP协议 接收来自 IP协议 的 逻辑地址，将其映射为相应的 物理地址，然后把物理地址递交给 数据链路层.

   1. **ARP请求**
1. 主机要找出网络中的另一个主机的物理地址，发送一个`ARP`请求报文
      2. 报文包含了发送方的`MAC`地址和`IP`地址以及接收方的`IP`地址。
3. 发送方不知道接收方的物理地址，所以查询会在网络层中广播。（见图1）
   2. ARP响应
   1. 局域网中的每一台主机接受并处理`ARP`请求报文，然后验证. 
      2. 查看接收方的IP地址是不是自己的地址， 验证成功的主机返回一个ARP响应报文
      1. 响应报文包含接收方的`IP`逻辑地址和`mac`物理地址。
      3. ARP响应报文用收到的ARP请求报文中的请求方物理地址以`单播`的方式直接发送给ARP请求报文的请求方
3. ARP协议报文字段抓包解析
   

   
   原文链接：https://blog.csdn.net/ever_peng/article/details/80008638

##### 5.8-ip不变、mac地址发生变化

1. 原理: 
   1.  网络中，数据传输依靠`MAC`地址而非IP地址



#### python 就业方向: 

1. 网站后台, django, flask

2. 爬虫

   1, 2 都用网络

3. 人工智能

