Chrome开发者工具面板：
面板上包含了Elements面板、Console面板、Sources面板、Network面板、
performance面板、Memory面板、Application面板、Security面板、Audits面板这些功能面板。
**Chrome V35 版本中的开发者工具分为 8 个大模块，每个模块及其主要功能为：**
**Element标签页** ： 用于查看和编辑当前页面中的 HTML 和 CSS 元素。
**Console 标签页**：用于显示脚本中所输出的调试信息，或运行测试脚本等。
**Source 标签页**：用于查看和调试当前页面所加载的脚本的源文件。
**Network 标签页**：用于查看 HTTP 请求的详细信息，如请求头、响应头及返回内容等。
**performance面板**:可以记录和分析页面在运行时的所有活动
**performance面板**有如下四个窗格：
　　1、controls。开始记录，停止记录和配置记录期间捕获的信息
　　2、overview。页面性能的高级汇总
　　3、火焰图。 CPU 堆叠追踪的可视化
　　4、统计汇总。以图表的形式汇总数据
**Memory面板**录制一段当前页面录制期间可以在页面进行你想监测的一些页面操作，时间自己掌握
**Application面板**:记录网站加载的所有资源信息，包括存储数据（Local Storage、Session Storage、IndexedDB、Web SQL、Cookies）、缓存数据、字体、图片、脚本、样式表等。
**Security面板**:判断当前网页是否安全。
**Audits面板**:对当前网页进行网络利用情况、网页性能方面的诊断，并给出一些优化建议。比如列出所有没有用到的CSS文件等。

注： 这一篇主要讲解前三个面板**Elements**、**Console**、**Sources**。

###### Elements面板实时编辑DOM节点和CSS样式

1. 双击DOM树视图里面的节点，可以实时编辑标签属性，修改的效果会立刻反应在浏览器里面
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515100855713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
2. 点击右侧Style面板，可以实时修改CSS的属性值，这里面的所有样式Name和Value都是可以编辑的;在每个属性后面单击可以添加新的样式，如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515100911338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)

1. 点击右侧Computed面板，可以编辑左侧选中的盒子模型参数，所有的值都是可以修改的;点击不同的位置(top、bottom、left、right)
   就可以修改元素的padding、border、margin属性值。
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515100929443.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
2. 查看网页的本地修改历史 点击Styles面板中修改过属性的文件名，会跳转到Source面板 在文件位置右击选择Local
   modifications,可以查看本地的所有修改记录 点击指定的时间点可以看到粉红背景的删除内容和绿色背景的添加内容

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515100946744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)

###### Console面板控制台输出日志

通过JS代码或者命令行**console.log()**、**console.warn()**
和**console.error()** 可以将日志信息输出到控制台

**console.log** 显示一般的基本日志信息，当要显示的基本日志太多时可以使用**console.group**将相关的日志进行分组
**console.warn** 显示带有黄色小图标的警告信息
**console.error** 显示带有红色小图标的红色的错误信息
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101005258.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
**console.assert** 当第一个参数为false时，才会显示第一个参数的值
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101021745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
**可以根据JS条件判断输出不同的日志信息**
注： 当需要换到下一行而不是回车的时候，请按Shift+Enter。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101046324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
**控制台交互**

JS表达式计算
在上一小节，我们已经看到可以在控制台输入JS表达式点击**Enter**即可得到表达式的值，当你在控制台输入命令时，会弹出相应的智能提示框，你可以用**Tab**自动完成当前的建议项

选择元素
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101105429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
快捷方式 描述
$() 返回与指定的CSS选择器相匹配的第一个元素，等同于document.querySelector()
$$() 返回与指定的CSS选择器相匹配的所有元素的数组，等同于document.querySelectorAll()
$x() 返回与指定的XPath相匹配的所有元素的数组

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101130874.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
**注**： 我在实际操作过程中发现**$()**并没有按预期返回相匹配的第一个元素，而是返回了所有匹配的元素数组，我也给Google提供了这个issue，等待Google的答复。

###### Sources面板

你可以在这个面板里面调试你的JS代码，也可以在工作区打开你的本地文件。
**调试JS代码**
你可以点击JS代码块前面的数字外来设置断点，如果当前代码是经过压缩的话，可以点击下方的花括号 **{ }** 来增强可读性，所有的断点都会列出右侧的断点区。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101147224.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
**设置断点**
断点可以在DOM元素节点发生改变时、XHR生命周期状态改变时、指定的事件执行时被触发

**①** DOM元素节点发生改变时
在Elements面板中指定的DOM节点上右击，在弹出的菜单中选择**Break on…**，可以看到三个选择项，比如我们选择**Subtree modifications，**那么当选择的节点里面的子节点被添加、删除、修改，则断点就会被触发。设置方式如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101202856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
下图是在我的系统里添加指定省市指定医院时由于增加了元素节点而触发的断点，通过单步调试可以看到会弹出一个 **div**对话框供用户添加数据。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101219548.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
② XHR生命周期状态改变时

当XHR生命周期状态发生改变或者XHR的URL与**Sources**面板右侧的**XHR Breakpoints**栏设置的字符串匹配时，则断点就会有触发。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101237642.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)
③ 指定的事件执行时

在**Sources**面板右侧的**XHR Breakpoints**栏下面是**Event Listener Breakpoints**,列出了各种类型的事件，勾选你要监听的事件，
在指定的事件执行时，断点就会有触发。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515101318120.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTI5Mjcw,size_16,color_FFFFFF,t_70)

-----------------------------

# Chrome 开发工具、Elements标签与Network标签及豆瓣电影网页分析

Chrome 开发工具、Elements标签与Network标签及豆瓣电影网页分析

**1.以分析豆瓣电影为例https://movie.douban.com/**
打开Chrome 开发者工具快捷键F12，或空白处单击右键选择“检查”，下图所示。
左边为网页原页面，右边为开发者工具界面。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409121250786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqeXhsZA==,size_16,color_FFFFFF,t_70)
**2.熟悉Elements标签与Network标签**
（1）Elements标签
在上图的右图中，找到Elements标签，右图点击出现两个部分，中间图显示整个网页的HTML信息，单击选中某行，右图部分styles标签会显示当前点击选中内容的CSS样式，并可对元素的CSS进行查看与编辑修改。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409121743563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqeXhsZA==,size_16,color_FFFFFF,t_70)
用鼠标放到中间图的某行，此行会“高亮”，同时左图相应的部分也“高亮”，表明自动显示并选中该元素在HTML里的位置。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409122411542.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqeXhsZA==,size_16,color_FFFFFF,t_70)
（2）Network标签
Network标签可以记录页面上的网络请求的详情信息，从发起网页页面请求Request后分析HTTP请求后得到的各个请求资源信息（包括状态、资源类型、大小、所用时间、Request和Response等），可以根据这个进行网络性能优化。

操作：依次点击Network、键盘按键ctrl+R、点击下图all。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409130237839.png)
选中下图左图中第一行，
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409130327135.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqeXhsZA==,size_16,color_FFFFFF,t_70)
出现右图，右图有5项，表明查看具体资源的详情
　　通过点击某个资源的Name可以查看该资源的详细信息，根据选择的资源类型显示的信息也不太一样，可能包括如下Tab信息：
Headers 该资源的HTTP头信息。
Preview 根据你所选择的资源类型（JSON、图片、文本）显示相应的预览。
Response 显示HTTP的Response信息。
Cookies 显示资源HTTP的Request和Response过程中的Cookies信息。
Timing 显示资源在整个请求生命周期过程中各部分花费的时间。

**对于爬虫，核心部分是下图：用户的请求头**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200409130550750.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dqeXhsZA==,size_16,color_FFFFFF,t_70)
这里就不再解释每部分的意思，网上非常多。