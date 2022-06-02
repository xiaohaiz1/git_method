#### jsonp

1. JSONP的客户端具体实现：

   1.    远程服务器remoteserver.com根目录下有个remote.js文件代码如下：

      1. `alert('我是远程文件');`

   2. 本地服务器localserver.com下有个jsonp.html页面代码如下：

      1. ```
         <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
         <html xmlns="http://www.w3.org/1999/xhtml">
         <head>
             <title></title>
             <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
         </head>
         <body>
          
         </body>
         </html>
         ```

         						1.  毫无疑问，页面将会弹出一个提示窗体，显示跨域调用成功。

   3. 现在我们在jsonp.html页面定义一个函数，然后在远程remote.js中传入数据进行调用。

      1.  jsonp.html页面代码如下：

         ```
         <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
         <html xmlns="http://www.w3.org/1999/xhtml">
         <head>
             <title></title>
             <script type="text/javascript">
             var localHandler = function(data){
                 alert('我是本地函数，可以被跨域的remote.js文件调用，远程js带来的数据是：' + data.result);
             };
             </script>
             <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
         </head>
         <body>
          
         </body>
         </html>
         ```

      2.  remote.js文件代码如下：

         ```
         localHandler({"result":"我是远程js带来的数据"});
         ```

         运行之后查看结果，页面成功弹出提示窗口，显示本地函数被跨域的远程js调用成功，并且还接收到了远程js带来的数据。 
         很欣喜，跨域远程获取数据的目的基本实现了，但是又一个问题出现了，我怎么让远程js知道它应该调用的本地函数叫什么名字呢？毕竟是jsonp的服务者都要面对很多服务对象，而这些服务对象各自的本地函数都不相同啊？我们接着往下看。

   4. 聪明的开发者很容易想到，只要服务端提供的js脚本是动态生成的就行了呗，这样调用者可以传一个参数过去告诉服务端 “我想要一段调用XXX函数的js代码，请你返回给我”，于是服务器就可以按照客户端的需求来生成js脚本并响应了。

      1. jsonp.html页面的代码：

         ```
         <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
         <html xmlns="http://www.w3.org/1999/xhtml">
         <head>
             <title></title>
             <script type="text/javascript">
             // 得到航班信息查询结果后的回调函数
             var flightHandler = function(data){
                 alert('你查询的航班结果是：票价 ' + data.price + ' 元，' + '余票 ' + data.tickets + ' 张。');
             };
             // 提供jsonp服务的url地址（不管是什么类型的地址，最终生成的返回值都是一段javascript代码）
             var url = "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998&callback=flightHandler";
             // 创建script标签，设置其属性
             var script = document.createElement('script');
             script.setAttribute('src', url);
             // 把script标签加入head，此时调用开始
             document.getElementsByTagName('head')[0].appendChild(script); 
             </script>
         </head>
         <body>
          
         </body>
         </html>
         ```

          这次的代码变化比较大，不再直接把远程js文件写死，而是编码实现动态查询，而这也正是jsonp客户端实现的核心部分，本例中的重点也就在于如何完成jsonp调用的全过程。 
              我们看到调用的url中传递了一个code参数，告诉服务器我要查的是CA1998次航班的信息，而callback参数则告诉服务器，我的本地回调函数叫做flightHandler，所以请把查询结果传入这个函数中进行调用。 
              OK，服务器很聪明，这个叫做flightResult.aspx的页面生成了一段这样的代码提供给jsonp.html

            （服务端的实现这里就不演示了，与你选用的语言无关，说到底就是拼接字符串）：

      2. 到这里为止的话，相信你已经能够理解jsonp的客户端实现原理了吧？剩下的就是如何把代码封装一下，以便于与用户界面交互，从而实现多次和重复调用

         jQuery如何实现jsonp调用？

         ```
         <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
         <html xmlns="http://www.w3.org/1999/xhtml" >
         <head>
              <title>Untitled Page</title>
               <script type="text/javascript" src=jquery.min.js"></script>
               <script type="text/javascript">
              jQuery(document).ready(function(){ 
                 $.ajax({
                      type: "get",
                      async: false,
                      url: "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998",
                      dataType: "jsonp",
                      jsonp: "callback",//传递给请求处理程序或页面的，用以获得jsonp回调函数名(一般默认为:callback)
                      jsonpCallback:"flightHandler",//自定义的jsonp回调函数名称，默认为jQuery自动生成的随机函数名，也可以写"?"，jQuery会自动为你处理数据
                      success: function(json){
                          alert('您查询到航班信息：票价： ' + json.price + ' 元，余票： ' + json.tickets + ' 张。');
                      },
                      error: function(){
                          alert('fail');
                      }
                  });
              });
              </script>
              </head>
           <body>
           </body>
         </html>
         ```

         是不是有点奇怪？为什么我这次没有写flightHandler这个函数呢？而且竟然也运行成功了！ 
            这就是jQuery的功劳了，jquery在处理jsonp类型的ajax时（，虽然jquery也把jsonp归入了ajax，但其实它们真的不是一回事儿），自动帮你生成回调函数并把数据取出来供success属性方法来调用，是不是很爽呀？ 

          

         **这里针对ajax与jsonp的异同再做一些补充说明：**

         1、ajax和jsonp这两种技术在调用方式上”看起来”很像，目的也一样，都是请求一个url，然后把服务器返回的数据进行处理，因此jquery和ext等框架都把jsonp作为ajax的一种形式进行了封装。

         2、但ajax和jsonp其实本质上是不同的东西。ajax的核心是通过XmlHttpRequest获取非本页内容，而jsonp的核心则是动态添加

          

         总结

            JSONP就是通过加载远程js文件，远程的js文件中写有调用当前域中已经写好的js方法的语句，并且这个语句中包含需要传入的数据，这样这个数据就传入到了当前域。用一句话概括就是：*\*通过js文件携带数据实现了数据的跨域访问\**。

----------------

#### jsonp原理详解

JSONP即JSON with Padding。由于同源策略的限制，XmlHttpRequest只允许请求当前源（域名、协议、端口）的资源。如果要进行跨域请求， 我们可以通过使用html的script标记来进行跨域请求，并在响应中返回要执行的script代码，其中可以直接使用JSON传递javascript对象。 这种跨域的通讯方式称为JSONP。

jsonp的原理：

```
 $.ajax({  
        async:false,  
        url: http://跨域的dns/document!searchJSONResult.action,  
        type: "GET",  
        dataType: 'jsonp',  
        jsonp: 'callback', 
        ...
 }
```

1.首先在客户端注册一个callback。（jsonp相当于自己再js中加了个script标签）。
//function jquery123456(…){…};
//< srcipt src=“[http://跨域的dns/document!searchJSONResult.action?callback=jquery123456&a=1](http://xn--dns-ib7er66kql3a/document!searchJSONResult.action?callback=jquery123456&a=1)…”/>

2.然后把callback的名字传给服务器。
//http://跨域的dns/document!searchJSONResult.action?callback=jquery123456&a=1…

3.此时，服务器先生成 json 数据。 然后以 javascript 语法的方式，生成一个function , function 名字就是传递上来的参数 jsonp。最后将 json 数据直接以入参的方式，放置到 function 中，这样就生成了一段 js 语法的文档，返回给客户端。
//String result = callback+"("+json+")"; 即 jquery123456(json数据);
//return result;

4.客户端浏览器，解析script标签，并执行返回的 javascript 文档，此时数据作为参数，传入到了客户端预先定义好的 callback 函数里。（动态执行回调函数）
// 获得 jquery123456(json数据);执行回调函数。

//jquery在处理jsonp类型的ajax时（，虽然jquery也把jsonp归入了ajax，但其实它们真的不是一回事儿），
//自动帮你生成回调函数并把数据取出来供success属性方法来调用。

jsonp前后台格式：
前台：

```
$.ajax(     
	type:"GET",           
	url:"http://www.deardull.com:9090/getMySeat", //访问的链接     
	dataType:"jsonp",  //数据格式设置为jsonp        
	jsonp:"callback",  //Jquery生成验证参数的名称.即链接左边的名称
    jsonpCallback: "showData",  //指定回调函数名称，即链接右边的名称，可不写随机生成。  
	success:function(data){  //成功的回调函数        
		alert(data);    
	},            
	error: function (e) {
		alert("error");
	}       
});
```

后台：

```
@ResponseBody
    
@RequestMapping("/getMySeat")
    
public String getMySeatSuccess(@RequestParam("callback") String callback，@RequestParam("name") String name){
        
	Gson gson=new Gson();
        
	Map<String,String> map=new HashMap<>();
        
	map.put("seat","1_2_06_12");
        
	logger.info(callback);
        
	return callback+"("+gson.toJson(map)+")";
    
}
```



----------



#### 模拟百度智能匹配功能

1. 首先搭建页面

   1. ![](I:/0 全程 - 学习笔记/images/tj.png)

2. 获取百度搜索请求的地址 : 打开调试工具，选network, 并在输入框键入内容

   1. ![](I:/0 全程 - 学习笔记/images/dz.png)

      1. 点击箭头指向获取其请求的url,  该url的status 是 200

         

   2. 选择该url,  查看其Headers

      1. ![](I:/0 全程 - 学习笔记/images/ull.png)
      2. RequestUrl就是其请求的地址
      3. 提取有用的参数:  `wd`对应搜索的值，`cb`对应回调函数的名称

3. jsonp的jQuery实现

   1. ```
      <script type="text/javascript">
      $(function(){
      		$('#input').keyup(function(){
      			var word = $(this).val();
      			$.ajax({
      				url:'https://www.baidu.com/sugrec?pre=1&p=3&ie=utf-8&json=1&prod=pc',
      				dataType:'jsonp',
      				jsonp:'cb',
      				data:{wd:word},
      				success:function(msg){
      					// console.log(msg.g)
      					var data = msg.g;
      					var html =  '<ul>';
      					$.each(data,function(i,e){
      						// console.log(e.q)e.q就是文本数据
      						html += "<li>"+e.q+"</li>";
      					})
      					html+='</ul>';
      					$('.data').html(html);
      				}
      			})
      		})
      	})
      </script>
      
      <body>
      	<input type="button" value="点击">
      	<input type="text" id="input">
      </body>
      ```

      1. 并没有在url后面加wd,cb参数, 因为jq发送jsonp时会自动补上，我们只需设置相关属性即可