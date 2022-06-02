


`http://www.bohyoh.com/`  柴田望洋 <<明解C++>>, <<明解java>> 比国内的好很多

[-参数] 可选的, 可有可无

#### 02-linux基础

##### 01 zip, unzip, which

1. zip  linux中,    Windows中常用

   1. 压缩

      1. `zip [-r] 目标文件(没有扩展名) 源文件`
         
      2. ```
   #将 /home/html/ 这个目录下所有文件和文件夹打包为当前目录下的 html.zip
         zip -q -r html.zip /home/html
         ```
      
   2. 解压 

      1. ```
      unzip -d 解压后目录 压缩文件
         ```
   
      2. ```
      unzip -d ./test myzip.zip  # 解压到当前目录下/test文件夹中
         ```
   
2. which 

   1. 命令的路径

   2. ```
      which ls  # 命令ls的路径
      ```


##### 02 03 linux多用户多任务操作系统, ssh, ping, exit

1. 网络
   1. NAT模式 # 借助虚拟NAT设备和虚拟DHCP服务器，虚拟机通过`Windows`上网
   2. 桥接模式  # linux通过 `物理网卡`上网
   3. 仅主机模式  # 仅linux与Windows能通信, 不能上网
2. 网卡:
   1. 本地连接: 真实物理网卡
   2. linux虚拟网卡
      1. ens33, 或 eth0, 或eth1等等
3. 查看ip
   1. ifconfig, 
   2. ping ip , ping 192.168.17.76  测试网络连接
4. ssh 远程登录
   1. ssh python@192.168.17.76
   2. ssh  用户名@要登录的ip
   3. 必须先安**ssh**服务
   4. Secure Shell(SSH) 
      1. IETF(The Internet Engineering Task Force) 制定, 应用层基础上的安全网络协议
      2. 专为远程登录会话(甚至Windows远程登录Linux服务器)和 网络服务提供安全性的协议, 有效弥补网络中的漏洞
      3. SSH， 传输的数据 加密， 防止DNS欺骗和IP欺骗
5. who 当前用户信息
   1. who -q  用户数, 用户名
   2. whoami  当前用户名
      1. 区别: `who, whoami, who am i`
         1. who：登录的用户名（不显示su命令切换用户的登录者）
            1. 第一列表示：用户；
            2. 第二列表示：线路；
            3. 第三、四列表示：用户登录的日期和时间；
            4. 第五列表示：备注（代表：当前网络模式所对应网卡的ip地址）
         2. who am i: 登录的用户名，尽管切换了多个用户
            1. who am i 和who 一样的，想当于who -m命令
            2. who am i : 当前终端（我当前是远程登录终端）的用户信息
            3. who: 所有终端（包括远程终端和虚拟机的终端）的用户信息
            4. 示例:
               1. 以root用户登录系统，然后执行su - hwangchen 切换到hwangchen用户下，此时who am i 显示的是root用户，而whoami则显示hwangchen用户。
         3. whoami:   当前用户名 (), **当前有效用户**的用户名
6. exit
   1. 注销当前用户 

##### 04 05 useradd, passwd, su, userdel

1. useradd

   1. 添加用户, 其家目录在 /home/下对应的家目录中
   2. `useradd laowang -m -d /home/laowang`
      1. `useradd 新用户名 -m -d /home/新用户名`
         1. -m 自动创建
         2. -d 创建家目录

2. passwd

   1. 设置密码

   2. `passwd laowang`
      1. 给用户laowang设置密码

3. su

   1. 切换用户 switch user
   2. `su laowang`
      1. `su - laowang`   # 切换用户至laowang, 同时跳转至其家目录

4. userdel

   1. 删除用户, 需要权限
   2. `userdel laowang`
      1. `sudo userdel laowang`

##### 06 sudo, groupadd, groupdel, groupmod

​	mod  modify修改

1. sudo 

   1. 添加权限

   2. `sudo ls`

   3. `sudo -s`  切换至超级用户

      1. ```
         python@ubuntu:/home$ # $代表普通用户
         root@ubuntu:/home#  # #代表超级用户
         ```


   4. su 切换用户

   5. Centos  root管理员与windows中adminstrator类似, ubuntu没有管理员

      ubuntu有超级用户root, 

      | Centos     | ubuntu       | windows            |
      | ---------- | ------------ | ------------------ |
      | root管理员 | root超级用户 | adminstrator管理员 |

      

   5. `sudo -s`  

2. 用户组

   1. groupmod  

      1. 查看用户组两个方法

         1. 法1 `cat /etc/group`
         2. 法2`groupmod和3tab键`

      2. 普通用户添加sudo权限

         1. 新创建的用户，默认不能sudo，需要进行一下操作

            1. ```
               sudo usermod -a -G adm 用户名
               sudo usermod -a -G sudo 用户名
               ```


   2. groupadd  用户组添加

      1. `groupadd ZZZ`  添加用户组ZZZ
         1. 用户必须有root权限
      2. 用户与组
         1. `useradd xxx -m -d /home/xxx -g ZZZ`
            1. 不指定组名, 创建默认同名组
            2. 指定组名`-g ZZZ`
         3. /home/xxx,用户xxx的家目录
            4. ZZZ是xxx的组

   3. usermod  用户更改所在组
   
   1. `usermod -g ZZZ laowang`  # 用户laowang修改为ZZZ用户组
      
2. `usermod -a -G MMM laowang`  # 用户laowang再添加入用户组MMM中
   
   4. groupdel 用户组删除
   
      1. ```
      groupdel ZZZ  # 删除用户组ZZZ
         ```


##### 07 chmod  

1. 修改权限

2. `drwxrwxrwx`  d:目录文件, rwx,用户权限, 第二个rwx 用户组权限, 第三个rwx 其他权限

   1. u 用户, g 用户组, o 其他, a 指u g o

   2. ```
      r read  4
      w write 2
      x 执行   1
      - 无
      ```

3. ```
   chmod u+w 1.py  # user 1.py文件增加w权限
   chmod g-w 1.py  # 用户组 1.py文件撤销w权限
   chmod u=rw 1.py  # 用户 1.py设置rw权限
   chmod u=, g=r, o=rw 1.py # 同时设置用户,组,其他的权限
   ```

4. 数字设定权限

   1. ```
      chmod 777 test.txt  # test.txt设置权限777 7=4+2+1(w+r+x)
      ```

5. **同时**设定文件夹及文档的权限

   1.  -R

   2. ```
      chmod 777 text -R
      chmod -R 777 text.txt
      ```


##### 08 cal, date, ps, top, kill

1. cal 显示日历

   1. 月份, 年, 星期,

2. date 

   1. 设置时间格式（需要管理员权限）

      1. ```
         date [MMDDhhmm[[CC]YY][.ss]] +format
         ```

         1. CC 年前两位, YY为年的后两位，MM为月，mm为分钟，DD为天，hh为小时   . ss为秒
         2. date 010203042016.55

   2. 显示时间格式(占位符方式)

      1. date '+%y,%m,%d,%H,%M,%S',   +后都是时间格式

         1.  %Y  2016, %y 16 年

         2. %m 月

         3. %d  日

         4. %H  时''

         5. %M  分

         6. %S  秒

         7. ```
            # 方法1:
            date '+%Y/%m/%d'
            
            # 方法2
            date '+%Y=%m=%d'
            ```

3. ps

   1. ```
      ps -aux
      ```

      1.  -a  所有进程
      2.  -u 详细状态
      3.  -x  显示没有控制终端的进程

   2. 只显示一次终端运行状态

   3. 没运行的叫程序, 运行的叫进程top

4. top

   1. 动态显示进程状态
   2.  user: 用户, COMMAND: 命令/程序, PID: 进程号

5. kill  PID

   1. 关闭进程
   2. `kill -9 10367`  完全关闭进程10367
      1. -9 是 -Signal

##### 09 关机,启动,ifconfig

1. reboot  重启

   1. 重启与挂起.  挂起: 休眠

2. shutdown 关机

   1. `-r` 重启,  `-h` 关机
      1. shutdown -r now  # 重新启动
      2. shutdown -h now  #立即关机
      3. shutdown -h 20:25  # 今天20:25关机
      4. shutdown -h +10  #过10分钟关机

3. init 0  #关机

4. init 6  # 重启

5. df

   1. 检查磁盘空间

   2. ```
      -a # 显示所有文件系统的磁盘情况
      -m # 以1024字节为单位显示
      -t # 显示指定文件系统的磁盘情况
      -T # 显示文件系统
      ```

   3. `df -ah  # -h 合适的单位显示

6. du 

   1. 与df 类似,更侧重于磁盘的使用状况

   2. | 选项 | 含义                                               |
      | :--- | -------------------------------------------------- |
      | -a   | 递归显示指定目录中各文件和子目录中文件占用的数据块 |
      | -s   | 显示指定文件或目录占用的数据块                     |
      | -b   | 以字节为单位显示磁盘占用情况                       |
      | -l   | 计算所有文件大小，对硬链接文件计算多次             |

7. ifconfig 网络设置

   1. ens33 网卡

   2. ```
      ifconfig ens33 down # 关闭网络
      ifconfig ens33 up # 打开网络
      ifconfig ens33 172.168.138.64  # 修改IP
      ```

8. ping

   1. `ping 173.168.138.64`

##### 10 编辑器: gedit, sublime 

1. gedit   基本的文本编辑

   1. ```
      geidt 1.py  #有打开, 没有创建打开
      ```

   2. 类似windows中记事本

2. sublime

   1. ```
      sublime 1.py # 有打开, 没有创建打开
      ```

   2. 跨平台 windows, linux , mac os

##### 11 vi  vim

1. vim 是vi的扩展

   1. 命令模式, 编辑模式, 末行模式

   2. 命令模式-->**编辑模式** 方法:

      1. ```
         i 光标前
         a 光标后
         o 光标下行
         I 行首
         A 行末
         O 光标上行
         ```

      2. ESC 退回到命令模式

   3. 命令模式-->**末行模式**

      1. `:`   #进入方式

      2. ```
         w 保存
         q 退出
         ! 强制
         ```

         1. `: w filename` （以指定的文件名filename保存）
         2. :wq  ,存盘并退出vi, 或: 命令模式 shift+2zz(保存退出)
         3. :q!   ,不存盘强制退出vi
         4. :X  ,加密, 大写

      3. 替换(末行模式)

         1. `:%s/abc/123/g`  #所有行中abc 用123 替换
         2. `:1,10s/abc/123/g`  #行号1至10, abc用123替换

      4. 执行shell命令

         1. ```
            !命令  #!ls 执行ls命令
            ```

      5. ESC 退回命令模式

   4. **命令模式**

      1. 移动上下左右

         1. | h    | j    | k    | l    |
            | ---- | ---- | ---- | ---- |
            | 左   | 下   | 上   | 右   |
         
      2. 移动 一个字/ 文件头 文件尾

         1. | w                      | b                      | gg       | G        | 15G        | M              | L        |
            | ---------------------- | ---------------------- | -------- | -------- | ---------- | -------------- | -------- |
            | 后移一个字   # 光标 后 | 前移一个字   # 前 光标 | 文件开头 | 文件末尾 | 移动至15行 | 屏幕中间行行首 | 末行行首 |

      3. 段移动

         1. | {         | }         |
            | --------- | --------- |
            | 按段 上移 | 按段 下移 |

      4. 移动屏单位

         1. | `ctrl d` | `ctrl u` | `ctrl f` | `ctrl b` |
            | -------- | -------- | -------- | -------- |
            | 下翻半屏 | 上翻半屏 | 下翻一屏 | 上翻一屏 |

      5. 复制,黏贴,剪切, 撤销, 反撤销

         1. | yy                          | p    | dd                          |
            | --------------------------- | ---- | --------------------------- |
            | 复制, 8yy 从光标到行号8复制 | 黏贴 | 剪切, 8dd 从光标到行号8剪切 |

      6. 删除

         1. | x                             | X                                   | d0                   | dw                         | dd         |
            | ----------------------------- | ----------------------------------- | -------------------- | -------------------------- | ---------- |
            | 删除光标后一个字符, 等价于del | 删除光标前一个字符, 等价于Backspace | 删除本行光标前的内容 | 删除本行光标后和光标的内容 | 删除光标行 |

      7. 撤销

            1. | u    | ctrl +r |
                  | ---- | ------- |
                  | 撤销 | 反撤销  |

                  

      8. 文本移动

            1. | `>>`     | `<<`     |
                  | -------- | -------- |
                  | 文本右移 | 文本左移 |

                   

      9. 多选(可视模式)

            1. | v          | V                   |
                  | ---------- | ------------------- |
                  | 按字符多选 | 按行多选, 配合 d,yy |

      10. 替换

            1. `[addr]s/源字符串/目的字符串/[option]`

                  1. [addr] : 检索范围,如:
                        1. `1,n`: 第1行到n行
                        2. `%` : 整个文件, 同"1,$"
                              ".,$":表示从当前行到文件尾
                        3. [addr]省略时表示当前行
                  2. s 替换操作, `substitute` 缩写
                  3. [option] :  操作类型
                        1. g : globe, 全局替换
                        2. c : confirm, 确认
                        3. p :  替代结果逐行显示(Ctrl + L恢复屏幕)
                        4. i : ignore,不区分大小写

            2. 示例

                  1. `:s/a/b/`    当前行第一个a替换为b
                  2. `:s/a/b/g`    当前行的所有a替换为b
                  3. `:%s/a/b`    每行第一个a替换为b
                  4. `:%s/a/b/g`     整个文件的所有a替换为b
                  5. `:n,$s/vivian/sky/`     替换第 n 行开始到最后一行中每一行的第一个 vivian 为 sky

            3. `#`或`+` 作为分隔符, 此时`/` 不作分隔符

                  1. `:s#vivian/#sky/#`         替换当前行第一个 vivian/ 为 sky/

            4. ```
                  r 当前字符
                  ```

      11. 查找 `/`

             1. | `/pattern<Enter> `        | `?pattern<Enter>`         |
                   | ------------------------- | ------------------------- |
                   | 向下查找pattern匹配字符串 | 向上查找pattern匹配字符串 |

             2. 用了查找命令之后，使用如下两个键快速查找

                   1. | n                | N          |
                         | ---------------- | ---------- |
                         | 同一方向继续查找 | 反方向查找 |

             3. 示例

                   1. `/abc<Enter>`      #查找ab
                   2. `/ abc <Enter>`    #查找abc单词（注意前后的空格） 

             4. pattern 特殊字符， （/、^、$、*、.）， 前三个 vi与vim通用的，“/”为转义字符。
                1. `/^abc<Enter>`    #查找以abc开始的行 
                2. `/test$<Enter>`    #查找以abc结束的行 
                3. `//^test<Enter>`    #查找^tabc字符串

             


​      

​      



