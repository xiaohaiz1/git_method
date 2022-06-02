

##### 网站

IDM  下载神器

Internet Download Manager

https://www.dalipan.com/search  大力盘搜

https://live.bilibili.com/3344545   山东大学:可视计算   

https://my.oschina.net/

https://www.wolframalpha.com/

https://pan.baidu.com/s/1Act_yiIxVMbE-JEFP-ROtg#list/path=%2Fsharelink2831817335-250264416223194%2F2018%E5%B9%B4%E4%BC%A0%E6%99%BA%E6%92%AD%E5%AE%A2%E9%BB%91%E9%A9%ACpython%2B%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%2015%E6%9C%9F&parentPath=%2Fsharelink2831817335-250264416223194

小刀娱乐网 www.x6d.com  

爱分享  http://www.chinacsbs.com/zxjc/2779.html



-------------------

##### 在线 linux

http://cb.vu/
https://www.masswerk.at/jsuix/index.html
http://www.xuegoo.com/lessons/linux/linux/

https://www.tutorialspoint.com/index.htm
https://www.tutorialspoint.com/codingground.htm
https://www.tutorialspoint.com/computer_programming_tutorials.htm
https://www.masswerk.at/jsuix/index.html
https://www.ubuntu.org.cn/tour/zh-CN/#   #在线ubuntu

-----

sudo (switch user do)

Advanced Package Tool (apt-get)

usr : unix system resource

---

1. 命令字+命令选项参数（选项）+命令操作参数（参数）
2. 命令行格式
   1. command [option] [arguments]
      1. command 命令名, 命令程序名
      2. [option],  []可选, 命令选项, 对命令的要求
         1. 长选项, 短选项
            1. 短选项： 如-h，-l，-s (-  后接单个字母)
            2. 长选项：如--help，--list(-- 后接单词)
      3. [arguments], []可选, 参数, 描述命令的作用对象

##### 01  02 

1. 帮助命令 man, --help
   1. man(manual, 有问题找男人)

      1. ```
         man 2 printf  #查看系统函数printf
         # 2或3 man 3 printf
         ```

      2. ```
         man ls
         ```

   2. ```
         ls --help  # 命令帮助
         ```


2. history

   1. 历史命令, 
   2. 显示历史上使用的命令

##### 03

1. 操作系统
   1. 虚拟的硬件, 屏蔽硬件

##### 04 linux 操作系统

1. 语言: BCPL --C
2. Unix-->Minix-->linux(类unix操作系统)
   1. unix阵营 版本: FreeBSD
   2. linux阵营 各种版本: ubuntu(发展很快,一般都有它), redhat, suse, fedora,Centos keli???黑客开发

##### 05 linux版本和应用领域

1. Linux内核及发行版介绍
2. 应用领域: 服务器领域(主要分支), 嵌入式领域, 个人桌面领域

##### 06 虚拟软件和操作系统的关系

1. 虚拟软件, VMware, 物理机的虚拟

##### 07 08 Ubuntu

1. 三角和圆圈

   1. 点击
   2. 输入想要的软件, 显示软件

2. terminal

     1. 打开终端方式

          1. 直接运行终端的可执行文件
               1. 单击ubuntu桌面左侧`启动器`内部的主文件夹
                    1. 进入文件系统内部usr文件夹下的bin目录。
               2. 单击“搜索”标识，搜索框输入“gnome-terminal”然后按回车键。
               3. 搜索结果中会出现“gnome-terminal”的可执行文件，双击即可打开终端。
          2. 命令行打开终端
               1. ubuntu系统, 按Alt+F2快捷键打开`命令输入框`
               2. 输入“gnome-terminal”命令， 按`回车`键 可打开终端
          3. 快捷键：`Ctrl+Alt+T` 打开终端
               1. ubuntu系统 ：Ctrl+Alt+T, 打开终端
               2. 五种方法中最简便快捷的方法。
          4. Dash主页打开终端
               1. 单击Ubuntu桌面左上边的Dash主页图标
               2. 搜索框内输入“ter”,  搜索结果出现终端，单击终端图标即可打开终端
          5. 终端图标锁定在左侧`启动器`，直接打开终端
               1. 拖拽终端到桌面左侧的启动器,  单击启动器上的终端图标

          

3. 文件夹

   1. 点击
   2. 右击

4. 目录

   1. 没有`盘符`CDE

   2. / usr share aisleriot

      1. /  `根`目录
      2. /usr
      3. /bin, /usr/bin  可执行二进制文件(程序)目录
      4. /dev  linux的设备
      5. /etc
         1. etc不是什么缩写
         2. 是and so on的意思
            1. 源于 法语的 et cetera 
            2. 翻译成中文就是 等等 的意思. 
            3. 为什么在/etc下面存放配置文件
               1. 按原始的UNIX的说法([linux文件结构](https://www.baidu.com/s?wd=linux文件结构&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)参考UNIX的教学实现MINIX) 这下面放的都是一堆零零碎碎的东西, 就叫etc, 这其实是个历史遗留.
         3. 存放程序所需的整个文件系统的配置文件.
      6. /home, 用户目录(用户的家目录),  常用的
         1. 存放所有的用户, 
         2. ~ , 当前用户
      3. ~edu  用户edu的家目录
         4. 写的代码, 操作的所有东西 放入/home
         5. /home下的文件夹是用户名的文件夹
            1. /home/python,  python 用户名, 创建用户时自动在/home/下创建一个对应名字的文件夹
            2. /home/python/123, 新建的123文件夹在桌面显示
      7. /usr 应用程序存放目录

5. /temp 用户/程序的临时存放文件的目录, 垃圾等

6. ```
         / 杠
      \ 反杠
     ```

##### 09 10命令 ls, pwd, cd

1. ls

   1. ls `当前目录`中的文件,文件夹名
   2.  ls /bin  bin夹下的东西
   3. ls Documents  当前下Documents夹下的所有东西
   4. `-a`   显示隐藏文件/文件夹
      1.  `ls -a`
      2. 隐藏文件/文件名  前加 `.`
   5. `-l`   列(list)显示
      
      1. `drwxrwxr-w`   d dir文件夹, r可读, w可写, x可执行
      
      2. `-rwxrwxr-w`  `-`杠, 文件
      
      3. ```
         -rw-r--r--   1    root    root     14  11月 24 01:31 file3
         注释
         - 文件类型
         rw-r--r--  文件存取控制
         1 硬链接数
         root 文件属主
         root 文件所属的组
         14 文件大小
         11月 24 01:31 文件修改时间
         file3 文件名
         ```
      
   6. `-h`  文件大小 以适当单位显示
   7.  对应windows中的 dir 命令
   
2. cd  (change dir)更改目录

   1. cd Desktop  进入Desktop文件夹
   2. cd 文件夹名字
   3. cd .. 挑战到当前路径的上一层
   4.  cd -  //跳转到上一次的路径(最近的两个路径轮流)  # -代指上一个
   5. `cd ~`  //跳转到当前用户的家目录

3. pwd

   1. 显示路径

4. clear 

   1. 清屏, (不是真清屏)

5. ```
   .  当前路径
   .. 上一个目录
   ```

6. 放大`Ctrl+shift+'+'`

   缩小`Ctrl+'-'`

7. 通配符

   1. `*`  任意多个字符,或没有
      1.  `ls *.txt,  ls *.*   ls ab*.txt`
   2. `?`  一个通配符, 必须一个tou
      1. ` ls abc.t?t`

8. touch  , 只允许在自己的目录下`~`创建文件

   1. 创建文件

   2. ```
      touch a.txt
      touch bb
      touch cc.doc
      touch dd.c
      都可以
      ```

9. [xn]

   1. `[]`

   2. 从`[]`内只选**一个**

   3. `ls *.t[abcdef]t`   任意一个字符

   4. 连续的[a-f]与[abcdef]等效, 选一个

      1. ```
         ls a-f  
         # 查找文件名为a-f的文件, "-"处于方括号外失去通配符的作用
         ```

      2. `*`   0 或多个字符
      
      3. `?`   任意一个字符

10.  转义符`\`

        1.   `ls \*ab`  此时`*`不是通配符, 转义了

11.  `tab`  自动补全

##### 11 重定向 >

1. ```
   ls > text.txt
   ls -alh > text.txt
   ```

   1.  text.txt没有, 则新建, 然后写入内容
   2. text.txt存在, 覆盖内容

2. ```
   ls >> text.txt
   ```

   1. 重定向,内容追加, 不覆盖

##### 12 more ,   |管道 ,  cat

1. more  

   1. 分屏显示内容

   2. ```
      more text.txt
      ```

      1. 在terminal中显示text.txt内容
      2. 分屏显示

2. cat

   1. 显示内容(全显示), 查看内容

      1. ```
         cat haha.txt
         ```

   2. 合并

      1. ```
         cat test1.txt test2.txt > test3.txt
         # 合并test1.txt, test2.txt的内容到test3.txt中, 
         # test3.txt不存在, 则创建; 存在则覆盖.
         ```

      2. ```
         cat test1.txt test2.txt >> test3.txt
         # 追加合并
         ```

3. | 

   1. 管道
   2.  ls -alh | more
      1.  显示的内容到 管道(|), 管道中暂存
      2. 一行不可以显示多个命令, 管道 | 就是解决这个问题的

5. gedit

   1. 相当于window中text
   2. 启动：
      1. 菜单启动：应用程序——>附件——>文本编辑器
      2. 命令行启动： gedit
   3. 打开文件
      1. 方式一: 双击
      2. 方式二:  `gedit text.txt`
         1. 打开 text.txt文件
   4. 对比 `Vi`
      1. vi编辑器 : 所有Unix及Linux系统下标准的编辑器
         1. vi可以分为三种状态，分别是命令模式（command mode）、插入模式（Insert mode）和底行模式（last line mode）

##### 13 clear 

1. clear 清屏

##### 14,15,16 mkdir, tree, rmdir, rm -r, ln, grep

1. mkdir  文件夹
   
   1. 创建新目录(文件夹)
   2. 文件名,文件不能重名
1. mkdir a/b/c/d -p
   
   1. 递归创建文件夹
1. tree
   
   1. 以目录树方式显示文件/文件夹结构
1. rmdir
   1. `rmdir a`
   1. 删除空目录, 否则失败

5. rm

   1. 删除文件

      1. ```
         rm text.txt  # 删除文件
         ```
   
2. 删除无法恢复
   
3. 参数
   
   1.  `-i, -f, -r`
   
   2. ```
         rm text.txt -i  # 交互方式删除, y, n
         rm text.txt -f  #强制删除, 忽略不存在的文件, 不提示
         rm text.txt -r  #递归删除目录下的内容. 删除文件夹必须加此参数
         ```
   
6. ln  

   1. 链接文件, 类似Windows下的快捷方式

   2. ```
      ln -s test.txt test_soft.txt
      # 软链接. 删除源文件, 软连接文件失效
      ```

   3. ```
      ln test.txt test_hard.txt
      # 两个文件名test.txt, test_hard.txt指向同一个文件内容
      # 删除软连接, 会改变硬链接数
      ```

7. grep

   1. 文件中找内容

   2. ```
      grep [-选项] '内容串' 文件名
      ```

      1. ```
         grep 'a' 1.txt  # 从1.txt文件中找含a的内容
         ```

      2. ```
         -v  取反
         -n  显示行号
         -i  忽略大小写
         ```

   3. 正则

      1. ```
         grep -n '^a' 1.txt  #  ^a  a开头 行
         ```

      2. ```
         grep -n 'ke$' 1.txt  # ke$  ke结尾 行, 
         ```

      3. ```
         grep -n '[Ss]igna[Aa]' 1.txt  #含有[]中一个
         ```

      4. ```
         grep -n 'e.e' 1.txt  # 含有, 不一定从头开始, 一个非换行符, 任一个字符, eee, eae, eve, 但不匹配ee, eaae;
         ```


##### 18 find, cp , mv

1. find

   1. 文件夹中找文件

   2. 语法格式

      1. ```
         find 路径 参数 '*.t?t'
         ```

   3. 参数: `-name, -size, -perm`
   
      1. ```
         find ./ -name test.sh  # -name 名字, 名test.sh的文件
         find ./ -size 2M # -size 大小, 2M 等于2M文件,+2M 大于2M, -2M 小于2M
      find ./ -perm 0777   # -perm 权限, 权限777的文件或目录
         ```

   4. 支持正则查找
   
      1. `'*.sh'`
      2. `"[A-Z]*"`
      3. `-size +4k -size -5M`  #+4k 大于4k, -5M 小于5M
      4. 

2. cp  拷贝(文件),  

   1. 文件复制到另一个文件中或另一个文件夹中,. 复制的源是文件, 不是文件的内容

   2. 语法

      1. ```
         cp [OPTION]... SOURCE... DIRECTORY
         cp [OPTION]... -t DIRECTORY SOURCE...
         ```

      2. 两个语法的效果一样的，都是把 SOURCE文件复制到 DIRECTORY 目录

   3. 示例

      1. `cp a b`  # a文件复制到b文件夹下, 不移动文件夹
      2. `cp a/* b`  # a文件夹下的所有文件复制到b文件夹下, 不移动文件夹

   4. 选项 -a, -f, -r, -v

      1.  -a  保留链接,文件属性, 
         1.  `cp -a a/* b `
      2.  -f  # 禁止交互操作/重名不提示
         1. `cp -f a/* b `
      3.  -r  复制文件夹 (文件, 文件夹), 文件夹到文件夹
         1. `cp -r a/* b `
      4.  -v 显示进度
         1. `cp -v a/* b`

   5. 文件复制重命名

      1. 示例
         1. `cp file1 dir1/renamed_file1`
         2. 把当前目录下的 file1 复制到 dir1 目录下，并且重命名为 renamed_file1

   6. 目录复制重命名

      1. 示例
         1. `cp -r dir1/ dir2/renamed_dir1`

   7. 文件覆盖

      1. 示例
         1. `cp  file1 file2 dir1`
         2. dir1 目录下存在 file1 文件， 默认覆盖
      2. 处理方法 : 选项
         1.  `-n` ，不覆盖操作。
         2.  `-i` ，提示是否覆盖。
         3.  `-b` ，覆盖前备份，备份文件名: 原始文件名加上一个波浪线。
         4.  `-u` ，文件比较新时，才覆盖。

3. mv  移动(剪切), -f, -i, -v

   1. `mv a b`  # a文件夹正移动(剪切)到b文件夹下, 同时可以改名
      
   2. 参数:
   
   1. `-f`  # 禁止交互式操作
   
   2.  `-i`# 交互式
      3.  `-v` # 显示进度
   
   3. `mv -f a b`   #-f  # 禁止交互操作/重名不提示


##### 19 打包 压缩 解压缩

1. tar ,  打包/解包

   1. tar 格式
   
      1. `tar -cvf Test.tar a.c`  # 打包a.c为Test.tar, tar打包格式. 打包不压缩

      2. `tar -xvf Test.tar`  #解包
      3. `tar -xvf  Text.tar  -C /usr/`  #解包到指定路径
   
   
   
   2. tar.gz ,  打包 压缩 /解压缩解包
   
      1. `tar -zcvf Test.tar.gz a.c` # 打包压缩a.c为 tar.gz格式
      
      2. `tar -zxvf Test.tar.gz`  #解压缩解包

   3. tar.bz2 ,  打包压缩/解包解压缩

      1. `tar -jcvf Test.tar.bz2 a.c`  # 打包, 压缩
      
      2. `tar -jxvf Test.tar.bz2`  # 解压缩 解包
      
         
   
2. gzip , 压缩/解压缩

   1. `gzip Test.tar`  # 压缩 ,
      
   2. `gzip -d Test.tar.gz` # 解压缩

3. bzip2   # 压缩

   1. `bzip Test.tar`  # 格式是 tar.gz2
   2.  对应 tar -jcvf , tar -jxvf

##### 20 文件扩展名

1. linux文件的`扩展名`并没有用处
2. 用于区别文件


---

**command [options] [arguments]** //中括号代表是可选

**选项分为长选项和短选项。**

**短选项：比如-h，-l，-s等。(-  接单个字母)**

　　l短选项都是使用‘-’引导，当有多个短选项时，各选项之间使用空格隔开。

　　l有些命令的短选项可以组合，比如-l –h 可以组合为–lh

　　l有些命令的短选项可以不带-，这通常叫作BSD风格的选项，比如ps aux

　　l有些短选项需要带选项本身的参数，比如-L 512M

**长选项：比如--help，--list等。(-- 接单词)**

　　l长选面都是完整的单词

　　l长选项通常不能组合

　　l如果需要参数，长选项的参数通常需要‘=’，比如--size=1G

**参数**

　　参数是指命令的作用对象。

　　如ls命令，不加参数的时候显示是当前目录，也可以加参数，如ls /dev, 则输出结果是/dev目录。

　　以上简要说明了选项及参数的区别，但具体Linux中哪条命令有哪些选项及参数，需要我们靠经验积累或者查看Linux的帮助了。



---











