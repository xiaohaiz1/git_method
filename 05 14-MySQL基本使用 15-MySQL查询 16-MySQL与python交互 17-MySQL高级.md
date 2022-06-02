

#### 学习方法

1. 不停地回忆
   1. 核心:尝试回忆
   2. 训练
      1. 列提纲
      2. 回忆 提纲下的内容 (根据提纲回忆)
      3. 第二次看一遍提纲
      4. 再回忆  一遍  提纲下的内容
      5. 再对照一遍提纲
2. 回忆
3. 原理

#### 重点

1. SQL 指令(关键字)不区分大小写
2. 数据字符串区分大小写, 字符串用 `" "` 或`' '` 括起来,  "A"与'a' 不同
3. 单引号与双引号 相同,  如: "a"与'a'相同, 
4. 字段名 不区分大小写, `SCHOOL, scHOOL, school, SCHOOl,...`的结果相同
5. 空格 是字符,  `' a'`与`'a'`不同,  
6. 字段名 不区分大小, 不用引号
7. 表名 不区分大小, 不用引号
8. 关键字 不区分大小, 不用引号. SELECT  select  Select 都是一样的.

### 数据库

SQL习题网站:

 https://leetcode-cn.com/problemset/all/    力扣网, 练习算法,数据库,shell

#### mysql密码

mysql57 服务名

密码: mysql

用户名root

服务面板中查看 mysql 的服务名  mysql57


---------

### 14 MySQL-基本使用

#### 14.1 数据库基础知识

1. 数据库知识点
   1. 数据库, 
   2. RDBMS : Relational Database Management System，从字面上可以理解为关系数据库管理系统
   3. SQL 语言
   4. MySQL :  数据库系统名

14.1.1 数据库存储

1. 存储手段

   1. 现代化手段 -- 文件

      1. 简单, 如python中的open 打开文件，用read/write对文件 读写，close关闭文件
      2. 容量较大的数据，不能够很好的满足

   2. 现代化手段 -- 数据库

      1. 持久化 存储
      2. 读写速度极 高
      3. 保证数据的 有效性
      4. 对 程序 支持性非常好，容易 扩展

   3. 程序员看到的 数据库 样子: 表格

      `mysql> select * from goods ;`

      <table>
          <tr><td>id</td><td>name</td><td>cate</td><td>price</td><td>is_sleoff</td><td>count</td><td>img_url</td></tr>
          <tr><td>1<br>2<br>3<br>4<br>5<br>6<br></td><td>Calaxy G7<br>荣耀8青春版<br>中兴(ZTE) A2<br>vLvo xpl<br>小辣板<br>红辣椒<br></td><td>双卡双待<br>双卡双待<br>双卡双待<br>双卡双待<br>双卡双待<br>双卡双待<br></td><td>1999.00<br>1999.00<br>1099.00<br>649.00<br>4498.00<br>849.00<br></td><td></td><td>10<br>20<br>30<br>15<br>20000<br>200</td><td>https://item. jd. com/4746242.html<br>https://item. jd. com/4746242.html<br>https://item. jd. com/4746242.html<br>https://item. jd. com/4746242.html<br>https://item. jd. com/4746242.html<br>https://item. jd. com/4746242.html</td></tr>
      </table>

   4. 客户看到的是 网页 的样子

##### 14.1.2 数据库

1. 数据库: 一种特殊的 文件， 存储着 数据
2. 关系型数据库 核心元素
   1. 数据 行 (记录)
   2. 数据 列 (字段)
   3. 数据 表 (数据 行 的集合)
   4. 数据 库 (数据 表 的集合)
3. 对比
   1. 一个Excel文件 好比一个数据库,  一个sheet 好比 一个数据表

##### 14.1.3 RDBMS

1. Relational Database Management System
2. 两种类型的数据库：关系型数据库、非关系型数据库
3. 关系型数据库RDBMS
   1. 建立在 关系模型基础 上的数据库
   2. 借助于 集合代数 等数学概念和方法来处理数据库中的 数据
4. 关系型数据库:
   1. oracle,  以前 的大型项目中 用,银行,电信等项目
   2. mysql,  web时代 使用最广泛的关系型数据库
   3. ms sql server,  微软的项目中使用
   4. sqlite：轻量级数据库，主要应用在 移动 平台

##### 14.1.4 RDBMS和数据库的关系

```
                                    ┌───────────────────┐
                                    │数据库1             │
                                  / │┌───────┐ ┌───────┐│
RDBMS-client       RDBMS-server /   ││数据表1 │ │数据表2 ││
                               /    │└───────┘ └───────┘│
 ┌───────┐          ┌───────┐ /     └───────────────────┘
 │       │  SQL语句  │       │       ┌───────────────────┐
 │       │ <─────>  │       │─────> │数据库2             │
 └───────┘          └───────┘       │┌───────┐ ┌───────┐│
                                    ││数据表1 │ │数据表2 ││
                                    │└───────┘ └───────┘│
                                    └───────────────────┘
```

总结

RDBMS-client $\leftarrow SQL语句 \rightarrow$RDBMS-server $\rightarrow$数据库n中的数据表i,  i$\in$n

##### 14.1.5 SQL 语言

1. Structured Query Language

   1. 结构化查询语言， 操作RDBMS的数据库语言
   2. SQL 支持 oracle,sql server,mysql,sqlite 等 所有的关系型的数据库
   3. 不区分大小写

2. SQL语句 分:

   1. **DQL**：数据查询,   如`select`
   2. **DML**：数据操作，数据的 增加、修改、删除，如`insert`、`udpate`、`delete`
   3. TPL：事务处理，事务处理，如`begin transaction`、`commit`、`rollback`
   4. DCL：数据控制，授权与权限回收，如`grant`、`revoke`
   5. **DDL**：数据定义，数据库、表的管理，如`create`、`drop`
   6. CCL：指针控制，用指针操作表，如`declare cursor`

3. 要求

   1. web程序员 ，重点是数据的crud（增删改查）， 熟练编写DQL、DML，DDL. 其他了解即可 如TPL、DCL、CCL

   1. ```
      # 创建Connection连接
      conn = connect(host='localhost', port=3306, user='root', password='mysql', database='python1', charset='utf8')
      
      #创建Cursor对象
      cs = conn.cursor()
      
      # 更新
      sql = 'update students set name="刘邦" where id=6'
      
      # 删除
      sql = 'delete from students where id=6'
      # 执行select语句，并返回受影响的行数：查询一条学生数据
      #sql = 'select id,name from students where id = 7'
      #不区分大小写, 上下两句都可以
      # sql = 'SELECT id,name FROM students WHERE id = 7'
      count=cs.execute(sql)
      # 打印受影响的行数
      print(count)
      ```

##### 14.1.6 MySQL

1. MySQL 关系型数据库管理系统

   1. 瑞典 MySQL AB 开发，后 被 Sun 收购，Sun 又被 Oracle 收购，Oracle的产品
   2. MySQL的发明者名叫 Michael “Monty” Widenius，MySQL 以他女儿的名字 “My”  命名 

2. 特点

   1. C和C++编写，多种编译器测试，源代码可移植性
   2. 支持多种操作系统，如**Linux**、**Windows**、AIX、FreeBSD、HP-UX、**MacOS**、NovellNetware、OpenBSD、OS/2 Wrap、Solaris等
   3. 为多种编程语言提供了API，如**C**、**C++**、**Python**、**Java**、**Perl**、**PHP**、Eiffel、Ruby等
   4. 多线程，充分利用CPU资源
   5. 优化的SQL查询算法，高速查询
   6. 多编码支持，如GB2312、BIG5、UTF8
   7. TCP/IP、ODBC、JDBC 连接数据库的 途径
   8. 管理、检查、优化数据库操作的管理工具
   9. 大型的数据库。千万条记录
   10. 多种存储引擎
   11. MySQL 软件 双授权政策， 社区版和商业版， 体积小、速度快、 成本低， 开放源码，中小型网站 首选 MySQL 为网站数据库
   12. 标准的SQL语言
   13. Mysq可 定制的，GPL协议，修改源码开发自己的Mysql系统
   14. 在线DDL更改
   15. 复制全局事务标识
   16. 复制无崩溃从机
   17. 复制多线程从机

   > 开源 免费 使用广, 跨平台, 多种语言调用的 API 数据库开发的首选

#### 14.2 MySQL 安装

1. 服务器端 安装

   1. 服务器端： 终端  命令，回车，提示

      `sudo apt-get install mysql-server`

      1. ubuntu镜像中已安装 `mysql服务器端`，无需再安装，并且 开机自启动

   2. 服务器端MySQL

      1. 服务器 `接收`客户端的请求、`执行`sql语句、`管理`数据库
      2. 服务器端 以服务方式管理，名称为mysql

   3. 启动服务

      `sudo service mysql start`

   4. 查看进程中 mysql服务

      `ps aux|grep mysql`

   5. 停止服务

      `sudo service mysql stop`

   6. 重启服务

      `sudo service mysql restart`

   7. 配置

      1. 配置文件目录  /etc/mysql/mysql.cnf

         ```
         !includedir /etc/mysql/conf.d/
         !includedir /etc/mysql/mysql.conf.d/
         ```

      2. 查看配置 

         1. conf.d目录，打开mysql.cnf， 没有配置. 

         2. mysql.conf.d目录，打开mysql.cnf， 看到配置项 

            ```
            cd mysql.conf.d/
            ls
            vi mysqld.cnf
            ```

      3. `service mysql`配置内容

         ```
         bind-address, 绑定的服务器的ip，默认 127.0.0.1
         
         port, 端口，默认 3306
         
         datadir, 数据库目录，默认 /var/lib/mysql
         
         general_log_file, 普通日志，默认 /var/log/mysql/mysql.log
         
         log_error, 错误日志，默认 /var/log/mysql/error.log
         ```

         

2. 客户端

   1. 客户端

      1. 供`开发人员` 与 `dba` 用
      2. 用 `socket` 方式与服务器端通信
      3. 使用的方式 `navicat`、`命令行mysql`

   2. navicat 图形化界面客户端

      1. navicat 安装

         1. [Navicat官网](https://www.navicat.com.cn/)下载

         2. 压缩文件拷贝到ubuntu虚拟机，放桌面，解压

            `tar zxvf navicat112_mysql_cs_x64.tar.gz`

         3. 进入解压目录，安装命令

            `./start_navicat`

            两次“取消”按钮, "试用"按钮

      2. 中文乱码

         1. 打开start_navicat文件, 修改

            `export LANG="en_US.UTF-8"改为export LANG="zh_CN.UTF-8"`

      3. 试用期

         1. 删除用户目录下的`.navicat64`目录

            ```
            cd ~
            rm -r .navicat64
            ```

   3. 命令行客户端

      1. 命令行myslq 安装

         1. 终端运行 命令

            `sudo apt-get install mysql-client`

            按提示填写信息

         2. 说明:
            1. ubuntu镜像中已 安装 `mysql客户端`，无需再安装

            2. 详细连接 命令 查看帮助文档

               `mysql --help`

      2. 登录

         最基本的登录命令，输入后回车

         `mysql -u root -pmysql`    //`-u` 用户名, `-p`密码

      3. 退出

         1. 按键: `ctrl+d`, 
         2. 命令 `quit` 或  `exit`

#### 14.3 数据完整性

- 一个数据库: 一个完整的业务单元， 多张表，数据 存储在表中

1. 数据类型

   1. 数据类型 原则：够用就行，尽量用范围小的， 不用大的， 节省存储空间

   2. mysql的数据类型 

      1. 整数：int，bit

      2. 小数：decimal, 浮点数, 如decimal(5,2) 共存5位，小数 2位

      3. 字符串：varchar,char, TEXT

         1. char 固定长度 字符串， char(3)， 填充'ab'时 补一个空格`'ab '`
         2. varchar 可变长度 字符串， varchar(3)，填充'ab'时 存储'ab'

      4. 日期时间: date, time, datetime

      5. 枚举类型: enum

         

   3. 说明

      1. 图片、音频、视频 不存储在数据库, 而是上传到某个服务器，表中存储文件的`保存路径`
      2. text, 存储大文本, 字符大于4000时推荐用

2. 约束

   1. primary key  主键, 物理上存储的顺序

   2. not null  非空, 字段不允许写 空值

   3. unique  惟一键, 字段的值不允许 重复

   4. default  默认, 不填写值时用默认值, 填写时以填写为准

   5. foreign key  外键,  对关系字段进行约束

      1. 当填写关系字段值时，会到关联的表中查询此值是否存在，如果存在则填写成功，如果不存在则填写失败并抛出异常
   2. 说明：外键约束保证数据的有效性，但数据的crud（增加、修改、删除、查询）时，降低数据库的性能. 不推荐用. 那么数据的有效性怎么保证呢？答：可以在逻辑层进行控制
   
6. 数值类型(常用)
   
      | 类型 INT    | 字节大小 | 有符号范围(Signed)                         | 无符号范围(Unsigned)     |
      | :---------- | :------- | :----------------------------------------- | :----------------------- |
      | TINYINT     | 1        | -128 ~ 127                                 | 0 ~ 255                  |
      | SMALLINT    | 2        | -32768 ~ 32767                             | 0 ~ 65535                |
      | MEDIUMINT   | 3        | -8388608 ~ 8388607                         | 0 ~ 16777215             |
      | INT/INTEGER | 4        | -2147483648 ~2147483647                    | 0 ~ 4294967295           |
   | BIGINT      | 8        | -9223372036854775808 ~ 9223372036854775807 | 0 ~ 18446744073709551615 |
   
7. 字符串
   
      | 类型    | 字节大小 | 示例                                                         |
      | :------ | :------- | :----------------------------------------------------------- |
      | CHAR    | 0-255    | 类型:char(3) 输入 'ab', 实际存储为'ab ', 输入'abcd' 实际存储为 'abc' |
      | VARCHAR | 0-255    | 类型:varchar(3) 输 'ab',实际存储为'ab', 输入'abcd',实际存储为'abc' |
   | TEXT    | 0-65535  | 大文本                                                       |
   
8. 日期时间类型
   
      | 类型      | 字节大小 | 示例                                                  |
      | :-------- | :------- | :---------------------------------------------------- |
      | DATE      | 4        | '2020-01-01'                                          |
      | TIME      | 3        | '12:29:59'                                            |
      | DATETIME  | 8        | '2020-01-01 12:29:59'                                 |
      | YEAR      | 1        | '2017'                                                |
      | TIMESTAMP | 4        | '1970-01-01 00:00:01' UTC ~ '2038-01-01 00:00:01' UTC |

#### 14.4 Navicat 图形界面工具 操作

1. Navicat连接

   1. 打开navicat，点击 “连接”，选择“mysql”，弹出窗口如下图

   2. 弹出窗口 填写:  `链接名:local` 、`主机ip: localhost`、`端口: 3306`、`用户名: root`、`密码: local`  、数据库名称 

   3. 确定， 左侧栏 `local`数据库名称，双击打开连接

##### 14.4.1 Navicat 创建数据库

1. 创建数据库
   1. 左侧栏 `空白处` 右击，选择“新建数据库”，点击

   2. 弹出`新窗口`，填写数据库`名称`  选择`语言`

      ```
      数据库名:python
      字符集: tf8 -- UTF-8 Uni code
      排序规则: utf8_general_ci
      ```

   3. 点击“确定”创建 数据库， 左侧 出现创建的数据库，`双击`打开

2. 编辑数据库 : 创建, 存储, 删除

   1. 左侧栏数据库 `python` 右击，弹出菜单

   2. 选择`编辑数据库`， 修改字符集、排序规则

      ```
      数据库名:python
      字符集: tf8 -- UTF-8 Uni code
      排序规则: utf8_general_ci
      ```

   3. 选择`转储sql文件`-->`结构和数据`，  结构和数据的备份

   4. 选择`删除数据库`， 数据库删除

##### 14.4.2 Navicat 创建数据表

1. 创建数据表

   1. 点击工具栏“表”，点击“创建表”

   2. 弹出新窗口，创建班级表`classes`表名，填写各字段, 类型

      ```
      id 字段， int类型，无符号，自动增长，主键，非空
      字符串类型 字段,指定 字符个数,指定字符集、排序规则，默认与数据库一致
      datetime 默认值 设置为now(), 或具体值，如'2000-1-1'
      ```

   3. 同样的方式创建学生表`students`

2. 编辑数据表

   1. 选择一张表: 工具栏第二行“打开表”、“设计表”、“删除表”变的可用
   2. 打开表: 查看表的当前数据, 窗口中增加、修改、删除数据
   3. 设计表: 和创建表的窗口一样，增加、修改、删除字段，或编辑字段的类型、约束
   4. 删除表: 表物理删除

##### 14.4.3 Navicat 数据操作

1. 查看数据
   1. 双击表，或 选择表, 点击工具栏第二行的“打开表”，查看表的数据
2. 增加数据
   1. 默认没有数据，列中填写数据，点击底部的`对勾`完成添加
      2. 注意：自动增长的`主键列`不用填写值
   3. 添加下一条数据, 点击询问的 `加号`, 添加一空白行，填写数据即可
3. 修改数据
   1. 点击某`单元格`，即可编辑值，修改后，点击底部的`勾`生效
4. 删除数据
   1. 点击某`单元格`，再点击询问的 `减号`，删除
   2. 说明：重要数据，推荐 `isdelete` 属性改为1，而不是 物理删除

#### 14.5 命令行操作(重点)

* 程序员 用`命令`, 程序员是客户端

1. 连接数据库 

   ```
   #打开终端，运行命令. root用户名
   mysql -uroot -p
   #回车后输入密码，如 密码为mysql
   ```

2. 查看效果

   ```
   select version();   #查看版本
   select now();   #显示当前时间
   ```

   

3. 退出登录

   `quit` / `exit` / `ctrl+d`

4. 修改输入提示符

   ```
   \D  #完整日期
   \U  #使用用户
   ```

##### 14.5.1数据库操作

1. 查看所有数据库

   ```
   show databases;
   ```

2. 使用数据库

   ```
   use 数据库名;
   ```

3. 查看当前使用的数据库

   ```
   select database();
   ```

4. 创建数据库

   ```
   create database 数据库名 charset=utf8;
   ```

   ```sql
   #示例
   create database python charset=utf8;
   ```

5. 删除数据库

   ```
   drop database 数据库名;
   ```

   ```
   #示例
   drop database python;
   ```

   1.  drop 主要用于删除结构
      1. 作用: 删除数据表或数据库，或删除数据表字段
      2. 删除数据库：drop database XX，
      3. 删除表 drop table XX。
      4. 字段也是结构的一种，也可以使用drop. 但改变表结构选用alter方法, `lter table student drop age`
   2. delete 主要用于删除数据
      1. `delete from student where name=‘张三’`  //删除 student表上名字为‘张三’的所有信息
   3. truncate：删除数据表中的数据（仅数据表中的数据，不删除表）
      1. `truncate table Student `  #truncate table 数据表名称
      2. 一种快速、无日志记录的方法。
      3. TRUNCATE TABLE语句与不含有 WHERE 子句的 DELETE 语句在功能上相同。
         1. 但是，TRUNCATE TABLE语句速度更快， 使用更少的系统资源和事务日志资源。”
      4. 作用:
         1. 清空表内的所有数据，没有条件
         2. 相当于将当前表**初始化**，恢复到刚创建的状态

##### 14.5.2 数据表操作

1. 查看当前数据库中所有表

   ```
   show tables;
   ```

2. 查看表结构

   ```
   desc 表名;
   ```

3. 创建表

   auto_increment  #字段值 自动增长

   ```
   #语法结构
   CREATE TABLE table_name(
       column1 datatype contrai,
       column2 datatype,
       column3 datatype,
       .....
       columnN datatype,
       PRIMARY KEY(one or more columns)  //主键字段,字段1,...,
   );
   ```

   ```
   #示例1, 创建 班级表
   create table classes(
       id int unsigned auto_increment primary key not null,
       name varchar(10)
   );
   ```

   ```
   #示例2, 创建 学生表
   create table students(
       id int unsigned primary key auto_increment not null,
       name varchar(20) default '',
       age tinyint unsigned default 0,
       height decimal(5,2),
       gender enum('男','女','人妖','保密'),
       cls_id int unsigned default 0
   )
   ```

4. 修改表-添加字段

   ```
   alter table 表名 add 列名 类型;
   例：
   alter table students add birthday datetime;
   ```

5. 修改表-修改字段：重命名 字段

   ```
   alter table 表名 change 原名 新名 类型及约束;
   例：
   alter table students change birthday birth datetime not null;
   ```

6. 修改表-修改字段：不重命名, 修改类型约束

   ```
   alter table 表名 modify 列名 类型及约束;
   例：
   alter table students modify birth date not null;
   ```

7. 修改表-删除字段

   ```
   alter table 表名 drop 列名;
   例：
   alter table students drop birthday;
   ```

8. 删除表

   ```
   drop table 表名;
   例：
   drop table students;
   ```

9. 查看表的创建语句

   ```
   show create table 表名;
   例：
   show create table classes;
   ```
   
10. 小结:

    1. 同一个数据库中的各个表是相互可见的.

##### 14.5.3 数据增删改查

* 增删改查(curd)

  curd: 创建( create ), 更新(update), 读取 (retrieve) 和删除 (delete) 

1. 查询

   1. 语法: 查询 所有列 

      ```
      select * from 表名;
      例：
      select * from classes;
      ```

   2. 查询 指定列

      as指定 列/表 别名

      ```
      select 列1,列2,... from 表名;
      例：
      select id,name from classes;
      ```

2. 增加

   1. 语法

      ```
      INSERT [INTO] tb_name [(col_name,...)] {VALUES | VALUE} ({expr | DEFAULT},...),(...),..
      ```

      1.  主键列 自动增长，全列插入时需要占位，用 `0` 或 `default` 或 ` null` 占位，插入成功 以实际数据为准

   2. `全列一行`插入, 值的顺序 与表中 字段的顺序 对应

      ```
      insert into 表名 values(...)
      例：
      insert into students values(0,’郭靖‘,1,'蒙古','2016-1-2');
      ```

      1.  0 是 主键列 值

   3. `全列多行`插入, 值的顺序 与 列顺序 对应

      ```
      insert into 表名(列1,...) values(值1,...)
      例：
      insert into students(name,hometown,birthday) values('黄蓉','桃花岛','2016-3-2');
      ```

      1. 向表中, 一次插入 一行 数据，一次性插入 多行 数据，减少与数据库的通信

      ```
      insert into 表名 values(...),(...)...;
      例：
      insert into classes values(0,'python1'),(0,'python2');
      ```

      ```
      insert into 表名(列1,...) values(值1,...),(值1,...)...;
      例：
      insert into students(name) values('杨康'),('杨过'),('小龙女');
      ```

   4. 部分列一行 插入：值顺序 与给出的 列顺序 对应

      ```
      insert into 表名(列1,...) values(值1,...)
      例：
      insert into students(name,hometown,birthday) values('黄蓉','桃花岛','2016-3-2');
      ```

      

3. 修改

   1. 语法

      ```
      UPDATE tbname SET col1={expr1|DEFAULT} [,col2={expr2|default}]...[where 条件判断]
      ```

   2. 示例

      ```
      update 表名 set 列1=值1,列2=值2... where 条件
      例：
      update students set gender=0,hometown='北京' where id=5;
      ```

4. 删除数据

   1. 语法: DELETE FROM tbname [where 条件判断]

      ```
      delete from 表名 where 条件
      例：
      delete from students where id=5;
      ```

5. 逻辑删除，本质就是修改操作

   1. 给students表添加一个 is_delete 字段 bit 类型  默认为0
      1. `alter table students add is_delete bit default 0;`
   2. isdelete=1  //逻辑删除
      1. `update students set isdelete=1 where id=1;`

   3. 逻辑上删除了, 物理上没有删除

##### 14.5.4 数据备份&恢复

1. mysql备份` 命令 `mysqldump`

   1. ```
mysqldump –uroot –p 数据库名 > python.sql;   //数据备份到 python.sql文件中
      ```
      
      1. root  用户名
2. `>`  定向
      3. 返回: 数据库名 的数据备份到 python.sql数据库
      4. 提示输入mysql的 密码
   
2. mysql恢复 流程

   1. 连接`mysql`软件，创建新的`数据库`, 退出连接

   2. 执行 ` 恢复` 语句

      `mysql -uroot –p 新数据库名 < python.sql`   //数据从python.sql中恢复到 `新数据库名`中

      

#### 14.6 数据库的设计

1. 数据库事务 4个特性 ACID
   
   1. 原子性(Atomicity), 一致性(consistency), 隔离性(Isolation), 持久性(Durability)
   
2. 数据库设计

   1. 关系型数据库: 以`E-R`模型为基础, 产品经理的`设计策划`，抽取 `模型与关系`，制定`表结构`. 这是项目的第一步
   2. 设计数据库的软件: 如power designer，db desinger.  软件 能直观的看到 实体间的关系
   3. 设计数据库的人: `数据库设计人员` 或 `开发组成员`,  由项目经理 带领 组员 完成
   4. 现阶段不 独立完成数据库设计，但要注意 积累经验

3. 数据库三范式

   1. 范式

      1. 设计数据库的规范，称为范式(Normal Form)
      2. 8种范式，遵守3范式即可

   2. 三范式

      1. 第一范式（1NF）: 列的原子性， 列不可再分

      2. 第二范式（2NF）:  属性完全依赖于主键. 即: 1NF基础上另加两个内容

         1. 一,  表 有一个主键
         2. 二, 非主键的列 完全依赖于主键, 不能依赖 主键的一部分。

      3. 第三范式（3NF）:  属性不依赖于其它非主属性,  直接依赖于主键.  即: 2NF基础上加 非主键列 直接依赖于主键

         1. 不能传递依赖
            1. 即不能：非主键列 A 依赖于非主键列 B，非主键列 B 依赖于主键

      4. 3范式 示例

         1. 不遵循1NF

            <table>
                <tr><td>contact</td><td rowspan=3></td><td>name</td><td>tel</td><td>addr</td></tr>
                <tr><td>张三, 10086, 山东</td> <td>张三</td><td>10086</td><td>山东</td></tr>
                <tr><td>错的</td> <td colspan=3>对的</td></tr>
            </table>

         2. 不遵循2NF

            <table>
                <tr><td colspan=7>orderDetail表</td></tr>
                <tr><td>OrderlD</td><td>ProductID</td><td>UnitPrice</td><td>Discount</td><td>Quantity</td><td>ProductName</td><td rowspan=2> 错的</td></tr>
                <tr><td>1001</td><td>7654</td><td>18.6</td><td>2.2</td><td>4</td><td>洗发水</td></tr>
            </table>

            <table>
                <tr><td colspan=5>orderDetail表</td></tr>
                <tr><td>OrderlD</td><td>UnitPrice</td><td>Discount</td><td>Quantity</td><td rowspan=2> 对的</td></tr>
               <tr><td>1001</td><td>7654</td><td>2</td><td>4</td></tr>
            </table>

            ​		

            <table>
                <tr><td colspan=4>Product表</td></tr>
                <tr><td>ProductID</td><td>UnitPrice</td><td>ProductName</td><td rowspan=2> 对的</td></tr>
               <tr><td>7654</td><td>18.6</td><td>洗发水</td></tr>
            </table>

         3. 不遵循3NF

            <table>
                <tr><td colspan=7>Order表</td></tr>
            <tr><td>OrderlD</td><td>OrderDate</td><td>CustomerID</td><td>CustomerName</td><td>CustomerAddr</td><td>CustomerCity</td><td rowspan=2>错的</td></tr>
            <tr><td>1001</td><td>2017-01-01 12:12:12PM</td><td>12352</td><td>老王</td><td>xx市x县xx镇x村</td><td>山东</td></tr>    
            </table>

            ​	

            <table>
                <tr><td colspan=4>order表</td></tr>
                <tr><td>OrderlD</td><td>OrderDate</td><td>CustomerID</td><td rowspan=2>对的</td></tr>
                <tr><td>1001</td><td>2017-01-01 12:12:12PM</td><td>12352</td></tr>
            </table>

            ​	

            <table>
                <tr><td colspan=5>Customer表</td></tr>
                <tr><td>CustomerID</td><td>CustomerName</td><td>CustomerAddr</td><td>CustomerCity</td><td rowspan=2>对的</td></tr>
                <tr><td>12352</td><td>老王</td><td>xx市x县xx镇x村</td><td>山东</td></tr>
            </table>

         4. 最终表

            <table>
                <tr><td colspan=3>order表</td></tr>
                <tr><td>OrderlD</td><td>OrderDate</td><td>CustomerID</td></tr>
                <tr><td>1001</td><td>2017-01-01 12:12:12PM</td><td>12352</td></tr>
            </table>

            <table>
                <tr><td colspan=4>Customer表</td></tr>
                <tr><td>CustomerID</td><td>CustomerName</td><td>CustomerAddr</td><td>CustomerCity</td></tr>
                <tr><td>12352</td><td>老王</td><td>xx市x县xx镇x村</td><td>山东</td></tr>
            </table>

            <table>
                <tr><td colspan=4>orderDetail表</td></tr>
                <tr><td>OrderlD</td><td>UnitPrice</td><td>Discount</td><td>Quantity</td></tr>
               <tr><td>1001</td><td>7654</td><td>2</td><td>4</td></tr>
            </table>

            <table>
                <tr><td colspan=3>Product表</td></tr>
                <tr><td>ProductID</td><td>UnitPrice</td><td>ProductName</td></tr>
               <tr><td>7654</td><td>18.6</td><td>洗发水</td></tr>
            </table>

4. E-R模型

   1. `E`  entry，实体

      1. 设计实体 像定义一个类一样，指定 方面 描述对象
      2. 一个实体 对应数据库中的 一个表

   2. `R`  relationship, 关系

      1. 关系, 两个实体之间的对应规则
      2. 关系的类型有 `一对一`、`一对多`、`多对多`

   3. 关系 是一种数据，用一个 `字段` 存储在表中

   4. 实体A对实体B为`1对1`，则在表A或表B中创建一个 `字段`，存储另一个表的 `主键值`

      ```
      ┌─────────────────────────────────────────────────────┐
      │┌───────────────────────────────────────────────────┐│
      ││  当一个表的列太多时，且某些列不经常出现在结果中，此时可以对列 ││
      ││  中的列进行拆分，分到两个表中，此时，两个表为一对-的关系     ││
      │└───────────────────────────────────────────────────┘│
      │                                                     │
      │┌────────────────────┐        ┌────────────────────┐ │
      ││                  1 │        │1                   │ │
      ││┌──────────────────┐│--------│┌──────────────────┐│ │
      │││  学生基本信息      ││        ││  学生基本信息      ││  │
      │││  如学号、姓名      ││        ││  如学号、姓名      ││  │
      ││└──────────────────┘│        │└──────────────────┘│ │
      │└────────────────────┘        └────────────────────┘ │
      └─────────────────────────────────────────────────────┘
      ```

   5. 实体A对实体B为1对多：在表B中创建一个字段，存储表A的主键值
   6. 实体A对实体B为多对多：新建一张表C，这个表只有两个字段，一个用于存储A的主键值，一个用于存储B的主键值

5. 逻辑删除

   1. 重要数据，不希望物理删除
   2. 删除方案：设置`isDelete`列，类型为bit，逻辑删除，默认值`0`
   3. 非重要数据，可物理删除



--------

### 15 MySQL - 查询

#### 15.1 MySQL查询

1. 创建数据库、数据表

   ```
   -- 创建数据库
   create database python_test_1 charset=utf8;
   
   -- 使用数据库
   use python_test_1;
   
   -- students表
   create table students(
       id int unsigned primary key auto_increment not null,
       name varchar(20) default '',
       age tinyint unsigned default 0,   //默认值0, default 0
       height decimal(5,2),
       gender enum('男','女','中性','保密') default '保密',  //默认值'保密', default '保密'
       cls_id int unsigned default 0,    //默认值0, default 0
       is_delete bit default 0    //默认值0, default 0
   );
   
   -- classes表
   create table classes (
       id int unsigned auto_increment primary key not null,
       name varchar(30) not null
   );
   ```

2. 准备数据

   ```
   -- 向students表中插入数据
   insert into students values
   (0,'小明',18,180.00,2,1,0),
   (0,'小月月',18,180.00,2,2,1),
   (0,'彭于晏',29,185.00,1,1,0),
   (0,'刘德华',59,175.00,1,2,1),
   (0,'黄蓉',38,160.00,2,1,0),
   (0,'凤姐',28,150.00,4,2,1),
   (0,'王祖贤',18,172.00,2,1,1),
   (0,'周杰伦',36,NULL,1,1,0),
   (0,'程坤',27,181.00,1,2,0),
   (0,'刘亦菲',25,166.00,2,2,0),
   (0,'金星',33,162.00,3,3,1),
   (0,'静香',12,180.00,2,4,0),
   (0,'郭靖',12,170.00,1,4,0),
   (0,'周杰',34,176.00,2,5,0);
   
   -- 向classes表中插入数据
   insert into classes values (0, "python_01期"), (0, "python_02期");
   ```

3. 查询所有字段

   ```
   select * from 表名;
   例：
   select * from students;
   ```

4. 查询指定字段

   ```
   select 列1,列2,... from 表名;
   例:
   select name from students;
   ```

5. 用 `as` 给字段起别名

   ```
   select id as 序号, name as 名字, gender as 性别 from students;
   ```

6. 用 ` as` 给表起别名

   ```
   -- 如果是单表查询 可以省略表明
   select id, name, gender from students;
   
   -- 表名.字段名
   select students.id,students.name,students.gender from students;
   
   -- 可以通过 as 给表起别名 
   select s.id,s.name,s.gender from students as s;
   ```

7. 消除重复行

   select后面列前用 `distinct` 可消除重复的行

   ```
   select distinct 列1,... from 表名;
   例：
   select distinct gender from students;  //查找gender, 并取消重复的值
   ```



#### 15.2 条件

1. where子句, 表中 数据筛选，结果 true的行 出现在结果集

   ```
   select * from 表名 where 条件;
   例：
   select * from students where id=1;
   ```

   1. where后面支持多种运算符
      1. 比较运算符,
      2. 逻辑运算符
      3. 模糊查询
      4. 范围查询
      5. 空判断
   
2. 比较运算符

   1. 比较符

      1. 等于: =
      2. 大于: >
      3. 大于等于: >=
      4. 小于: <
      5. 小于等于: <=
      6. 不等于: != 或 <>

   2. 示例

      1.  查询编号大于3的学生

         `select * from students where id > 3;`

      2. 查询编号不大于4的学生

         `select * from students where id <= 4;`

      3. 查询姓名不是“黄蓉”的学生

         `select * from students where name != '黄蓉';`

      4. 查询没被删除的学生

         `select * from students where is_delete=0;`

3. 逻辑运算符

   1. 逻辑符

      1. and
      2. or
      3. not

   2. 示例

      1. 查询编号大于3的女同学

         `select * from students where id > 3 and gender=0;`

      2. 查询编号小于4或没被删除的学生

         `select * from students where id < 4 or is_delete=0;`

4. 模糊查询

   1. 模糊关键字

      1. `like`
      2. `% `   任意  多个字符
      3. `_`   任意  一个字符

   2. 示例

      1. 查询姓黄的学生

         `select * from students where name like '黄%';`

      2. 查询姓黄并且“名”是一个字的学生

         `select * from students where name like '黄_';`

      3. 查询姓黄或叫靖的学生

         `select * from students where name like '黄%' or name like '%靖';`

5. 范围查询

   1. 范围符

      1.  ` in`  一个`非连续`的范围内,  数值或字符串值
      2. between ... and ... 一个`连续`的范围内

   2. 示例

      1. 查询编号是1或3或8的学生

         `select * from students where id in(1,3,8);`

      2. 查询编号: 3至8 的学生

         `select * from students where id between 3 and 8;`

      3. 查询编号是3至8的男生

         `select * from students where (id between 3 and 8) and gender=1;`

6. 空判断

   1. `is null`,   `is not null`

      1. 判空  `is null`
      2. 判非空  `is not null`
      3. `null` 与 `''`  不同

   2. 示例

      1. 查询没有填写身高的学生

         `select * from students where height is null;`

      2. 查询填写了身高的学生

         `select * from students where height is not null;`

      3. 查询填写了身高的男生

         `select * from students where height is not null and gender=1;`

7. 优先级

   1. 优先级由高到低：小括号，not，比较运算符，逻辑运算符
   2. and比or高.  希望先算or，用`()`

#### 15.3 排序

1. 语法

   `select * from 表名 order by 列1 asc|desc [,列2 asc|desc,...]`

2. 说明

   1. 行数据按`列1`排序, 如 某些行列1的值相同, 则按 列2排序, 以此类推
   2. 默认 列值升序（asc）
   3. asc 升序
   4. desc 降序

3. 示例

   1. 查询未删除男生信息，按学号降序

      `select * from students where gender=1 and is_delete=0 order by id desc;`

   2. 查询未删除学生信息，按名称升序

      `select * from students where is_delete=0 order by name;`

   3. 显示所有的学生信息,双重排序, 先年龄从大-->小,再身高从高-->矮

      `select * from students  order by age desc,height desc;`



#### 15.4 聚合函数

* 聚合函数:  统计数据, 5个聚合函数 

1. 总数

   1.  `count(*)`, 总`行数`，`*`与`列名` 结果相同

   2. 查询学生总数

      `select count(*) from students;`

2. 最大值

   1. `max(列字段名)`   此列的最大值

   2. 查询女生的编号最大值

      `select max(id) from students where gender=2;`

3. 最小值

   1. `min(列字段名)` 此列的最小值

   2. 查询未删除的学生最小编号

      `select min(id) from students where is_delete=0;`

4. 求和

   1. `sum(列字段名)` 此列的和

   2. 查询男生的总年龄

      ```
      select sum(age) from students where gender=1;
      ```

      ```
      -- 平均年龄
      select sum(age)/count(*) from students where gender=1;
      ```

5. 平均值

   1. `avg(列字段名)` 此列的平均值

   2. 查询未删除女生的编号平均值

      `select avg(id) from students where is_delete=0 and gender=2;`



#### 15.5 分组

1. `group by`

   1. 查询结果按 `1个`/`多个` 字段分组，值相同的为一组
   2. `单个/多个` 字段分组

2. `group by`示例

   1. `select * from students;`

      ```
      +----+-----------+------+--------+--------+--------+-----------+
      | id | name      | age  | height | gender | cls_id | is_delete |
      +----+-----------+------+--------+--------+--------+-----------+
      |  1 | 小明      |   18 | 180.00 | 女     |      1 |           |
      |  2 | 小月月    |   18 | 180.00 | 女     |      2 |          |
      |  3 | 彭于晏    |   29 | 185.00 | 男     |      1 |           |
      |  4 | 刘德华    |   59 | 175.00 | 男     |      2 |          |
      |  5 | 黄蓉      |   38 | 160.00 | 女     |      1 |           |
      |  6 | 凤姐      |   28 | 150.00 | 保密   |      2 |          |
      |  7 | 王祖贤    |   18 | 172.00 | 女     |      1 |          |
      |  8 | 周杰伦    |   36 |   NULL | 男     |      1 |           |
      |  9 | 程坤      |   27 | 181.00 | 男     |      2 |           |
      | 10 | 刘亦菲    |   25 | 166.00 | 女     |      2 |           |
      | 11 | 金星      |   33 | 162.00 | 中性   |      3 |          |
      | 12 | 静香      |   12 | 180.00 | 女     |      4 |           |
      | 13 | 周杰      |   34 | 176.00 | 女     |      5 |           |
      | 14 | 郭靖      |   12 | 170.00 | 男     |      4 |           |
      +----+-----------+------+--------+--------+--------+-----------+
      ```

   2. `select gender from students group by gender;`

      ```
      +--------+
      | gender |
      +--------+
      | 男     |
      | 女     |
      | 中性   |
      | 保密   |
      +--------+
      ```

      1. gender字段 值有4个'男','女','中性','保密'， 分 4组
      2. group by单独 用, 只显示 每组的第一条记录. group by单用 意义不大

3. `聚合函数...group by`

   1. 聚合函数对各个分组内的字段进行统计

   2. `group_concat()...group by`

      1. `group_concat(字段名)`  , 合并函数，分组的 `某字段的各个值`合并
         1.  作 一个字段 用

   3. 示例

      `select gender,group_concat(age) from students group by gender;`  //分组内age的各个值合并.

      ```
      +--------+----------------------+
      | gender | group_concat(age)    |
      +--------+----------------------+
      | 男     | 29,59,36,27,12       |
      | 女     | 18,18,38,18,25,12,34 |
      | 中性   | 33                   |
      | 保密   | 28                   |
      +--------+----------------------+
      ```

      分别统计性别为男/女的人年龄平均值

      `select gender,avg(age) from students group by gender;`

      ```
      +--------+----------+
      | gender | avg(age) |
      +--------+----------+
      | 男     |  32.6000 |
      | 女     |  23.2857 |
      | 中性   |  33.0000 |
      | 保密   |  28.0000 |
      +--------+----------+
      ```

      分别统计性别为男/女的人的个数

      `select gender,count(*) from students group by gender;`

      ```
      +--------+----------+
      | gender | count(*) |
      +--------+----------+
      | 男     |        5 |
      | 女     |        7 |
      | 中性   |        1 |
      | 保密   |        1 |
      +--------+----------+
      ```

5. `group by... having`

   1. `having`

      1. having 条件,  分组后指定 条件 查询结果
      2. having 和where一样，但having只用于`group by`

   2. 示例

      `select gender,count(*) from students group by gender having count(*)>2;`

      ```
      +--------+----------+
      | gender | count(*) |
      +--------+----------+
      | 男     |        5 |
      | 女     |        7 |
      +--------+----------+
      ```

6. `聚合函数...group by...with rollup`

   1. `with rollup`  新增一行，对各组结果再执行一次聚合操作

   2. 示例

      `select gender,count(*) from students group by gender with rollup;`

      ```
      +--------+----------+
      | gender | count(*) |
      +--------+----------+
      | 男     |        5 |
      | 女     |        7 |
      | 中性   |        1 |
      | 保密   |        1 |
      | NULL   |       14 |
      +--------+----------+
      ```

      `select gender,group_concat(age) from students group by gender with rollup;`

      ```
      +--------+-------------------------------------------+
      | gender | group_concat(age)                         |
      +--------+-------------------------------------------+
      | 男     | 29,59,36,27,12                            |
      | 女     | 18,18,38,18,25,12,34                      |
      | 中性   | 33                                        |
      | 保密   | 28                                        |
      | NULL   | 29,59,36,27,12,18,18,38,18,25,12,34,33,28 |
      +--------+-------------------------------------------+
      ```

#### 15.6 分页

1. 获取部分行

   1. 情景: 数据量 大,  查看 麻烦
   2. `limit`

2. 语法

   1. `select * from 表名 limit start,count;`

   2. 说明:  `start` 开始，获取 `count` 条数据

      查询前3行男生信息

      `select * from students where gender=1 limit 0,3;`

3. 示例：分页

   1. 场景

      1. 已知：每页 m条数据，当前第n页

      2. 求总页数：此段逻辑 在python中实现

         - 查询总条数p1
         - 使用p1除以m得到p2
         - 如果整除则p2为总数页
         - 如果不整除则p2+1为总页数

      3. 求第n页的数据

         `select * from students where is_delete=0 limit (n-1)*m,m`



#### 15.7 连接查询

1. 连接查询

   1. 两个/多个 表连接
   2. 结果的列来源于多张表, 将多 `表` 连接成一个大的数据`集`，再选择 `列`返回

2. mysql 三种 连接查询

   1. 内连接查询, 结果为两个表匹配的数据.  $A \bigcap B$,  A 左表, B 右表
   2. 右连接查询, 结果为两个表匹配到的数据，右表B 特有的数据， 左表A 不存在的数据 用null填充
      1. A 左表, B 右表
      2. $A \bigcap B\bigcup B$ ,  A中不存在的 用null填充
   3. 左连接查询, 结果为两个表匹配到的数据，左表A 特有的数据，右表B 不存在的数据使用null填充
      1. A 左表, B 右表
      2. $A\bigcup A \bigcap B$ ,  B中不存在的 用null填充

3. 语法

   ...inner/left/right join...on...

   `select * from 表1 inner/left/right join 表2 on 表1.列 = 表2.列`

4. 示例

   1. `inner join`内连接查询班级表与学生表 

      `select * from students inner join classes on students.cls_id = classes.id;`

   2. `left join`左连接查询班级表与学生表

      as为表起别名，目的是编写简单

      `select * from students as s left join classes as c on s.cls_id = c.id;`

   3. `right join`右连接查询班级表与学生表

      `select s.name,c.name from students as s right join classes as c on s.cls_id = c.id;`

#### 15.8 自关联

1. 背景

   1. 设计2个表

      1. provinces表结构
         - id
         - ptitle
      2. citys表结构
         - id
         - ctitle
         - proid  #proid 表示城市的省，对应 provinces表的id值

   2. 问题: 两个表合成一张表

      观察: citys表比provinces表多一个列proid，其它列的类型 一样

   3. 意义:
      1. 存储的是`地区信息`, 每种信息的数据量 有限，没必要增加一个新表
      2. 或者将来还要存储`区、乡镇信息`，都增加`新表`的开销太大

2. 答案(解决方法/方案)

   1. 定义表areas，结构
      * id
      * atitle
      * pid
   2. 说明
      1. 省没有所属的省份，填写`null`
      2. 市所属的省份pid，填写省 对应的编号 id
      3. 自关联: 表中的`某一列`，关联了表自己的`另一列`，但 业务逻辑含义 不一样, 城市的pid引用省的id
      4. 表 结构不变，可添加区 县、乡镇街道、村社区等信息

3. 方法 示例

   1. 创建`areas表`

      ```
      create table areas(
          aid int primary key,
          atitle varchar(20),
          pid int
      );
      ```

   2. 从 `sql文件` 中导入数据

      ```
      source areas.sql;
      ```

   3. 查询 共有多少个省

      `select count(*) from areas where pid is null;`

   4. 查询省的名称为“山西省”的所有城市

      ```
      select city.* from areas as city
      inner join areas as province on city.pid=province.aid
      where province.atitle='山西省';
      ```

      1. `city.*`   //city表的*

   5. 查询市的名称为“广州市”的所有区县

      ```
      select dis.* from areas as dis
      inner join areas as city on city.aid=dis.pid
      where city.atitle='广州市';
      ```

      1. `dis.*`  // dis表的*

#### 15.9 子查询

1. 子查询

   1. 一个 select 语句中,嵌入了另外一个 select 语句
   2. 被嵌入的 `select 语句` 称为`子查询语句`

2. 主查询

   1. 要查询的对象 在的语句的第一条 `select 语句`

3. 主查询和子查询的关系

   1. 子 嵌入 主 
   2. 子 辅助 主 , 当 条件,或当 数据源
   3. 子查询 可 独立存在的语句,是 一条完整的 select 语句

4. 子查询分类

   1. 标量子查询, 子查询返回 `一个数据`(一行一列) 
   2. 列子查询, 返回 一列(一列多行)
   3. 行子查询, 返回 一行(一行多列)

5. 标量子查询

   1. 查询班级学生的平均身高
      1. 查询班级学生平均年龄
      2. 查询大于平均年龄的学生
   2. `select * from students where age > (select avg(age) from students);`

6. 列级子查询

   1. 查询还有学生在班的所有班级名字
      1. 找出学生表中所有的班级 id
      2. 找出班级表中对应的名字
   2. `select name from classes where id in (select cls_id from students);`

   

7. 行级子查询

   1. 查找班级年龄最大,身高最高的学生
      1. 行元素: 将多个字段合成一个行元素
      2. 行级子查询中会使用到行元素
   2. `select * from students where (height,age) = (select max(height),max(age) from students);`
      1. 一次主句执行对应多次从句执行

8. 子查询中特定关键字

   1. in 范围
   2. 格式: `主查询 where 条件 in (列子查询)`



#### 15.10 总结

1. 查询的完整格式 

   ```
   // []是可选的
   SELECT select_expr [,select_expr,...] [      
         FROM tb_name
         [WHERE 条件判断]
         [GROUP BY {col_name | postion} [ASC | DESC], ...] 
         [HAVING WHERE 条件判断]
         [ORDER BY {col_name|expr|postion} [ASC | DESC], ...]
         [ LIMIT {[offset,]rowcount | row_count OFFSET offset}]
   ]
   ```

   

2. 完整的select语句

   ```
   select distinct *
   from 表名
   where ....
   group by ... having ...
   order by ...
   limit start,count
   ```

3. 执行顺序

   ```
   1. from 表名
   2. where ....
   3. group by ...
   4. select distinct *
   5. having ...
   6. order by ...
   7. limit start_n,count_n
   ```

4. 实际中，只用 语句中 部分关键字的组合， 不是全部关键字的组合

---------

### 16. MySQL与python交换

#### 16.1 准备数据

1. 创建数据表

   1. 创建 "京东" 数据库

      `create database jing_dong charset=utf8;`

   2. 使用 "京东" 数据库

      `use jing_dong;`

   3. 创建 一个goods 数据表, 一个商品一行记录(数据)

      ```
      create table goods(
          id int unsigned primary key auto_increment not null,
          name varchar(150) not null,
          cate_name varchar(40) not null,
          brand_name varchar(40) not null,
          price decimal(10,3) not null default 0,
          is_show bit not null default 1,
          is_saleoff bit not null default 0
      );
      ```

2. 插入数据

   1. 向goods表中插入数据

      ```
      insert into goods values(0,'r510vc 15.6英寸笔记本','笔记本','华硕','3399',default,default); 
      insert into goods values(0,'y400n 14.0英寸笔记本电脑','笔记本','联想','4999',default,default);
      insert into goods values(0,'g150th 15.6英寸游戏本','游戏本','雷神','8499',default,default); 
      insert into goods values(0,'x550cc 15.6英寸笔记本','笔记本','华硕','2799',default,default); 
      insert into goods values(0,'x240 超极本','超级本','联想','4880',default,default); 
      insert into goods values(0,'u330p 13.3英寸超极本','超级本','联想','4299',default,default); 
      insert into goods values(0,'svp13226scb 触控超极本','超级本','索尼','7999',default,default); 
      insert into goods values(0,'ipad mini 7.9英寸平板电脑','平板电脑','苹果','1998',default,default);
      insert into goods values(0,'ipad air 9.7英寸平板电脑','平板电脑','苹果','3388',default,default); 
      insert into goods values(0,'ipad mini 配备 retina 显示屏','平板电脑','苹果','2788',default,default); 
      insert into goods values(0,'ideacentre c340 20英寸一体电脑 ','台式机','联想','3499',default,default); 
      insert into goods values(0,'vostro 3800-r1206 台式电脑','台式机','戴尔','2899',default,default); 
      insert into goods values(0,'imac me086ch/a 21.5英寸一体电脑','台式机','苹果','9188',default,default); 
      insert into goods values(0,'at7-7414lp 台式电脑 linux ）','台式机','宏碁','3699',default,default); 
      insert into goods values(0,'z220sff f4f06pa工作站','服务器/工作站','惠普','4288',default,default); 
      insert into goods values(0,'poweredge ii服务器','服务器/工作站','戴尔','5388',default,default); 
      insert into goods values(0,'mac pro专业级台式电脑','服务器/工作站','苹果','28888',default,default); 
      insert into goods values(0,'hmz-t3w 头戴显示设备','笔记本配件','索尼','6999',default,default); 
      insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default); 
      insert into goods values(0,'x3250 m4机架式服务器','服务器/工作站','ibm','6888',default,default); 
      insert into goods values(0,'商务双肩背包','笔记本配件','索尼','99',default,default);
      ```
      
      1. id 输入是0, 但会自动增加1
      2. default 可作做出现在 `insert into...values(...default,...);`中, 且没有 引号

#### 16.2 SQL 练习

1. SQL语句 强化

   1. 查询 类型cate_name为 '超极本' 的商品名称、价格

      `select name,price from goods where cate_name = '超级本';`

      1. 字符串类型的值 要加引号, 单引号或双引号都可以

   2. 显示商品的种类

      `select cate_name from goods group by cate_name;`

   3. 求所有电脑产品的平均价格,并且保留两位小数

      `select round(avg(price),2) as avg_price from goods;`

   4. 每种商品的平均价格

      `select cate_name,avg(price) from goods group by cate_name;`

   5. 每种类型的商品中 最贵、最便宜、平均价、数量

      `select cate_name,max(price),min(price),avg(price),count(*) from goods group by cate_name;`

   6. 所有价格大于平均价格的商品，并且按价格降序排序

      ```
      select id,name,price 
      from goods 
      where price >
      	(
      	select round(avg(price),2) as avg_price
          from goods
          ) 
      order by price desc;
      ```

      1. round()  //四舍五入函数
      2.  `> ` 是两个值比较, 不是一个值与一个范围内多个值比较

   7. 每种类型中最贵的电脑信息

      ```
      select * from goods
      inner join 
          (
              select
              cate_name, 
              max(price) as max_price, 
              min(price) as min_price, 
              avg(price) as avg_price, 
              count(*) from goods group by cate_name
          ) as goods_new_info 
      on goods.cate_name=goods_new_info.cate_name and goods.price=goods_new_info.max_price;
      ```

2. 创建 "商品分类"" 表

   1. 创建商品分类表

      ```
      create table if not exists goods_cates(
          id int unsigned primary key auto_increment,
          name varchar(40) not null
      );
      ```

      1. `...if no exists...`
      2. unsigned
      3. primary key
      4. auto_increment

   2. goods表中商品的种类

      `select cate_name from goods group by cate_name;`

   3. 分组结果写入到goods_cates数据表

      `insert into goods_cates (name) select cate_name from goods group by cate_name;`
      
      1. 子句查询结果做 `insert into...values()`中的values的参数时, 不能用关键字values(), 否则报错.
      2. 子句加`()`可以, 不加也可以
         1. `insert into goods_cates (name) (select cate_name from goods group by cate_name); `
         2. 该语句也是对的

3. 同步表数据

   1. 用goods_cates数据表 更新goods表

      `update goods as g inner join goods_cates as c on g.cate_name=c.name set g.cate_name=c.id;`

4. 创建 "商品品牌表" 表

   1. 用`create...select`  创建数据表, 同时写入记录,一步到位

      注意: brand_name 用as起别名，否则name字段 没有值

      ```
      create table goods_brands (
          id int unsigned primary key auto_increment,
          name varchar(40) not null) 
          select brand_name as name
          from goods 
          group by brand_name;
      ```

      

5. 同步数据

   1. 用`goods_brands`数据表 更新goods数据表

      ```
      update goods as g 
      inner join goods_brands as b 
      on g.brand_name=b.name 
      set g.brand_name=b.id;
      ```

6. 修改表结构

   1. 查看 goods 的数据表结构

      `desc goods;`

      1. 发现 cate_name 和 brand_name对应的类型为 `varchar` 但是存储的都是数字

   2. `alter table`语句修改表结构

      ```
      alter table goods  
      change cate_name cate_id int unsigned not null,
      change brand_name brand_id int unsigned not null;
      ```

      1. 一个 alter, 2个 change, 2个字段修改名字 类型 约束.
      2. 一次修改多个结构

7. 插入数据

   1. goods_cates 和 goods_brands表中插入记录

      ```
      insert into goods_cates(name) values ('路由器'),('交换机'),('网卡');
      insert into goods_brands(name) values ('海尔'),('清华同方'),('神舟');
      ```

      1. 一个字段一次插入多个值

   2. goods 表中写入任意记录
   
   `insert into goods (name, cate_id, brand_id, price)
      values ('LaserJet Pro P1606dn 黑白激光打印机', 12, 4,'1849');`

      1. 一次操作: 多个字段一次插入一个值
   
8. 表联合查询

   1. 三表联合查询 所有商品 信息 (通过内连接)

      ```
      select g.id,g.name,c.name,b.name,g.price from goods as g
      inner join goods_cates as c on g.cate_id=c.id
      inner join goods_brands as b on g.brand_id=b.id;
      ```

      1.  1个select, 2个 inner join...on..., 3个表 内连接

   2. 查询所有商品的详细信息 (通过左连接)

      ```
      select g.id,g.name,c.name,b.name,g.price from goods as g
      left join goods_cates as c on g.cate_id=c.id
      left join goods_brands as b on g.brand_id=b.id;
      ```

      1.  1个select, 2个 left join...on..., 3个表 左连接

9. 外键

   1. 外键作用: 防止无效信息的插入, 或 插入前判断类型或者品牌名称是否存在 

   2. 外键(约束)

      1. 数据的有效性 验证
      2. 关键字: `foreign key`,  `references`
      3. `innodb数据库引擎` 支持外键约束, 只有, 其他不行

   3. 已存在的 数据表 创建 `外键`约束

      1. 给字段`brand_id` 添加外键约束

         `alter table goods add foreign key (brand_id) references goods_brands(id);`   //对字段brand_id定义外键 id

         1. 字段`brand_id` 是 `goods 表` 中的字段

      2. 给字段`cate_id` 添加外键

         `alter table goods add foreign key (cate_id) references goods_cates(id);`  

         1. 字段`cate_id` 是 `goods 表` 中的字段
         2. 失败, 出现`1452`错误
         3. 错误原因: 已添加了一个不存在的cate_id值12, 要先删除

      3. 创建数据表时 设置外键约束

         goods 中的 cate_id 的类型一定要和 goods_cates 表中的 id 类型一致

         ```
         create table goods(
             id int primary key auto_increment not null,
             name varchar(40) default '',
             price decimal(5,2),
             cate_id int unsigned,
             brand_id int unsigned,
             is_show bit default 1,
             is_saleoff bit default 0,
             foreign key(cate_id) references goods_cates(id),
             foreign key(brand_id) references goods_brands(id)
         );
         ```

   4. 取消外键约束

      1. 先, 查看表创建语句 获取 外键约束名称, 该名称系统自动生成

         `show create table goods;`

         获得 外键名称

      2. 再, 根据名称 删除外键约

         `alter table goods drop foreign key 外键名称;`

   5. 实际开发, 很少 用 外键约束.  外键 降低表更新的效率

#### 16.3 数据库设计

1. 创建 "商品分类" 表

   ```
   create table goods_cates(
       id int unsigned primary key auto_increment not null,
       name varchar(40) not null
   );
   ```

2. 创建 "商品品牌" 表

   ```
   create table goods_brands (
       id int unsigned primary key auto_increment not null,
       name varchar(40) not null
   );
   ```

3. 创建 "商品" 表

   ```
   create table goods(
       id int unsigned primary key auto_increment not null,
       name varchar(40) default '',
       price decimal(5,2),
       cate_id int unsigned,
       brand_id int unsigned,
       is_show bit default 1,
       is_saleoff bit default 0,
       foreign key(cate_id) references goods_cates(id),
       foreign key(brand_id) references goods_brands(id)
   );
   ```

   1. goods表的字段cate_id, brand_id 有外键约束在表goods_cates,goods_brands
   2. 有外键约束关系, 表goods_cates,goods_brands 必须先与表goods创建

4. 创建 "顾客" 表

   ```
   create table customer(
       id int unsigned auto_increment primary key not null,
       name varchar(30) not null,
       addr varchar(100),
       tel varchar(11) not null
   );
   ```

5. 创建 "订单" 表

   ```
   create table orders(
       id int unsigned auto_increment primary key not null,
       order_date_time datetime not null,
       customer_id int unsigned,
       foreign key(customer_id) references customer(id)
   );
   ```

6. 创建 "订单详情" 表

   ```
   create table order_detail(
       id int unsigned auto_increment primary key not null,
       order_id int unsigned not null,
       goods_id int unsigned not null,
       quantity tinyint unsigned not null,
       foreign key(order_id) references orders(id),
       foreign key(goods_id) references goods(id)
   );
   ```

7. 外键约束 说明
   1. 创建表有的顺序要求.
      1. goods表的外键约束在表goods_cates或goods_brands中
      2. 要先创建这2个表,后创建表goods
   2. 创建外键时,类型要相同,否则失败

#### 16.4 Python操作MySQL步骤

 1. 用 python DB API 访问数据库 流程

    导包 --> 创建connection实例 --> 创建cursor实例 --> 执行查询/执行命令/获取数据/处理数据 --> 关闭cursor --> 关闭connection 结束

2. 使用方法

   1. 引入模块

      py文件中引入`pymysql`模块

      `from pymysql import *`

   2. Connection 对象

      `connect()`方法创建对象, 用于建立与数据库的 连接, 

      `conn=connect(host, port, user, password, database, charset)`

      1. host：数据库所在地址(主机)，如果本机是'localhost', 
      2. port：连接的`mysql`主机的`端口`，默认 3306
      3. database：要操作的数据库名, 可以稍后指定，非必须
      4. user：数据库的用户名
      5. password：数据库的密码
      6. charset：通信采用的编码方式，推荐使用utf8

   3. Connection对象的方法

      1. close()  关闭连接
      2. commit()  提交
      3. cursor()  返回Cursor对象, 执行sql语句并获得结果

   4. Cursor对象

      1. 用于 执行sql语句, 使用频度高的语句为select、insert、update、delete
      2. 获取Cursor对象：调用Connection对象的cursor()方法

      `cs1=conn.cursor()`

   5. Cursor对象的方法

      1. close()  #关闭
      2. execute(operation [, parameters ])执行语句
         1. 返回受影响的行数
         2. 执行 insert、update、delete、create、alter、drop等语句
      3. fetchone() 执行查询语句
         1. 获取查询结果集的 一个行 数据，返回一个元组
         2. 像 生成器的 next() 函数一样, 输出下一行数据记录
      4. fetchall()执行查询
         1. 获取结果集的 所有行
      2. 一行 构成一个 元组，再将这些 元组 装入一个元组返回
   
6. cursor对象的属性
   
      1. rowcount   //只读属性, 最近一次execute()执行 受影响的行数
      2. connection  //获得当前连接对象

#### 16.5 增删改查

1. 增删改查

   ```
   from pymysql import *
   
   def main():
       # 创建Connection连接
       conn = connect(host='localhost',port=3306,database='jing_dong',user='root',password='mysql',charset='utf8')
       # 获得Cursor对象
       cs1 = conn.cursor()
       # 执行insert语句，并返回受影响的行数：添加一条数据
       # 增加
       count = cs1.execute('insert into goods_cates(name) values("硬盘")')
       #打印受影响的行数
       print(count)
   
       count = cs1.execute('insert into goods_cates(name) values("光盘")')
       print(count)
   
       # # 更新
       # count = cs1.execute('update goods_cates set name="机械硬盘" where name="硬盘"')
       # # 删除
       # count = cs1.execute('delete from goods_cates where id=6')
   
       # 提交之前的操作，如果之前已经之执行过多次的execute，那么就都进行提交
       conn.commit()
   
       # 关闭Cursor对象
       cs1.close()
       # 关闭Connection对象
       conn.close()
   
   if __name__ == '__main__':
       main()
   ```

   1. connection() 函数连接的是一个库, 不是一个表, 
   2. 创建的cursor对象 可以操作库内的任何一张表
   3. cs1.execute() 返回受影响的行数

2. 查询一行数据  fetchone()

   ```
   from pymysql import *
   
   def main():
       # 创建Connection连接
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='jing_dong',charset='utf8')
       # 获得Cursor对象
       cs1 = conn.cursor()
       # 执行select语句，并返回受影响的行数：查询一条数据
       count = cs1.execute('select id,name from goods where id>=4')
       # 打印受影响的行数
       print("查询到%d条数据:" % count)
   
       for i in range(count):
           # 获取查询的结果
           result = cs1.fetchone()
           # 打印查询的结果
           print(result)
           # 获取查询的结果
   
       # 关闭Cursor对象
       cs1.close()
       conn.close()
   
   if __name__ == '__main__':
       main()
   ```

3. 查询多行数据  fetchall()

   ```
   from pymysql import *
   
   def main():
       # 创建Connection连接
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='jing_dong',charset='utf8')
       # 获得Cursor对象
       cs1 = conn.cursor()
       # 执行select语句，并返回受影响的行数：查询一条数据
       count = cs1.execute('select id,name from goods where id>=4')
       # 打印受影响的行数
       print("查询到%d条数据:" % count)
   
       result = cs1.fetchall()
       print(result)
   
       # 关闭Cursor对象
       cs1.close()
       conn.close()
   
   if __name__ == '__main__':
       main()
   ```


#### 16.6 参数化

1. 参数化

   1. sql语句的 参数化, 有效防止`sql注入`
   2. sql的参数化的`%s`占位 与python的字符串占位符不同, 有的类似
      1. sql 占位符与 参数 是两个参数, 逗号隔开,  经 cursor.execute()后自动添加 `;`
      2. python字符串占位符, 没有逗号隔开

2. 示例

   ```
   from pymysql import *
   
   def main():
   
       find_name = input("请输入物品名称：")
   
       # 创建Connection连接
       conn = connect(host='localhost',port=3306,user='root',password='mysql',database='jing_dong',charset='utf8')
       # 获得Cursor对象
       cs1 = conn.cursor()
   
   
       # # 非安全的方式
       # # 输入 " or 1=1 or "   (必须带 双引号)
       # sql = 'select * from goods where name="%s"' % find_name
       # print("""sql===>%s<====""" % sql)
       # # 执行select语句，并返回受影响的行数：查询所有数据
       # count = cs1.execute(sql)
   
       # 安全的方式
       # 构造参数列表
       params = [find_name]
       # 执行select语句，并返回受影响的行数：查询所有数据
       count = cs1.execute('select * from goods where name=%s', params)
       # 注意：
       # 如果 有多个参数，需要进行参数化
       # 那么params = [数值1, 数值2....]，此时sql语句中有多个%s
   
       # 打印受影响的行数
       print(count)
       # 获取查询的结果
       # result = cs1.fetchone()
       result = cs1.fetchall()
       # 打印查询的结果
       print(result)
       # 关闭Cursor对象
       cs1.close()
       # 关闭Connection对象
       conn.close()
   
   if __name__ == '__main__':
       main()
   ```
   
   1. `'select * from goods where name=%s'%'商务双肩背包'` 输出结果是 `'select * from goods where name=商务双肩背包'`
   2. `'select * from goods where name="商务双肩背包"'` 输出结果是 `'select * from goods where name="商务双肩背包"'`
   3. `1与2`中语句区别是 2中的 双卫双肩背包有 双引号

--------

### 17  MySQL高级

#### 17.1 视图

1. 问题

   1. 多表关联查询,或 数据库 改变. 为了查询 结果不变, 需要修改，维护麻烦
   2. 解决办法, 视图

2. 视图是什么

   1. 视图:  一条SELECT语句 返回的结果集

   2. 视图

      1. 一张虚表, 若干 `基本表`的引用
      2. 查询语句的结果，不存储具体的数据
      3. 基本表 变了，视图 跟着变

   3. 特点: 

      1. 方便操作，特别是查询操作

      2. 减少复杂的SQL语句，增强可读性

         

3. 定义视图

   建议  `v_` 开头命名

   `create view 视图名称 as select语句;`

4. 查看视图

   查看表, 视图和其他表都列出

   `show tables;`   //显示视图和表

5. 使用视图

   视图用于`查询`

   `select * from v_stu_score;`

6. 删除视图

   ```
   drop view 视图名称;
   例：
   drop view v_stu_sco;
   ```

   

7. 视图demo

   1. `select * from goods as g left join goods_cates as c on g.cate_id=c.id left join goods_brands as b on g.brand_id=b.id;`

   2. `select g.*, c.name as cate_name, b.name as brand_name from goods as g left join goods_cates as c on g.cate_id=c.id left join goods_brands as b on g.brand_id=b.td;`

   3. `create view  v_goods_info as select g.*, c.nane as cate_name, b.name as brand_name from goods as g left join goods_cates as c on g.cate_id=c.id left join goods_ brands as b on g.brand_id = b.1d;`

      `show tables;`    //查看视图和表

      `desc v_goods_info;`

   4. `select *  from v_goods_info;`

   5. `update goods set name="老老王牌电脑" where id=22;`

      `select * from v_goods_info;`

      

8. 视图的作用

   1. 提高 `重用性`， 像一个函数
   2. 数据库`重构`，不影响程序运行
   3. 提高了`安全`，可以对不同的用户
   4. 让数据`清晰`

#### 17.2 事务

1. 为什么要有事务

   1. 事务 用于`订单`系统、`银行`系统
   2. 事务, 是`操作序列`，
      1. 这些操作要么都执行，要么都不执行
      2. 是一个不可分割的工作单位。
   3. 如，银行转帐
      1. 一个帐号扣款, 另一个帐号增款
      2. 这两个操作要么都执行，要么都不执行

2. 事务四大特性(简称ACID)

   1. ACID

      1. 原子性(Atomicity)
         1. 事务必须 为一个不可分割的最小工作单元
      2. 一致性(Consistency)
         1. 数据库 从一个一致性的状态 到另一个一致性的状态
      3. 隔离性(Isolation)
         1. 一个事务的 修改 在提交以前，对其他事务是不可见的
      4. 持久性(Durability)
         1. 事务提交,  修改永久 保存 到数据库

   2. 示例

      1. 一个银行的数据库有两张表：支票表（checking）和储蓄表（savings）。用户Jane的支票账户转移200美元到她的储蓄账户，需三步

         1. 检查支票账户, 余额高于或者等于200美元。
         2. 支票账户余额 减200美元。
         3. 储蓄帐户余额 增加200美元。

         三个步骤的 必须打包在一个事务, 任何一个步骤失败, 必须回滚所有的步骤

      2. 事务的结构
      
         1. `START TRANSACTION` 开始一个事务
         2. sql操作语句
         3.   `COMMIT`提交数据持久保存
         4. `ROLLBACK`撤销修改。
      
      3. 事务 `SQL语句`
         1. `start transaction;`  //开启事务, 与`begin;`相同
         2. `select balance from checking where customer_id = 10233276;`
         3. `update checking set balance = balance - 200.00 where customer_id = 10233276;`
         4. `update savings set balance = balance + 200.00 where customer_id = 10233276;`
         5. `commit;`

3. 事务命令

   表的引擎 必须是`innodb`类型才 用事务， mysql表的默认引擎

   1. 查看表的创建语句，看到engine=innodb

      -- 选择数据库
      `use jing_dong;`
      -- 查看goods表的创建
      `show create table goods;`   //查看建表语句, 类比 `show create database abc`

   2. 开启事务

      开启事务后 执行修改, 变更 维护到 `本地缓存` 中，不维护到 `物理表` 中

      `begin;`
      或者
      `start transaction;`

   3. 提交事务

      缓存中的 数据变更 维护到物理表中

      `commit;`

   4. 回滚事务

      放弃 缓存中变更的数据

      `rollback;`

4. 注意

   1. 修改数据的命令 自动触发 事务，包括`insert`、`update`、`delete`
   2. SQL语句中有 手动 开启事务的原因
      1. 可 进行多次数据的修改
      2. 如果成功 `一起成功`，否则 `一起会滚` 到之前的数据

##### 17.2.1 提交

* 演示: 
  * 打开两个终端窗口: 终端1, 终端2
  * 用同一个数据库，操作同一张表（如`jing_dong`数据）

步骤

1. step1：连接

   终端1：查询 商品分类信息

   `select * from goods_cates;`

2. step2：增加数据

   终端2：开启 事务，插入 数据

   `begin;`   //开启事务

   `insert into goods_cates(name) values('小霸王游戏机');`

   

   终端2: 查询 数据，此时有新增的数据

   `select * from goods_cates;`

3. step3：查询

   终端1：查询数据，发现 没有新增的数据

   `select * from goods_cates;`

4. step4：提交

   终端2：提交

   `commit;`

5. step5：查询

   终端1：查询，发现 新增的数据

   `select * from goods_cates;`



##### 17.2.2 回滚

* 演示:
  1. 两个终端窗口: 终端1, 终端2
  2. 同一个数据库，操作同一张表

步骤:

1. step1：连接

   终端1

   `select * from goods_cates;`

2. step2：增加数据

   终端2：开启事务，插入数据

   `begin;`

   `insert into goods_cates(name) values('小霸王游戏机');`

   终端2：查询数据，此时有新增的数据

   `select * from goods_cates;`

3. step3：查询

   终端1：查询数据，发现并没有新增的数据

   `select * from goods_cates;`

4. step4：回滚

   终端2：完成回滚

   `rollback;`  //rollback代替 commit

5. step5：查询

   终端1：查询数据，发现没有新增的数据

   `select * from goods_cates;`

#### 17.3 索引

1. 思考

   1. 图书馆中是如何找到一本书?
   2. 数据库 读写比 10:1

2. 解决办法

   优化方案：索引

3. 索引是什么

   1. 特殊的文件,  包含 记录的 引用指针
   2. `InnoDB数据表` 的索引是表的 组成部分
   3. 如, 数据库索引好比 书的目录，能加 查询速度

4. 索引目的

   1. 提高查询效率
   2. 可 类比字典

5. 索引原理

   1. 字典,  车次表, 图书目录
      1. 原理
         1. 不断缩小 数据的范围 筛选结果
         2. 同时把随机的事件变成 顺序的事件
   2. 数据库
      1. 原理
         1. 分段查询
         2. 如1000条数据，1到100分成第一段，101到200分成第二段，201到300分成第三段....

6. 索引的使用

   1. 查看索引

      `show index from 表名;`

   2. 创建索引

      `create index 索引名称 on 表名(字段名称(长度))`

      1. 如 指定字段是 `字符串`，需 指定长度，建议与定义字段的长度一致
      2. 若 字段类型`不是字符串`，可不 写长度部分

   3. 删除索引

      `drop index 索引名称 on 表名;`

7. 索引demo

   1. 创建测试表testindex

      `create table test_index(title varchar(10));`

   2. 用python程序（ipython也可）通过pymsql模块 向表中加入十万条数据

      ```
      from pymysql import connect
      
      def main():
          # 创建Connection连接
          conn = connect(host='localhost',port=3306,database='jing_dong',user='root',password='mysql',charset='utf8')
          # 获得Cursor对象
          cursor = conn.cursor()
          # 插入10万次数据
          for i in range(100000):
              cursor.execute("insert into test_index values('ha-%d')" % i)
          # 提交数据
          conn.commit()
      
      if __name__ == "__main__":
          main()
      ```

   3. 查询

      1. 开启运行时间监测

         `set profiling=1;`

      2. 查找第1万条数据ha-99999

         `select * from test_index where title='ha-99999';`

      3. 查看执行的时间

         `show profiles;`

      4. 为表title_index的title列创建索引

         `create index title_index on test_index(title(10));`

      5. 执行查询语句

         `select * from test_index where title='ha-99999';`

      6. 再次查看执行的时间

         `show profiles;`

8. 注意

   1. 太多的索引 会影响更新和插入的 速度
      1. 因为索引需要 更新每个索引文件
      2. 如 经常 更新和插入的表格，没有必要为很少使用的where字句单独建立索引了
      3. 如 比较小的表，排序的开销不会很大， 没有必要建立另外的索引。
   2. 索引会占用磁盘空间

#### 17.4 账号管理(了解)

1. 生产环境, 用特定的账户， 账户特定权限, 主要操作 数据的crud . 绝不用`root账户`.

2. MySQL账户体系

   账户 权限 不同，MySQL的账户 分 几种

   1. 服务 实例级 账号：启动 一个mysql，即为一个数据库实例
      1. 如 某用户`root` 有服务实例级 权限，可 删除所有的数据库、连同库中的表
   2. 数据 库级别 账号：对特定数据 库 执行`增删改查` 操作
   3. 数据 表级别 账号：对特定 表 执行`增删改查`操作
   4. 字段级别 的权限：对表的特定 字段 操作
   5. 存储 程序级别 的账号：对存储程序 `增删改查`  操作

3. 账户的操作`创建账户`、`删除账户`、`修改密码`、`授权权限`

4. 注意

   1. `root`账户, 有最高的实例级权限, 操作账户
   2. 通常用数据 库级 操作权限

#### 17.5 MySQL主从 同步配置

1. 主从同步的定义

   1. 定义
      1. 主从同步:  数据复制 从一个数据库服务器到其他服务器
      2. 一台服务器当 主服务器（master），多台的服务器当 从服务器（slave）
      3. 复制是异步的. 从服务器 不 一直连接着主服务器，从服务器 通过拨号断断续续地连接主服务器
      4. 通过配置文件, 指定复制的 数据库, 表
   2. 好处
      1. 增加从服务器, 提高数据库的性能
         1. 主服务器 写入和更新
         2. 从服务器提供 读功能
         3. 动态调整从服务器数量
      2. 数据安全
         1. 数据 复制到从服务器，从服务器 终止复制进程
         2. 数据在从服务器 备份, 不破坏主服务器 数据
      3. 提高主服务器的性能
         1. 主服务器 生成实时数据
         2. 从服务器 上分析这些数据

2. 主从同步的机制

   1. 基于`二进制日志`机制 实现 Mysql服务器 主从同步 
      1. 主服务器 用`二进制日志` 记录数据库的变动
      2. 从服务器 读取/执行日志 保持和主服务器的 数据一致
   2. 使用二进制日志
      1. 主服务器的操作 被记录
      2. 从服务器 接收 日志的 副本
      3. 从服务器可指定执行 日志中的 事件（如只插入数据或更新数据），默认 执行日志中的所有语句 
   3. 主服务器 从服务器 有唯一的ID号

3. 主从同步 配置的基本步骤

   很多种方法, 总结, 步骤

   1. 主服务器, 开启`二进制日志`机制 配置一个ID
   2. 每一个从服务器 配置一个ID，创建账号 专门复制主服务器数据
   3. 复制前，主服务器上记录二进制文件的位置
   4. 复制前, 数据库 有数据, 先创建 数据快照（用mysqldump导出数据库，或 复制数据文件）
   5. 配置从服务器要连接的 主服务器的IP地址  登陆授权，二进制日志名 位置

4. 配置主从同步 详细方法

   主和从的身份 自己指定. 虚拟机Ubuntu中MySQL作 主服务器, Windows中MySQL作 从服务器。保证Ubuntu与Windows间 网络连通。

   1.  备份主服务器原有数据到从服务器 ---- 两步: 主备份, 从还原

      设置主从同步前，主服务器有数据,  用`mysqldump`备份并还原到从服务器 

      1. 主服务器Ubuntu 数据备份, 命令：

         `mysqldump -uroot -pmysql --all-databases --lock-all-tables > ~/master_db.sql`

         1. `-u `：用户名
         2. `-p` ：密码
         3. `--all-databases `  : 选项, 所有数据库
         4. `--lock-all-tables` 锁住所有表，防止操作时 数据修改
         5. `~/master_db.sql`  导出的备份数据(sql文件)位置, 自己指定

      2. 从服务器Windows 数据还原

         1. 找到 Windows上`mysql`命令的位置, 在其目录中, 空白处按shift键点右键 打开`命令窗口`

         2. 主服务器Ubuntu中导出的文件复制到从服务器Windows中，放在 mysql命令 在的文件夹中, 供还原使用

         3. 在打开的命令黑窗口中 还原数据

            `mysql –uroot –pmysql < master_db.sql`

   2. 配置主服务器master(主服务器的名字)（在Ubuntu中 MySQL）

      1. 设置 mysqld配置文件，设置: log_bin和server-id

         `sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf`

         ```
         server-id = 1
         log_bin = /var/log/mysql/mysql-bin.log
         ```

      2. 重启mysql服务

         `sudo service mysql restart`

      3. 登入主服务器Ubuntu中的mysql, 创建帐号用于从服务器 同步数据

         1. `mysql -uroot -pymysql`

         2. `GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%' identified by 'slave';`
         3. `FLUSH PRIVILEGES;`

      4. **查看**主服务器的二进制日志信息

         `SHOW MASTER STATUS;`

         <table>
             <tr><td>File</td><td>Position</td><td>Binlog_Do_DB</td><td>Binlog_Ignore_DB</td><td>Executed_Gtid_ Set</td></tr>
             <tr><td>mysqL- bin.000006</td><td>590</td><td></td><td></td><td></td></tr>
         </table>

         1. File 日志文件名字
         2. Position 文件位置
         3. 这两个参数 配置从服务器时 用到

   3. 配置从服务器slave(从服务器的名字)（Windows中的MySQL）

      1. 找到Windows中MySQL的配置文件 `my.ini`

         `c:\\ProgramData\\MySQL\\MySQL Server 5.7`

      2. 编辑my.ini文件， server-id设置为2， 保存退出。

         ```
         #Server Id.
         server-id = 2
         ```

      3. 打开windows服务管理

         开始菜单, 输入`services.msc`找到, 运行 

      4. 打开的服务管理中找到`MySQL57`， 重启该服务

5. 进入windows的mysql，设置 连接到 `master`主服务器

   ```
   change master to master_host='10.211.55.5',
   master_user='slave',
   master_password='slave', 
   master_log_file='mysql-bin.000006', 
   master_log_pos=590;
   ```

   1. master_host：主服务器Ubuntu的ip地址
   2. master_log_file: 前面查询到的主服务器 日志文件名
   3. master_log_pos: 前面查询到的主服务器 日志文件位置

6. 从服务器中 开启同步，查看同步状态

   进入从服务器的c:\Windows\system32\cmd.exe, 双击cmd.exe, 输入`mysql -uroot -pymysql`登录mysql, 在 命令窗口中 在输入

   1. `start slave;`

   2. `show slave status \G;`

      Slave_IO_Running: Yes

      Slave_SQL_Running:Yes

      #表示同步已正常运行

7. 测试主从同步

   1. Ubuntu的MySQL中（主服务器）创建 一个数据库

      `show databases;`

   2. Windows的MySQL中（从服务器）查看 新建的数据库是否存在

      `show databases;`

      

-----

#### 结束 ---- 课件

----

#### 58到家 数据库30条军规解读

58沈剑 [架构师之路](javascript:void(0);)

军规适用场景：并发量大、数据量大的互联网业务

军规：介绍内容

解读：讲解原因，解读比军规更重要

 

一、基础规范

（1）必须使用**InnoDB**存储引擎

解读：支持事务、行级锁、并发性能更好、CPU及内存缓存页优化使得资源利用率更高

 

（2）必须使用**UTF8**字符集

解读：万国码，无需转码，无乱码风险，节省空间

 

（3）数据表、数据字段必须加入**中文注释**

解读：N年后谁tm知道这个r1,r2,r3字段是干嘛的

 

（4）禁用 : 存储过程、视图、触发器、Event

解读：高并发大数据的互联网业务，架构设计思路是“解放数据库CPU，将计算转移到服务层”，并发量大的情况下，这些功能很可能将数据库拖死，业务逻辑放到服务层具备更好的扩展性，能够轻易实现“增机器就加性能”。数据库擅长**存储**与**索引**，CPU计算还是上移吧

 

（5）禁止存储**大文件**或者大照片

解读：为何要让数据库做它不擅长的事情？大文件和照片存储在文件系统，数据库里存URL多好

 

二、命名规范

（6）只允许使用**内网域名**，而不是ip连接数据库

 

（7）线上环境、开发环境、测试环境数据库内网域名遵循**命名规范**

业务名称：xxx

线上环境：dj.xxx.db

开发环境：dj.xxx.rdb

测试环境：dj.xxx.tdb

**从库** : 在名称后加-s标识，**备库** : 在名称后加-ss标识

线上从库：dj.xxx-s.db

线上备库：dj.xxx-sss.db

 

（8）库名、表名、字段名：**小写**，**下划线**风格，不超过32个字符，必须见名知意，禁止拼音英文混用

 

（9）表名**t_xxx**，非唯一索引名**idx_xxx**，唯一索引名**uniq_xxx**

 

三、表设计规范

（10）单实例**表数目**必须小于**500**

 

（11）单表**列数目**必须小于**30**

 

（12）表必须有**主键**，例如自增主键

解读：

a）主键递增，数据行写入可以提高插入性能，可以避免page分裂，减少表碎片提升空间和内存的使用

b）主键要选择较短的数据类型， Innodb引擎普通索引都会保存主键的值，较短的数据类型可以有效的减少索引的磁盘空间，提高索引的缓存效率

c） 无主键的表删除，在row模式的主从架构，会导致备库夯(hang)住

 

（13）禁用**外键**，如果有外键完整性约束，需要应用程序控制

解读：外键会导致表与表之间耦合，update与delete操作都会涉及相关联的表，十分影响sql 的性能，甚至会造成死锁。高并发情况下容易造成数据库性能，大数据高并发业务场景数据库使用以性能优先

 

四、字段设计规范

（14）必须把字段定义为**NOT NULL**并且提供**默认值**

解读：

a）null的列使索引/索引统计/值比较 复杂，对MySQL来说更难优化

b）null 类型MySQL内部需要进行特殊处理，增加数据库处理记录的复杂性；同等条件下，表中有较多空字段的时候，数据库的处理性能会降低很多

c）null值 要更多的存储空，无论是表还是索引中每行中的null的列都要额外的空间来标识

d）null 的处理: 只能采用is null或is not null，而不能采用=、in、<、<>、!=、not in这些操作符号。如：where name!=’shenjian’，如果存在name为null值的记录，查询结果就不会包含name为null值的记录

 

（15）禁止使用**TEXT、BLOB**类型

解读：会浪费更多的磁盘和内存空间，非必要的大量的大字段查询会淘汰掉热数据，导致内存命中率急剧降低，影响数据库性能

 

（16）禁止使用**小数**存储货币

解读：使用整数吧，小数容易导致钱对不上

 

（17）必须使用**varchar(20)**存储手机号

解读：

a）涉及到区号或者国家代号，可能出现+-()

b）手机号会去做数学运算么？

c）varchar可以支持模糊查询，例如：like“138%”

 

（18）禁止使用**ENUM**，可使用**TINYINT**代替

解读：

a）增加新的ENUM值要做DDL操作

b）ENUM的内部实际存储就是**整数**，你以为自己定义的是字符串？

 

五、索引设计规范

（19）单表索引 建议控制在**5**个以内

 

（20）单索引字段数 不允许超过**5**个

解读：字段超过5个时，实际已经起不到有效过滤数据的作用了

 

（21）禁止在更新十分频繁、区分度不高的属性上建立**索引**

解读：

a）更新会变更B+树，更新频繁的字段建立索引会大大降低数据库性能

b）“性别”这种区分度不大的属性，建立索引是没有什么意义的，不能有效过滤数据，性能与全表扫描类似

 

（22）建立**组合索引**，必须把区分度高的字段放在前面

解读：能够更加有效的过滤数据

 

六、SQL使用规范

（23）禁止使用 `SELECT *`，只获取必要的**字段**，需要显示说明列属性

解读：

a）读取不需要的列会增加CPU、IO、NET消耗

b）不能有效的利用覆盖索引

c）使用`SELECT *`容易在增加或者删除字段后出现程序BUG

 

（24）禁止使用`INSERT INTO t_xxx VALUES(xxx)`，必须显示指定插入的**列属性**

解读：容易在增加或者删除字段后出现程序BUG

 

（25）禁止使用**属性隐式转换**

解读：SELECT uid FROM t_user WHERE phone=13812345678 会导致全表扫描，而不能命中phone索引，猜猜为什么？（这个线上问题不止出现过一次）

 

（26）禁止在**WHERE**条件的**属性**上使用**函数或者表达式**

解读：SELECT uid FROM t_user WHERE from_unixtime(day)>='2017-02-15' 会导致全表扫描

正确的写法是：SELECT uid FROM t_user WHERE day>= unix_timestamp('2017-02-15 00:00:00')

 

（27）禁止**负向查询**，以及**%**开头的模糊查询

解读：

a）负向查询条件：NOT、!=、<>、!<、!>、NOT IN、NOT LIKE等，会导致全表扫描

b）%开头的模糊查询，会导致全表扫描

 

（28）禁止**大表**使用**JOIN**查询，禁止大表使用**子查询**

解读：会产生临时表，消耗较多内存与CPU，极大影响数据库性能

 

（29）禁止使用**OR**条件，必须改为**IN**查询

解读：旧版本Mysql的OR查询是不能命中索引的，即使能命中索引，为何要让数据库耗费更多的CPU帮助实施查询优化呢？

 

（30）应用程序必须捕获**SQL异常**，并有相应处理



总结：大数据量高并发的互联网业务，极大影响数据库性能的都不让用，不让用哟。



-----

#### mysql sql常用语句大全

网络收集

1. 常用操作数据库的命令
   1. `show databases;` 查看所有的数据库
   2. `create database test;` 创建一个叫test的数据库
   3. `drop database test;` 删除一个叫test的数据库
   4. `use test;` 选中库 ,在建表之前必须要选择数据库
   5. `show tables;` 在选中的数据库之中查看所有的表
   6. `create table 表名 (字段1 类型, 字段2 类型);`
   7. `desc 表名;`  查看所在的表的字段
   8. `drop table 表名;` 删除表
   9. `show create database 库名;`  查看创建库的详细信息
   10. `show create table 表名;`  查看创建表的详细信息
2. 修改表的命令
   1. 修改字段类型 `alter table 表名 modify 字段 字段类型;`
   2. 添加新的字段 `alter table 表名 add 字段 字段类型`
   3. 添加字段并指定位置  `alter table 表名 add 字段 字段类型   after 字段;`
   4. 删除表字段  `alter table 表名 drop 字段名;`
   5. 修改指定的字段  `alter table 表名 change 原字段名字  新的字段名字 字段类型;`
3. 对数据的操作
   1. 增加数据(insert)3种方式
      1. `insert into 表名 values(值1，值2，. . . );`(很少用)
      2. `insert into 表名(字段1，字段2. . . ) values(值1，值2，. . . . );`（较常用）
      3. `insert into 表名(字段1，字段2. . . ) values(值1，值2，. . . . )，(值1，值2，. . . . )，(值1，值2，. . . . );`
   2. 删除数据 : `delete from 表名 where 条件` 注意：where 条件必须加，否则数据会被全部删除
   3. 更新数据: `update 表名 set字段1 = 值1, 字段2 = 值2 where 条件`
   4. 查询数据(select)
      1. 查询表中的所有数据   `select * from 表名`
      2. 指定数据查询    `select 字段 from 表名 `
         根据条件查询出来的数据  `select 字段 from 表名 where 条件 (最常用的)`
         where 条件后面跟的条件
          关系：>,<,>=,<=,!=  
          逻辑：or, and 
          区间：id between 4 and 6 ;闭区间，包含边界
   5. 排序
      `select 字段 from 表 order by 字段  排序关键词(desc | asc)`
      排序关键词 desc 降序 asc 升序(默认)
      1. 通过字段来排序
         例如 ：`select * from star order by money desc, age asc;`   
      2. 多字段排序
         `select 字段 from 表 order by 字段1  desc |asc,. . . 字段n desc| asc;`
   6. 常用的统计函数 sum，avg，count，max,min
      只分组:select * from 表 group by 字段
      例子: `select count(sex) as re,sex from star group by sex having re > 3;`
      分组统计: `select count(sex) from star group by sex;`
   7. 分组  `select * from 表名  limit 偏移量,数量;`
      说明:
      1. 不写偏移量的话就是默认的为0
      2. 实现分页的时候必须写偏移量
         偏移量怎么计算？:
          limit (n-1)*数量 ,数量 
4. 多表联合查询
   1. 内连接
      1. 隐式内连接 `select username,name from user,goods where user,gid=gods,gid;`
      2. 显示内连接
         `select username,from user inner join goods on user.gid=goods.gid;`
         `select * from user left join goods on user.gid=goods.gid;`
   2. 外链接
      1. 左连接 包含所有的左边表中的记录以及右边表中没有和他匹配的记录
      2. 右连接 
         `select * from user where gid in(select gid from goods);`
         `select * from user right jOin goods on user. gid=goods. gid;`
   3. 子嵌套查询
   4. 数据联合查询
      `select * from user left join goods on user.gid=goods.gid union select * from user right join goods on user.gid=goods.gid;`
   5. 两个表同时更新
      `update user u, goods g set u.gid=12,g.price=1 where u.id=2 and u.gid=g. gid;`
5. DCL 数据控制语言
   1. 创建用户: `create user'xiaoming'@'localhost' identified by '666666';`
   2. 授权用户:`grant all on test.*to'xiaoming'@'localhost';`
   3. 刷新权限:`flush privileges;`
   4. 取消授权:`revoke all on test.* from 'xiaoming'@'localhost';`
   5. 删除用户: `drop user'xiaoming'@'localhost';`
6.  DTL 数据事务语言
   1. 开启事务：`set autocommit=0;`
   2. 操作回滚：`rollback;`
   3. 提交事务：`commit;`

