##### 参考网站 :

https://blog.csdn.net/rao_limon/category_9286428.html



## 总结:

1.  html,css, js是从页面的第一行开始执行, 语句在上面与在下面的执行结果不同

2.  怎么从html语句向js语句中传参数

3.  js:  href前没有style,  对象名.href

4.  src 和 href 
    1.  **href**(Hypertext Reference超文本引用)属性, 指定Web资源的位置，定义当前元素或当前文档与目标资源之间的链接或关系
        1.  超文本引用， 在link和a等元素上，href是引用和页面关联， 当前元素和引用资源之间建立联系

    2.  **src**(Source)属性, 将资源嵌入到当前文档中元素所在位置
        1.  引用资源， 替换当前元素， 在img，script，iframe上，src是页面内容不可缺少的一部分

    3.  src和href的区别: 
        1.  src 将资源嵌入到当前文档中. `<iframe src="https://www.baidu.com"></iframe>` 将百度嵌入到当前页
        2.  href 建立一个关联指向资源. `<a href="https://www.baidu.com"></a>` 不会将百度嵌入到当前页面中

5.  html中向js传参数

    1.  ```
        <head>
            <script type="text/javascript">
        
          function trans(Y){
            console.log(Y);
            alert(Y);
          }
        </script>
        </head>
        <body>
        <button type="button" onclick='trans("X")'>传参</button>
        </body>
        ```

    2.  ```
        <head>
            <meta charset="UTF-8">
            <title>Title</title>
        </head>
        <script language="javascript" type="text/javascript">
            function a(callback)
            {
                alert("我是parent函数a！");
                alert("调用回调函数");
                callback();
            }
            function b(){
                alert("我是回调函数b");
        
            }
            function c(){
                alert("我是回调函数c");
        
            }
        
            function test()
            {
                a(b);
                a(c);
            }
        
        </script>
        <body>
        <h1>学习js回调函数</h1>
        <button onClick=test()>click me</button>
        <p>应该能看到调用了两个回调函数</p>
        </body>
        </html>
        ```

    3.  



## 01-JavaScript变量和操作元素

#### 01 js介绍-js引入页面

1. js介绍

   1.  前端3大块之一: HTML(结构), CSS(表现), javaScript(行为)
   2. jQuery 是 js一个库
   3. 分为 : 原生js, 和 js库  
   4. 浏览器解释执行的. 不用编译
   5. 其他前端语言 : 知道就好 
      1. JScript (微软独有)
      2. ActionScript( flash的 语言)
   6. vue是一个构建用户界面的框架(库),它的目标是通过尽可能简单的api实现响应的数据绑定和组合的视图集合, 尤雨溪

2. 实现方式: 嵌入页面

   1. 实例

      4. ```
         <head>
             <meta charset="UTF-8">
             
             <!-- 方式3: head中的script标签中导入js文件 -->
             <script type="text/javascript" src="js/hellom.js"></script>
             
             <!-- 方式2: head嵌入script脚本, 脚本中嵌入事件 -->
             <script type="text/javascript">
                 alert("hello 2");  /*系统自带的函数*/
             </script>
         </head>
         
         <body>
             <!-- 方式1: body行中嵌入事件 -->
             <input type="button" name="" value="点击" onclick="alert('hello world!')">
         </body>
         ```

         1. hello.js
                   1. `alert("hello 3")`

3. js变量和类型

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript">
      
              // 单行注释
      
              /*
                  多行注释
                  下面将两个变量申明合成一句
              */
              
              //var iNum = 12;
              //var sTr = 'abc';
      
      
              var iNum = 12,sTr='abc';
              var iNum02;
      
              alert(iNum);
      
              //alert(sTr);
              // alert(iNum02);   未定义,显示undefined
      
          </script>
      </head>
      ```

   2. 注释 `//,  /*...*/`

   3. 变量 var

4. js获取元素

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		window.onload = function(){
      			/*
      			document.getElementById('div1').style.color = 'red';
      			document.getElementById('div1').style.fontSize = '30px'			
      			用变量简化代码：
      			*/
      
      			var oDiv = document.getElementById('div1');
      
      			oDiv.style.color = 'red';
      			oDiv.style.fontSize = '30px';
      		}
      	</script>
      </head>
      <body>
      	<div id="div1">这是一个div元素</div>
      </body>


#### 02 js变量类型-获取元素

1. 弱类型. 

   1. 类型由值 定

2. 变量

   1. var, 关键字

   2. 命名规范

      1. 区分大小写 , 
      2. 第一个字符: 字母, _, $
      3. 其他字符:  字母, _, $ , 数组
      4. 匈牙利风格

   3. | o      | a      | s         | i          | b           | f      | fn        | re           |
      | ------ | ------ | --------- | ---------- | ----------- | ------ | --------- | ------------ |
      | Object | Array  | String    | Integer    | Boolean     | Float  | Function  | RegExp       |
      | oDiv   | aItems | sUserName | iItemCount | blsComplete | fPrice | fnHandler | reEmailCheck |

      

3. 变量类型

   1.  基本类型(小写)
      1. number
      2. string
      3. boolean:  true, false
      4. undefined,  变量声明未初始化, 值是undefined
      5. null, 空对象, 初始化值 null, 将来保存对象
   2. 复合类型
      1. object

4. 语句

   1.  `;` 结尾  与css一样, 与c语音一样

   2. ```
      // 单行注释
      /* 多行注释 */

5.  获取元素

   1.  js 操作 html元素标签

   2. ```
      <!DCOTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	
      	<script type="text/javascript">
      		//当整个页面加载完之后再执行花括号里面的语句
      		window.onload = function(){
      			// 通过ID名获取元素赋值给oDiv变量
                  var oDiv = document.getElementByid('div1')            
                  oDiv.style.color='red';  // = 等号设置属性值
                  oDiv.style.fontSize='30px';
      		}
      	</script>
      </head>
      <body>
      	<div id="div1">这是一个div</div>
      </body>
      </html>
      ```
      
        1. document , 是window对象的一个子对象，能访问页面中所有元素的对象
              2. window.onload  ,  网页加载完毕后立刻执行的等号后的操作
               1. window.onload是一个事件，文档加载完成后能立即触发，能为该事件注册事件处理函数。将对象或模块操作的代码存放在处理函数中。即：window.onload =function (){这里写操作的代码};
                  3. font-size 属性 变形 为 fontSize,  - 去掉, 小写s变大写S
                      4. 内置对象: document , window

   3. 总结:

      1.  html 从上往下 执行.
      2.  js位置
          1.  放入 `<head> </head>`
          2.  放入 `HTML`的body行中
          3.  `<scrapy></scrapy>`  独立, 与head, body标签并列

#### 03 复习

1.  浏览器 文字 最小 12px

2. H5页面的应用

   1.  手机端网页
   2. 微信页面
   3. App : H5页面 套个android 或 ios的壳

3. 浏览器前缀

   1. ```
      -webkit-transform:rotate(30deg);
      ```

#### 04 元素属性读写

1. 属性值读写 用 =

   1. html属性  js属性  单词一样, 但横杠  改成驼峰式

      1. 如：style=“font-size:5px”-->改成style.fontSize=5px

   2. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		
      		window.onload = function(){
      			var oDiv = document.getElementById('div1');
      			var oA = document.getElementById('link');
      			var oDiv2 = document.getElementById('div2');
      
      			// 读取属性
      			var sId = oDiv.id
      			alert(sId);
      			
      			// 写属性, 对象.style.属性=值; link对象名.属性=值;
      			oDiv.style.color = "red";    //style属性
      			oA.href = "http://www.baidu.com";
      			oA.title = "这是去到百度网的链接";
      
      			// 操作class属性需要写成 对象名.className
      			// className是内部保留的关键字,没有style
      			oDiv2.className = "box2";     
      		}
      
      	</script>
      
      	<style type="text/css">		
      		.box{
      			font-size:20px;
      			color:gold;
      		}
      		.box2{
      			font-size:30px;
      			color:pink;
      		}
      	</style>
      </head>
      <body>
      	<div id="div1">这是一个div元素</div>
      	<a href="#" id="link">这个一个链接</a>
      	<div class="box" id="div2">这是第二个div</div>
      </body>	
      ```
      
      1. 先执行window.onload, 再执行 css

#### 05 style属性操作

1. js属性: 非表单属性, 表单属性, style属性, 自定义属性, className属性

1. style属性操作方法 

   1. `对象名.sytle.属性名`

      1. ```
         <script type="text/javascript">
            	widow.onload=function(){
                    var oDiv = document.getElementById('div1');
            		//读 对象名.属性名
                    var sId = oDiv.id;
                    //写 对象名.属性名
                    oDiv.style.color = "red";
            	}
            </script>
      
      
      
   2. 变量操作属性: `对象名.style[变量]`,  
   
      1. 变量, 操作 属性, 操作变量,  无 引号
   
      2. ```
          <script type="text/javascript">
                window.onload = function(){
                    var oDiv = document.getElementById('div1');
                    /*变量代替属性*/
                    var sMystyle = 'color';
                    var sValue = 'red';
                          /*变量代替属性, 用[]操作 */
              	  oDiv.style[sMystyle] = sValue;
            }
         </script>
      
      
      
       
      

#### 06 改变元素内容

1. innerHTML   属性

   1.  innerHTML在JS是双向功能：获取对象的内容 或 向对象插入内容

   2. ```
      <head>
      <script type="text/javascript">
          window.onload = function(){
              var oDiv = document.getElementById('div1');
              //读取
              var sTxt = oDiv.innerHTML;
              alert(sTxt);
              //写入
              oDiv.innerHTML = '<a href="http://www.baidu.cn">百度<a/>';
          }
      </script>
      </head>
      <body>
      <div id="div1">这是一个div元素</div>
      </body>
      ```

#### innerHTML,  innerText, value

1. innerHTML

   1. 控件中添加html代码 , 标签对文本有效

   2. ```
      <!doctype html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
      </head>
      <body>
          <h1 id="demo">demo1</h1>  <!--<b>world23</b>代替demo1, 输出word23-->
      </body>
      <script>
          document.getElementById("demo").innerHTML="<b>world23</b>"   <!--不是innerHtml-->
      </script>
      </html>
      ```

2. innerText

   1. 代替标签之间的纯文本，显示标签，标签无效. 低版本的火狐浏览器不支持

   2. ```
      <!doctype html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
      </head>
      <body>
          <h1 id="demo">demo1</h1>  <!--<h5>word</h5>代替demo1-->
      </body>
      <script>
          document.getElementById("demo").innerText="<h5>word</h5>";  <!--不是innerTEXT-->
      </script>
      </html>
      ```

3. value

   1. 标签的值属性, 显示双引号中的全部内容，显示标签，标签无效

   2. ```
      <!doctype html>
          <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
      </head>
      <body>
          <h3 id="demo">demo2</h3>
          <input id="input" type="text">
      </body>
      <script>
          document.getElementById("demo").value="<b>hello</b>"
          document.getElementById("input").value="<b>world</b>"
      </script>
      </html>
      ```

      




## 02-JavaScript函数

#### 01函数定义与调用

1. 函数定义 和 执行

   1. 语法

      1. ```
         //实名函数定义
         function 函数名(){    //function关键字
         	语句块;
         }
         
         函数名();  //执行函数
         ```
         
      2. ```
         //匿名函数定义
         function(){    //匿名函数, function关键字
         	语句块;
         }
         ```
      
         
      
   2. 函数定义与执行.html
   
      1. ```
         <head>
             <script type="text/javascript">
                 // 函数定义
                 function fnMyalert(){
                     alert('hello!');
                 }
                 // 函数执行
                 fnMyalert();
                 
                 // 函数定义二
                 function fnChange(){
                     var oDiv = document.getElementById('div1');
                     oDiv.style.color="red";
                     oDiv.style.fontSize="30px"
                 }
             </script>
         </head>
         <body>
         	<!--标签内调用 : 行间事件 写法: 函数带() -->
         	<div id="div1">这是一个div元素</div>
         	<input type="button" name="" value="改变div" onclick="fnChange()">
         </body>
         ```
   
         1. scrapy中调用: 函数名(), 无双引号
   
         2. html中调用: 行间事件, onclick = "函数名()" 调用, 有双引号
   
         2. onclick: 事件属性
         
            

#### 02提取行间事件

1.   提取行间事件, 结果与 行为 分离

2. 提取行间事件.html

   1. ```
      <head>
          <script type="text/javascript">
          	<!--页面嵌入调用 --> <!--写法: 函数不带(), -->
      		window.onload = function(){
      			var oBtn = document.getElementById('btn01');
      			
      			//引用: 对象.事件属性=函数名, 实现事件与函数关联
      			oBtn.onclick = fnChange;
      			
                  function fnChange(){
                  	var oDiv = document.getElementById('div1')
                      oDiv.style.color="red";
                      oDiv.style.fontSize="30px"
                  }
              }
          </script>
      </head>
      <body>
      	
      	<div id="div1">这是yi个div元素</div>
      	<input type="button" name="" value="改变div" id="btn01">
      </body>
      ```

      1.  函数执行 函数名(),  函数 引用 函数名不带 ()

         

#### 03 匿名函数

1. 匿名函数.html

   1. ```
      <script type="text/javascript">
          // 页面嵌入调用, 写法: 函数不带()
          window.onload = function(){
              var oBtn = document.getElementById('btn01')
      
              //匿名函数
              oBtn.onclick = function(){
                  var oDiv = document.getElementById('div1')
                  oDiv.style.color="red";
                  oDiv.style.fontSize="30px"
              }
          }
      </script>
      
      <body>    
          <div id="div1">这是yi个div元素</div>
          <input type="button" name="" value="改变div" id="btn01">
      </body>
      ```
      
      1. function() {}   匿名函数,  function后没有函数名
      2. 两个过程 -->一个过程



#### 04网页换肤

1. 网页换肤

   1. 网页换肤.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<link rel="stylesheet" type="text/css" href="css/skin01" id="link01">
         	<script type="text/javascript">
         		window.onload = function(){
                     var oBtn01 = document.getElementById('btn01');
                     var oBtn02 = document.getElementById('btn02');
                     var oLink = document.getElementById('link01');
                     
                     oBtn01.onclick = function(){
                         oLink.href = "css/skin01.css"   //对象.属性=属性值
                     }
                     
                     oBtn02.onclick = function(){
                         oLink.href = "css/skin02.css"   //href前没有style
                     }
         		}
         	</script>
         </head>
         <body>
         	<input type="button" name="" value="皮肤1" id="btn01">
         	<input type="button" name="" value="皮肤2" id="btn02">
         </body>
         </html>
         ```

         1.  link 标签 添加 id 属性,
         2. 通过id , javascript获取标签,  操作标签属性
   
      2. skin01.css
   
         1. ```
            body{
            	background-color:green;
            }
            
            input{
            	width:200px;
            	height:50px;
            	background-color:gold;
            	border:0;
            }
            ```
   
      3. skion02.css
   
         1. ```
            body{
            	background-color:#ddd;
            }
            
            input{
            	width:200px;
            	height:50px;
            	border-radius:25px;
            	background-color:pink;
            	border:0;
            }
            ```
   
         
   
   2. js对标签的属性赋值函数, 实现标签的动作



#### 05函数和变量预解析

1.  预解析: 声明提前

   1.  变量的声明提前  ( 不一定初始化)
   2.  函数声明 定义 提前.
   3. javascript解析过程: 先 编译阶段, 再 执行阶段

2.  变量和函数的与解析.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">		
      
      		// 预解析 让变量的声明提前
      		alert(iNum);  //弹出undefined
      
      		// 预解析会让函数的声明和定义提前
      		myalert();
      
      		// 使用一个未声明的变量，出错！
      		//alert(iNum02);
      
      		var iNum = 12;
      
      		function myalert(){
      			alert('hello world!');
      		}
      	</script>
      </head>
      <body>
      	
      </body>
      ```

   



#### 06函数传参

1. 函数传参.html

   1. ```
      <head>
          <script type="text/javascript">
              window.onload = function(){
                  //函数传参
                  function fnChangestyle(mystyle, val){
                      var oDiv = document.getElementById('div1');
                      oDiv.style[mystyle] = val;  //对象.style[变量]=值,对应 对象.style['属性名']=值
                  }
                  
                  fnChangestyle('fontSize', '30px');
                  fnChangestyle('color', 'red');
              }
          </script>
      </head>
      <body>
          <div id="div1"> 一个div元素</div>
      </body>
      ```

2. 带参函数语法

   1. ```
      function 函数名(参数){  //参数没有类型
      	语句块
      }
      ```

3. function中嵌套函数定义语法:

   1. ```
      function(){
      	function 函数名1(){
      		语句块
      		}
      	function 函数块2(){
      		语句块
      		}
      }

   1. 函数可以没有return 语句 

#### 07函数 return关键字

1. 函数返回值.html

   2. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		
      		function fnAdd(a,b) {
      			var c = a + b;
      
      			// 返回c的值，然后结束函数的运行
      			return c;  //停止, 后面不执行
      
      			alert("内部的c="+c);
      		}
      
      		var iResult = fnAdd(2,5);
      		alert(iResult);
      	</script>
      </head>
      <body>
      	
      </body>
      ```

3. return 关键字: 

      1.  返回函数 结果
      2.  结束函数 运行
      3.  阻止 默认行为

4. 按需要: 需要写, 不需要不写.

---

## 03分支语句

#### 001条件语句-切换例子

1. 012条件语句.html

   1. ```
      if(){}
      else{} 
      ```

   2. ```
      <script type="text/javascript">
      	var iNum01=12;
      	var iNum02 = 24;
      	if(iNum01 > iNum02){
              alert('大于');
      	}
      	else{
              alert('不大于');
      	}
      </script>
      ```

2. 013按钮切换元素显示.html

   1. ```
      <script type="text/javascript">
      	window.onload = function(){
              var oBtn = document.getElementById('btn01');
              var oDiv = document.getElementById('box01');
              
              oBtn.onclick = function(){
                  if(oDiv.style.display == 'none'){
                      oDiv.style.display = 'block';
                  }
                  else{
                      oDiv.style.display = 'none';
                  }
              }
      	}
      </script>
      <style type="text/css">
      	.box{
              width:200px;
              height:400px;
              background-color:gold;
      	}
      </style>
      <body>
      	<input type="button" name="" value="切换" id="btn01">
      	<div class="box" id="box01"></div>
      </body>
      ```

3. 按钮切换 优化

   1. ```
      <script type="text/javascript">
      	window.onload = function(){
              var oBtn = document.getElementById('btn01');
              var oDiv = document.getElementById('box01');
              
              oBtn.onclick = function(){
                  if(oDiv.style.display == 'block' || oDiv.style.display==''){
                      oDiv.style.display = 'none';
                  }
                  else{
                      oDiv.style.display = 'block';
                  }
              }
      	}
      </script>
      ```

      



#### 002 多重条件 

1. ```
   if(){}
   else if(){}
   ......
   else if(){}
   else{}
   
   与c语言及其类似
   ```
   
2. 014多重条件语句.html

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript">
              window.onload = function(){
                  var iWeek = 5;
                  var oBody = document.getElementById('body01');
      
                  if(iWeek==1){
                      oBody.style.backgroundColor = 'gold';
                  }
                  else if(iWeek==2)
                  {
                      oBody.style.backgroundColor = 'green';
                  }
                  else if(iWeek==3){
                      oBody.style.backgroundColor = 'pink';
                  }
                  else if(iWeek==4){
                      oBody.style.backgroundColor = 'yellowgreen';
                  }
                  else if(iWeek==5){
                      oBody.style.backgroundColor = 'lightblue';
                  }
                  else{
                      oBody.style.backgroundColor = 'lightgreen';
                  }
              }
          </script>
      </head>
      <body id="body01">    
      </body>
      ```

-----

#### 003作业

1. 计算器

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript">
              window.onload = function(){
      
                  var oInput01 = document.getElementById('input01');
                  var oInput02 = document.getElementById('input02');
      
                  var oBtn = document.getElementById('btn');
      
      
                  oBtn.onclick = function(){
                      var sVal01 = parseInt(oInput01.value) + parseInt(oInput02.value);
      
                      alert(sVal01);
                  }
              }
          </script>
      </head>
      <body>
          <input type="text" name="" id="input01"> ＋
          <input type="text" name="" id="input02">
          <input type="button" name="" value="相加" id="btn">
      </body>
      
      ```

#### 04复习

#### 05加法运算练习

1. parseInt()  // 字符串转 Int

#### 06 求余 - 赋值运算符

1. 运算符

   1. 算术运算符： +(加)、 -(减)、 *(乘)、 /(除)、 %(求余)
      1. C/C++, C#, JAVA, PHP这几门主流语言中，’%’运算符都是做取余运算，而在python中的’%’是做取模运算
      2. 取余: 商值向0方向舍弃小数位
      3. 取模: 商值向负无穷方向舍弃小数位
   2. 赋值运算符：=、 +=、 -=、 *=、 /=、 %=
   3. 条件运算符：==、===、>、>=、<、<=、!=、&&(而且)、**||**(或者)、!(否)

2. 002求余数.html

   1. ```
      <script type="text/javascript">
      	var iNum01 = 11;
      	var iNum02 = 2;
      	
      	alert(iNum01 % iNum02); // 弹出1
      </script>
      ```

3. 003赋值运算.html

   1. ```
      <script type="text/javascript">
      	var iNum01 = 12;
      	// iNum01 = iNum01 + 2; //简写
      	iNum01 += 2;
      	alert(iNum01);
      </script>
      ```

#### 07 条件运算符

1. 004条件运算符.html

   1. ```
      <script type="text/javascript">
      	var iNum01 = 2;
      	var sNum01 = '2';
      	
      	if(iNum01 === sNum01){
              alert('相等')
      	}
      	else{
              alert('不相等')
      	}
      </script>
      ```

      1. `==`   值相等,  先转同一类型, 再比较大小
      2. `===`  值类型 都相等

#### 08 switch语句

1. switch 比 if -- else if  性能高

   1. if(){} else if(){} ...else{}

   2. swith(){ case 1: {...break;}... default: {}}

   3. ```
      <script type="text/javascript">
          window.onload = function(){
              var iWeek = 3;
              var oBody = document.getElementById('body01');
      
              if(iWeek==1){
                  oBody.style.backgroundColor = 'gold';
              }
              else if(iWeek==2)
              {
                  oBody.style.backgroundColor = 'green';
              }
              else if(iWeek==3){
                  oBody.style.backgroundColor = 'pink';
              }
              else if(iWeek==4){
                  oBody.style.backgroundColor = 'yellowgreen';
              }
              else if(iWeek==5){
                  oBody.style.backgroundColor = 'lightblue';
              }
              else{
                  oBody.style.backgroundColor = 'lightgreen';
              }
          }
      </script>
      ```

      

2. 005 switch语句.html

   1. ```
      <script type="text/javascript">
      		window.onload = function(){
      			var iWeek = 3;
      			var oBody = document.getElementById('body01');
      
      			switch (iWeek){
      				case 1:
      					oBody.style.backgroundColor = 'gold';
      					break;
      				case 2:
      					oBody.style.backgroundColor = 'green';
      					break;
      				case 3:
      					oBody.style.backgroundColor = 'pink';
      					break;
      				case 4:
      					oBody.style.backgroundColor = 'yellowgreen';
      					break;
      				case 5:
      					oBody.style.backgroundColor = 'lightblue';
      					break;
      				default:
      					oBody.style.backgroundColor = 'lightgreen';
      			}
      		}
      </script>
      ```

## 04-数组和循环语句

#### 01 数组及操作方法

1. js中数组 类似 python 中 list, 但元素类型可为各种类型

   1. 创建数组
      1. var aList01 = new Array(1,2,3);
      2. var aList02 = ['a',2,3];

   2. 元素类型可以不同

2. 006数组.html

   1. ```
      <script type="text/javascript">
      	// 类实例化方式 创建数组
      	var aList01 = new Array(1, 2, 3);
      	
      	// 直接量方式 创建数组
      	var aList02 = ['a', 2, 3];
      	
      	//操作数组
      	//数组的长度：aList.length
      	alert(aList01.length);
      	
      	//[]下标操作 某个元素, 下表从零开始
      	alert(aList[0])
      	
      	//push(),pop() 数组最后增加/删除成员
      	aList01.push(5) ; 
      	alert(aList01); //弹出 1, 2, 3, 5
      	
      	aList01.pop();
      	alert(aList01); //弹出1,2,3
      	
      	//unshift(),shift() 数组前面增加/删除成员
      	aList01.unshift(5); //前面(左侧)添加5
      	alert(aList01); // 弹出 5,1,2,3
      	
      	aList01.shift(); //前面(左)删除5
      	alert(aList01);// 弹出1,2,3
      	
      	// 数组反转
      	aList01.reverse();  
      	alert(aList01); // 弹出 3,2,1
      	aList01.reverse();
      	
      	//indexOf(2) 第一次出现2的索引值
      	alert(aList01.indexOf(2))  //弹出 1
      	
      	//splice() 增加/删除成员
      	//参数: 第一个:起始索引, 第二个:删除个数, 之后:要添加的元素
      	aList01.splice(2,1,7,8,9); //从第2个元素开始，删除1个，在此位置增加'7,8,9'
      	alert(aList01); //弹出 1,2,7,8,9
      	
      	//join() 元素用分隔符连成新字符串, 原数组不变
      	var aJoin = aList01.join('-'); //aList01不变, 返回字符串
      	alert(aJoin); // 返回 1-2-7-8-9
      </script>
      ```

      1.  join(),  返回新字符串, 不修改原数组, 
      2. 数组元素为不同类型
      3. 下标从 0 开始

#### 02多维数组

1. 多维数组: 元素也是数组

2. 007多维数组.html

   1. ```
      <script type="text/javascript">
      	// 直接量方式 创建数组
      	var aList = [[1,2,3,4],['a','b',4],[6,7,8]]
      	
      	alert(aList.lenght);  // 3
      	alert(aList[1][0]);  // 弹出a
      </script>
      ```

      1.  多维数组, 各维度长度 不必相等
      2. 多维数组, 各元素数组类型 不必相同

#### 03 for循环-实例

1. for 循环: 遍历数组.

   2. ```
      for(var i=1; i< iLen; i++){}
      与c语言类似
      ```
      
      

2. 008 for 循环语句.html

   1. ```
      <script>
      	var aList = ['a', 'b', 'c', 'd'];
      	var iLen = aList.length;
      	
      	for(var i=0; i<iLen; i++)
      	{
              alert(aList[i]);
      	}
      </script>
      ```
      
      1.  for(var i=0; i<iLen; i++) 中 用iLen 代替 aList.Length , 避免每次都查询. 是优化 方法.

3. 009 循环将数据放入都页面中

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript">
              window.onload = function(){
                  var oUl = document.getElementById('list');
                  var aList = ['美人鱼','疯狂动物城','魔兽','美国队长3','湄公河行动'];
                  var iLen = aList.length;
                  var sTr = '';
      
      
                  for(var i=0;i<iLen;i++)
                  {
                      sTr += '<li>'+ aList[i]+ '</li>';
                  }
      
                  oUl.innerHTML = sTr;
              }
          </script>
      
          <style type="text/css">        
              .list{
                  list-style:none;
                  margin:50px auto 0;
                  padding:0;
                  width:300px;
                  height:305px;            
              }
      
              .list li{
                  height:60px;
                  border-bottom:1px dotted #000;
                  line-height:60px;
                  font-size:16px;
              }
          </style>
      </head>
      <body>
          <ul class="list" id="list"> 
          </ul>
      </body>
      ```
      
      1. js 用innerHTML把字符串格式 添加html形成 html的 标签 文本



#### 04 while 循环

1. while 循环

   1. ```
      var i=0;
      while(i<8){
      	语句块;
      }
      ```
   
2. 数组去重

   3. ```
      <head>
      	<scripy type="text/javascript">
      		var aList = [2,3,4,3,5,6,7,8,3,4,5,2,7,8,9,3,4,5,6,3,4,8,9];
      		var iLen = aList.length;
      		var aList2 = [];
      		
      		for(var i=0;i<iLen;i++)
      		{
                  if(aList.indexOf(aList[i])==i){
                      aList2.push(a)
                  }
      		}
      	</scripy>
      	</style>
      </head>	
      <body>
      	<ul class="list" id="list">	
      	</ul>
      </body>
      ```

      1. indexOf(aList[i])   返回值是i, i 下标

## 05-JavaScript体系和字符串

#### 01标签获取元素 - 实例

1. 通过标签获取元素

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>通过标签获取元素</title>
          <script type="text/javascript">
              window.onload = function(){
      
                  //标签名获取li标签, 生成选择集aLi
                  var aLi = document.getElementsByTagName('li');
      
                  //读取选择集内元素个数
                  alert(aLi.length);  // 弹出13
      
                  var iLen = aLi.length;
      
      
                  aLi[0].style.backgroundColor = 'green';   
      
                  //不能给选择集设置样式属性
                  // aLi.style.backgroundColor = 'gold';  //错误
      
                  //for循环遍历选择集中的元素, 给各个标签赋值
                  for(var i=0;i<iLen;i++)
                  {
                      aLi[i].style.backgroundColor = 'pink';
                  }
                  
      
                  //通过Id获取标签
                  var oUl = document.getElementById('list1');
                  var aLi2 = oUl.getElementsByTagName('li');   //返回标签集
      
                  var iLen2 = aLi2.length;
      
                  for(var i=0;i<iLen2;i++)
                  {
                      if(i%2==0)
                      {
                          aLi2[i].style.backgroundColor = 'blue';
                      }
                  }
      
              }
      
          </script>
      </head>
      <body>
          <ul id="list1">
              <li>1</li>
              <li>2</li>
              <li>3</li>
              <li>4</li>
              <li>5</li>
              <li>6</li>
              <li>7</li>
              <li>8</li>
          </ul>
      
          <ul id="list2">
              <li>1</li>
              <li>2</li>
              <li>3</li>
              <li>4</li>
              <li>5</li>
          </ul>
      </body>
      ```

#### 02  javascript 生态体系

1. | ECMAscript                                                   | DOM                                              | BOM                       |
   | ------------------------------------------------------------ | ------------------------------------------------ | ------------------------- |
   | javascript的语句                                             | 文档对象模型                                     | 浏览器对象模型            |
   | 具体指: 常量, 变量, 函数, 数据类型, for循环语句, 运算符, 变量作用域 | 具体指: 操作html和css的方法                      | 具体指 : 操作浏览器的方法 |
   | 如 :  var i = 10; for(){ },  switch(){ }                     | 如  :  getElementById(),  getElementsByTagName() | 如 : alert()              |

   1. 共 3 部分
      1. ECMAScript是JavaScript的核心，浏览器脚本语言的规范, 规定了JS的语法规范
      2. **DOM**，全称Document Object Model, 即文档对象模型， 允许脚本(js)控制Web页面、窗口和文档
         1. HTML DOM 定义了 HTML 的一系列标准的对象，以及访问和处理 HTML 文档的标准方法
         2. DOM是 把html里的各种数据当做对象处理的一种思路
      3. BOM
         1. 含windows（窗口）、navigator（浏览器）、screen（浏览器屏幕）、history（访问历史）、location（地址）
         2. BOM即浏览器对象模型，它提供了独立于内容而与浏览器窗口进行交互的对象，其核心对象是window

#### 03 字符串操作方法 - 实例

1.  js 处理 字符串不强, 一般后台处理强.

    1.  字符串+数字, js中可以直接连接

2.  字符串处理 10种方法

   1. ```
      <scripy type="text/javascript">
      	//1. + , 加, 连接. 偷懒性(只连接, 不加)
      	var iNum01 = 12;
      	var sNum03 = '12';
      	alert(iNum01 + sNum03);// 弹出1212
      	
      	//2. parseInt()  字符-->整数
      	var sNum01 = '12';
      	var sNum03 = '12.32';
      	
      	//
      	alert(parseInt(sNum01)); //弹出数字12
      	alert(sNum03); ////弹出数字12 字符串小数转化为数字整数
      	
      	//3. parseFloat() 字符串转化float
      	var sNum03 = '12.32'
      	alert(parseFloat(sNum03));  //弹出 12.32 将字符串小数转化为数字小数
      	
      	//4. split()  字符串分隔为数组
      	var sTr = '2017-4-22';
      	var aRr = sTr.split("-");
      	var aRr2= sTr.split("");
      
      	alert(aRr);  //弹出['2017','4','2']
      	alert(aRr2);  //弹出['2','0','1','7','-','4','-','2','2']
      	
      	//5. charAt(3) 下标3的一个字符
      	var sId = "#div1";
      	var sTr = sId.charAt(0);
      	alert(sTr); //弹出 #
      	
      	//6. indexOf() 字符的下标
      	var sTr = "abcdefgh";   //下标从0开始
      	var iNum = sTr.indexOf("c");  
      	alert(iNum); //弹出2, 起始下标
      	
      	//7. substring(start,end)（不包括end）截取字符串
      	var sTr = "abcdefghijkl";
      	var sTr2 = sTr.substring(3,5);
      	var sTr3 = sTr.substring(1);
      
      	alert(sTr2); //弹出 de
      	alert(sTr3); //弹出 bcdefghijkl
      	
      	//8. toUpperCase() 转大写
      	var sTr = "abcdef";
      	var sTr2 = sTr.toUpperCase();
      	alert(sTr2); //弹出ABCDEF
      	
      	//9. toLowerCase() 转小写
      	var sTr = "ABCDEF";
      	var sTr2 = sTr.toLowerCase();
      	alert(sTr2); //弹出abcdef
      	
      	//10. reverse() 字符串翻转
      	var str = 'asdfj12jlsdkf098';
      	var str2 = str.split('').reverse().join('');
      	alert(str2);
      </scripy>
      ```






## 06-定时器和变量作用域

#### 1 定时器

1. 定时器

   1. 作用: 制作动画,  异步操作, 函数缓冲与节流

   2. 定时器类型 语法

      1. setTimeout(函数名, 时间)    只执行一次的定时器 
      2. clearTimeout(函数名, 时间)  关闭只执行一次的定时器
      3. setInterval(函数名, 时间)   反复执行的定时器
      4. clearInterval(函数名) 关闭反复执行的定时器
      2. 总结: 一次定时器, 反复定时器 
      
   3. 定时器种类.html
   
      1. ```
         <script type="text/javascript">
         	function myalert(){
                 alert('ok!');
             }
         	//一次定时器的参数: 第一个: 函数名或匿名函数, 第二: 时间,ms单位 
         	var time1 = setTimeout(myalert,2000);  //一次定时器, myalert 函数名    
         
            // clearTimeout(time1);  //关闭一次定时器. 关闭后不弹不出
             
         
             // 反复定时器    
             var time2 = setInterval(myalert,2000); //反复定时器, 
             //clearInterval(time2);  //关闭反复定时器, 一次也不执行了
         </script>
         ```
   
      2. 匿名函数式
   
         1. ```
            var timer02 = setInterval(function(){
                    alert('ok!');
            },2000);
            ```
   
   4. 014定时器动画.html
   
      1. ```
         <head>
         <script type="text/javascript">
             window.onload = function(){
                 var oDiv = document.getElementById('div1');
                 var iLeft = 0;
                 /*
                 var timer = setInterval(moving, 30);
                 function moving(){
                 	ileft += 3;
                 	oDiv.style.left = iLeft + 'px';   //3px=3+'px', px是字符串
                 }        
                 */
                 var timer = setInterval(function(){
                 	iLeft += 3;
                 	oDiv.style.left = iLeft + 'px';
                 	if(iLeft > 700){
                 		clearInterval(timer);
                 	}
                 }, 30);
                 
             }
         
         </script>
         <style type="text/css">
         	.box{
         	width:200px;
         	height:200px;
         	background-color:gold;
         	position:absolute;
         	left:0;
         	top:100px;
         	}
         </style>
         </head>
         <body>
         	<div id="div1" class="box"></div>
         </body>
         ```
   
   5. 制作倒计时定时器.html
   
      1. ```
         <script>
         <script type="text/javascript">
             window.onload = function(){
                 var oDiv = document.getElementById('div1');
                 function timeleft(){
                     var now = new Date();  //类实例化, 现在日期时间
                     var future = new Date(2016,8,12,24,0,0);  //单位ms
                     var lefts = parseInt((future-now)/1000);  //剩余时间
                     var day = parseInt(lefts/86400);
                     var hour = parseInt(lefts%86400/3600);
                     var min = parseInt(lefts%86400%3600/60);
                     var sec = lefts%60;
                     str = '距离2016年9月12日晚24点还剩下'+day+'天'+hour+'时'+min+'分'+sec+'秒';
                     oDiv.innerHTML = str; 
                 }
                 timeleft();
                 setInterval(timeleft,1000);        
             }
         
         </script>
         <body>
         	<div id="div1"></div>
         </body>
         ```

#### 2复习

1. 空

#### 3定时器动画

1. 定时器动画-- 往返运动动画.html

   1. ```
      <script type="text/javascript">
          window.onload = function(){
              var oDiv = document.getElementById('div1');
              var iLeft = 0;
              /*
              var timer = setInterval(moving, 30);
              function moving(){
                  ileft += 3;
                  oDiv.style.left = iLeft + 'px';   //3px=3+'px', px是字符串
              }        
              */
              iSpeed=3;
              var timer = setInterval(function(){
      
                  iLeft += iSpeed;
                  oDiv.style.left = iLeft + 'px';
                  if(iLeft > 700){
                      iSpeed=-3;
                  }
      
                  if(iLeft<0)
                  {
                      iSpeed = 3;
                  }
      
              }, 20);
              
          }
      
      </script>
      <style type="text/css">
          .box{
          width:200px;
          height:200px;
          background-color:gold;
          position:absolute;
          left:0;
          top:100px;
          }
      </style>
      </head>
      <body>
          <div id="div1" class="box"></div>
      </body>
      ```

#### 4 无缝滚动01

#### 5 无缝滚动02

1.  滚动图.html

   ```
   <!DOCTYPE html>
   <html lang="en">
   <head>
   	<meta charset="UTF-8">
   	<title>无缝滚动</title>
   	<style type="text/css">
   		*{
   			margin:0;
   			padding:0;
   		}
   
   		.list_con{
   			
   			width:1000px;
   			height:200px;
   			border:1px solid #000;
   			margin:10px auto 0;
   			background-color:#f0f0f0;
   			position:relative;
   			overflow:hidden;
   		}
   
   		.list_con ul{
   			list-style:none;
   			width:2000px;
   			height:200px;
   			position:absolute;
   			left:0;
   			top:0;
   		}
   
   
   		.list_con li{
   			width:180px;
   			height:180px;
   			float:left;
   			margin:10px;
   		}
   
   		.btns_con{
   			width:1000px;
   			height:30px;
   			margin:50px auto 0;
   			position:relative;
   		}
   
   		.left,.right{
   			width:30px;
   			height:30px;
   			background-color:gold;
   			position:absolute;
   			left:-40px;
   			top:124px;
   			font-size:30px;
   			line-height:30px;
   			color:#000;
   			font-family: 'Arial';
   			text-align:center;
   			cursor:pointer;
   			border-radius:15px;
   			opacity:0.5;
   		}
   
   		.right{
   			left:1010px;			
   			top:124px;			
   		}
   
   	</style>
   	<script type="text/javascript">
   		window.onload = function(){
   			var oDiv = document.getElementById('slide');
   			var oBtn01 = document.getElementById('btn01');
   			var oBtn02 = document.getElementById('btn02');
   
   
   			//通过标签获名，获取的是选择集 [ul] 即[object HTMLCollection]，通过下标才能获取到元素			
   			var oUl = oDiv.getElementsByTagName('ul')[0];
   			var iLeft = 0;
   			var iSpeed = -2;
   			var iNowspeed = 0;
   
   			//将ul里面的内容复制一份，整个ul里面就包含了10个li
   			oUl.innerHTML = oUl.innerHTML + oUl.innerHTML;	
   
   			function moving(){
   				iLeft += iSpeed;
   
   				// 当ul向左滚动到第5个li时，瞬间将整个ul拉回到初始位置
   				if(iLeft<-1000)
   				{
   					iLeft=0;
   				}
   				//当ul在起始位置往右滚动时候，瞬间将整个ul拉回到往左的第5个li的位置
   				if(iLeft>0)
   				{
   					iLeft = -1000;
   				}
   
   				oUl.style.left = iLeft + 'px';
   			}
   
   			var timer = setInterval(moving,30);
   
   
   			oBtn01.onclick = function(){
   				iSpeed = -2;
   			}
   
   			oBtn02.onclick = function(){
   				iSpeed = 2;
   			}
   			// 当鼠标移入的时候
   			oDiv.onmouseover = function(){
   				iNowspeed = iSpeed;
   				iSpeed = 0;
   			}
   		    // 当鼠标移出的时候
   			oDiv.onmouseout = function(){
   				iSpeed = iNowspeed;
   			}
   
   		}
   
   
   	</script>
   </head>
   <body>
   	<div class="btns_con">
   		<div class="left" id="btn01">&lt;</div>
   		<div class="right" id="btn02">&gt;</div>
   	</div>
   	<div class="list_con" id="slide">
   		<ul>
   		<li><a href=""><img src="images/goods001.jpg" alt="商品图片"></a></li>
   		<li><a href=""><img src="images/goods002.jpg" alt="商品图片"></a></li>
   		<li><a href=""><img src="images/goods003.jpg" alt="商品图片"></a></li>
   		<li><a href=""><img src="images/goods004.jpg" alt="商品图片"></a></li>
   		<li><a href=""><img src="images/goods005.jpg" alt="商品图片"></a></li>
   		</ul>		
   	</div>
   </body>
   </html>
   ```
   
2. 事件

   1.  对象名.onclick = function(){}
   2.  对象名.onmouseover = function(){}
   3.  对象名.onmouseout = function(){}


#### 6 变量作用域

1. 变量作用域

   1. 全局变量：函数外, 整个页面共用，函数内外都可访问。
   2. 局部变量：函数内, 定义函数内访问，函数外部无法访问。

2. 002变量的作用域

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      
      		//全局变量,函数外 ，函数内部和外部都可以访问
      		var iNum01 = 12;
      
      		// 重复定义，后面的覆盖前面的值
      		//var iNum01 = 14;
      
      		function myalert(){
      			//var iNum01 = 24;
      			// 局部变量，内部定义, 内部可以访问，外部不能访问
      			var iNum02 = 36;
      
      			alert(iNum01);
      			iNum01 += 10;
      		}
      
      		function myalert02(){
      			alert(iNum01);
      		}
      		
      		//调用函数
      		myalert();  // 弹出 12
      		myalert02();  // 弹出 22
      
      		//alert(iNum02);  出错！内部变量
      	</script>
      </head>
      <body>
      	
      </body>
      ```

#### 7 日期时间

1. 时间函数

   1.  Date(年, 月, 日, 时, 分, 秒);

      1.  内部类
      2. 获取 年, 月, 日, 时, 分, 秒的方法
      3.  年从0 , 星期从0, 日从0 开始

   2.  getFullYear(), getMonth(), getDate(), getDay(), getHours(), getMinutes(), getSeconds()

2. 时钟.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		window.onload = function(){
      			var oDiv = document.getElementById('div1');
      
      			function fnTimego(){
      				
      				//1. 日期
      				var sNow = new Date();
      				//2. 年
      				var iYear = sNow.getFullYear();
      
      				//3. 月，月份是从0到11，0表示一月，11表示十二月
      				var iMonth = sNow.getMonth()+1;
      				
      				//4. 日
      				var iDate = sNow.getDate();
      
      				// 星期是从0到6,0表示星期天
      				//5. 星期
      				var iWeek = sNow.getDay();
      				//6. 时
      				var iHour = sNow.getHours();
      				//7. 分
      				var iMinute = sNow.getMinutes();
      				//8. 秒
      				var iSecond = sNow.getSeconds();
      
      				var sTr = '当前时间是：'+ iYear + '年'+ iMonth + '月'+ iDate+'日 ' + fnToweek(iWeek) + ' ' +fnTodou(iHour) + ':' + fnTodou(iMinute) + ':' + fnTodou(iSecond);
      
      				oDiv.innerHTML = sTr;
      			}
      
      
      			// 刚开始调用一次，解决刚开始1秒钟空白的问题
      			fnTimego();
      
      			setInterval(fnTimego,1000);  // 1000 间隔时间
      
      
      			function fnToweek(n){
      				if(n==0)
      				{
      					return '星期日';
      				}
      				else if(n==1){
      					return '星期一';
      				}
      				else if(n==2){
      					return '星期二';
      				}
      				else if(n==3){
      					return '星期三';
      				}
      				else if(n==4){
      					return '星期四';
      				}
      				else if(n==5){
      					return '星期五';
      				}
      				else{
      					return '星期六';
      				}
      			}
      
      			//个位加0
      			function fnTodou(n){
      				if(n<10)
      				{
      					return '0'+n;
      				}
      				else
      				{
      					return n;
      				}
      			}
      		}
      
      
      	</script>
      	<style type="text/css">		
      		div{
      			text-align:center;
      			font-size:30px;
      			color:pink;
      		}
      	</style>
      </head>
      <body>
      	<div id="div1"></div>
      </body>
      </html>
      ```

#### 8 倒计时

1. 倒计时.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		window.onload = function(){
      			var oDiv = document.getElementById('div1');
      
      			function fnTimeleft(){
      				//实际开发中需要读取后台的时间，可以通过ajax来读取
      				var sNow = new Date();
      
      				// 未来时间：4月30日晚24点
      				var sFuture = new Date(2017,3,30,24,0,0);
      
      				//计算还有多少秒
      				var sLeft = parseInt((sFuture-sNow)/1000);
      
      				//计算还剩多少天
      				var iDays = parseInt(sLeft/86400);
      
      				// 计算还剩多少小时
      				var iHours  = parseInt((sLeft%86400)/3600);
      
      				// 计算还剩多少分
      				var iMinutes = parseInt(((sLeft%86400)%3600)/60);
      
      				// 计算还剩多少秒
      				var iSeconds = sLeft%60;
      
      				var sTr = '距离5月1日还剩：'+ iDays + '天' + fnTodou(iHours) + '时' + fnTodou(iMinutes) + '分'+ fnTodou(iSeconds) + '秒';
      
      
      				oDiv.innerHTML = sTr;
      			}
      
      			fnTimeleft();
      			setInterval(fnTimeleft,1000);
      			
      			//个位加0
      			function fnTodou(n){
      				if(n<10)
      				{
      					return '0'+n;
      				}
      				else{
      					return n;
      				}
      			}
      		}
      	</script>
      </head>
      
      <style type="text/css">
      	div{
      		text-align:center;
      		font-size:30px;
      		color:pink;
      	}
      </style>
      <body>
      	<div id="div1"></div>
      </body>
      ```

## 07-封闭函数和常用内置对象

#### 1 封闭函数

1. 封闭函数

   1. 语法: (function(){})();   括号代表 执行

      1. 独立: 创建独立空间, 内部函数 变量不受外界影响.

   2. 用途

      1. 网站增加新功能, 多用封闭函数, 安全, 不用其他功能.

      

2.  005封闭函数.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/main.js"></script>
      	<script type="text/javascript">
      		
      	//一般匿名函数
      	function myalert(){
      			alert('hello world!1');
      		}
      	myalert();
      
      	
      	// 写法1: 封闭函数
      	(function(){
      		alert('hello world!2');
      	})();
      
      
      	//写法2: 封闭函数
      	!function(){
      		alert('hello world!3');
      	}();
      
      	//写法3: 封闭函数
      	~function(){
      		alert('hello world!4');
      	}();
      	
      	var iNum01 = 12;
      
      	function myalert02(){
      		alert('hello world!5');
      	}
      
      	// 封闭函数前加一个“;” , 避免js压缩时候出错。
      	;(function(){
      
      		var iNum01 = 24;
      		//封闭函数内定义函数, 与外面同名函数没有关系
      		function myalert02(){
      			alert('hello hello!6');
      		}
      		alert(iNum01);
      		myalert02();
      
      	})();
      	
      	alert(iNum01);
      	myalert02();
      	</script>
      </head>
      <body>
      	
      </body>
      </html>
      ```
      
      1. scrapt中的所有函数都执行

#### 2 常用内置函数01

1. 常用内置对象
   1.  document
      1. 方法 
         1.  document.getElementById //通过id获取元素对象. 方法
         2. document.getElementsByTagName //通过标签名获取元素集.  方法
      2. 属性
         1. document.referrer  //上个跳转页面 的地址(服务器环境).
      
   2. window.location
   
      1.  属性
          1.  window.location.href  //获取或者重定url地址, 实现跳转
          2.   window.location.search //获取**地址参数**
          3.  window.location.hash //获取页面**锚点**或者叫哈希值
   
   3. window.location和window.location.href和document.location的关系
   
      1.  window.location和window.location.href
   
         1.  window.location.href是一个字符串。 与document.location.href是一样的
         2.  window.location是一个对象，包含属性有
         3.  hash 从井号 (#) 开始的 URL（锚）
         4.  host 主机名和当前 URL 的端口号
         5.  hostname 当前 URL 的主机名
         6.  href 完整的 URL
         7.  pathname 当前 URL 的路径部分
         8.  port 当前 URL 的端口号
         9.  protocol 当前 URL 的协议
         10.  search 从问号 (?) 开始的 URL（查询部分）
         11.  它们是包含关系
             1.  location 是 location.href 的简写，无论是访问还是赋值。
             2.  本质上, location是一个对象，location.href是它的一个属性。这种怪异的行为为了兼容。
   
      2.  document.location和window.location
   
         1.  document, 理解为文档，网页
         2.  window, 理解为窗口，ie浏览器包含的。
            1.  无框架：没有框架，是等同的
            2.  有框架：有框架，最外层是相同的. 在iframe里面的document.location和window.location不同的。
            3.  iframe的document.location 看不ie地址，只改变iframe部分，此时的window.location和top.location效果一致
   
      3.  006  window.location的属性.html
   
         1. ```
            <head>
            	<meta charset="UTF-8">
            	<title>Document</title>
            	<script type="text/javascript">
            		window.onload = function(){
            
            			//上一个网页的地址：
            			//var sUrl = document.referrer;
            			var oBtn = document.getElementById('btn01');
            
            			oBtn.onclick = function(){
            
            				//window.location.href = sUrl;
            				window.location.href = "http://www.baidu.com";
            
            			}
            		}
            	</script>
            </head>
            <body>
            	<input type="button" name="" value="跳转" id="btn01">
            </body>
   
      4. 页面通过浏览器传递参数
   
         1. 007  window.location的属性02.html
   
            1. ```
               <head>
               	<meta charset="UTF-8">
               	<title>Document</title>
               	<script type="text/javascript">
               		window.onload = function(){
               			var oBody = document.getElementById('body01');
               			//获取url中的参数
               			var sData = window.location.search;
               			alert(sData);
               			
               			//获取url中的锚点
               			var sHash = window.location.hash;
               			alert(sHash);
               
               			if(sData!='')
               			{
               				var aRr = sData.split("=");
               				var iNum = aRr[1];
               				
               				if(iNum==1)
               				{
               					oBody.style.backgroundColor = 'gold';
               				}
               				else if(iNum==2)
               				{
               					oBody.style.backgroundColor = 'green';
               				}
               				else
               				{
               					oBody.style.backgroundColor = 'pink';
               				}
               			}
               		}
               
               	</script>
               </head>
               <body id="body01">
               	<h1>007页面</h1>
               </body>
               ```
   
         2. 008链接到007页面.html
   
            1. ```
               <head>
               	<meta charset="UTF-8">
               	<title>Document</title>
               </head>
               <body>
               	<a href="007window-location的属性02.html">链接到007页面</a><br><br>
               	<a href="007window-location的属性02.html?aa=1">链接到金色背景007页面</a><br><br>
               	<a href="007window-location的属性02.html?aa=2">链接到绿色背景007页面</a>
               	<br><br>
               	<a href="007window-location的属性02.html?aa=3">链接到粉色背景007页面</a>
               	<br><br>
               </body>
               ```

#### 3  Math对象

1. Math对象

   1.  有 方法, 有 属性

   2. 方法

      1. ```
         Math.random() 获取0-1的随机数
         Math.floor() 向下取整
         Math.ceil() 向上取整
         ```

2. math.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		// Math常量
      		var iPi = Math.PI;		
      
      		var arr = [];  //js中的数组
      
      		for (var i=0;i<100;i++)
      		{	
      			// Math.random 只能返回从0到1之间的随机数，不包括0，也不包括1
      			var iNum = Math.random();
      			arr.push(iNum);   //向数组后面放入数据
      		}
      		//alert(arr);
      
      		console.log(arr);  //控制台输出
      
      		//向下取整，去掉小数部分
      		alert(Math.floor(5.6)); // 弹出5
      
      		//向上取整，去掉小数部分，整体加1
      		alert(Math.ceil(5.2))
      
      		// 10 - 20 之间的随机数
      		var iN01 = 10;
      		var iN02 = 20;
      		var arr2 = [];
      
      		for(var i=0;i<40;i++)
      		{
      			// 生成从10到20的随机数
      			var iNum02 = Math.floor((iN02-iN01+1)*Math.random()) + iN01;
      			arr2.push(iNum02);
      		}
      		console.log(arr2);
      	</script>
      </head>
      <body>
      	
      </body>
      ```

#### 4 调试js的方法

1.  alert()

   1. 先执行alert(), 暂停其他程序执行
   
2. console.log()

   1. 浏览器 console 控制台中输出, 
   2. 局部变量不能在console 中输出

3. document.title

   1. 对象document的属性, 页面 title中输出调试结果

4. 010调试js的方法.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      	   window.onload = function(){
      
      	   	var oBody = document.getElementById('body01');
      
      	   	var iNum01 = 12;
      
      	   	// alert 会阻止程序的运行
      		//alert(iNum01);
      
      		console.log(iNum01);
      
      		oBody.style.backgroundColor = 'gold';
      
      		var iNum02 = 24;
      
      		var iNum03 = 36;
      
      		//alert(iNum02);
      
      		console.log(iNum02);
      
      		setInterval(function(){
      			iNum03++;
      
      			//console.log(iNum03);
      			document.title = iNum03;
      			 
      			 //内部变量不能在consloe.log()输出
      			 //var iNum04++;
      			 //console.log(iNum04++);
      
      		},100)
      	   }
      	</script>
      </head>
      ```

#### 5 弹框 作用

#### 6 复习 倒计时弹框练习

1. 001作用-倒计时弹框.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <style type="text/css">     
              .menu{
                  height:80px;
                  background-color:gold;
                  position:fixed;
                  width:960px;
                  top:0px;
                  left:50%;
                  margin-left:-480px;
              }
              p{
                  text-align:center;
              }
              .popup{
                  width:500px;
                  height:300px;
                  border:1px solid #000;
                  background-color:#fff;
                  position:fixed;
                  left:50%;
                  margin-left:-251px;
                  top:50%;
                  margin-top:-151px;
                  z-index:9999;
              }
              .popup h2{
                  background-color:gold;
                  margin:10px;
                  height:40px;
              }
      
              .mask{
                  position:fixed;
                  width:100%;
                  height:100%;
                  background-color:#000;
                  left:0;
                  top:0;
                  opacity:0.5;
                  z-index:9998;
              }
              .pop_con{
                  display:none;
              }
          </style>
          <script type="text/javascript">
              window.onload = function(){
                  var oPop = document.getElementById('pop_con');
                  var oBtn = document.getElementById('btn');
                  var oPop_tip = document.getElementById('pop_tip');
                  var iTime = 5;
                  oBtn.onclick = function(){
                      oPop.style.display = 'block';
                      /* //定时器和实名函数组合
                      var timer = setInterval(fnDaojishi,1000);
      
                      function fnDaojishi(){                  
                          iTime--;
                          oPop_tip.innerHTML = '还有'+ iTime +'秒钟关闭弹框';
      
                          if(iTime==0)
                          {
                              oPop.style.display = 'none';
                              clearInterval(timer);
                          }
                      }
                      */
                      //定时器, 匿名函数做定时器参数
                      var timer = setInterval(function(){                 
                          iTime--;
                          oPop_tip.innerHTML = '还有'+ iTime +'秒钟关闭弹框';
      
                          if(iTime==0)
                          {
                              oPop.style.display = 'none';
                              clearInterval(timer);
                              iTime=5;
                              oPop_tip.innerHTML = '还有5秒钟关闭弹框';
                          }
                      },1000);   //1000是1000ms, 即1s
                  }
              }
          </script>
      </head>
      <body>
          <div class="menu">菜单文字</div>    
          <div class="pop_con" id="pop_con">
              <div class="popup">
                  <h2>弹框的标题</h2>  
      
                  <p id="pop_tip">还有5秒钟关闭弹框</p>
              </div>
              <div class="mask"></div>
          </div>
      
          <input type="button" name="" value="弹出弹框" id="btn">
      
          <p>网页文字</p>
          <p>网页文字</p>
          <p>网页文字</p>
      </body>
      </html>
      ```

结束.

----


---

总结: 

1.  **内部**函数:
   1.  alert()
   2. parseInt()  //转Int类型,
      1.  html输入的都是 string 类型
2.  内置对象
   1. window对象
      1. window.onload = fnChange()
   2. document对象
3.  style属性
   1.  style 属性
4.  元素/ 元素对象的事件
   1.  onclick 事件
       1.  oBtn01.onclick = function(){ iSpeed=-2;};
   2.  onmouseover
       1.   oDiv.onmouseover = function(){ iSpeed = 0;}
   3.  onmouseout
       1.  oDiv.onmouseout = function(){ iSpeed = iNowspeed;}
5.  获取元素
   1. 方法1 getElementById(),  //获得一个
   2. 方法2 getElements ByTagName(), 标签集,  选择集,  获取多个相同的元素
      1. 不能给选择集设置属性
      2.  for 循环给每个元素设置

6.  javascript的值默认是 字符串 
7.  left, top, 标签相当于父盒的位置.

8.  系统自带的类
   1.  Date();  // 时间 ms
      1.  new Date();  //实例化 时间对象, 
         1. 默认 当前时间, ms单位.
         2. new Date(2021, 3, 30, 24, 0,0);  //设定未来时间
   2.  对应方法: 取 年, 月, 日, 时, 分, 秒
      1. getFullYear(), getMonth(), getDate(), getDay(), getHours(), getMinutes(), getSeconds()

## JavaScript自定义对象的方式

1. JavaScript中，对象

   1. 拥有属性和方法的数据
   2. 自定义对象的方式：直接创建方式，对象初始化器方式，构造函数方式，原型模式，混合模式（原型+构造），动态方式，工厂方式

2. 直接创建方式，

   1. ```
      <script type="text/javascript">
      	var student=new Object();  //直接创建对象
      	student.name="丹",  //对象指定属性,方法
      	student.age=21,
      	student.sex="女",
      	student.doHomework=function(){
      		console.log("正在做作业");
      		console.log(this.name+"正在做作业");
      	}
      	
      	console.log(student.name);
      	student.doHomework();
      </script>

3. 对象初始化器方式，

   1. ```
      <script type="text/javascript">
      	var student={
      		name:"丹",
      		age:21,
      		sex:"女",
      		doHomework:function(){
      		console.log("正在做作业");
      		console.log(this.name+"正在做作业");（this写在对象里边）
      		}
      	};
      	
      	console.log(student.name);
      	student.doHomework();
      </script>

4. 构造函数方式，

   1. ```
      //方式1
      <script type="text/javascript">
      	function Student(){  //定义对象的构造函数
              	this.name="丹",  //this关键字
              	this.doHomework=function(){
              	console.log("正在做作业");
                             console.log(this.name+"正在做作业");
                      }
          }
               
          varstudent = new Student();
          student.doHomework();
      </script>
      ```

   2. ```
      //方式2
      <script type="text/javascript">
      	function Student(){
      		this.name="丹",
      		this.doHomework=homework;
      	}
      	function homework(){
      		console.log(this.name+"正在做作业");
      	}
      	var student = new Student();
            student.doHomework();
      </script>
      ```

   3. 构造函数方式小结:

      1. 和上面两种方式对比，构造函数方式创建对象有效地节省代码
      2. 构造函数方式创建对象，this不能省略，这是和普通函数的区别。
      3. 构造函数方式创建对象，第二种方式更可取，提高了代码的复用性。

5. prototype原型模式，

   1. 声明新函数，函数有一个prototype属性，该属性为对象添加属性和方法。（JavaScript中，函数也是对象）

   2. ```
      <script type="text/javascript">
      	function Student(){}  //构造函数方法构造空对象
      	Student.prototype.name="丹丹";
      	Student.prototype.age=22;
      	Student.prototype.doHomeWork=function(){
      		console.log(this.name+"正在做作业");
      	};
      	
      	
      	var student = new Student();
      	student.doHomeWork();
      </script>

6. 混合模式（原型+构造）——最常用

   1. 构造函数定义对象属性，原型式定义对象方法 

   2. ```
      <script type="text/javascript">
      	function Student(){
      		this.name="丹丹";
      		this.age=22;
      	}
      	Student.prototype.doHomeWork=function(){
      		console.log(this.name+"正在做作业");
      	};
      	var student = new Student();
      	student.doHomeWork();
      </script>
      ```

   3. 小结

      1. 构造函数方式便于动态为属性赋值，但这种方式将方法定义在了构造方法体中，代码比较杂乱
      2. 原型式实现了属性和方法分离，但不便于为属性动态赋值
      3. 取长补短——构造方法定义对象属性，原型方式定义对象方法。

7. 动态方式，

   1. ```
      <script type="text/javascript">
      		function Student(name, age){  //构造函数式
      			this.name = name;    
      			this.age = age;    
      			this.friends = ["嗯哼","哼嗯"];
      			if(typeof this.sayName!="function"){    
      				Student.prototype.sayName = function(){     
      					alert(this.name);           
      				}        
      			}
      		}
      		
      		var student = new Student("dan",22);
      		student.sayName();
      </script>

8. 工厂方式

   1. ```
      function Student(name, age, job){ 
      	var o = new Object();
      	o.name = name; 
      	o.age = age; 
       	o.sayName = function(){        
      		alert(this.name);    
      	}
      	return o;
      }
      var student = new Student("dan",22);