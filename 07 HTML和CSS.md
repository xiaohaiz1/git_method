#### 总结

1.  html : 
    1.  属性的值, 都是字符串类型,  值用双引号"" 包括
    2.  注释 <! -- 注释 -->
2.  CSS : (每个都有自己的属性特质)
    1.   标签的属性是 键值对, 键, 值都没有引号,  不同属性用 `; `分割
    2.  注释 /* 注释 */
    3.  层级选择器用空格分割 ` `
    4.  组选择器用逗号分割 `,`
    5.  伪选择器 前缀 : 

3. HTML DOM  : HTML 中，所有标签都是节点，构成一个 HTML DOM 树
   1. HTML 文档的所有内容都是节点：整个文档是 文档节点,  HTML 元素是元素节点, HTML 元素内的文本是文本节点,  HTML 属性是属性节点, 注释是注释节点

---

## Web前端三大主流框架

1. React,  vue.js  尤雨溪 轻量级,  Angular.js 谷歌开发

http://www.41818.net/xuexi1.htm

## 01-html文档结构和常用标签

#### 001岗位:

1. 产品经理: Axure RP是一款专业的快速原型设计工具。Axure（发音：Ack-sure），代表美国Axure公司；RP则是Rapid Prototyping（[快速原型](https://baike.baidu.com/item/%E5%BF%AB%E9%80%9F%E5%8E%9F%E5%9E%8B/7432267)）的缩写。

   Axure RP是美国Axure Software Solution公司旗舰产品，是一个专业的快速原型设计工具，让负责定义需求和规格、设计功能和界面的专家能够快速创建应用软件或Web网站的[线框图](https://baike.baidu.com/item/%E7%BA%BF%E6%A1%86%E5%9B%BE/7460023)、[流程图](https://baike.baidu.com/item/%E6%B5%81%E7%A8%8B%E5%9B%BE/206961)、原型和规格说明文档。作为专业的原型设计工具，它能快速、高效的创建原型，同时支持多人协作设计和版本控制管理 [1][ ]() 。

   Axure RP的使用者主要包括[商业分析师](https://baike.baidu.com/item/%E5%95%86%E4%B8%9A%E5%88%86%E6%9E%90%E5%B8%88/6896629)、[信息架构师](https://baike.baidu.com/item/%E4%BF%A1%E6%81%AF%E6%9E%B6%E6%9E%84%E5%B8%88/4774481)、[产品经理](https://baike.baidu.com/item/%E4%BA%A7%E5%93%81%E7%BB%8F%E7%90%86/11013391)、IT咨询师、[用户体验设计师](https://baike.baidu.com/item/%E7%94%A8%E6%88%B7%E4%BD%93%E9%AA%8C%E8%AE%BE%E8%AE%A1%E5%B8%88/985005)、[交互设计师](https://baike.baidu.com/item/%E4%BA%A4%E4%BA%92%E8%AE%BE%E8%AE%A1%E5%B8%88/3329267)、[UI设计](https://baike.baidu.com/item/UI%E8%AE%BE%E8%AE%A1)师等，另外，架构师、[程序员](https://baike.baidu.com/item/%E7%A8%8B%E5%BA%8F%E5%91%98/62748)也在使用Axure。

2. UI设计师:  Photoshop(少量设计师用firework)来设计的

3. 前端工程师: HTML, CSS , js(库和框架, 统一开发, 提高效率), ps, 等等

4. 测试工程师: 找bug 

5. 运维工程师:

6. CTO : 管所有技术人员的人

7. 有产品的公司有进步,有产品. 外包公司没有进步,没有产品.

#### 002html文档结构

1. 做布局

2. 右键> 查看源代码.

   1. 标签
      1. <, > , head, 成对出现
      2. 单个出现
   2. html, htm :  文件扩展名

3. Sublime Text 编写html的编辑器

   1. 桌面新建文件夹 abc

   2. project > Add Folder to Project

      1. 选文件夹abc, 目录
      
   3. 创建html文件: 

      1. 选择abc文件夹 > 右键 > New file > Ctrl + s
      2. 输入 abc.html. 
      3. Ctrl +  s 保存, 然后删除abc.html, 再编写

   4. ```
      <!DOCTYPE html>  //文档声明, 很重要
      <html>
      	<head>
          // 标题, 引用外部资源
          	<metacharset="UTF-8"> //网页编码
          	<title>个人页面</title>
          </head>
      	<body>
          你好! 欢迎访问
          </body>
      </html>
      ```

   5. 查看页面

      1. Preferences > Package Settings > View in Browser >

      2. Preferences > Key Bindings  //快捷键设置

         1. ```
            ctrl + alt + f
            ```
      
      
   
3. `ctrl+shift+p`  命令板
   
4. `f2`  view in  browser
   
   

#### 3.标题-段落-字符串实体-换行

1. ```
   <html lang="en">  //定义网页的语言为英文,不定义也没什么影响
   ```

2. 编写, 两个版本

   1. xhtml1.0 版本

      1. ```
         html:xt , 按 tab键   // 快速创建
         ```

      2. 文档声明, xmlns声明, meta编码声明

   2. html5 版本

      1. ```
         ! 按tab键 //快速创建 直接生成框架
         ```

      3. 标签用法就是html
      
   3. 注释  `<!--注释-->`

      1. 行注释：Ctrl + /

      2. 块注释：Ctrl + Shift + /

      3. **ps**

         1、无论是行注释还是块注释，都要先选中要注释的内容；

         2、注释与取消注释的快捷键一样。

3. 打开方式: 

   1. 编辑器: 不认识标签, 都是字符串
   2. 浏览器: 认识标签, 渲染
   3. 一个html 是一个网页

4. 标签

   1. ```
      <h1> ....<h6>   //6种标签,浏览器搜索引擎识别标签 同过标签理解文档结构
      h1按tab键
      ```
      
   2.     <h1>一级标题加粗样式<h1>
          <h2>二级标题<h2>   // 有间距, 带样式 空行



   2. ```
      <p> </p>  //段落标签, body中, 带样式, 带两个<p></p>之间带空行. 不建议在嵌套p标签, 也可以. 
      p 按tab键
      ```



3. 字符实体
   
      1. `&nbsp;`   //空格, 2个为半个空格, 不精确.       用样式控制空格大小
   2. `&gt;`  // > 大于号
   `&lt;`  // < 小于号
   
    3. `<br />`   //换行, 单标签规范写法: 标签+ 一个空格 + /
       `<br>`   //不规范写法, 都行的
   
   4. 标签输入. 快捷方法:  标签 按tab键
   
   1. 子元素 >  :  div>ul>li  按tab键
      
         1. ```
            <div>
                <ul>
                    <li></li>
                </ul>
            </div>
            ```
   
   
   
      2. 并列+ ： div+ul>li
      
         1. ```
            <div></div>
            <ul>
            	<li></li>
            </ul>
            ```
      
      3. 上级^ ： ul>li^div
      
         1. ```
            <ul>
                <li></li>
            </ul>
            <div></div>
            ```
      
      4. 上级多层^^  :  ul>li>a^^div
      
         1. ```
            <ul>
            	<li><a href=""></a></li>
            </ul>
            <div></div>
            ```
      
      5. 重复 *  :  ul>li*3
      
         1. ```
            <ul>
                <li></li>
                <li></li>
                <li></li>
            </ul>
            ```
      
      6. 分组 () ： div>(p>span)*2
      
         1. ```
            <div>
                <p><span></span></p>
                <p><span></span></p>
            </div>
            ```
   
   5. 属性
   
      1. id和类  :   div#header+div.main+div#footer ,  id #,  类 .
      
         1. ```
            <div id="header"></div>
            <div class="main"></div>
            <div id="footer"></div>
            ```
      
         2. ` # 是 id,  . 是 class`
      
      2. 属性值  :    a[title=test target=_self]
      
         1. ```
            <a title="test" target="_self" href=""></a>
            ```
      
      3. 数列值   p.item*3    
      
         1. ```
            <p class="item1"></p>
            <p class="item2"></p>
            <p class="item3"></p>
            ```
      
      4. p.item$$*3    ,  $ 0到9,  
      
         1. ```
            <p class="item01"></p>
            <p class="item02"></p>
            <p class="item03"></p>
            ```
      
      5. 数列操作符@ ,  p.item$@-*3 ,  (@- = -1)
      
         1. ```
            <p class="item3"></p>
            <p class="item2"></p>
            <p class="item1"></p>
            ```
      
      6. p.item$@3*3  ,  @3 从3开始3次
      
         1. ```
            <p class="item3"></p>
            <p class="item2"></p>
            <p class="item1"></p>
            ```
      
      7. p.item$@-3*3  ,  @-3 = 3次后到3结束
      
         1. ```
            <p class="item5"></p>
            <p class="item4"></p>
            <p class="item3"></p>
            ```
   
   6. 字符  {} , 快捷方式, 输入后字符上没有{}
   
      1.  a{click}
      
         1. ```
            <a href="">click</a> 
            ```
      
      2. a>{click}+span{me}
      
         1.  `<a href="">click<span>me</span></a>`




​         

   7. 缺省元素:  类属性id属性默认 div,   ul下的li,  table下的 tr, td
   
      1. ```
         .header+.footer   -----   div.header+div.footer
         ```
      
         1. ```
            <div class="header"></div>
            <div class="footer"></div>
            ```
      
      2. ```
         ul>.item*3   -----     ul>li.item*3 
         ```
      
         1. ```
            <ul>
            	<li class="item"></li>
            	<li class="item"></li>
            	<li class="item"></li>
            </ul>
            ```
      
      3. ```
         table>.row*4>.cell*3 -------------- table>tr.row*4>td.cell*3
         ```
      
         1. ```
            <table>
                <tr class="row">
                    <td class="cell"></td>
                    <td class="cell"></td>
                    <td class="cell"></td>
                </tr>
                <tr class="row">
                    <td class="cell"></td>
                    <td class="cell"></td>
                    <td class="cell"></td>
                </tr>
                <tr class="row">
                    <td class="cell"></td>
                    <td class="cell"></td>
                    <td class="cell"></td>
                </tr>
                <tr class="row">
                    <td class="cell"></td>
                    <td class="cell"></td>
                    <td class="cell"></td>
                </tr>
            </table>
            ```
   
   8. css缩写
   
      1. ```
         w100   
         width:100px;
         
         h10p
         height:10%;
         
         m5e
         margin:5em;
         ```

#### 004语义标签-图片

1. 块标签,  放body中

   1. ```
      <div>  </div>   //块元素 标签,一块内容,  没有语义. 不带任何样式: 空行
      可以再嵌套div
      ```

      1. ```
         <div>
         	<div>...</div>
         <div>
         ```

   2. ```
      <span>  </span>  //行元素, 行内一小段, 没有语义
      // 单独用没有意义
      ```

   3. 含**样式和语义**的标签

      1. ```
         <em>  </em>//斜体, 语义强调,行内元素
         <i>...</i>  //斜体, 专业词汇, 行内元素
         <b>..</b>  //加粗, 关键字或产品名, 行内元素
         <strong>..</strong>  //加粗, 重要内容.行内元素
         ```

   4. 语义标签

      1.  h1  标题
      2. p 段落
      3.  ul li 列表
      4. a 连接
      5. dl, dt, dd  定义列表

   5.  html图片

      1. html所在的目录中, 先建立 images图片文件夹

      2. ```
         img 按tab键
         
         <img src="images/002.jpg alt="水果图片" />   //单标签, src属性加图片, alt加载失败时显示文字, 盲人识别图片.
         // 先设置 Preferences>Package Settings>AutoFileName, 否则图片出不来
         ```
   
      3. 路径
      
         1.  相对路径: 当前目录,"./images/002.jpg" . 当前目录, ./  可省略, 相对**当前html**文件来说的
            1. 用相对路径
         2.  绝对路径: 相对磁盘路径, ` src="D://abc/003.jpg" `,  容易找不到


#### 005html复习1

1. CEO -> 产品经理 -> 后端工程师, UI工程师 <--> 前端工程师 -->测试工程师 --> 运维工程师
2. 页面布局: HTML CSS, 页面功能: JavaScript, UI: Photoshop  firework

#### 006链接

1.  a

   1. ```
      <a href="006.html">图片 </a>  // href要跳转的网页地址, 本地链接
      <a hef="www.baidu.com> 百度 </a>  //远程链接
      <a hef="www.baidu.com><img src="baidu.jpg" alt="baidu log" /> </a>  //图片做做链接
      <a hef="www.baidu.com" title="去百度网站" target="_blank"> 百度 </a>  //title 鼠标悬停提示, _blank新开一个网页, 默认 target="_self" 替换原来页面
      ```

   2. ```
      <a hef="#"> 图片 </a>  // #链接到顶部
      ```

#### 007列表

1. 有序列表

   1. ```
      <ol>
      	<li> html </li>
      	<li> css </li>
      </ol>
      1. html
      2. css
      ```

      1. ol>li*3   按tab键  //快捷写法
   
2. 无序列表

   1. ```
      // ul>li*3  按tab键  //快捷写法
      <ul>
      	<li><a herf="#"> 新闻1 </a></li>
      	<li>新闻2</li>
      	<li>新闻3</li>
      </ul>
      
      ul>(li>a{新闻1})*3  tab键 //快捷键
      ```

      1. 多用 ul li

3. 自定义列表

   1. ```
      dl>(dt+dd)*3   // 加号并列
      <dl>
      	<dt> html</dt>
      	<dd> 结构</dd>
      	<dt> css</dt>
      	<dd> 表现</dd>
      	<dt> javascript</dt>
      	<dd> 行为</dd>
      </dl>
      // dt 内容定格
      // dd 内容缩进
      ```

4. tab快捷键 

## 02-表格标签和传统布局

#### 01表格

1. ```
   <table border="1" width="300" height="200" align="center">  // table设置属性,border线宽,width宽,height高, align水平对齐 left center right
   	<tr>
   		<th>表头</th>  //表头, 带样式 居中 加粗
   	</tr>
   	<tr>
   		<td align="center">1</td>   //align内容居中水平对齐
   		<td>2</td>
   	</tr>
   	<tr>
   		<td valign="top">1</td>  //valign竖直对齐top|middle|bottom
   		<td>2</td>
   	</tr>
   <table>
   ```

   1. 页面布局. 4个标签组合

   2. 属性

      1. ```
         // table>(tr>td*5)*6  
         <table border="1" width="600" height="300" align="center">  // 600像素
         	<tr>
         		<td colspan="5"></td>  //水平合并, 多的删除
         	</tr>
         	<tr>
         		<td width="20%"> </td>
                 <td width="20%"> </td>
         		<td width="20%"> </td>
                 <td width="20%"> </td>
                 <td rowspan="5" width="20%"> <img src="images/person.jpg" alt="图片" /> </td>
         	</tr>
         	<tr>
         		<td> </td>
                 <td> </td>
         		<td> </td>
                 <td> </td>
                 // 多的删除<td> </td>
         	</tr>
         	.......
         </table>
         ```

         1. colspan, rowspan 是 td, th的属性, ="距离"

      

#### 02传统布局实例

1.  布局: 是排版. 2种方式:

   1.  table布局
   2. HTML+CSS  (DIV标签+CSS)布局

2. table布局  传统布局

   1. ```
      <body topmargin="0">   //topmargin 顶部部间距
      	<table border="1" cellpadding="20" cellspacing="20">  //cellpadding="内容与格子距离", cellspacing="格子与格子距离"
      	
      	</table>
      
      </body>
      ```

      1. tabel中border、cellpadding、cellspacing 设置为0，边框和间距不占空间，只起划分作用

      2. align, valign 对齐方式

      3. 表格中嵌套表格, 复杂表格

         1. ```
            <table>
            	<tr>
            		<td valign="top" bgcolor="#f2f2f2">  //bgcolor="背景色"
            			<table>
            				<tr> 
                            	<td>...</td>
                            </tr>
            			</table>
            		</td>
            		<td width="480">
            			<table >
            				<tr> <td>一行</td></tr>  //加一行
            				<tr>
            					<td><img src="abc.png" /></td>
            				</tr>
            			</table>
            			<br />  //换行
            		</td>
            		<tr> 
            			<td colspan="2"><b>个人信息</b><td>   //b加粗
            		</tr>
            	<tr>
            </table>
            ```

         2. 选择多行: ctr+点点多行, 可以同时输入

   2. ```
      <body topmargin="0">   <!--  topmargin 顶部部间距 -->
      	<table cellpadding="20" cellspacing="20">  <!--  cellpadding="内容与格子距离",cellspacing="格子与格子距离" -->
      
      </body>
      ```

## 03-表单标签

#### 01表单01

1.  表单  收集用户数据, 一个区域

2. ```
   <body>
   	<form>
   		<p>
   			<lable>用户名:<label>  //文字标注
   			<input type="text" name="" /> //输入框
   		</p>   //p有行高间距样式
   		<div>
   			<input type="password" name="" />  //密码框
   		</div> // div没有样式
   		
   		<div>
   			<input type="radio" name="gender" />男  //单选按钮, name指定同一个
   			<input type="radio" name="gender" />女
   		</div>
   		
   		<div>    //name指定同一复选框
   			<input type="checkbox" name="abc">学习
   			<input type="checkbox" name="abc">python
   		</div>
   		
   		<div>
   			<input type="file" name="">      //上传文件,图片,word等
   		</div>
   		
   		<div>
   			<textarea name="about"> </textarea>  //多行输入框
   		</div> 
   		
   		<br /> 换行
   		
   		<div>
   			<select>  //下拉列表
   				<option>北京1</option>
   				<option>北京2</option>
   				<option>北京3</option>
   			</select>
   		</div>
   		
   		<div>   
   			<input type="submit" name="" value="提交">  //提交按钮 value="显示文本"
   			<input type="reset" name="" value="重置">  //重置按钮
   		</div>
   </form>
   </body>
   ```
   
3.  form的标签

    1.  form, 属性 
        1.  action, method 
    2.  input
        1.  属性type的值: text, password, radio, checkbox, file, submit, reset
        2.  属性name,
        3.  属性value
    3.  textarea,  属性name的值
    4.  select > option

#### 02表单02

1. 属性

   1. ```
      <form action="www.baidu.com" method="post">  //action提交地址, 表单属性
      	<div>
      		<input type="text" name="username" />   //input表单控件,name属性值是键名
      	</div>
      	
      	<div>
      		<input type="radio" name="gender" value="0"/>男  //单选按钮, name指定同一个
      		<input type="radio" name="gender" value="1"/>女
      	</div>
      	
      	<div>
      		<textarea name="introduce"></textarea>
      	</div>
      	
      	<div>
      		<select name="site">  //下拉列表
      			<option value="北京1">北京1</option>
      			<option value="北京2">北京2</option>
      			<option value="北京3">北京3</option>
      		</select>
      	</div>
      		<input type="image" src="./abc.png">  //图片提交,提交x="5",y="10"
      		<input type="submit">  //submit提交按钮
      	<div>
      	
      	<br />
      	<input type="hidden" name="hid01" value="12">  //也会提交过去 hid01=12 提交出去
      	
      	
      </form>
      ```

      1. name: 属性名, 属性值与数据库**字段**名对应,  属性值是引号包括的字符串
      2. radio, checkbox, select用 属性 **value**提交值,  提交值是value的值
      3. input 标签,
         1. 属性 type, type="hidden", 隐藏的表单域, 存储值
         2. 属性 method="提交方式",  
         3. 属性 type
            1. type="submit"  //submit提交
            2. type="image"  //图片提交 
            3. 一般用样式做提交, 不用form做提交. 提交按钮做漂亮些
      4. 后台接收这些参数(键值对)

#### 03表单03

1. label 的for属性 绑定

   1. ```
      <form>
      	<div>
      		<label for="username">用户名:</label>    //点"用户名"激活输入框
      		<input type="text" name="username" id="username" />
      		
      		<input type="radio" name="gender" value="0" id="male"><label for="male">男</label>
      		<input type="radio" name="gender" value="1" id="female"><label for="female">女</label>		
      
      </form>
      ```

      1.  for  id组合,  标签label 关联(绑定)的text, radio等表单元素
          1.  for 指定关联的 id
          2.  id 标记被关联的表单元素
      
      

2. 总结

   1. 属性名1="属性值1"   属性名2="属性值2"    ,  属性值必须用引号,  属性用空格分割
   2. 页面上的元素没有先后顺序. 

## 04-CSS基本语法

#### 01样式介绍

1. Cascading Style Sheet 层叠样式表

   1. CSS  :  样式: 大小, 颜色
   2. html : 结构, 内容

2. 引入样式的方式

   1. 引入 CSS的 方法四种，它们分别是行内式、内嵌式、链接式和导入式

      1. 行内式

      2. 内嵌式

      3. 链接式 `<link href="mystyle.css" rel="stylesheet" type="text/css"/>`
      
      4. 导入式 : style标签中引入 
      
         1. ```
            <style type="text/css">
               	@import"mystyle.css"; 此处要注意.css文件的路径
            </style>
            ```

2. 方式1: **链接式**.  样式文件 main.css
   
   1. 创建 css/main.css    //文件夹/文件
   
         main.css
   
         ```
         div{
          font-size:26px;   //默认16
             color:red;
      }
      
      ```
   
         键值对, 键, 值都没有引号
   
      2. ```
         // css 与 html的关联
         <head>
         	<link rel="stylesheet" type="text/css" herf="./css/main.css">  //关联css
         </head>
         <body>
         	<div> div标签</div>
         </body>
         ```
      
   2. 属性 : rel="样式表", type="类型"  herf="目录+样式文件名"
   
   2. rel是 relationship , 意思是“关系”
   
   3. link标签, 在head中.
   
      style标签中 type属性
      
      
      
      
      
      


#### 02引入样式的方式

1. 方式2: 内嵌式, 

   1.  style标签, head中, html页面上

   2. ```
      <head>
      	<style type="text/css">
      		h1{
                  font-size:20px;
      		}
      	</style>
      	
      </head>
      <body>
      	<h1>页面标签</h1>
      	<div>div标签</div>
      </body>
      ```

      1. 插入html页面中

   3. 首页上, 速度快

2. 方式3: 行内式

   1.   style属性, 值是"" 字符串, 字符串内是 : 引导的键值对

   2. ```
      <body>
      	<a href="www.baidu.com" style="font-size:20px; color:pink"> 百度</a>
      </body>	
      ```

      1. 插入标签中
      2. 不需要选择, 其他两种需要选择器

## 05-CSS常用文本样式

#### 01常用文本样式01

1. ```
   <head>
   	<style type="text/css">
   		div{
               <!--字体属性设置-->
               font-size:20px;  //文字大小
               color:pink;    <!--字体颜色-->
               font-family:'微软雅黑';   // 字体,'Microsoft Yahei' 字符串单引号 
               font-style: italic;  //是否倾斜
               line-height:40px;  //行高, 
   		}
   		em{
               font-style:normal;  //常量值不用引号
               color:gold;
               font-weight:bold;   //bold加粗, normal不加粗
               
   		}
   	</style>
   </head>
   <body>
   	<div>
   	ABC
   	</div>
   	<em>
           ABCD
       </em>
   </body>
   ```

#### 02 CSS常用样式02

1. form 表单 的 input的属性:

   1.  action 提交目标
   2.  method 提交方式 get  post(大, 敏感)
   3.  name 键名, value 键值
   4.  type="text"  单行,  textarea  多行文本
   5.  CSS , 下面样式覆盖上面样式. Html变简洁

   

2. color : 文字颜色


   7. font  :  是否加粗 字号/行高 字体;   // font: normal 12px/36px '微软雅黑', 综合属性, 顺序

   8. text-decoration: underline;  下划线 一般用于设置没有

      ```
        <head>
        	<style type="text/css">
        		a{
                    text-decoration:none;
        		}
        	</style>
        </head>
        <body>
        	<a herf="www.baidu.com">百度</a>
        </body>
      ```
            属性, 
            1. text-align
            2. text-indent
            3. text-decoration

   9. text-indent :40px;  //首行缩进, 20px是一个字, 精确

   10. ```
       p{
           text-align:center;   //文字水平居中, left, right
       }
       <p> 这是一个p标签</p>
       ```

       1. p 默认是网页宽度
       2. a 默认是标签之间宽度

## 06-CSS样式选择器

#### 01 颜色

1. 3种: 

   1. 颜色名, red, gold

   2. rgb: rgb(255,0,0)   , 红绿蓝的光, 光越多越亮

   3. 16进制数值:  #ff0000

   4. ```
       <style type="text/css">
       	div{
              font-size:30px;
              color:rgb(0,0,255);
       	}
       </style>
       
       <body>
       	<div> div标签 </div>
       </body>
       ```

      




#### 标签选择器

1. ```
   <head>
       <style type="text/css">
           div{
               font-size:30px;
               color:rgb(0,0,255);
           }
        </style>
   </head>
    <body>
    	<div> div标签 </div>
    </body>
   ```

   1.  div 标签选择器
   2. ` *`  所有标签 选择器

#### id 选择器 

1. ```
   <head>
       <style type="text/css">    
           #div1{  // 定义
               color:blue;
           }
        </style>
   </head>
    <body>
    	<div id="div1"> div标签 </div>
    </body>
   ```

   1. id在标签中使用,  在head中 #定义, 
   2. 唯一的, 不允许重复. 用的少.

#### 类选择器

1. ```
   <head>
       <style type="text/css">    
           .big{  // 定义
               color:blue;
           }
           .green{  // 定义
               color:blue;
           }
        </style>
   </head>
    <body>
    	<div class="big green"> div标签 </div>   //两个class. 并联使用
    </body>
   ```

   1.  用到很多.  head中定义
   2.  id 权重高于 class

#### 属性选择器[]

1. ```
   <head>
       <meta charset="UTF-8">
       <title>Example</title>
       <style type="text/css">
       /*群组选择器*/
       span, [title="a"], div {   /*属性选择器
           color: blue;
       }
   
       [title=""]{color: green;}
       </style>
   </head>
   
   <body>
       <div>群组选择器</div>
       <p title="a">群组选择器</p>
       <p title="">群组选择器</p>
       <span>群组选择器</span>
   </body>
   ```

#### 02 兄弟选择器

1. 相邻兄弟选择器（+）、

   1. 紧接元素A后的另一个元素B，且二者具有相同的父亲元素

      1. ```
         <!DOCTYPE html>
         <html>
         <head>
             <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
             <title>相邻兄弟选择器</title>
             <style type="text/css">
                 h1+p{
                     color:red;
                 }
             </style>
         </head>
          
         <body>
             <p>Hello word!</p>
             <p>Hello word!</p>
             <h1>Hello word!</h1>
             <p>Hello word!</p>  <!-- 显示为红色 -->
             <p>Hello word!</p>
             <p>Hello word!</p>
             <p>Hello word!</p>
         </body>
         </html>
         ```

   2. 也会循环查找，即当两个兄弟元素相同时，会再一次循环查找

      1. ```
         <style type="text/css">
             li + li {
                 color:red;
             }
         </style>
         
         <div>
           <ul>
             <li>List item 1</li>  <!--原来颜色-->
             <li>List item 2</li>  <!--红色-->
             <li>List item 3</li>  <!--红色-->
           </ul>
         </div>
         ```

   

2. 兄弟选择器（~）

   1. 一个指定元素的 后 的所有兄弟结点

      1. ```
         <style type="text/css">
             h1 ~ p{    /*h1~p与h1 ~ p是一样的*/
                 color:red;
             }
         </style>
         </head>
         <body>
             <p>1</p>   <!-- 不变色 -->
             <h1>2</h1>
             <p>3</p>  <!-- 红色 -->
             <p>4</p>  <!-- 红色 -->
             <p>5</p>  <!-- 红色 -->
         </body>
         ```

#### 后代选择器（包含选择器）

1. 某元素后代的元素（子子孙孙元素）

2. 子选择器（>）

   1. 只能选择某元素儿子元素，不包括孙元素、曾孙元素

3. 后代选择器 (空格)

   1. 限定范围

      1. 如: .box span   //类选择器 标签选择器
      2. 后代选择器 指所有后代元素，空格选择。
      3. 从属关系, 最多4层

   2. ```
      <head>
          <style type="text/css">    
              .box{  /* 定义
                  font-size:20px;
                  line-height:30px;
                  text-indent:40px;  /* 缩进
              }
              
              .box span{  /*.box下的span, 层级定义
                  color:blue;
              }
              
              .box em{
                  font-style: normal;
                  text-decoration: underline;
              }
           </style>
      </head>
       <body>
       	<div class="box"> 主要应用在选择父元素下的<span>子元素</span>，或者子元素下面的子元素，可与标签元素结合使用，减少命名，同时也可以通过层级，防止命名冲突
       	</div>   // 
       </body>
      ```

      1. ```
         .box .span02{}   //层级选择器, 类box下的类span02
         ```

4. 示例: 后代选择器，子选择器和相邻兄弟选择器结合

   1. `html > body table + ul {margin-top:20px;}`
      1.  table 元素后出现的第一个相邻兄弟 ul 元素，该 table 元素包含在一个 body 元素中，body 元素本身是 html 元素的子元素

5. first-child

   1. 示例

      1. ```
         css:
         li:first-child
         {
         background:yellow;
         }
         
         HTML:
         <ul>
           <li>咖啡</li>   <!--黄色-->
           <li>茶</li>
           <li>可口可乐</li>
         </ul>
         
         <ol>
           <li>咖啡</li>  <!--黄色-->
           <li>茶</li>
           <li>可口可乐</li>
         </ol>
         ```

         

   2. 如果其父元素的第一个元素（p）不是该指定类型元素（li），则第一个元素不会有样式

      1. ```
         css:
         li:first-child
         {
         background:yellow;
         }
         
         html:
         <ol>
           <p>测试</p>
           <li>咖啡</li>   <!--不变-->
           <li>茶</li>
           <li>可口可乐</li>
         </ol>
         ```

1. :last-child、

   1. 父元素的最后一个子元素的每个元素

   2. p:last-child 等同于 p:nth-last-child(1)

   3. 其父元素的最后一个子元素的 p 元素的背景色

      1. ```
         css:
         p:last-child
         { 
         background:#ff0000;
         }
         
         html:
         <body>
         	<h1>这是标题</h1>
         	<p>第一个段落。</p>
         	<p>第二个段落。</p>
         	<p>第三个段落。</p>
         	<p>第四个段落。</p>
         	<p>第五个段落。</p>   <!--红色--> 
         </body>
         ```

2. :nth-child()、

   1. 父元素的第 N 个子元素，**与类型无关**

   2. 示例

      1. ```
         css:
         p:nth-child(2)
         {
         background:#ff0000;
         }
         
         
         html:
         <body>
         <h1>这是标题</h1>
         <p>第一个段落。</p>
         <p>第二个段落。</p>
         <p>第三个段落。</p>
         <p>第四个段落。</p>
         <p><b>注释：</b>Internet Explorer 不支持 :nth-child() 选择器。</p>
         </body>
         ```

   3. nth-child()的详细用法
      nth-child(3) 表示选择列表中的第三个元素。

      nth-child(2n)表示列表中的偶数标签，即选择第2、第4、第6……标签

      nth-child(2n-1) 表示选择列表中的奇数标签，即选择 第1、第3、第5、第7……标签

      nth-child(n+3) 表示选择列表中的标签从第3个开始到最后（>=3）

      nth-child(-n+3) 表示选择列表中的标签从0到3，即小于3的标签(<=3)

      nth-last-child(3) 表示选择列表中的倒数第3个标签

3. :nth-of-type(n)

   1. 父元素的**特定类型**的第 N 个子元素的每个元素. n 可以是数字、关键词或公式

   2. 示例

      1. ```
         css:
         p:nth-of-type(2)
         {
         background:#ff0000;
         }
         
         html:
         <body>
         	<h1>这是标题</h1>
         	<p>第一个段落。</p>
         	<div>测试</div>
         	<p>第二个段落。</p>    <!--红色--> 
         	<p>第三个段落。</p>
         	<p>第四个段落。</p>
         	<p>第五个段落。</p>
         </body>
         ```

4. :nth-last-child()的用法

   1. 元素的第 N 个子元素的每个元素，不论元素的类型，从最后一个子元素开始计数。

#### CSS3 :not 选择器

1. **:not(selector) 选择器匹配非指定元素/选择器的每个元素**
2. http://www.w3school.com.cn/cssref/css_selectors.asp

#### css: 属性选择器

1. 选择器	示例	示例说明
   [attribute]	[target]	选择所有带有target属性元素
   [attribute=value]	[target=-blank]	选择所有使用target="-blank"的元素
   [attribute~=value]	[title~=flower]	选择标题属性包含独立单词"flower"的所有元素
   [attribute ^= language]	[lang ^= en]	[lang ^= en] 选择一个lang属性的起始值="EN"的所有元素
   [attribute $= language]	[lang $= en]	选择一个lang属性的结尾值="EN"的所有元素
   [attribute *= language]	[lang *= en]	选择一个lang属性的包含"EN"的所有元素

2. CSS属性选择器中“~=”和“*=”的区别

   1. “value是完整单词” 类型的比较符号: ~= , |=

   2. “value是拼接字符串” 类型的比较符号: *= , ^= , $=

   3. 示例

      1. ```
         [attribute~=value] 属性中包含独立的单词为value
         
         [title~=flower]  
         <img src="/i/eg_tulip.jpg" title="tulip flower" />
         
         [attribute*=value] 属性中做字符串拆分，只要能拆出来value这个词就行
         
         [title~=flower]  
         <img src="/i/eg_tulip.jpg" title="ffffflowerrrrrr" />
         ```

#### 伪类选择器

1. | 选择器   | 示例        | 示例说明               |
   | -------- | ----------- | ---------------------- |
   | :link    | a:link      | 选择所有未访问链接     |
   | :visited | a:visited   | 选择所有访问过的链接   |
   | :hover   | a:hover     | 把鼠标放在链接上的状态 |
   | :active  | a:active    | 选择正在活动链接       |
   | :focus   | input:focus | 选择元素输入后具有焦点 |

#### 伪元素选择器

1. | 选择器                      | 作用                     | 说明                                                         |
   | --------------------------- | ------------------------ | ------------------------------------------------------------ |
   | ::before/:before            | 在被选元素前插入内容。   | 需要使用content属性来指定要插入的内容。被插入的内容实际上不在文档树中 |
   | ::after/:after              | 在选被元素后插入内容     | 其用法和特性与:before相似。                                  |
   | :first-letter/:first-letter | 匹配元素中文本的首字母。 | 被修饰的首字母不在文档树中。                                 |
   | :first-line/:first-line     | 匹配元素中第一行的文本。 | 这个伪元素只能用在块元素中，不能用在内联元素中。             |

2. 伪元素选择器:  响应鼠标的选择器

   1. 鼠标悬浮响应,链接上

      1. ```
         <head>
         	<style type="text/css">
         		.link{
                     font-size:30px;
                     text-decoration:none;
                     color:gree;
         		}
         		
         		.link:hover{
                     color:gold;
                     font-weight:bold;
         		}
         	</style>
         </head>
         
         <body>
         	<a herf="http://www.baidu.com" class="link">baidu</a>
         </body>
         ```

         1. 选择:伪选择器, 格式

         2. ```
            .link:hover{}
            ```

   2. 指定位置插入内容

      1. ```
         <head>
         	<style type="text/css">
         		.box01, box02{
                     font-size:20px;
         		}
         		
         		.box01:before{
                     content:"before";
                     color:red;
         		}
         		
         		.box02:after{
                     content:"after";
                     color:red;
         		}
         		
         		.box01 box02:over{
         			color: green;
          		}
          	</style>
          </head>
          
          <body>
          	<div class="box01">第一个div</div>  <!--before第一个div-->
           	<div class="box02">第二个div</div>  <!--第二个divafter-->
          </body>
         ```

3. 伪类和伪元素的区别

   1. 伪元素和伪类都给一些特殊需求加样式，定义上基本一致。
   2. 伪类像类选择器一样给已存在某个元素添加额外的样式；伪元素则是给自己虚拟的元素添加样式。

4. 

   

#### 组选择器

1. 元素共同的属性构成的选择器

2. ```
   <head>
       <style type="text/css"> 
       	.box01, .box02, box03{   //组选择器, 公共的提取出来
               font-size:20px;
               text-indent:40px;
       	}
   		.box01{
               color:red;
   		}
   		
   		.box02{
               color:pink;
   		}
   		
   		.box03{
               color:gold;
   		}
        </style>
   </head>
    <body>
    	<div class="box01">第一个div</div> 
    	<div class="box02">第2个div</div>
    	<div class="box03">第3个div</div>
    </body>
   ```

   1. 组选择器: 逗号`,`  分割.



 

## 07-盒子模型

#### 01 盒子模型

1. 布局方式 2种 : table方式,  div+css方式

2. div+css: 通过css控制div的布局

   2. 元素 是 标签, 是一个盒子

      1. 盒子由content  padding  border三部分组成
         1. padding是盒子内的
      2. 盒子外是   margin 

   3. ```
       <head>
        	<style type="text/css">
                .box{
                    width:200px;  /*宽*/
                    height:200px; /*高*/
                    background-color:gold;
                    
                    border-top-color:#000;  /*边框-顶-颜色*/
                    border-top-width:10px;  /*边框-顶-宽*/
                    border-top-style:solid; //solid实线,dashed 虚线, dotted 点线
                }
           </style>
        </head>
        <body>
        	<div class="box"> 元素一个块, 类似一个盒子</div>  <!--文本是盒子的内容-->
        </body>  
       ```
   

        1.  border-top: 10px solid  #000;
          
           border-bottom
          
           border-left
          
           border-right
          
        2. border: 10px solid #000;    /*宽 型 色*/
   
      

3. padding

   1. ```
      <head>
      	<style type="text/css">
              .box{
                  width:200px;
                  height:200px;
                  background-color:gold;
                  
                  padding-top:#000;
                  padding-left:10px;
              }        
         </style>
      </head>
      <body>
      	<div class="box"> 元素一个块, 类似一个盒子</div>
      </body> 
      ```

      1. padding:  20px, 80px, 100px, 40px;   //4个值: 顺时针,从上开始.
      2. padding: 20px, 80px, 100px;   // 3个值: 上, 左右, 下
      3. padding: 20px, 80px;  // 2个值: 上下, 左右
      4. padding:20px;   //1个值, 四个值一样的

4. margin

   1.  盒子的border 与 其他元素 距离

   2. ```
      <head>
      	<style type="text/css">
              .box{
                  width:200px;
                  height:200px;
                  background-color:gold;
                  
                  margin-top:#000;
                  margin-left:10px;
                  margin:100px 0 0;
              }        
         </style>
      </head>
      <body>
      	<div class="box"> 元素一个块, 类似一个盒子</div>
      </body> 
      ```

5. 盒子的总宽度 :  内容宽(width, height), padding宽, border宽, margin宽

#### 02 盒子模型练习 - 盒子真实尺寸

1. 盒子组合: 内容, padding, border
   
2. 

   ```
   <head>
   	<style type="text/css">
           .box{
               width:180px;  //内容宽
               height:180px;
               background-color:gold;
               
               border:10px solid #000;
               padding:20px;
           }        
      </style>
   </head>
   <body>
   	<div class="box"> 元素一个块, 类似一个盒子</div>
   </body> 
   ```

#### 03复习-标题练习

1.  css文件

   ```
   选择器{属性:值;  属性2:值2; 属性3:值3;}
   ```

2.  学习**盒子模型**为了学习 **Div+css** 布局方式

   1. 盒子模型 方便 布局

3. ```
   //body中 .box 按tab, 快捷键
   <div class="box"> </div>
   
   //body中 h1.mytitle按tab, 快捷键
   <h1 class="mytitle"> </h1>
   ```

4. 盒子大小

   1. 盒子宽: width + 左右padding + 左右border
   2. 盒子高: height + 上下padding + 上下border

5. 内容缩进 padding-left,  text-indent
   
   ```
   <head>
   	<style type="text/css">
   		.news_title{
               width:380px;
               height:35px;
               border-top:1px solid #f00;
               border-bottom:3px solid #666;
               font-size:20px;
               color:#333;
               font-family:'Microsoft Yahei';
               font-weight:normal;
               padding-top:15px;
               line-height:20px;  //行高
               padding-left:20px;  <!--内容缩进方式1 : padding方式
   		}
   	</style>
   </head>
   <body>
   	<h3 class="new_title">新闻列表</h3>
   </body>
   ```

   方法2
   
   ```
   <head>
   	<style type="text/css">
   		.news_title{
               width:380px;
               height:35px;
               border-top:1px solid #f00;
               border-bottom:3px solid #666;
               font-size:20px;
               color:#333;   /*字体颜色*/
               font-family:'Microsoft Yahei';
               font-weight:normal;
               padding-top:15px;
               line-height:50px;  //行高
               text-indent:20px;  <!--内容缩进方式2 : text-indent  
   		}
   	</style>
   </head>
   <body>
   	<h3 class="new_title">新闻列表</h3>
   </body>
   ```
   
   

## 08-margin和overflow属性

#### 01 margin使用技巧01

1. margin 属性:

   1. 盒子水平居中:  
      1. margin: 300px, auto;
      2. margin:300px, -1px;   //设置步骤. 
   2. **margin-top**嵌套塌陷
      1. 盒子嵌套, 内层盒子**margin-top**失效.  
      2. 解决之3方法
         1.  外部盒子设置一个边框
         2.  外部盒子设置 overflow:hidden 
         3.  使用伪元素类：
      3.  margin-top塌陷和 margin-left,  margin-right,  margin-bottom没有关系
      4. 不浮动有 margin-top 塌陷, 浮动没有margin-top塌陷
   3. margin 垂直合并,解决方法.
      1. 使用该方法. 合并,取两者最大者
      2. 一般设置margin-top. 此时没有margin合并问题
      3. 元素浮动或定位(破解margin-top塌陷)

2. ```
   <head>
   	<style type="text/css">
   		.box{
               width:200px;
               height:200px;
               background-color:gold;
               margin: 50px auto 100px auto; // 左右auto, 自动调节, 居中
               // margin: 50px auto 100px;  //效果同上
               //margin:30px 60px 100px 200px; // margin-left:200px;
   		}
   	</style>
   </head>
   <body>
   	<div class="box"> </div>
   </body>
   ```
   
   margin 的顺序: top, left, bottom, right
   
   margin:5px 10px; 上下是5px,左右是10px
   
   margin:5px auto 10px;   上是5px,左右是auto,下为10px

#### 02 margin使用技巧02

1. margin -10px;  负值  元素位移与边框合并

2. ```
   <head>
   	<style type="text/css">
   		body{
               margin:0px;
   		}
   		.box{
               width:200px;
               height:200px;
               background-color:gold;
               margin: 50px auto 100px auto; // 左右auto, 自动调节, 居中
               // margin: 50px auto 100px;  //效果同上
               //margin:30px 60px 100px 200px; // margin-left:200px;
   		}
   	</style>
   </head>
   <body>  
   	<div class="box"> </div>
   </body>
   ```
```
   
   1. body 自带 8px的margin
2.  margin-left: -50px; 跑到左边去了 
   
3. ```
   <head>
     <style type="text/css">
       body{
               margin:0px;
       }
       .box{
               width:202px;
               height:156px;
               background-color:green;
               margin: 50px auto0; 
       }
       
       .box div{
               width:200px;
               height:30px;
               border:1px solid gold;
               //下面样式覆盖上面样式
       }
   
     </style>
   </head>
   <body>  
   	<div class="box"> 
   		<div>A</div>
   		<div>B</div>
   	</div>
</body>
```

   实现方法2: margin负值

   ```
   		.box div{
               width:200px;
               height:30px;
               border:1px solid red;
               background-color:gold;
               border-top:-1px; //每一个与上面的重叠 1px
		}
   ```

   

4.  view > layout > Columns:2  打开两个

#### 03 margin合并及实例

1. 外边距合并

   1. 垂直方向合并取较大者

   2. ```
      <head>
      	<style type="text/css">
      		.box01, .div02{
                  width:200px;
                  height:30px;
                  background-color:gold;
      		}
      		
      		.box01{
                  margin-bottom:20px;
      		}
      		.box02{
                  margin-top:20px;
      		}
      	</style>
      </head>
      <body>  
      	<div class="box01">1</div>
      	<div class="box02">2</div>
      </body>
      
      ```
   ```
      
   3.  方案2种: 
   
      1. 设置一般设置 margin-top
      2. 元素设为浮动或定位
   ```

2. 垂直边距合并

   1. ```
      <head>
          <style type="text/css">
              .box{
                  width:200px;
                  border:1px solid #000;
                  margin:50px auto 0;
              }
              
              .box div{
                  border:1px solid #000;
                  margin-left:20px;
                  margin-right:20px;
                  margin-top:20px;
                  margin-bottom:20px;
                  //定价于 margin:20px;
              }
          </style>
      </head>
      <body>  
          <div class="box">
              <div>ABCD</div>
              <div>ABCD</div>
              <div>ABCD</div>
          </div>
      </body>
      ```
      
   2. 故意这样设计的

#### 04 margin-top塌陷

1. 只有margin-top 塌陷. 其他的不塌陷

2. 两个盒子嵌套, 内部盒子margin-top会加到外边盒子上, 内部盒子margin-top设置失败

   1. 解决方法

      1. 方法1: 外部盒子设置 overflow:hidden

         1. 元素溢出方法

         2. ```
            .con{
                overflow:hidden;   //把溢出的元素隐藏起来
            }
            ```

      3. 方法2: 外部盒子使用伪元素类：

         1. ```
            .clearfix:before{
               content:'';
               display:table;
            }
            ```
            
            1. 类似于加边框
            2. .clearfix是约定的类
            
         2. 最常用的方法: 伪元素类

   2. 嵌套出现塌陷

      1. ```
         <head>
         	<style type="text/css">
         		.con{
                     width:300px;
                     height:300px;
                     background-color:gold;
                     /*方法1*/
                     overflow:hidden;   //把溢出的元素隐藏起来
         		}
         		
         		/*方法2
         		.clearfix:before{
                 	content:'';
                 	display:table;
             	}
         		
         		.box{
                    width:200px;
                     height:100px;
                     background-color:green;
                     margin-top:100px;
         		}
         	</style>
         </head>
         <body>  
         	<div class="con clearfix">
         		<div class="box"></div>
         	</div>
         </body>
         ```

#### 05 元素溢出

1. 解决了溢出

   1.   overflow : hidden;   //把溢出的子元素隐藏
   2.  定位的元素会裁切掉. 用clearfix:before{} 解决

2. 子元素大于父元素出现元素溢出

   1. ```
      <head>
      	<style type="text/css">
      		.box{
                  width:300px;
                  height:3px;
                  border:1px solid #000;
                  margin:50px auto 0;
                  background-color:gold;
                  line-height:30px;
                  overflow:hidden;  //溢出的文本藏起来
      		}
      	</style>
      </head>
      <body>  
      	<div class="box">ABCD</div>
      </body>
      
      ```

3. **overflow：**的值

   1.  **visible** 默认值。内容不修剪，呈现在元素框之外。
   2.  hidden 内容会被修剪，多出的不可见的. 此属性还有清除浮动、清除margin-top塌陷的功能。
   3.  scroll 内容会被修剪. 滚动条显示。
   4.  **auto** 内容被修剪，显示滚动条, 否则不显示滚动条。
   5.  inherit  从父元素继承 overflow 属性的值。

## 09-display属性

#### 01块元素-内联元素

1. 盒子的类型

   1. 块元素, 或 行元素 占一行
   2. 内联元素
   3. 内联块源素

2. 块元素

   1. body, div、p、h1~h6、ul、li、dl、dt、dd

   2. 特点

      1. 支持全部的样式
      2. 继承父元素宽度, 默认的宽度为父级宽度100%
      3. 盒子占据一行、即使设置了宽度

   3. ```
      <head>
          <style type="text/css">
              body{
      /*            margin:0px; */
              }
              .box{
                  background-color:gold;
              }
          </style>
      </head>
      <body>    <!-- 自带margin:8px -->
          <div class="box">ABCD</div>   <!--  父元素是body -->
      </body>
      ```
```
   
3.  内联元素

   1. span、a、b、em、i、strong

   2. 特性

      1. 部分样式（不支持:  宽、高、margin上下、padding上下）
      2. 宽高由内容决定
      3. 在一行
      4. 代码换行，盒子之间会产生间距
      5. 子元素是内联元素，父元素用text-align属性设置子元素水平对齐方式

   3. ```
      <head>
      	<style type="text/css">
      		.box{
      			width:500px;
      			height:300px;
      			border:1px solid #000;
      			margin:50px auto 0;
      		}
      		.box a{
                  background-color:gold;
                  //margin:
                  //padding:0
      		}
      	</style>
      </head>
      <body>    //自带margin:8px
      	<div class="box">
      		<a href="#">连接1</a>
      		<a href="#">连接2</a>
      		<a href="#">连接3</a>
      	</div>   // 父元素是body
      </body>   
```

​      

#### 02内联块-实例

1. 内联块元素, 行内块元素

   1. 内联块元素，行内块元素，新增的元素类型

   2. img和input元素的行为类似这种元素，但是也归类于内联元素

      1.  没有内联块, 通过display转的

   3. display属性将块元素或者内联元素转化成这种元素。

      

2. 布局行为：

   1. 全部样式
   2. 没设置宽高，宽高由内容决定
   3. 盒子  在一行
   4. 代码换行，盒子产生间距
   5. 子元素是内联块元素，父元素用**text-align**属性设置子元素水平对齐方式。

3. ```
   <head>
       <style type="text/css">
           .box{
               width:500px;
               height:300px;
               border:1px solid #000;
               font-size: 0px;
           }
           .box a{
               background-color:gold;
               padding:0 10px;
               font-size:16px;
               display:inline-block;
           }
       </style>
   </head>
   <body>    <!-- 自带margin:8px -->
       <div class="box">
           <!-- 多个行内元素一行内显示, 元素间空隙解决fangan -->
           <a href="#">连接1</a>
           <a href="#">连接2</a>
           <a href="#">连接3</a>
       </div>   <!--  父元素是body -->
   </body> 
   ```
```
1. 间隙解决方法:
         步骤1: 父元素 font-size:0;
         步骤2: 子元素: font-size:16px;
```

4. 示例

   ```
   <head>
       <style type="text/css">
           .menu{
               width:694px;
               height:50px;
               background-color:cyan;
               margin:50px auto 0;
               font-size:0; /* 消除子盒子之间的空缺*/
           }
           
           .menu a{
   
               width:98px;
               height:48px;
               background-color:green;
               font-size:16px;
               font-family:'Microsoft Yahei' ;/*  单引号*/
               color: black;
               line-height:48px;            
               display:inline-block;  /* 块转内联块*/        
               text-decoration:none;  /* 去掉下划线*/
               text-align:center; /* 内容居中*/            
               border:1px solid gold; /*  48+2=50 */
               margin-left:-1px; /* 去掉margin重叠 */ 
   
   
           }
           
           .menu a:hover{   /*//hover伪类*/
               background-color:gold;
               color:#fff;
           }
       </style>
   </head>
   <body>
       <div class="menu">
           <a href="#">首页</a>
           <a href="#">公司简介</a>
           <a href="#">公司简介</a>
           <a href="#">公司简介</a>
           <a href="#">公司简介</a>
           <a href="#">公司简介</a>
        <a href="#">公司简介</a>
       </div>
   </body>
   ```
   
   

#### 03display属性

1. **display属性**

   1. 设置元素的类型及隐藏，常用的属性有：

      1. none . 隐藏元素并脱离文档流. 元素隐藏且不占位置
      2. block 元素以**块元素**显示,
         1. 不设置宽度时，宽度为父元素宽度
         2. 独占一行, 支持宽高
      3. inline 元素以**内联元素**显示 
         1. [1]内容撑开宽度. [2]非独占一行. [3]不支持宽高　[4]代码换行被解析成空格
      4. inline-block 元素以**内联块元素**显示
         1. [1]不设置宽度时，内容撑开宽度. [2]非独占一行. [3]支持宽高. [4]代码换行被解析成空格

   2. ```
      display:inline-block;
      ```

   3. ```
      <head>
          <style type="text/css">
              .body{
                  font-size:0; /*//消除子盒子之间的空缺*/
              }
              
              .box{
                  width:200px;
                  height:200px; 
                  background-color:gold;
                  display:inline-block;  /* 块转内联块, 在一行种显示*/
              }
                
              .con{
                  width:200px;
              }
              
              .con h3{
                  font-size:30px; 
              }
              
              .box2{
                  width:200px;
                  height:200px;
                  background-color:gold;
                  font-size:16px;   
                  display:none; /*// 与:hover伪类配合隐藏或显示*/
              }
              
              /* 放上鼠标显示,移开显示*/
              .con:hover .box2{
                  display:block;
              }
              
          </style>
      </head>
      <body>
          <div class="box">div1元素</div>   <!-- //放在同一行也去除空缺 -->
          <div class="box">div2元素</div>
          
          <br />
          <div class="con">
              <h3>文字标题</h3>
              <div class="box2">文字标题的说明</div>
          </div>
      </body>
      ```

## 10- float 布局

#### 01用列表制作菜单

1. 浮动块 + ul, li

   1. ```
      <head>
          <style type="text/css">
              .menu{
                  width:694px;
                  height:50px;
                  background-color:cyan;
                  /*去掉小圆点*/
                  list-style:none; /* 去掉小圆点*/
                  margin:50pa auto 0; /*//居中*/
                  padding:0;            
              }
              
              .menu li{
                  width:98px;
                  height:48px;
                  border:10px solid gold;
                  background-color:#fff;
                  float:left;/*//浮动, 去掉空缺, display:inline-block; 等效*/
                  margin-left:-1px;             
              }
              
              /*//文字区域转块区域*/
              .menu li a{
                  display:block; /*//文字区域转为块*/
                  width:98px;
                  height:48px;
                  text-align:center;
                  line-height:48px;
                  text-decoration:none;
                  font-size:16px;
                  font-family:'Microsoft Yahei';
                  color:pink;
              }
              
              .menu li a:hover{
                  background-color:gold;
                  color:#fff;
              }
          </style>
      </head>
      <body>
      <!-- ul.menu>(li>a{公司简介})*7 tab键 -->
          <ul class="menu">  <!-- ul有margin -->
              <li><a href="#">公司简介</a></li>  <!-- 去掉圆点, --> 
              <li><a href="#">公司简介</a></li>  <!--  a行内元素,文字区域的大小 -->
              <li><a href="#">公司简介</a></li>
              <li><a href="#">公司简介</a></li>
              <li><a href="#">公司简介</a></li>
              <li><a href="#">公司简介</a></li>
              <li><a href="#">公司简介</a></li>
          </ul>
      <body>
      ```

      1.   list-style:none;  //列表去掉小圆点
      2. float:left;//浮动, 去掉空缺, 与, display:inline-block; 等效

#### 02清除浮动

1. overflow 对float溢出的处理

   1. 清除浮动

      1. 子元素浮动, 撑开父元素的高度

   2. ```
      <head>
      <style type="text/css">
          .list{
              width:210px;
              //height:400px;  /*不给高度时出现浮动问题.*/
              border:1px solid #000;
              margin:50px auto 0;
              list-style:none;  /*去掉小圆点*/
              padding:0;
              /*清除浮动方法1 */
              overflow:hidden;  /*父元素加入, 父元素能被子元素撑开*/
          }
          
          .list li{
              width:50px;
              height:50px;
              background-color:gold;
              margin:10px;
              float:left;   /*浮动, 不能撑开父元素盒子*/
          }
          
          /*不行, 清除浮动方法2: 加入父级设置, 伪选择器通过样式向父标签塞东西*/
             .clearfix:before,.clearfix:after{
              content:"";
              display:table;
              clear:both;
          }
          
          
          /*清除浮动方法3: IE特有的,兼容IE*/
          .clearfix{
              zoom:1;
          }
          
      
      </style>
      </head>
      <body>
          <!--ul.list>list{$}*8 -->  <!-- //$ 数字1,2,3,4... -->
          <ul class="list clearfix">  <!-- //添加伪元素 -->
              <li>1</li>
              <li>2</li>
              <li>3</li>
              <li>4</li>
              <li>5</li>
              <li>6</li>
              <li>7</li>
              <li>8</li>
              <!--清除浮动 方法4-->
              <div style="clear:both"></div>
      </body>
      
      ```

2.   清除浮动方法:

   1. overflow:hidden;  , **父级**标签内 增加overflow, 清除浮动
   2. `<div style="clear:both"></div>`最后一个 **子元素** 后面增加一个空的div, 给它样式属性 clear:both  (不推荐)
   3. **父级** 类中添加 .clearfix:after{   //after是状态, 不用添加
              content:"";
              display:table; //元素作为块级显示（类似<table>）,表格前后有换行符。
              clear:both;
               	}
   
3. 父元素给高度, 子元素没有float属性, 没有浮动问题.

#### 03浮动01

1. float

   1.  ```
         <head>
           <style type="text/css">
               .con{
                  width:400px;
                  height:80px;
                  border:1px solid gold;
                  margin:50px auto 0;  //最后;可写可不xie
               }
               
               .con div{  /*两个选择器的权重大于一个选择器的权重*/
                  width:60px;
                  height:60px;
                  
                  /*
                  display:inlin-block;  /*元素转换,排列方式改变*/
                  float:left; /*元素div浮动*/
               }
               
               .box01{
                  background-color:green;
                  float:left;
               }
               
               .box02{
                  background-color:pink;
                  float:right;
               }
           </style>
         </head>
         <body>   
           <div class="con">
               <div class="box01"></div>
               <div class="box02"></div>
           </div>   
      </body>
      ```
      
   2. ```
      <head>
         <style type="text/css">
            .con2{
                  width:400px;
                  height:400px;
                  border:1px solid #000;
                  margin:10px auto 0;
            }
            
            .con2 div{
                  width:50px;
                  height:50px;
                  background-color:gold;
                  margin:5px; 
                  float:left; /*float破解合并, 元素的间距=左margin+右margin*/
            }
            
         </style>
      </head>
      <body>  
         <div class="con2">
            <div>1</div>
            <div>2</div>
            <div>3</div>
            <div>4</div>
         </div>
      </body>
      ```

#### 04浮动02

1. 有浮动元素 与 无浮动元素, 

   1. 浮动

      1. 左浮动(float:left)和右浮动(float:right)两种
      2. 浮动的元素，碰到父元素边界、其他元素才停
      3. 相邻浮动的块元素 并在一行，超出父级宽度就换行
      4. 浮动让**行内元素**或**块元素**自动转化为**行内块元素**(此时没有行内块元素间隙问题)
      5. 浮动元素后面没浮动的**元素** 占据浮动元素的位置，没浮动的元素内的**文字**会避开浮动的元素，饶图
      6. 父元素没有设置尺寸(一般高度不设置)，父元素内整体浮动的元素无法撑开父元素，父元素需要清除浮动
      7. 浮动元素之间没有垂直margin合并

   2. 文字绕图效果

      1. 占位: 浮动元素后的固定元素占据浮动的位置. 
      2. 绕图: 文字围绕浮动元素

   3. ```
      <head>
         <style type="text/css">
            .con{
                  width:450px;
                  height:210px;
                  border:1px solid #000;
                  margin:50px auto 0;
            }
            
            .pic{
                  width:80px;
                  height:80px;
                  background-color:gold;
                  float:left;
              }
                  
              .text{
                  background-color:green;
                  height:130px;
              }
            
         </style>
      </head>
      <body>  
         <div class="con">
            <div class="pic"></div>    <!-- 设浮动 -->
            <div class="text">2</div>  <!-- 无浮动 -->
         </div>
      </body>
      ```

   4. 父元素没有高度, 子盒浮动, 父元素撑不开高度. 父级盒子要清除浮动
   
2. 清除浮动方法

   1. 方法1: **额外标签法：** 给谁清除浮动，就在其后额外添加一个空白标签 

      1. **优点：** 通俗易懂，书写方便。（`不推荐使用`）

      2. **缺点：** 添加许多无意义的标签，结构化比较差。

      3. 元素small清除浮动（在small后添加一个空白标签clear(类名可以随意），设置clear:both;即可）

      4. ```
         <div class="fahter">
                 <div class="big">big</div>
                 <div class="small">small</div>
                 <div class="clear">额外标签法</div>
         </div>
         
         .clear{
                 clear:both;
             }
         
         ```

   2. 方法2: **父级添加overflow方法**

      1. 通过触发BFC的方式，实现清楚浮动效果。

         1. 必须定义width或zoom:1，同时不能定义height，
         2. 使用overflow:hidden时，浏览器会自动检查浮动区域的高度

      2. 简单、代码少、浏览器支持好

      3. 内容增多时候容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素。不能和position配合使用，因为超出的尺寸的会被隐藏。

      4. ```
         .fahter{
             width: 400px;
             border: 1px solid deeppink;
             overflow: hidden;
         }
         ```

      5. 别加错位置，是给父亲加（并不是所有的浮动都需要清除，谁影响布局，才清除谁。）

   3. **after伪元素清除浮动**

      1. `:after`方式为空元素的升级版，好处是不用单独加标签了。IE8以上和非IE浏览器才支持:after，，zoom(IE专有属性)可解决ie6,ie7浮动问题（`较常用推荐`）

      2. **优点：** 符合闭合浮动思想，结构语义化正确，不容易出现怪问题（目前：大型网站都有使用，如：腾迅，网易，新浪等等）

      3. **缺点：** 由于IE6-7不支持`：after`，使用`zoom：1`

      4. ```
         .clearfix:after{/*伪元素是行内元素 正常浏览器清除浮动方法*/
                 content: "";
                 display: block;
                 height: 0;
                 clear:both;
                 visibility: hidden;
             }
             .clearfix{
                 *zoom: 1;/*ie6清除浮动的方式 *号只有IE6-IE7执行，其他浏览器不执行*/
             }
          
         <body>
             <div class="father clearfix">
                 <div class="big">big</div>
                 <div class="small">small</div>
                 <!--<div class="clear">额外标签法</div>-->
             </div>
             <div class="footer"></div>
         </body>
         
         ```

   4. **before和after双伪元素清除浮动：**（`较常用推荐`）

      1. ```
         <style>
                     .father{
                         border: 1px solid black;
                         *zoom: 1;
                     }
                     .clearfix:after,.clearfix:before{
                            content: "";
                            display: block;
                            clear: both;
                        }
                        .big ,.small{
                         width: 200px;
                         height: 200px;
                         float: left;
                        }
                        .big{
                         background-color: red;
                        }
                        .small{
                         background-color: blue;
                        }
                 </style>
            <div class="father clearfix">
                 <div class="big">big</div>
                 <div class="small">small</div>
            </div>
             <div class="footer"></div>
         
         </div>
         ```

      2. **父级div定义 height：** 父级div手动定义height，就解决了父级div无法自动获取到高度的问题。

      3. **优点：** 简单、代码少、容易掌握

      4. **缺点：** 只适合高度固定的布局，要给出精确的高度，如果高度和父级div不一样时，会产生问题

#### 05复习-练习

1. float  布局

2. ```
   <head>
      <style type="text/css">
         .news_title{
               width:400px;
               height:40px;
               border_bottom:1px solid #ff8200;
               margin:50px auto 0;
         }
   
         .new_title h3{
               float:left;   /*.new_title h3对应的标签浮动*/
               width:80px;
               height:40px;
               background-color:#ff8200;
               margin:0px; /*h3默认有margin样式*/
               
               /*内部文字设置*/
               font-size:16px;
               color:#fff;
               text-align:center;   /*标签中间的文字*/
               line-height:40px;
               font-weight:normal; 
         }
         
         .new_title a{
               float:right;
               /*line-height:40px;*/
               font:normal 14px/40px 'Microsoft Yahei';
               color:#666;
                text-decoration:none;
         }
         
         .new_title a:hover{
               color:#ff8200;
         }
      </style>
   </head>
   <body>  
      <div class="new_title">
         <h3>新闻列表</h3>
         <a href="#">更多&gt;</a>
      </div>
   </body>
   ```

## 11-position布局

#### 01定位

1. 文档流

   1. 各盒子 按编写顺序排列

2. 定位

   1. position  属性 值:

      1. relative;   相对**自身**偏移, 元素原位置保留,
         1. left, right, top,bottom属性
      2. absolute;
         1. 相对于页面定位, 固定定位. 相对于已定位的**父级**元素定位. 直到body
      3. fixed;
         1. 相对于窗口定位. 脱离文档流, 不占文档流位置, 浮动在文档流上方, 
      4. static;
         1. 没有定位. 元素出现在文档流中, 相当于取消定位属性或不设置定位属性
      5. inherit
         1. 继承父元素 position

   2. 定位特性:

      1. 绝对定位, 固定定位的块元素 和 行内元素 自动转化为**行内块**元素
         1. 不设置宽高时, 宽高由文字决定.
         2. 设置宽高由设置决定

   3. 实例:

      1. relative

         1. ```
            <head>
               <style type="text/css">
                  .con{
                        width:400px;
                        height:400px;
                        border:1px solid #000;
                        margin:50px auto 0;
                  }
                  
                  .box01,.box02{
                        width:30px;
                        height:100px;
                        margin:10px;
                  }
                  
                  .box01{
                        background-color:green;
                        position:relative;  /* 相对定位*/ 
                        left:50px;
                        top:50px;
                  }
            
                  
                  
                  .box02{
                        background-color:gold;
                  }
               </style>
            </head>
            <body>
               <div class="con">
                  <div class="box01"></div>
                  <div class="box02"></div>
               </div>
            </body>
            ```

      2. absolute
      
         3. ```
              .box01{
                        background-color:green;
                        position:absolute;  /*决定定位 
                        left:50px;
                        top:50px;
              		}
              ```
            ```
            
            ```
      
      3.  fixed
      
         4. ```
              .box01{
                        background-color:green;
                        position:fixed;  /*相对窗口定位
                        left:50px;
                        top:50px;
                    }
              ```
         
      
      

3.  定位的层级

   1.  z-index:3;  值范围 0-9999, 大数值最上面

   2. ```
      <head>
      	<style type="text/css">
      		.con{
                  width:400px;
                  height:400px;
                  border:1px solid #000;
                  margin:50px auto 0;
      		}
      		
      		.con div{
                  width:200px;
                  height:200px;
                  position:absolute; //4个div的设置
      		}
      		
      		.box01{
                  background-color:green;
                  left:20px;
                  top:20px;
                  z-index:10; //设置层级, 越大越在上面
      		}
      		
      		.box02{
                  background-color:gold;
                  left:40px;
                  top:40px;
      		}
      		
      		.box03{
                  background-color:pink;
                  left:60px;
                  top:60px;
      		}
      		
      		.box04{
                  background-color:yellowgreen;
                  left:80px;
                  top:80px;
      		}
      	</style>
      </head>
      <body>
      	<div class="con">
      		<div class="box01"></div>
      		<div class="box02"></div>
      		<div class="box03"></div>
      		<div class="box04"></div>
      	</div>
      </body>
      
      ```

4.  圆角

   1. border-radius:10px;  //圆角

   8. ```
       <style type="text/css">
       		.con{
                  width:100px;
                  height:100px;
                  background-color:gold;
                  margin:50px auto 0;
                  position:relative;
                  border-radius:4px;
       		}
       		
       		.box{
                  width:28px;
                  height:28px;
                  background-color:red;
                  color:#fff;
                  text-align:center;
                  line-height:28px;
                  
                  position:absolute;
                  left:86px;
                  top:-14px;
                  
                  border-radius:14px;  /*圆角*/
       		}
       		
       		.box01{
                  background-color:green;
                  position:relative;  /*相对定位 */
                  left:50px;
                  top:50px;
       		}
       		
       		.box02{
                  background-color:gold;
       		}
       	</style> 
       	
      </head>
      <body>
      	<div class="con">
      		<div class="box"></div>
      	</div>
      </body>
      ```
      
      

#### 02定位实例

1.   固定悬浮顶部,水平居中菜单

   1. ```
      <head>
            <style type="text/css">
                  .menu{
                  
                  background-color:gold;
                  
                  /*固定悬浮页首 方法*/
                  position:absolute;  /*fixed, relative*/
                  width:600px;
                  top:0px;  /*margin没有定位功能, 不用margin*/
                  /*水平居中技巧, 先50%定位中间, 在margin拉回一半距离*/
                  left:50%;  /*百分量不加px*/
                  margin-left: -300px;
                  }
            </style> 
      </head>
      <body>
                  <div class="menu">菜单文字</div>
      </body>
      
      ```

2.  弹框样式

   1. ```
      <head>
            <style type="text/css">
                  .menu{
                  width:80px;
                  background-color:gold;
                  
                  /*固定悬浮页首 方法*/
                  position:fixed;
                  width:960px;
                  top:0px;  /*不用margin, margin没有定位功能*/
                  /*水平居中技巧, 先50%定位中间, 在margin拉回一半距离*/
                  left:50%;
                  margin-left:-480px;
                  }
                  
                  /*弹框*/
                  .popup{
                  width:500px;
                  height:300px;
                  border:1px solid #000;
                  background-color:#fff;
                  
                  /*相对窗口固定*/
                  position:fixed;
                  /*窗口左右居中*/
                  left:50%;
                  margin-left:-251px;
                  /*窗口上下居中*/
                  top:50%;
                  margin-top:-151px;
                  z-index:9999;
                  }
                  
                  .popup h2{
                  background-color:gold;
                  margin:10px;
                  text-align: center;
                  width:100%; /*相对父元素的百分数*/
                  height:80%;
      
                  }
                  
                  .mask{
                  position:fixed;
                  width:100%;
                  height:100%;
                  background-color:#000;
                  left:0;
                  top:0;
                  opacity:0.5; /*透明度*/
                  z-index:9998; /*层级*/
                  }
                  
                  .pop_con{
                  display:block;  /*弹框显示隐藏*/
                  }
            </style> 
      </head>
      <body>
                  <div class="menu">菜单文字</div>
                  
        <div class="pop_con">        
                  <div class="popup">
                      <h2>弹框标题</h2>
                  </div>
      
                  <div class="mask"></div>
             </div>
      </body>
      ```

## 12-background属性

#### 01 background属性-实例

1.  background

   1.  background-image:ulr();  设置背景图片, url的参数没有引号
   2.  background-color 设置背景颜色
            1. background-color:cyan;
   3. background-repeat 设置背景图片如何重复平铺
      1. repeat-x; //只平铺x轴, 
      2. repeat-y平铺y,
      3. no-repeat只平铺一次; 
      4. repeat, 缺失值, 平铺所有.
   4. background-position  背景图片的位置(方位值)
      1. 方位值:
         1. left, center, right
         2. top, center, bottom
            1.  1与2的组合
      2. 数值 ( 相对于盒子的位移)
         1.  background-position:-20px 10px;
      3. 数值和方位值配合使用 
   5. background-attachment 设置背景图片是固定还是随着页面滚动条滚动
   6. 组合使用
      1. background:url(images/bg.jpg)  -20px 10px  no-repeat  cyan;
      2. 无顺序要求.
      3. 数值, 图片移动像素值.

2.  实例

   1. backgound-image:url()

      1. ```
         <head>
             <style type="text/css">
                 .box{
                     width:400px;
                     height:200px;
                     border:5px solid #000;
                     margin:50px auto 0;
                     background-image:url(./image/bj.jpg);   /*以背景图加入图, 默认平铺满*/
                     background-repeat:repeat-x;  /*只平铺x轴, repeat-y平铺y,no-repeat只平铺一次; repeat, 缺失值, 平铺所有.*/
                     background-position:left bottom; /* 方位值*/
                 }
            </style>
         </head>
         <body>
             <div class="box">
         </body>
         ```
      
      
   
2. 背景图定位
   
      1. ```
         <head>
         	<style type="text/css">
         		.box{
                     width:100px;
                     height:100px;
                     border:5px solid #000;
                     margin:50px auto 0;
                     background:url(image/bg.jpg) -103px -151px no-repeat;  
         		}
            </style>
         </head>
         <body>
         	<div class="box"></div>
         </body>
         ```

#### 02复习-雪碧图背景列表练习

1.  雪碧图:  如 10张图组成一张图, 不用请求, 直接读取缓存即可获取

2.  ```
      <head>
       <style type="text/css">
          .list{
               list-style:none;
               width:300px;
               height:305px;
               margin:50px auto 0;
               padding:0;
               background:cyan;
          }
          
          .list li{
               height:60px;
               border-bottom:1px dotted #000; /*粗 线 色*/
               line-height:60px; /*行高*/
               text-indent:50px; /*首行缩进*/
               background:url(image/bj01.jpg) 0px 10px no-repeat;
          }
          
          .list .icon02{
               background-position:0px -71px;   /*原点的位置*/
          }
          
          .list .icon03{
               background-position:0px -154px;
          }
          
          .list .icon04{
               background-position:0px -235px;
          }
          
          .list .icon05{
               background-position:0px -315px;
          }
          
       </style>
      </head>
      <body>
       <!-- ul.list>li{美人鱼}*5  -->
       <ul class="list">
          <li>美人鱼</li>   <!-- 第一个是正确的, 不用在修改-->
          <li class="icon02">美人鱼</li>
          <li class="icon03">美人鱼</li>
          <li class="icon04">美人鱼</li>
          <li class="icon05">美人鱼</li>
       </ul>
      </body>
    ```

   1. 权重值:   权重(.list .icon02) > 权重(.list li) > 权重(.con02)
      1. 选择器个数越多权重越多
      2. 权重(类) > 权重(元素)
   
3. ```
      <head>
       <style type="text/css">
           .list{
      
              width:300px;
              height:500px;
              /*margin:50px auto 0;*/
              /*padding:0;*/
              /*background:cyan;*/
           }
           
           li{
               list-style:none;
               padding: 0;
               width: 300px;
               height:100px;
               border:1px dotted #000;  /*粗 线 色*/
               line-height:60px;  /*行高*/
               text-indent:150px;  /*首行缩进*/
      
               background-image:url(./image/bj01.jpg);
               background-repeat:no-repeat;
           }
           
           ul .icon01{
              background-position:30px 0px;
           }
      
           ul .icon02{
              background-position:30px -100px;
           }
           
           ul .icon03{
              background-position:30px -200px;
           }
           
           ul .icon04{
              background-position:30px -300px;
           }
      
           
       </style>
     </head>
     <body>
         <!-- ul.list>li{美人鱼}*5  -->
         <ul class="list">
            <li class="icon01">美人鱼1</li>   <!-- 第一个是正确的, 不用在修改-->
            <li class="icon02">美人鱼2</li>
            <li class="icon03">美人鱼3</li>
            <li class="icon04">美人鱼4</li>
         </ul>
     </body>   
   ```

## 13-布局练习和图片格式

#### 01布局练习

1. 网页平铺图片背景

   1. 图片 背景

      1. 背景不占网页空间
      2. 网页背景不动效果:  background-attachment:fixed;

   2. ```
      <head>
      	<style type="text/css">
      		body{
                  background:url(image/bj.jpg) -103px -151px no-repeat; 
                  background-attachment:fixed; //网页背景图不动
      		}
      		
      		p{
                  text-align: center;
                  color:red;
      		}
         </style>
      </head>
      <body>
      	<p>网页文字</p>
      	<br />
      	<p>网页文字</p>
      	<br />
      	<p>网页文字</p>
      	<br />
      </body>
      ```

2. 例子

   3. 内联块: 父元素的text-align属性设置子元素水平对齐

   4. 翻页效果

      1. ```
         <head>
              <style type="text/css">
                   .pagenation{
                        list-style:none;
                        margin:50px auto 0;
                        padding:0;
                        width:600px;
                        height:40px;
                        border:1px solid #666;
                        
                        font-size:0;
                        text-align:center;
                   }
                   
                   .pagenation li{
                        display:inline-block;
                        height:26
                     //background-color:gold;  /*背景色给到li上*/
                     font-size:12px;
                     margin:7px 5px 0;   /*左右距离=margin left + margin right =5+5=10*/
                   }
                   
                   .pagenation li a{
                     display:block;
                     background-color:gold;   /*背景色给到a上*/
                     height:30px;
                     line-height:15px;
                     padding:0 15px;   /*内部填充, 左右10,数字在页面中尺寸一样大*/
                     font:normal 7px/30px 'Microsoft Yahi';
                     color:#000;   /*字体颜色*/
                     text-decoration:none;   /*下划线none*/
                   }
                   
                   .pagenation li a:hover{
                     background-color:red;
                     color:#fff;
                   }
            </style>
         </head>
         <body>
              <!--ul.pagenation>(li>a{$})*11 -->
              <ul class="pagenation">
                   <li><a href="#">上一页</a></li>
                   <li><a href="#">1</a></li>
                   <li><a href="#">2</a></li>
                   <li><a href="#">3</a></li>
                   <li><a href="#">4</a></li>
                   <li><span>...</span></li>
                   <li><a href="#">17</a></li>
                   <li><a href="#">18</a></li>
                   <li><a href="#">19</a></li>
                   <li><a href="#">20</a></li>
                   <li><a href="#">下一页</a></li>
              </ul>
              <p>网页文字</p>
              <br />
              <p>网页文字</p>
              <br />
              <p>网页文字</p>
              <br />
         </body>
         ```

3. 导航条02

   1. **行内块元素**之间会有**空隙**

      1. 父元素 font-size:0; // 解决方法

   2. ```
      <head>
           <style type="text/css">
                .menu{
                  list-style:none;
                     margin:50px auto 0;
                     padding:px;
                     width:700px;
                     height:80px;
                     border:1px solid gold;
                    
                }
                
                .menu li{
                     display:inline-block;
                     height:80px;   /*li元素高*/
                     border:1px solid green;
                }
                
                .menu li a{
                  display:block;  /*转成块, a宽度==li的宽度*/
                  height:30px;   /*a高*/
                  font:normal 14px/20px 'Microsoft Yahei';  /*//字体*/
                  text-decoration:none;  /*//下划线*/
                  color:#000;   /*字体颜色*/ 
                  border:1px solid blue;         
                }
                
                .menu li a:hover{
                  color:#ff8800;
                  border:1px solid cyan;            
                }
                
                .menu li span{
                  display:block;
                  height:40px;
                  font:normal 14px/40px 'Microsoft Yahei';
                  margin:0 20px;  /*上下0, 左右20px*/
                  border:1px solid red;
                }
           </style>
      </head>
      <body>
           <!--ul.menu>(li>a{网站建设})*7-->
           <ul class="menu">
                <li><a href="">网站建设</a></li>
                <li><span>|</span></li>
                <li><a href="">网站建设</a></li>
                <li><span>|</span></li>
                <li><a href="">网站建设</a></li>
                <li><span>|</span></li>
                <li><a href="">网站建设</a></li>
                <li><span>|</span></li>
                <li><a href="">网站建设</a></li>
                <li><span>|</span></li>
                <li><a href="">网站建设</a></li>
           </ul>
      </body>
      ```

6.  菜单02

   1. ```
      <head>
           <style type="text/css">
                .menu02{
                  list-style:none;
                     margin:50px auto 0;
                     padding:0;
                     width:960px;
                     height:100px; /*不给高度,子盒撑起父盒高度*/
                     position:relative;
                     overflow:hidden; /*没有高度时,出现溢出的解决方法*/
                     border:1px solid gold;
      
                }
                
                .menu02 li{
                     width:100px;
                     height:5px;
                     float:left;
                     border:1px solid green;
                }
                
                .menu02 li a{
                  display:block;  /*转成块,与父等宽. a宽度==li的宽度*/
                  width:100px;
                  height:50px;   /*a高*/
                  line-height:40px;
                  text-align:center;
                  font-size:14px;
                  font-family:'Microsoft Yahei';
                  color:#000;
                  text-decoration:none;   /*下划线*/
                  border:1px solid blue;
                }
                
                .menu02 li a:hover{   /*伪类*/
                  background-color:#00619f;  
                  border:1px solid red;          
                }
                
                .menu02 .new{
                  width:3px;
                  height:10px;
                  background:url(image/bj01.jpg) no-repeat;
                  position:absolute;  /*相对于已定位的父元素*/
                  left:432px;
                  top:0;
                  border:1px solid yellow;
                }
           </style>
      </head>
      <body>
           <!--ul.menu02>(li>a{公司简介})*7-->
           <ul class="menu02">
                <li><a href="">公司简介</a></li>
                <li><a href="">公司简介</a></li>
                <li><a href="">公司简介</a></li>
                <li><a href="">公司简介</a></li>
                <li><a href="">公司简介</a></li>
                <li><a href="">网站建设</a></li>
                <li class="new"><img src="image/bj01.jpg" alt="new标签图片">aaaa</li>
           </ul>
      </body>
      ```

#### 02图片格式

2. 图片格式
   1. 位图, 矢量图
      1. 位图  方形像素点组成
      2. 矢量图  放大不是真.
         1.  函数计算生成的.
   2. 位图
      1. psd   ps专业格式, 网页不能用, 辅助做网页的
         1. 有图层, 透明,未压缩的数据, 比较大	
      2. **jpg**   网页用, 压缩高, 小, 有损压缩 失真. 不支持透明背景, 不能做成动画.
         1. 不用透明背景时, 多用jpg
      3. **gif**  无损压缩，容量小、制作**动画**、支持透明背景。
         1. 缺点：只有256色，不支持半透明，透明图像边缘有锯齿。
      4. **png**   网页比较普遍格式。
         1. 优点：无损压缩，容量小、支持透明背景和半透明色彩、透明的边缘光滑。
         2. 缺点：不能制作动画
      5. webp    将取代jpg。
         1. 优点：同jpg格式，容量比jpg小。
         2. 缺点：同jpg格式，不支持所有的浏览器。
   3. 矢量图
      1. svg  目前首选
         1. 优点：容量小、不失真、支持透明背景和半透明色彩、图像边缘光滑。
         2. 缺点：色彩不够丰富。
      2. flash  退出历史的
         1. 优点：容量小、不失真、支持透明背景和半透明色彩、图像边缘光滑、制作动画、可编写交互。
         2. 缺点：不支持搜索引擎、运行慢、浏览器需要装插件才可支持。
   4. 总结:
      1. 网页制作:
         1. 大图,
            1. 不透明背景, jpg
            2. 透明,半透明, 用 png
         2. 小图
            1. 多种颜色, gif png
            2. 单色, svg
            3. 动画, gif

## end

