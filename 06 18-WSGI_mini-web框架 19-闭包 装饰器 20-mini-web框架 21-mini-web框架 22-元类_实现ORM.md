#### 总结

1. 当 try 块没有出现异常时，程序 执行 else 块
2. 琢磨, 努力, 提炼, 怎么实现量变到质变.
3. 装饰器第一层参数是函数体引用, 第二层参数是普通变量.

### 18 WSGI, mini-web框架

###### 文档版本

1. web服务器和web框架的作用及其关系

   1. web服务器作用：

      1. 解析请求报文, 调用框架程序处理请求。
      2. 组织响应报文, 返回内容给客户端。

   2. web框架程序的作用

      1. 路由分发(根据url找到对应的处理函数) 。
      2. 处理函数处理业务。

   3. web服务器维持服务器和客户端的连接

      1. web服务器接收到请求，将请求及相关的参数传递给web框架

      2. 框架负责生成内容，并将生成的内容传递给web服务器

      3. web服务器的职责: 接受并返回请求，web框架的职责: 内容生成

      4. ```mermaid
         flowchart LR
         A[客户端]
         subgraph 服务器
         
         	B[web服务器]
         	C[web框架程序]
         end
         
         A --> B
         B -->|wsgi|C
         C --> |wsgi| B
         ```

         1. 服务器的组件包括 web服务器 和 web框架程序

2. wsgi: Web Server Gateway Interface, 或 Python Web Server Gateway Interface.

   1. Python 语言定义的 Web 服务器和 Web 应用程序或框架之间的一种简单而通用的接口
   2. WSGI 被开发出来以后，许多其它语言中也出现了类似接口。
   3. 很多框架都自带了 WSGI server . 如 Flask，webpy，Django、CherryPy等等。性能不好，自带的 web server多是测试用途，发布时则用生产环境的 WSGI server或联合 nginx 做 uwsgi

3. Web服务器、WSGI、Web框架、Web应用程序 的联系的区别

   1. 名词解释

      1. web服务器：接收HTTP请求并返回数据
      2. WSGI：是一种通信协议, 只适用于 [Python](https://so.csdn.net/so/search?from=pc_blog_highlight&q=Python) 语言，其全称为 Web Server Gateway Interface
      3. Web框架：方便开发Web应用程序
      4. Web应用程序：对接收的数据处理

   2. 类比

      1. HTTP请求：大臣上报皇帝的事情
      2. Web服务器：门卫
      3. WSGI：太监宫女
      4. Web框架：辅助皇帝决策的人（内阁）
      5. Web应用程序：皇帝

   3. 关系

      1. ```mermaid
         flowchart TB
         A1[Web框架<br>内阁]
         B1[WSGI<br>太监宫女]
         C1[Web服务器<br>门卫]
         D1[client<br>大臣上报]
         
         A2[Web框架<br>内阁]
         B2[WSGI<br>太监宫女]
         C2[Web服务器<br>门卫]
         D2[client<br>大臣上报]
         subgraph 响应 
         	A1 --> B1 --> C1 --> D1
         end
         subgraph 请求
         D2 --> C2 --> B2 --> A2
         end
         ```

      

   4. 常见的Web服务器

      1. Apache、Nginx、Tomcat、IIS

   5. 常见的开发框架

      1. Python：Django、Flask
      2. PHP：CakePHP、Laravel
      3. Node.js ：Express
      4. Ruby：Rails、Sinatra
      5. java : spring, mybatis,

   6. Nginx，WSGI，Flask 之间的对话

      Nginx：Hey，WSGI，我刚收到了一个请求，我需要你作些准备，然后由Flask来处理这个请求。

      WSGI：OK，Nginx。我会设置好环境变量，然后将这个请求传递给Flask处理。

      Flask：Thanks WSGI！给我一些时间，我将会把请求的响应返回给你。

      WSGI：Alright，那我等你。

      Flask：Okay，我完成了，这里是请求的响应结果，请求把结果传递给Nginx。 

      WSGI：Good job！Nginx，这里是响应结果，已经按照要求给你传递回来了。

      Nginx：Cool，我收到了，我把响应结果返回给客户端。大家合作愉快~

#### 18.1 服务器动态资源请求

1. 浏览器请求动态页面 过程

   1. 浏览器  -->  web服务器
      1.  http请求动态资源  -->
   2. web服务器 <--> 应用程序框架
      2.  通过WSGI调一个属性  --> 
      3. <--  通过引用, 调用 <u>web服务器的方法</u>，设置返回的状态和头信息
      4.  调用返回，此时web服务器端保存了刚刚设置的信息. -->
      5. 应用程序框架:  查询数据厍等，生成动态页面的body信息
      6. <--  生成的body信息返回给web服务器的调用
   3. 浏览器  <--  web服务器
      7. <--  web服务器把数据返回给浏览器
   4. 总结: 浏览器  web服务器  应用程序框架 数据库 四者之间的关系

2. WSGI

   1. WSGI把web框架和web服务器分开。
      1. 匹配web服务器和web框架，选 适合的配对
      2. web服务器: waitress, Nginx/uWSGI, Gunicorn
      3. web框架: pyramid, Flask, Django

3. WSGI接口

   1. 特点

      1. web服务器 有`WSGI接口`
      2. Python Web框架有`WSGI接口`，
      3. 使 web服务器和 web框架 协同工作

   2. 定义WSGI接口

      1. WSGI接口 非常简单

         1. 实现 `application`函数， 处理`HTTP请求`。

      2. `application()`函数, 符合WSGI标准的 处理HTTP的 函数

         ```
         def application(environ, start_response):
             start_response('200 OK', [('Content-Type', 'text/html')])
             return 'Hello World!'
         ```

         1. 接收两个参数
            1. `environ`,  HTTP请求信息的 `dict对象`
            2. `start_response`,  发送HTTP响应的 `函数`
         2. `application()`函数
            1. 分离 web服务器`解析部分` 应用程序`逻辑部分` 
            2. 没有涉及 解析HTTP的部分
         3. `application()`函数由 `WSGI服务器` 调用
            1. 有很多符合WSGI规范的服务器

4. web服务器-----WSGI协议---->web框架 传递的字典

   ```
   {
       'HTTP_ACCEPT_LANGUAGE': 'zh-cn',
       'wsgi.file_wrapper': <built-infunctionuwsgi_sendfile>,
       'HTTP_UPGRADE_INSECURE_REQUESTS': '1',
       'uwsgi.version': b'2.0.15',
       'REMOTE_ADDR': '172.16.7.1',
       'wsgi.errors': <_io.TextIOWrappername=2mode='w'encoding='UTF-8'>,
       'wsgi.version': (1,0),
       'REMOTE_PORT': '40432',
       'REQUEST_URI': '/',
       'SERVER_PORT': '8000',
       'wsgi.multithread': False,
       'HTTP_ACCEPT': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
       'HTTP_HOST': '172.16.7.152: 8000',
       'wsgi.run_once': False,
       'wsgi.input': <uwsgi._Inputobjectat0x7f7faecdc9c0>,
       'SERVER_PROTOCOL': 'HTTP/1.1',
       'REQUEST_METHOD': 'GET',
       'HTTP_ACCEPT_ENCODING': 'gzip,deflate',
       'HTTP_CONNECTION': 'keep-alive',
       'uwsgi.node': b'ubuntu',
       'HTTP_DNT': '1',
       'UWSGI_ROUTER': 'http',
       'SCRIPT_NAME': '',
       'wsgi.multiprocess': False,
       'QUERY_STRING': '',
       'PATH_INFO': '/index.html',
       'wsgi.url_scheme': 'http',
       'HTTP_USER_AGENT': 'Mozilla/5.0(Macintosh;IntelMacOSX10_12_5)AppleWebKit/603.2.4(KHTML,likeGecko)Version/10.1.1Safari/603.2.4',
       'SERVER_NAME': 'ubuntu'
   }
   ```
   
5. web各组件关系

   1. web各组件关系: 浏览器  web服务器  应用程序框架 数据库 四者之间的关系

#### 18.2 应用程序示例

application(environment, start_response) 函数, 即web应用程序

```
import time

def application(environ, start_response):
    status = '200 OK'
    response_headers = [('Content-Type', 'text/html')]
    start_response(status, response_headers)
    return str(environ) + '==Hello world from a simple WSGI application!--->%s\n' % time.ctime()
```



#### 18.3 Web动态服务器-基本实现

1. 文件结构

   ```
   `一个普通的文件夹, 如 webframe 文件夹`
   webframe #普通文件夹
   ├── web_server.py  #web server,内容是类 名是WSGI, 相当于web服务器或http服务器
   ├── web  #普通文件夹        
   │   └── my_web.py   #内容application()函数, 是web框架,内部有应用程序
   └── html #普通文件夹
       └── index.html  #模板
       .....
   ```

2. web_frame/web/my_web.py  #web框架 相当于如 flask, Django等, application相当于web框架的应用程序

   ```
   import time
   
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
       return str(environ) + '==Hello world from a simple WSGI application!--->%s\n' % time.ctime()
   ```

3. web_frame/web_server.py  #服务器上的web服务器(即http服务器)

   ```
   import select
   import time
   import socket
   import sys
   import re
   import multiprocessing
   
   
   class WSGIServer(object):
       """web服务器或http服务器: 一个web服务器的类"""
       
   	#参数:
   	#port, type(port) int, 值: 7890
   	#documents_root, 类型: str, 值:'./html'
   	#app, 类型: funtion, 值 <function application at 0x0000000023BFDC0)
       def __init__(self, port, documents_root, app):
   
           # 1. 创建套接字
           self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           # 2. 绑定本地信息
           self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           self.server_socket.bind(("", port))
           # 3. 变为监听套接字
           self.server_socket.listen(128)
   
           # 设定资源文件的路径
           self.documents_root = documents_root
           #类型<class 'str'>, 值: './html'
   
           # 设定web框架中调用的WSGI接口函数(对象)
           #self.app = <function application at 0x0000000023BFDC0)
           self.app = app
           #类型 <class 'function'>, 值: <function application at 0x00000023BFDC0>, 对应一个内存空间
           
   
       def run_forever(self):
           """运行服务器"""
   
           # 等待对方链接
           while True:
               new_socket, new_addr = self.server_socket.accept()
               # 创建一个新的进程来完成这个客户端的请求任务
               new_socket.settimeout(3)  # 3s
               new_process = multiprocessing.Process(target=self.deal_with_request, args=(new_socket,))
               new_process.start()
               new_socket.close()
   
       def deal_with_request(self, client_socket):
           """以长链接的方式，为这个浏览器服务"""
   
           while True:
               try:
                   request = client_socket.recv(1024).decode("utf-8")
               except Exception as ret:
                   print("========>", ret)
                   client_socket.close()
                   return
   
               # 判断浏览器是否关闭
               if not request:
                   client_socket.close()
                   return
   
               request_lines = request.splitlines()
               for i, line in enumerate(request_lines):
                   print(i, line)
   
               #提取请求的文件(index.html)
               # GET /a/b/c/d/e/index.html HTTP/1.1
               ret = re.match(r"([^/]*)([^ ]+)", request_lines[0])
               if ret:
                   print("正则提取数据:", ret.group(1))
                   print("正则提取数据:", ret.group(2))
                   file_name = ret.group(2)
                   if file_name == "/":
                       file_name = "/index.html"
   
               # 如果不是以py结尾的文件，认为是普通的文件
               if not file_name.endswith(".py"):
   
                   # 读取文件数据
                   try:
                       f = open(self.documents_root+file_name, "rb")
                   except:
                       response_body = "file not found, 请输入正确的url"
   
                       response_header = "HTTP/1.1 404 not found\r\n"
                       response_header += "Content-Type: text/html; charset=utf-8\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       response = response_header + response_body
   
                       # 将header返回给浏览器
                       client_socket.send(response.encode('utf-8'))
   
                   else:
                       content = f.read()
                       f.close()
   
                       response_body = content
   
                       response_header = "HTTP/1.1 200 OK\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       # 将header返回给浏览器
                       client_socket.send(response_header.encode('utf-8') + response_body)
   
               # 以.py结尾的文件，就认为是浏览需要动态的页面
               else:
                   # 准备一个字典，存放要传递给web框架的数据
                   env = {}
                   #调用my_web框架中的application应用程序,返回值存于response_body中
                   response_body = self.app(env, self.set_response_headers)
                   
                   print("response_body", response_body)
                   """
                   response_body {}==Hello world from a simple WSGI application!--->Tue Jun 22 11:50:35 2021   #页面刷新, 时间变化
                   """
                   
   
                   # 合并header和body
                   response_header = "HTTP/1.1 {status}\r\n".format(status=self.headers[0])
                   response_header += "Content-Type: text/html; charset=utf-8\r\n"
                   response_header += "Content-Length: %d\r\n" % len(response_body)
                   for temp_head in self.headers[1]:
                       response_header += "{0}:{1}\r\n".format(*temp_head)
   
                   response = response_header + "\r\n"
                   response += response_body
   
                   client_socket.send(response.encode('utf-8'))
   
       def set_response_headers(self, status, headers):
           """这个方法，会在 web框架中被默认调用"""
           response_header_default = [
               ("Data", time.ctime()),
               ("Server", "ItCast-python mini web server")
           ]
   
           # 将状态码/相应头信息存储起来
           # [字符串, [xxxxx, xxx2]]
           self.headers = [status, response_header_default + headers]
   
   
   #设置静态资源访问的相对路径
   g_static_document_root = "./html"
   #设置动态资源访问的相对路径. 路径要添加到sys.path中, 否则无法使用到
   g_dynamic_document_root = "./web"
   
   def main():
       """控制web服务器整体"""
       # python xxxx.py 7890 my_web:application
       if len(sys.argv) == 3:
           # 获取web服务器的port
           for i in range(0,len(sys.argv)):
               print(i, sys.argv[i])
           """
           sys.argv[0]: 'my_web.py'  #字符串类型
           sys.argv[1]: '7890'  #字符串类型
           sys.argv[2]: 'my_web:application'  #字符串类型
           """
           port = sys.argv[1]   #'7890' 转换为 7890, 字符串类型转整数类型
           if port.isdigit():
               port = int(port)
           # 获取web服务器需要动态资源时，访问的 web框架名字
           web_frame_module_app_name = sys.argv[2]  #'my_web:application'
       else:
           print("运行方式如: python3 xxx.py 7890 my_web_frame_name:application")
           return
   
       print("http服务器使用的port:%s" % port)
   
       #将动态路径(存放py文件的路径)添加到path中，这样python就能够找到这个路径了
       sys.path.append(g_dynamic_document_root)  #动态资源访问的相对路径'./web'
   
       print("sys.path", sys.path)
       """
       #在sys.path列表中添加'./web'字符串, 每个元素都是一个路径字符串
       sys.path ['F:\\anconda project\\webframe', 'D:\\Python3.8.3\\python38.zip', 'D:\\Python3.8.3\\DLLs', 'D:\\Python3.8.3\\lib', 'D:\\Python3.8.3', 'D:\\Python3.8.3\\lib\\site-packages', './web']
       """
   
       ret = re.match(r"([^:]*):(.*)", web_frame_module_app_name)  #'my_web:application'
       if ret:
           # 获取模块名
           web_frame_module_name = ret.group(1)   #'my_web'
           #返回值类<class 'str'>, 返回值: 'my_web'
           
           # 获取可以调用web框架的应用名称
           app_name = ret.group(2)   #'application'
           #type(app_name), #返回值类型 <class 'str'>, 返回值: 'application'
   
       # 导入web框架的主模块. 从sys.path中指定的路径中import模块,
       #参数: web_frame_module_name的值是'my_web', 字符串类型
       web_frame_module = __import__(web_frame_module_name)  
       #返回值类型: <class 'module'>, 返回值 <module 'my_web' from './web\\my_web.py'>, 即导入模块my_web.py    
       #type(web_frame_module), #返回值 <class 'module'>
       
       
       #获取那个可以直接调用的函数(对象)
       app = getattr(web_frame_module, app_name)  #参数:(my_web.py模块,'application'字符串)
       #type(app), <class 'function'>, 
       #返回值, app: <function application at 0x00000023BFDC0>
   
       print("app", app)  #app <function application at 0x0000000002419D30>
       # 启动http服务器
       http_server = WSGIServer(port, g_static_document_root, app)  #参数 7890, "./html", <function application at 0x0000000002419D30>
       # 运行http服务器
       http_server.run_forever()
   
   
   if __name__ == "__main__":
       main()
   ```

4. 运行

   1. window中,开始>运行>打开终端>进入webframe路径，输入命令开启服务器 #最好输入, 复制粘贴会出错

      `python3 web_server.py 7890 my_web:application`   //python3解释器

      或
      
      `python web_server.py 7890 my_web:application`  //python解释器
      
      1. 7890 是端口
      
   2. my_web.py 在`./web` 文件夹中
   
2. 打开浏览器
   
   1. 浏览器输入: 127.0.0.1:7890/xxx.py   #以`.py` 结尾的url
   
   2. 页面显示:
   
         `{}==-Hello world from a simple WSGI appli tion!-->Wed Nov 1 14:51:09 2017`    #刷新页面, 时间变化
   
      

#### 18.4 mini web框架-1-文件结构

1. 文件结构

   ```
   webframe   #项目文件夹名, 普通文件夹类型, 不是python包
   ├── dynamic ---存放py模块, 普通文件夹
   │   └── my_web.py  #框架
   ├── templates ---存放模板文件, 普通文件夹
   │   ├── center.html
   │   ├── index.html
   │   ├── location.html
   │   └── update.html
   ├── static ---存放静态的资源文件, 普通文件夹
   │   ├── css   , 普通文件夹
   │   │   ├── bootstrap.min.css
   │   │   ├── main.css
   │   │   └── swiper.min.css
   │   └── js, 普通文件夹
   │       ├── a.js
   │       ├── bootstrap.min.js
   │       ├── jquery-1.12.4.js
   │       ├── jquery-1.12.4.min.js
   │       ├── jquery.animate-colors.js
   │       ├── jquery.animate-colors-min.js
   │       ├── jquery.cookie.js
   │       ├── jquery-ui.min.js
   │       ├── server.js
   │       ├── swiper.jquery.min.js
   │       ├── swiper.min.js
   │       └── zepto.min.js
   └── web_server.py ---mini web服务器(http服务器)
   ```

2. my_web.py   #相当于应用程序框架, 如 Django

   ```
   #模块 my_web.py
   #框架,内定义函数 
   import time
   
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       
       #web_server.py模块中WSGIServer类中的方法
       start_response(status, response_headers)
       
       #框架中的应用程序返回的内容, 
       return str(environ) + '==Hello world from a simple WSGI application!--->%s\n' % time.ctime()
   ```

3. web_server.py  #相当于web服务器或http服务器

   ```
   #web_server.py 相当于web服务器
   import select
   import time
   import socket
   import sys
   import re
   import multiprocessing
   
   
   class WSGIServer(object):
       """定义一个WSGI服务器的类"""
   
       def __init__(self, port, documents_root, app):
   
           # 1. 创建套接字
           self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           # 2. 绑定本地信息
           self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           self.server_socket.bind(("", port))
           # 3. 变为监听套接字
           self.server_socket.listen(128)
   
           # 设定资源文件的路径
           self.documents_root = documents_root
   
           #引用web框架的函数(对象)
           self.app = app
   
       def run_forever(self):
           """运行服务器"""
   
           # 等待对方链接
           while True:
           	#接收客户端请求, 生成客户端服务套接字
               new_socket, new_addr = self.server_socket.accept()
               
               new_socket.settimeout(3)  # 3s
               
               # 创建一个新的进程来完成这个客户端的请求任务
               new_process = multiprocessing.Process(target=self.deal_with_request, args=(new_socket,))
               #启动继承
               new_process.start()
               new_socket.close()
   
       def deal_with_request(self, client_socket):
           """以长链接的方式，为这个浏览器服务"""
   
           while True:
               try:
                   request = client_socket.recv(1024).decode("utf-8")
               except Exception as ret:
                   print("========>", ret)
                   client_socket.close()
                   return
   
               # 判断浏览器是否关闭
               if not request:
                   client_socket.close()
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
   
               # 如果不是以py结尾的文件，认为是普通的文件
               if not file_name.endswith(".py"):
   
                   # 读取文件数据
                   try:
                       f = open(self.documents_root+file_name, "rb")
                   except:
                       response_body = "file not found, 请输入正确的url"
   
                       response_header = "HTTP/1.1 404 not found\r\n"
                       response_header += "Content-Type: text/html; charset=utf-8\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       response = response_header + response_body
   
                       # 将header返回给浏览器
                       client_socket.send(response.encode('utf-8'))
   
                   else:
                       content = f.read()
                       f.close()
   
                       response_body = content
   
                       response_header = "HTTP/1.1 200 OK\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       # 将header返回给浏览器
                       client_socket.send(response_header.encode('utf-8') + response_body)
   
               # 以.py结尾的文件，就认为是浏览需要动态的页面
               else:
                   # 准备一个字典，存放需要传递给web框架(my_web.py)的数据
                   env = dict()
                   
                   #调用my_web.py模块的函数application(), 此时application函数体在当前模块中可见
                   #self.app()是my_web.py模块中的函数. 调用之, 返回值存入response_body
                   #参数:env 是 dict类型, self.set_response_headers 是函数体引用
                   response_body = self.app(env, self.set_response_headers)
                   #type(http_server.set_response_headers), 返回值: <class 'method'>
   
                   # 合并header和body
                   response_header = "HTTP/1.1 {status}\r\n".format(status=self.headers[0])
                   response_header += "Content-Type: text/html; charset=utf-8\r\n"
                   response_header += "Content-Length: %d\r\n" % len(response_body)
                   for temp_head in self.headers[1]:
                       response_header += "{0}:{1}\r\n".format(*temp_head)
   
                   response = response_header + "\r\n"
                   response += response_body
   
                   client_socket.send(response.encode('utf-8'))
   
   
       def set_response_headers(self, status, headers):
           """这个方法， 在 web框架中被调用"""
           response_header_default = [
               ("Data", time.time()),
               ("Server", "ItCast-python mini web server")
           ]
   
           # 将状态码/相应头信息存储起来
           # [字符串, [xxxxx, xxx2]]
           self.headers = [status, response_header_default + headers]
   
   
   # 设置静态资源访问的路径
   g_static_document_root = "./static"
   # 设置动态资源访问的路径
   g_dynamic_document_root = "./dynamic"
   
   def main():
       """控制web服务器整体"""
       
       #sys.argv: 类型: <class 'list'>, 值: ['web_server.py','7890','my_web:application']
       #三个元素都是字符串类型
       
       # python3 xxxx.py 7890
       if len(sys.argv) == 3:
           # 获取web服务器的port
           port = sys.argv[1]
           if port.isdigit():
               port = int(port)
           # 获取web服务器需要动态资源时，访问的web框架名字
           web_frame_module_app_name = sys.argv[2]
       else:
           print("运行方式如: python3 xxx.py 7890 my_web_frame_name:application")
           return
   
       print("http服务器使用的port:%s" % port)
   
       # 将动态路径即存放py文件的路径，添加到path中，这样python就能够找到这个路径了
       sys.path.append(g_dynamic_document_root)
   
       ret = re.match(r"([^:]*):(.*)", web_frame_module_app_name)
       if ret:
           # 获取模块名
           web_frame_module_name = ret.group(1)
           #类型: str, 值: 'my_web'
           
           
           # 获取可以调用web框架的应用名称
           app_name = ret.group(2)
           #类型: str, 值: 'application'

       # 导入web框架的主模块
       web_frame_module = __import__(web_frame_module_name)
       #返回值类型: <class 'module'> , 值: <module 'my_web' from './dynamic\\my_web.py'>
       
       # 获取那个可以直接调用的函数(对象)
       #参数: web_frame_moduel是web_server.py模块, app_name是'application'字符串
       app = getattr(web_frame_module, app_name) 
       #type(app) : <class 'function'>
       #app : <function application at 0x000000023CBEE0> 函数体
   
       # print(app)  # for test
   
       #创建http服务器实例
       #参数: port: int类型 7890, g_static_documetn_root: str './static', app: 函数体类型,application函数
       http_server = WSGIServer(port, g_static_document_root, app)
       # 运行http服务器
       http_server.run_forever()
   
   
   if __name__ == "__main__":
       main()
   ```
   
   1. window的开始 > cmd, 打开命令窗口, 输入: 
   
      python web_server.py 7890 my_web:application
   
   2. my_web.py在 `./dynamic` 中
   
4. web_server.py程序运行后, my_web.py中application()函数修改后保存, 然后刷新浏览器, 浏览器请求的信息才显示application()修改后的内容.

   

#### 18.5 mini web框架-2-显示页面

1. ./static/index.html

   ```
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>index only</title>
   </head>
   <body>
   index
   "1content1"
   index
   </body>
   11111
   </html>
   ```
   
2. ./dynamic/my_web.py (更新)

   ```
   import time
   import os
   
   template_root = "./templates"
   
   
   def index(file_name):
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
           return content
   
   
   def center(file_name):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
           return content
   
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       if file_name == "/index.py":
           return index(file_name)
       elif file_name == "/center.py":
           return center(file_name)
       else:
           return str(environ) + '==Hello world from a simple WSGI application!--->%s\n' % time.ctime()
   ```

3. web_server.py (更新)

   ```
   import select
   import time
   import socket
   import sys
   import re
   import multiprocessing
   
   
   class WSGIServer(object):
       """定义一个WSGI服务器的类"""
   
       def __init__(self, port, documents_root, app):
   
           # 1. 创建套接字
           self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           # 2. 绑定本地信息
           self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           self.server_socket.bind(("", port))
           # 3. 变为监听套接字
           self.server_socket.listen(128)
   
           #请求静态资源, 静态资源文件的路径
           self.documents_root = documents_root
   
           #请求动态资源时, 设定web框架可以调用的函数(对象)
           self.app = app
   
       def run_forever(self):
           """运行服务器: 创建客服端服务套接字, 接收客户端请求"""
   
           # 等待对方链接
           while True:
               new_socket, new_addr = self.server_socket.accept()
               # 创建一个新的进程来完成这个客户端的请求任务
               new_socket.settimeout(3)  # 3s
               new_process = multiprocessing.Process(target=self.deal_with_request, args=(new_socket,))
               new_process.start()
               new_socket.close()
   
       def deal_with_request(self, client_socket):
           """处理客户端请求. 以长链接的方式，为浏览器服务"""
   
           while True:
               try:
                   request = client_socket.recv(1024).decode("utf-8")
               except Exception as ret:
                   print("========>", ret)
                   client_socket.close()
                   return
   
               #判断浏览器是否关闭
               if not request:
               	#客户端服务套接字关闭
                   client_socket.close()
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
   
               #不是以py结尾的文件，认为是静态的文件
               if not file_name.endswith(".py"):
   
                   # 读取文件数据
                   try:
                       print(self.documents_root+file_name)
                       f = open(self.documents_root+file_name, "rb")
                   except:
                       response_body = "file not found, 请输入正确的url"
   
                       response_header = "HTTP/1.1 404 not found\r\n"
                       response_header += "Content-Type: text/html; charset=utf-8\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       response = response_header + response_body
   
                       # 将header返回给浏览器
                       client_socket.send(response.encode('utf-8'))
   
                   else:
                       content = f.read()
                       f.close()
   
                       response_body = content
   
                       response_header = "HTTP/1.1 200 OK\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       # 将header返回给浏览器
                       client_socket.send(response_header.encode('utf-8') + response_body)
   
               # 以.py结尾的文件，认为动态文件,需要web框架处理
               else:
                   # 准备一个字典，里面存放需要传递给web框架的数据
                   env = dict()
                   # ----------更新---------
                   env['PATH_INFO'] = file_name  # 例如 index.py
   
                   #调用my_web.py模块(模拟框架)的方法处理请求,返回值保存到response_body中
                   response_body = self.app(env, self.set_response_headers)
   
                   # 合并header和body
                   response_header = "HTTP/1.1 {status}\r\n".format(status=self.headers[0])
                   response_header += "Content-Type: text/html; charset=utf-8\r\n"
                   response_header += "Content-Length: %d\r\n" % len(response_body.encode("utf-8"))
                   for temp_head in self.headers[1]:
                       response_header += "{0}:{1}\r\n".format(*temp_head)
   
                   response = response_header + "\r\n"
                   response += response_body
   
                   client_socket.send(response.encode('utf-8'))
   
   
       def set_response_headers(self, status, headers):
           """这个方法，会在 web框架中被默认调用"""
           response_header_default = [
               ("Data", time.time()),
               ("Server", "ItCast-python mini web server")
           ]
   
           # 将状态码/相应头信息存储起来
           # [字符串, [xxxxx, xxx2]]
           self.headers = [status, response_header_default + headers]
   
   
   # 设置静态资源访问的路径
   g_static_document_root = "./static"
   # 设置动态资源访问的路径
   g_dynamic_document_root = "./dynamic"
   
   def main():
       """控制web服务器整体"""
       # python3 xxxx.py 7890
       #sys.argv = ['web_server.py', '7890', 'my_web:application']
       if len(sys.argv) == 3:
           # 获取web服务器的port
           port = sys.argv[1]
           if port.isdigit():
               port = int(port)
           # 获取web服务器需要动态资源时，访问的web框架名字
           web_frame_module_app_name = sys.argv[2]
       else:
           print("运行方式如: python3 xxx.py 7890 my_web_frame_name:app")
           return
   
       print("http服务器使用的port:%s" % port)
   
       # 将动态路径即存放py文件的路径，添加到path中，这样python就能够找到这个路径了
       sys.path.append(g_dynamic_document_root)
   
       ret = re.match(r"([^:]*):(.*)", web_frame_module_app_name)
       if ret:
           # 获取模块名
           web_frame_module_name = ret.group(1)   #返回值 'my_web'
           # 获取可以调用web框架的应用名称
           app_name = ret.group(2)  #返回值: 'application'
   
       # 导入web框架的主模块
       web_frame_module = __import__(web_frame_module_name)
       # 获取那个可以直接调用的函数(对象)
       app = getattr(web_frame_module, app_name)   #返回值: 一个函数
   
       # print(app)  # for test
   
       # 启动http服务器
       #port端口, g_static_document_root静态资源 对应静态请求, app处理动态请求
       http_server = WSGIServer(port, g_static_document_root, app)
       # 运行http服务器
       http_server.run_forever()
   
   
   if __name__ == "__main__":
       main()
   ```

4. 运行程序

   1. 方法1: window > 开始 > cmd , cd 进入webframe文件夹,  输入

      `python web_server.py 7890 my_web:application`

      1. cmd做服务器

      

   2. 方法2: pycharm > python console中一步一步的输入

      1. python console做服务器

5. 浏览器打开看效果

   1. 浏览器中输入 ` http://127.0.0.1:7890/center.py`   # 显示 ./templates/center.html网页内容

   2. 文件结构

      ```
      webframe   #项目文件夹名 #普通文件夹类型, 不是python包
      ├── dynamic ---存放py模块
      │   └── my_web.py
      ├── templates ---存放模板文件
      │   ├── center.html
      │   ├── index.html
      │   ├── location.html
      │   └── update.html
      └── web_server.py ---mini web服务器
      ```

6. 浏览器端

   1. 右键 > 检查 > Elements, 显示的是渲染过的代码

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
             <meta charset="UTF-8">
             <title>index only</title>
         </head>
         <body>
         index
         "1content1"
         index
         </body>
         11111
         </html>
         ```

   2. 右键 > 查看网页源代码, 没有渲染过的

#### 18.6 mini web框架-3-替换模板

1. `.\templates\index.html`

   ```
   <!DOCTYPE html>
   <html lang="en">
       <head>
           <meta charset="UTF-8">
           <title>index only</title>
       </head>
       <body>
           index
           {%content1%}   #{%content1%}被格式相同的内容代替
           {%content2%}   #{%content2%}被格式相同的内容代替
           {%content3%}   #{%content3%}被格式相同的内容代替
           index
       </body>
       11111
   </html>
   ```

2. dynamic/my_web.py  #相当于 web框架的应用程序

   ```
   #相当于 web框架的应用程序
   import time
   import os
   import re
   
   #模板路径
   template_root = "./templates"
   
   
   def index(file_name):
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # --------更新-------
           data_from_mysql = "数据还没有敬请期待...."
           content = re.sub(r"{%content1%}", data_from_mysql, content)
           
           #和上句功能相同, 与html中 {%content2%} 发生代替
           content = re.sub(r"\{%content2%}", data_from_mysql, content)
           
           #和上上句功能相同, 与html中 {%content3%} 发生代替
           content = re.sub("\{%content3%\}", data_from_mysql, content)
           
   
           return content
   
   
   def center(file_name):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # --------更新-------
           data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           content = re.sub(r"\{%content%\}", data_from_mysql, content)
   
           return content
   
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       if file_name == "/index.py":
           return index(file_name)
       elif file_name == "/center.py":
           return center(file_name)
       else:
           return str(environ) + '==Hello world from a simple WSGI application!--->%s\n' % time.ctime()
   ```

   1. application函数中指定的文件`index.py`, `center.py` 才可以访问, 没有指定的文件 `index.py` `update.py` 不能访问,

3. 浏览器打开看效果

   1. 浏览器中输入

      ```
      http://127.0.0.1:7890/index.py
      http://127.0.0.1:7890/center.py
      ```

      有对应 网页 响应

   2. 浏览器中输入

      ```
      http://127.0.0.1:7890/location.py
      http://127.0.0.1:7890/update.py
      ```

      没有对应 网页 响应

   3. 原因:

      WSGI接口的`application`函数中指定的文件`index.py`, `center.py` 才可以访问, 没有指定的文件 `index.py` `update.py` 不能访问,

4. 文件结构

   ```
   webframe   #项目文件夹名 #普通文件夹类型, 不是python包
   ├── dynamic ---存放py模块
   │   └── my_web.py
   ├── templates ---存放模板文件
   │   ├── center.html
   │   ├── index.html
   │   ├── location.html
   │   └── update.html
   └── web_server.py ---mini web服务器
   ```

   

---------

------

### 01-WSGI-mini-web框架

###### 视频课版本

#### 01-课程介绍

1. Django

#### 02-多进程-面向对象-web服务器

1. linux 命令
   1.  文件中寻找字符的方法:
       1.  命令模式:输入 `/main`, 然后`enter` 回车 #找到main,  `n`下找, `N`上找
   2.  `gg` 文件头部
   3.  `cp  a/b -r`  # -r拷贝的是文件夹
   4.  `F5 ` ,  `ctrl + r` 刷新
2. 基础知识
   1. 文件复制后, 底层 fd + 1.    # `fd`文件描述符
      1. 文件描述符的本质是数组元素的下标
   2. '文件内容'.splitlines()  #按行('\r', '\r\n', \n')分隔，返回值: 包含各行作为元素的列表
3. web_server.py
   1.  bind 7890, 端口, 用来通信的
   2.  Web服务器
       1.   网关接口: Python Web Server Gateway Interface，缩写`WSGI`
       2.  Python语言定义的 Web服务器 和 Web应用程序/框架  间的简单而通用的 接口
4. 面向对象--类
   1. 类 
      1. `__new__`函数 创建对象
      2. 方法: 任务运行
      3. `__init__`  初始化对象

#### 03-静态资源、动态资源、web服务器支持动态解析

1. 静态响应 
   1. 浏览器 发送 http请求(如: index.html) 给服务器
   2. http服务器 处理浏览器请求, 解析出访问资源
   3. http服务器 从服务器硬盘 读取资源
   4. http服务器 向浏览器 发送资源
2. 动态响应
   1. 浏览器 发送http请求(index.html)给服务器
   2. http服务器把 http请求 转发给 `web框架`
   3. `web`框架 处理请求,  向http服务器返回处理`header`和`body`
   4. http服务器把`header`和`body` 封装为`http应答` 返回给 浏览器

#### 04-静态、动态资源强调

​	1.静态资源:  html, css, png等, 现成的, 存于服务器硬盘上

2. 动态资源:  `web`框架(如 Django) 处理http请求, 根据http请求随时生成header  body

#### 05-实现很简单的 web框架，让web服务器支持

1. 没有解耦
   
   web_server.py  #相当于http服务器(web服务器)
   
   ```
   #WSGIServer中 有http访问的`资源名`
   class WSGIServer(object):
   	....
           if ret:
               file_name = ret.group(1)
            # print("*"*50, file_name)
               if file_name == "/":
                   file_name = "/index.html"   # 获取请求的文件名 /文件名
   ```
   
   
   
2. 解耦 web服务器与 web应用程序(框架) 分开

   1. web_server.py   #web服务器

      ```
      #WSGIServer中 没有http访问的`资源名`
      class WSGIServer(object):
      	....
              else:
                  # 2.2 如果是以.py结尾，那么就认为是动态资源的请求
                  header = "HTTP/1.1 200 OK\r\n"
                  header += "\r\n"
      
                  # body = "hahahah %s " % time.ctime()
                  # if file_name == "/login.py":
                  #     body = mini_frame.login()
                  # elif file_name == "/register.py":
                  #     body = mini_frame.register()
                  body = mini_frame.application(file_name)   #调用mini_frame中的application函数, 实现web服务器与应用程序(框架)解耦
      
                  response = header+body
                  # 发送response给浏览器
                  new_socket.send(response.encode("utf-8"))
      ```

      1. 解耦了,  

   2. mini_frame.py   #web框架(mini框架/Django同等),  web应用程序的抽象, 它具体处理http请求

      ```
      import time
      
      
      def login():
          return "i----login--welcome hahahh to our website.......time:%s" % time.ctime()
      
      def register():
          return "-----register---welcome hahahh to our website.......time:%s" % time.ctime()
      
      def profile():
          return "-----profile----welcome hahahh to our website.......time:%s" % time.ctime()
      
      def application(file_name):
      	if file_name == "/login.py":
      		return login()
      	elif file_name == "/register.py":
      		return register()
      	else:
      		return "not found you page...."
      ```

      1. 判断条件: "/文件名.py", 带着/

#### 06-（重点）WSGI的介绍

1. WSGI

   1. Web服务器与Web应用程序框架 信息 交换的`协议`

2. WSGI流程

   1. 浏览器-->http请求动态资源-->web服务器
   2. web服务器-->通过wsgi调用一个属性-->web应用程序框架. 
      1. 服务器方法: 如 set_response_header
      2. web应用程序框架 : application(env, func_p)
         1. env 是字典

           2. func_p  是web服务器中的函数引用set_response_header
   3. web应用程序框架--通过引用**调用**web服务器的方法(set_response_header) ,设置返回的状态和头信息--web服务器,

      1. 通过 web应用程序框架的application 调用 web服务器的  set_response_header 设置 header
      2. 此时web服务器保存了刚刚设置的信息--web应用程序框架
   5. web应用程序框架--查询数据库等, 生成动态页面的body信息
   6. web应用程序框架--把生成的body信息返回-->web服务器的调用
   7. web服务器-->把header+body打包返回-->浏览器

#### 07-web服务器支持WSGI

1. WSGI流程梳理
   1. http请求动态资源
   2. web服务器-->通过一个属性, 传递`实参`给`形参`env, func_p(web应用程序框架的程序application)
   3. web应用程序框架的application() 使用  `set_response_header`(web服务器的方法) 传递过来 状态和头信息
   4. web应用程序框架 -->set_response_header调用**返回** -->web应用程序框架
   5. web应用程序框架--application() 执行其他任务
   6. web应用程序框架--application() 生成的body信息-->web服务器的调用

2. 函数调用关系
   1.  web服务器: 调用 `框架.方法` ,  如: mini.application(env, set_response_header)
      1. env参数传递信息,  set_response_header调用函数, 
   2.  web应用框架的程序中:  start_response-('200 ok', [('Content-Type', 'text/html')])--对应web服务器的方法 set_response_header
      1. 通过参数调用函数, 传递信息
   3. web服务器中:   set_response_header('200 ok', [('Content-Type', 'text/html')])
      1. 函数执行, 设置响应的头信息
   4. web应用框架中:  application(env, set_response_header)  执行 并 return body
      1. 函数执行, 生成响应的 body信息
   5. web服务器中:  接收 mini.application(env, set_response_header) 返回的body
      1. 接收返回值

#### 08-web服务器通过字典将参数传递给 mini_frame框架

1.  mini_frame.py中
   1. application(env, func_p)  #即WSGI接口
      2. env 字典,  浏览器传递过来的客户请求
      2. func_p ,  web服务器中的函数, 向 web 服务器返回 mini_frame 框架程序 的状态和header
         1. func_p的参数是 WGSIServer类的对象属性,  对象属性保存状态和header
2.  Sublime同时多个操作:  双击 按住 ctrl+r, 在删除, 同时删除多个, 同时输入多个

#### 09-mini_frame获取页面模板数据

1. 路径
   1. python3 x.py启动后触发的py模块中with打开的文件的路径, 以x.py为基准计算

#####    WGSI流程

2. WGSI流程:
   1. 浏览器 http请求 web服务器
   2. web服务器 转发给 web框架
   3. web框架打开模板, 查询数据
   4. 框架  调用服务器函数 返回 状态, header
   5. 框架 生成body, return body
   6. web服务器 组装 header+body
   7. 传给 浏览器

#### 10-给程序传递参数、添加web服务器的配置文件、添加shell功能

1. 导入

   1.  import  mini_frame  # 导入 mini_frame.py模块

   2.  `__import__(mini_frame)`   # mini_frame是字符串

   3. ```
      import sys
      sys.path.append('./')
      mini_f = __import__('mini_frame')   #参数类型是字符串
      b1 = getattr(mini_f, 'a1')  # 参数类型:(模块, 字符串)
      ```

      1. getattr的 参数类型:(模块, 字符串)
      2. 获取模块的属性
         1. 'a1' 是字符串, 属性或方法的名字的 字符串
   
2. ```
   命令行输入数据
   import sys
   ret = sys.argv
   ```

   1. python3命令后的字符串作为字符串传入
   2.  python3  test.py  123  345  # ret = ['test.py', '123', '345']

------

### 19 闭包,装饰器

###### 文档版本

#### 19.1 闭包

1. 函数引用

   ```
   def test1():
       print("--- in test1 func----")
   
   #调用函数
   test1()
   
   #引用函数
   ret = test1
   
   print(id(ret))   #61225280
   print(id(test1))  #61225280, 即同一个地址空间
   
   #用引用调用函数
   ret()
   ```

   1. 函数调用, 函数名后有`()`,  有`实参`  ,  foo()  #执行foo函数
   2. 函数引用, 只有函数名, 没有`()`,  没有`实参`,  foo # foo函数

2. 什么是闭包

   1. 函数被当成对象返回时，夹带了外部变量，就形成了一个闭包

      1. 记忆方法: 这个函数的情景或模板在头脑中的影像中

   2. 示例

      ```
      def test(number):
          #print(1)  
          #函数A内部再定义函数B，函数B用到函数A的变量，那么 函数B和用到的一些变量称为闭包
          def test_in(number_in):  #定义内层函数test_in
              print(2)
              print("in test_in 函数, number_in is %d" % number_in)
              return number+number_in   #number的值是第一次传入的值
          	#其实返回的是闭包的结果
          #print(3)
          return test_in  #返回 内层函数的名(即 返回内层函数的引用)
      ```

      ```
      # 给test函数赋值，这个20就是给参数number
      ret = test(20)  #test(20) 返回内层函数的引用, ret实际是test_in的引用.
      #赋值时输出
      1
      3
      ```

      ```
      # 注意这里的100其实给参数number_in
      ret(100)  #调用时输出, 实际是test_in(100)
      #输出
      2
      in test_in 函数, number_in is 100
      120
      ```

   3. 闭包:  函数A内部再定义一个函数B，内层函数B用到了外层函数A的变量，那么函数B以及用到变量 统称为 `闭包`

      1. 外层函数必须返回 内层函数的`引用`, #必须返回内层函数引用, 否则报错

3. 一个闭包的实际例子

   1. 示例

      ```
      def line_conf(a, b):
          def line(x):
              return a*x + b
          return line
      
      line1 = line_conf(1, 1)  #执行完, line1变成line函数
      line2 = line_conf(4, 5)
      print(line1(5))  #6 
      print(line2(5))  #25
      ```

      1. 函数line与变量a,b构成 闭包

   2. 闭包特点

      1. 提高代码可 复用性
      2. 闭包使用外层函数的局部变量，则外层函数的 局部变量 没有及时释放，消耗内存. 
         1. 外层函数的局部变量 的值 一直**<u>保存</u>**着,
         2. 下次调用时继续使用
         3. 除非再次调用外层函数
      3. 调用闭包的外层函数, 返回值是 `内层函数的引用`
         1. 若调用 返回的`引用`, 实际是 调用内层函数.
         2. 给 返回`引用`的传递实参, 实际是给 内层函数传递实参

4. 外部函数的变量的修改, 修改结果保存的方法

   1. python3的方法: 用 nonlocal修饰

      1. 示例

         ```
         def counter(start=0):
             def incr():
                 nonlocal start  #nonlocal修饰外层函数的局部变量, 必须加nonlocal
                 start += 1  #外层变量start值修改,  
                 return start
             return incr
         ```

         ```
         c1 = counter(5)
         c1()  #6
         c1()  #7
         ```

         ```python
         c2 = counter(50)
         c2()  #51
         c2()  #52
         ```

      2. 小结

         1. 调用外层函数后返回内层函数的引用, 
         2. 调用返回的引用, 实际是调用 内层函数
         3. 闭包的局部变量的值一直保存着,  下次调用闭包时仍可用这个值
            4. 除非再一次调用外部函数
         4. 闭包中的外层的局部变量 最好用 `nonlocal` 修饰, 
            1. nonlocal在闭包中 可修饰外层函数的形参变量
         2. `nonlocal`  是 python3中引入的
   
2. python2的方法 : 用 list 类型变量保存
   
   1. 示例
   
         ```
         def counter(start=0):
             count=[start]
             def incr():
                 count[0] += 1
                 return count[0]
             return incr
      ```
   
         ```
         c1 = counter(5)
         c1()   #6
         c1()  #7
         ```
   
         ```
         c2 = counter(100)
         c2()  #101
         c2()  #102
         ```
   
      2. 闭包场景: 
      
         1. 若不用 `nonlocal` 修饰 外层函数的局部变量, 必须在外层函数中创建一个 list 对象 保存外层函数的变量.

#### 19.2 装饰器

1. 函数名 是 变量

   1. 示例

      ```
      #### 第一波 ####
      def foo():
          print('foo')
      
      foo  #函数引用
      foo()  #执行foo函数
      
      #### 第二波 ####
      def foo():
          print('foo')
      
      foo = lambda x: x + 1
      
      #python是重写(或覆盖)前面同名的函数, python没有重载
      foo()  # 执行lambda表达式，而不再是原来的foo函数，因为foo这个名字被重新指向了另外一个匿名函数
      ```

   2. `函数名` 是 变量，是函数的引用,  指向 函数.

   3. `函数名=xxx`,  被修改,  执行其他了内容

2. 需求来了:示例
   1. 问题

      1. 问题描述: 已写好的代码, 添加验证机制. 即, 执行前，先验证

      2. 已写好的代码
         
         ```
         #### 基础平台部 提供的功能如下  ####
         
         def f1():
             print('f1')
         
         def f2():
             print('f2')
         
         def f3():
             print('f3')
         
         def f4():
             print('f4')
         
         #### 业务部门A 调用基础平台提供的功能 ####
         
         f1()
         f2()
         f3()
         f4()
         
         #### 业务部门B 调用基础平台提供的功能 ####
         
         f1()
         f2()
         f3()
      f4()
         ```
         
         

   2. 解决方案:

      1. Low B 做法

         1. 每个业务部门 自己写代码，调用基础平台的功能之前先验证

      2. Low BB 做法

         1. 继承平台的 每个函数添加验证代码, 业务部门不变

            ```
            #### 基础平台提供的功能如下 ####
            
            def f1():
                # 验证1
                # 验证2
                # 验证3
                print('f1')
            
            def f2():
                # 验证1
                # 验证2
                # 验证3
                print('f2')
            
            def f3():
                # 验证1
                # 验证2
                # 验证3
                print('f3')
            
            def f4():
                # 验证1
                # 验证2
                # 验证3
                print('f4')
            
            #### 业务部门不变 #####
            ......
            ```

            

      3. Low BBB 做法

         1. 基础平台 代码重构， 业务部门不变

         2. ```
            ##### 基础平台提供的功能如下 ####
            
            def check_login():
                # 验证1
                # 验证2
                # 验证3
                pass
            
            
            def f1():
            
                check_login()
            
                print('f1')
            
            def f2():
            
                check_login()
            
                print('f2')
            
            def f3():
            
                check_login()
            
                print('f3')
            
            def f4():
            
                check_login()
            
                print('f4')
            ```

         3. 有 装饰器的 思想

      4.  原则 : 开放 封闭

         1. 写代码的原则
         2. 即: 已实现的 功能代码 不允许修改，但可以扩展
            1. 封闭：已实现的功能代码块, 不修改
            2. 开放： 扩展开发

      5. 总结

         ```
         #定义闭包函数
         def w1(func):   #遇到定义, 申请空间生成函数
             def inner():
                 # 验证1
                 # 验证2
                 # 验证3
                 func()
             return inner
         
         @w1
         def f1():
             print('f1')
         ```

         1. python解释器 从上到下解释代码，步骤
            1. def w1(func)   #将w1函数加载到内存
            2. @w1    #@函数名 python的 语法糖
         2. 装饰器:  闭包函数A修饰函数B, 函数A就是函数B的装饰器
            1. `@加有闭包的函数的引用` 就是装饰器

3. 示例中 语法糖`@w1` 内部 操作步骤
      1. 步骤1: 执行w1函数: f1作参数塞进 inner中, 返回 inner引用

         1. *执行* w1函数. 将 @w1 修饰的函数 f1 作 w1 函数的参数. 即,`@w1` 等价于 `w1(f1)` , 其内部 执行函数w1(f1), 

            ```
            def inner(): 
                #验证 1
                #验证 2
                #验证 3
                f1()    #参数func的实参是f1 
         return inner # 返回 inner，inner是函数的引用，非执行函数. 把 f1 函数塞进 inner 函数中
            ```

      2. 步骤2: w1的返回值: `inner`引用赋给 f1

         1. w1函数`返回值` 赋值 给@w1下面的函数的 `函数名f1` , 即 w1的返回值inner赋值给 f1
         2. 以后 执行 f1() 函数,  执行的是新的 inner()函数
         3. inner()函数 先执行验证, 再执行原来的f1函数 

      3. 执行流程 总结: 
         1. @w1下的函数f1 塞入 闭包中, 并返回内层函数inner,  inner又赋给f1, 即f1指向新函数
         2. 新函数f1 先执行验证, 再执行原函数f1,

3. 再议装饰器

   1. ```
      # 定义闭包函数：完成包裹数据
      def makeBold(fn):
          def wrapped():
              return "<b>" + fn() + "</b>"
          return wrapped
      
      # 定义闭包函数：完成包裹数据
      def makeItalic(fn):
          def wrapped():
              return "<i>" + fn() + "</i>"
          return wrapped
      
      @makeBold
      def test1():
          return "hello world-1"
      
      @makeItalic
      def test2():
          return "hello world-2"
      
      @makeBold
      @makeItalic
      def test3():
          return "hello world-3"
      
      print(test1())  #<b>hello world-1</b>
      print(test2())
      print(test3())
      ```

   2. 执行结果

      ```
      <b>hello world-1</b>
      <i>hello world-2</i>
      <b><i>hello world-3</i></b>   #从外向内执行
      ```

      

4. 装饰器(decorator)功能

   1. 引入`日志`
   2. 函数执行`时间统计`
   3. 执行函数前`预备处理`
   4. 执行函数后`清理`功能
   5. 权限`校验`等场景
   6. 缓存

5. 装饰器示例
   1. 例1: 装饰器装饰无参数的函数

      1. 代码

         ```
         from time import ctime, sleep
         
         def timefun(func):
             def wrapped_func():
                 print("%s called at %s" % (func.__name__, ctime()))
                 func()
             return wrapped_func
         
         @timefun
         def foo():
             print("I am foo")
         
         foo()  #foo called at Wed Jun 23 10:21:21 2021
                #I am foo
         sleep(2)
         foo()  #foo called at Wed Jun 23 10:21:23 2021
                #I am foo
         ```

      2. 理解装饰器

         ```
         foo = timefun(foo)  # foo先作为参数赋给func,后 foo指向timefun返回的wrapped_func
         foo()  # 调用 foo(),即等价于调用 wrapped_func()
         # 内部函数wrapped_func被引用，所以外部函数的func变量(自由变量)没有释放
         # func 保存 原foo函数对象
         ```

         

   2. 例2: 装饰器装饰 有参数的的函数

      1. 示例

         ```
         from time import ctime, sleep
         
         def outer(func):
         	#c = 10
             def inner(a, b):
                 print("%s called at %s" % (func.__name__, ctime()))
                 print(a, b)
                 #print(c)  #可以, 正确的
                 func(a, b)
             return inner
         
         @outer
         def foo(a, b):
             print(a+b)
         
         foo(3,5)  #foo called at Wed Jun 23 10:48:22 2021
                   #3 5
                   #8
         sleep(2)
         foo(2,4)  #foo called at Wed Jun 23 10:48:40 2021
                   #2 4
                   #6
         ```

      2.  闭包的参数签名 与 foo的参数签名 **一致**. 即参数的类型, 个数, 位置相同

         1. inner的参数签名 与 foo的参数签名 一致.
         2. inner的参数只能接受foo的实参, 不能接受outer内的变量
         3. inner的内部可以用 outer内的变量, outer的参数除外

   3. 例3:被装饰的函数有不定长参数

      1. 示例

         ```
         from time import ctime, sleep
         
         def outer(func):
             def inner(*args, **kwargs):
                 print("%s called at %s"%(func.__name__, ctime()))
                 func(*args, **kwargs)
             return inner
         
         @outer
         def foo(a, b, c):
             print(a+b+c)
             
             
         foo(3,5,7)  #foo called at Wed Jun 23 11:02:50 2021
                     #15
         ```

   4. 例4:装饰器中的`return`

      1. 装饰中有 return

         ```
         from time import ctime, sleep
         
         def outer(func):
             def inner():
                 print("%s called at %s" % (func.__name__, ctime()))
                 return func()
             return inner
         
         @outer
         def foo():
             print("I am foo")
         
         @outer
         def getInfo():
             return '----hahah---'
         ```

         ```
         foo()  #foo called at Wed Jun 23 11:07:57 2021
                #I am foo
         ```

         ```
         getInfo()  #getInfo called at Wed Jun 23 11:08:05 2021
                    #'----hahah---'
         ```

      2. 装饰器中没有 return

         ```
         from time import ctime, sleep
         
         def outer(func):
             def inner():
                 print("%s called at %s" % (func.__name__, ctime()))
                 func()
             return inner
         
         @outer
         def foo():
             print("I am foo")
         
         @outer
         def getInfo():
             return '----hahah---'
         ```

         ```
         foo()  #foo called at Wed Jun 23 11:10:52 2021
                #I am foo
         ```

         ```
         getInfo()  #getInfo called at Wed Jun 23 11:10:59 2021
         ```

      3. 总结:

         1. 为了通用, 装饰器中可有`return`

   5. 例5:装饰器带参数. 即 原有装饰器的基础上，加 外部变量

      1. 解决方法: 构建三层闭包,每层返回 相邻内层函数引用, 三层函数作装饰器

         ```
         
         
         ​```
         from time import ctime, sleep
         
         def outer(pre="hello"):
             def middler(func):
                 def inner():
                     print("%s called at %s %s" % (func.__name__, ctime(), pre))
                     return func()
                 return inner
             return middler
         
         @outer("itcast")  #先执行装饰器,返回midder
         def foo():  #再foo=middler(foo), middler(foo)返回inner, foo=inner, 执行foo()相当于执行inner()
             print("I am foo")
         
         @outer("python")
         def too():
             print("I am too")
         ```

         ```
         foo()  #foo called at Wed Jun 23 11:40:33 2021 itcast
                #I am foo
         ```

         ```
         too()  #too called at Wed Jun 23 11:40:41 2021 python
                #I am too
         ```

      2. 带参数 装饰器 执行过程

         1. 调用outer("itcast")
         2. 将步骤1得到的返回值，即middler返回， 然后middler(foo)
         3. 将middler(foo)的结果返回，即inner
         4. 让foo = inner，即foo现在指向inner

   6. 例6：类装饰器（扩展，非重点）

      1. 装饰器函数 本质 

         1.  是一个 `接口约束`, 接受一个 callable对象 作为参数,  返回一个 callable对象

      2. callable对象

         1. Python中一般指 函数
         2. 重写了  `__call__()` 方法的 类的对象, 也是callable的

      3. 示例

         ```
         class Test():
             def __call__(self):
                 print('call me!')
         
         t = Test()  #t对象是 callable的
         t()  # call me, 
         ```

      4. 类装饰器demo

         1. 代码示例

            ```
            class Test(object):
                def __init__(self, func):
                    print("--初始化--")
                    print("func name is %s"%func.__name__)
                    self.__func = func
                def __call__(self):
                    print("--装饰器中的功能--")
                    self.__func()
            
            @Test
            def foo():
                print("--foo--")
            ```

            运行pycharm输出

            ```
            --初始化--
            func name is foo
            ```

            执行:`foo()` ,  输出

            ```
            --装饰器中的功能--
            --foo--
            ```

         2. 执行过程

            1. 用`Test`类  作装饰器对 `foo()`函数 装饰, 先 创建Test类的实例对象, 把`foo函数名`参数传到`__init__`方法中,   即`__init__`方法中的属性`__func`指向 foo指向的函数

            2. 然后, foo指向了 Test类的 实例对象, 即 foo = 实例对象

            3. 调用`foo()`时, 相当于调用 `实例对象()`，自动执行对象的`__call__`方法

            4. 实现方法:

               1.   `__init__`方法中用一个 实例属性 保存 foo指向的原函数体

                  `self.__func = func`, 

               2.  `__call__`方法中 调用 保存的函数

                  `self.__func()`    #`__` 表示是实例对象私有

-----------

#### 02-闭包

###### 视频版本

##### 01-闭包-1

1. 内层函数用到外层函数的变量

##### 02-闭包-2

1. `__call__`  魔法方法,  对象()  触发调用

   ```
   class Line():
   	def __init__(self, b, k)
   		self.b = b
   		self.k = k
   	def __call__(self, x)
   		print(self.k * x + self.b)
   		
   v1 = Line(1, 2)
   v(5)  # v() 自动触发调用__call__(x)方法
   ```

2. 闭包

   ```
   def fun1(a1):
   	def fun2(a2)
   		print(a1 + a2)
   	return fun2
   	
   v3 = fun1(1)  # fun1(1)把1传给a1,返回fun2, fun1(1)返回值指向fun2函数, fun2引用赋值给#v3. 
   v3(2)     #fun3是fun2, v3(2)就是fun2(2),调用fun2(2)函数
   ```

   1.  两个函数嵌套定义
   2.  内层函数用到外层函数的变量
   3.  外层函数返回内层函数名引用, 返回值必须是**函数名**
   4. 内层函数 和 外层变量 的整体 成为 闭包

##### 03-闭包-3-补充

1. 闭包
   1. 闭包 代码简介
   2. 有数据, 有代码(功能)

##### 04-闭包-4-修改数据

1. nonlocal,  python3有

2.  ```
   def  fun1():
       x = 200
       def fun2():
           nonlocal x
           print("1", x)
           x = 300
       return fun2
   ```

   1.  内层函数修改外层函数变量

#### 03-装饰器

##### 01-装饰器-1-介绍

1. @闭包名+ def 函数,  或 闭包修饰函数

2. 装饰器

   ```
   def fun1(a):
       def fun2():
           print(1)
           a()     # 对应 fun3() 函数
       return fun2
       
   @fun1
   def fun3():    # fun3 = fun1(fun3), fun3做参数传入fun1, 返回值fun2赋给fun3
       print(3)
   ----执行-------
   fun3()
   ```

   1. 闭包的外层函数必须有一个参数

   2. 公式: fun3 = fun1(fun3)

   3. 过程:

      1. fun3作参数传入fun1, 返回值 fun2 赋给 fun3, fun2必须是函数名

      2. 特简例:

         ```
         def fun1(a):   #传入的变量不是是函数名才可以
             return a
         @fun1
         def fun3():
             print(3)
         ```

         fun3()  # 执行

3. 原理(同等效果) 

   ```
   def fun1(a):
       def fun2():
           print(1)
           a()
       return fun2
       # return 1
   
   def fun3():
       print(3)
   
   v4 = fun1(fun3)
   fun3 = v4
   
   ----执行-------
   fun3()
   ```



##### 02-装饰器-2-手动实现装饰器

1.  用途: 在一个函数之前或之后实行某种任务

##### 03-装饰器-3-作用demo、对有参数函数、无参数函数的装饰

1.  装饰有参函数

   ```
   def fun1(a):
       def fun2(num):
           print(1)
           a(num)     # 对应 fun3(num) 函数
       return fun2
       
   @fun1
   def fun3(num):    # fun3 = fun1(fun(num)), 
       print(3)
   ----执行-------
   fun3()
   ```

   1. @有特殊功能的意思

##### 04-装饰器-4-再理解

1. python解释器遇到装饰器就装饰, 遇到@就装饰
2. python在调用装饰器之前已经装饰了

##### 05-装饰器-5-不定长参数的函数装饰

```
def fun1(func):
	def fun2(*args, **kwargs):  #2闭包也必须有不定长参数
		print("---1---")
		func(*args, **kwargs)  # 拆包
	return fun2
@fun1
def fun3(num, *args, **kwargs):  #1被装饰函数有不定长参数
	print('--2---')
```



##### 06-装饰器-6-对应有返回值函数进行装饰、通用装饰器

1. 通用装饰器

   ```
   def fun1(func):
   	def fun2(*args, **kwargs):
   		print("---1---")
   		return func(*args, **kwargs)  # 拆包, 加入没有返回值没有影响
   	return fun2
   @fun1
   def fun3(num, *args, **kwargs):
   	print('--2---')
   	return 'OK'
   ```

   1. return 对没有返回值没有影响	

##### 07-装饰器-7-多个装饰器对同一个函数装饰

1. ```
   def fun11(func):
   	print("--开始装饰fun11--")
   	def fun12(*args, **kwargs):
   		print("--验证1--")
   		return func(*args, **kwargs)
   	return fun12
   	
   def fun21(func):
   	print("--开始装饰fun21--")
   	def fun22(*args, **kwargs):
   		print("--验证2--")
   		return func(*args, **kwargs)
   	return fun22
   
   #开始装饰: 由内向外装饰, (先fun21装饰, 在fun11装饰)
   @fun11
   @fun21
   def func():
   	print('--func--')
   ```
   
   1. 结果

      ```
   --开始装饰fun21--
      --开始装饰fun11--
      ```
      
   
2. ```
   #执行过程: 由外向内执行( 先f12, 在f22)
   func()  #执行
   ```

   1. 结果

      ```
      --验证1--
      --验证2--
      --func--
      ```

3. 装饰过程: 由下向上.  执行过程: 由上向下(装饰: 由内向外.  执行 由外向内, 层层调用)

##### 08-装饰器-8-多个装饰器对同一个函数装饰demo

1. 装饰器模板

   1. ```
      def func1(func):
      	def func2(*args, **kwargs):
      		......
      		func()
      		return None  #最内层可以没有return语句
      	return func2
      	
      @func1
      def fun(*args, **kwargs):
      	......
      ```

   2.装饰器执行步骤:

   	1. 申请内存空间创建func1.  id(func1) 是 79699400
   	2. fun做func1的参数传入, 并申请内存空间创建fun. id(fun)是79798344
   	3. 新函数返回func2, fun指向func2
   	4. func2 传入参数(*args, **kwargs),  调用创建的函数. 即加() 调用是最后一步

2. functools.wraps

   1. 可以将原函数对象的指定属性复制给包装函数对象, 默认有 **module**、**name**、**doc**,或者通过参数选择

      1. ```
         from functools import wraps
         def outer(func):
             @wraps(func)
             def wrapper(*args,**kwargs):
                 print("before func")
                 wrapper_result=func(*args,**kwargs)
                 print("after func")
                 return wrapper_result
             return wrapper
         
         @outer
         def foo(a,b):
             return a+b
         ```

         ```
         >>> print(foo.__name__)
         foo
         ```

##### 带参数的装饰器

1. ```
   def outer(outer_args):
       def middle(func):
           def wrapper(*args,**kwargs):
               print("before func")
               print("use args: "+outer_args)
               wrapper_result=func(*args,**kwargs)
               print("after func")
               return wrapper_result
           return wrapper
       return middle
   
   @outer("hello")
   def foo(a,b):
       return a+b
   ```

   ```
   >>> res=foo(3,3)
   before func
   use args: hello
   after func
   >>> res
   6
   ```

   1. 带参数修饰器效果等价于

      1. ```
         >>> foo=outer("hello")(foo)
         ```

      2. 解析

         1. 括号后还有一个括号，说明第一个函数返回了一个函数
         2. 如果后面还有括号，说明前面那个也返回了一个函数。以此类推。
         3. 这叫 python method chaining

   2. 装饰器函数特点:

      1. 三层函数嵌套定义

   3. 执行过程:

      1. 先执行装饰器的函数 outer("hello"),  实参'hello' 传入, 返回 函数引用 middle

         1. 变形为:

            ```
            @middle
            def foo(a, b):
            	return a+b
            ```

      2. 再执行装饰器 `@middle`

2. 类后面多个括号连续写的代码写法

   1. ```
      class add(int):
          def __call__(self,n):
      
              return add(self+n)
      
      print add(1)(2)(3)
      ```

   2. 解析:

      1. 首先要理解这个`__call__`方法
         1. 当类中定义了`__call__`方法后，类的实例对象可以带括号调用。
            `class_x(arguments)<=>class_ x.__call__(arguments)`
      2. add(1) 是类的实例对象。
         1. 类中定义了方法`__call__ `所以实例对象add(1)(2) 是调用`__call__`方法，`__call__`方法要参数，所以这里传了一个实际参数2。
      3. python特殊函数` __call__()`
         1. Python中，函数是一个对象,函数可以被调用，所以，函数对象被称为可调用对象。
         2. 一个类实例可以变成一个可调用对象，只要实现一个`__call__()`。

##### 09-装饰器-9-（了解）用类对函数进行装饰

1. ```
   # 用类装饰函数
   
   class Test(object):
       def __init__(self, func):
           self.func = func
   
       def __call__(self, *args):
           print("--装饰器添加的功能--", *args)
           return self.func()
   
   #类作装饰器
   @Test
   def func():  # func = Test(func)
       return "hha"
   
   func   #<__main__.Test at 0x4bf0588>
   
   func(10, 22)
   ```
```
   
   1. class 类是双层的
   
   2. `__call__` 可以带多个参数
   
   3. 结果:
   
      1. ```
         --装饰器添加的功能-- 10 22
         out : 'haha'
```

​         

--------------

### 20 mini-web框架 添加路由、MySQL功能

###### 文档版本

#### 20.1 mini web框架-4-路由

1. dynamic/my_web.py   #web框架: 执行功能, (或 应用程序: 执行功能)

   ```
   import time
   import os
   import re
   
   template_root = "./templates"
   
   # ----------更新----------
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py": index_func,
   #   "/center.py": center_func
   # }
   
   #用于url与方法一一配对
   g_url_route = dict()
   
   # ----------更新----------
   #三层嵌套函数定义, 为了做带参数的装饰器
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url] = func  #保存func的,使url与方法一一对应起来
           
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   #装饰器此处作用: 建立"/index.html"与index的对应关联, url是键, index函数名是值
   @route("/index.py")  # ----------更新----------
   def index(file_name):
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")  #'.py'被'.html'代替
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           content = re.sub(r"\{%content%\}", data_from_mysql, content)
   
           return content
   
   
   @route("/center.py")  # ----------更新----------
   def center(file_name):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           content = re.sub(r"\{%content%\}", data_from_mysql, content)
   
           return content
   
   #框架中的application方法处理客户请求. 
   #参数1: 客户请求文件名, 参数2: 函数引用 WSGI服务器设置响应头的函数, 
   def application(environ, start_response):
   	#状态
       status = '200 OK'
       #响应头
       response_headers = [('Content-Type', 'text/html')]
       #调用web_server.py中函数, 设置状态码, 响应头信息
       start_response(status, response_headers)

       file_name = environ['PATH_INFO']
       # ----------更新----------
       try:
       	#g_url_route是dict类型, 键是url, 值是函数体引用
           return g_url_route[file_name](file_name)  
           #g_url_route[file_name] 是函数名, 加上()参数, 是执行装饰器函数
           
       except Exception as ret:
           return "%s" % ret
   ```
   
2. web_server.py

   ```
   import select
   import time
   import socket
   import sys
   import re
   import multiprocessing
   
   
   class WSGIServer(object):
       """定义一个WSGI服务器的类"""
   
       def __init__(self, port, documents_root, app):
   
           ......
   
           #请求静态资源, 静态资源文件的路径
           self.documents_root = documents_root
   
           #请求动态资源时, 设定web框架可以调用的函数(对象)
           self.app = app
   
       def run_forever(self):
           pass
   
       def deal_with_request(self, client_socket):
           """处理客户端请求. 以长链接的方式，为浏览器服务"""
   		......
   
                   #调用my_web.py模块(模拟框架)的方法处理请求,返回值保存到response_body中
                   response_body = self.app(env, self.set_response_headers)
   				
   				......
                   response += response_body
                   client_socket.send(response.encode('utf-8'))
   
   
       def set_response_headers(self, status, headers):
           """这个方法，会在 web框架中被默认调用"""
           ......
           self.headers = [status, response_header_default + headers]
   
   
   # 设置静态资源访问的路径
   g_static_document_root = "./static"
   # 设置动态资源访问的路径
   g_dynamic_document_root = "./dynamic"
   
   def main():
       """控制web服务器整体"""
   	......
       if ret:
           # 获取模块名
           web_frame_module_name = ret.group(1)   #返回值 'my_web'
           # 获取可以调用web框架的应用名称
           app_name = ret.group(2)  #返回值: 'application'
   
       # 导入web框架的主模块
       web_frame_module = __import__(web_frame_module_name)
       # 获取那个可以直接调用的函数(对象)
       app = getattr(web_frame_module, app_name)   #返回值: 一个函数对象
   
       # 启动http服务器
       #port端口, g_static_document_root静态资源 对应静态请求, app处理动态请求
       http_server = WSGIServer(port, g_static_document_root, app)
       # 运行http服务器
       http_server.run_forever()
   
   
   if __name__ == "__main__":
       main()
   ```

#### 20.2 伪静态、静态和动态的区别

* 动态网站
* 静态URL、动态URL、伪静态URL

1. 静态URL
   1. 静态URL
      1. `域名/news/2012-5-18/110.html`,  真静态URL, 是`物理地址`
      2. 每个网页有真实的 物理路径，即 每个网页 真实存在服务器里 
   2. 优点
      1. 速度 快, 网址结构 友好
   3. 缺点
      1. 中大型网站， 页面 多，不好管理
   4. 总结:
      1. 静态URL对SEO 有加分的影响. 打开速度快, 是本质
      2. *SEO*（Search Engine Optimization），中文翻译为“搜索引擎优化”
2. 动态URL
   1. 动态URL
      1. `域名/NewsMore.asp?id=5`   或  `域名/DaiKuan.php?id=17`    带`？`URL
      2. 每个URL 是一个逻辑地址，网页不 真实 物理存在服务器硬盘
   2. 优点
      1. 适合中大型网站，修改页面 方便， 逻辑地址, 比纯静态网站小
   3. 缺点
      1. 要 运算,  速度稍慢.  服务器缓存技术 可 解决速度问题
      2. URL结构 复杂，不 利于记忆
   4. 总结
      1. 动态URL对`SEO`  有影响.   百度`SE`已 很好的理解 `动态URL`
3. 伪静态URL
   1. 伪静态URL
      1.  `域名/course/74.html`,   和真静态URL类似。
      2. 用 `伪静态规则` 把 `动态URL`伪装成 `静态URL`
      3. 逻辑地址，不存在 物理地址
   2. 优点
      1. URL比较友好，利于记忆
      2. 适合大中型网站，是个 折中方案
   3. 缺点
      1. 设置`麻烦`，服务器要支持`重写规则`，
         1. 小企业网站或 玩不好的 不要折腾
      2. 伪静态URL 速度 没 快, 额外运算解释, 增加服务器负担，速度反而变慢
      3. 动态URL和静态URL 被 搜索引擎 收录. 用`robots`禁止掉 动态地址
   4. 总结
      1. 和动态URL一样，对SEO没有什么减分影响

#### 20.3 mini-web框架-实现伪静态url   

* web框架指  application函数

1. readme.txt(新建)

   运行方式`python3 web_server.py 7890 my_web:application`

   网页运行: `127.0.0.1/index.html`

2. web_server.py(部分更新)

   ```
   import select
   import time
   import socket
   import sys
   import re
   import multiprocessing
   
   
   class WSGIServer(object):
       """定义一个WSGI服务器的类"""
   
       def __init__(self, port, documents_root, app):
   
           # 1. 创建套接字
           self.server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
           # 2. 绑定本地信息
           self.server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
           self.server_socket.bind(("", port))
           # 3. 变为监听套接字
           self.server_socket.listen(128)
   
           #静态资源文件的路径
           self.documents_root = documents_root
   
           #动态资源请求处理
           #设定web框架可以调用的函数(对象)
           #设置: 状态码, 响应头, 响应body
           self.app = app
   
       def run_forever(self):
           """运行服务器"""
   
           # 等待对方链接
           while True:
               new_socket, new_addr = self.server_socket.accept()
               # 创建一个新的进程来完成这个客户端的请求任务
               new_socket.settimeout(3)  # 3s
               new_process = multiprocessing.Process(target=self.deal_with_request, args=(new_socket,))
               new_process.start()
               new_socket.close()
   
       def deal_with_request(self, client_socket):
           """以长链接的方式，为这个浏览器服务器"""
   
           while True:
               try:
                   request = client_socket.recv(1024).decode("utf-8")
               except Exception as ret:
                   print("========>", ret)
                   client_socket.close()
                   return
   
               # 判断浏览器是否关闭, 没有收到请求 关闭
               if not request:
                   client_socket.close()
                   return
                   
   			#对收到的请求request按行split
               request_lines = request.splitlines()
               for i, line in enumerate(request_lines):
                   print(i, line)
   
               # 提取请求的文件(index.html). 正则方法提取浏览器的请求信息
               # GET /a/b/c/d/e/index.html HTTP/1.1
               ret = re.match(r"([^/]*)([^ ]+)", request_lines[0])
               if ret:
                   print("正则提取数据:", ret.group(1))
                   print("正则提取数据:", ret.group(2))
                   file_name = ret.group(2)
                   if file_name == "/":
                       file_name = "/index.html"
   			
   			#判断请求在 普通文件 中还是 html文件 中
               # 如果不是以.html结尾的文件,都认为是普通的文件
               # 如果是.html结尾的请求,那么就让web框架进行处理
               
               #普通文件中
               if not file_name.endswith(".html"):  # --------- 进行了更新-------
   
                   # 读取文件数据
                   try:
                       print(self.documents_root+file_name)
                       f = open(self.documents_root+file_name, "rb")
                   except:
                       response_body = "file not found, 请输入正确的url"
   
                       response_header = "HTTP/1.1 404 not found\r\n"
                       response_header += "Content-Type: text/html; charset=utf-8\r\n"
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       response = response_header + response_body
   
                       # 将header返回给浏览器
                       client_socket.send(response.encode('utf-8'))
   
                   else: #try没有异常执行 else 块
                       content = f.read()
                       f.close()
   
                       response_body = content
   
                       response_header = "HTTP/1.1 200 OK\r\n"
                       
                       #长连接设置
                       response_header += "Content-Length: %d\r\n" % (len(response_body))
                       response_header += "\r\n"
   
                       # 将header返回给浏览器
                       client_socket.send(response_header.encode('utf-8') + response_body)
   
               # 以.py结尾的文件，就认为是浏览需要动态的页面
               else:
                   # 准备一个字典，存放传递给web框架的数据
                   env = dict()
                   env['PATH_INFO'] = file_name  # 例如 index.py
   
                   #调用web框架(应用程序), 参数1是访问文件名,参数2 是函数引用 用于设置响应头
                   response_body = self.app(env, self.set_response_headers)
   
                   # 合并header和body
                   response_header = "HTTP/1.1 {status}\r\n".format(status=self.headers[0])
                   response_header += "Content-Type: text/html; charset=utf-8\r\n"
                   response_header += "Content-Length: %d\r\n" % len(response_body.encode("utf-8"))
                   for temp_head in self.headers[1]:
                       response_header += "{0}:{1}\r\n".format(*temp_head)
   
                   response = response_header + "\r\n"
   
                   response += response_body
   
                   client_socket.send(response.encode('utf-8'))
   
   	
       def set_response_headers(self, status, headers):
           """这个方法，会在 web框架中被默认调用"""
           response_header_default = [
               ("Data", time.time()),
               ("Server", "ItCast-python mini web server")
           ]
   
           # 将状态码/相应头信息存储起来
           # [字符串, [xxxxx, xxx2]]
           self.headers = [status, response_header_default + headers]
   
   
   # 设置静态资源访问的路径
   g_static_document_root = "./static"
   # 设置动态资源访问的路径
   g_dynamic_document_root = "./dynamic"
   
   def main():
       """控制web服务器整体"""
       # python3 xxxx.py 7890
       if len(sys.argv) == 3:
           # 获取web服务器的port
           port = sys.argv[1]   #7890
           if port.isdigit():
               port = int(port)
           # 获取web服务器需要动态资源时，访问的web框架名字
           web_frame_module_app_name = sys.argv[2]  #'my_web:application'
       else:
           print("运行方式如: python3 xxx.py 7890 my_web_frame_name:application")
           return
   
       print("http服务器使用的port:%s" % port)
   
       # 将动态路径即存放py文件的路径，添加到path中，这样python就能够找到这个路径了
       sys.path.append(g_dynamic_document_root)
   
       ret = re.match(r"([^:]*):(.*)", web_frame_module_app_name)   #'my_web:application'
       if ret:
           # 获取模块名
           web_frame_module_name = ret.group(1)   #"my_web"
           # 获取可以调用web框架的应用名称
           app_name = ret.group(2)    #"application"
   
       # 导入web框架的主模块. 内存中导入模块, 立即执行模块中的语法糖
       web_frame_module = __import__(web_frame_module_name)   #my_web.py模块
       # 获取框架中 直接调用的函数(对象). 从py模块中获取方法或属性
       app = getattr(web_frame_module, app_name)  #app是函数引用
   
       # print(app)  #<function application at 0x00000000242AE50>
   
       # 启动http服务器. main函数中启动服务器, 并把框架app传给服务器
       http_server = WSGIServer(port, g_static_document_root, app)
       
       # 运行http服务器
       http_server.run_forever()
   
   
   if __name__ == "__main__":
       main()
   ```

   1. 内存中导入模块, **立即**执行模块中的语法糖
   2. main() 中 `web服务器`与 框架中的`applicaiton`函数组合起来
   3. `Web服务器` 调用框架中的 `application`函数, 并传入 浏览器访问的文件 和 WSGI类中设置响应头的函数的引用
   4. my_web.py框架 定义 application函数, application函数中 调用 WSGI类中处理响应头的函数.

3. my_web.py(部分修改)   #web框架

   ```
   import time
   import os
   import re
   
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   
   def route(url):               #第一层
       def func1(func):          #第二层
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name): #第三层
               return func(file_name)
           return func2
       return func1
   
   
   @route("/index.html")  # ------- 修改------
   def index(file_name):
       """返回index.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           content = re.sub(r"\{%content%\}", data_from_mysql, content)
   
           return content
   
   
   @route("/center.html")  # ------- 修改------
   def center(file_name):
       """返回center.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           content = re.sub(r"\{%content%\}", data_from_mysql, content)
   
           return content
   
   #框架中的application方法 用于处理客户请求. 参数1 是客户请求文件名, 参数2是函数引用 WSGI服务器设置响应头的函数
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           return g_url_route[file_name](file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           return str(environ) + '-----404--->%s\n'
   ```

   

#### 20.4 准备股票数据

1. 登录mysql

   windwo > 开始 > 运行 > cmd

   `mysql -uroot -p,  mysql`

2. 创建数据库

   `create database stock_db charset=utf8;`

3. 选择数据库

   `use stock_db;`

4. 导入数据

   `source stock_db.sql`    #导入stock_db.sql数据库

5. 表结构如下

   1. `desc focus;`

      ```
      +-----------+------------------+------+-----+---------+----------------+
      | Field     | Type             | Null | Key | Default | Extra          |
      +-----------+------------------+------+-----+---------+----------------+
      | id        | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
      | note_info | varchar(200)     | YES  |     |         |                |
      | info_id   | int(10) unsigned | YES  | MUL | NULL    |                |
      +-----------+------------------+------+-----+---------+----------------+
      ```

   2. `desc info;`

      ```
      +----------+------------------+------+-----+---------+----------------+
      | Field    | Type             | Null | Key | Default | Extra          |
      +----------+------------------+------+-----+---------+----------------+
      | id       | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
      | code     | varchar(6)       | NO   |     | NULL    |                |
      | short    | varchar(10)      | NO   |     | NULL    |                |
      | chg      | varchar(10)      | NO   |     | NULL    |                |
      | turnover | varchar(255)     | NO   |     | NULL    |                |
      | price    | decimal(10,2)    | NO   |     | NULL    |                |
      | highs    | decimal(10,2)    | NO   |     | NULL    |                |
      | time     | date             | YES  |     | NULL    |                |
      +----------+------------------+------+-----+---------+----------------+
      ```

#### 20.5 mini-web框架-从mysql中查询数据

1. my_web.py(更新)   #相当于 web框架

   ````
   import pymysql
   import time
   import os
   import re
   
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   
   @route("/index.html")
   def index(file_name):
       """返回index.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # --------添加---------
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           content = re.sub(r"\{%content%\}", str(data_from_mysql), content)
   
           return content
   
   
   @route("/center.html")
   def center(file_name):
       """返回center.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           content = re.sub(r"\{%content%\}", data_from_mysql, content)
   
           return content
   
   
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           return g_url_route[file_name](file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           return str(environ) + '-----404--->%s\n'
   ````

   

#### 20.6 mini-web框架-组装数据为html格式

1. my_web.py(更新)   #相当于web框架

   ```
   import pymysql
   import time
   import os
   import re
   
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   @route("/index.html")
   def index(file_name):
       """返回index.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)  #打开 html请求的文件 
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()  #读取 html请求的文件的内容
           f.close()
   
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   		
   		#网页 是字符串
           html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
                   </tr>"""
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   		#用`\{%content%\}`匹配content中的内容, 匹配的地方用html的内容代替
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route("/center.html")
   def center(file_name):
       """返回center.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   		
   		#`\{%content%\}`是正则表达式, 匹配content的内容,`\`转义字符 对`{`与`}`转义,`%`普通符号 没有任何意义.
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   # 参数1是浏览器请求的文件的url,  参数2是 WSGI类中设置响应头的函数的引用
   def application(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           return g_url_route[file_name](file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           return str(environ) + '-----404--->%s\n'
   ```
      1. 框架中返回给浏览器的信息, 在网页中有对应的字段接收, 才能显示. 没有接收字段, 无法在网页上显示的.
   
               1. 即 `.html` 文件中有对应的字段才可以显示
   
      3. 浏览器接收的是字符串,   根据字符串中的 html的标记解析字符串. 
   
      4. 举例
   
            1. 框架应用中语句  `re.sub(r"\{%content%\}", html, content)`  字符串html替换content中与`\{%content%\}`匹配的内容. 
   
                     1. 在模板文件 如index.html中也有 `\{%content%\}` . 这两`\{%content%\}`对应的部分被html的字符串代替.
                     2. 不是`\{%content%\}`, 其他re字符串也可以. 只有两个一样就可以用re.sub()替换
              
         2. my_web.py, #   
         
            1. ```
                    ........
                    @route(r"/index.html")
                    def index(file_name, url=None):
                    	.........
                ```
         
              		#用正则表达式"1content1"匹配content, 匹配的地方用html替换, 然后返回给调用者, 返回内容是 html字符串. content是index.html中的字符串
              	            return_content = re.sub(r"1content1", html, content)
              	........
              	            return return_content
               ```
            
               ```
         
         3. index.html
         
            1. ```
                    <!DOCTYPE html>
                    <html lang="en">
                    <head>
                        <meta charset="UTF-8">
                        <title>index only</title>
                    </head>
                    <body>
                    index
                    1content1
                index
                    </body>
                    11111
                    </html>
               ```
         
               1. `.html`文档中的`1content1`被my_web.py中匹配的`1content1` 用html替换,




​      

​      

-----

#### 04-mini-web框架添加路由和MySQL功能

###### 视频版本

##### 01-带有参数的装饰器.flv

1. ```
   # 带参数的装饰器
   -----
   def fun0(level_num):
   	def fun1(func):
   		def fun2(*args, **kwargs):
   			if level_num == 1:
   				print("--级别1--")
               elif level_num == 2:
               	print("--级别2--")
               return func()
           return fun2
       return fun1
       
   @fun0(1)  #1 先执行fun0(1). 2 再返回值fun1 对func装饰, 即@fun1装饰func
   def func():
   	print("--func--")
   	return "OK"
   	
   func()
   ```

   1. 流程2步 (先传参, 再装饰)
      1. 先调用fun0(1), 传递参数
      2. 返回值 fun1装饰func, 即@fun1装饰func
   2. 装饰器是3层函数定义, 每层都返回对应函数名
   3. 参数:
      1.  fun0: 装饰器参数,  如权限level_num
      2.  fun1: 第二层函数的参数
      3.  fun2: 第三层函数的参数

##### 02-用装饰器完成路由功能-1.flv

1. dict实现路由功能: 路径与相应函数的一一匹配, 代码

   1. ```
      def index():
      	pass
      	
      def center():
      	pass
      	
      URL_FUNC_DICT = {
      '/index.py': index,   #前面已经定义了, 可以用,
      '/center.py': center
      }
      
      def application(env, start_response):
      	start_response('200 OK',[('Content-Type','text/html;charset=utf-8')])
      	file_name = env['PATH_INFO']
      	func = URL_FUNC_DICT[file_name]
      	return func()
      ```

      1. func  引用函数,   func() 调用函数

##### 03-用装饰器完成路由功能-2.flv

1. 带参数装饰器实现路由功能

2. 代码

   1. ```
      URL_FUNC_DICT = dict()
      
      def route(url):
      	def set_func(func):
      		# URL_FUNC_DICT["/index.py"] = index
      		# URL_FUNC_DICT["center.py"] = center
      		URL_FUNC_DICT[url] = func
      		def call_func(*args, **kwargs):
      			return func(*args, **kwargs)
      		return call_fun
      	return set_func
      
      @route("/index.py")
      def index():
      	pass
      	
      @route("/center.py")	
      def center():
      	pass	
      
      def application(env, start_response):
      	start_response('200 OK',[('Content-Type','text/html;charset=utf-8')])
      	file_name = env['PATH_INFO']
      	func = URL_FUNC_DICT[file_name]
      	return func()
      ```

   2. 装饰器 : 三层 
      1. 第一层传入 装饰器 参数, 返回值: 第二层函数引用
      2. 第二层传入 被装饰函数, 返回值: 第三层函数引用
      3. 第三层传入 被装饰函数的实参, 返回值: 被装饰函数嵌入的函数

   3. python解释器遇到装饰器 **立即装饰** 的特性

      

      

##### 04-用装饰器完成路由功能-3.flv

1.  装饰器:

   1.  函数作参数被被调用,  返回函数引用

2. 装饰器函数的 调用, 用 字典键取值加() 实现

   1. ```
      def application(env, start_response):
      	start_response('200 OK',[('Content-Type','text/html;charset=utf-8')])
      	file_name = env['PATH_INFO']
      	try:
      		return URL_FUNC_DICT[file_name]()
      	except Exception as ret:
      		return "异常%s" % str(ret)
      ```

   

##### 05-用装饰器完成路由功能-4.flv

 	1.  路由: 
 	   	1.  一种特殊设备: 连接两个网络, 实现数据转发
 	2.  路由功能:
 	   	1.   根据请求不同 调用不用的函数
 	   	2.  原理: 映射, 请求 映射 处理请求的函数

##### 06-静态、动态、伪静态url.flv

 	1. 网页有真实的物理路径. 文件直接存于 某个硬盘某个文件夹下的某个文件, 固定不变
 	  	1. 快
 	  	2. SEO, 搜索引擎的优化
 	2. 带?url 动态URL,  
 	  	1. URL只是一个逻辑地址，并不是真实物理存在服务器硬盘里的
 	3. 伪静态规则把动态URL伪装成静态网址。是逻辑地址，不存在物理地址
 	  	1. 把 URL 改成.html结尾

##### 07-让web服务器支持伪静态.flv

1.  模板中超链接 以`.html`结尾
2.  装饰器 以 `.html`结尾的实参

##### 08-mini_frame框架添加MySQL功能-1.flv

1. 准备数据

   ```
   1. 创建数据库
   create database stock_db charset=utf8;
   2. 选择数据库
   use stock_db;
   3. 导入sql数据
   source stock_db.sql  # 当前路径下,能看到stock_db.sql文件执行此语句
   4. 导入语法:
   	source sql文件路径
   	示例: source C://abc//stock_db.sql,   没有分号
   ```

2. ACID, 原子性, 一致性, 隔离性, 持久性

3. 数据库操作

   ```
   # 创建Connection 连接
   
   @route("/index.html")
   def index(file_name):
   	......
   	with open("/.templates/index.html") as f:
   		content = f.read()
   	......
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
       sql = """select * from info;"""
       cursor.execute(sql)
       data_from_mysql = cursor.fetchall()
       cursor.close()
       db.close()
   
        html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
              </tr>"""
   
   	html = ""
   
   	for info in data_from_mysql:
   		html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   	content = re.sub(r"\{%content%\}", html, content)
   
   	return content
   ```



##### 09-mini_frame框架添加MySQL功能-2.flv

1. ```
   @route("/center.html")
   def center(file_name):
       """返回center.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   ```

2. 切面编程

   1. 编写函数,  装饰器调用的方法de编程
   2. python 装饰器 特有

----

### 21 mini-web框架 添加log日志、路由支持正则

###### 文档版本

#### 21.1- mini-web框架 添加log日志、路由支持正则

1. my_web.py #相当于web框架应用

   ```
   import pymysql
   import time
   import os
   import re
   
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   @route(r"/index.html")
   def index(file_name, url=None):
       """返回index.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
                   </tr>"""
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/center.html")
   def center(file_name, url=None):
       """返回center.html需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   # ------- 添加 -------
   @route(r"/update/(\d*)\.html")
   def update(file_name, url):
       """显示 更新页面的内容"""
       try:
       	#对文件路径进行更换
           template_file_name = template_root + "/update.html"
           f = open(template_file_name)
       except Exception as ret:
           return "%s,,,没有找到%s" % (ret, template_file_name)
       else:
           content = f.read()
           f.close()
   
           ret = re.match(url, file_name)
           if ret:
               stock_code = ret.group(1)
   
           # 将提取到的股票编码返回到浏览器中,以检查是否能够正确的提取url的数据
           return stock_code
   
   
   def app(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           for url, call_func in g_url_route.items():
               print(url)
               ret = re.match(url, file_name)
               if ret:
                   return call_func(file_name, url)
                   break
           else:
               return "没有访问的页面--->%s" % file_name
   
       except Exception as ret:
           return "%s" % ret
   
       else:
           return str(environ) + '-----404--->%s\n'
   ```

--------------------

#### 21.2 - mini-web框架-9-mysql增

1. my_web.py(修改), 相当于web框架

   ```
   import pymysql
   import time
   import os
   import re
   import sys
   from urllib.parse import unquote
   
   #存放目标
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   #装饰器函数(3层
   def route(url):   #参数是字符串类型
       def func1(func):   #参数是函数类型
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)  #原函数有默认参数, 现在只需要一个值
           return func2
       return func1
   
   
   @route(r"/index.html")
   def index(file_name, url=None):   #url是默认参数, 
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
                   </tr>"""
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/center.html")
   def center(file_name, url=None):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/update/(\d*)\.html")
   def update(file_name, url):
       """显示 更新页面的内容"""
       try:
           template_file_name = template_root + "/update.html"
           f = open(template_file_name)
       except Exception as ret:
           return "%s,,,没有找到%s" % (ret, template_file_name)
       else:
           content = f.read()
           f.close()
   
           ret = re.match(url, file_name)
           if ret:
               stock_code = ret.group(1)
           else:
               stock_code = 0
   
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           # 会出现sql注入，怎样修改呢？ 参数化
           sql = """select focus.note_info from focus inner join info on focus.info_id=info.id where info.code=%s;""" % stock_code
           cursor.execute(sql)
           stock_note_info = cursor.fetchone()
           cursor.close()
           db.close()
   
           content = re.sub(r"\{%code%\}", stock_code, content)
           content = re.sub(r"\{%note_info%\}", str(stock_note_info[0]), content)
   
           return content
   
   
   @route(r"/update/(\d*)/(.*)\.html")
   def update_note_info(file_name, url):
       """进行数据的真正更新"""
       stock_code = 0
       stock_note_info = ""
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
           stock_note_info = ret.group(2)
           stock_note_info = unquote(stock_note_info)
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
       # 会出现sql注入，怎样修改呢？ 参数化
       sql = """update focus inner join info on focus.info_id=info.id set focus.note_info="%s" where info.code=%s;""" % (stock_note_info, stock_code)
       cursor.execute(sql)
       db.commit()
       cursor.close()
       db.close()
   
       return "修改成功"
   
   
   # ------ 添加 -------
   @route(r"/add/(\d*)\.html")
   def add(file_name, url):
       """添加关注"""
   
       stock_code = 0
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
   
       # 判断是否已经关注
       sql = """select * from focus inner join info on focus.info_id=info.id where info.code=%s;""" % (stock_code)
       cursor.execute(sql)
       if cursor.fetchone():
           cursor.close()
           db.close()
           return "已经关注过了，请不要重复关注"
   
       # 如果没有关注，那么就进行关注
       sql = """insert into focus (info_id) select id from info where code="%s";""" % (stock_code)
       cursor.execute(sql)
       db.commit()
       cursor.close()
       db.close()
   
       return "关注成功"
   
   
   def app(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           for url, call_func in g_url_route.items():
               print(url)
               ret = re.match(url, file_name)
               if ret:
                   return call_func(file_name, url)
                   break
   
           else:
               return "没有访问的页面--->%s" % file_name
   
       except Exception as ret:
           return "%s" % ret
   
       else:
           return str(environ) + '-----404--->%s\n'
   ```

   1. SQL注入语句, 注入的字符串值自动加引号`""`或`''`

#### 21.3 - mini-web框架-10-mysql删

1. my_web.py(修改)

   ```
   import pymysql
   import time
   import os
   import re
   import sys
   from urllib.parse import unquote
   
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   
   @route(r"/index.html")
   def index(file_name, url=None):
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
                   </tr>"""
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/center.html")
   def center(file_name, url=None):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/update/(\d*)\.html")
   def update(file_name, url):
       """显示 更新页面的内容"""
       try:
           template_file_name = template_root + "/update.html"
           f = open(template_file_name)
       except Exception as ret:
           return "%s,,,没有找到%s" % (ret, template_file_name)
       else:
           content = f.read()
           f.close()
   
           ret = re.match(url, file_name)
           if ret:
               stock_code = ret.group(1)
           else:
               stock_code = 0
   
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           # 会出现sql注入，怎样修改呢？ 参数化
           #两个表中都有的信息中的一条
           sql = """select focus.note_info from focus inner join info on focus.info_id=info.id where info.code=%s;""" % stock_code
           cursor.execute(sql)
           stock_note_info = cursor.fetchone()
           cursor.close()
           db.close()
   
           content = re.sub(r"\{%code%\}", stock_code, content)
           content = re.sub(r"\{%note_info%\}", str(stock_note_info[0]), content)
   
           return content
   
   
   @route(r"/update/(\d*)/(.*)\.html")
   def update_note_info(file_name, url):
       """进行数据的真正更新"""
       stock_code = 0
       stock_note_info = ""  
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
           stock_note_info = ret.group(2)
           stock_note_info = unquote(stock_note_info)
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
       # 会出现sql注入，怎样修改呢？ 参数化
       sql = """update focus inner join info on focus.info_id=info.id set focus.note_info="%s" where info.code=%s;""" % (stock_note_info, stock_code)
       cursor.execute(sql)
       db.commit()   #commit后数据库数据才修改
       cursor.close()
       db.close()
   
       return "修改成功"
   
   
   @route(r"/add/(\d*)\.html")
   def add(file_name, url):
       """添加关注"""
   
       stock_code = 0
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
   
       # 判断是否已经关注
       sql = """select * from focus inner join info on focus.info_id=info.id where info.code=%s;""" % (stock_code)
       cursor.execute(sql)
       if cursor.fetchone():
           cursor.close()
           db.close()
           return "已经关注过了，请不要重复关注"
   
       # 如果没有关注，那么就进行关注
       #一旦执行, 数据表focus中增加了相应的数据行
       sql = """insert into focus (info_id) select id from info where code="%s";""" % (stock_code)
       cursor.execute(sql)
       db.commit()
       cursor.close()
       db.close()
   
       return "关注成功"
   
   
   # ------ 添加 -------
   @route(r"/del/(\d*)\.html")
   def delete(file_name, url):
       """取消关注"""
   
       stock_code = 0
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
   
       # 判断是否已经关注
       sql = """select * from focus inner join info on focus.info_id=info.id where info.code=%s;""" % (stock_code)
       cursor.execute(sql)
       if not cursor.fetchone():
           cursor.close()
           db.close()
           return "并没有关注，为什么要取消关注呢？不理解，啦啦啦。。。"
   
       # 如果有关注，那么就进行取消关注
       #符合条件的行记录一次从focus表删除
       sql = """delete from focus where info_id = (select id from info where code="%s");""" % (stock_code)
       cursor.execute(sql)
       db.commit()
       cursor.close()
       db.close()
   
       return "取消关注成功"
   
   
   def app(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           for url, call_func in g_url_route.items():
               print(url)
               ret = re.match(url, file_name)
               if ret:
                   return call_func(file_name, url)
                   break
   
           else:
               return "没有访问的页面--->%s" % file_name
   
       except Exception as ret:
           return "%s" % ret
   
       else:
           return str(environ) + '-----404--->%s\n'
   ```

   1. 小结: 对数据表focus的操作是真实的, 添加数据, 删除数据

#### 21.4 - mini-web框架-11-mysql改

1. my_web.py(修改)

   ```
   import pymysql
   import time
   import os
   import re
   import sys
   
   template_root = "./templates"
   
   # 用来存放 `url路由映射`, 即 url:处理url的函数
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   #route的作用: 浏览器请求的文件的url与处理url的函数形成 键值对 映射关系
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   
   @route(r"/index.html")
   def index(file_name, url=None):
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
                   </tr>"""
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/center.html")
   def center(file_name, url=None):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/update/(\d*)\.html")
   def update(file_name, url):
       """显示 更新页面的内容"""
       try:
           template_file_name = template_root + "/update.html"
           f = open(template_file_name)
       except Exception as ret:
           return "%s,,,没有找到%s" % (ret, template_file_name)
       else:
           content = f.read()
           f.close()
   
           ret = re.match(url, file_name)
           if ret:
               stock_code = ret.group(1)
           else:
               stock_code = 0
   
           # ------ 添加 -------
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           #sql注入， 修改 参数
           sql = """select focus.note_info from focus inner join info on focus.info_id=info.id where info.code=%s;""" % stock_code
           cursor.execute(sql)
           stock_note_info = cursor.fetchone()
           cursor.close()
           db.close()
   
           # ------ 修改 -------
           content = re.sub(r"\{%code%\}", stock_code, content)
           content = re.sub(r"\{%note_info%\}", str(stock_note_info[0]), content)
   
           return content
   
   
   # ------ 添加 -------
   @route(r"/update/(\d*)/(.*)\.html")
   def update_note_info(file_name, url):
       """进行数据的真正更新"""
       stock_code = 0
       stock_note_info = ""
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
           stock_note_info = ret.group(2)
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
       #sql注入
       sql = """update focus inner join info on focus.info_id=info.id set focus.note_info="%s" where info.code=%s;""" % (stock_note_info, stock_code)
       cursor.execute(sql)
       db.commit()
       cursor.close()
       db.close()
   
       return "修改成功"
   
   
   def app(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           for url, call_func in g_url_route.items():
               print(url)
               ret = re.match(url, file_name)
               if ret:
                   return call_func(file_name, url)
                   break
   
           else:
               return "没有访问的页面--->%s" % file_name
   
       except Exception as ret:
           return "%s" % ret
   
       else:
           return str(environ) + '-----404--->%s\n'
   ```

#### 21.5 - mini-web框架-12-url编码

1. python3对url编解码

   ```
   import urllib.parse
   # Python3 url编码
   print(urllib.parse.quote("天安门"))
   # Python3 url解码
   print(urllib.parse.unquote("%E5%A4%A9%E5%AE%89%E9%97%A8"))
   ```

2. my_web.py(修改)

   ```
   mport pymysql
   import time
   import os
   import re
   import sys
   # ------- 添加 --------
   from urllib.parse import unquote
   
   template_root = "./templates"
   
   # 用来存放url路由映射
   # url_route = {
   #   "/index.py":index_func,
   #   "/center.py":center_func
   # }
   g_url_route = dict()
   
   
   def route(url):
       def func1(func):
           # 添加键值对，key是需要访问的url，value是当这个url需要访问的时候，需要调用的函数引用
           g_url_route[url]=func
           def func2(file_name):
               return func(file_name)
           return func2
       return func1
   
   
   @route(r"/index.html")
   def index(file_name, url=None):
       """返回index.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据，请等待学习mysql吧，学习完mysql之后，这里就可以放入mysql查询到的数据了"
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select * from info;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%d</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <input type="button" value="添加" id="toAdd" name="toAdd" systemidvaule="%s">
                   </td>
                   </tr>"""
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[7], info[1])
   
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/center.html")
   def center(file_name, url=None):
       """返回center.py需要的页面内容"""
       # return "hahha" + os.getcwd()  # for test 路径问题
       try:
           file_name = file_name.replace(".py", ".html")
           f = open(template_root + file_name)
       except Exception as ret:
           return "%s" % ret
       else:
           content = f.read()
           f.close()
   
           # data_from_mysql = "暂时没有数据,,,,~~~~(>_<)~~~~ "
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           sql = """select i.code,i.short,i.chg,i.turnover,i.price,i.highs,j.note_info from info as i inner join focus as j on i.id=j.info_id;"""
           cursor.execute(sql)
           data_from_mysql = cursor.fetchall()
           cursor.close()
           db.close()
   
           html_template = """
               <tr>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>%s</td>
                   <td>
                       <a type="button" class="btn btn-default btn-xs" href="/update/%s.html"> <span class="glyphicon glyphicon-star" aria-hidden="true"></span> 修改 </a>
                   </td>
                   <td>
                       <input type="button" value="删除" id="toDel" name="toDel" systemidvaule="%s">
                   </td>
               </tr>
               """
   
           html = ""
   
           for info in data_from_mysql:
               html += html_template % (info[0], info[1], info[2], info[3], info[4], info[5], info[6], info[0], info[0])
   
           content = re.sub(r"\{%content%\}", html, content)
   
           return content
   
   
   @route(r"/update/(\d*)\.html")
   def update(file_name, url):
       """显示 更新页面的内容"""
       try:
           template_file_name = template_root + "/update.html"
           f = open(template_file_name)
       except Exception as ret:
           return "%s,,,没有找到%s" % (ret, template_file_name)
       else:
           content = f.read()
           f.close()
   
           ret = re.match(url, file_name)
           if ret:
               stock_code = ret.group(1)
           else:
               stock_code = 0
   
           db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
           cursor = db.cursor()
           # 会出现sql注入，怎样修改呢？ 参数化
           sql = """select focus.note_info from focus inner join info on focus.info_id=info.id where info.code=%s;""" % stock_code
           cursor.execute(sql)
           stock_note_info = cursor.fetchone()
           cursor.close()
           db.close()
   
           content = re.sub(r"\{%code%\}", stock_code, content)
           content = re.sub(r"\{%note_info%\}", str(stock_note_info[0]), content)
   
           return content
   
   
   @route(r"/update/(\d*)/(.*)\.html")
   def update_note_info(file_name, url):
       """进行数据的真正更新"""
       stock_code = 0
       stock_note_info = ""
   
       ret = re.match(url, file_name)
       if ret:
           stock_code = ret.group(1)
           stock_note_info = ret.group(2)
           stock_note_info = unquote(stock_note_info)  # ------ 添加 -------
   
       db = pymysql.connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cursor = db.cursor()
       #sql注入
       sql = """update focus inner join info on focus.info_id=info.id set focus.note_info="%s" where info.code=%s;""" % (stock_note_info, stock_code)
       cursor.execute(sql)
       db.commit()
       cursor.close()
       db.close()
   
       return "修改成功"
   
   #application中调用 url对应的处理函数, 调用wsgi服务器中的 设置响应头的函数
   def app(environ, start_response):
       status = '200 OK'
       response_headers = [('Content-Type', 'text/html')]
       start_response(status, response_headers)
   
       file_name = environ['PATH_INFO']
       try:
           for url, call_func in g_url_route.items():
               print(url)
               ret = re.match(url, file_name)
               if ret:
                   return call_func(file_name, url)
                   break
   
           else:
               return "没有访问的页面--->%s" % file_name
   
       except Exception as ret:
           return "%s" % ret
   
       else:
           return str(environ) + '-----404--->%s\n'
   ```

#### 21.6 - logging日志模块

* 输出方式
  * print() 输出方式
  * log 输出方式:  控制台, log日志

1. 日志5个等级，从低到高

   1. DEBUG  #出现在诊断问题
   2. INFO  #确认一切按预期运行
   3. WARNING  #警告
   4. ERROR   #严重的问题
   5. CRITICAL  #严重的错误

2. 日志输出

   * 日志两种输出，一 输出 `控制台`，二 输出到 `日志文件`

   1. 将日志输出到控制台

      1. log1.py

         ```
         import logging  
         
         logging.basicConfig(level=logging.WARNING,  
                             format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s')  
         
         # 开始使用log功能
         logging.info('这是 loggging info message')  
         logging.debug('这是 loggging debug message')  
         logging.warning('这是 loggging a warning message')  
         logging.error('这是 an loggging error message')  
         logging.critical('这是 loggging critical message')
         ```

         运行结果:

         ```
         2017-11-06 23:07:35,725 - log1.py[line:9] - WARNING: 这是 loggging a warning message
         2017-11-06 23:07:35,725 - log1.py[line:10] - ERROR: 这是 an loggging error message
         2017-11-06 23:07:35,725 - log1.py[line:11] - CRITICAL: 这是 loggging critical message
         ```

      2. logging.basicConfig(level, format)  #level 级别, format 格式字符串

         1. 输出格式 在网上查找

   2. 日志输出到文件

      * `logging.basicConfig(level, filename, filemode, format) ` 

        `logging.basicConfig` 函数中设置 输出到 文件的`文件名`和 写文件的`模式`

        log2.py

        ```
        import logging  
        
        logging.basicConfig(level=logging.WARNING,  
                            filename='./log.txt',  
                            filemode='w',  
                            format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s')  
        # use logging  
        logging.info('这是 loggging info message')  
        logging.debug('这是 loggging debug message')  
        logging.warning('这是 loggging a warning message')  
        logging.error('这是 an loggging error message')  
        logging.critical('这是 loggging critical message')
        ```

        输出结果

        ```
        python@ubuntu: cat log.txt 
        2017-11-06 23:10:44,549 - log2.py[line:10] - WARNING: 这是 loggging a warning message
        2017-11-06 23:10:44,549 - log2.py[line:11] - ERROR: 这是 an loggging error message
        2017-11-06 23:10:44,549 - log2.py[line:12] - CRITICAL: 这是 loggging critical message
        ```

   3. 日志输出到 控制台  同时写入 日志文件

      * `Logger对象` 帮忙

      * ```
        import logging  
        
        # 第一步，创建一个logger  
        logger = logging.getLogger()  
        logger.setLevel(logging.INFO)  # Log等级总开关  
        
        # 第二步，创建一个handler，用于 写入日志文件  
        logfile = './log.txt'  
        fh = logging.FileHandler(logfile, mode='a')  # open的打开模式这里可以进行参考
        fh.setLevel(logging.DEBUG)  # 输出到file的log等级的开关  
        
        # 第三步，再创建一个handler，用于 输出到控制台  
        ch = logging.StreamHandler()  
        ch.setLevel(logging.WARNING)   # 输出到console的log等级的开关  
        
        # 第四步，定义handler的输出格式  
        formatter = logging.Formatter("%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s")  
        fh.setFormatter(formatter)  
        ch.setFormatter(formatter)  
        
        # 第五步，将logger添加到handler里面  
        logger.addHandler(fh)  
        logger.addHandler(ch)  
        
        # 日志  
        logger.debug('这是 logger debug message')  
        logger.info('这是 logger info message')  
        logger.warning('这是 logger warning message')  
        logger.error('这是 logger error message')  
        logger.critical('这是 logger critical message')
        ```

   4. 日志格式说明

      1. logging.basicConfig函数, 参数 `format` 指定 输出格式

         1. %(levelno)s: 打印日志级别的数值
         2. %(levelname)s: 打印日志级别名称
         3. %(pathname)s: 打印当前执行程序的路径，其实就是sys.argv[0]
         4. %(filename)s: 打印当前执行程序名
         5. %(funcName)s: 打印日志的当前函数
         6. %(lineno)d: 打印日志的当前行号
         7. %(asctime)s: 打印日志的时间
         8. %(thread)d: 打印线程ID
         9. %(threadName)s: 打印线程名称
         10. %(process)d: 打印进程ID
         11. %(message)s: 打印日志信息

      2. 常用格式

         `format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s'`

         日志的打印时间，哪个模块输出的，日志级别，输入的日志内容



-------------

### 05-mini-web框架添加正则和log日志功能

###### 视频版本

#### 01-今日课程介绍.flv

#### 02-股票列表.flv

1. 路由添加正则

   ```
   #实现路由的功能: url与方法一一配对
   def route(url):
       def set_func(func):
           # URL_FUNC_DICT["/index.py"] = index
           URL_FUNC_DICT[url] = func
           def call_func(*args, **kwargs):
               return func(*args, **kwargs)
           return call_func
       return set_func
       
   @route(r"/index.html")
   def index():
   	....
   	
   
   @route(r"/center.html")
   def center():
   
   # 路由添加正则表达式的原因：在实际开发时，url带有很多参数，例如/add/000007.html中000007就是参数，
   # 如果没有正则的话，需要编写N次route,添加 url对应的函数 到字典中，此时字典中的键值对有N个，浪费空间
   # 而采用了正则的话，那么只要编写1次route就可以完成多个 url例如/add/00007.html /add/000036.html等对应同一个函数，此时字典中的键值对个数会少很多
   @route(r"/add/\d+\.html")   # 路由添加正则
   def add_focus():
       return "add  ok ...."
   ```

#### 02- 股票列表

1. 正则路由 可以 匹配很多请求

   ````
   URL_FUNC_DICT = dict()
   
   
   def route(url):
       def set_func(func):
           # URL_FUNC_DICT["/index.py"] = index
           URL_FUNC_DICT[url] = func
           def call_func(*args, **kwargs):
               return func(*args, **kwargs)
           return call_func
       return set_func
   
   
   @route(r"/index.html")   # 添加 URL_FUNC_DICT["/index.html"] = index
   def index(ret):
   	......
   
   
   @route(r"/center.html")   # 添加 URL_FUNC_DICT["/center.html"] = center
   def center(ret):
   	......
   
   
   @route(r"/add/(\d+)\.html")  # 添加 URL_FUNC_DICT["/add/(\d+)\.html"] = add_focus
   def add_focus(ret):
   	......
   
   
   def application(env, start_response):
       start_response('200 OK', [('Content-Type', 'text/html;charset=utf-8')])
       
       file_name = env['PATH_INFO']
       # file_name = "/index.py"
   
       """
       if file_name == "/index.py":
           return index()
       elif file_name == "/center.py":
           return center()
       else:
           return 'Hello World! 我爱你中国....'
       """
   
       try:
           # func = URL_FUNC_DICT[file_name]
           # return func()
           # return URL_FUNC_DICT[file_name]()
           #URL_FUNC_DICT是字典, url是键, fun是值(函数引用)
           for url, func in URL_FUNC_DICT.items():
               # {
               #   r"/index.html":index,
               #   r"/center.html":center,
               #   r"/add/\d+\.html":add_focus
               # }
               # /add/(\d+)\.html 正则url 可以匹配很多 file_name
               ret = re.match(url, file_name)
               if ret:
                   return func(ret)
           else:
               return "请求的url(%s)没有对应的函数...." % file_name
   
   
       except Exception as ret:
           return "产生了异常：%s" % str(ret)
   ````

#### 03-关注股票.flv

1. sql两个表联合查找

1. ```
   @route(r"/add/(\d+)\.html")  #?? 添加 URL_FUNC_DICT["/index.py"] = add_focus??
   def add_focus(ret):
   
       # 1. 获取股票代码
       stock_code = ret.group(1)
   
       # 2. 判断试下是否有这个股票代码
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cs = conn.cursor()
       sql = """select * from info where code=%s;"""
       cs.execute(sql, (stock_code,))
       # 如果要是没有这个股票代码，那么就认为是非法的请求
       if not cs.fetchone():
           cs.close()
           conn.close()
           return "没有这支股票，大哥 ，我们是创业公司，请手下留情..."
   
       # 3. 判断以下是否已经关注过
       sql = """ select * from info as i inner join focus as f on i.id=f.info_id where i.code=%s;"""
       cs.execute(sql, (stock_code,))
       # 如果查出来了，那么表示已经关注过
       if cs.fetchone():
           cs.close()
           conn.close()
           return "已经关注过了，请勿重复关注..."
   
       # 4. 添加关注
       sql = """insert into focus (info_id) select id from info where code=%s;"""
       cs.execute(sql, (stock_code,))
       conn.commit()
       cs.close()
       conn.close()
   
       return "关注成功...."
   ```

   1.  sql 注入  cs.execute(sql, (stock_code,))   #对注入的字符串自动加引号.

   2.  python mysql自动开启事务

      ```
      cs.execute("sql语句", (注入的数据,))
      cs.commit()
      cs.close()
      conn.close()
      ```

-------------

#### 04-取消关注.flv

1. sql 两个表联合 删除

2. ```
   @route(r"/del/(\d+)\.html")
   def del_focus(ret):
   
       # 1. 获取股票代码
       stock_code = ret.group(1)
   
       # 2. 判断试下是否有这个股票代码
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cs = conn.cursor()
       sql = """select * from info where code=%s;"""
       cs.execute(sql, (stock_code,))
       # 如果要是没有这个股票代码，那么就认为是非法的请求
       if not cs.fetchone():
           cs.close()
           conn.close()
           return "没有这支股票，大哥 ，我们是创业公司，请手下留情..."
   
       # 3. 判断以下是否已经关注过
       sql = """ select * from info as i inner join focus as f on i.id=f.info_id where i.code=%s;"""
       cs.execute(sql, (stock_code,))
       # 如果没有关注过，那么表示非法的请求
       if not cs.fetchone():
           cs.close()
           conn.close()
           return "%s 之前未关注，请勿取消关注..." % stock_code
   
       # 4. 取消关注
       sql = """delete from focus where info_id = (select id from info where code=%s);"""
       cs.execute(sql, (stock_code,))
       conn.commit()
       cs.close()
       conn.close()
   
       return "取消关注成功...."
   ```

   

#### 05-更新备注信息.flv

1. 程序员网站:  博客园, csdn, 51cto, 开源中国, github, 知乎, 简书

2. 显示,  保存是分开的

   ```
   # 显示
   @route(r"/update/(\d+)\.html")
   def show_update_page(ret):
       """显示修改的那个页面"""
       # 1. 获取股票代码
       stock_code = ret.group(1)
   
       # 2. 打开模板
       with open("./templates/update.html") as f:
           content = f.read()
   
       # 3. 根据股票代码查询相关的备注信息
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cs = conn.cursor()
       sql = """select f.note_info from focus as f inner join info as i on i.id=f.info_id where i.code=%s;"""
       cs.execute(sql, (stock_code,))
       stock_infos = cs.fetchone()
       note_info = stock_infos[0]  # 获取这个股票对应的备注信息
       cs.close()
       conn.close()
   
       content = re.sub(r"\{%note_info%\}", note_info, content)
       content = re.sub(r"\{%code%\}", stock_code, content)
   
       return content
   ```

   ```
   # 保存
   @route(r"/update/(\d+)/(.*)\.html")
   def save_update_page(ret):
       """"保存修改的信息"""
       stock_code = ret.group(1)
       comment = ret.group(2)
       
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='stock_db',charset='utf8')
       cs = conn.cursor()
       sql = """update focus set note_info=%s where info_id = (select id from info where code=%s);"""
       cs.execute(sql, (comment, stock_code))
       conn.commit()
       cs.close()
       conn.close()
   
       return "修改成功..."
   ```

   

#### 06-url编解码.flv

1. url编码, 保证服务器解析url不出错

   ```
   import urllib.parse
   # Python3 url编码
   print(urllib.parse.quote("天安门"))
   # Python3 url解码
   print(urllib.parse.unquote("%E5%A4%A9%E5%AE%89%E9%97%A8"))
   ```

2. ```
   # mini_frame.py
   
   @route(r"/update/(\d+)/(.*)\.html")
   def save_update_page(ret):
       """"保存修改的信息"""
       stock_code = ret.group(1)
       comment = ret.group(2)
       comment = urllib.parse.unquote(comment)
   ```

   1. urllib.parse.quote()  # 字符串参数
   2. urllib.parse.unquote() # 编码后的字符串参数



------------

#### 07-log日志功能.flv

1. 两种方式记录

   1. 控制台记录方式， 文件记录方式，如日志文件
   2. logging 同时写文件, 同时

2. ```
   import logging  
   
   # 设置logging 格式
   logging.basicConfig(level=logging.WARNING,  
                       format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s')  
   
   # 开始使用log功能
   logging.info('这是 loggging info message')  
   logging.debug('这是 loggging debug message')  
   logging.warning('这是 loggging a warning message')  
   logging.error('这是 an loggging error message')  
   logging.critical('这是 loggging critical message')
   ```

3. logging 输入 文件, 控制台

   ```
   import logging  
   
   # 第一步，创建一个logger  
   logger = logging.getLogger()  
   logger.setLevel(logging.INFO)  # Log等级总开关  
   
   # 第二步，创建一个handler，用于写入日志文件  
   logfile = './log.txt'  
   fh = logging.FileHandler(logfile, mode='a')  # open的打开模式这里可以进行参考
   fh.setLevel(logging.DEBUG)  # 输出到file的log等级的开关  
   
   # 第三步，再创建一个handler，用于输出到控制台  
   ch = logging.StreamHandler()  
   ch.setLevel(logging.WARNING)   # 输出到console的log等级的开关  
   
   # 第四步，定义handler的输出格式  
   formatter = logging.Formatter("%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s")  
   fh.setFormatter(formatter)  
   ch.setFormatter(formatter)  
   
   # 第五步，将logger添加到handler里面  
   logger.addHandler(fh)  
   logger.addHandler(ch)  
   
   # 日志  
   logger.debug('这是 logger debug message')  
   logger.info('这是 logger info message')  
   logger.warning('这是 logger warning message')  
   logger.error('这是 logger error message')  
   logger.critical('这是 logger critical message')
   ```

4. 代码中logging

   ```
   import logging
   
   def application(env, start_response):
   	......
       logging.basicConfig(level=logging.INFO,  
                           filename='./log.txt',  
                           filemode='a',  
                           format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s')  
   
       logging.info("访问的是，%s" % file_name)
   ```

---------------------

