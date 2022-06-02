

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

1. 不区分大小写与区分大小写
   1. SQL 指令(关键字)不区分大小写
   2. 数据字符串区分大小写, 字符串用 `" "` 或`' '` 括起来,  "A"与'a' 不同
   3. 单引号与双引号 相同,  如: "a"与'a'相同, 
   4. 字段名 不区分大小写, `SCHOOL, scHOOL, school, SCHOOl,...`的结果相同
   5. 空格 是字符,  `' a'`与`'a'`不同,  
   6. 字段名 不区分大小, 不用引号
   7. 表名 不区分大小, 不用引号
   8. 关键字 不区分大小, 不用引号. SELECT  select  Select 都是一样的.
2. 登录
   1. `mysql -uroot -p`    //后面没有分号
3. 导入数据库
   1. `source F://a/b.sql`   //后面没有分号
   2. 导入的是数据库, 不是数据库的一张表

### 数据库

### 兄弟连 姚青林

SQL习题网站:

 https://leetcode-cn.com/problemset/all/    力扣网, 练习算法,数据库,shell

#### mysql密码

mysql57 服务名

密码: mysql

用户名root

服务面板中查看 mysql 的服务名  mysql57

#### d1_select

1. `select...from...where...group by...having...order by...limit...;`

   1. 执行顺序1-->7
      1. `from...`
      2. `where...`,  数据来自 from, 可以没有 where
         1. `where 1=1`   # 永远正确
      3. `group by...`,  数据来自`from->where `之后
         1. group by之后只有各个组的**第一条**数据
         2. 若只有两组, 则group by 之后只有两条数据
         3. group by 之后是一个临时的**新数据表**(VT表)
      4. `select...` ,选择显示的 数据对象
         1. select 后的数据 决定了输出了 什么
         2. `select count(1) `  # 输出计数个数count(1)
         3. `select count(*)`  # 输出计数个数count(*)
         4. `1`和`*`作用是一样的,`1,2,3,4,'1','A'`都可以
      5. `having...` 对`group by...`的每组的**首行**
         1. `having id < 5 and id >2`   # 符合条件 `id 小于5 和 id 大于 2`的数据
         2. `having count(1) > 2`   # 组内成员个数>2的数据
         3. 聚合函数起作用
      6. `order by...`  对having后`新VT表`进行排序 .  VT virtual table
         1. order by 的值, 默认 `ASC`,  降序 `DESC`
      7. `limit...` 分页,
         1. `limit 2,3`;   # 从第二条开始取3条,不包括第二条
         2. 从1开始计数
      8. 总结:
         1. from, where, group by 与筛选有关
         2. select, having, order by, limit, 与显示有关
         3. 先筛选, 再显示
   2. select  **显示**  列或行

2. 聚合函数

   1. 参数是每个**组**
   2. 聚集函数,共 6 个,参数是每个组
      1. `count(id)`   # 统计id的计数
         1. `count(distinct name)`   # name去重后的计数
      2. `sum(id)` ,  `sum(distinct id)`
      3. `max(id)`,  `max(distinct id)`
      4. `min(id)` ,  `min(distinct id)`
      5. `avg(id)`,  `avg(distinct id)`
      6. `group_concat(id)` ,   `group_concat(distinct id)`
   3. 逻辑符号 , `AND`  `OR`   `!=`  不等于

3. `group by...`

   1. <table>
          <tr><td>id</td><td>name</td><td>clid</td><td>temp</td><td>count(1)</td></tr>
          <tr><td>1</td><td>A</td><td>1</td><td>1</td><td>4</td></tr>
          <tr><td>2</td><td>B</td><td>1</td><td>1</td><td>4</td></tr>
          <tr><td>3</td><td>C</td><td>2</td><td>1</td><td>3</td></tr>
          <tr><td>4</td><td>D</td><td>2</td><td>1</td><td>3</td></tr>
          <tr><td>5</td><td>E</td><td>1</td><td>1</td><td>4</td></tr>
          <tr><td>6</td><td>F</td><td>1</td><td>1</td><td>4</td></tr>
          <tr><td>7</td><td>G</td><td>2</td><td>1</td><td>3</td></tr>
      </table>

      1. temp, count(1) 是 group by后 mysql软件自己加上去的, 在group by之前没有

   2. `select * from stu group by clid;`

      结果:

      <table>
          <tr><td>id</td><td>name</td><td>clid</td><td>temp</td><td>count(1)</td></tr>
          <tr><td>1</td><td>B</td><td>1</td><td>1</td><td>4</td></tr>
          <tr><td>3</td><td>C</td><td>2</td><td>1</td><td>3</td></tr>
      </table>

      1. `group by...`后只有两条数据, 新建的子表. 
      2. `group by...`语句消失后, 子表也消失

   3. `select count(1)  from stu group by clid;`

      结果:

      <table>
          <tr><td>count(1)</td></tr>
          <tr><td>4</td></tr>
          <tr><td>3</td></tr>
      </table>

      1. `count(1)` 中的 1  可换成任意值 `*, 2, 3, 5, 'A', 'a', "A", "a", "我"`, 都可以的
      2. `count(1)`  作我字段名输出的

4. select: 一行一行输出

5. 关键点

   1. `on...`
      1. 从`from 表`中筛选数据 建 **临时表**
      2. from后的第一次筛选
   2. `where...`
      1. `同行`数据比较
      2. 不同行要join 
      3. from后的第二次筛选
   3. `having...`
      1. group by得到的数据的 筛选
   4. 聚合函数, 
      1. 一般必须有group by
      2. 列内数操作

6. `null或 NULL` 不可作判断条件

   1. NULL与任何值比较返回FALSE
   2. 用`IS` 比较
      1. IS NULL
      2. IS NOT NULL
   3. `=` 始终返回FALSE
      1. =,!=,>,<都是false

7. 小结

   1. From 中的字段,可在select中用
   2. select 中的字段要显示出来
   3. select子句中添加别名 as name1. name1 作 输出字段名
   4. as后的`别名`   用/不用`引号` 都可以
      1. `AS a`
      2. `AS 'a'`
      
      

8. ```
   SELECT (CASE WHEN COUNT(1)<=1 THEN null WHEN COUNT(1)>=2 THEN (SELECT salary FROM employee GROUP BY salary ORDER BY salary LIMIT 1,1) AS SecondHighestSalary FROM employee
   ```

   1. select中的 字段 有多种输出时用`CASE WHEN`
   2. `SELECT (CASE WHEN a.name THEN a.price WHEN...THEN...END) FROM ...`
      1. `CASE WHEN...`  每次一行,与`select`同步

9. 子查询中 `where =` 具有分组功能

   1. zb表

      <table>
          <tr><td>zhubo_id</td><td>level</td><td>gap</td></tr>
          <tr><td>123</td><td>9</td><td>30</td></tr>
          <tr><td>123</td><td>9</td><td>40</td></tr>
          <tr><td>123</td><td>8</td><td>20</td></tr>
          <tr><td>123</td><td>9</td><td>30</td></tr>
          <tr><td>123</td><td>9</td><td>40</td></tr>
          <tr><td>123</td><td>8</td><td>20</td></tr>
          <tr><td>123</td><td>9</td><td>10</td></tr>
          <tr><td>123</td><td>10</td><td>10</td></tr>
          <tr><td>246</td><td>6</td><td>20</td></tr>
          <tr><td>246</td><td>6</td><td>30</td></tr>
          <tr><td>246</td><td>6</td><td>20</td></tr>
          <tr><td>246</td><td>6</td><td>30</td></tr>
          <tr><td>246</td><td>10</td><td>10</td></tr>
      </table>

   2. ```
      SELECT level FROM zb zb1 WHERE level in (SELECT level FROM zb zb2 WHERE zb2.zhubo_id = zb1.zhubo_id);
      ```

   3. 执行过程:

      1. 主句: 先`from zb`从表zb1中取所有数据行, 再从zb1中取`第一行`记录(数据),
      2. 从句: 先`from zb`从表zb2中取所有数据行, 再从zb2中取`第一行`记录数据
      3. zb1的`一行`记录与zb2的`一行`记录 在子句的where中比较: `zb2.zhubo_id = zb1.zhubo_id`,  
         1. true, 返回子句的zb2的当前行记录的level值
         2. false, 不返回
      4. 然后 再从子句的表zb2中取 一行 记录, 重复第3步. 直至zb2的 所有`行记录`比较完
         1. 至此, zb1表的第一行与zb2表的所有行比较一遍
      5. 又 从主表zb1中取 `第二行` 记录
      6. zb1的第二行的`zb1.zhubo_id`与zb1的第一行的`zb1.zhubo_id` 相等比较. 不用计算.因为'='情况时,重复相等的值不计算.
      7. 直到zb1第8行, 都是重复步骤6
      8. 又 从主表zb1中取 第9行 记录. 主表zb1的第9行的 `zb1.zhubo_id` 的值改变, 重复步骤2-7. 返回zb2中的level值.
      9. 如此循环, 直到主表zb1的所有的zb1.zhubo_id值与从表zb2中的所有的zb2.zhubo_id 比较一遍. 返回zb2中的level值. 
      10. 至此筛选出子表b2中符合条件的level
      11. 再以此level数据为条件, 从表zb1中 一行一行筛选符合条件的level
      
   4. 聚合函数示例: 
   
      1. 以主表  `zb1.zhubo_id` 值的不同 分组, 然后对每组做 MAX()运算. 
         1. 备注:如果没有MAX(),则各组间对应位置  合在一起.

#### d2- CASE WHEN...THEN...ELSE...END 和 JOIN

1. where 语句, 过滤, 同行比较, 不同行的数据不能比较

   1. 必须 JOIN 成行 才可以
   2. where 对行数据筛选

2. where与having 比较

   1. where分组group by前 过滤数据，没有聚集函数，结果: 过滤出特定的行。
   2. having分组group by后 过滤数据，有 聚合函数，结果: 过滤出特定的组
   
   3. having 必须 group by之后才用
   
3. 聚合函数

   1. count(),sum(),max(),min(),avg(),group_concat()
   2. 组内的`一列数` 统计处理
   3. 操作数据在一组中

4. `case when..then..else..when..then..else..end`  //select 中的字段

   1. ```
      //示例: case when作select 中的字段
      select id,name,(case when clid = 20 then 10 when clid = 6 then 3 else null end) as A from stu;
      ```
   ```
      
   2. 顺序
   
      1. 先 from,从stu中取一行record
      2. 再 在case when语句进行record筛选,选出字段
      3. select 对选定的字段输出
   
   3. `else`可以省略,但`end`不能省略
   
   4. `case when`应用情景
   
      1. on条件子句中  `from st join sc on case when sc.sid=1 then sc.sid end `
      2. where条件子句中  `where case when sid=1 then sid end`
      3. having条件子句中  `having case when sid = 1 then sid end`
      4. 聚合函数中 `max(case when sid = 1 then sid end)`
   ```

5. join

   1. 所有的操作都可以用 `join` 完成

      1. `inner join, left join, right join`

   2. inner join
   
   1. `select * from table1 a inner join table2 b on a.id=b.id`
   
2. select a 与 b 的交集
   
   3. left join
   
      1. `SELECT * FROM table1 AS a LEFT JOIN table2 as b ON a.column=b.column`
      2. 特点
         1. 必须有on 条件
         2. 左侧表内容全出现
         1. 显示符合条件的 数据行记录
         2. 显示左边所有数据行，右边没有对应的行记录时NULL填充
   
4. right join 与left join 原理是一样的
   
   5. 经典句
   
      1. ```
      select e1.name from employ as e1 join employ m on e1.id = m.managerid where e1.salary > m.salary;
         ```
   
      1. 别名可以不用as
      
   2. from后的子表必须有`别名ALIAS`
      
      2. ```
          select name,max(case when stage = '基础' then score else null end) as '基',max(case when stage ='爬虫' then score else null end) as '爬虫',max(case when stage='SQL' then score else null end) as 'SQL' from score group by name;
          ```
        ```
      
        ```
      
         1. group by 分组
         2. max() 操作分组后的数据
         3. 字段name与max()用 `逗号`隔开
         4. as后是 新字段 `别名`
         5. 同一个表命名为e1,m以jion..on连接 区分
      ```
      
      ```

#### d2 pymysql 包

##### pymysql包

1. `pymysql` 是 python版的 mysql软件的包

   

2. pycharm中使用使用 pymysql 包

   1. ```
      #导入python版pyMysql的包,不是python内置包, 用pip安装
      import pymysql
      
      #创建与数据库连接(IP, mysql用户名, 登录mysql密码, 数据库名)
      #返回Connection类的实例对象
      conn = pymysql.connect('localhost','root','123456','python20')
      
      #创建游标 cursor
      cursor = conn.cursor()
      
      #sql语句
      sql = "insert into user_table values (7,'太狼','不知道','pwd6','pwd6@163.com',11,'234567');"
      
      # 游标执行 sql语句
      cursor.execute(sql)
      
      #data = cursor.fetchone()
      
      #链接 提交
      conn.commit()
      
      # 游标 关闭
      cursor.close()
      
      # 链接关闭
      conn.close()
      ```
   1. 流程
         1. 创建数据库 链接
         2. 使用 链接 创建并返回游标
         3. 执行操作
      2.  `pymysql` 版本不一样，
         1. `0.9.2`  不用指定各参数运行，
         2. 最新版 `1.0.2` 要指定参数名，**不能够省略**
      
   2. pymysql 解析

      1. pymysql.Connect() 类
         1. 参数
            1. host(str):      MySQL服务器地址
            2. port(int):      MySQL服务器端口号
            3. user(str):      用户名
            4. passwd(str):    密码
            5. db(str):        数据库名称
            6. charset(str):   连接编码
      2. connection() 的对象
         1. 方法
            1. cursor()        使用该连接创建并返回游标对象
            2. commit()        提交当前事务
            3. rollback()      回滚当前事务
            4. close()         关闭连接
      3. cursor() 对象
         1. 方法
            1. execute(op)     执行一个数据库的查询命令
            2. fetchone()      取得结果集的下一行
            3. fetchmany(size) 获取结果集的下几行
            4. fetchall()      获取结果集中的所有行
            5. rowcount()      返回数据条数或影响行数
            6. close()         关闭游标对象

##### sql 语句  -- 重点

1. create   # 创建 数据库/ 数据表 索引, 主键

   1. create database 数据库

      1. `create database abc default charset=utf8; #创建数据库abc`
      2. `create database aa default charset=utf8;`

   2. create table 数据表

      1. `create table ab(id int primary key auto_increment,name varchar(10)) default charset=utf8; #创建数据表ab`

         1. `auto_increment`, `primary key`  先后顺序 都可以

         2. AUTO_INCREMENT类型的属性

            1. 设置主键 自增长，默认从1 ，每次+1
            2. 一个表只有1个自增长字段，且自增长的字段一定配合主键用
               1. 也就是说“被标识为自增长的字段，一定是主键
               2. 但主键不一定是自增长的. 自增长只对整数类、整数列有效，对字符串无意义

         3. primary key

            1. 主键的作用：为一条数据的唯一标识，像每个人的身份证一样

            2. 主键字段的要求：值不重复且值具有唯一性。主键不能为空

            3. 设置“单字段主键”和“多字段主键（复合主键）”，用多个字段确定唯一性

               1. ```
                  #设置多字段段主键 
                  #将id与card同时设置为主键，设置后的结构图如下所示： 
                  create TABLE if not EXISTS timez( id int auto_increment, atime year, card char(18), primary key(id,card))engine=innodb charset = utf8;
                  ```

            4. rimary书写时可省略

   3. 索引

      1. create index 创建索引
         1. 创建唯一索引
            1. `CREATE UNIQUE INDEX index_name ON a(name)`  # a 表名, name 表的字段, index_name 新建的索引名
            2. UNIQUE INDEX唯一索引, 省略UNIQUE可创建重复索引
         2. 创建普通索引
            1. `CREATE INDEX index_name ON a(name)`  # a表名, name字段名
               1. 简单索引, 
               2. ON a  # 表a
         3. 降序索引 `列name` 的值
            1. `CREATE INDEX index_name ON a(name DESC)`
      2. drop index 删除索引
         1. `alter table ab drop index index_email;` # 删除 索引名
            1. index_email # 索引名
      3. add index 添加索引
         1. `alter talbe ab  add index index_email(email);` #对字段email添加索引index_email
            1. index_email # 新建的索引名
            2. (email)  列字段名   # 对email列创建索引
      4. 索引类型
         1. PRIMARY KEY（主键索引）
            1. `ALTER TABLE  table_name  ADD PRIMARY KEY ( column )`
         2. UNIQUE(唯一索引)
            1. `ALTER TABLE table_name ADD UNIQUE (column)`
         3. INDEX(普通索引)
            1. `ALTER TABLE table_name ADD INDEX index_name ( column )`
         4. FULLTEXT(全文索引)
            1. `ALTER TABLE table_name ADD FULLTEXT ( column )`
         5. 多列索引
            1. `ALTER TABLE table_name ADD INDEX index_name ( column1, column2, column3 )`

   4. 创建 主键

      1. create方式
         2. CREATE TABLE
      2. alter 方式
         1. //添加一个字段pid, 设置: 主键（auto_increment）自增（auto_increment），不可为null,类型为int unsigned
            1. `alter table table1 add pid int unsigned not null auto_increment primary key;`

2. use  使用数据库

   1. `use abc;` #使用数据库abc

3.  show, desc  查看 数据表

   1. `show tables;` #查看数据库所有的`表`
   2. `show create table ab;`  #查看建表ab的语句
   3. `desc ab;` #查看指定表结构

4. alter table 修改表的字段, alter table...drop/add...after/modify/change

   1. alter...drop 删除字段

      `alter table ab drop clid2,add clid varchar(10); `

      1. 修改表ab, 删除字段clid2,接着 添加字段clid

   2. alter...add...after, 指定字段后 添加新字段

      `alter table ab add clid2 varchar(10) after clid1; `

      1. 修改表ab, 添加新字段`clid2`在字段`clid1`后

   3. alter...modify 修改 `字段类型`,

      `alter table ab modify clid int default 10;`

      1.  #字段名不变, 类型改变

   4. alter...change 修改 `字段名和类型`

      `alter table ab change clid clidd varchar(10) default 10; `  # clid 改为 clidd

      1. 字段名 `clid` 改为 `clidd`
      2. 类型 改为  `varchar(10)  default 10`

   5. 修改 `primary key`主键

      `ALTER TABLE aa DROP PRIMARY KEY,ADD PRIMARY KEY(email);`

      1. 修改主键: 
         1. 删除主键 `alter table...DROP PRIMARY KEY`
         2. 添加主键 `alter table...ADD PRIMARY KEY(email)`  #设置email字段为主键

5. drop... 删除 库/表

   1. drop database   删除数据库

      `drop database abc;`   # 删除数据库abc

   2. drop table   删除数据表

      `drop table ab;`  # 删除表ab

6. insert into...values()  字段添加数值. 操作数据是字段.

   1. 指定字段添加数值

      1. `insert into ab(id,name,clidd) values(1,"a","1");`

         #表ab的 指定字段 id, name, clidd中添加数值 1, "a", "1"

      2. `insert into ab(id,name) values(3,"c"); ` 

         #表ab的 指定字段 id, name 中添加数值 3, "c"

      3. `insert into ab(id,name) values(4,"c"), (5,"d"), (6,"e"); ` 

         #表ab的 指定字段 id, name 中添加多条记录(4,"c"), (5,"d"), (6,"e")

   2. 所有字段添加数值

      1. `insert into ab values(2,"b","2");`

         #表ab的 所有字段 id, name, clidd中添加数值 2, "b", "2"

         #字符串数据 要 `" "` 或`' '`, 否则报错

   3. insert into...values() 是字段级别的操作, 没有`table` 关键字

      

7. update...set...  更新字段数值. 操作数据是字段

   1. `update ab set id = 10,name = 'GG' where id = 1;`

      #指定条件的指定字段更新数值

      #表ab的指定条件id=1的指定字段id, name更新数值

      #SET  列字的部分数据的集合

8. delete from...  删除数据(行)

   1. `delete from ab where id = 2;` #删除指定条件的数据

9. select...from... 查询

   1. `select * from ab;` #查询表ab所有的字段的数值
   2. `select id from ab;` # 只查表中字段id的数值
   3. `select * from ab where id =1;`  # 指定条件 查询
   4. select指定的字段
      1. 要么在group by语句的后面，作为分组的依据
      2. 要么包含在聚合函数中

10. order by  排序

    1. `select * from stu order by name; `  # 按字段name 升序排序
    2. `select * from stu order by name desc;`  # 降序排序
    3. 默认 ASC 升序,  DESC 降序
    
11. 连接

    1. concat
       1. 多个字符串连接成一个字符串
       2. concat(str1, str2,...)
          1. 返回结果为连接参数产生的字符串，如果有任何一个参数为null，则返回值为null。
       3. `select concat(id, '.', name) as info from a`
    2. concat_ws()
       1. 和concat()一样. 多个字符串连接成一个字符串， 一次性指定分隔符 （concat_ws就是concat with separator）
       2. concat_ws(separator, str1, str2, ...)
       3. `select concat_ws('.', id, name, score) as info from a`
    3. group_concat()
       1. group by产生的同一个分组中的值连接起来，返回一个字符串结果
       2. 语法: `group_concat( [distinct] 要连接的字段 [order by 排序字段 asc/desc ] [separator '分隔符'] )`
          1. distinct 排除重复值； 结果中的值排序，用order by子句；separator 一个字符串值，缺省为一个逗号
       3. `select name, group_concat(id) from a group by name;`
          1. 每个名字出现一次，又显示所有的名字相同的人的id
       4. `select name, group_concat(id order by id desc separator '_') from a group by name;`
          1. 将上面的id号从大到小排序，用'_'作为分隔符

12. 修改表名

    1. 语法1: `remane table a to b`
    2. 语法2: `alter table tbl_name rename[to|as] new_tbl_name`


#### d3 子句查询

1. 任何的 子查询  都可以用`JOIN`语句 实现

2. 子查询 种类

   1. where子句:  where语句中 子查询
   2. join子句:  join语句中 子查询
   3. select子句:  select语句中 子查询
      1.  `(case when...then...when...then...end)`
   4. 子句 用`()` 包围

3. where 子查询

   1. `=` 逻辑, 等于一个值, 子句 要返回 一个值

      `SELECT name FROM stu WHERE classid = (SELECT id FROM class WHERE manager = '小黄');`  

      

   2. `in` 逻辑, 包含在多个记录中. 子句 要返回 多行记录(一列值)

      1. `SELECT name FROM stu WHERE classid in (SELECT id FROM class WHERE manager = '小黄');`
      2. `SELECT * FROM stu WHERE id IN (2,3,5) GROUP BY classid;`

4. join 子查询

   1. `SELECT stu.name FROM stu JOIN (SELECT * FROM class WHERE manager = '蓝') AS C ON stu.classid = C.id`
   2. 一个指令实现一个功能

5. 半连接查询(参考6 子查询)

   1. 相关查询
   2. 子查询用到父查询中表的字段

6. 子查询

   1. 都从`FROM` 开始

   2. 非相关 子查询 特点:  先执行 **子句**,再执行 **父句**.

   3. 相关 子查询 特点:

      1. 不能 一次 求解子查询, 内(子)查询与外(父)查询有关连, 要反复求值.

      2. ```
         select t.sno,t.sname,t.sage,t.sgentle,t.sbirth,sdept from student t 
         where 80<=(
         	select f.score 
         	from grade f 
         	where f.sno=t.sno and f.cname='计算机基础'
         	);
         ```

         1. 子查询用到 父句字段t.sno
         2. 都从 父句查询from 开始的: 
         3. 执行一次顺序:  父from,父where,子from,子where . 反复执行之.
            1. 父from,  从t中取一个记录行a
            2. 父where, 该记录行a到 where中filter(过滤)
            3. 子from, 从f中取一个记录行b
            4. 子where, 该记录行b到 where中filter(过滤)
            5. a在b的结果中过滤 (真正执行 `父where`)

   4. 特点: 反复求值.

7. 示例

   1. 表

      1. student表的字段  | SId | Sname | Sage | SSex |
      2. course表的字段    | CId | Cname | TId |
      3. teacher表字段      | TId | Tname |

   2. SQL语句

      1. ```
         SELECT CId 
         FROM COURSE 
         WHERE TId IN (
         	SELECT T.TId 
         	FROM Teacher T
         	WHERE T.CId IN (
         		SELECT SC.SId 
         		FROM Student SC
         		WHERE SC.SId IN(
         			SELECT S.SId 
         			FROM student s
         			WHERE s.Sname='张三'
         		)
         	)
         );
         ```

   

8. 子查询 执行顺序

   1. FROM, FROM子句中前两个表 执行笛卡尔积--生成-->虚拟表VT1
   2. ON,  VT1表应用ON筛选器 , <join_condition>为真的行插入--生成-->VT2
   3. OUTER(JOIN),  
      1. 若指定 OUTER JOIN, 保留表(preserved table)中未找到的行 作为外部行添加到 VT2 --生成-->T3. 
      2. 若FROM 有两个以上表, 则 上一次联结生成的结果表和下一个表重复执行步骤1和步骤3 直到结束
   4. WHERE,  VT3 应用WHERE筛选器, <where_condition>为TRUE的行被插入--生成-->VT4
   5. GROUP BY, GROUP BY后列字段为分组标准 对 VT4行分组--生成-->VT5
   6. CUBE|ROLLUP, 超组(Supergroups)插入VT5--生成-->VT6
   7. HAVING, VT6 应用 HAVING 筛选器, <having_condition> 为TRUE的组才插入VT6--生成-->VT7
   8. SELECT, SELECT处理 表 VT7的各列 --生成-->VT8
   9. DISTINCT, VT8中去除重复的行 --生成-->VT9
   10. ORDER BY, VT9的行按 ORDER BY后的列字段 排序 生成一个游标 VC10
   11. TOP, 从VC10的开始处选择指定数量或比例的行 生成VT11 并返回给调用者

9. 知识点

   1. ON 筛选

      1.  JOIN 后必须ON, 否则报错

      2. ```
         FROM a JOIN b ON a.ID = b.ID 正确
         ```
         
      3.  `FROM a , b ON a.ID = b.ID 报错, 为什么??`

          1.  ON要与join搭配, 不能与`,`搭配
          2.  `select * from a, b where a.ID = b.ID`  #正确, `,`与`where`搭配, 不用`ON`搭配

   2. group by

      1. 默认输出每组首行
      2. `SELECT * FROM SC GROUP BY cid;` 
         1. 依据字段cid的 三个不同值 分成三组,输出每组的第一行

   3. 输出数据

      1. 输出内容: 整个SQL语句执行完的数据, 不是SELECT 后的语句
      2. `SELECT * FROM SC GROUP BY cid HAVING SC.ID = SC.CPT;`
         1. 执行顺序, FROM--> GROUP BY --> SELECT -->HAVING
         2. SELECT 执行完后 再执行 HAVING 语句

   4. case when..then..else..end

      1. 字段数据 按行取
      2. WHEN 当前字段, THEN 不能 是上条字段或下条字段的值
      3. 语法: CASE WHEN 条件1 AND 条件2 THEN..ELSE..END
      4. 逻辑词:  AND, OR, != 

   5. IS

      1. `SC.Score IS NOT NULL`  #正确
      2. `SC.Score IS NULL`  #正确
      3. 去掉 IS 报错

   6. SUM()/COUNT()  #除法, 正确

   7. 逗号`,`  表示并列关系

      1. `FROM Sc,Course`  #逗号并列关系,正确, 与where 搭配, 不与ON 搭配

   8. 引号: 单引号/双引号

      1. `字段字符串` 不用, 用报错
      2. `表名字符串`   #不用, 用报错
      3. `数据字符串`,  #用 单引号/双引号, 不用报错





#### d4-习题

1. 派生表 必须有别名

   every derived table must have its own alias. #每个派生表必须有自己的别名

   1. `SELECT * FROM (SELECT * FROM A GROUP BY name) AS B; `
      1. 正确
      2. 没有AS B报错.  派生表没有别名 报错
   2. `SELECT * FROM (SELECT * FROM A GROUP BY Grade) AS F GROUP BY name;`
      1. 在内查询结果VT1的基础上再进行查询. 不是一直 在A上

2. group by

   1. 表A

      <table>
          <tr><td>grade</td><td>name</td><td>math</td><td>chi</td></tr>
          <tr><td>G1</td><td>A</td><td>100</td><td>97</td></tr>
          <tr><td>G1</td><td>B</td><td>89</td><td>129</td></tr>
          <tr><td>G1</td><td>C</td><td>105</td><td>110</td></tr>
          <tr><td>G2</td><td>A</td><td>123</td><td>98</td></tr>
          <tr><td>G2</td><td>B</td><td>140</td><td>130</td></tr>
          <tr><td>G2</td><td>C</td><td>108</td><td>107</td></tr>
          <tr><td>G3</td><td>A</td><td>135</td><td>100</td></tr>
          <tr><td>G3</td><td>B</td><td>140</td><td>133</td></tr>
          <tr><td>G3</td><td>C</td><td>125</td><td>100</td></tr>
      </table>

2. `SELECT name, AVG(math),AVG(chi) FROM A GROUP BY name;`
   
   <table>
          <tr><td>name</td><td>AVG(math)</td><td>AVG(chi)</td></tr>
          <tr><td>A</td><td>119.3333</td><td>98.3333</td></tr>
          <tr><td>B</td><td>123.0000</td><td>130.6667</td></tr>
          <tr><td>C</td><td>112.6667</td><td>105.6667</td></tr>
      </table>
   
   1. 按name字段的值(A,B,C) 按行切割成 三组
   
3. `SELECT name,AVG(math) FROM A GROUP BY name,Grade;`
   
   <table>
          <tr> <td>name</td><td>AVG(math)</td><td>AVG(chi)</td></tr>
          <tr><td>A</td><td>100.0000</td><td>97.0000</td></tr>
          <tr><td>A</td><td>123.0000</td><td>98.0000</td></tr>
          <tr><td>A</td><td>135.0000</td><td>100.0000</td></tr>
          <tr><td>B</td><td>89.0000</td><td>129.0000</td></tr>
          <tr><td>B</td><td>1140.0000</td><td>130.0000</td></tr>
          <tr><td>B</td><td>140.0000</td><td>133.0000</td></tr>
          <tr><td>C</td><td>105.0000</td><td>110.0000</td></tr>
          <tr><td>C</td><td>108.0000</td><td>107.0000</td></tr>
          <tr><td>C</td><td>125.0000</td><td>100.0000</td></tr>
      </table>
   
   1. 按字段name,Grade值的不同组合 分成 6组
   
4. `SELECT * FROM A GROUP BY name;`
   
   <table>
          <tr><td>grade</td><td>name</td><td>math</td><td>chi</td></tr>
          <tr><td>G1</td><td>A</td><td>100</td><td>97</td></tr>
          <tr><td>G1</td><td>B</td><td>89</td><td>129</td></tr>
          <tr><td>G1</td><td>C</td><td>105</td><td>110</td></tr>
      </table>
   
5. `SELECT name,AVG(math),AVG(chi) FROM (SELECT * FROM A GROUP BY name) AS F GROUP BY grade;`
   
   <table>
          <tr><td>name</td><td>AVG(math)</td><td>AVG(chi)</td></tr>
          <tr><td>A</td><td>98.0000</td><td>112.0000</td></tr>
      </table>
   
   1. 把A变成F,然后对F再分组
   
3. 联合更新 , join  update

   `UPDATE A JOIN B ON A.URL=B.URL SET member_ID = '0002567543' where Log_time BETWEEN '2018-05-13 00:00:00' AND '2018-05-13 23:59:59' AND B.log_Class='购物';`

   1. `A JOIN B ON A.URL = B.URL`   #构成虚拟表, 是 `UPDATE` 更新的表, 
   2. UPDATE...SET... ,  JOIN...ON...
   3. WHERE...BETWEEN...AND...

4. on,where, having  筛选条件

   1. 数据的字段筛选条件, 不是数据表VT本身

#### d5-练习

1. 数据表介绍

   ```
   --1.学生表
   Student(SId , Sname，sage ,ssex)
   --SId学生编号,Sname学生姓名,Sage出生年月,Ssex学生性别
   --2.课程表
   Course(CId,Cname,TId)
   --CId课程编号,Cname课程名称,TId教师编号
   --3.教师表
   TeaChErTId, Tname)
   --TId教师编号,Tname教师姓名
   --4.成绩表
   SC(sId,cId,score)
   --SId学生编号,CId课程编号,score分数
   ```



练习题目

1. 查询" 01“课程比”02“课程成绩高的学生的信息及课程分数
   1. --分数，SC, 两行之间的对比， 要自己JOIN 自己
      一一
      学生信息，Student
      01课程比02课程成绩高的学生ID

---------

练习语句1

1. ```
   -- 最低的课程大于80分的学生
   SELECT name
   FROM stu 
   GROUP BY name 
   HAVING MIN(Score) > 80;
   ```

   ```
   # 方法2
   SELECT name 
   FROM stu 
   WHERE name NOT IN
   	(SELECT name 
   	FROM stu 
   	WHERE Score <= 80)
   ```

   

2. ```
   SELECT year,
   MAX(CASE WHEN month = '1' THEN amount ELSE NULL END) as '1',
   MAX(CASE WHEN month = '2' THEN amount ELSE NULL END) as '2',
   MAX(CASE WHEN month = '3' THEN amount ELSE NULL END) as '3',
   MAX(CASE WHEN month = '4' THEN amount ELSE NULL END) as '4'
   FROM Order 
   GROUP BY year
   ```

3. ```
   # 1. 
   SELECT COUNT(1) 
   FROM patient;
   ```

   ```
   #2. 两个表JOIN, 确定时间的关系
   SELECT p.ID, MAX(value)
   FROM patient p JOIN patient_ana pa
   ON p.ID = pa.ID
   WHERE pa.Date < p.Date AND item = '血红蛋球浓度'
   GROUP BY p.ID
   ```

4. ```
   #1. 
   SELECT u.Name, d.Name
   FROM User u JOIN Department d ON u.dept_id = d.id
   ORDER BY d.order_code, u.order_code
   ```

   ```
   SELECT Name, salary
   FROM User WHERE (dept_id, salary) in
   (SELECT dept_id, MAX(salary)
   FROM User u 
   GROUP BY dept_id)
   ```

   ```
   SELECT dept_id, COUNT(1)
   FROM User WHERE salary > 4000
   GROUP BY dept_id
   ```

----

练习语句2

1. 某奶粉品牌有以下销售数据(订单表Orderinfo),请计算每个人的消费金额。清费频次、
   购买产品数量。第次购买时 间和最后次购买时 间

   <table>
       <tr><td>CustomeriD</td><td>OrderiD</td><td>Sales</td><td>Quantity</td><td>OrderDate</td></tr>
       <tr><td>A</td><td>01</td><td>100</td><td>1</td><td>2017-03-01</td></tr>
       <tr><td>A</td><td>02</td><td>420</td><td>3</td><td>2017-03-15</td></tr>
       <tr><td>B</td><td>03</td><td>300</td><td>4</td><td>2017-03-02</td></tr>
       <tr><td>B</td><td>04</td><td>1000</td><td>1</td><td>2017-04-01</td></tr>
       <tr><td>C</td><td>05</td><td>500</td><td>3</td><td>2017-05-03</td></tr>
       <tr><td>C</td><td>06</td><td>200</td><td>1</td><td>2017-05-04</td></tr>
       <tr><td>......</td><td></td><td></td><td></td><td></td></tr>
   </table>
解: 没有答案
   
2. ```
   SELECT CustomerId, SUM(Sales), COUNT(1), SUM(Quantity), MIN(OrderDate), MAX(OrderDate)
   FROM OrderInfo
   ORDER BY CustomerID;
   ```

3. 该奶粉品牌还有张订 单明细表(OrderDetail), 请结合上题的订单表，计算出每个SKU
   被多少客户购买了。

   <table>
       <tr><td>OrderDetaillD</td><td>OrderID</td><td>SKU</td><td>Quantity</td></tr>
       <tr><td>01</td><td>01</td><td>SKU1</td><td>1</td></tr>
       <tr><td>02</td><td>02</td><td>SKU1</td><td>2</td></tr>
       <tr><td>03</td><td>02</td><td>SKU2</td><td>1</td></tr>
       <tr><td>04</td><td>03</td><td>SKU2</td><td>2</td></tr>
       <tr><td>05</td><td>03</td><td>SKU3</td><td>2</td></tr>
       <tr><td>06</td><td>04</td><td>SKU6</td><td>1</td></tr>
       <tr><td>07</td><td>05</td><td>SKU4</td><td>2</td></tr>
       <tr><td>......</td><td></td><td></td><td></td></tr>
   </table>

   解:

   ```
   SELECT SKU, COUNT(DISTINCT o1.CustomerID)
   FROM OrderInfo o1 JOIN OrderDetail o2 ON o1.OrderID = o2.OrderID
   GROUP BY SKU
   ```

   1. DISTINCT  去重

4. 请结合Orderinfo表与OrderDetal 表，找出购买了SKU1又购买SKU2产品的人。

   解:

   ```
   SELECT DISTINCT o1.CustomerID
   FROM (OrderInfo o1 JOIN OrderDetail o2 ON o1.OrderID = o2.OrderID)
   JOIN (OrderInfo o3 JOIN OrderDetail o4 ON o1.OrderID = o2.OrderID)
   ON o3.CustomerID = o1.CustomerID
   WHERE o2.SKU = 'SKU1' AND o4.SKU = 'SKU2'
   ```

5.   请结合Orderinfo表与OrderDetail表，计算出2016年的客户在2017年的回柜率( Retention Rate.

   解:

   ```
   SELECT COUNT(DISTINCT CustomerID) / (SELECT COUNT(DISTINCT CustomerID) FROM OrderInfo WHERE year(OrderDate) = 2016) AS 'Retention Rate'
   FROM OrderInfo WHERE CustomerID IN (SELECT CustomerID FROM OrderInfo WHERE year(OrderDate) = 2016 ) AND year(OrderDate) = 2017
   ```

   1.  比值 作为 `Retention Rate`

6. 请结合OrderInfo 表与OrderDetail表，计算出2017年新老客的二购率(Repeat Purchase Rate.

   解:   

   ```
   SELECT COUNT(1) / (SELECT COUNT(DISTINCT CustomerID) FROM OrderInfo WHERE year(OrderDate) = 2017) AS 'Repeat Purchase Rate'
   FROM 
   (SELECT CustomerID
   FROM OrderInfo 
   WHERE year(OrderDate) = 2017
   GROUP BY CustomerID
   HAVING COUNT(1) >= 2) Repeat
   ```

7. 请通过订单表OrderInfo 把客户按2017年消费金额，从高到低100等分，计算出每等分中的总客户数量、总订单数量、总消费金额、总购买产品数量

   没有答案

--------

主播的面试问题

```
首先找到每个主播的最大level所对应的数据, 按照 zhubo_id 和 这个最大level进行分组, 最终获取到最小的gap

SELECT zhubo_id, level, min(gap)
FROM zhubo z1 WHERE level = 
(SELECT MAX(level) FROM zhubo WHERE zhubo_id = z1.zhubo_id)
GROUP BY zhubo_id, level
```



#### d6 d7 索引

1. 数据库的二分法查找

   1. 一分二,二分四..
   2. 每个节点: 有一个数据域, 最多有两个子节点

2. B+树

   1. 多分后查找, 每个节点分为多个
   2. 数据都出现在叶节点中
   3. 上一叶节点指向下一叶节点.

3. 索引

   1. create方法 创建索引  

      1.  `CREATE INDEX index_name ON table_name(name);` 
         1. 用表a的name字段 创建索引 index_name
      2. 错: `CREATE TABLE a (id int,....,index(id));`   #错的语句
         1. index不是约束词, 不能出现在 `列` 的定义约束中
         2. index是像table, set, database一样的关键字
   2. alter方法 创建索引 
      1. `ALTER TABLE table_name index_name(name);`  # a_name表名, index_name索引名
         1. name列字段值 变为索引index_name
   3. 查看索引
      1. `SHOW INDEX FROM a_name;`  #查看表a_name的索引

4. 为啥要 索引

   1. 顺序决定时间 效率

5. 索引分类

   1. 按 个数 分

      1. 单索引  `where name = 'li'`   # 1个索引 name
      2. 联合索引  `where name='li' and age < 4`    # 2个索引(name,age)
      3. 单索引与联合索引 都可以 用 聚簇索引 和 非聚簇索引

   2. 按 主键 分

      1. 聚簇索引

         1. 定义:

            1. 叶子节点的 值 是 主键 + 一行记录
            2. 主键 直接连接一行 数值

         2. ```
                          /\
                        /    \
                      /        \
                  /   聚簇索引   \
                └──────────────┘ 
                 │      │  ... │
            ┌───┐  ┌───┐  ┌───┐
            │主键│  │主键│  |主键│
            | + |  | + |  | + |
            | 一|  | 一|   | 一|
            | 行|  | 行|   | 行|
            | 记|  | 记|   | 记|
            | 录|  | 录|   | 录|  
            └───┘  └───┘  └───┘ 
            
            主键是 primary key
            ```

      2. 非聚簇索引

         1. 定义

            1. 叶子节点的 值 是 索引值 + 主键值, 即 索引后接 主键  
            3. 主键再调用 聚簇索引
            4. 非聚簇索引 依赖 聚簇索引 而存在
         
      2. ```
                          /\
                        /    \
                      /        \
                  /   聚簇索引  \
                └──────────────┘ 
                 │    │  ...  │
            ┌───┐  ┌───┐  ┌───┐
            │索引│  │索引│  |索引│
            | + |  | + |  | + |
            |主键|  |主键|  |主键|
            └───┘  └───┘  └───┘
            ```
         
         1. 主键再调用聚簇索引, 故非聚簇索引依赖聚簇索引工作

6. 主键

   1. 主键 创建方式

      1. CREATE 方式, 创建表时定义主键索引: 又有两种方式

         1. ```
            #方式1: 定义时指定主键, 表a
            CREATE TABLE if not exists a (id int not null auto_increment primary key, ..., ...);
            ```

            1. 只有主键 可以自动增长

            2. 关键字

               ```
               not null
               auto_increment
               primary key
               ```

               

         2. ```
            #方式2: 单独指定主键 , 表a
            create table if not exists a(
                id int,
                ......
                primary key(id) 
            );
            ```

            1. 关键字 ` primary key(id)`   #定义 id  为主键
            
         3. 联合主键

            ```
            #表a, primary key(id1, id2)
            CREATE TABLE IF NOT EXISTS a(id1 int,id2 int,..PRIMARY KEY(id1,id2));
            ```

      2. ALTER... add  primary key() 方式 , 修改表创建主键

         1. ```
            #表a
            ALTER TABLE table_name ADD PRIMARY KEY(id);
            ```

   2. 删除主键

      1. `alter table table_name drop primary key;`   #只有这一种方式

   3. 主键特性:  

      1. 有 唯一键 属性, 值不同
      2. 比 唯一键 严格

7. 唯一键 unique

   1. 字段要有唯一性，数据不重复

      1. 一张表中只能有一个主键。
      2. 唯一键（unique key）可以解决表中多个字段唯一性约束的问题。
      3. 唯一键的本质与主键差不多
      4. 唯一键默认的允许自动为空，而且可以多个为空（因为字段为空不参与唯一性比较）。

   2. 创建方式: 3种

      1. create创建, 字段之后直接跟unique关键字

         1. ```
            #方式1
            CREATE TABLE a(name int unique comment '名字',..);
            ```

            1. `comment '名字'`,  #comment是 备注,可以没有, 

      2. create创建, 所有的字段之后增加unique key(字段列表); -- 支持复合唯一键

         1. ```
            #方式2
            CREATE TABLE a(name int,...,unique key(name));
            ```

      3. alter..add  unique key() 创建, 创建表之后增加唯一键

         1. ```
            ALTER TBALE a add unique key(name);  #添加唯一键
            ```

   3. 删除唯一键 

      1.  语法1: `alter table table_name drop index 索引名;`    #
      2. 语法2: `alter table table_name drop key 索引名;`
      3. 以上两种方法删除了加在 字段上的 唯一键(头衔).  用 index 或 key 关键字都可以

   4. 小结: 

      1. 唯一键和主键本质相同，唯一区别是唯一键允许为空，而且多个字段允许为空。

      

8. 联合索引

   1. 创建联合索引
   1. create 方式创建 联合索引
         1. `CREATE INDEX index_co_name ON table_name(c1,c2,c3,c4);`   #index_co_name联合索引名
      2. alter...add 方式创建 联合索引
         1. `ALTER TABLE table_name ADD INDEX index_co_name(c1,c2,c3,c4);`  #index_co_name联合索引名
   2. 查看索引
      1. `show index from table_name;`
   3. 删除联合索引
      1. `alter table table_name drop index index_co_name`    #index_co_name联合索引名

9. 全文索引

   1. 创建全文索引
      1. create创建表时,
         1. `create table table_name(id int(10), content text not null, tag varchar(255), FULLTEXT INDEX fulltext_name(content, tag)) ENGINE = MyISAM DEFAULT CHARSET=utf8;`
      2. create 在已存在的表上创建全文索引
         1. `create fulltext index fulltext_index_name on table_name(content, tag);`
      3. alter...add FULLTEXT INDEX 方式创建
         1. `ALTER TABLE table_name ADD FULLTEXT INDEX(content,tag)`
            1. 表名 table_name
            2. FULLTEXT 关键字
   2. 删除全文索引
      1. drop index 方式
         1. `drop index fulltext_index_name on table_name;`
      2. alter table 方式
         1. `alter table table_name drop index fulltext_index_name;`
   3. 使用全文索引
      1. 和模糊匹配 like + % 不同，全文索引语法格式用 `match` 和 `against` 关键字
         1. Match()指定被搜索的列
         2. Against()指定 搜索表达式, 要搜索的表达式
      2. `select * from table_name where match(content, tag) against("xxx xxx")
   4. mysql全文搜索有三种模式
      1. 自然语言查找。这是mysql默认的全文搜索方式
         1. 隐式自然语言查找
            1. `select id,title FROM post WHERE MATCH(content) AGAINST ('search keyword')`
         2. 显式自然语言搜索方式
            1. `select id,title FROM post WHERE MATCH(content) AGAINST ('search keyword' IN NATURAL LANGUAGE MODE)`
      2. 布尔查找
         1.  适用情景: 没有自然查找模式中的50%规则
            1. `select id,title FROM post WHERE MATCH(content) AGAINST ('search keyword' IN BOOLEAN MODE)`
               1. 一定要加  “IN BOOLEAN MODE”(布尔全文搜索)  不然 如果数据重复超过50%会 出现无数据的情况。
               2. 全文索引的字段 用  ‘,’ 或 `空格` 分隔，才能有效
      3. 带子查询扩展的自然语言查找
         1.  `select id,title FROM post WHERE MATCH(content) AGAINST ('search keyword' IN BOOLEAN MODE WITH EXPANSION)`

   

10. 经典例题

   1. ```
      SELECT zhubo_id, level,min(gap) from zz z1 WHERE level = (SELECT MAX(level) FROM zz z2 WHERE z1.zhubo_id = z2.zhubo_id) GROUP BY zhubo_id,level;
      ```

      1. where子句中 子查询select 中的where具有 分类功能
      2. SELECT子句中的子查询一次返回 一行数据, 否则报错
      3. WHERE子句中的SELECT子查询一次可以返回多行数据
      4. 聚合函数的参数是每个组的所有数据

11. 全覆盖索引

    1. SQL语句用的`列字段` 包含在索引中, 这个索引叫SQL的全覆盖索引
    2. 多指普通索引,或非聚簇索引

12. 能否用索引 依据?

       1. 能比较 用索引
       2. 不能比较,不用索引

------

### sql示例 - 马艺楠

#### 语法 快捷键

1. mysql软件 命令行版本 `语法`特点

   1.  `;` 分号 结束
   2. 关键字, 字段 不区分`大小写`,  但是`规范`下需要区分大小写
   3. `\c` ,  命令打错了换行后不能修改, 终止执行用`\c`

   

2. 快捷键

   1.  `\G`   #格式化输出
       1.  语句: `desc table_name \G`
       2.  语句: `select * from table_name \G`
   2.  `ctrl + c`   #停止语句执行
   3.  语句: `\s`   #查看服务器系统信息
   4.  语句: `\q`   #退出界面
   5.  语句: `\h`	#查看帮助命令

#### SQL语句大全

1. 创建

   ```
   #创建数据库
   create database 数据库库名 default charset=utf8;	#创建数据库
   ```

   ```
   #创建数据表
   create table 表名 (字段 类型 约束,字段 类型 约束) default charset=utf8;  #建表
   ```

2. 使用

   ```
   #使用数据库
   use 数据库名;  #使用指定的数据库,这个数据库下 的所有表都可以操作
   ```

3. 查看

   ```
   #查看当前数据库下的 所有表
   show tables;
   ```

   ```
   #查看指定的表的 表结构
   desc 表名;
   ```

   ```
   #查看指定表的 建表语句
   show create table 表名;
   ```

4. 修改表 -- 增, 删, 改

   修改表结构

   ````
   #增加 字段
   alter table 表名 add 字段名 类型 字段约束;  #(新字段默认添加到 最后)
   ````

   ```
   #增加字段 在指定字段后面 
   alter table 表名 add 字段名 类型 字段约束 after name;  #字段后面(name)添加新字段
   ```

   ```
   #删除 字段
   alter table 表名 drop 要删除的字段名;  #删除指定字段
   ```

   ```
   #修改字段类型和约束条件, 不改 字段名
   alter table 表名 modify 原字段名 新的字段类型 新的字段约束;  #不改字段名,只改字段类型和约束条件
   ```

   ```
   #修改字段名,或再修改字段类型等
   alter table 表名 change 原字段名 新字段名(自定义) 类型 约束;	即修改字段名有修改字段类型  或者  只修改字段名不修改字段类型
   ```

5. 删除

   ```
   #删除 数据库
   drop database 数据库名;
   ```

   ```
   #删除 数据表
   drop table 数据表名;	
   ```

6. 数据操作 -- 增/删/改/查

   ```
   #增加 数据
   insert into 表名 (字段1,字段2,字段3) values (值1,值2,值3);
   insert into 表名 values (值1,值2,值3);
   insert into 表名 (字段1,字段2) values (值1,值2);
   ```

   ```
   #删除 数据
   delete from 表名 [where 条件];  #删除指定条件的数据(删数据需谨慎)
   ```

   ```
   #修改 数据
   update 表名 set 字段1=值1,字段2=值2,... where 条件;  #修改指定条件的数据
   ```

   ```
   #查找 数据
   
   #1. 基础查询
   select * from 表名;  #返回所有字段信息(查询全部)
   select 字段1,字段2 from 表名;  #只查 字段1和字段2(部分查询)
   
   #2. where子句的查询
   select * from 表名 where 限制条件;   #带条件查询
   ```

   ```
   #排序
   select * from 表名 order by 待排序的字段;  #升序排序(默认)
   select * from 表名 order by 待排序的字段 desc;  #降序排序
   ```

7. 逻辑删除

   1. 实现方法: 用一个字段 表示 信息不能用
   2. `alter  table students add is_delete bit default 0;`  //给students表添加一个 is_delete 字段 bit 类型 默认为0
   3. `update students set is_delete =1 where id =6;`  //is_delete=1 逻辑删除

8. 小结

   1. 数据库的 创建/使用/删除
   2. 数据表的 增/删/改/查
   3. 数据表中数据的 增/删/改/查/序

9. 约束条件

   1. 约束条件分类

      1. 关键字

         1. 主键约束：primary key:
            唯一约束 + 非空约束 的组合，可以同时保证唯一性和非空。
         2. 外键约束：foreign key：
            用于限制两个表的关系，保证从表该字段的值来自于主表相关联的字段的值
         3. 非空约束：not null ：
            保证字段的值不能为空
         4. 唯一约束：unique：
            保证唯一性但是可以为空，
         5. 默认约束：default：
            保证字段总会有值，即使没有插入值，都会有默认值！
         6. 自增长列：auto_increment：
            一个表中有且只能有一个自增长列
         7. 检查性约束：check
            在MySQL已经不支持，语法不报错，但无效可以使用枚举类型替代：枚举类型(enum)

      2. 示例

         1. ```
            create table student(
            studentno int auto_increment primary key , //定义学生学号为主键，设置位自增长
            sname nvarchar(8) not null, //定义学生姓名为非空
            sex nchar(1) default '男',  //定义性别，默认值为‘男’
            birthday datetime null,     //定义出生日期，默认为null
            classno nchar(6) ,          //定义所属教室号
            phone nchar(11) unique,     //定义唯一约束
            email nvarchar(30) unique,  //定义唯一约束
            constraint fk_class         //给约束起名字
            foreign key (classno) references class(classno) //将教室号设置为外键
            );
            ```

   2. 约束范围

      1. 列级约束

         1. ```
            create table student(
            studentno int auto_increment primary key , 
            sname nvarchar(8) not null
            );
            ```

      2. 表级约束

         1. ```
            create table student(
            studentno int auto_increment  , 
            sname nvarchar(8) not null,
            primary key(studentno,sname)
            );
            ```

      3. 表级约束可以给约束起名字，方便以后删除

         1. ```
            create table student(
            studentno int auto_increment  , 
            sname nvarchar(8) not null,
            constraint pk_student primary key(studentno,sname)
            );
            ```

   3. 修改约束

      1. 主键 (primary key)
         1. 添加：`alter table 表名 add primary key(数据);`
         2. 修改：`alter table 表名 modify column 列名 数据类型 primary key;`
         3. 删除：`alter table 表名 drop primary key ;`
      2. 外键 (foreign key)：
         1. 添加：`alter table 表名 add foreign key(列名) references 外键表名(外键列名);`
         2. 删除：`alter table 表名 drop foreign key 外键约束名称;`
      3. 唯一 (unique)：
         1. 添加：`alter table 表名 modify column 列名 数据类型 unique;`
         2. 删除：`alter table 表名 drop index seat;`
         3. 查看：`show index from 表名;`
      4. 其他
         1. 添加：`alter table 表名 modify column 列名 数据类型 约束条件;`
         2. 删除：`alter table 表名 modify column 列名 数据类型 ;`

10. 默认值 

    1. `...default 10`,  `...default null`

----

### 补充知识

 ---- 可不看, 网上采集的:  https://blog.csdn.net/u010002184/article/details/79354136

1. 修改字段类型、字段名、字段注释、类型长度、字段默认值

   1. mysql修改字段类型

      1. 能修改字段类型、类型长度、默认值、注释
      2. 对某字段进行修改
         ALTER  TABLE 表名 MODIFY [COLUMN] 字段名 新数据类型 新类型长度  新默认值  新注释;  # COLUMN关键字可以省略不写

   2. ```
      #正常，能修改字段类型、类型长度、默认值、注释
      alter  table table1 modify  column column1  decimal(10,1) DEFAULT NULL COMMENT '注释';   
      ```

   3. ```
      #正常，能修改字段类型、类型长度、默认值、注释
      alter  table table1 modify column1  decimal(10,2) DEFAULT NULL COMMENT '注释'; 
      ```

   4. mysql修改字段名

      ```
      #模板
      ALTER  TABLE 表名 CHANGE [column] 旧字段名 新字段名 新数据类型;
      ```

      ```
      #正常，此时字段名称没有改变，能修改字段类型、类型长度、默认值、注释
      alter  table table1 change column1 column1 varchar(100) DEFAULT 1.2 COMMENT '注释';  
      
      #正常，能修改字段名、字段类型、类型长度、默认值、注释
      alter  table table1 change column1 column2 decimal(10,1) DEFAULT NULL COMMENT '注释';
      
      #正常，能修改字段名、字段类型、类型长度、默认值、注释
      alter  table table1 change column2 column1 decimal(10,1) DEFAULT NULL COMMENT '注释';
      
      #报错 
      alter  table table1 change column1 column2;
      ```

      ```
      #示例  -- 正确
      alter table white_user change column name nick_name  varchar(50) null comment '昵称';
      ```

2. 修改表名

   1. 
      ALTER TABLE 旧表名 RENAME TO 新表名 ;    #rename to

   2. ```
      #示例  -- ok
      alter table white_user rename to white_user_new ;  
      ```

3. 修改表的注释

   1. ALTER TABLE 表名 COMMENT '新注释';   #模板

   2. ```
      #示例 -- ok
      alter table  white_user_new comment '新表-白名单表' ;
      ```

4. 指定位置插入新字段

   1. ALTER TABLE 表名 ADD [COLUMN] 字段名 字段类型 是否可为空 COMMENT '注释' AFTER 指定某字段 ;

   2. ```
      #示例 - ok
      alter table white_user_new add column erp varchar(50) not null comment 'erp账号' after name ;   #新字段名 erp, 注解 comment 'erp账号'
      ```

   3. ```
      #示例 - ok
      alter table white_user_new add position varchar(50) not null comment '岗位' after name ;  #字段名name后添加新字段position
      ```

5. 删除字段

   1. ALTER TABLE 表名 DROP [COLUMN] 字段名 ;

   2. ```
      #示例 - ok
      alter table white_user_new drop column position ;
      或
      alter table white_user_new drop position ;
      ```


---------

