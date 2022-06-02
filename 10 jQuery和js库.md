**做到极致, 承受不能承受之重.**

#### 总结:

1. jQuery事件函数

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
          	//定义 JS函数
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
      
              //定义jQuery函数, jQuery调用函数
              $(function(){
                  //jQuery事件函数: jQuery对象.事件函数名(事件处理函数(){})
                  $('#input01').keyup(function(){
                      alert('获得焦点');
                      a(b);
                      a(c); 
                  })
              })
          </script>
      </head>
      <body>
      <input type="text" id="input01">
      </body>
      ```

2. css通配符: 

   1. `^` 前匹配, `$`后匹配, `*` 任意匹配
   2. [attr^="val"] , [attr$="val"] , [attr*="val"]

3. bootcss网站: `https://v3.bootcss.com/css/`, 或 从网站:`v3.bootcss.com/css/#tables`寻找资源

4. jQuery的选择器的参数都是字符串类型, 必须有`""`

   1. ```
      $("#id")            //ID选择器
      $("div")            //元素选择器
      $(".classname")     //类选择器
      $(".classname,.classname1,#id1")     //组合选择器
      ```

5. `$.each()`，`$().each()`和`forEach()`

   1. `$.each(parentData,function(index,childData){})`

      1. 这个方法， 集合数据用的较多

      2. | 参数                    | 描述                                                         |
         | ----------------------- | ------------------------------------------------------------ |
         | function(index,element) | 必需。为每个匹配元素规定运行的函数。index：选择器的index位置；element：当前的元素(也可使用‘this’选择器) |

      3. ```
         //遍历JSON
         var json=[{"name":"limeng","email":"xfjylimeng"},{"name":"hehe","email":"fjylimeng"}]
         $.each(json,function(index,data){
         	alert("索引:"+index+","+"对应值为："+data.name);
         });
         ```

      4. ```
         //遍历MAP
         var obj = { one:1, two:2, three:3, four:4, five:5 };
         $.each(obj, function(key, val) {
         	alert(obj[key]);
         });
         ```

      5. ```
         //遍历list
         var arr1 = [ "one", "two", "three", "four", "five" ];
         $.each(arr1, function(){
         	alert(this);
         });
         输出：one   two  three  four   five
         ```

      6. ```
         //遍历数组
         var arr2 = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
         $.each(arr2, function(i, item){
         	alert(item[0]);
         });
         输出：1   4   7
         ```

      7. ```
         //遍历JSON并取数据进行操作
         $.each(countyJson, function(index, data) {
            if (data.parent == selValue) {
                var option = "<option value='" + data.id + "'>" + data.county + "</option>";
                $("#selDistrict").append(option);
            }
         });
         ```

   2. array.forEach(),  用于调用数组的每个元素，并将元素传递给回调函数

      1. 是JS中遍历数组的，不能遍历对象. 遍历对象用 for in，或者将对象转化为数组，用[].slice.call(jQuery实例对象)；

      2. foreach的回调函数function有三个参数，第一个是val为数组当前的值，第二个index为当前值的下标，第三个arr为原数组；
         forEach()中没有返回值；

      3. forEach() 对于空数组是不会执行回调函数的

      4. ```
         //语法格式
         array.forEach(function(currentValue, index, arr), thisValue)
         ```

         

   3. `$().each(function(){})`-DOM处理

      1. 页面有多个input标签类型为checkbox， 这时用`$().each` 处理多个checkbook

      2. ```
         $("input[name='ch']").each(function(index){
         	if($(this).attr("checked")==true){
         		//一些操作代码
         	}
         })
         ```

      3. 回调函数是可以传递参数，index就为遍历的索引

      4. 



## 01-jQuery介绍 和jQuery选择器

#### 01 jquery介绍-jquery加载

1. jquery

   1. js函数库, 46%使用率. 不用原生js.

   2. jquery 用js 写的

   3. 版本:

      1. 1.x 低版本浏览器

      2. 2.x, 3.x 高版本浏览器

      3. ```
         jquery-1.12.4.js  未压缩版
         jquery-1.12.4.min.js  压缩版的, 用压缩版就可以了. 功能一样的
         ```

         1.  封闭函数写法.
         2. 混淆, 

2. $函数

   1. `typedef($)`,  返回function 类型,  使用 `$` 应该写成 `$()`;  函数 有参数

   2. $函数的参数

      1. function 做参数

         1. ```
            $(function(){ undefined });
            ```

      2. document 对象 做参数

         1. `$(document).ready(function(){undefined});`

      3. 字符串 做参数

         1. `$("div"),  $("#btn"),  $(".class")`,  与css对应的

3. $()函数的7种用法

   1. 前言: 

      1. jQuery对象是**类数组**的对象，有连续的整形属性及一系列的jQuery方法。
      2. 所有的操作包装在jQuery()函数中，形成统一(惟一)的操作入口。
      3. 函数$()或jQuery()，调用根据传入的参数的不同而达到不同的效果。

   2. 7种用法

      1. jQuery(selector,context), 如 `$('span')`, `$(css选择器)`

         1. 参数:

            1. 一个css选择器表达式(selector)
            2. 可选的选择器上下文(context),
               1. 默认查找从**根元素ducument对象**开始，也就是说查找范围是整棵文档树
               2. 给定了上下文context，则在指定上下文中查找

         2. 返回值: 返回一个包含了匹配的DOM元素的 jQuery对象

         3. 示例

            1. html

               1. ```
                  <span>body span</span>
                  <span>body span</span>
                  <span>body span</span>
                  <div class="wrap">
                  	<span>wrap span</span>
                  	<span>wrap span</span>
                  	<span>wrap span</span>
                  </div>
                  ```

            2. js

               1. ```
                  $('span').css('background-color','red');//所有的span变红
                  $('span','.wrap').css('background-color','red');//只.wrap的span变红
                  ```

      2. jQuery(html,ownerDocument) 、jQuery(html,props).  如 `$('<div>')`

         1. 作用: 用提供的html代码创建DOM元素

         2. jQuery(html,ownerDocument)

            1. 参数:

               1. html: 单标签或多层标签的嵌套
               2. ownerDocument: 用于创建新DOM元素的**文档对象**，不传入则默认为当前的文档对象

            2. 示例

               1. ```
                  //单标签  两种方式都可以往body中插入div
                  
                  1  $('<div>').appendTo('body');
                  2  $('<div></div>').appendTo('body');  
                  
                  // 多标签嵌套
                  $('<div><span>dfsg</span></div>').appendTo('body');
                  ```

      3. jQuery(html,props),  **该用法有待考证**

         1. 参数:

            1. props: 一个包含属性和事件的普通的对象

         2. 示例

            1. ```
               $('<div>我是div</div>',{
               	title:'我是新的div',
               	click:function(){
               	$(this).css('color','red');
               	console.log(this);
               	}
               }).appendTo('body');
               ```

   3. jQuery(element or elementsArray),  如: `$('li')`

      1. 参数

         1. 传入一个DOM元素或DOM元素的数组

      2. 返回值: 把DOM元素封装到jQuery对象中, 并返回jQuery对象

      3. html

         1. ```
            <ul>
            	<li>1</li>
            	<li>2</li>
            	<li>3</li>
            	<li>4</li>
            	<li>5</li>
            </ul>

      4. js: 传入DOM元素

         1. ```
            // 传入DOM元素
            $('li').each(function(index,ele){
            	$(ele).on('click',function(){
            		$(this).css('background','red');//这里的DOM元素是this
            })
            ```

      5. js: 传入DOM数组

         1. ```
             //传入DOM数组
             var aLi=document.getElementsByTagName('li');  //DOM数组,对象集
             aLi=[].slice.call(aLi);//集合转数组
             var $aLi=$(aLi);  //传入DOM数组, 赋值给变量 $ALi
             $aLi.html('我是jQuery对象');//所有的li的内容都变成'我是jQuery对象'
            ```

   4. jQuery(object) ,  如: `$(obj)`

      1. 参数: 一个object对象，

      2. 返回值: object对象封装到jQuery对象中并返回 jQuery对象

      3. 示例

      4. ```
         var obj={name:'谦龙'};
         var $obj=$(obj);//封装成jQuery对象, 赋值给变量$obj
         //绑定自定义事件
         $obj.on('say',function(){
         	console.log(this.name)//输出谦龙
         	});
         $obj.trigger('say');
         ```

   5. jQuery(callback) ,  如` $(函数名(){})`

      1. 参数: 是函数, 在`document`对象上绑定一个ready事件监听函数，DOM结构加载完成时执行

      2. 返回值 

      3. 示例:

         1. ```
            $(function(){
            
            })
            
            //以上代码和下面的效果是一样的
            $(document).ready(function(){
            ...//代码
            })	
            ```

   6. jQuery(jQuery object)   , 如:   `$(jQuery 对象)`

      1. 参数: 一个jQuery对象

      2. 返回值: 创建该jQuery对象的一个副本并返回该对象。副本与传入的jQuery对象引用完全相同的元素

      3. 示例

         1. ```
            var aLi=$('li');
            var copyLi=$(aLi);//创建一个aLi的副本
            console.log(aLi);
            console.log(copyLi);
            console.log(copyLi===aLi);
            ```

   7. jQuery(),  如`$()`

      1. 不传任何的参数
      2. 返回值: 返回一个空的jQuery对象，属性length为0
      3. 功能: 复用jQuery对象
         1. 如创建一个空的jQuery对象，需要时先手动修改元素，然后再调用jQuery方法。避免重复创建jQuery对象。

4. jQuery中的  `$`和 `$()`区别

   1. `$`和 `$()`区别
      1. `$` 是 jQuery 的别名, 
         1. jQuery库还提供了另外的机制对 jQuery函数起别名
      2. `$()` 是 `jQuery()` , 里面传参数, 作用: 获取元素,  返回 Object对象
         1. jQuery()是 jQuery库的一个函数
         2. 这个函数根据()里的参数查找和选择html文档中的元素.  
            1. 作用之一:  代替 getElementById()
            2. 如: `$(document) ` 选取整个文档对象
      3. `$`调用不需要操作对象的方法（像java代码中的静态方法，不用声明对象就可以操作），如`$.prototype`. 如对象赋值（注意，这里有操作对象），要用`$('#abc').val('123')`的方式，val()方法用到`$(this)` 操作当前对象
   2. `$.post(),  $.get(), $.ajax()` 是jQuery对象的方法
   3. jQuery的工厂函数 $(),  
      1. 以$为开始，引出整个jQuery的架构
      2. 它本质上是一个DOM对象，但它的方法都封装在了jQuery上

5. 使用

   1. 加载

      1. `<script  type="text/javascript" src="js/jquery-1.12.4.min.js"></script>`

   3.  002jquery加载.html

      1. ```
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript">
         		// 原生js方法
         		window.onload = function(){
         			var oDiv = document.getElementById('div1');
         			alert('原生弹出的'+oDiv);
         		}
         		
         		//jquery对象的ready()方法, 比window.onload快
         		//原因: window.onload是等标签加载完后，再渲染完,之后才运行，
         		// ready: 标签加载完后就运行
         
         		//1. ready()的完整写法：
         		$(document).ready(function(){
         			//声明以$开头的变量 $div
         			var $div = $('#div1');
         			alert('jquery弹出的'+$div);
         
         		})
         	
         		//2. ready的简写,  省略了 (document).ready
         		$(function(){
         			var $div = $('#div1');  
         			alert('jquery弹出的'+$div);
         		})
         	</script>
         ```
   
6. jQuery编程方式

   1. 结构:

      1. 一个 `<script></script> `引入 jquery
      2. 再一个`<script></script> `编写 jQuery

   2. 不能在jquery的引入标签里写js代码，要另一个script标签里面写js代码

      1. 否则, 报错: `Failed to load resource: net::ERR_FILE_NOT_FOUND`

      2. ```
         //此处报错
         <head>
             <meta charset="UTF-8">
             <title>Document</title>
             <!--不能在jquery的引入标签里写js代码，要另一个script标签里面写js代码   -->
             <script type="text/javascript" src="js/jquery-1.12.4.min.js">
                 
                 $(function(){
                     alert(5)
         
                 })
         
             </script>
         
         </head>
         <body>
             <div id="div1">div元素</div>
         </body>

7. jQuery比 js 快 原理:

   1. ready 页面节点加载完就执行
   2. onload , 节点加载完, 再渲染完再执行.  或再数据加载完再执行.

#### 02jquery选择器01

1.  jquery用法**思想**

   1.  思想一: 选择某个网页**元素**，然后操作它
   2.  思想二: 同一个函数 取值 赋值
   
2. jquery选择器与 css样式相同.

   1. jquery选择器

      1. 单个元素选择器

         1. ```
            $('#myId') //选择id为myId的元素, 生成对象
            $('.myClass') // 选择class为myClass的元素, 生成对象
            $('li') //选择所有的li元素, 生成对象
            $('#ul1 li span') //选择id为为ul1元素下的所有li下的span元素, 生成对象
            $('input[name=first]') // 选择name属性等于first的input元素, 生成对象
            ```

         2. 规则和css样式相同

            1.  单引号, 双引号, 都可以,  `$('#myId')` 与 `$("#myId")` 相同
            2. `$('#myId') , $('.myClass'), 带有符号 #, .`

         3. 用length属性判断是否选择成功

      2. 选择集 过滤

         1. ```
            $('div').has('p');   // 包含p元素的div元素
            $('div').not('.myClass');    //class不等于myClass的div元素
            $('div').filter('.myClass'); //class等于myClass的div元素
            $('div').eq(5);   //第6个div元素
            ```

      3. 选择集 转移

         1. ```
            $('div').prev();    // div元素之前紧挨的同辈元素
            $('div').prevAll(); // div元素之前所有的同辈元素
            $('div').next();    // div元素之后紧挨的同辈元素
            $('div').nextAll(); // div元素之后所有的同辈元素
            $('div').parent();  // div的父元素
            $('div').children(); // div的所有子元素
            $('div').siblings(); // div的同级元素
            $('div').find('.myClass'); // div内 class等于myClass的子元素
            ```
            
         2. 转移函数中添加选择条件:

            1. ```
               $('div').children('ul');
               ```

      4. 判断是否选择 元素

         1.  jquery 容错机制， 没有找到元素，也不 出错
         2. length 判断是否找到了元素 
            1. length等于0，就是没选择到元素
            2. length大于0，就是选择到了元素

   2. jQuery可批量操作 标签集,  js不可以

   3. jquery流程

      1. 先获取元素(标签)
      2. 再做事情.



2. 004 jquery选择器.html

   1. ```
      <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      <script type="text/javascript">		
      	$(function(){
      
      		var $div = $('#div1');  //生成jQuery对象, 付给变量$div
      		$div.css({'color':'red'});
      
      
      		var $div2 = $('.box');
      		$div2.css({'color':'green'});
      
      
      		var $li = $('.list li');
      		// 带“-”的样式属性，可以写成驼峰式，也可以写成原始
      		$li.css({'background-color':'pink','color':'red'});
      
      	});
      </script>
      
      <body>
      	<div id="div1">这是一个div元素</div>
      
      	<div class="box">这是第二个div元素</div>
      	<div class="box">这是第三个div元素</div>
      
      	<!-- ul.list>li{$}*8 -->
      
      	<ul class="list">
      		<li>1</li>
      		<li>2</li>
      		<li>3</li>
      		<li>4</li>
      		<li>5</li>
      		<li>6</li>
      		<li>7</li>
      		<li>8</li>
      	</ul>
      
      </body>
      ```

      1.  jQuery对象 设置css样式: `jQuery对象.css({键:值}); `

3. 005选择器过滤和转移.html

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              $(function(){
      
                  /*
                  var $div = $('div');
                  $div.css({'backgroundColor':'gold'});
                  */
                  //只用一次, 不用保存
                  $('div').css({'backgroundColor':'gold'});
                  
                  //有p元素的div元素的对象设置属性
                  $('div').has('p').css({'fontSize':'30px', 'color':'red'});
                  //下标从0 开始 , 4 是第5个
                  $('div').eq(4).css({'textIndent':'20px'});
                  //div的 class="tip"的子元素
                  $('div').find('.tip').css({'fontSize':'30px'});
              })
          </script>
      </head>
      <body>
          <div>1</div>
          <div>292<p>2</p></div>
          <div>3</div>
          <div>4</div>
          <div>5</div>
          <div>6</div>
          <div>7</div>
          <div><span>8</span><span class="tip">9</span></div>
      </body>
      ```
      
      1. 过滤方法:
         1. has('p') 是父元素包括的元素p都设置属性
         2. find('.tip'), 是 find开始的子元素.tip开始设置属性
         3. .eq(4),  下标4

4. $div2.length;  // 判断是否选中元素了.

5.  jquery, 没有选中, 不报错

## 02-样式操作和click事件

#### 01样式的操作

1.  jquery思想二: 用同一个函数取值, 赋值

1.  获取样式, 设置样式

   1. css() 方法

      2. ```
         // 获取div的样式
         $("div").css("width");
         $("div").css("color");
         
         //设置div的样式
         $("div").css("width","30px");   //方式1, 一个属性
         $("div").css("height","30px");
         $("div").css({fontSize:"30px",color:"red"});  //方式2: 多个属性
         ```
      
   2. 007css方法.html
   
      1. ```
         <head>
             <meta charset="UTF-8">
             <title>Document</title>
             <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
             <script type="text/javascript">
                 $(function(){
                     var $div = $('#box');
                     // 读取属性值
                     var sTr = $div.css('color');
                     alert(sTr);
         
                     // 写属性值            $div.css({'color':'red','backgroundColor':'pink','fontSize':'24px'});
                    
                     // 原生js无法读取行间没有定义的css属性值
                     var oDiv = document.getElementById('box');
                     alert(oDiv.style.fontSize);
                 })
         
             </script>
         </head>
         <body>
             <div id="box">div元素</div>
         </body>
         ```
   
2. 操作类名

   1. 类名关联样式, 添加/删除类名实际是添加/删除样式
   
   2. ```
      $("#div1").addClass("divClass2") // id为div1的对象 追加 类名divClass2, 没有.
      $("#div1").removeClass("divClass")  //id为div1的对象 移除 类名divClass
      $("#div1").removeClass("divClass divClass2") //移除类名
      $("#div1").toggleClass("anotherClass") //切换 类名anotherClass
      ```

      1. addClass, removeClass, toggleClass 三个函数的参数前没有`.`,  只写类名字符串, 有引号. 对比: 
         1. `$(".类名")` 中类名前有 `.`,  有引号
         2. html中类名字符串前没有 `.`, 有引号.  
         3. css中的类名前有`.` 没有引号
   
   3. 008类名操作.html
   
      1. ```
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         
         			var $div = $('.box');
         
         			// 在原来样式名的基础上加上“big”的样式名
         			$div.addClass('big');
         
         			// 在原来样式名的基础上去掉“red”的样式名, 其他类不变, 没有.号
         			$div.removeClass('red');
         			
         		})
         
         	</script>
         	<style type="text/css">
         		
         		.box{
         			width:100px;
         			height:100px;
         			background-color:gold;
         		}
         
         		.big{
         			font-size:30px;
         		}
         
         
         		.red{
         			color:red;
         		}
         
         
         	</style>
         </head>
         <body>
         	<div class="box red">div元素</div>
         </body>
         ```
   

#### 02 click事件

1. 元素绑定click事件

   1. 绑定: $('#btn').click(function(){});    // 对象.click(function(){});

   2. 009绑定click事件.html

      1. ```
         <head>
             <meta charset="UTF-8">
             <title>Document</title>
             <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
             <script type="text/javascript">
                 $(function(){
                     
                     // 绑定click事件
                     $('#btn').click(function(){
         
                         /*
                         if($('.box').hasClass('col01'))
                         {
                             $('.box').removeClass('col01');
                         }
                         else
                         {
                             $('.box').addClass('col01');
                         }*/
         
                         //简化成下面的写法：  toggleClass切换类名               
                         $('.box').toggleClass('col01');
                     })
         
         
                 })
         
             </script>
         </head>
         <style type="text/css">
             .box{
                 width:200px;
                 height:200px;
                 background-color:gold;
             }
             .col01{
                 background-color:green;
             }
         
         </style>
         <body>
             <input type="button" name="" value="切换样式" id="btn">
             <div class="box">div元素</div>
         </body>
         ```
      
   3. jquery与css获取标签的方式一样的

      1.  $('#btn'), 写#号, 获取对象, 单引号, 双引号都可以.
   2. $('.box'),  获取类标签对象

#### 03 元素的索引值 -- 选项卡

1. 元素的索引值

   1.  $('.list li').index()    //jQuery对象.index();  返回值: 元素的索引值, 或元素数组的第一个元素的索引值

   2. 010获取元素索引值.html

      1. ```
         <head>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         
         			var $li = $('.list li');
         
         			//alert($li.length); 弹出 8
         			//alert($li.eq(3).index()); 弹出3
         
         			alert( $li.filter('.myli').index() );
         		})	</script>
         </head>
         <body>
         	<ul class="list">
         		<li>1</li>
         		<li>2</li>
         		<li>3</li>
         		<li>4</li>
         		<li class="myli">5</li>
         		<li>6</li>
         		<li>7</li>
         		<li>8</li>
         	</ul>
         </body>
         ```
      
         1. jQuery对象的选择函数 .has(), .eq(),  .filter(),  .find()

   3.  011选项卡.html
   
      1. ```
         <head>
             <meta charset="UTF-8">
             <title>Document</title>
             <style type="text/css">
                 .btns input{
                     width:100px;
                     height:40px;
                     background-color:#ddd;
                     border:0;
                 }
         
                 .btns .current{
                     background-color:gold;
                 }
         
         
                 .cons div{
                     width:500px;
                     height:300px;
                     background-color:gold;
                     display:none;
                     text-align:center;
                     line-height:300px;
                     font-size:30px;
                 }
         
                 .cons .active{
                     display: block;
                 }
         
             </style>
             <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
             <script type="text/javascript">
                 $(function(){
                     var $btn = $('.btns input');  //标签集对象$btn
                     var $div = $('.cons div');   //标签集对象$div
         
                     $btn.click(function(){
                         //this:原生的this，当前点击的对象，jquery的当前对象 $(this)
         
                         //siblings(): 兄弟方法, 返回被选元素的所有同级元素
                         //当前点击的按钮加上current样式，它的兄弟按钮去掉current样式
                         $(this).addClass('current').siblings().removeClass('current');
         
                         alert( $(this).index()) ; //弹出当前按钮的索引值
                         // 当前点击的按钮对应索引值的div加上active样式，其他的去掉active样式
                         $div.eq( $(this).index() ).addClass('active').siblings().removeClass('active');
                     })
                 })
         
             </script>
         </head>
         <body>
             <div class="btns">
                 <input type="button" name="" value="01" class="current">
                 <input type="button" name="" value="02">
                 <input type="button" name="" value="03">
             </div>  
             <div class="cons">
                 <div class="active">选项卡一的内容</div>
                 <div>选项卡二的内容</div>
                 <div>选项卡三的内容</div>
          </div>
         </body>
      
      2. this 是 js 的当前点击对象,.  jquery 用$(this) 获取当前点击对象.

## 03-jQuery特效和链式调用

#### 01jquery特效 -- 显示/隐藏

1. 函数

   1.  $(selector).fadeIn(speed,callback)    ,  淡入效果来显示被选元素，假如该元素是隐藏的

      1.  参数

         1.  speed	可选。规定元素从隐藏到可见的速度。默认为 "normal"。可能的值：毫秒 （比如 1500）"slow" "normal" "fast" 在设置速度的情况下，元素从隐藏到可见的过程中，会逐渐地改变其透明度（这样会创造淡入效果）
         2.  callback	可选。fadeIn 函数执行完之后，要执行的函数。

         

      2.  fadeIn()  淡入/显示

      3.  $('#div').fadeIn(1000, 'swing', function(){});   // 对象.方法(参数);

   2.  $(selector).fadeOut(speed,callback)

      1.  参数
         1.  speed。规定元素从隐藏到可见的速度。默认为 "normal"。可能的值：毫秒 （比如 1500）"slow" "normal" "fast" 在设置速度的情况下，元素从隐藏到可见的过程中，会逐渐地改变其透明度（这样会创造淡入效果）
         2.  callback	可选。fadeIn 函数执行完之后，要执行的函数。

   3.  fadeToggle() 切换淡入淡出

   4.  hide( [duration] [, easing] [, complete]) 隐藏匹配的元素

      1.  duration：一个字符串或者数字决定动画将允行多久，如果不传第一个参数，但是传了后面的参数字符串 'fast' 和 'slow'，分别代表200和600毫秒
      2.  easing：一个字符串，过度缓动函数（默认：swing）.  jQuery提供"linear"和"swing"，其他过渡效果可以使用相关的插件完成
      3.  complete：在动画执行时完成的函数，函数内部的this指向指向执行动画的DOM元素

   5.  show([duration] [, easing] [, complete]) 显示元素

   6.  toggle() 切换隐藏可见状态

      1.  toggle( [duration] [, easing] [, complete] )
      2.  toggle(Boolean) 一个布尔值，使用 true 来显示元素，或 false 隐藏它

   7.  slideDown( [duration] [, easing] [, complete] ),  用滑动动画显示匹配元素

      1.  slideDown(speed,callback)  展开, callback可选。slideDown函数执行完之后，要执行的函数。

   8.  slideUp()  用滑动动画隐藏匹配元素

      1.  slideUp( [duration] [, easing] [, complete] )

   9.  slideToggle( [duration] [, easing] [, complete] ) , 滑动动画显示或隐藏匹配元素

      1.  slideToggle(speed,callback) 滑动效果（高度变化）切换元素的可见状态. callback可选。toggle 函数执行完之后，要执行的函数。

      

      

2. 012 特殊效果.html

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <style type="text/css">
              
              .box{
                  width:300px;
                  height:300px;
                  background-color:gold;
                  display:none;
              }
          </style>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              
              $(function(){
      
                  $('#btn').click(function(){
      				//$('.box') 获取.box的元素对象
                      $('.box').fadeIn(1000,function(){
                          alert('1 动画完了！');
                      });
                      
      
                      $('.box').fadeToggle(1000,function(){
                          alert('2 动画完了！');
                      });
                      
      
                      $('.box').show(2000);
      
                      // $('.box').toggle();
      
                      // $('.box').slideDown();
      
                      $('.box').slideToggle(1000,function(){
                          alert('3 动画完了')
                      });
                  })
              })
          </script>
      </head>
      
      <body>
          <input type="button" name="" value="动画" id="btn">
          <div class="box"></div>
      </body>
      ```

#### 02链式调用-特效-层级菜单

1. jquery对象的方法 执行完后返回 jquery对象

   1. jquery对象的方法连起来写

   2. ```
      $('#div1') // 返回值: id为div1的元素的对象
      .children('ul') //div1的孩子是ul子元素
      .slideDown('fast') //高度从零变到实际高度来显示ul元素
      .parent()  //跳到ul的父元素，即id为div1的元素
      .siblings()  //跳到div1元素平级的所有兄弟元素
      .children('ul') //兄弟元素中的子元素ul
      .slideUp('fast');  //滑动隐藏子元素ul
      ```
      
   3. 过滤函数 children(), find() 区别

      1. children() 方法返回元素的所有直接子元素 （只找儿子不要孙子（即:不会递归去遍历）
      2. find() 方法获得当前元素集合中每个元素的后代 （注意 find()方法，必须传参数，否者无效）

2. 层级菜单

   1. 层级菜单.html

      1. ```
         <head>
             <meta charset="UTF-8">
             <title>层级菜单</title> 
             <style type="text/css">
                 body{
                     font-family:'Microsoft Yahei';
                 }
                 body,ul{
                     margin:0px;
                     padding:0px;
                 }
                 
                 ul{list-style:none;}
         
                 .menu{
                     width:200px;
                     margin:20px auto 0;
                 }
         
                 .menu .level1,.menu li ul a{
                     display:block;
                     width:200px;
                     height:30px;
                     line-height:30px;
                     text-decoration:none;
                     background-color:#3366cc;
                     color:#fff;
                     font-size:16px;
                     text-indent:10px;           
                 }
         
                 .menu .level1{
                     border-bottom:1px solid #afc6f6;
                     
                 }
         
                 .menu li ul a{
                     font-size:14px;
                     text-indent:20px;
                     background-color:#7aa1ef;
                             
                 }
         
                 .menu li ul li{
                     border-bottom:1px solid #afc6f6;
                 }
                 
         
                 .menu li ul{
                     display:none;
                 }
         
                 .menu li ul.current{
                     display:block;
                 }
         
                 .menu li ul li a:hover{
                     background-color:#f6b544;
                 }
             </style>    
             <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
             <script type="text/javascript">
                 $(function(){
                     $('.level1').click(function(){
                         //当前点击的元素紧挨的同辈下一个元素向下展开，再跳到此元素的父级(li),再跳到父级的其他的同辈元素(li),选择同辈元素(li)的子元素ul，然后将它向上收起。
         
                         //stop()方法修正反复点击导致的持续动画的问题
                         $(this).next().stop().slideToggle().parent().siblings().children('ul').slideUp();
                     })
                 })
             </script>
         </head>
         <body>
             <ul class="menu">
                 <li>
                     <a href="#" class="level1">水果</a>
                     <ul class="current">
                         <li><a href="#">苹果</a></li>
                         <li><a href="#">梨子</a></li>
                         <li><a href="#">葡萄</a></li>
                         <li><a href="#">火龙果</a></li>
                     </ul>
                 </li>
                 <li>
                     <a href="#" class="level1">海鲜</a>
                     <ul>
                         <li><a href="#">蛏子</a></li>
                         <li><a href="#">扇贝</a></li>
                         <li><a href="#">龙虾</a></li>
                         <li><a href="#">象拔蚌</a></li>
                     </ul>
                 </li>
                 <li>
                     <a href="#" class="level1">肉类</a>
                     <ul>
                         <li><a href="#">内蒙古羊肉</a></li>
                         <li><a href="#">进口牛肉</a></li>
                         <li><a href="#">野猪肉</a></li>                
                     </ul>
                 </li>
                 <li>
                     <a href="#" class="level1">蔬菜</a>
                     <ul>
                         <li><a href="#">娃娃菜</a></li>
                         <li><a href="#">西红柿</a></li>
                         <li><a href="#">西芹</a></li>
                         <li><a href="#">胡萝卜</a></li>
                     </ul>
                 </li>
                 <li>
                     <a href="#" class="level1">速冻</a>
                     <ul>
                         <li><a href="#">冰淇淋</a></li>
                         <li><a href="#">湾仔码头</a></li>
                         <li><a href="#">海参</a></li>
                         <li><a href="#">牛肉丸</a></li>
                     </ul>
                 </li>
                 
             </ul>
         </body>
         ```
         
      2. `$(this).next()与$(this).children()`
      
         1. `$(this).next()`  当前元素同级的下个元素，而非子元素
         2. `$(this).children()` 当前元素的下一级元素的集合， 子元素的集合，不管子元素的后代元素
      
         
      
      3. stop() 方法: 修正反复点击导致的持续动画的问题
      
         1. ```
            $(this).next().stop().slideToggle().parent().siblings().children('ul').slideUp();
            ```

## 04-jQuery animate动画

#### 01滑动选项卡

1.  001滑动选项卡.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<style type="text/css">
      		.btns input{
      			width:100px;
      			height:40px;
      			background-color:#ddd;
      			border:0;
      		}
      
      		.btns .current{
      			background-color:gold;
      		}
      
      		.cons{
      			width:500px;
      			height:300px;
      			overflow:hidden;  // 对子盒切割
      			position:relative;  //父盒
      		}
      		
      		.slides{
      			width:1500px;
      			height:300px;
      			position:absolute;  //子盒
      			left:0;
      			top:0;
      		}
      		
      		.cons .slides div{
      			width:500px;
      			height:300px;
      			background-color:gold;
      			display:none;
      			text-align:center;
      			line-height:300px;
      			font-size:30px;
      			float:left;
      		}
      
      		.cons .active{
      			display: block;
      		}
      
      	</style>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      			var $btn = $('.btns input');  //标签集对象$btn,
      			var $slides = $('.cons .slides');   //标签集对象$slides,
      
      			$btn.click(function(){
      				// this 是原生js的this，当前点击的对象. 获取jquery的对象用$(this)
      
      				// 当前点击的按钮加上current样式后，除了当前，其他的按钮去掉current样式
      				$(this).addClass('current').siblings().removeClass('current');
      				
      				//$slide.css({'left':-500*$(this).index()});  //css方法
      				$slide.stop().animate({'left':-500*$(this).index()})  //动画方法
      			})
      		})
      	</script>
      </head>
      <body>
      	<div class="btns">
      		<input type="button" name="" value="01" class="current">
      		<input type="button" name="" value="02">
      		<input type="button" name="" value="03">
      	</div>	
      	<div class="cons">
      		<div class="slides">
                  <div class="active">选项卡一的内容</div>
                  <div>选项卡二的内容</div>
                  <div>选项卡三的内容</div>
              </div>
      	</div>
      </body>
      ```

#### 02 animate

1.  $(selector).animate({params},speed,callback);

   1.  参数:
      1.  params: 必需的,定义动画的 CSS 属性。
      2.  speed : 可选的,时长。值："slow"、"fast" 或毫秒。
      3.  callback : 可选的,  动画完成后执行的函数名。
   
2. 013 animate动画.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              $(function(){
      
                  $('#btn').click(function(){
                      $('.box').animate({'width':600},1000,function(){
                          $('.box').animate({'height':400},1000,function(){
                              $('.box').animate({'opacity':0});
                          });
                      });
                  })
                  $('#btn2').click(function(){
                      $('.box2').stop().animate({'width':'+=100'});
                  })
              })
          </script>
          <style type="text/css">
              .box,.box2{
                  width:100px;
                  height:100px;
                  background-color:gold;
              }
          </style>
      </head>
      <body>
          <input type="button" name="" value="动画" id="btn">
          <div class="box"></div>
      
          <br />
          <br />
          <input type="button" name="" value="动画" id="btn2">
          <div class="box2"></div>
      </body>
      </html>			
      ```
      
      

#### 03 手风琴

1. ```
   <head>
   <meta charset="utf-8">
   <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
   <style>
       *{margin:0;padding:0;}
       body{font-size:12px;}
       #accordion{width:727px; height:350px; margin:100px auto 0 auto; position:relative; overflow:hidden; border:1px solid #CCC;}
       #accordion ul{list-style:none;}
       #accordion ul li{width:643px;height:350px; position:absolute; background:#FFF;}
       #accordion ul li span{display:block;width:20px; height:350px; float:left; text-align:center; color:#FFF; padding-top:5px; cursor:pointer;}
       #accordion ul li img{display:block; float:right;}
       .bar01{left:0px;}
       .bar02{left:643px;}
       .bar03{left:664px;}
       .bar04{left:685px;}
       .bar05{left:706px;}
       .bar01 span{background:#09E0B5;}
       .bar02 span{background:#3D7FBB;}
       .bar03 span{background:#5CA716;}
       .bar04 span{background:#F28B24;}
       .bar05 span{background:#7C0070;}
   </style>
   
   <script type="text/javascript">
   
       $(function(){
           $('#accordion li').click(function() { 
               $(this).animate({left:$(this).index()*21}); 
               $(this).prevAll().each(function(){ 
                   $(this).animate({left:$(this).index()*21});
               });
   
               $(this).nextAll().each(function(){
                   $(this).animate({left:(727-(5-$(this).index())*21)});
               });
           });
       })
   </script>
   
   
   <title>手风琴效果</title>
   </head>
   <body>
   <div id="accordion">
       <ul>
       <li class="bar01"><span>非洲景色01</span><img src="images/001.jpg" /></li>
       <li class="bar02"><span>非洲景色02</span><img src="images/002.jpg" /></li>
       <li class="bar03"><span>非洲景色03</span><img src="images/003.jpg" /></li>
       <li class="bar04"><span>非洲景色04</span><img src="images/004.jpg" /></li>
       <li class="bar05"><span>非洲景色05</span><img src="images/005.jpg" /></li>
       </ul>
   </div>
   </body>
   ```

#### 04 jquery常用方法之next()、prev()、nextAll()、prevAll()方法

1. next ( ) :  被选元素的下一个且同级元素节点；

   1. next ( )中添加条件，如：选出div下一个且必须是p标签的元素

   2. ```
        <div class="div1"></div>
        <p class="p1"></p>
        <div class="div2"></div>
      <script>
             $(".div").next("p")
      </script>
      ```

2. prev ( ) :  被选中元素的上一个且同级元素节点；

3. nextAll ( ):  被选中元素的下面的所有同级元素节点；

   1. nextAll( )中可添加条件，如下：

      1. 选出input下边所有且type="checkbox"属性的元素，（一个全选功能，关于prop的用法上一篇文章有介绍）：

      2. ```
         
         全选<input type="checkbox"><br>
         财富<input type="checkbox">
         地位<input type="checkbox">
         权利<input type="checkbox">
          <script>
              $("input[type='checkbox']").eq(0).click(function(){
                  if( $(this).prop("checked") ){//判断下 全选框是否被选中，返回的是布尔值
                      $(this).nextAll("input[type='checkbox']").prop("checked",true);
                  }else{
                      $(this).nextAll("input[type='checkbox']").prop("checked",false);
                  }
              })
         ```

4. prevAll ( ):  被选中元素的上面所有同级元素节点；

5. nextUntil ( ): 

   1. 被选中元素下边所有同级且有type="checkbox"属性的元素直到h1为止，括号中填写条件；如下：

   2. nextUntil（param1，param2），

      1. 参数
         1. param1： 选择范围
         2. param2 :  选择的是什么类型的

   3. ```
      <div>
          全选<input type="checkbox"><br>
          <input type="checkbox">
          <input type="checkbox">
          <h1>title</h1>
          <input type="checkbox">
          <input type="checkbox">
      </div>
      <script>
      	$("input[type='checkbox']").nextUntil("h1","input[type='checkbox']")
      </script>

   

## 05-元素的尺寸、位置和页面滚动事件

#### 01元素尺寸-元素位置-加入购物车

1. 小结: 获取和设置元素的尺寸

   1. ```
      $div.width()、$div.height()    获取元素的内容尺寸, width和height  
      $div.innerWidth()、$div.innerHeight()  包括padding的width和height  
      $div.outerWidth()、$div.outerHeight()  包括padding和border的width和height  
      $div.outerWidth(true)、$div.outerHeight(true)   包括padding和border以及margin的width和height
      ```

2. 002获取元素尺寸.html

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              $(function(){
                  var $div = $('.box');
      
                  // 盒子内容的尺寸
                  console.log(1, $div.width());
                  console.log(2, $div.height());
      
                  // 盒子内容加上padding的尺寸
                  console.log(3, $div.innerWidth());
                  console.log(4, $div.innerHeight());
      
                  //盒子的真实尺寸，内容尺寸+padding+border
                  console.log(5, $div.outerWidth());
                  console.log(6, $div.outerHeight());
      
                  // 盒子的真实尺寸再加上margin
                  console.log(7, $div.outerWidth(true));
                  console.log(8, $div.outerHeight(true));
              })
      
          </script>
          <style type="text/css">     
              .box{
                  width:300px;
                  height:200px;
                  padding:20px;
                  border:10px solid #000;
                  margin:20px;
                  background-color:gold;
              }
          </style>
      </head>
      <body>
          <div class="box"></div>
      </body>
      ```
      
      

3. 003 元素绝对位置.html

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              
              $(function(){
                  var $div = $('.box');
                  $div.click(function(){      
                      var oPos = $div.offset();
                      document.title = oPos.left + "|" + oPos.top;
                      console.log(1, oPos.left, oPos.top);
                  })
                  
              })
          </script>
      </head>
      <style type="text/css">
          .box{
              width:200px;
              height:200px;
              background-color:gold;
              margin:50px auto 0;
          }
      </style>
      <body>
          <div class="box">        
          </div>
      </body>
      ```
      
   2. 元素相对页面的绝对位置

      1.  offset();

5.  004加入购物车动画.html

   1. ```
      <head>
          <meta charset="UTF-8">
          <title>Document</title>
          <style type="text/css">
              
              .chart{
                  width:150px;
                  height:50px;
                  border:2px solid blue;
                  text-align:center;
                  line-height:50px;
                  float:right;
                  margin-right:100px;
                  margin-top:50px;
              }
      
              .chart em{
                  font-style: normal;
                  color:red;
                  font-weight:bold;
              }
      
      
              .add{
                  width:100px;
                  height:50px;
                  background-color:green;
                  border:0;
                  color:#fff;
                  float:left;
                  margin-top:300px;
                  margin-left:300px;
                  border: 1px solid black;
              }
      
              .point{
                  width:16px;
                  height:16px;
                  background-color:red;
                  position:fixed;
                  left:0;
                  top:0;
                  display:none;
                  z-index:9999;
                  border-radius:50%;
              }
          </style>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              $(function(){
                  var $chart = $('.chart');    //购物车
                  var $count = $('.chart em');
                  var $btn = $('.add');    // 加入购物车
                  var $point = $('.point');
      
                  var $w01 = $btn.outerWidth();
                  var $h01 = $btn.outerHeight();
      
                  var $w02 = $chart.outerWidth();
                  var $h02 = $chart.outerHeight();
      
                  $btn.click(function(){
                      var oPos01 = $btn.offset();
                      var oPos02 = $chart.offset();
      
                      //设置点的left,top值
                      $point.css({'left':oPos01.left+parseInt($w01/2)-8,'top':oPos01.top+parseInt($h01/2)-8});
                      $point.show();  //对象.显示
      
                      $point.stop().animate({
                          'left':oPos02.left+parseInt($w02/2)-8,
                          'top':oPos02.top+parseInt($h02/2)-8},
                          800,
                          function(){
                              $point.hide();   //对象.隐藏
                              var iNum = $count.html(); //获取em标签中的html字符串
                              console.log(1, $count.html());
                              $count.html(parseInt(iNum)+1);
      
                      });
                  })
              });
          </script>
      </head>
      <body>
          <div class="chart">购物车<em>0</em></div>
          <input type="button" name="" value="加入购物车" class="add">
          <div class="point"></div>
      </body>
      ```

#### 02可视区尺寸    

1. 005可视区尺寸 - 文档尺寸.html

   1. ```
      <head>
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
              $(function(){
                  //浏览器可视区宽度高度
                  console.log('可视区的宽度：'+$(window).width() );
                  console.log('可视区的高度：'+$(window).height() );
      
                  //页面文档的宽度高度
                  //文档的宽度高度 大于或等于 可视区宽度高度
                  console.log('文档的宽度'+$(document).width() );
                  console.log('文档的高度：'+$(document).height() );
                  
                  console.log('scrollTop'+$(document).scrollTop());  
                  console.log('scrollLeft'+$(document).scrollLeft());
          })
          </script>
      </head>
      <body>
      	<p>文档内容</p>
      	<p>文档内容</p>
      	<p>文档内容</p>
      	<p>文档内容</p>
      </body>
      ```

#### 03 scroll 事件

1. 页面滚动距离

   1. ```
      $(document).scrollTop();  
      $(document).scrollLeft();
      ```

2. 滚动事件

   1. ```
      //窗口滚动事件
      $(window).scroll(function(){  
          ......  
      })
      ```

   2. scroll() 是多次触发方法

悬浮菜单-滚动到顶部

1.  006置顶菜单.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<style type="text/css">
      		
      		body{
      			margin:0;
      		}
      
      		.banner{
      			width:960px;
      			height:200px;
      			background-color:cyan;
      			margin:0 auto;
      		}
      
      		.menu{
      			width:960px;
      			height:80px;
      			background-color:gold;
      			margin:0 auto;
      			text-align:center;
      			line-height:80px;
      		}
      
      		.menu_back{
      			width:960px;
      			height:80px;
      			margin:0 auto;
      			display:none;  //该元素盒子不显示
      		}
      
      		p{
      			text-align:center;
      			color:red;
      		}
      
      		.totop{
      			width:60px;
      			height:60px;
      			background-color:#000;
      			color:#fff;
      			position:fixed;
      			right:20px;
      			bottom:50px;
      			line-height:60px;
      			text-align:center;
      			font-size:40px;
      			border-radius:50%;
      			cursor:pointer;
      			display:none;
      		}
      
      	</style>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      			$menu = $('.menu');
      			$menu_back = $('.menu_back');
      			$totop = $('.totop');
      
      			$(window).scroll(function(){  // scroll方法触发
      				//console.log('abc');
      
      				var iNum = $(document).scrollTop();
      				//document.title = iNum;
      
      				if(iNum>200)
      				{
      					$menu.css({
      						'position':'fixed',  //定位
      						'left':'50%',   // 居中设置
      						'top':0,
      						'marginLeft':-480  //jquery中写法与css中写法不同.
      					});
      
      					$menu_back.show();
      				}
      				else
      				{
      					$menu.css({
      						'position':'static',						
      						'marginLeft':'auto'
      					});
      
      					$menu_back.hide();
      				}
      
      				if(iNum>400){
      					$totop.fadeIn();  // 显示
      				}
      				else
      				{
      					$totop.fadeOut();  //隐藏
      				}
      
      			})
      			//事件click(function(){})
      			$totop.click(function(){
      				$('html,body').animate({'scrollTop':0});  //对象.属性'scrollTop':0
      			})
      		})
      	</script>
      </head>
      <body>
      	<div class="banner"></div>
      	<div class="menu">菜单</div>
      	<div class="menu_back"></div>
      
      	<div class="totop">↑</div>
      	<p>文档内容</p>
      	<br />
      	<br />
      	<br />
      	<br />
      	<br />
      	<br />
      	<br />
      	<br />
      	<br />
      	<br />
      	<p>文档内容</p>
      	<br />
      	<br />
      	<br />
      </body>
      ```

## 06-元素属性操作和jQuery循环

#### 01属性操作

1.  html()   取出 / 设置html**内容**

   1. ```
      // 取出html内容
      
      var $htm = $('#div1').html();
      
      // 设置html内容
      
      $('#div1').html('<span>添加文字</span>');
      ```

2.  prop()  取出 / 设置 某个属性的**值**

   1. ```
      // 取出图片的地址
      
      var $src = $('#img1').prop('src');
      
      // 设置图片的地址和alt属性
      
      $('#img1').prop({src: "test.jpg", alt: "Test Image" });
      ```

3.  prop 元素属性操作.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      
      			var $a = $('.link');
      			var $img = $('#img01');
      			var $div = $('#div1');
      
      			// 读取class属性值
      			console.log( $a.prop('class') );
      
      			// 没有设置的属性读取为空
      			console.log( $a.prop('title') );
      
      			// 获取是图片的绝对地址
      			console.log($img.prop('src'));
      
      			//alert($img.prop('src'));
      
      			// 设置属性
      			$a.prop({'href':'http://www.baidu.com','title':'百度网链接'});
      
      			//console.log( $a.prop('title') );
      			//读取标签内包含的内容
      			console.log( $a.html() );
      
      			$div.html('<span>div里面的span元素</span>');
      		})
      	</script>
      </head>
      <body>
      	<a href="#" class="link">这是一个链接</a>
      	<img src="images/002.jpg" id="img01" alt="水果">
      
      	<div id="div1"></div>
      
      </body>
      ```

#### html(), prop(), val()

1. html() 取出或设置html内容

   1. ```
      // 取出html内容
      var $htm = $('#div1').html();
      
      // 设置html内容
      $('#div1').html('<span>添加文字</span>');
      ```

   2. `html()`相当于原生`javascript`的`innerHtml`，即获取元素的之间的html内容，还创建新的html元素。

2. prop() 取出或设置某个属性的值

   1. prop :  property属性的意思

   2. ```
      // 取出图片的地址
      var $src = $('#img1').prop('src');
      
      // 设置图片的地址和alt属性
      $('#img1').prop({src: "test.jpg", alt: "Test Image" });
      ```

3. val()  ,  input元素的value值用 val() 获取

4. innerHTML

   1. ```
      <!DOCTYPE html>
      <html>
      <head>
          <title></title>
          <script type="text/javascript" src="jquery/jquery-3.3.1.min.js"></script>
          <script type="text/javascript">
              window.onload = function(){
                  var div = document.getElementById('box1');
                  // alert(div.innerHTML);
      
                  js_method = function(){
                      alert("点击a标签");
                  }
      
                  // div.innerText = "<a href='javascript:js_method();'>点击ok</a>";
      
                  div.innerHTML = "<a href='javascript:js_method();'>点击ok</a>";
      
                  // $('#box1').html("<a href='javascript:js_method();'>点击ok</a>");
              }
          </script>
      </head>
      <body>
          <div id="box1">这是一个div</div>
      </body>
      </html>

#### attr() prop() css()

1. attr( )  设置元素的属性（给元素新增加属性）,获取元素的本来的属性以及额外设置的属性。如获取的属性没有设置，结果是 undefined;
   1. attr()获取属性值，那么该属性必须**显式的设置在HTML代码中**或者**通过attr新增的属性**才能被获取到，如果没有设置，那么将返回undefined
2. prop( ) 设置元素的属性（HTML固有的属性，添加属性）, 获取元素的固有的属性值，额外设置的属性，无法通过prop（ ）获取
   1. prop（）获取属性值，那么该属性**只能是HTMl的固有属性**，无论是否显式的设置，都可以获取其对应的属性值，如果是额外增加的属性，那么将无法获取
3. css() 方法设置或返回被选元素的一个或多个样式属性
   1. $("p").css("background-color");

#### 03 animate()动画

1. animate() 执行一个基于css属性的自定义动画

   1. 两种形式的用法

      1. `jQueryObject.animate( cssProperties [, duration ] [, easing ] [, complete ] )`

      2. `jQueryObject.animate( cssProperties, options )` 法二是用法一的变体

         1. 参数

            1. | 参数          | 描述                                                         |
               | :------------ | :----------------------------------------------------------- |
               | cssProperties | Object类型一个或多个css属性的键值对所构成的Object对象。      |
               | duration      | 可选/String/Number类型指定动画运行多长时间(毫秒数)，默认值为400。该参数也可以为字符串"fast"(=200)或"slow"(=600)。 |
               | easing        | 可选/String类型指定使用何种动画效果，默认为"swing"，还可以设为 "linear"或其他自定义的动画样式函数。 |
               | complete      | 可选/Function类型元素显示完毕后需要执行的函数。函数内的`this`指向当前DOM元素。 |
               | options       | Object类型指定的选项参数对象。                               |

         2. 返回值: jQuery类型，返回当前jQuery对象本身

   2. css属性为一个单一的数值, 非数值的css属性无法执行动画

      1.  width、height、left、top 可用于动画
      2.  color、background-color无法用于动画(除非用[jQuery.Color()](https://github.com/jquery/jquery-color)插件)。
      3.  默认的数值单位为像素(px), 除非属性值指定了单位(如：px、em、%)，。
      4.  css属性值设为一些特定的字符串， 如："show"、"hide"、"toggle"，则jQuery会调用该属性默认的动画形式。
      5.  css属性值可以是相对的，属性值加上前缀"+="或"-="，在原来的属性值上增加或减少指定的数值。例如：`{ "height": "+=100px" }`，表示在原有高度的基础上增加100px。

   3. 速写的css属性可能无法获得完整全面的支持，例如：border、margin等，因此不推荐使用

   4. 示例

      1. ```
         HTML:
         <div id="myDiv" style="width:300px; height: 100px; background-color: #eee;">CodePlayer</div>
         动画效果：
         <select id="animation">
             <option value="1">动画1</option>
             <option value="2">动画2</option>
             <option value="3">动画3</option>
             <option value="4">动画4</option>
             <option value="5">动画5</option>
         </select>
         <input id="exec" type="button" value="执行动画" >
         
         
         jQuery代码:
         $("#exec").click( function(){
             var v = $("#animation").val();
             var $myDiv = $("#myDiv");
             if(v == "1"){
                 // 数值的单位默认是px
                 $myDiv.animate( { height: 200 } );
             }else if(v == "2"){
                 // 在现有高度的基础上增加300px (如果原来是100px，增加后就是400px)
                 // 多个动画连续执行
                 $myDiv.animate( { height: "+=300px" }, "slow" ); 
                 $myDiv.animate( { width: "50%" }, 1000 );       
                 $myDiv.animate( { width: "200px", height: "100px" }, 1000 );        
             }else if(v == "3"){
                 // font-size或fontSize均可，由多个单词构成的属性均是如此
                 $myDiv.animate( { fontSize: "30px" }, 2000 );
                 $myDiv.animate( { fontSize: "14px" }, 2000, function(){
                     alert("动画3执行完毕!");
                 });
             }else if(v == "4"){
                 $myDiv.animate( { width: "50%", height: "50%" }, { duration: 2000, easing: "linear" });
             }else if(v == "5"){
                 // 根据高度切换显示/隐藏，显示时高度从0增加到原高度，隐藏时高度从原高度减小到0
                 $myDiv.animate( { height: "toggle" });
             }   
         } );
         ```

      

2. 手风琴.html

   1. ```
      <head>
      <meta charset="utf-8">
      <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      <style>
      	*{margin:0;padding:0;}
      	body{font-size:12px;}
      	#accordion{width:727px; height:350px; margin:100px auto 0 auto; position:relative; overflow:hidden; border:1px solid #CCC;}
      	#accordion ul{list-style:none;}
      	#accordion ul li{width:643px;height:350px; position:absolute; background:#FFF;}
      	#accordion ul li span{display:block;width:20px; height:350px; float:left; text-align:center; color:#FFF; padding-top:5px; cursor:pointer;}
      	#accordion ul li img{display:block; float:right;}
      	.bar01{left:0px;}
      	.bar02{left:643px;}
      	.bar03{left:664px;}
      	.bar04{left:685px;}
      	.bar05{left:706px;}
      	.bar01 span{background:#09E0B5;}
      	.bar02 span{background:#3D7FBB;}
      	.bar03 span{background:#5CA716;}
      	.bar04 span{background:#F28B24;}
      	.bar05 span{background:#7C0070;}
      </style>
      
      <script type="text/javascript">
      		$(function(){
      			var $li = $('#accordion li');
      			$li.click(function(){
      				//alert($(this).html());
      				$(this).animate({'left':21*$(this).index()});
      
      				//点击的li前面的li向左运动到各自的位置
      				$(this).prevAll().each(function(){
      
      					//这里的$(this)指的是循环选择的每个li
      					$(this).animate({'left':21*$(this).index()});
      
      				})
      
      				//   第5个li在右边的left值   727-21*1 等同于 727-21*(5-$(this).index())
      				//   第4个li在右边的left值   727-21*2 等同于 727-21*(5-$(this).index())
      				//   第3个li在右边的left值   727-21*3 等同于 727-21*(5-$(this).index())
      
      				$(this).nextAll().each(function(){
      					$(this).animate({'left':727-21*(5-$(this).index())});
      				})
      			})
      		})
      </script>
      <title>手风琴效果</title>
      </head>
      <body>
      <div id="accordion">
      	<ul>
      	<li class="bar01"><span>非洲景色01</span><img src="images/001.jpg" /></li>
          <li class="bar02"><span>非洲景色02</span><img src="images/002.jpg" /></li>
          <li class="bar03"><span>非洲景色03</span><img src="images/003.jpg" /></li>
          <li class="bar04"><span>非洲景色04</span><img src="images/004.jpg" /></li>
          <li class="bar05"><span>非洲景色05</span><img src="images/005.jpg" /></li>
      	</ul>
      </div>
      </body>
      ```

   


#### 04 each()

1. $selector.each(function(){}), 用于jQuery循环

1. css()  方法 对 标签集 一次 操作, css() 样式操作

   1. ````
      $(function(){
          var $li = $('.list li');
          //$li.css({'backgroundColor':'gold'});
          
          $li.each(function(a){
          	//alert(a);
          	//alert( $(this).html() );
          	//alert($(this).index());
          	if($(this).index()%2==0)
          	{
          		$(this).css({'backgroundColor':'gold'});
          	}
          })
      })
      ````

2. each()  对标签集 循环操作

   1. each()方法能使DOM循环结构简洁，不容易出错

   2. 可以遍历一维数组、多维数组、DOM, JSON

   3. ```
      var arr2 = [['a', 'aa', 'aaa'], ['b', 'bb', 'bbb'], ['c', 'cc', 'ccc']]
      
      $.each(arr2, function(i, item){      
            alert(i);   
            alert(item);      
      }); 

   

3. 008 jquery循环.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      
      			var $li = $('.list li');
      
      			//$li.css({'backgroundColor':'gold'});
      
      			$li.each(function(a){
      				//alert(a);
      				//alert( $(this).html() );
      				//alert($(this).index());
      
      				if($(this).index()%2==0)
      				{
      					$(this).css({'backgroundColor':'gold'});
      				}
      			})
      		})
      	</script>
      </head>
      <body>
      	<ul class="list">
      		<li>1</li>
      		<li>2</li>
      		<li>3</li>
      		<li>4</li>
      		<li>5</li>
      		<li>6</li>
      		<li>7</li>
      		<li>8</li>
      	</ul>
      </body>
      ```

#### 05 jquery事件

1.  对象关联事件的方式一: 对象.事件函数名(函数);

2. 事件函数

   1. blur() 对象失去焦点事件函数
   2. focus() 对象获得焦点事件函数// 一般不绑定函数方法 , 只为获取$('#input01').focus();
   3. click() 鼠标单击事件函数
   4. mouseover() 鼠标进入事件函数（进入子元素也触发）
   5. mouseout() 鼠标离开事件函数（离开子元素也触发）
   6. mouseenter() 鼠标进入事件函数（进入子元素不触发）
   7. mouseleave() 鼠标离开事件函数（离开子元素不触发）
   8. hover() 同时为mouseenter和mouseleave事件指定处理函数
   9. ready() DOM加载完成事件函数
   10. resize() 浏览器窗口的大小改变事件函数
   11. scroll()  滚动条事件函数
   12. submit()   提交表单事件函数

   

2.  009jquery事件.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		
      		$(function(){
      
      			// 在获得焦点的时候做什么事情
      			/*$('#input01').focus(function(){
      				alert('获得焦点')
      			})*/
      
      			//focus 一般用来让input元素开始就获取焦点，只能是一个元素获得焦点
      			$('#input01').focus();
      			
      			//失去焦点
      			$('#input01').blur(function(){
      				// 获取input元素的value值用 val()
      				var sVal = $(this).val();
      				alert(sVal);
      			})
      
      			$('#form1').submit(function(){  //提交表单form1
      				//alert('提交');
      				// 阻止默认的提交行为
      				return false;
      			})
      		})
      	</script>
      </head>
      <body>
      	<form id="form1" action="http://www.baidu.com">
      		<input type="text" name="dat01" id="input01">
      		<input type="text" name="dat02" id="input02">
      		//input submit向form action指定的网页提交form数据
      		<input type="submit" name="" value="提交" id="sub">
      	</form>
      </body>
      ```

3.  010 jquery鼠标移入移出事件.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		
      		$(function(){
      
      			// 鼠标移入，移入的子元素也会触发
      			$('.con').mouseover(function(){
      				alert('移入');
      			})
      			// 鼠标移出，移出的子元素也会触发
      			$('.con').mouseout(function(){
      				alert('移出');
      			})
      
      
      			// 鼠标移入，移入的子元素不会触发
      			/*
      			$('.con2').mouseenter(function(){
      				alert('移入');
      			})
      
      			$('.con2').mouseleave(function(){
      				alert('移出');
      			})			
      			*/
      			
      			// mouseenter与mouseleave合并为hover：
      			$('.con2').hover(function(){
      				alert('移入')
      			},function(){
      				alert('移出')
      			})
      		})
      
      	</script>
      	<style type="text/css">
      		.con,.con2{
      			width:200px;
      			height:200px;
      			background-color:gold;
      		}
      		
      		.box,.box2{
      			width:100px;
      			height:100px;
      			background-color:green;
      		}
      	</style>
      </head>
      <body>
      	<div class="con">   //父元素
      		<div class="box"></div> //子元素 
      	</div>
      	<div class="con2">
      		<div class="box2"></div>
      	</div>
      </body>
      ```

4.  011 resize()事件.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		
      		$(window).resize(function(){
      			var $w = $(window).width();
      			document.title = $w;
      		});
      	</script>
      </head>
      <body>	
      </body>
      ```

      1. document.title  网页标题

## 07-jQuery事件

#### 01 bind()绑定/ unbind()解绑

1.  对象关联事件的方式二:  对象.bind("事件", 响应函数)

1.  click()事件 与 bind("click mouseover", function(){})

   1. `$(selector).bind(event,data,function)`

      1. 参数:
         1. event 必须。添加到元素的一个或多个事件如：blur，focus，load，resize，scroll，unload，click，dblclick，mousedown，mouseup，mousemove，mouseout，mouseover，mouseenter，mouseleave，change，select，submit，keydown，keypress，keyup，error。
         2. data 可不填。传递到函数的额外数据，如：$(selector).bind(“click”,”input”,function(){});
         3. function(){} 必填。绑定事件触发的函数

   2.  bind绑定多个函数

      1. ```
         $("button").bind({ // 参数格式json
                 click:function(){$("div").css("border","5px solid orange");},
                 mouseover:function(){$("div").css("background-color","red");},  
                 mouseout:function(){$("div").css("background-color","#FFFFFF");}  
             });
         ```

   3. bind绑定数据

      1. ```
         // bind() 绑定 click 事件传 参数2 并且打印出 参数2
         $('button').bind('click',['路飞','索隆','乌索普'],function(event){
             alert(event.data[0]); // 路飞
         });
         ```

   4. 示例

      1. ```
         var $cr = $("#btn");
         $cr.bind("click",myfun = function(){                   //bind绑定事件，可以没有额外数据。可以将事件赋值给一个引用变量，在其他地方使用这个函数
             this.innerHTML;                                    //this表示绑定对象
             $(this).html();                                    //将this通过$转化为jquery对象，以便调用jquery函数
         });
          
          
         $cr.bind("click.mynamespace",function(){ //mynamespace用于表示事件的命名空间，是一个用于区别相同类型事件的不同函数.
         });
          
          
         $cr.bind("mouseover",function(event){   //bind更换绑定事件，event事件对象，调用不用传递传参数，会自动拥有此参数。
             event.preventDefault();             //阻止默认行为，如超链接自动跳转，提交按钮自动提交。
             event.stopPropagation();            //停止事件冒泡。因为事件大部分默认是冒泡的
             return false;                       //return false也代表了preventDefault和stopPropagation，可以代替他们
         }).bind("mouseout",function(event){     //对于没有返回值的对象函数，函数后可以继续进行访问，jquery特色的链式操作。
             event.type;                         //事件类型
             event.target;                       //触发事件的元素，此处相当远鼠标离开的元素
             event.relatedTarget;                //相关元素，此处相当于鼠标进入的元素
             event.pageX+event.pageY;            //光标相对于页面的位置，如果有滚动条要加上滚动条的位置
             event.which;                        //鼠标按键（1,2,3,表示左右中键）或键盘按键
             event.metaKey;                      //ctrl按键
         });
          
          
         $cr.bind("mouseover mouseout",function(){   //可以一次绑定多个事件
             alert("光标进出");
         });
          
          
         $cr.bind("myclick",function(event,message1,message2){  //为对象绑定自定义事件，不会自动触发，必须代码触发。
             alert(message1.toString()+message2.toString());
         });
          
          
         $cr.one("click",function(){  //对只执行一次，就解除绑定的事件，使用one绑定
         });
         ```

      2. 

2. 001 bind()事件的其他方法.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      			/*
      			$('#btn').click(function(){				
      				alert('click事件')		
      			})
      			*/
      
      			// 点击或者鼠标移入的时候都执行绑定的函数
      			$('#btn').bind('click mouseover',function(){
      				alert('触发事件绑定的函数');
      			})
      		})
      	</script>
      </head>
      <body>
      	<input type="button" name="" value="按钮" id="btn">
      </body>
      ```
      
   2. bind('事件名', 函数名);  // 语法

3. 解绑事件

   1. ```
      $(function(){
          $('#div1').bind('mouseover click', function(event) {
              alert($(this).html());
      
              // $(this).unbind();
              $(this).unbind('mouseover');
      
          });
      });
      ```

      

#### 02 参数 event

1. 冒泡

   1. 定义: 对象触发某事件（如onclick事件），若没有此事件处理程序或事件返回true，事件向父对象传播，从里到外，直至事件被处理, 或到达最顶层, 即document对象（有些浏览器是window）. 

      1. 即 事件 从 里向外的 传播
      2.  按标签的 书写顺序, 不是 标签的布局顺序

   2. 作用

      1. 允许多个操作被**集中处理**（事件处理器添加到一个父元素上，避免事件处理器添加到多个子级元素），在对象层的不同级别捕获事件。

2. 阻止冒泡

   1. 阻止冒泡

      1. event.stopPropagation();  //阻止冒泡.  冒泡不是事件, 是处理事件的一种方法

   2. 阻止默认行为

      1. ```
         $('#form1').submit(function(event){
             event.preventDefault();
         })
         ```

   3. 合并阻止操作

      1. 阻止冒泡和阻止默认行为合并起来写

      2. ```
         // event.stopPropagation();
         // event.preventDefault();
         
         // 合并写法：
         return false;
         ```

3. 002事件的冒泡.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      				// event: 发生事件时的事件对象本身，第一个参数传入
      				$('.son').click(function(event){					
      					alert(1);
      					//event对象的stopPropagation方法阻止事件的冒泡
      					//event.stopPropagation();
      				})
      
      				$('.father').click(function(event){
      					alert(2);
      					event.stopPropagation();
      				})	
      
      				$('.grandfather').click(function(){
      					alert(3);
      					// 阻止事件冒泡和阻止默认行为的统一写法：
      					return false;
      				})
      
      				$(document).click(function(){
      					alert(4);
      				})
      		})
      
      	</script>
      	<style type="text/css">		
      		.grandfather{
      			width:300px;
      			height:300px;
      			background-color:green;
      			position:relative;
      		}
      
      		.father{
      			width:200px;
      			height:200px;
      			background-color:gold;
      		}
      
      
      		.son{
      			width:100px;
      			height:100px;
      			background-color: red;
      			position:absolute;
      			left:0;
      			top:400px;
      		}
      	</style>
      </head>
      <body>
      	<div class="grandfather">		
      		<div class="father">			
      			<div class="son"></div>
      		</div>
      	</div>
      </body>
      </html>
      ```



document   标签顶级, 整个文档, 等级与html一致

#### 03阻止事件的冒泡-弹框实例

1. 003弹框.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      
      			$('#btn').click(function(){
      				$('.pop_con').fadeIn();
      				return false;  // 阻止事件冒泡
      			})
      			$(document).click(function(){
      				$('.pop_con').fadeOut();
      
      			})
      			
      			//阻止 .pop_con 的事件的冒泡
      			$('.pop').click(function(){
      				return false;
      			})
      			$('#close').click(function(){
      				$('.pop_con').fadeOut();
      			})
      		})
      
      
      	</script>
      	<style type="text/css">
      
      		.pop_con{
      			display:none;
      		}
              
      		.pop{
      			position:fixed;
      			width:500px;
      			height:300px;
      			background-color:#fff;
      			border:3px solid #000;
      			left:50%;
      			top:50%;
      			margin-left:-250px;
      			margin-top:-150px;
      			z-index:9999;
      		}
      
      		.mask{
      			position:fixed;
      			width:100%;
      			height:100%;
      			background-color:#000;
      			opacity:0.3;
      			filter:alpha(opacity=30);
      			left:0;
      			top:0;
      			z-index:9990;
      		}
      
      		.close{
      			float:right;
      			font-size:30px;
      		}
      	</style>
      </head>
      <body>
      	<input type="button" name="" value="弹出" id="btn">
      
      	<div class="pop_con">
      		<div class="pop">
      			弹框里面文字
      			投资金额：
      			<input type="text" name="">
      			<a href="#" id="close" class="close">×</a>
      		</div>
      		<div class="mask"></div>
      	</div>
      </body>
      </html>
      ```

#### 04 delegate

1.  事件委托

   1. 利用事件的冒泡，把事件加到父级，判断事件来源的子集，执行相应的操作.
   2. 优点:
      1. 减少事件绑定次数，提高性能.
      2. 新加入的子元素有相同的操作。

2.  `$([select].delegate(childSelector,event,data,function)`

    1.  参数
        1.  childSelector 必需。要附加事件的一个或多个子元素
        2.  event 必需。附加到元素的一个或多个事件。空格分隔多个事件值。必须是有效的事件。
        3.  data 可选。传递到函数的额外数据。
        4.  function 必需。事件发生时运行的函数。

    2.  
        delegate作用: 实现事件委托, 函数被某一类型的共同父元素调用

3.  事件委托的写法

   1. $list.delegate('li', 'click', function(){}) 方法,  `$list`父标签, 'li' 子标签, 'click' 事件, function(){} 函数

4.  004 事件委托.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<style type="text/css">
      		
      		.list{
      			background-color:gold;
      			list-style:none;
      			padding:10px;
      		}
      
      		.list li{
      			height:30px;
      			background-color:green;
      			margin:10px;
      		}
      
      	</style>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){		
      
      			/*
      			$('.list li').click(function(){
      				$(this).css({'backgroundColor':'red'});
      			});
      			*/
      
      			// 新建一个li元素赋值给变量 $li
      			//var $li = $('<li>9</li>');
      
      			//让新加的li有相同的事件，需要单独绑定
      			//$li.click(....)
      
      			//新建的li元素放到ul子集的最后面
      			//$('.list').append($li);	
      			
      			//事件委托，li的事件委托给li的父级.list
      			$('.list').delegate('li','click',function(){
      				//$(this) 指的是委托的子元素
      				$(this).css({'backgroundColor':'red'});
      			})
      
      			var $li = $('<li>9</li>');
      			$('.list').append($li);
      		})
      	</script>
      </head>
      <body>
      	<ul class="list">
      		<li>1</li>
      		......
      		<li>8</li>
      	</ul>
      </body>
      </html>
      ```


## 08-节点操作

#### 01节点操作

1. 节点操作
   1. 创建节点

      1. ```
         var $div = $('<div>');   //创建<div></div>节点
         var $div2 = $('<div>这是一个div元素</div>');    //带文本的节点
         ```

   2. 插入节点 

      1. 原理: dom树是 标签名字符串组成的list列表

      2. 方法
   
         1. 父元素.append(子元素);   // 内部, 后面
   
            1. ```
               var $span = $('<span>这是一个span元素</span>');
               $('#div1').append($span);   // 父元素.append(子元素), 内部的后面插入
               ......
               <div id="div1"></div>
   
         2. 子元素.appendTo(父元素);   // 内部, 后面
   
         3. 父元素.prepend(子元素);   // 内部, 前面
   
         4. 子元素.prependTo(父元素);   // 内部

            

         5. 父元素.after(子元素);   // 外部, 后面
   
         6. 子元素.insertAfter(父元素);   // 外部, 后面
   
         7. 父元素.before(子元素);   // 外部, 前面
   
         8. 子元素.insertBefore(父元素);   // 外部, 前面

   3. 删除节点

      1. 方法:
      
         1. remove() - 删除被选元素（及其子元素） 
         2. empty() - 从被选元素中删除子元素
      
      2. ```
         $("p").remove(".italic");
         ```
   
2. 示例

   1. 005新建的节点操作.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         
         			// html的字符串的方式添加节点性能最高
         			//$('#div1').html($('#div1').html()+'<a href="#">链接</a>')
         
         			// 新建一个带有属性的a元素，把它赋值给$a
         			$a = $('<a href="#">链接</a>');
         
         			// 新建一个空的a元素
         			$a2 = $('<a>');
         
         			$p = $('<p>这是一个p元素</p>');
         			$h2 = $('<h2>这是一个h2</h2>');
         			$h3 = $('<h3>这是一个h3</h3>');
         
         			// 父元素内的后面放入子元素
         			//$('#div1').append($a);   //父.append(子);
         
         			//子元素放入到父元素内部的后面
         			$a.appendTo($('#div1'));
         
         			// 父元素内的前面放入子元素
         			//$('#div1').prepend($p);
         
         			//子元素放入到父元素内部的前面
         			$p.prependTo($('#div1'));
         			
         			//元素外部添加元素
         			//$('#div1').after($h2);
         			$h2.insertAfter($('#div1'));
         
         			//$('#div1').before($h3);
         			$h3.insertBefore($('#div1'));
         		})
         	</script>
         </head>
         <body>
         	<div id="div1">
         		<h1>这是一个H1元素</h1>
         	</div>
         </body>
         </html>
         ```

         

   2. 006已有的节点操作.html

      1. ```
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         			
         			//$('#p1').insertBefore($('#title01'));  元素.insertBefore(元素)
         			$('#title01').before($('#p1'));
         
         			$('#span01').appendTo($('#p1'));
         			$('#link01').prependTo($('#p1'));
         
         			$('#title01').remove();  //删除#title01元素
         		})
         	</script>
         </head>
         <body>	
         	<h1 id="title01">这是一个H1元素</h1>
         	<p id="p1">这是一个p元素</p>
         	<span id="span01">这是一个span元素</span>
         	<a href="#" id='link01'>链接</a>
         </body>
         ```

   3.  007 a链接的 href属性.html

      1. ```
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         </head>
         <body>
         	<p>页面文字内容</p>
         	<br />
         	<br />
         	<p>页面文字内容</p>
         	<br />
             <!-- "#" : 默认会链接到页面顶部   -->
             <!-- <a href="#">链接</a> -->
         
             <!-- 字符串: 链接javascript语句 -->
             <a href="javascript:alert('ok!');">链接</a>
             
             <!-- 空:链接执行javascript的空语句，什么都不做   -->
             <!-- <a href="javascript:;">链接</a> -->
         </body>
         ```
         
         

#### 02 todolist案例

1. 案例是 节点操作

2. todolist案例.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>todolist</title>
      	<style type="text/css">
      		.list_con{
      			width:600px;
      			margin:50px auto 0;
      		}
      		.inputtxt{
      			width:550px;
      			height:30px;
      			border:1px solid #ccc;
      			padding:0px;
      			text-indent:10px;			
      		}
      		.inputbtn{
      			width:40px;
      			height:32px;
      			padding:0px;
      			border:1px solid #ccc;
      		}
      		.list{
      			margin:0;
      			padding:0;
      			list-style:none;
      			margin-top:20px;
      		}
      		.list li{
      			height:40px;
      			line-height:40px;
      			border-bottom:1px solid #ccc;
      		}
      
      		.list li span{
      			float:left;
      		}
      
      		.list li a{
      			float:right;
      			text-decoration:none;
      			margin:0 10px;
      		}
      	</style>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      
      			var $inputtxt = $('#txt1');
      			var $btn = $('#btn1');
      			var $ul = $('#list');
      
      			$btn.click(function(){
      				// val()获取input输入框的输入内容
      				var $val = $inputtxt.val();
      				if($val=="")
      				{
      					alert('请输入内容');
      					return;
      				}
      				
      				//创建节点:下面两个语句相同
      				/*var $li = '<li><span>'+ $val +'</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>';*/
      				//创建节点,规范写法
      				var $li = $('<li><span>'+ $val +'</span><a href="javascript:;" class="up"> ↑ </a><a href="javascript:;" class="down"> ↓ </a><a href="javascript:;" class="del">删除</a></li>');
      
      			/*	var $a = $li.find('.del');
      
      				$a.click(function(){
      					$(this).parent().remove();
      				})*/
      				
      				//对象的后面添加子节点
      				$ul.append($li);
      				$inputtxt.val("");
      			})
      
      			/*
      			$('.del').click(function(){
      				$(this).parent().remove();
      			})*/
      
      			$ul.delegate('a','click',function(){
      				//定义变量,赋值class的属性值, 值是字符串
      				var $handler = $(this).prop('class');
      
      				if($handler=='del')
      				{	
      					//parent的元素是li,
      					$(this).parent().remove();
      				}
      
      				if($handler=='up')
      				{
      					//parent的元素是li,
      					if($(this).parent().prev().length==0)
      					{
      						alert('到顶了！');
      						return;
      					}
      					//parent的元素是li,
      					$(this).parent().insertBefore( $(this).parent().prev() );
      				}
      
      				if($handler=='down')
      				{
      					if($(this).parent().next().length==0)
      					{
      						alert('到底了！');
      						return;
      					}
      					
      					//parent的元素是li, 
      					$(this).parent().insertAfter( $(this).parent().next() );
      				}
      			})
      		})
      
      	</script>	
      </head>
      <body>
      	<div class="list_con">
      		<h2>To do list</h2>
      		<input type="text" name="" id="txt1" class="inputtxt">
      		<input type="button" name="" value="增加" id="btn1" class="inputbtn">	
      		<ul id="list" class="list">
      			<li>  
      				<span>学习html</span>
      				<a href="javascript:;" class="up"> ↑ </a>
      				<a href="javascript:;" class="down"> ↓ </a>
      				<a href="javascript:;" class="del">删除</a>
      			</li>
      			<li>
      				<span>学习css</span>
      				<a href="javascript:;" class="up"> ↑ </a>
      				<a href="javascript:;" class="down"> ↓ </a>
      				<a href="javascript:;" class="del">删除</a>
      			</li>
      			<li>
      				<span>学习javascript</span>
      				<a href="javascript:;" class="up"> ↑ </a>
      				<a href="javascript:;" class="down"> ↓ </a>
      				<a href="javascript:;" class="del">删除</a>
      			</li>
      		</ul>
      	</div>
      </body>
      </html>
      ```

      1. `$ul.delegate('a','click',function(){});`   //事件委托
      2.  `var $val = $inputtxt.val();`  // 获取input输入框的内容
      3.  $(this).parent().remove();  // 删除
      4.  `$(this).parent().prev().length==0`  //比较判断获取的元素数量 是否 是0

#### prev()  next()

1. prev() : 获取指定元素同辈的上一个元素

2. next() : 获取指定元素同辈的下一个元素

3. 示例

   1. ```
      HTML代码:
      <ul>
          <li>我是第一个li标签</li>
          <li>我是绿色的字体</li>     <!--绿色-->
          <li class="element">我是第三个li标签</li>
          <li>我是红色的字体</li>     <!--红色-->
          <li>我是第五个li标签</li>
      </ul>
      
      
      jQuery代码:
      $(".element").next().css("color","red");
      $(".element").prev().css("color","green");
      ```

      

## 09-整屏滚动

#### 01整屏滚动

1.  鼠标滚轮事件
   1. jquery的滚轮事件插件jquery.mousewheel.js
   
   2. jquery没有
   
   3. 原生js中的鼠标滚轮事件不兼容
   
      
   
2. 函数节流
   1.  javascript 高频率事件
      1. onresize事件 ( 对应jQuery中 resize)
      2. onmousemove事件(对应jQuery中 mousemove)
      3. 鼠标滚轮事件
   2. 函数节流
      1. 短时间内多次触发绑定的函数，用定时器减少触发 次数，实现函数节流

#### 02 mousewheel()

1.  鼠标滚轮事件

   1. mousewheel() ; // 事件函数

   2. 008 鼠标滚轮事件.html

      1. ```
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/jquery.mousewheel.js"></script>
         	<script type="text/javascript">
         
         		//var i = 0;		
         		$(window).mousewheel(function(event,dat){  //函数的参数dat默认值是1
         			alert(dat);
         
         			//i++;
         			//console.log(i);
         		})
         	</script>
         </head>
         ```

2.  整屏滚动.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>整页滚动</title>
      	<link rel="stylesheet" type="text/css" href="css/test.css">
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/jquery.mousewheel.js"></script>
      	<script type="text/javascript">
      		$(function(){
      			var $screen = $('.pages_con');
      			var $pages = $('.pages');
      			var $len = $pages.length;
      			var $h = $(window).height();  //window 高度
      			var $points = $('.points li');
      			var timer = null;
      
      			var $nowscreen = 0;
      			$pages.css({'height':$h});
      			$pages.eq(0).addClass('moving');
      
      
      			$points.click(function(){
      				$nowscreen = $(this).index();
      				$points.eq($nowscreen).addClass('active').siblings().removeClass('active');
      				$screen.animate({'top':-$h*$nowscreen},300);  //滚动窗口
      				$pages.eq($nowscreen).addClass('moving').siblings().removeClass('moving');
      			})
      
      
      
      			$(window).mousewheel(function(event,dat){
      				clearTimeout(timer);    //函数节流
      				timer = setTimeout(function(){
      
      					if(dat==-1)  //向下滚动
      					{
      						$nowscreen++;
      					}
      					else   // 向上滚动
      					{
      						$nowscreen--;
      					}
      					if($nowscreen<0)
      					{
      						$nowscreen=0;
      					}
      
      					if($nowscreen>$len-1)
      					{
      						$nowscreen=$len-1;
      					}
      
      					$screen.animate({'top':-$h*$nowscreen},300);
      					$pages.eq($nowscreen).addClass('moving').siblings().removeClass('moving');//页面滚动,添加/移除class
      
      					$points.eq($nowscreen).addClass('active').siblings().removeClass('active');//点滚动, 添加/移除class
      
      					},200)		
      
      			})
      		})
      
      	</script>	
      </head>
      <body>
      	<div class="pages_con">
      
      		<div class="pages page1">
      			<div class="main_con">
      				<div class="left_img"><img src="images/001.png"></div>
      				<div class="right_info">
      				Web前端开发是从网页制作演变而来的，名称上有很明显的时代特征。在互联网的演化进程中，网页制作是Web1.0时代的产物，那时网站的主要内容都是静态的，用户使用网站的行为也以浏览为主。
      					
      				</div>
      			</div>
      		</div>
      
      		<div class="pages page2">
      			<div class="main_con">
      				<div class="right_img"><img src="images/002.png"></div>
      				<div class="left_info">
      				2005年以后，互联网进入Web2.0时代，各种类似桌面软件的Web应用大量涌现，网站的前端由此发生了翻天覆地的变化。网页不再只是承载单一的文字和图片，各种富媒体让网页的内容更加生动，网页上软件化的交互形式为用户提供了更好的使用体验，这些都是基于前端技术实现的。
      				</div>
      			</div>
      			
      		</div>
      
      		<div class="pages page3">
      			<div class="main_con">
      				<div class="left_img"><img src="images/004.png"></div>
      				<div class="right_info">
      				以前会Photoshop和Dreamweaver就可以制作网页，现在只掌握这些已经远远不够了。无论是开发难度上，还是开发方式上，现在的网页制作都更接近传统的网站后台开发，所以现在不再叫网页制作，而是叫Web前端开发。
      
      					
      				</div>
      			</div>			
      		</div>
      
      		<div class="pages page4">
      			<div class="main_con">
      				<div class="left_img"><img src="images/003.png"></div>
      				<div class="right_info">
      					Web前端开发在产品开发环节中的作用变得越来越重要，而且需要专业的前端工程师才能做好，这方面的专业人才近几年来备受青睐。Web前端开发是一项很特殊的工作，涵盖的知识面非常广，既有具体的技术，又有抽象的理念。简单地说，它的主要职能就是把网站的界面更好地呈现给用户。
      				</div>
      			</div>			
      		</div>
      
      		<div class="pages page5">
      			<div class="main_con">
      				<div class="center_img"><img src="images/005.png"></div>		
      			</div>
      			
      		</div>
      	</div>
      	<ul class="points">
      		<li class="active"></li>
      		<li></li>
      		<li></li>
      		<li></li>
      		<li></li>
      	</ul>
      </body>
      </html>
      ```

      1.  $('.pages_con');  大页面元素
      2.  $('.pages');  多个页面的集合
      3.  $pages.length;  // 页面元素个数
      4.  $(window).height(); //窗口高
      5.  $(this).index();  // 当前元素的索引号 index() 
      6.  $(window).mousewheel(function(event,dat){}); // 鼠标滚动事件

   2. 函数节流

      1.  事件 高频触发的处理方法, 使用一次时间定时器, setTimeout(function(){}, 200),  产生延迟, 过滤掉高频触发, 只保留了最后的一次触发.
      2.  鼠标滚动 高频触发时间

#### 03 js制作css3动画

1. ```
   <head>
   	<meta charset="UTF-8">
   	<title>Document</title>
   	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
   	<script type="text/javascript">
   		$(function(){
   			$('#btn').click(function(){
   				$('.box').addClass('moving');
   			})
   		})
   
   	</script>
   	<style type="text/css">
   		.box{
   			width:200px;
   			height:200px;
   			background-color:gold;
   			margin:50px auto 0;
   			transition:all 1s ease;
   		}
   
   		.moving{
   			transform:rotate(135deg);
   		}
   	</style>
   </head>
   <body>
   	<input type="button" name="" value="动画" id="btn">
   	<div class="box"></div>
   </body>

​	动画:  transition,  transform:rotate()



## 10 幻灯片

#### 01幻灯片

01 02 03 04 都是幻灯片

1. index.html

   1. ```
      <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
      <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
      <head>
          <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
          <link rel="stylesheet" type="text/css" href="css/reset.css">
          <link rel="stylesheet" type="text/css" href="css/main.css">
          <script type="text/javascript" src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
          <script type="text/javascript" src="js/slide2.js"></script>
          <script type="text/javascript">
              $(function(){
                  $.ajax({
                      url:'js/data.json',
                      type:'get',
                      dataType:'json'
                  })
                  .done(function(dat){
      
                      $('.user_login_btn').hide();
      
                      $('.user_info em').html(dat.name);
      
                      $('.user_info').show(); 
                      
                  })
                  .fail(function(){
                      alert('服务器超时！')
                  })
              })
          </script>
          <title>天天生鲜-首页</title>
      </head>
      <body>
      
          <!--  页面顶部   -->
          <div class="header_con">
              <div class="header">
                  <div class="welcome fl">欢迎来到天天生鲜!</div>
                  
                  <div class="top_user_info fr">
                      <div class="user_login_btn fl">
                          <a href="">登录</a>
                          <span>|</span>
                          <a href="">注册</a>
                      </div>
      
                      <div class="user_info fl">
                          欢迎您：<em>张山</em>
                      </div>
      
                      <div class="user_link fl">
                          <span>|</span>
                          <a href="">我的购物车</a>
                          <span>|</span>
                          <a href="">我的订单</a>
                      </div>
                  </div>
              </div>      
          </div>
      
      
          <div class="center_con">
              <!--  logo   -->
              <a href="index.html" class="logo fl"><img src="images/logo.png" alt="天天生鲜网站logo"></a>
      
              <!--  搜索     -->
              <div class="search_con fl"> 
                  <form>              
                      <input type="text" name="" class="input_txt fl" placeholder="搜索">
                      <input type="submit" name="" value="搜素" class="input_sub fr">
                  </form>
              </div>
              
              <!--  购物车    -->
              <div class="chart_con fr">
                  <a href="#" class="fl">我的购物车</a>
                  <span class="fr">5</span>
              </div>
          
          </div>
      
          <div class="main_menu_con">     
              <div class="main_menu">
                  <h2 class="fl">全部商品分类</h2>
                  <ul class="fl">
                      <li><a href="">首页</a></li>
                      <li><a href="">手机生鲜</a></li>
                      <li><a href="">抽奖</a></li>
                  </ul>
              </div>
          </div>
      
          <div class="center_con2">
              
              <ul class="sub_menu fl">
                  <li><a href="">新鲜水果</a></li>
                  <li><a href="" class="icon02">新鲜水果</a></li>
                  <li><a href="" class="icon03">新鲜水果</a></li>
                  <li><a href="" class="icon04">新鲜水果</a></li>
                  <li><a href="" class="icon05">新鲜水果</a></li>
                  <li><a href="" class="icon06">新鲜水果</a></li>
              </ul>
      
      
              <div class="slide fl">
                  
                  <ul class="slide_list">
                      <li><a href=""><img src="images/slide.jpg" alt="专题图片" /></a></li>
                      <li><a href=""><img src="images/slide02.jpg" alt="专题图片" /></a></li>
                      <li><a href=""><img src="images/slide03.jpg" alt="专题图片" /></a></li>
                      <li><a href=""><img src="images/slide04.jpg" alt="专题图片" /></a></li>
                  </ul>
      
                  <div class="prev"></div>
                  <div class="next"></div>
      
                  <ul class="points">
                      <!-- <li class="active"></li>
                      <li></li>
                      <li></li>
                      <li></li> -->
                  </ul>
              </div>
              <div class="adv fr">
                  <a href="#"><img src="images/adv01.jpg" alt="广告图片"></a>
                  <a href="#"><img src="images/adv02.jpg" alt="广告图片"></a>
              </div>
          </div>
      
          
          <div class="common_model">
              
              <div class="common_title">
                  <h3>新鲜水果</h3>
                  <ul>
                      <li><span>|</span></li>
                      <li><a href="">加州提子</a></li>
                      <li><a href="">加州提子</a></li>
                      <li><a href="">加州提子</a></li>
                  </ul>
                  <a href="#" class="more fr">查看更多 &gt;</a>
              </div>
      
      
              <div class="common_goods_con">
                  <div class="common_banner fl">
                      <a href="#"><img src="images/banner01.jpg" alt="商品banner"></a>
                  </div>
      
                  <ul class="common_goods_list fl">
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                  </ul>
              </div>
          </div>
      
      
          <div class="common_model">
              
              <div class="common_title">
                  <h3>海鲜水产</h3>
                  <ul>
                      <li><span>|</span></li>
                      <li><a href="">加州提子</a></li>
                      <li><a href="">加州提子</a></li>
                      <li><a href="">加州提子</a></li>
                  </ul>
                  <a href="#" class="more fr">查看更多 &gt;</a>
              </div>
      
      
              <div class="common_goods_con">
                  <div class="common_banner fl">
                      <a href="#"><img src="images/banner02.jpg" alt="商品banner"></a>
                  </div>
      
                  <ul class="common_goods_list fl">
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                  </ul>
      
              </div>
      
      
      
          </div>
      
          <div class="common_model">
              
              <div class="common_title">
                  <h3>猪牛羊肉</h3>
                  <ul>
                      <li><span>|</span></li>
                      <li><a href="">加州提子</a></li>
                      <li><a href="">加州提子</a></li>
                      <li><a href="">加州提子</a></li>
                  </ul>
                  <a href="#" class="more fr">查看更多 &gt;</a>
              </div>
      
      
              <div class="common_goods_con">
                  <div class="common_banner fl">
                      <a href="#"><img src="images/banner03.jpg" alt="商品banner"></a>
                  </div>
      
                  <ul class="common_goods_list fl">
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                      <li>
                          <h4>草莓</h4>
                          <a href="#"><img src="images/goods_pic.jpg" alt="商品图片"></a>
                          <div>¥ 38.00</div>
                      </li>
                  </ul>
              </div>
          </div>
      
          <div class="footer">
              
              <div class="footer_links">
                  <a href="">关于我们</a>
                  <span>|</span>
                  <a href="">联系我们</a>
                  <span>|</span>
                  <a href="">招聘人才</a>
                  <span>|</span>
                  <a href="">友情链接</a>
              </div>
      
              <p class="copyright">
                  CopyRight © 2016 北京天天生鲜信息技术有限公司 All Rights Reserved<br />
      电话：010-****888    京ICP备*******8号
      
              </p>
          </div>
      
      
      
      </body>
      </html>
      ```

2. slide2.js   //js文件

   1. ```
      $(function(){
      	var $slide = $('.slide');
      
      	//选取所有的幻灯片的集合
      	var $li = $('.slide_list li');
      
      	//获取幻灯片的个数
      	var $len = $li.length;
      
      	//选择小圆点的容器
      	var $points_con = $('.points');
      
      	// 要运动过来的幻灯片的索引值
      	var nowli = 0;
      
      	// 要离开的幻灯片的索引值
      	var prevli = 0;
      
      	var $prev = $('.prev');
      	var $next = $('.next');
      
      	var timer = null;
      
      	var ismove = false;
      
      	// 根据幻灯片的个数，动态创建小圆点
      	for(var i=0;i<$len;i++)
      	{
      		var $newli = $('<li>');
      
      		//第一个小圆点含有'active'的样式
      		if(i==0)
      		{
      			$newli.addClass('active');
      		}
      		$newli.appendTo($points_con);
      	}
      
      	//第一个幻灯片不动，将其他的幻灯片放到右边去
      	$li.not(':first').css({'left':760});
      	// 获取小圆点
      	var $points = $('.points li');
      
      	//小圆点点击切换幻灯片
      	$points.click(function(){
      		nowli = $(this).index();
      		// 修复重复点击的bug
      		if(nowli==prevli)
      		{
      			return;
      		}
      		$(this).addClass('active').siblings().removeClass('active');
      		move();
      	})
      
      	//向前的按钮点击切换幻灯片
      	$prev.click(function(){
      
      		if(ismove)
      		{
      			return;
      		}
      
      		ismove = true;
      
      		nowli--;
      		move();
      		$points.eq(nowli).addClass('active').siblings().removeClass('active');
      	})
      
      	//向后的按钮点击切换幻灯片
      	$next.click(function(){
      
      		if(ismove)
      		{
      			return;
      		}
      
      		ismove = true;
      
      		nowli++;
      		move();
      		$points.eq(nowli).addClass('active').siblings().removeClass('active');
      
      	})
      
      
      	timer = setInterval(autoplay,5000);
      
      
      	$slide.mouseenter(function(){
      		clearInterval(timer);
      	});
      
      
      	$slide.mouseleave(function(){
      		timer = setInterval(autoplay,3000);
      	});
      
      
      	function autoplay(){		
      		nowli++;
      		move();
      		$points.eq(nowli).addClass('active').siblings().removeClass('active');
      	}
      
      
      
      	// 幻灯片运动函数，通过判断nowli和prevli的值来移动对应的幻灯片
      	function move(){
      		// 第一张幻灯片往前的时候
      		if(nowli<0)
      		{
      			nowli = $len-1;
      			prevli = 0;
      			$li.eq(nowli).css({'left':-760});
      			$li.eq(nowli).animate({'left':0});
      			$li.eq(prevli).animate({'left':760},function(){
      				ismove = false;
      			});
      			prevli = nowli;
      			return;
      		}
      
      		//最后一张幻灯片再往后的时候
      		if(nowli>$len-1)
      		{
      			nowli = 0;
      			prevli = $len-1;
      			$li.eq(nowli).css({'left':760});
      			$li.eq(nowli).animate({'left':0});
      			$li.eq(prevli).animate({'left':-760},function(){
      				ismove = false;
      			});
      			prevli = nowli;
      			return;
      		}
      
      		// 幻灯片从右边过来
      		if(nowli>prevli)
      		{	
      			// 把要过来的幻灯片先放到右边去
      			$li.eq(nowli).css({'left':760});			
      			$li.eq(prevli).animate({'left':-760});			
      		}
      		else //幻灯片从左边过来
      		{
      			// 把要过来的幻灯片先放到左边去
      			$li.eq(nowli).css({'left':-760});		
      			$li.eq(prevli).animate({'left':760});			
      		}
      
      		$li.eq(nowli).animate({'left':0},function(){
      			ismove = false;
      		});
      		prevli = nowli;
      
      
      	}
      })
      ```

   2.  $li.not(':first').css({'left':760});    第一个幻灯片不动，其他的幻灯片放到右边去

## 11-ajax和jsonp

#### 01  json	

1.  json,  JavaScript Object Notation 的首字母,  javascript对象表示, 一种数据格式

   1.  前后端交换数据的一种格式, 轻量级的文本数据交换格式.  属性和值 组成
       1.   JSON字符串: 一般是后端向前端传输的格式。
       2.  JSON对象：一般是前端希望得到的
       3.  JSON字符串与JSON对象之间的转换
           1.  `JSON.parse(JSON格式的字符串)`：正解析，JSON字符串转换为JSON对象
           2.   `JSON.stringify(JSON对象)`：反解析，JSON对象转换为JSON字符串 
       
       1.  JSON 用 JavaScript 语法描述数据对象
       2.  JSON 独立于语言和平台。JSON 解析器和 JSON 库支持许多不同的编程语言。
       3.  JSON标准不允许有[字节序掩码](https://zh.wikipedia.org/wiki/字节序掩码)，不支持注释

2.  JSON数据类型

   1.  2种结构类型 : 大括号 {} 对象类型（object）, 中括号 [] 数组类型

       1.  数组：有序的零个或者多个值。每个值为任意类型。方括号`[，]`括起来。元素间用逗号` , `分割。如：[value1, value2],  ["tom",18,"programmer"]  

       2.  对象：花括号{}内的“键-值对”(key-value pairs)， 键只能是字符串。键-值对用逗号` , `分隔。键与值之间用冒号`:`分割。 无序.

           1.  ```
               {
                   "name":"tom",
                   "age":18
               }
               ```

               1. 属性名 字符串值 用**双引号** ,  单引号或不用引号 错误

           2.  被理解为 对象（object） ，纪录（record），结构（struct），字典（dictionary），哈希表（hash table），有键列表（keyed list），或者关联数组 （associative array）。

   2.  5种基本数据类型：string, number, true, false,null

       1.  字符串类型,  用双引号, ` ""`, 支持反斜杠的转义字符序列
       2.  数字类型，直接写数字, 数字用双引号， 变成了字符串。
       3.  true和false是布尔型
       4.  空，关键字null表示

   

3.  JSON文档的根节点必须是一个对象或一个数组

    1.  json文档不是 json是不同的概念
    2.  json数据 由键值对构成的
        1.  键用字符串
        2.  值的类型：
          1. 数字（整数或浮点数）
          2. 字符串（ 双引号中）
          3. 逻辑值（true 或 false）
          4. 数组（ 方括号中）	{"persons":[{},{}]}
          5. 对象（ 花括号中） {"address":{"province"："陕西"....}}
          6. null
        3.  数据由逗号分隔： 键值对由逗号分隔
        4.  花括号保存对象：使用{}定义json 格式
        5.  方括号保存数组：[]
            

    

4.  json获取数据

   1.  语法

      1.  json对象.键名
      2.  json对象[“键名”]
      3.  数组对象[索引]
      4.  遍历

   2.  ```
      <head>
          <meta charset="UTF-8">
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript">
          	//自定义json对象
              var person = {"name": "张三", age: 23, 'gender': true};
      
              //获取person对象中所有的键和值
              //for in 循环
              for(var key in person){
                  //这样的方式获取不行。因为相当于  person."name"
                  //alert(key + ":" + person.key); //显示name:undefined,age:undefined,gender:undefined
                  alert(key+":"+person[key]);
              }
          </script>
      </head>
      ```
      

5.  javascript对象初始化器方式 自定义对象, 字符串值  用 单引号. 属性名不用 引号, 与json数据相区别

   1.  ```
      var oMan = {
          name:'tom',
          age:16,
          talk:function(s){
              alert('我会说'+s);
          }
      }

6.  jquery或 javascript 不能操作文件, 安全考虑,  

#### 02 node.js

1.  node.js 

   1.  简介:  Node.js 是一门技术, 不是学习一种新的语言

       1.  执行 [JavaScript](https://so.csdn.net/so/search?from=pc_blog_highlight&q=JavaScript) 代码的工具. 工具是安装在计算机操作系统之上的软件
       2.  Node.js 构建服务器端应用和创建前端工程化工具

   2.  为什么浏览器和 Node.js 都可以运行 JavaScript

       1.  浏览器和 Node.js 都内置了 JavaScript V8 Engine
       2.  DOM 和 DOM 是浏览器环境中特有的, 是JavaScript V8 Engine 中添加的控制浏览器窗口以及 HTML 文档的 API
       3.  Node.js 中没有 DOM 和 BOM 的. 即 Node.js 中不能执行和它们相关的代码，比如 `window.alert()` 或 `document.getElementById()`
       4.  Node.js 中添加了很多系统级别的 API，如操作系统中的文件和文件夹
       5.  小结
           1.  JavaScript 在浏览器中控制的是浏览器窗口和 DOM 文档。
           2.  JavaScript 在 Node.js 中控制的操作系统级别的内容。
           3.  浏览器运行在用户的操作系统中的，如果能控制系统级别的 API 存在安全问题。Node.js 运行在远程的服务器中，访问的是服务器系统 API，不存在安全问题。

   3.  配置服务器环境

       1.  下载node-v14.6.0-x64.msi”文件， 安装Node.js , 傻瓜式安装.

       2.  打开cmd,  cd 进入 `server.js`所在的目录

       3.  `node -v` , 显示node.js版本, 验证安装成功

       4.  `npm -v` 显示npm版本号

       5.  输入 `node server.js` , 启动服务器

       6.  static file server running at http://localhost:8888/

       7.  该目录中新建 index.html

          1.  ```
             <!DOCTYPE html>
             <head>...</head>
             <body>
             	<h1> test server </h1>
             </body>
             </html>

       8.  浏览器中  输入: `http://localhost:8888/`,   启动了 index.html

       

2.  npm

    1.  Node.JS的包管理工具（package manager）。JavaScript （运行在 Node.js 上）写的,全称 Node Package Manager
    2.  为什么要一个这样的包管理工具？
        1.  node.js开发用到很多别人写的javascript代码。以前是根据名称，搜索官方网站，下载代码，解压，再使用，非常的繁琐。
        2.  npm解决这个问题而产生的一个工具。自己开发的模块打包放到npm官网，要用，通过npm直接用

    3.  安装: nodejs安装的时顺带安装了 npm
    4.  使用案例 : 用npm安装一个jquery  步骤:
        1.  一个项目，名称: Project，在D盘。
        2.  打开cmd，进入D盘的Project项目的目录
        3.  输入 `npm i jquery`(注意大小写会影响)
        4.  这时，Project的文件夹里面就会有query文件。


#### 03 $.ajax() 

1.  javascript : 运行于客户机上, 不能操作文件( 安全考虑, 故意为之)

2.  ajax : 全名 async javascript and XML(异步JavaScript和XML)

   1.  是技术 : 是js与服务器端交互的手段
   2.  与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下
   3.  目的: ajax 让javascript发送http请求，实现ajax与后台通信，获取数据
   4.  原理: ajax 实例化xmlhttp对象，ajax 用此对象与后台通信
   5.  特点: ajax异步通信 不影响后续javascript的执行. 即无需刷新页面便可向服务器传输或读写数据(又称无刷新更新页面), 这一特点得益于XMLHTTP组件的XMLHTTPRequest对象
       1.  同步, 异步
           1.  异步: 程序中, 同时做几件事
           2.  同步: 程序中,  做一件事后再做另一件事
           3.  与生活中 同步异步 含义相反

3.  ajax 又称作 局部刷新

   1.  ajax 局部刷新, 也叫 无刷新
   2.  过程:
      1. ajax 发送http请求，不通过浏览器的地址栏， 页面**不刷新**
      2. ajax 获取 后台数据，更新页面数据的部分，页面**局部刷新**

4.  同源策略

   1.  Ajax请求地址与当前页面的地址必须是同协议，同主机，同端口, 才发送Ajax请求，三者任何一个不一样，则此次请求是跨域请求，浏览器阻止这个请求行为
   2.  跨域解决方式
       1.  CORS
       * 服务器反向代理
       * JSONP

5.  原始js 实现方式 : 步骤

    1.  步骤1: 创建核心对象（查看W3C）

        1.  创建XMLHttpRequest对象

            1.  `xmlhttp = new XMLHttpRequest()`

            2.  XMLHttpRequest 对象方法描述

                1.  | 方法                                                         | 描述             |
                    | ------------------------------------------------------------ | ---------------- |
                    | abort()                                                      |                  |
                    | getAllResponseHeaders()                                      |                  |
                    | getResponseHeader("header")                                  |                  |
                    | open("method", "URL", [asyncFlag], ["userName"], ["password"]) |                  |
                    | send(content)                                                | 向服务器发送请求 |
                    | setRequestHeader("header","value")                           |                  |

            3.  XMLHttpRequest 对象属性描述(用于和服务器交换数据)

                1.  | 属性               | 描述                                                         |
                    | ------------------ | ------------------------------------------------------------ |
                    | onreadystatechange | 存储函数(或函数名), 当readyState改变时, 调用该函数           |
                    | readyState         | 存储XMLHttpRequest的状态, 0: 请求未初始化, 1: 服务器连接已建立, 2: 请求已接收, 3: 请求处理中, 4: 请求已完成, 且响应已就绪 |
                    | responseText       |                                                              |
                    | responseXML        | 获取XML形式响应数据                                          |
                    | responseBody       |                                                              |
                    | responseStream     |                                                              |
                    | status             |                                                              |
                    | statusText         | 获取字符串形式响应数据                                       |

                    

    2.  步骤2: `建立起连接`，如何实现

        1.  分为： 向服务器发送请求 ，接收服务器响应，处理数据

        2.  向服务器发送请求

            1.   用 `XMLHttpRequest` 对象的 `open()` 和 `send()` 方法

            2.  ```
                //GET方式, 请求参数在URL后边拼接。send方法为空参
                xmlhttp.open("GET","test1.txt?name=bill&age=18",true);  //请求方式, URL路径, 同步或异步
                xmlhttp.send();  //发送请求
                ```

            3.  ```
                //POST方式, 参数在send方法中定义
                xmlhttp.open("POST","test1.txt",true);  //请求方式, URL路径, 同步或异步
                xmlhttp.send("name=bill&age=18");
                ```

            4.  参数

                1.  | 方法                     | 描述                                                         |
                    | ------------------------ | ------------------------------------------------------------ |
                    | open(method, url, async) | method: GET或 POST, url: 文件在服务器上的位置, async : true(异步), false(同步) |
                    | send(string)             | string :  用于POST请求                                       |

        3.  接收服务器响应，处理数据

            1.  有两种方式（这里只说`responseText 属性`）

            2.  ```
                xmlhttp.onreadystatechange=function()
                {
                if (xmlhttp.readyState==4 && xmlhttp.status == 200)
                	{
                	document.getElementById("myDiv").innerHTML = xmlhttp.responseText;
                	}
                }
                ```

                

            

6.   jQuery实现方式 : `$.ajax()`  

   1. 原生JS实现太繁琐.

   2. 语法: `$.ajax({键值对});`

      1. 以前写法

         1. ```
            <script type="text/javascript">
                $.ajax({
                   url: "http://www.hzhuti.com",    //请求的url地址
                   dataType: "json",   //返回格式为json
                   async: true, //请求是否异步，默认为异步，这也是ajax重要特性
                   data: {
                             "id": "value"
                         },    //参数值
                  type: "GET",   //请求方式
                  beforeSend: function() {
                      //请求前的处理
                  },
                  success: function(req) {
                      //请求成功时处理，一般只用到这一个
                  },
                  complete: function() {
                      //请求完成的处理
                  },
                  error: function() {
                      //请求出错处理
                  }
                });
            </script>
            ```

            1. 参数
               1. url 请求地址
               2. type 请求方式， 'GET'， 'POST'
               3. dataType  返回的数据格式， 'json'格式，也可为'html'
               4. data 设置发送给服务器的数据
               5. success()  请求成功后的回调函数 
               6. error()  请求失败后的回调函数
               7. async  是否异步，默认值是'true'，表示异步

         2. 案例2

            1. ```
               $.ajax({
                   url: 'js/data.json',
                   type: 'GET',
                   dataType: 'json',
                   data:{'aa':1}
                   success:function(data){
                       alert(data.name);
                   },
                   error:function(){
                       alert('服务器超时，请重试！');
                   }
               });

      2. 新写法

         1. ```
            <head>
            	<meta charset="UTF-8">
            	<title>Document</title>
            	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
            	<script type="text/javascript">
                    $.ajax({
                        url: 'js/data.json',
                        type: 'GET',
                        dataType: 'json',
                        data:{'aa':1}
                    })
                    .done(function(data) {
                        alert(data.name);
                    })
                    .fail(function() {
                        alert('服务器超时，请重试！');
                    });
            	</script>
            </head>
            <body>
            </body>
            ```

         2. data.json文件的内容 `{"name":"tom","age":18}`

   3. `$.get() 方式`：发送get请求

      1. `$.get(url, [data], [callback], [type])`

           1. 参数：
                1. url：请求路径
                2. data：请求参数
                3. callback：回调函数
                4. type：响应结果的类型

      2. ```
         <head>
             <script src="js/jquery-1.12.4.min.js"></script>
         </head>
         <script>
             function fun() {
                 $.get(
                     //url：请求路径
                     url ="./js/data.json",
                     //data：请求参数
                     {"username":"jack"},
                     //callback：回调函数
                     function (data) {
                         alert(111)
                     },
                     //type：响应结果的类型
                     type="text"
                 )
             }
         </script>
         <body>
         <input type="button" value="点击" onclick=fun();>
         <input type="text">
         </body>
         ```

   4. `$.post()方式`：发送post请求

      1. `$.post(url, [data], [callback], [type]) `
         1. 参数:
            1. url：请求路径
            2. data：请求参数
            3. callback：回调函数
            4. type：响应结果的类型

   5. index.html

      1. ```
         <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
         <html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
         <head>
         	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
         	<link rel="stylesheet" type="text/css" href="css/reset.css">
         	<link rel="stylesheet" type="text/css" href="css/main.css">
         	<script type="text/javascript" src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/slide2.js"></script>
         	<script type="text/javascript">
         		$(function(){
         			$.ajax({
         				url:'js/data.json',
         				type:'get',
         				dataType:'json'
         			})
         			.done(function(dat){
         
         				$('.user_login_btn').hide();
         
         				$('.user_info em').html(dat.name);
         
         				$('.user_info').show();	
         				
         			})
         			.fail(function(){
         				alert('服务器超时！')
         			})
         		})
         	</script>
         	<title>天天生鲜-首页</title>
         </head>
         <body>
         
         	<!--  页面顶部	 -->
         	<div class="header_con">
         		<div class="header">
         			<div class="welcome fl">欢迎来到天天生鲜!</div>
         			
         			<div class="top_user_info fr">
         				<div class="user_login_btn fl">
         					<a href="">登录</a>
         					<span>|</span>
         					<a href="">注册</a>
         				</div>
         
         				<div class="user_info fl">
         					欢迎您：<em>张山</em>
         				</div>
         				.......
         			</div>
         		</div>		
         	</div>
         	......
         </body>
         </html>				
         ```

         

#### 05 jsonp 跨域请求

1. jsonp 与 ajax

   1. jsonp 跨域数据 请求,  ajax 同域 数据请求,. 

      1.  jsonp和ajax原理完全不一样, jquery将它们封装成同一个函数

          1.  用 JSON 模式请求数据，服务端返回的是一串 JSON 类型的数据，
          2.  用 JSONP 模式来请求数据的时候服务端返回的是一段可执行的 JavaScript 代码

      2.  jsonp要点: 用户传递一个callback参数给服务端，然后服务端返回数据时将callback参数作函数名包裹住JSON数据

      3.  jsonp 跨域的原理是: 动态加载 script 的 src, ”src”属性的标签有跨域的能力. 所以只能把参数通过 url 的方式传递 , 所以 jsonp 的 type 类型只能是 get 

          1. 示例

             1.  ```
                 $.ajax({
                     url: 'http://192.168.1.114/yii/demos/test.php', // 不同的域
                     type: 'GET', // jsonp 模式只有 GET 是合法的
                     data: {
                         'action': 'aaron'
                     },
                     dataType: 'jsonp', // 数据类型
                     jsonp: 'backfunc', // Jquery生成验证参数的名称.即链接左边的名称
                     jsonpCallback: "showData",  //指定回调函数名称，即链接右边的名称，可不写随机生成。
                 })
                 ```

             2.  jquery 内部会转化成`
                 http://192.168.1.114/yii/demos/test.php?backfunc=jQuery2030038573939353227615_1402643146875&action=aaron`

             3.  然后动态加载 `<script type="text/javascript"src="http://192.168.1.114/yii/demos/test.php?backfunc= jQuery2030038573939353227615_1402643146875&action=aaron"></script>`, 然后后端执行 backfunc( 传递参数 ) ，数据通过实参的形式发送出去

             4.  JSONP 模式请求数据的整个流程：

                 1.  客户端发送一个请求，规定一个可执行的函数名（这里 jQuery 做了封装的处理，自动生成回调函数并把数据取出来供 success 属性方法调用 , 不是传递一个回调句柄）
                 2.  服务器端接受这个 backfunc 函数名，把数据用实参的形式发送出去

             5.  jquery 源码中， jsonp 的实现方式: 动态添加` <script>` 标签调用服务器的 js 脚本实现的。

                 1.   jquery 在 window 对象中加载一个全局的函数，当 `<script>` 代码插入时函数执行，执行完毕后就 `<script>` 会被移除。
                 2.  jquery 还对非跨域的请求进行了优化，域名请求像正常的 Ajax 请求一样工作。

   2. JSONP原理

      1. 开始 前端部分

         1. 判断请求与当前页面的域是否同源，如果同源，则发送正常的Ajax，就没有跨域的事情了；
         2. 如果不同源，会生成一个script标签
         3. 生成一个随机的callback名字，还得创建一个名为该名字的方法
         4. 设置script标签的src，设置为要请求的接口
         5. 将callback作为参数拼接在后面

      2. 后端部分

         1. 后端接收到请求后，开始准备要返回的数据
         2. 后端拼接数据，将要返回的数据用callback的值和括号包裹起来
            1. 如：callback=asd123456，要返回的数据为{“a”: 1, “b”: 2}, 拼接为：asd123456({“a”: 1, “b”: 2});

      3. 最后 后端部分

         1. 浏览器接收到内容，作js代码执行
         2. 执行名为asd123456的方法，这样就接收到了后端返回给我们的对象

      4. JS封装jsonp前后端数据交互

         1. ```
            var $ = {
                ajax: function(options){
                    var url = options.url;
                    var type = options.type;
                    var dataType = options.dataType;
                    //判断是否同源(协议、域名、端口号)
                    //获取目标url的域
                    var targetProtocol = "";//目标接口的协议
                    var targetHost = "";//目标接口的host，host是包含域名和端口的
                    //如果url中不带http，那么访问的一定是相对路径，相对路径一定是同源的
                    if (url.indexOf("http://") === 0 || url.indexOf("https://") === 0){
                        var targetUrl = new URL(url);
                        targetProtocol = targetUrl.protocol;
                        targetHost = targetUrl.host;
                    }else{
                        targetProtocol = location.protocol;
                        targetHost = location.host;
                    }
                    //首先判断是否为jsonp，因为不是jsonp不用做其它的判断，直接发送ajax
                    if (dataType === 'jsonp'){
                        //要看是否同源
                        if (location.protocol === targetProtocol && location.host === targetHost){
                            //此处省略，因为同源，jsonp会当作普通的ajax做请求
                        }else{
                            //不同源，跨域
                            //随机生成一个callback
                            var callback = 'cb' + Math.floor(Math.random() * 1000000);
                            //给window上添加一个方法
                            window[callback] = options.success;
                            //生成script标签
                            var script = document.createElement('script');
                            if (url.indexOf("?") > 0){
                                //表示已经有参数了
                                script.src = url + "callback=" + callback;
                            }else {
                                //表示没有参数
                                script.src = url + "?callback=" + callback;
                            }
                            script.id = callback;
                            document.head.appendChild(script);
                        }
                    }
                }
            }
            
            
            
            $.ajax({
                url: "http://developer.duyiedu.com/edu/testJsonp",
                type: "get",
                dataType: 'jsonp',
                success: function(data){
                    console.log(data);
                }
            })

   3. 语法 

      1. 第一种方式

         1. ```
            <script type="text/javascript">
                $.ajax({
                    url:'js/data.js', //请求地址
                    type:'get',
                    dataType:'jsonp',  //jsonp请求必须指定数据类型为jsonp
                    jsonp:'fnBack'  //回调函数名,默认jQuery123545_43456的随机字符串，可以自定义
                    data:'参数名'//服务器接收的回调函数的参数名如callback ,cb等等默认callback
                })
                .done(function(data){
                    alert(data.name);
                })
                .fail(function() {
                    alert('服务器超时，请重试！');
                });
            </script>
            ```

         2. data.js中数据  `fnBack({"name":"tom","age":18});`

      2. 第二种方式  ` $.getJSON('地址','回调函数')`

   

3. 360联想词获取

   1. 360联想词获取.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript">
         
         		// https://sug.so.360.cn/suggest?callback=suggest_so&encodein=utf-8&encodeout=utf-8&format=json&fields=word&word=d
         
         		// https://sug.so.360.cn/suggest?
         		$(function(){
         
         			$('#input01').keyup(function(){
         
         				var $val = $(this).val();
         
         				$.ajax({
         					url:'https://sug.so.360.cn/suggest?',
         					type:'get',
         					dataType:'jsonp',
         					data:{'word':$val}
         				})
         				.done(function(data){
         					//console.log(data);
                             
         					// 清空元素里面的内容
         					$('#list01').empty();
         					var dat = data.s;
         					for(var i=0;i<dat.length;i++)
         					{
         						var $newli = $('<li>');
         
         						$newli.html(dat[i]);
         
         						$newli.appendTo($('#list01'));
         					}
         				})
         			})
         		})
         	</script>
         </head>
         <body>
         	<input type="text" name="" id="input01">
         	<ul id="list01">	</ul>
         </body>
         </html>
         ```
         
         1.  keyup() 方法

## 12 Jquery-ui和本地存储

#### 01 $.cookie()

1. cookie本地存储

   1. cookie()函数,  `jquery.cookie.js`包
   2. 新增的localStorage 对象, sessionStorage对象.  是存储json类型的对象

2. cookie

   1. 特征

      1. 存储在本地，容量最大4k
      2. 同源http请求时携带传递
      3. 可设置访问路径，只有此路径及此路径的子路径才能访问此cookie
      4. 过期时间之前有效

   2. cookie构成

      1. `name=value;[expires=date];[path=path];[domain=somewhere.com];[secure],`
         1. expires:过期时间
            必须填写日期对象（过期日期） 默认时间是会话，会话结束后就清除
            注：系统会自动清除过期的cookie
         2. path 限制访问路径
            如果不设置path，会默认设置当前路径
            注：我们设置的cookie的路径，和加载的当前文件的路径。必须一致，如果不一致，cookie访问失败
         3. domain 限制访问域名：
            如果不设置domain，默认当前服务器的域名、ip
            注：如果当前加载文件域名和设置的域名不一样，设置cookie失败
         4. secure 安全
            如果不设置。设置cookie 可以通过http协议加载文件设置
            也可以通过https协议加载文件设置
            如果设置了安全 只能设置https协议加载cookie

   3. juqery的 `$.cookie()`的使用方法

      1. $.cookie() 使用方法

         1. 具体调用格式
            1. `$.cookie(name)`   获取name取值
            2. `$.cookie(name,value)` 设置name和value
            3. `$.cookie(name,value {undefined
               可选项
               raw:true    //raw:true value 不进行编码 默认是false进行编码
               })`
            4. `$.cookie(name，null)`    删除cookie

      2. jquery 设置cookie

         1. ```
            $.cookie('mycookie','123',{expires:7,path:'/'})
            ```

            1.  expires:7,7天过期
            2. path:'/'  , / 根目录

      3. jquery实现cookie示例

         1. ```
            <head>
                <meta charset="UTF-8">
                <title>Document</title>
                <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
                <script type="text/javascript" src="js/jquery.cookie.js"></script>
                <script type="text/javascript">
            
                    $.cookie("变种人","x教授",{ //添加cookie
                        //可选项
                        expires:7,    //7天后过期
                        raw:true
                    })
                     
                    $.cookie("赛亚人","贝吉塔")
                     
                    $.cookie("海贼王","路飞",{
                        expires:1
                    })
            
                    alert($.cookie("变种人")); //显示cookie
                    alert($.cookie("赛亚人"));
                    alert($.cookie("海贼王")); 
            
                    $(function(){  //删除cookie
                        $("button").click(function(){
                            $.cookie("变种人",null);
                            alert($.cookie("变种人"));
                        })
                    })
                </script>
            </head>
            ```

   4. testcookie.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/jquery.cookie.js"></script>
         	<script type="text/javascript">
         
         		// 设置cookie 过期时间为7天，存在网站根目录下
         		//$.cookie('mycookie','ok!',{expires:7,path:'/'});
         
         		//读取cookie
         		var mycookie = $.cookie('mycookie');
         		alert(mycookie);
         
         	</script>
         </head>
         <body>
         	<h1>测试页面</h1>
         </body>
         </html>
         ```

   5. [jquery](https://so.csdn.net/so/search?from=pc_blog_highlight&q=jquery).cookie.js 方法设置和读取cookie值, 必须将html等文件放在服务器上访问，不然设置cookie之后读取结果为undefined,这和ajax一样，考虑到网络安全问题

3. localStorage 对象

   1. localStorage

      1.  存储在本地，容量为5M或者更大
      2. 请求时 不 携带传递
      3. 同源窗口中共享
      4. 数据一直有效，除非人为删除，可作为长期数据

   2. 使用方法

      1. 设置：

         1. ```
            localStorage.setItem("dat", "456");
            ```

         2. ```
            localStorage.dat = '456';
            ```

      2. 获取：

         1. ```
            localStorage.getItem("dat");
            ```

         2. ```
            localStorage.dat
            ```

      3. 删除

         1. ```
            localStorage.removeItem("dat");
            ```

4. sessionStorage 对象

   1. sessionStorage
      1. 存储在本地，容量为5M或者更大
      2. 请求时候不携带传递
      3. 同源的当前窗口关闭前有效
   2. sessionStorage, localStorage 是H5的新功能

5. Web Storage 对象

   1. localStorage 和 sessionStorage 合称

      1.  支持事件通知机制，可以将数据更新的通知监听者
      2.  Web Storage的api接口使用更方便。
      3.  iPhone的无痕浏览不支持Web Storage，只能用cookie
      
   2. webstorage.html
   
      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
             <meta charset="UTF-8">
             <title>Document</title>
             <script type="text/javascript">
                 //设定值: (键, 值)
                 localStorage.setItem("dat", "456");   //在客户端
                 alert(localStorage.getItem("dat"));
         
                 sessionStorage.setItem('dat1','789');   //在服务器端
                 alert(sessionStorage.getItem("dat"));
         
             </script>
         </head>
         <body>
             <h1>测试webstorage</h1>
         </body>
         </html>
         ```



#### 02只弹一次的弹框

1.  使用 cookie() 保存信息, 设置一次弹框

2.  003readtip.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/jquery.cookie.js"></script>
      	<script type="text/javascript">
      		
      		$(function(){
      
      			var hasread = $.cookie('hasread');
      			//alert(hasread);
      
      			// 没有存cookie, undefined， 弹框
      			if(hasread==undefined)    
      			{
      				$('.pop_con').show();				
      			}
      			
      			//用户点击知道后，存cookie，把弹框关掉
      			$('#user_read').click(function(){
      				$.cookie('hasread','read',{expires:7,path:'/'});   //设置
      				$('.pop_con').hide();
      			})
      		})
      
      
      	</script>
      	<script type="text/javascript">
      		
      
      	</script>
      	<style type="text/css">
      
      		.pop_con{
      			display:none;
      		}		
      		.pop{
      			position:fixed;
      			width:500px;
      			height:300px;
      			background-color:#fff;
      			border:3px solid #000;
      			left:50%;
      			top:50%;
      			margin-left:-250px;
      			margin-top:-150px;
      			z-index:9999;
      		}
      
      		.mask{
      			position:fixed;
      			width:100%;
      			height:100%;
      			background-color:#000;
      			opacity:0.3;
      			filter:alpha(opacity=30);
      			left:0;
      			top:0;
      			z-index:9990;
      
      		}
      
      		.close{
      			float:right;
      			font-size:30px;
      		}
      
      
      
      	</style>
      </head>
      <body>
      
      	<div class="pop_con">
      		<div class="pop">
      			亲！本网站最近有优惠活动！请强力关注！			
      			<a href="#" id="close" class="close">×</a>
      
      			<a href="javascript:;" id="user_read">朕知道了！</a>
      		</div>
      		<div class="mask"></div>
      	</div>
      
      	<h1>网站内容</h1>
      </body>
      </html>
      ```
      
      1.  undefined 是 javascript的 常量值
      2.  $('.pop_con').show();   //js函数
      3.  $('.pop_con').hide();   //js函数

#### 03 jquery-ui.min.js包

1. jquery-ui.min.js

   1. 以 jQuery 为基础的代码库,  `jquery-ui.min.js`包
   2. 拖拽, 交互、动画、特效. 直接构建有交互性的web应用程序
   3. http://jqueryui.com/

2. jquery-ui应用示例: $(selector).draggable() -拖拽 

   1. 拖拽.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/jquery-ui.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         
         			$('.box').draggable({
         
         				// 限制在x轴向拖动
         				//axis:'x',
         
         				// 限定在父级的范围内拖动
         				containment:'parent',
         
         				drag:function(ev,ui){
         					//console.log(ui);
         					//document.title = ui.position.left;
         
         					$('#shownumber').val(parseInt(100*(ui.position.left/600)))
         				}
         			});
         		})
         
         	</script>
         	<style type="text/css">
         		.con{
         			width:800px;
         			height:200px;
         			border:1px solid #000;
         			margin:50px auto 0;
         		}
         
         		.box{
         			width:200px;
         			height:200px;
         			background-color: gold;
         		}
         	</style>
         </head>
         <body>
         	<div class="con">
         		<div class="box"></div>
         	</div>
         	<input type="text" name="" id="shownumber">
         </body>
         </html>
         ```

      2. $('.box').draggable({}) 的参数是 json格式

         1. 参数的每个元素: 是键值对
            1. $('.box').draggable({ axis:'x'})    //   实现拖拽效果,  字典数据做参数
            2. `drag:function(ev,ui){}`
            3. containment:'parent',   限定在父级的范围内拖动
            4. axis:'x'   限制在x轴			
   
   2. 自定义滚动条.html
   
      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>自定义滚动条</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/jquery-ui.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         
         			var h = $('.paragraph').outerHeight();
         
         			//整体文本的高度减去外面容器的高度
         			var hide = h-500;
         
         			$('.scroll_bar').draggable({
         				axis:'y',
         				containment:'parent',
         				opacity:0.6,
         				drag:function(ev,ui){
         					var nowtop = ui.position.top;
         					var nowscroll = parseInt(nowtop/290*hide);
         					$('.paragraph').css({top:-nowscroll});
         				}
         
         			});
         
         
         		})
         
         	</script>
         	<style type="text/css">
         		.scroll_con{
         			width:400px;
         			height:500px;
         			border:1px solid #ccc;
         			margin:50px auto 0;
         			position:relative;
         			overflow:hidden;
         		}
         
         		.paragraph{
         			width:360px;
         			position:absolute;
         			left:0;
         			top:0;
         			padding:10px 20px;
         			font-size:14px;
         			font-family:'Microsoft Yahei';
         			line-height:32px;
         			text-indent:2em;
         		}
         
         		.scroll_bar_con{
         			width:10px;
         			height:490px;
         			position:absolute;
         			right:5px;
         			top:5px;
         		}
         
         		.scroll_bar{
         			width:10px;
         			height:200px;
         			background-color:#ccc;
         			position:absolute;
         			left:0;
         			top:0;
         			cursor:pointer;
         			border-radius:5px;
         
         		}
         	</style>
         </head>
         <body>
         	<div class="scroll_con">
         		<div class="paragraph">
         			2005年以后，
         		</div>
         		<div class="scroll_bar_con">
         			<div class="scroll_bar">
         		</div>		
         	</div>	
         	</div>	
         </body>
         </html>
         ```

## 13-移动端js事件和swiper库

#### 01 zepto.js

1. 移动端touch 事件

   1. 包含 4 个子事件
      1. touchstart : //手指放屏幕上时触发
      2. touchmove : //手指屏幕上滑动式触发
      3. touchend : //手指离开屏幕时触发
      4. touchcancel : //系统取消touch事件的时触发，比较少用
   2. 三种操作: 点击、滑动、拖动
   
2. zepto.js, 移动端库

   1. 1.  轻量级,  高级浏览器的JavaScript库， 与jquery有 类似的api
   2. 目标,  移动端 一个精简的js库, 类似jquery, 与 jquery 的用法基本一样
      3.  经典, 但 逐渐不用的一个库
   
2.  004 zepto测试.html
   
   1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<script type="text/javascript" src="js/zepto.min.js"></script>
         	<script type="text/javascript">
         		$(function(){
         
         			alert( $('#div1').html() );
         
         		})
         	</script>
         </head>
         <body>
         	<div id="div1">这是一个div元素</div>
         </body>
         </html>
         ```
   
      

#### 02 swiper.js

1. swiper.js  : PC端和移动端的**滑动效果**插件 库

   1. 触屏焦点图、触屏整屏滚动
   2. 2.x版本和3.x版本
      1. 2.x版本 低版本浏览器(IE7)
      2. 3.x 应用在移动端。

2. swiper使用方法：

   1. css/reset.css

      1. ```
         /*  去掉标签默认的间距   */
         body,ul,ol,p,h1,h2,h3,h4,h4,h6,dl,dd,select,input,form{
         	margin:0;
         	padding:0;
         }
         /*  去掉小圆点以及数字   */
         ul,ol{
         	list-style:none;
         }
         /* 去掉下划线 */
         a{text-decoration:none;}
         
         /* 去掉斜体 */
         em{
         	font-style:normal;
         }
         /* 让h标签继承body的文字设置 */
         h1,h2,h3,h4,h4,h6{
         	font-size:100%;
         	font-weight:normal;
         }
         /* 在IE下去掉图片做链接时生成的框线 */
         img{
         	border:0px;
         }
         
         /* 清除浮动以及清除margin-top塌陷 */
         
         .clearfix:before,.clearfix:after{
         	content:'';
         	display:table;
         }
         .clearfix:after{
         	clear:both;
         }
         .clearfix{
         	zoom:1;
         }
         
         
         .fl{
         	float:left;
         }
         
         .fr{
         	float:right;
         }
         ```

   2. css/main.css

      1. ```
         .main_wrap{
         	position:absolute;
         	background-color:gold;
         	left:0px;
         	right:0px;
         	top:0px;
         	bottom:0px;
         }
         
         
         /* 顶部样式   */
         
         .header{
         	height:2.5rem;
         	background-color:#37ab40;
         	position:relative;
         }
         
         .logo{
         	display:block;
         	width:4.45rem;
         	height:1.8rem;
         	margin:0.35rem auto 0;
         }
         
         .logo img{
         	width:100%;
         }
         
         .search{
         	width:1.35rem;
         	height:1.35rem;
         	position:absolute;
         	right:0.75rem;
         	top:0.6rem;
         	background:url(../images/icons.png) left top no-repeat;
         	background-size:3.0rem 42.0rem;
         }
         
         
         /* 中间滚动区域的样式   */
         
         .center_con{
         	position:absolute;
         	left:0px;
         	right:0px;
         	top:2.5rem;
         	bottom:2.5rem;
         	background-color:#efefef;
             
            /* 只自动出现竖向滚动条，不出现横向滚动条  */
         	overflow-y:auto;
         	overflow-x:hidden;
         
         	padding-bottom:0.5rem;
         }
         
         
         .slide{
         	height:7.0rem;
         }
         
         .slide img{
         	width:100%;
         }
         
         
         .menu_con{
         	height:9.25rem;
         	background-color:#fff;
         	margin-top:0.5rem;
         	overflow:hidden;
         }
         
         .menu_con ul{
         	width:18rem;
         	height:8.375rem;
         	margin:0.6rem 0 0 1.375rem;	
         }
         
         .menu_con li{
         	width:2.8rem;
         	height:4.05rem;
         	float:left;
         	margin-right:1.625rem;
         }
         
         .menu_con li a{
         	display:block;
         	height:2.8rem;
         	background:url(../images/icons.png) left -3.025rem no-repeat;
         	background-size:3.0rem 42.0rem;
         }
         
         
         .menu_con li:nth-child(2) a{
         	background-position:left -6rem;
         }
         
         .menu_con li:nth-child(3) a{
         	background-position:left -9rem;
         }
         
         
         .menu_con li:nth-child(4) a{
         	background-position:left -12rem;
         }
         
         
         .menu_con li:nth-child(5) a{
         	background-position:left -15rem;
         }
         
         
         .menu_con li:nth-child(6) a{
         	background-position:left -18rem;
         }
         
         .menu_con li:nth-child(7) a{
         	background-position:left -21rem;
         }
         
         .menu_con li:nth-child(8) a{
         	background-position:left -24rem;
         }
         
         .menu_con li h2{
         	font:bold 13px/1.25rem 'Microsoft Yahei';
         	text-align:center;
         	color:#666;
         }
         
         
         .common_model{
         	height:17.25rem;
         	background-color:#fff;
         	margin-top:0.5rem;
         }
         
         .common_title{
         	width:17.75rem;
         	height:0.9rem;
         	margin:0.8rem auto 0;
         }
         
         .common_title h3{
         	float:left;
         	font:bold 15px/0.9rem  'Microsoft Yahei';
         	color:#fbc83d;
         	border-left:0.25rem solid #fbc83d;
         	text-indent:0.4rem;
         }
         
         .common_title a{
         	float:right;
         	font:normal 12px/0.9rem  'Microsoft Yahei';
         	color:#7f7f7f;
         }
         
         
         .banner{
         	display:block;
         	width:17.75rem;
         	height:4.5rem;
         	margin:0.5rem auto 0;
         }
         
         .banner img{
         	width:100%;
         }
         
         .goods_list{
         	width:17.75rem;
         	height:9.325rem;	
         	margin:0.5rem auto 0;
         }
         
         .goods_list li{
         	width:5.9rem;
         	height:9.325rem;
         	border-right:1px solid #e7e7e7;
         	float:left;
         	box-sizing:border-box;
         	position:relative;
         }
         
         .goods_list li:last-child{
         	border-right:0px;
         }
         
         
         .goods_link{
         	display:block;
         	width:4.5rem;
         	height:4.5rem;
         	margin:0.375rem auto 0;
         }
         
         .goods_link img{
         	width:100%;
         }
         
         .goods_list h4{
         
         	font:normal 15px/15px 'Microsoft Yahei';
         	width:5.0rem;
         	margin:0.925rem auto 0;
         	
         	overflow:hidden;
         
         	/* 强制文字不换行 */
         	white-space:nowrap;
         
         	/* 超出部分显示省略号   */
         	text-overflow:ellipsis;
         
         }
         
         .unit{
         	width:5.0rem;
         	font:normal 12px/12px 'Microsoft Yahei';
         	color:#bbb;
         	margin:0.8rem auto 0;
         }
         
         
         .price{
         	width:5.0rem;
         	font:normal 12px/12px 'Microsoft Yahei';
         	color:#ff4400;
         	margin:0.5rem auto 0;
         }
         
         
         .add_chart{
         	position:absolute;
         	width:1.7rem;
         	height:1.7rem;
         	right:0.675rem;
         	bottom:0;
         
         	background:url(../images/icons.png) left -27.0rem no-repeat;
         	background-size:3.0rem 42.0rem;
         }
         
         
         
         
         .footer{
         	position:absolute;
         	left:0;
         	bottom:0;
         	width:100%;
         	height:2.5rem;
         	background-color:#fff;
         
         }
         
         
         .footer li{
         	width:25%;
         	height:2.5rem;
         	float:left;
         }
         
         .footer li a{
         	display:block;
         	width:1.3rem;
         	height:1.3rem;
         	margin:0.35rem auto 0;
         	background:url(../images/icons.png) left -30.0rem no-repeat;
         	background-size:3.0rem 42.0rem;
         }
         
         .footer li:nth-child(2) a{
         	background-position:left -33rem;
         }
         
         .footer li:nth-child(3) a{
         	background-position:left -36rem;
         }
         
         .footer li:nth-child(4) a{
         	background-position:left -39rem;
         }
         
         .footer li h2{
         	font:normal 12px/0.8rem  'Microsoft Yahei';
         	color:#949392;
         	text-align:center;
         }
         
         
         .swiper-button-next,.swiper-button-prev {
             position: absolute;
             top: 50%;
             width: 14px;
             height: 22px;
             margin-top: -11px;
             z-index: 10;
             cursor: pointer;
             -moz-background-size: 27px 44px;
             -webkit-background-size: 27px 44px;
             background-size: 14px 22px;
             background-position: center;
             background-repeat: no-repeat
         }
         
         
         .swiper-pagination-bullet-active {
             opacity: 1;
             background: #ff8800;
         }
         
         .swiper-pagination {
         	text-align:right;
         }
         
         .swiper-container-horizontal>.swiper-pagination-bullets span:last-child{
         	margin-right:20px;
         }
         ```

   3.  css/swiper.min.css

      1. ```
         /**
          * Swiper 3.4.0
          * Most modern mobile touch slider and framework with hardware accelerated transitions
          * 
          * http://www.idangero.us/swiper/
          * 
          * Copyright 2016, Vladimir Kharlampidi
          * The iDangero.us
          * http://www.idangero.us/
          * 
          * Licensed under MIT
          * 
          * Released on: October 16, 2016
          */
         .swiper-container{margin-left:auto;margin-right:auto;position:relative;overflow:hidden;z-index:1}.swiper-container-no-flexbox .swiper-slide{float:left}.swiper-container-vertical>.swiper-wrapper{-webkit-box-orient:vertical;-mozt:0;top:0;width:100%;height:100%;-webkit-..很多代码......
         }}@keyframes swiper-preloader-spin{100%{transform:rotate(360deg)}}

   4. 其他js文件

      1. js/set_root.js, js/jquery-1.12.4.min.js, js/swiper.jquery.min.js

   5. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      	<link rel="stylesheet" type="text/css" href="css/reset.css">
      	<link rel="stylesheet" type="text/css" href="css/swiper.min.css">
      	<link rel="stylesheet" type="text/css" href="css/main.css">
      	<script type="text/javascript" src="js/set_root.js"></script>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/swiper.jquery.min.js"></script>
      	<script type="text/javascript">
      		$(function(){
      			var swiper = new Swiper('.swiper-container', {
      					  pagination: '.swiper-pagination',
      					  prevButton: '.swiper-button-prev',
      					  nextButton: '.swiper-button-next',
      					  // 初始的幻灯片是第几张
      					  initialSlide :0,
      
      					 // 小圆点是否可点击
      					  paginationClickable: false,
      
      					  //是否连续播放(设置false会在最后一张返回)
      					  loop: true,
      
      					  // 设置多长时间间隔播放一张
      					  autoplay:3000,
      
      					  // 用户操作后还是否自动播放
      					  autoplayDisableOnInteraction:true
      				})
      		});
      
      	</script>
      	<title>天天生鲜-首页</title>
      </head>
      <body>
      	<div class="main_wrap">
      		<div class="header clearfix">
      			<a href="#" class="logo"><img src="images/logo.png" alt="天天生鲜logo"></a>
      			<a href="#" class="search"></a>
      		</div>
      		<div class="center_con">
      			<div class="slide">
      
      				<div class="swiper-container">
      				  <div class="swiper-wrapper">
      				    <div class="swiper-slide"><a href="#"><img src="images/slide.jpg" alt="幻灯片"></a></div>
      				    <div class="swiper-slide"><a href="#"><img src="images/slide.jpg" alt="幻灯片"></a></div>
      				    <div class="swiper-slide"><a href="#"><img src="images/slide.jpg" alt="幻灯片"></a></div>
      				  </div>
      				    <div class="swiper-pagination"></div>
      				   <div class="swiper-button-prev"></div>
      				   <div class="swiper-button-next"></div>
      				</div>
      
      			</div>
      			.....
      		</div>
      	</div>
      </body>
      </html>			
      ```

      1. 先引用 css,  swiper.min.css 放在 main.css的上面,  下面的css 会覆盖上面的css

3. 修改样式:

   1. 页面, 右键点击修改元素, 选择"检查", 显示元素的 html ,右侧 style 显示样式.
   2. 复制对应样式 到 main.css,  修改样式.

4. 页面若引用 jquery或 zepto，则用 swiper.jquery.min.js,它的容量比swiper.min.js大

## 14-Bootstrap容器、栅格、按钮、表单

vue: 功能开发框架（功能代码如何组织），他搭建好了功能架子，基于他 可以快速开发功能

bootstrap: 界面效果框架（界面效果什么样子），他定义好了界面显示效果，比如按钮是什么样，输入框是什么样

#### 01 表单-字体图标

1. bootsrap的表单

   1.  表单

      1.  form 声明一个表单域, 它的class属性值
      
         1.  form-inline 内联表单域, 内部标准在一行内
         2.  form-horizontal 水平排列表单域
      
      2.  form内 div  的class属性
      
         1.  form-group 表单组、包括表单文字和表单控件
         2.  form-group-lg 大尺寸表单
         3.  form-group-sm 小尺寸表单
      
      3.  form内 div内的 input 的class属性
      
         1.  form-control 文本输入框、下拉列表控件样式
         2.  checkbox checkbox-inline 多选框样式
         3.  radio radio-inline 单选框样式
      
      4.  form内 div内的span的class属性
      
         1.  input-group-btn 表单控件组物件为按钮的样式
         2.  input-group-addon 表单控件组物件样式
         3.  input-group 表单控件组
      
         

2. 字体图标

   1. 字体代替图标，font文件夹和css文件夹在同一目录

3. 001 表单 字体图标.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      	<title>Document</title>
      	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/bootstrap.min.js"></script>
      	<style type="text/css">
      		
      		.glyphicon-heart{
      			font-size:16px;
      			color:gold;
      		}
      
      		.glyphicon-user{
      			font-size:20px;
      			color:pink;
      		}
      
      	</style>
      </head>
      <body>
      
      	<div class="container">   //响应式布局 container
      		<div class="row">
      			<form>  //表单域
      				<div class="form-group">  //表单组
      					<label for="input01"><span class="glyphicon glyphicon-user"></span></label>
      					<input type="text" name="" class="form-control" id="input01">
      				</div>
      			</form>
       
      
      			<form class="form-inline">   // 内联表单域, 内部标准在一行内
      				<div class="form-group form-group-lg">
      					<label for="input02">用户名:</label>
      					<input type="text" name="" class="form-control" id="input02">
      				</div>
      
      				<div class="form-group">
      					<label for="pwd02">密码:</label>
      					<input type="password" name="" class="form-control" id="pwd02">
      				</div>
      			</form>
       
      
      			<form class="form-horizontal">
      				<div class="form-group form-group-lg">   //表单组 大尺寸表单
      					<label for="input03" class="col-xs-2">用户名:</label>
      					<div class="col-xs-10">
      					<input type="text" name="" class="form-control" id="input03">
      					</div>
      				</div>
       
      
      				<div class="form-group">   // 表单组, 栅格col-xs-
      					<label for="pwd03"  class="col-xs-2">密码:</label>
      					<div class="col-xs-10">
      					<input type="password" name="" class="form-control" id="pwd03">
      					</div>
      				</div>
      			</form>
         
         
      			<form>
      				<div class="input-group">   //表单控件组
      				  <span class="input-group-addon">@</span> //表单控件组 物件样式
      				  <input type="text" class="form-control">				  
      				</div>
       
      
      				<div class="input-group">	//表单控件组			  
      				  <input type="text" class="form-control"> //文本输入框.下拉列表控件样式
      				  <span class="input-group-btn">  //表单控件组物件为按钮的样式
      
      				  	<button class="btn btn-primary">搜索</button>
      				  </span>				  
      				</div>
       
      				<div class="input-group">	//表单控件组			  
      				  <input type="text" class="form-control">
      				  <span class="input-group-btn">
      				  	<button class="btn btn-primary"><span class="glyphicon glyphicon-heart"></span></button>
      				  </span>				  
      				</div>
      
      			</form>
      		</div>
      	</div>
      </body>
      </html>
      ```

      

#### 02 bootstrap

https://blog.csdn.net/vanliujian/article/details/106226397?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163792961316780271520917%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=163792961316780271520917&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-106226397.first_rank_v2_pc_rank_v29&utm_term=bootstrap&spm=1018.2226.3001.4187

1. bootstrap 是框架

   1. bootstrap 

      1.  一个前端开发的框架，Bootstrap是基于html、css、Javascript的
          1. 由 bootstrap.min.js 和 bootstrap.min.css 构成
          2. 2, 3, 4版本
          3. 框架 大于 库
      2. 做响应式 布局
         1. meta:vp, //设置视口 手机/屏蔽用的

   2. 应用

      1. 下载源码

         1. ```
            bootstrap.min.js  放入js夹
            bootstrap.min.css 放入css夹
            font文件夹, 图标当字体用
            ```

      2. 文件夹结构

         1. ```
            css
            fonts
            js
            005bootstrap文档.html   // h5文档
            ```

         2. 实际开发, js,css 独立出去, 共用, 不写在下方

   3. 005bootstrap文档.html

      1. jquery-1.12.4.min.js,  bootstrap.min.js,   bootstrap.min.css
      
      2. ```
         <head>
         	<meta charset="UTF-8">
         	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
         	<title>bootstrap文档</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/bootstrap.min.js"></script>
         	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
         </head>
         <body>
         	<div></div>	
         </body>

2. bootstrap的 容器的样式: 

   1. container-fluid : 流式容器

   2. container 容器   响应式容器

      1. ```
         1170
         970
         750
         100%
         ```

   3. bootstrap响应式查询区间

      1. 大于等于768
      2. 大于等于992
      3. 大于等于1200

3. 006 bootstrap的容器.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      	<title>bootstrap文档</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/bootstrap.min.js"></script>
      	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
      	<style type="text/css">
      		
      		.container-fluid,.container{
      			height:50px;
      			border:1px solid #000;
      			background-color: gold;
      		}
      	</style>
      </head>
      <body>
      	<div class="container-fluid">流体容器</div>
      
      	<br>
      	<div class="container">响应式容器</div>	
      </body>
      </html>
      ```

      1. bootstrap框架的布局的class属性值: container-fluid , container

#### 03栅格系统

1. bootstrap的 栅格系统

   1. bootstrap 将页面横线方向12等分，按照12等分定义了适应不同宽度等分的样式类. 这些样式 组成了 流式栅格系统

   2. 样式:

      1. col-lg-n,  大于1200排成一行，小于1200分别占一行, n是份额大小, 1到12
      2. col-md-n, 大于992排成一行，小于992分别占一行
      3. col-sm-n, 大于768排成一行，小于768分别占一行
      4. col-xs-n,  始终排列成一行

2. 示例

   1. 007 bootstrap 栅格系统.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
         	<title>bootstrap文档</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/bootstrap.min.js"></script>
         	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
         	<style type="text/css">
         		
         		div[class*='col-']{
         			height:50px;
         			background-color:gold;
         			border:1px solid #000;
         		}
         	
         	</style>
         </head>
         <body>
         	<div class="container">
         		<div class="row">
         		<h2>栅格系统</h2>
         		</div>
         	</div>
         
         	<div class="container">		
         		<div class="row">
         			<div class="col-lg-3">col-lg-3</div>
         			<div class="col-lg-4">col-lg-4</div>
         			<div class="col-lg-2">col-lg-2</div>
         			<div class="col-lg-3">col-lg-3</div>
         		</div>
         	</div>	
         </body>
         </html>
         ```

      2. col-lg-3 ,  3 份额数, 

      3. `div[class*='col-'] {;;;}   //标签选择器, class*表示含有col-的标签元素`

3. 栅格里面的row.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
          <title>Document</title>
          <link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript" src="js/bootstrap.min.js"></script>
          <style type="text/css">
              
              .container{
                  height:50px;
                  background-color:gold;
              }
      
              /*  用负边距去掉容器左右15px的间距   */
              .row{
                  height:50px;
                  background-color:green;
              
              }
      
              .col-lg-5{
                  height:50px;
                  background-color:red;
              }
      
          </style>
      
      </head>
      <body>
          <div class="container">
              
              <div class="row">
                  <div class="col-lg-5">col-lg-5</div>
      
              </div>
      
          </div>
      </body>
      </html>


#### 04栅格响应式原理

1. 008 栅格响应式布局.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      	<title>bootstrap文档</title>
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/bootstrap.min.js"></script>
      	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
      	<style type="text/css">		
      		/* div[class*='col-']{
      			background-color:gold;
      			border:1px solid #000;
      		} */
      		.box{
      			height:200px;
      			max-width:240px;
      			background-color:cyan;
      			border:1px solid #000;
      			margin:20px auto;
      		}	
      	</style>
      </head>
      <body>
      	<div class="container">		
      		<div class="row">
      			<div class="col-lg-3 col-md-3 col-sm-6"><div class="box"></div></div>
      			<div class="col-lg-3 col-md-3 col-sm-6"><div class="box"></div></div>
      			<div class="col-lg-3 col-md-3 col-sm-6"><div class="box"></div></div>
      			<div class="col-lg-3 col-md-3 col-sm-6"><div class="box"></div></div>
      		</div>		
      	</div>
      
      	
      </body>
      </html>
      ```

   2.  class="col-lg-3 col-md-3 col-sm-6" 窗口尺寸适用那个选择那种尺寸

#### 05栅格偏移-栅格隐藏

1. 栅格偏移

   1. 偏移

      1. ```
         列偏移
         1、col-lg-offset-1,  对应的class属性标记的标记向右移动1份
         2、col-md-offset-2
         3、col-sm-offset-3
         4、col-xs-offset-4
         ```

   2.  009栅格偏移.html   offset-

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
             <meta charset="UTF-8">
             <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
             <title>bootstrap文档</title>
             <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
             <script type="text/javascript" src="js/bootstrap.min.js"></script>
             <link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
             <style type="text/css">     
                 div[class*='col-']{
                     background-color:gold;
                     border:1px solid #000;
                     height:50px;
                 }
             
             </style>
         </head>
         <body>
             <div class="container">  
                     <div class="row">   
                     <div class="col-lg-5">col-lg-5</div>
                     <div class="col-lg-1">col-lg-1</div>
                     <div class="col-lg-5">col-lg-5</div>
                     <div class="col-lg-1">col-lg-1</div>
                 </div>   
                 <div class="row">
                     <div class="col-lg-5 col-lg-offset-1 col-md-5 col-md-offset-1">col-lg-5  col-lg-offset-1</div>
                     <div class="col-lg-5 col-md-5">col-lg-5</div>       
                 </div>
                 <br>
                 <div class="row">
                     <div class="col-lg-4 col-lg-offset-1 col-md-4 col-md-offset-1">col-lg-4</div>
                     <div class="col-lg-4 col-lg-offset-2  col-md-4 col-md-offset-2">col-lg-4</div>
                 </div>  
             </div>
         </body>
         </html>
         ```

2. bootstrap 隐藏类

   1. 隐藏类

      1. ```
         1、hidden-xs
         2、hidden-sm
         3、hidden-md
         4、hidden-lg
         ```

      2.  010 栅格隐藏.html

         1. ```
            <!DOCTYPE html>
            <html lang="en">
            <head>
            	<meta charset="UTF-8">
            	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
            	<title>bootstrap文档</title>
            	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
            	<script type="text/javascript" src="js/bootstrap.min.js"></script>
            	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
            	<style type="text/css">		
            		div[class*='col-']{
            			background-color:gold;
            			border:1px solid #000;
            			height:50px;
            		}
            	
            	</style>
            </head>
            <body>
            	<div class="container">
            		<div class="row">
            		<h2>栅格隐藏</h2>
            		</div>
            	</div>
            
            	<div class="container">		
            		<div class="row">
            			<div class="col-lg-3 col-md-4 col-sm-6">1</div>
            			<div class="col-lg-3 col-md-4 col-sm-6">2</div>
            			<div class="col-lg-3 col-md-4 col-sm-6">3</div>
            			<div class="col-lg-3 col-md-4 hidden-md col-sm-6 hidden-xs">4</div>
            		</div>	
            	</div>
            	
            </body>
            </html>
            ```

            1. class="col-lg-3 col-md-4 hidden-md col-sm-6 hidden-xs"  , hidden-md 当 col-md响应式触发隐藏, hidden-xs 当 col-sm响应时触发隐藏

#### 06按钮的创建

1.  表格创建方法

   1.  从网站:`v3.bootcss.com/css/#tables`寻找资源
   2.  右键 > 检查 > 选中 `<table>...</table>`, 右键 copy > copy outerHTML, 然后再自己的html中粘贴. 

2. bootstrap 按钮

   1.  创建bootstrap按钮方法: 引入bootstrap框架, input标签, a 标签带class属性 都可以创建按钮

   2.  按钮

      1. 格式 class ="btn btn-default"  // btn 样式

      2. ```
         1、btn 声明按钮
         
         样式:
         2、btn-default 默认按钮样式
         3、btn-primay
         4、btn-success
         5、btn-info
         6、btn-warning
         7、btn-danger
         8、btn-link
         9、btn-lg
         10、btn-md
         11、btn-xs
         12、btn-block 宽度是父级宽100%的按钮
         13、active    // 默认是响应的效果
         14、disabled
         15、btn-group 定义按钮组
         ```

   3.  按钮组: 多个按钮紧挨着, 中间没有空间
      1. 多个`a` 外套 一个 `<div class="btn-group"></div>` 实现 按钮组,
      2. `<input type="button"> `, 每个input外面套着一个`<div class"btn-group><div>`可以实现按钮组

   4.  011 按钮.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
         	<title>bootstrap文档</title>
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/bootstrap.min.js"></script>
         	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
         </head>
         <body>
         	<div class="container">
         		<div class="row">
         			<input type="button" name="" value="按钮1" class="btn btn-primary">
         
         			<a href="#" class="btn btn-success">a标签按钮2</a>
         			<a href="#" class="btn btn-info">a标签按钮3</a>
         
         			<a href="#" class="btn btn-danger active">a标签按钮4</a>
         			<a href="#" class="btn btn-danger disabled">a标签按钮5</a>
         		</div>
          
         		<div class="row">
         			<a href="#" class="btn btn-success btn-lg">大按钮6</a>
         			<a href="#" class="btn btn-info btn-md">中等按钮7</a>
         			<a href="#" class="btn btn-warning btn-xs">小按钮8</a>
         			<a href="#" class="btn btn-danger">一般的按钮9</a>
         		</div>
         
         		<!--组按钮-->
         		<div class="row">
         			<div class="btn-group">
         				<a href="#" class="btn btn-primary">步骤一10</a>
         				<a href="#" class="btn btn-primary">步骤二11</a>
         				<a href="#" class="btn btn-default">步骤三12</a>
         			</div>
         		</div>
          
         		<div class="row">
         			<div class="btn-group btn-group-justified">
         				<a href="#" class="btn btn-primary">步骤一13</a>
         				<a href="#" class="btn btn-primary">步骤二14</a>
         				<a href="#" class="btn btn-default">步骤三15</a>
         			</div>
         		</div>
          
         		<div class="row">
         			<div class="btn-group btn-group-justified">
         				<div class="btn-group">
         					<input type="button" name="" value="步骤一16" class="btn btn-primary">
         				</div>
         				<div class="btn-group">
         				<input type="button" name="" value="步骤二17" class="btn btn-primary">
         				</div>
         				<div class="btn-group">
         				<input type="button" name="" value="步骤三18" class="btn btn-default">
         				</div>
         			</div>
         		</div>
         	</div>
         </body>
         </html>
         ```

#### 

## 15-Bootstrap菜单、对话框、布局实例

#### 01菜单01

#### 02菜单切换效果

1. bootstrap的 导航条

   1.  导航条样式: class属性值

      1. ```
         1、navbar 声明导航条
         2、navbar-default 声明默认的导航条样式
         3、navbar-inverse 声明反白的导航条样式
         4、navbar-static-top 去掉导航条的圆角
         5、navbar-fixed-top 固定到顶部的导航条
         6、navbar-fixed-bottom 固定到底部的导航条
         7、navbar-header 申明logo的容器
         8、navbar-brand 针对logo等固定内容的样式
         11、nav navbar-nav 定义导航条中的菜单
         12、navbar-form 定义导航条中的表单
         13、navbar-btn 定义导航条中的按钮
         14、navbar-text 定义导航条中的文本
         15、navbar-left 菜单靠左
         16、navbar-right 菜单靠右
         ```

   2.  创建方式:  div标签+ class="bootstrap导航条样式值"
   
      1. `<div class="navbar navbar-inverse navbar-static-top "> </div>`

   3.  002 导航条.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
         	<title>Document</title>
         	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/bootstrap.min.js"></script>
         
         </head>
         <body>
         	<div class="navbar navbar-inverse navbar-static-top">	// 导航栏	
         		<div class="container">  //container响应式布局
         			
         			<!-- 定义logo, 切换小图标右侧三个杠   -->
         			<div class="navbar-header">
         
         				<button class="navbar-toggle" data-toggle="collapse" data-target="#mymenu"> //data-target 关联菜单项 #mymenu
         					<span class="icon-bar"></span>
         					<span class="icon-bar"></span>
         					<span class="icon-bar"></span>
         				</button>
         
         				<a href="#" class="navbar-brand">LOGO</a>
         			</div>
         
         			<div class="collapse navbar-collapse" id="mymenu">
         
         				<!-- 定义菜单 , ul li方式定义 菜单项  -->  
         				<ul class="nav navbar-nav">
         					<li class="active"><a href="#">首页</a></li>
         					<li><a href="#">公司简介</a></li>
         					<li><a href="#">解决方案</a></li>
         				</ul>
         
         				<!-- 定义菜单里面的表单 form, 表单制作搜索框  -->
         				<form class="navbar-form navbar-right">
         					<div class="form-group">
         						<div class="input-group">
         							<input type="text" name="" class="form-control">
         							<span class="input-group-btn">
         								<button class="btn btn-default"><span class="glyphicon glyphicon-search"></span></button>
         							</span>
         						</div>
         					</div>
         				</form>
         				
         			</div>
         		</div>
         	</div>
         </body>
         </html>
         ```

         1. 先`<div class>`设定导航条, 再 `<div class="container "> `设定响应式布局
         2.  data-target="#mymenu" // 关联菜单
   
   4.  data-target和data-toggle介绍
   
       1.  bootstrap框架 中data-target和data-toggle，常用于 隐藏组件和展开组件, 做前端时用
   
       2.  具体详情请参https://www.bootcss.com/ Bootstrap中文网。
   
       3.  data-toggle介绍
   
           1.  data-toggle做事件绑定的，以什么事件触发，如modal,popover,tooltips,collapse等；
           2.  事件类型
               1.  modal： ，表示模态框也就是我们平时见得到的弹出框等；
               2.  popover： 弹出框, 如需激活弹出框，用户只需把鼠标悬停在元素上即可；
               3.  tooltips： 提示工具插件根据需求生成内容和标记；
               4.  collapse： 表示需要显示或隐藏的组件，点击事件后会在显示和隐藏之间切换，非常好用；
               5.  **tag:**标签页.
   
       4.  data-target介绍
   
           1.  data-target表示事件触发的目标，可以是class也可以是id(其他还没做测试)
   
       5.  示例
   
           1.  前提是需要先导入boostrap的样式文件。目标对象：先定义隐藏的，这个可以自定义. 按钮触发对象：指定位置图
   
           2.  ```
               <head>
                   <meta charset="UTF-8">
                   <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
                   <title>Document</title>
                   <link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
                   <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
                   <script type="text/javascript" src="js/bootstrap.min.js"></script>
               
               </head>
               <body>
                   <div class="top-search hidden" id="top-search"></div>
                   <button class="search" data-toggle="collapse" data-target="#top-search"></button>
               </body>
               ```
   
               

#### 03模态框

1.  模态框, 即弹出框

2.  语法格式

   1. modal 声明一个模态框
   2. 样式:
      1. modal-dialog 定义模态框尺寸
      2. modal-lg  大尺寸模态框
      3. modal-sm  小尺寸模态框
      4. modal-header
      5. modal-body
      6. modal-footer

3.  003 模态框.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
          <title>Document</title>
          <link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
          <script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
          <script type="text/javascript" src="js/bootstrap.min.js"></script>
      
      </head>
      
      <body>
          <div class="container"> 
              <div class="row">
                  <button class="btn btn-primary" data-toggle="modal" data-target="#modal01">弹出大模态框1</button>
                  <button class="btn btn-primary" data-toggle="modal" data-target="#modal02">弹出中等模态框2</button>
                  <button class="btn btn-primary" data-toggle="modal" data-target="#modal03">弹出小的模态框3</button>
                  
              </div>
          </div>
          
          <!--定义三个模态框, 按钮通过data-target属性值id关联模态框 -->
      
          <div class="modal fade" id="modal01">
              <div class="modal-dialog modal-lg">
                  <div class="modal-content">
      
                      <div class="modal-header">
                          提示弹框1
                      </div>
      
                      <div class="modal-body">
                          <p>文字内容1</p>
                      </div>
      
                      <div class="modal-footer">
                          <button class="btn btn-primary">确定1</button>
                          <button class="btn btn-default" data-dismiss = "modal">取消1</button>
                      </div>
      
                  </div>
              </div>
          </div>
      
          <div class="modal fade" id="modal02">
              <div class="modal-dialog">
                  <div class="modal-content">
      
                      <div class="modal-header">
                          提示弹框2
                      </div>
      
                      <div class="modal-body">
                          <p>文字内容2</p>
                      </div>
      
                      <div class="modal-footer">
                          <button class="btn btn-primary">确定2</button>
                          <button class="btn btn-default" data-dismiss = "modal">取消2</button>
                      </div>
      
                  </div>
              </div>
          </div>
      
          <div class="modal fade" id="modal03">
              <div class="modal-dialog modal-sm">
                  <div class="modal-content">
      
                      <div class="modal-header">
                          提示弹框3
                      </div>
      
                      <div class="modal-body">
                          <p>文字内容3</p>
                      </div>
      
                      <div class="modal-footer">
                          <button class="btn btn-primary">确定3</button>
                          <button class="btn btn-default" data-dismiss = "modal">取消3</button>
                      </div>
      
                  </div>
              </div>
          </div>
      
      </body>
      </html>
      ```

      
      
      1. ```
         data-toggle="modal" data-target="#modal01"  // 关联模态框
         ```

      2. 模态框有3层
      
         1.  ```
            modal
            modal-dialog
            modal-content
            	最后是模态框内的布局 html, 如 <div class="">...</div>
            ```
      
      3. 模态框 内容
      
         1. 三部分: model-header, model-body, modal-footer

#### 04js控制弹框弹出和隐藏

1. 弹框行为 用 js 控制

   1.   `$('#btn01').click()`
   2.   模态框函数 `$('#modal04').modal('show');`,  `$('#modal04').modal('hide');`

2. 003 模态框- 弹框的 js方式实现.html

   1. ```
      <!DOCTYPE html>
      <html lang="en">
      <head>
      	<meta charset="UTF-8">
      	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
      	<title>Document</title>
      	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
      	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
      	<script type="text/javascript" src="js/bootstrap.min.js"></script>
      	<script type="text/javascript">
      		// 用js 控制弹框
      		$(function(){
      
      			$('#btn01').click(function(){
      				// 点取消隐藏, 点弹框外不隐藏
      				//$('#modal04').modal('show');
      				$('#modal04').modal({
      					show:true,
      					backdrop:'static'  // c
      				});
      			})
      
      			$('#shutoff').click(function(){
      				$('#modal04').modal('hide');
      			})
      		})
      	</script>
      </head>
      
      <body>
      	<div class="container"> 
      		<div class="row">			
      			<button class="btn btn-primary" id="btn01">js控制的弹框</button>
      		</div>
      	</div>
      	
      	<!--快速创建: .modal>.modal-dialog>.modal-content>.modal-header+.modal-body+.modal-footer -->
      
      
      	<div class="modal fade" id="modal04">
      		<div class="modal-dialog">
      			<div class="modal-content">
      
      				<div class="modal-header">
      					提示弹框
      				</div>
      
      				<div class="modal-body">
      					<p>js控制的弹框</p>
      				</div>
      
      				<div class="modal-footer">
      					<button class="btn btn-primary">确定</button>
      					<button class="btn btn-default" id="shutoff">取消</button>
      				</div>
      
      			</div>
      		</div>
      	</div>
      
      
      </body>
      </html>
      ```

#### 05布局实例01

1. breadcrumb 面包屑, 实现路径导航

   1. ```
      <ol class="breadcrumb">
        <li><a href="#">首页</a></li>    //
        <li><a href="#">产品列表</a></li>
        <li class="active">水果</li>
      </ol>
      ```

      

   2. breadcrumb 示例: 004 路径导航-下拉菜单.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
         	<title>Document</title>
         	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
         	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
         	<script type="text/javascript" src="js/bootstrap.min.js"></script>
         
         </head>
         <body>
         	<div class="container">
         		<div class="row">
         			<ol class="breadcrumb">
         			  <li><a href="#">首页</a></li>
         			  <li><a href="#">产品列表</a></li>
         			  <li class="active">水果</li>
         			</ol>
         		</div>
         		
         		<br>
         
         		<br>
         		
         		<div class="row">            
         		    <div class="dropdown">
         
         		        <div class="btn btn-primary  dropdown-toggle" data-toggle="dropdown">
         		            下拉菜单
         		           <span class="caret"></span>
         		        </div>
         
         		        <ul class="dropdown-menu">
         		            <li><a href="#">菜单一</a></li>
         		            <li><a href="#">菜单二</a></li>
         		            <li><a href="#">菜单三</a></li>
         		        </ul>
         
         		    </div>
         		</div>
         
         	</div>
         </body>
         </html>
         ```

         1.  用 ol, li结构实现 路径导航, div 中嵌套 ol, li
         2. container 响应式布局,  

      2. bootstrap 下拉菜单

         1. 语法结构构成
            1. `<div data-toggle="dropdown">...</div>`
            2. `<ul class="dropdown-menu">...<ul>`
         2. ul, li 实际 菜单


#### 05布局实例02

1. bootstrap 图片

   1.  img-responsive  响应式图片

2. bootstrap 字体图标

   1.  font文件夹 和css文件夹在同一目录

3. class属性值 "jumbotron", 巨幕,  即:超大屏幕, 文字等放的很大, 如h1的jumbotron比正常的h1大

   1. ```
      <div class="jumbotron">
        <div class="container">
          ...
        </div>
      </div>
      ```

4. 水果节

   1. 文件结构

      1. ```
         水果节
         	css 文件夹
         		bootstrap.min.css
         		index.css
         	fonts 文件夹
         	images 文件夹
         	js 文件夹
         		bootstrap.min.js
         		jquery-1.12.4.min.js
         	index.html
         ```

   2. 程序

      1. index.html
   
         1. ```
            <!DOCTYPE html>
            <html lang="en">
            <head>
            	<meta charset="UTF-8">
            	<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
            	<title>天天生鲜-水果节</title>
            	<link rel="stylesheet" type="text/css" href="css/bootstrap.min.css">
            	<link rel="stylesheet" type="text/css" href="css/index.css">
            	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
            	<script type="text/javascript" src="js/bootstrap.min.js"></script>
            </head>
            <body>
            
            	<div class="navbar navbar-inverse navbar-static-top">		
            		<div class="container">
            			
            			<!-- 定义logo和切换小图标   -->
            			<div class="navbar-header">
            
            				<button class="navbar-toggle" data-toggle="collapse" data-target="#mymenu">
            					<span class="icon-bar"></span>
            					<span class="icon-bar"></span>
            					<span class="icon-bar"></span>
            				</button>
            
            				<a href="#" class="navbar-brand"><img src="images/logo.png" alt="天天生鲜logo"></a>
            			</div>
            
            			<div class="collapse navbar-collapse" id="mymenu">
            
            				<!-- 定义菜单   -->
            				<ul class="nav navbar-nav">
            					<li class="active"><a href="#">首页</a></li>
            					<li><a href="#">推荐商品</a></li>
            					<li><a href="#">手机生鲜</a></li>
            					<li><a href="#">抽奖</a></li>
            				</ul>
            
            				<!-- 定义菜单里面的表单   -->
            				<form class="navbar-form navbar-right">
            					<div class="form-group">
            						<div class="input-group">
            							<input type="text" name="" class="form-control">
            							<span class="input-group-btn">
            								<button class="btn btn-default"><span class="glyphicon glyphicon-search"></span></button>
            							</span>
            						</div>
            					</div>
            				</form>
            
            			</div>
            
            
            		</div>
            	</div>
            
            	<!--巨幕-->
            	<div class="jumbotron">
            	  <div class="container">
            		
            		<div class="row">
            			<div class="col-lg-5 col-lg-offset-1 col-md-5 col-md-offset-1">
            				<img src="images/banner_title.png" class="banner_title img-responsive">
            				<h2 class="banner_detail_title">水果节介绍</h2>
            				<p class="banner_detail">天天生鲜将在北京、天津、上海、南京、苏州、杭州、成都、武汉8座核心城市同期推出北京水果专场，借助天天生鲜产地端到用户端的渠道，果品流转效率得以大大提高。依托天天生鲜的渠道优势，首届果节做到了高质低价。</p>
            			</div>
            
            			<div class="col-lg-4 col-lg-offset-1 col-md-4 col-md-offset-1 hidden-sm hidden-xs">
            				<img src="images/basket.png" class="img-responsive">
            			</div>
            		</div>
            
            	  </div>
            	</div>
            
            	<div class="container">
            		<div class="row active-info">
            			<h3 class="text-center">活动图片</h3>
            			<p class="text-center">天天生鲜产地直采的果品甚至可以追溯到种植者和生产的地块儿。确定具体采摘地块儿后，在适合的时间将水果采摘下来后，直接在地头包装成箱，根据订单分装运到各个分仓，然后由配送员送到用户手中。以下是本次活动相关的图片</p>
            		</div>
            		
            		<div class="row active-pic-list">
            			<div class="col-lg-3 col-md-3 col-sm-6">
            				<div class="thumbnail">
            					<img src="images/active01.jpg">
            					<h4>现场采摘活动</h4>
            				</div>
            			</div>
            
            			<div class="col-lg-3 col-md-3 col-sm-6">
            				<div class="thumbnail">
            					<img src="images/active02.jpg">
            					<h4>现场采摘活动</h4>
            				</div>
            			</div>
            
            			<div class="col-lg-3 col-md-3 col-sm-6">
            				<div class="thumbnail">
            					<img src="images/active03.jpg">
            					<h4>现场采摘活动</h4>
            				</div>
            			</div>
            
            			<div class="col-lg-3 col-md-3 col-sm-6">
            				<div class="thumbnail">
            					<img src="images/active04.jpg">
            					<h4>现场采摘活动</h4>
            				</div>
            			</div>
            
            		</div>
            
            	</div>
            
            
            	<div class="container">
            		<div class="row goods_list">
            			<div class="col-lg-2">
            				<div class="goods_con">
            					<img src="images/goods.jpg" class="img-reponsive">
            					<h4>商品名称</h4>
            					<p>¥ 25.00/500g</p>
            				</div>
            			</div>
            
            			<div class="col-lg-2">
            				<div class="goods_con">
            					<img src="images/goods.jpg" class="img-reponsive">
            					<h4>商品名称</h4>
            					<p>¥ 25.00/500g</p>
            				</div>
            			</div>
            
            			<div class="col-lg-2">
            				<div class="goods_con">
            					<img src="images/goods.jpg" class="img-reponsive">
            					<h4>商品名称</h4>
            					<p>¥ 25.00/500g</p>
            				</div>
            			</div>
            
            			<div class="col-lg-2">
            				<div class="goods_con">
            					<img src="images/goods.jpg" class="img-reponsive">
            					<h4>商品名称</h4>
            					<p>¥ 25.00/500g</p>
            				</div>
            			</div>
            
            			<div class="col-lg-2">
            				<div class="goods_con">
            					<img src="images/goods.jpg" class="img-reponsive">
            					<h4>商品名称</h4>
            					<p>¥ 25.00/500g</p>
            				</div>
            			</div>
            
            		</div>
            	</div>
            	
            </body>
            </html>
            ```
   
            1.  菜单, 巨幕
               1. 菜单:  class="navbar"的div 包含 class="container"的div
               2. 巨幕: class="jumbotron"的div 包含 class="container"的div
            2. 图片活动, 商品名称
               1. 图片活动: class="container"的div 包含 div
               2. 商品名称: class="container" 的div包含 div

         2. index.css
   
            1. ```
               .navbar-brand{
               	padding:7px 15px 0 15px;
               }
               
               .navbar-inverse{
               	background-color:#ff722b;
               	border-color: #ff722b;
               }
               
               .navbar-inverse .navbar-nav>li>a {
                   color: #fff;
               }
               
               
               .navbar-inverse .navbar-nav>.active>a,.navbar-inverse .navbar-nav>.active>a:hover,.navbar-inverse .navbar-nav>.active>a:focus {
                   color: #fff;
                   background-color: #c75922;
               }
               
               
               .navbar-inverse .navbar-toggle {
                   border-color: #fff
               }
               
               
               .navbar-inverse .navbar-toggle:hover,.navbar-inverse .navbar-toggle:focus {
                   background-color:  #c75922;
               }
               
               
               .navbar-inverse .navbar-collapse, .navbar-inverse .navbar-form{
               	border-color:#fff;
               }
               
               
               .navbar{
               	margin-bottom:0px;
               }
               
               
               .jumbotron{
               	background:url(../images/banner_bg.jpg) center center no-repeat;
               	padding:25px 0;
               }
               
               
               .banner_title{
               	margin-top:47px;
               }
               
               
               @media (max-width:1200px){
               	.banner_title{
               		margin-top:30px;
               	}
               }
               
               
               @media (max-width:992px){
               	.banner_title{
               		margin-top:15px;
               	}
               }
               
               .banner_detail_title{
               	font-size:18px;
               	color:#ffff00;
               }
               
               .jumbotron .banner_detail{
               	font-size:14px;
               	color:#fff;
               	line-height:28px;
               }
               
               .active-info{
               	margin:0;
               }
               
               .active-info h3{
               	margin-top:0;
               	font-size:30px;
               	color:#333;
               }
               
               .active-info p{
               	font-size:14px;
               	color:#333;
               }
               
               .active-pic-list .thumbnail{
               	max-width:260px;
               	margin:0 auto 20px;
               }
               
               .active-pic-list .thumbnail h4{
               	text-align:center;
               	font-size:15px;
               	color:#333;
               }
               
               .goods_con{
               	border:1px solid #ddd;
               	margin:0 auto 20px;
               	max-width:260px;
               
               }
               
               .goods_con img{
               	width:100%;
               }
               
               
               .goods_list .col-lg-2{
               	width:20%;
               }
               
               
               @media (max-width:1200px){
               	.goods_list .col-lg-2{
               		width:20%;
               		float:left;
               	}
               }
               
               @media (max-width:992px){
               	.goods_list .col-lg-2{
               		width:33.33%;
               		float:left;
               	}
               }
               
               
               @media (max-width:768px){
               	.goods_list .col-lg-2{
               		width:100%;
               		float:left;
               	}
               }
               
               
               
               ```
   
               1. 响应式: @media(max-width:768px){...;...;}  
               2. 先link bootstrap.min.css,  再link index.css

## 16-正则表达式和前端性能优化

#### 01正则表达式-表单验证

1. 正则表达式

   1. 生成正则对象:

      1.  var re=new RegExp('字符串格式的规则', '可选参数');
          1.  参数:
              1.  'g'： global，全文搜索，默认搜索到第一个结果接停止
              2.  'i'： ingore case，忽略大小写，默认大小写敏感
      2.  `var re=/规则/参数;`     , 用` /    /` 标记正则规则
      3.  正则语句写在`<script>...</script>`中

   2. 规则字符

      1.  普通字符

         1. `/a/` 匹配字符 ‘a’，`/a,b/` 匹配字符 ‘a,b’
         
      2. 转义字符

         1. ```
            \d 匹配一个数字，即0-9
            \D 匹配一个非数字，即除了0-9
            \w 匹配一个单词字符（字母、数字、下划线）
            \W 匹配任何非单词字符。等价于[^A-Za-z0-9_]
            \s 匹配一个空白符
            \S 匹配一个非空白符
            \b 匹配单词边界
            \B 匹配非单词边界
            . 匹配一个任意字符
            ```
      
      3. 量词

         1.  对左边的匹配字符定义个数

         2. ```
            ? 出现 零次 或 一次（最多出现一次）
            + 出现 一次 或 多次（至少出现一次）
            * 出现 零次 或 多次（任意次）
            {n} 出现 n次
            {n,m} 出现 n到m 次
            {n,} 至少出现 n次
            ```
      
      4. 任意一个或者范围

         1. ```
            [abc123]  匹配‘abc123’中的任意一个字符
            [a-z0-9]  匹配a到z或者0到9中的任意一个字符
            ```
      
      5. 开头结尾

         1. ```
            ^ 以紧挨元素开头
            $ 以紧挨元素结尾
            ```
      
      7. 正则对象.函数

         1. test
            1. 正则.test(字符串);   测试正则与字符串是否匹配, 返回真/假
            
            2. ```
               var re02 = /a/;
               var sTr01 = 'abcdefg';
               re02.test(sTr01);
               ```
            
         2. 字符串.replace(正则，新的字符串)
            1.  字符串.replace(正则，新的字符串) 匹配成功的字符去替换新的字符
      
   3. 规则

      1. 匹配成功就结束，不会继续匹配
      2. 区分大小写

   4. 常用正则规则

      1.  用户名验证：(数字字母或下划线6到20位)

         1. ```
            var reUser = /^\w{6,20}$/;
            ```

      2. 邮箱验证

         1. ```
            var reMail = /^[a-z0-9][\w\.\-]*@[a-z0-9\-]+(\.[a-z]{2,5}){1,2}$/i;
            ```

      3. 密码验证

         1. ```
            var rePass = /^[\w!@#$%^&*]{6,20}$/;
            ```

      4. 手机号码验证

         1. ```
            var rePhone = /^1[3458]\d{9}$/;
            ```

2.  006正则表达式.html

   1. ```
      <head>
      	<meta charset="UTF-8">
      	<title>Document</title>
      	<script type="text/javascript">
      		
      		var re01 = new RegExp('a','i');
      		var re02 = /a/;
      		var re03 = /\d/;
      
      		var re04 = /\d+/;
      
      		var re05 = /^\d+$/;
      
      		var re06 = /^ab$/i;  //添加修饰符i, 不区分大小写
      
      
      		var sTr01 = 'abcdefg';
      		var sTr02 = 'cdefgh';
      		var sTr03 = '1234abcd';
      		var sTr04 = '12345678';
      		var sTr05 = 'Ab';
      
      
      		//alert( re02.test(sTr01));
      		//alert(re02.test(sTr02));
      		//alert(re03.test(sTr03)); // 弹出true
      
      		//alert(re04.test(sTr03));
      		//alert(re05.test(sTr03));
      
      		//alert(re05.test(sTr04));
      		alert(re06.test(sTr05));
      
      	</script>
      </head>
      ```

3.  注册表单验证

   1.  项目结构

      1. ```
         注册表单验证
         	js
         		register02.js
         		......
         	css
         	images
         	register.html
         ```

   2.  代码文档

      1. register.html
   
         1. ```
            <head>
            	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
            	<title>天天生鲜-注册</title>
            	<link rel="stylesheet" type="text/css" href="css/reset.css">
            	<link rel="stylesheet" type="text/css" href="css/main.css">
            	<script type="text/javascript" src="js/jquery-1.12.4.min.js"></script>
            	<script type="text/javascript" src="js/register02.js"></script>
            </head>
            <body>
            	<div class="register_con">
            		<div class="l_con fl">
            			<a class="reg_logo"><img src="images/logo02.png"></a>
            			<div class="reg_slogan">足不出户  ·  新鲜每一天</div>
            			<div class="reg_banner"></div>
            		</div>
            
            		<div class="r_con fr">
            			<div class="reg_title clearfix">
            				<h1>用户注册</h1>
            				<a href="#">登录</a>
            			</div>
            			<div class="reg_form clearfix">
            				<form>
            				<ul>
            					<li>
            						<label>用户名:</label>
            						<input type="text" name="user_name" id="user_name">
            						<span class="error_tip">提示信息</span>
            					</li>					
            					<li>
            						<label>密码:</label>
            						<input type="password" name="pwd" id="pwd">
            						<span class="error_tip">提示信息</span>
            					</li>
            					<li>
            						<label>确认密码:</label>
            						<input type="password" name="cpwd" id="cpwd">
            						<span class="error_tip">提示信息</span>
            					</li>
            					<li>
            						<label>邮箱:</label>
            						<input type="text" name="email" id="email">
            						<span class="error_tip">提示信息</span>
            					</li>
            					<li class="agreement">
            						<input type="checkbox" name="allow" id="allow" checked="checked">
            						<label>同意”天天生鲜用户使用协议“</label>
            						<span class="error_tip2">提示信息</span>
            					</li>
            					<li class="reg_sub">
            						<input type="submit" value="注 册" name="">
            					</li>
            				</ul>				
            				</form>
            			</div>
            
            		</div>
            
            	</div>
            
            	<div class="footer no-mp">
            		<div class="foot_link">
            			<a href="#">关于我们</a>
            			<span>|</span>
            			<a href="#">联系我们</a>
            			<span>|</span>
            			<a href="#">招聘人才</a>
            			<span>|</span>
            			<a href="#">友情链接</a>		
            		</div>
            		<p>CopyRight © 2016 北京天天生鲜信息技术有限公司 All Rights Reserved</p>
            		<p>电话：010-****888    京ICP备*******8号</p>
            	</div>
            	
            </body>
            ```
   
      2. register.js
   
         1. ```
            //定义封闭函数
            $(function(){
            
            	var $username = $('#user_name');  //获取
            
            	$username.blur(function(){
            		check_username();
            	});
            
            	$username.click(function(){
            		$(this).next().hide();
            	});
            
            	function check_username(){
            
            		var val = $username.val();
            		var re = /^\w{6,20}$/;
            
            		if(val=='')		{
            			$username.next().html('用户名不能为空');
            			$username.next().show();
            			return;
            		}
            
            		if(re.test(val))
            		{
            			$username.next().hide();
            		}
            		else
            		{
            			$username.next().html('用户名是6到20位的数字、字母或下画线');
            			$username.next().show();
            		}
            	}
            })
            ```
            
            1.  .blur()  // 取消焦点事件函数
            2. $(this).next()   //当前标签的下一个标签
            3.  .hide()  // 方法: 隐藏
            4.  .show()   //方法:显示

#### 02表单验证02

1.  register.js

   1. ```
      $(function(){
      
      	var error_name = false;
      	var error_password = false;
      	var error_check_password = false;
      	var error_email = false;
      	var error_check = false;
      
      
      	$('#user_name').blur(function() {
      		check_user_name();
      	});
      
      	$('#user_name').focus(function() {
      		$(this).next().hide();
      	});
      
      
      	$('#pwd').blur(function() {
      		check_pwd();
      	});
      
      	$('#pwd').focus(function() {
      		$(this).next().hide();
      	});
      
      	$('#cpwd').blur(function() {
      		check_cpwd();
      	});
      
      	$('#cpwd').focus(function() {
      		$(this).next().hide();
      	});
      
      	$('#email').blur(function() {
      		check_email();
      	});
      
      	$('#email').focus(function() {
      		$(this).next().hide();
      	});
      
      	$('#allow').click(function() {
      		if($(this).is(':checked'))
      		{
      			error_check = false;
      			$(this).siblings('span').hide();
      		}
      		else
      		{
      			error_check = true;
      			$(this).siblings('span').html('请勾选同意');
      			$(this).siblings('span').show();
      		}
      	});
      
      
      	function check_user_name(){
      		//数字字母或下划线
      		var reg = /^\w{6,15}$/;
      		var val = $('#user_name').val();
      
      		if(val==''){
      			$('#user_name').next().html('用户名不能为空！')
      			$('#user_name').next().show();
      			error_name = true;
      			return;
      		}
      
      		if(reg.test(val))
      		{
      			$('#user_name').next().hide();
      			error_name = false;
      		}
      		else
      		{
      			$('#user_name').next().html('用户名是5到15个英文或数字，还可包含“_”')
      			$('#user_name').next().show();
      			error_name = true;
      		}
      
      	}
      
      
      	function check_pwd(){
      		var reg = /^[\w@!#$%&^*]{6,15}$/;
      		var val = $('#pwd').val();
      
      		if(val==''){
      			$('#pwd').next().html('密码不能为空！')
      			$('#pwd').next().show();
      			error_password = true;
      			return;
      		}
      
      		if(reg.test(val))
      		{
      			$('#pwd').next().hide();
      			error_password = false;
      		}
      		else
      		{
      			$('#pwd').next().html('密码是6到15位字母、数字，还可包含@!#$%^&*字符')
      			$('#pwd').next().show();
      			error_password = true;
      		}		
      	}
      
      
      	function check_cpwd(){
      		var pass = $('#pwd').val();
      		var cpass = $('#cpwd').val();
      
      		if(pass!=cpass)
      		{
      			$('#cpwd').next().html('两次输入的密码不一致')
      			$('#cpwd').next().show();
      			error_check_password = true;
      		}
      		else
      		{
      			$('#cpwd').next().hide();
      			error_check_password = false;
      		}		
      	}
      
      	function check_email(){
      		var re = /^[a-z0-9][\w\.\-]*@[a-z0-9\-]+(\.[a-z]{2,5}){1,2}$/;
      		var val = $('#email').val();
      
      		if(val==''){
      			$('#email').next().html('邮箱不能为空！')
      			$('#email').next().show();
      			error_email = true;
      			return;
      		}
      
      		if(re.test(val))
      		{
      			$('#email').next().hide();
      			error_email = false;
      		}
      		else
      		{
      			$('#email').next().html('你输入的邮箱格式不正确')
      			$('#email').next().show();
      			error_email = true;
      		}
      	}
      
      
      	$('.reg_form').submit(function() {
      
      		check_user_name();
      		check_pwd();
      		check_cpwd();
      		check_email();
      
      		if(error_name == false && error_password == false && error_check_password == false && error_email == false && error_check == false)
      		{
      			return true;
      		}
      		else
      		{
      			return false;
      		}
      
      	});
      })
      ```

      1. `$(this).is(':checked');`  , 其中`:checked`伪类选择器
      2.  js 可以 调用在前, 定义 声明 在后

2.  `:checked`

    1.  判断一个单选（复选）框是否选中

        1.  ```
            <input id="checkbox1" type="checkbox" checked>
            <input id="checkbox2" type="checkbox>
            ```

    2.  正确做法

        1.  ```
            $("#checkbox1").is(":checked") // true
            $("#checkbox2").is(":checked") // false
            ```

    3.  错误做法:

        1.  ```
            $("#checkbox1").attr("checked") // checked
            $("#checkbox2").attr("checked") // undefined
            ```

        2.  无论是否选中，返回的值都是 checked 。

            1.  checked属性的值只有checked一个
            2.  写checked=false，checked="true",checked="unchecked" 毫无意义的。等同于checked="checked"

    4.  上述问题的机理:

        1.  html元素，有两种类型属性: HTML元素属性 和 DOM节点属性

        2.  DOM节点属性, 读取它的方法就是 attr() 方法

            1.  `<input id="checkbox1" type="checkbox" checked data-custom='customattribute'>`

        3.  HTML元素属性，这种属性你看不到，但是确实存在， 大部分情况和DOM节点属性对应的值一样. jQuery 取这种值的方法是 prop()

            1.  `<input id="checkbox1" type="checkbox" checked data-custom='customattribute'>`

            2.  ```
                $("#checkbox1").prop("id") //checkbox1
                $("#checkbox1").prop("type") //checkbox
                $("#checkbox1").prop("checked") //true
                $("#checkbox1").prop("data-custom") // undefined 自定义的DOM节点属性用取HTML元素属性的方法是取不到的
                ```

            3.  d和type和attr()方法返回的一样。但 checked()方法返回的值就不一样了，

                1.  DOM节点属性为静态的，页面渲染完，checked属性就确定了，是checked。
                2.  HTML元素属性是动态的，随时改变，对于checked这个属性，对应的值是true或者false。

            4.  ```
                <head>
                    <script type="text/javascript">
                        $(function(){alert($("#checkbox1").prop("id"));})
                    </script>
                </head>
                <body>
                    <input id="checkbox1" type="checkbox" checked data-custom='customattribute'>  
                </body>
                
                ```

                




#### 03前端性能优化

1. 几个方面

   1. 代码部署

      1. 代码的压缩与合并
      2. 图片、js、css等静态资源 用不同域名地址存储(和主站域名不同)，传输资源时不带cookie信息。
      3. 内容分发网络 CDN  (北京, 杭州分别 存储信息)
      4. 文件设置Last-Modified、Expires和Etag (运维要做的事情)
      5. GZIP压缩传送
      6. 尽量少的DNS查找次数(使用不同域名会增加DNS查找)
      7. 尽量少重定向(加"/")

   2. 编码

      1. html：

         1. 结构清晰，简单，语义化标签
         2. 避免空的src, href
         3. 尽量少在 HTML中 **缩放图片**

      2. css：

         1. 精简css选择器

         2. 把CSS放到顶部

         3. css中使用base64图片数据格式取代图片，减少请求数，在线转base64网站：http://tool.css-js.com/base64.html

            1.  base64 图片格式,  小图片直接转换为 base64数据格式, 存于html中

         4. css动画来取代javascript动画.  css动画快. js动画慢

         5. 使用字体图标

         6. 使用css sprite(雪碧图)

         7. 使用svg图形

         8. 避免@import方式引入css样式  (老的引入css 方法)

         9. 避免使用CSS表达式

            1. ```
               body{
                background-color: expression( (new Date()).getSeconds()%2 ? "#B8D4FF" : "#F08A00" );  
               }
               ```

         10. 避免使用css滤镜

      3. javascript

         1. 使用requirejs或seajs异步加载js

         2. JS 在页面底部引入

         3. 使用原生js方法

         4. switch代替if else语句

         5. 使用字面量表达式 初始化数组或对象

            1. ` val $iDiv = $("#user_name")`

         6. innerHTML取代 元素注入  . innerHTML 即 字符串

         7. 使用事件代理(事件委托)

         8.  使用Web Storage缓存数据

         9. 高频触发事件设置使用函数节流

         10. 减少引用库的个数

         11. 减少语句数，比如说多个变量声明可以写成一句

         12. 避免多次访问dom选择集

             1. ```
                for(i=1; i< $iDiv.length; i++){
                	alert(i);
                }
                
                替换为:
                val iLen = $iDiv.length; //使用变量存储多次使用的结果
                for(i=1; i < iLen; i++){
                	alert(i);
                }
                ```

         13. 避免全局查找

2.  base64图片数据

   1. base64.html

      1. ```
         <!DOCTYPE html>
         <html lang="en">
         <head>
         	<meta charset="UTF-8">
         	<title>Document</title>
         	<style type="text/css">
         		
         		.box{
         			width:200px;
         			height:200px;
         			background:url("data:image/gif;base64,R0lGODlhIAAgAPcAAP//////zP//mf//Zv//M///AP/M///MzP/Mmf/MZv/MM//MAP+Z//........AAOw==");
         		}
         
         	</style>
         </head>
         <body>
         	<div class="box"></div>
         </body>
         </html>
         ```

         

----

**图片 2种导入方式**

1. 背景图: 在css 中 用` background:url("ab/image.jpg"); `引入
2. 内容图: 在html文档中 用 `<img src="cd/image2.jpg"/ > `引入



结束.

-----

总结:

1. document是 js 原生的内置对象
   1. $(document)
2. windown是 js 原生内置对象
   1. $(windown)
   2. $(window).scroll()  
3. jquery , javascript 用 单引号,  ajax用双引号



------------------



