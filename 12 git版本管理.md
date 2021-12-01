#### 总结

1. apt: Advanced Packaging Tool,  Linux的安装包管理工具
2. apt-get， linux命令，Unix和Linux的应用程序管理器, 适用于`deb包管理式的操作系统`， 
3. git 的构成
   1. 三个指针: master, HEAD, 自定义的dev
   2. 分支: 所有分支都在一条时间线上, 指针指向点不同, 抽取的分支线不同
4. 工作目录下的文件两种状态：已跟踪或未跟踪。
   1. 已跟踪的文件 指被纳入版本控制管理的文件，上次快照中有它们的记录，工作一段时间后，它们的状态可能是未更新，已修改或者已放入暂存区。
   2. cd进入工作区, 输入`git  status` 查看文件状态
   3. 工作区新建的文件a.txt是未跟踪状态, 用`git add a.txt`，使其变成已跟踪状态
   4. 若文件a.txt 处于已跟踪，但未暂存状态，用git commit -m无法提交最新版本的a.txt, 要先用 git add a.txt,  然后才能用 git commit -m.  或直接用 `git commit -am `. 即 `git commit -am`是 `git add`与`git commit -m`的组合
   5. 小结: git commit -m 有提交和推送的功能
5. git add 是个多功能命令， 目标文件的状态不同， 命令的效果也不同：
   1. 用它跟踪新文件，或把已跟踪的文件放到暂存区，还用于合并时把有冲突的文件标记为已解决状态等
6. git clone与git pull区别
   1. git clone
      1. 从远程服务器克隆一个一模一样的版本库到本地,复制的是整个版本库，叫做clone.（clone是将一个库复制到你的本地，是一个本地从无到有的过程）
      2. git clone是远程操作的第一步，通常是从远程主机克隆一个版本库，eg: `git clone`  会在本地主机生成一个目录，与远程主机的版本库同名，如指定不同的目录名，可将目录名作为git clone 命令的第二个参数。eg：` git clone  版本库网址 本地目录名`
      3.   git clone支持多种协议，除了http（s),还有ssh,git,本地协议。

   2. git pull
      1. 从远程服务器获取到一个branch分支的更新到本地，并更新本地库，叫做pull.（pull是指同步远程版本库）
      2. git pull : 从远程获取最新版本并merge（合并）到本地   git pull = git fetch + git merge，git fetch更安全一些
      3. git pull 作用是，取远程主机某个分支的更新，与本地的指定分支合并
      4. 完整格式eg: `git pull 远程主机(origin) 远程分支(next) ： 本地分支(master)`, 
         如远程分支是与当前分支合并，则冒号后面的部分可以省略。eg: `git pull 远程主机(origin) 远程分支(next)`# 相当于1. `git fetch origin`  2.`git merge origin/next`




### 1 git简介

#### 1.1 git, 

1. 世界上最先进的 `分布式版本控制系统`
2. 背景: Linus , linux发明者, 手工管理linux代码, 10多年

#### 1.2 git的两大特点

1.  版本控制
   1. 解决 多人开发 问题
   2. 解决找回 历史代码 问题
2. 分布式
   1. Git 分布式版本控制系统
      1. 同一个Git仓库，分布 不同的机器上。
      2. 先找一 电脑 当服务器 ， 24小时开机
      3. 其他人都从 “服务器”仓库克隆一份到自己的电脑上，
         1. 各自的 提交 推送到服务器仓库 
         2. 也可 服务器仓库中 拉取 别人的提交
   2. 可 自己搭建这台`服务器`，也可 用`GitHub网站`

### 2 安装与配置

1. linux中安装
   1.  `sudo apt-get install git`,  
       1.  su root,  切换到root用户
       2.  sudo, 暂时切换到超级用户模式,执行超级用户权限
   2.  输入:`git` , 运行命令, 检验

### git基本操作

#### 3 创建版本库 (仓库)

1.  Ubuntu命令行中操作步骤:
    1.   `mkdir git_test` 新建一个目录 git_test, 
    2.   `cd  git_test`  进入目录
    3.   `ls -al `   //查看目录
    4.   `git init`  //创建一个版本库,
         1.  git_test目录下创建了一个`.git` 隐藏目录, 这就是版本库

2.  工作区, 暂存区, 版本库

    1. 电脑中看到的目录 就是工作区, 编辑文件就在工作区

       如 `git_test` 目录

    2. git的版本库

       1. 工作区里 隐藏目录 `.git` 是版本库
       2. 不是工作区
       3. 内有很多东西
          1. 暂存区 stage/index
          2. 第一个分支 `master分支`, git自动的
          3. 指向master的 一个指针 `HEAD`

    3. 暂存区

       1. stage(或者叫index) 暂存区. 在 git的版本库里
       2. `git add 文件名` #把工作区的文件放入暂存区

    4. `git commit` #暂存区的内容提交到master分支. 暂存区的修改 一次性做一个版本记录

    5. 关系

       ```
       git_test            .git 版本库
        工作区             ┌─────────────────────────────┐
       ┌──────────┐       | 暂存区stage      HEAD        |
       │          │       |                             │
       │          │git add│          git commit         │
       │          │       │┌──────────┐     ┌──────────┐|
       │ code.txt │       |│          │     │ master   │|
       │          │       |│          │     │ 版本2     │|
       └──────────┘       |│          │     │ 版本1     │|
                          |└──────────┘     └──────────┘|
                          └─────────────────────────────┘
       ```

    

#### 4 - 版本创建与回退

1. 版本创建与回退步骤(linux命令窗口)

   1. 创建一个文件

      `touch code.TXT`   #git_test目录下创建 code.txt 

      `vi code.txt`

      `cat code.txt`

   2. `git commit`为文件创建 一个 版本

      ```
      git add code.txt //将本地文件添加到本地的暂存区
      git commit –m '版本1' #'版本说明信息'  //将暂存区的代码提交到本地仓库，再用git push将本地仓库的代码推送到远程服务器端
      ```

      `git commit -am`省略`git add a.txt`这一步，因为git commit -am提交跟踪过的文件

   3. `git log`查看版本记录

      `git log`,   git日志查看提交的提交历史

   4. 继续编辑code.txt

      ```
      vi code.txt
      cat code.txt
      ```

   5. 为文件创建 另 一个 版本

      ```
      git add code.txt
      git commit –m '版本2' #'版本说明信息'
      ```

   6. 再 查看版本记录

      `git log`,  查看git log

      输出:

      ```
      commit ba37943ec8b588b3c121f 79028ce373cd57b7f1e #版本号
      Author: smartliit <smartli i t@163. com>
      Date : Mon Sep 18  13:12:39  2017 +0800
      ```

   7. `git reset`回退到某个版本 

      1. 方法1,输入命令: ` git reset --hard HEAD^`

         1. HEAD 指向当前版本, HEAD^ 前一个版本
         2. HEAD~1 前一个版本,HEAD~100 当前版本的前100版本
      2. 方法2, 返回到指定版本: `git reset --hard 版本序列号`
         1. `git log` 查看git日志
            1. commit `ba37943ec8b588b3c121f 79028ce373cd57b7f1e` #版本号
         2. 终端输入: `git reset --hard ba37943e`  #不必写全版本号
         3. 最后, 再 `git log`  #查看提交的提交历史
      3. git reset --hard与 git reset --soft

         1. `git reset –-soft`：回退到某个版本，只回退了commit的信息，不恢复到index file一级。如果还要提交，直接commit即可
         2. `git reset -–hard`：**彻底回退到某个版本**，本地的源码也会变为上一个版本的内容，撤销的commit中所包含的更改被冲掉

   8. 案例: 关闭终端再开启, 回到之前的版本号方法:

      1. 先查看历史版本记录
      1. `git reflog`  # 看到 `ba37943 HEAD@{3}: commit: 版本2`
         2. `git reflog`  查看所有分支的所有操作记录(提交、回退、已删除的提交操作记录)
   2. 指定版本号回退
         1. `git reset --hard ba37943`  #设置当前版本号
      2. `git log`  #查看提交的提交历史

2. 往版本库 添加 新版本

   1. 分两步(概述)
      1. `git add 文件名.扩展名`   #文件添加,  把文件 添加到暂存区stage
      2. `git commit -m 版本信息`  #把暂存区内容 提交 当前分支master 
   2. 示例
      1. git_test目录下再 建 文件code2.txt
         1. touch code2.txt
         2. vi code2.txt
         3. cat code2.txt
      2. 再编辑 code.txt
         1. vi code.txt
         2. cat code.txt  #显示内容
      3. 查看 工作树 状态: 显示工作目录和暂存区的状态
         1. `git status`   #提示  code.txt被修改, code2.txt没 被跟踪
      4. code.txt和code2.txt 加 到暂存区
         1. `git add code.txt`
         2. `git add code2.txt`
      5. 再查看 工作树(工作目录和暂存区) 状态
         1. `git  status`  #显示工作目录和暂存区的状态
      6. 暂存区的修改 `提交` 分支 创建一个版本
         1. `git commit -m '版本3'`
            1. 提交后, 工作区是 干净的
            2. 命令:  ` git status`  ,看工作树状态, 显示`无文件要提交，干净的工作区`
      7. 查看当前版本
         1. git log  #git日志, 查看提交历史, 当前版本的信息, 如: commit Author Date等等
   
4. `git commit`  只提交 暂存区 的修改创建版本

   1. `git commit -m '版本4'`  #git commit 暂存区的`修改`提交到版本仓库
   
5. `git checkout` 撤销修改 (工作区的修改)

   1. 工作区 撤销
      1. `git checkout -- code.txt`  #工作区的改动, 直接丢弃. code.txt(文件名)
   2. 暂存区撤销 (即未commit), 2 步
      1. ` git reset HEAD code.txt`   #暂存区的修改撤销,  放回工作区
      2. `git checkout -- code.txt`  #工作区的改动, 直接丢弃
   3. 已 提交到版本库的撤销用版本回退方法处理
   4. 小结
      1. 撤销工作区 修改 ( 场景1)  `git checkout -- 文件名`   
      2. ( 场景2) 暂存区撤销修改, 两步
         1. git reset HEAD file  回到了场景1
         2. 按场景1操作, 丢弃工作区 修改
      3. ( 场景3) 修改已保存到版本库, 用版本回退方法
   
6. `git diff` 对比版本的不同

   1. 工作区和版本库对比 

      1. ` git diff HEAD -- code.txt `   #对比工作区code.txt和HEAD版本code.txt的不同

         1. 显示:

            --- a/code. txt,      `-` 代表HEAD版本中 code.txt的内容

            +++ b/code. txt,     `+`代表工作区中 code.txt的内容

   2. 两个版本的文件的对比

      1. `git diff HEAD HEAD^ -- code.txt`   #code.txt的HEAD版本与HEAD^版本不同

         1. 显示:

            --- a/code. txt,       `-` 代表HEAD版本中 code.txt的内容

            +++ b/code. txt     `+`代表HEAD^中 code.txt的内容

6. 删除文件: 两种场景:

   1. (工作区)目录中 code2.txt 删除: `rm`
      1. `rm code2.txt`  // 等价于windows`右键删除`
         1. 这时，git知道删除了文件. 工作区和版本库不一致了
         1. `git status` 显示哪些文件被删除了
      1. 后续: `git add code2.txt`   //将删除动作添加到 暂存区
      1. 最后: `git commit -m “delete code2.txt`  //将删除动作提交到版本库
   2. 版本库中删除该文件: `git rm`
      1. 先 `git rm code2.txt`     //删除工作区中的文件, 同时将删除动作添加到暂存区; 
         1. git rm 相当于 rm + git add 两个命令
      2. 再 `git commit -m '删除文件code2.txt'`   //将删除动作提交到版本库，版本库中该文件记录也被删除
   3. 删错了 找回
      1. `git checkout – code2.txt`  #找回
   4. 小结
      1. 文件已 提交到版本库， 不用担心误删
      2. 但只能恢复 到最新版本, 会丢失**最近一次**提交后你修改的内容
   5. git rm -f 参数介绍
      1. `git rm  -f`  , 表示强制删除 工作区中 的文件, 并将删除添加到暂存区；
      2. 工作区中的文件修改后，用git add 添加到暂存区后， 用 `git rm -f ` 表示强制删除 工作区中和暂存区 中的文件，并将删除添加到暂存区；

7. echo

   1. echo: 在显示器上显示一段文字，起提示作用。也可在文件中写的内容。也可脚本编程时显示某一个变量的值，或直接输出指定的字符串。
   2. 语法是： echo [选项] [字符串]
      1. 两个选项： -n 和 -e
      2. -n :  输出后不换行，直接显示新行的提示符
      3.  -e : 转义字符按对应的方式进行处理

   

-----------

#### 5- git分支管理

1. master分支,  master指针 

   1. 分支: 像 平行宇宙, 互不干扰. 
   2. 自己的分支, 别人看不到
      2. 在自己分支上编程，想提交就提交
      3. 开发完毕后，一次性合并到 master分支 . 
         1. master分支 用来运行 工作 
   3. 指针HEAD-->master分支, 指针HEAD-->dev分支, 
      1. HEAD 指针, 指向master分支,或指向dev分支
      2. master指针, 指向一个版本节点
      3. dev指针, 指向一个版本节点
   
2. 创建与合并分支

   1. 分支

      1. 每次提交的版本串成的一条时间线
   
      2. 主分支，即`master分支`. 其对应的 `master指针`, 指向 最新提交的版本
   
      3. `HEAD`指针 指向 当前分支, `HEAD`不是指向提交的版本. master 指向提交的版本
   
         
   
   2. checkout 切换分支或恢复工作树文件
   
      1. 创建新分支：git branch branchName
   
      2. 切换到新分支：git checkout branchName
   
      3. 上面两个命令也可以合成为一个命令
   
         `git checkout -b branchName`
   
   3. 创建,合并, 删除 分支的理论:
   
      1. 创建 分支过程(描述)
         1. 最初, master分支 是 `一条线`， 指针master指向 最新的提交. 指针HEAD指向指针master.
            1. 每次提交，master分支向前移动一步. 不断提交，master分支线越来越长
            2. `master分支` 是分支,  `master`是 指针
         2. 创建 新dev分支，同时也新建 `dev`指针, 
            1. 指针dev指向 master相同的提交
            2. 再把HEAD指向dev， 则当前分支是dev分支
         3. 现在, 工作区的修改和提交在dev分支上进行
            1. 如新提交一次，dev指针 前移一步
            2. master指针不动
      2. 合并 分支(描述)
         1. 直接把 指针`master`指向 分支dev的当前提交， 即完成 合并
            1. git合并分支很快: 因为只改指针的指向, 工作区内容 不变
      3. 删除分支dev
         1. 把指针dev 删掉. 只剩下 `master分支`
      4. 小结:
         1. 开始: 分支 master, 指针master, 指针HEAD, 
         2. 然后: 新建分支 dev和指针dev. 指针HEAD指向指针dev
   
   4. 创建,合并, 删除 分支 : 代码示例
   
      1. 代码 实操( 创建dev分支, 合并到master分支)
         1. git branch  #查看当前有几个分支并且看到在哪个分支下工作
         
         2. git checkout -b dev   #创建一个分支dev并切换到其上进行工作
         
         3. git branch #看当前有几个分支, 发现指针master,dev指向同一个分支, 指针HEAD指向指针dev
         
         4. 修改code.txt内容，在里面添加一行，并进行提交
         
            ```
            vi code.txt   #修改code.txt
            cat code.txt  #显示内容
            git add code.txt  #添加到暂存区stage
            git commit -m 'dev分支提交'  #提交到仓库, 并添加提交信息
            ```
         
         5. git checkout master  #dev分支工作完成，切换回master分支, 指针HEAD指向指针master
         
         6. git branch  #查看 分支 工作分支
         
         7. git merge dev  #dev分支的工作成果合并到master分支上
            1. Fast-forward  “快进模式”
            2. master 直接指向dev的当前提交, 非常快。
            2. git merge dev  #合并dev分支到当前master分支
            
         8. git branch -d  dev  #删除dev分支
            1. 合并后，放心地删除dev分支
            2. git branch  #查看branch，只剩下master分支
         
      2. 小结
         1. `git brancn`  # 查看当前有几个分支, 在哪个分支 工作
         2. `git checkout -b dev`  #创建 分支dev, 并切换到dev
         3. 分支操作 命令
   
            1. git branch  #查看分支
            2. git branch dev(分支名)  #创建分支
            3. git checkout dev(分支名)  #切换分支
            4. git checkout -b dev(分支名)  #创建并切换 dev分支
            5. git merge dev(分支名)   #合并分支
            6. git branch -d dev(分支ming)   #删除分支
   
   5. 分支冲突
   
      1.  两个分支都修个了同一个文件,且提交, 无法'快速合并',  必须手动处理
      2. 示例
         1. 创建一个新分支dev, `git checkout -b dev`
         2. 在dev分支, 修改code.txt，并提交,  
            1. vi code.txt
            2. cat code.txt
            3. git add code.txt  //提交到暂存区stage
            4. git commit -m 'dev分支提交2'   //从暂存区提交到本地仓库, 
         3. 切换回 master分支, `git checkout master`
         4. 在master分支, 修改code.txt, 并提交
            1. vi code.txt
            2. cat code.txt
            3. git add code.txt
            4. git commit -m 'master提交'
            5. master分支和dev分支 分别有新的提交, 
               1. dev指针 指向 dev分支
               2. master指针 指向 master分支
               3. 此时, dev分支 与 master分支 **分开**了
         5. git merge dev # dev分支合并到master分支, 合并不成功(因为master分支和dev分支 分别有新的提交)
            1. 此时, 无法“快速合并”. 要手动解决冲突后再提交
         6. git status;  #显示工作树状态
         7. git log --graph --pretty=oneline  #git日志, 带参数的分支查看git log命令, 查看合并情况
         8. git branch -d dev  #工作完成, 删除dev分支
   
   6. 分支合并模式
      1. 快速合并模式策略1: 合并时, 如 可用`fast forward`, 则用之
      2. 禁止快速合并模式策略2: , 禁止快速合并,  `-no-ff`
         1. git checkout -b dev  #创建并切换到新dev分支
         2. 修改code.txt, 提交
            1. vi code.txt
            2. git add code.txt
            3. git commit -m '添加新行'  #提交
         3. git checkout master  #切换回master分支
         4. `git merger --no-ff -m '禁止fast-forward合并' dev`  #合并dev分支, `--no-ff`参数 禁用Fast forward
         5. git log  #看分支日志
   
   7. git stash 工作现场保存至堆栈

      1. 情景:
   
         1. 接到 修复代号001的bug的任务时.解决方案: 创建一个分支bug-001修复它
         2. 同时正在进行的 dev的工作还没提交, 工作进行到一半， 没法提交
         3. 策略: 
            1. 每个bug 用一个新的 `临时分支` 修复
            2. 修复后 合并 分支，后 删除 临时分支

      2. 解决方案

         1. 方案: `git stash` 把工作现场保存至堆栈，以后`git stash apply`恢复现场工作
            1. git stash解析
               1. `git stash`  //将未提交的修改（工作区和暂存区）保存至堆栈，用于后续恢复当前工作目录. 
                  1. stash中的内容可以恢复到原先开发的分支，也可以恢复到其他任意指定的分支上。
                  2. git stash作用的范围包括工作区和暂存区中的内容，也就是说没有提交的内容都会保存至堆栈中
               2. stash是本地的，不会通过`git push`上传到git server上。
               3. 实际应用: `git stash save`取代`git stash`命令,给stash加一个message，用于记录版本。示例如下：
                  1. git stash save "修改了index文件"
                  2. git stash list
               4. `git stash pop`   恢复缓存的工作目录. 将缓存堆栈中的stash删除，并将对应修改应用到当前的工作目录下
               5. ` git stash apply`  将缓存堆栈中的stash多次应用到工作目录中，并不删除stash拷贝
   
      3. 解决方案的步骤
   
         1. 先 `git  stash`  //把工作现场保存至堆栈
   
         2. 再确定bug所在分支.  如在`master分支`上, 在master创建临时分支
   
            1. `git checkout master`  #切换到master分支
   
            2. `git checkout b bug-001`  #在master创建临时分支并切换到临时分支
   
         3. 在临时分支bug-001上修复bug, 并提交
   
            1. `vi code.txt`  
            2. `cat code.txt`
            3. `git add code.txt`
            4. `git commit -m '修复bug-001'`
   
         4. 切换到`master分支`，合并，删除bug-001分支
   
            1. `git checkout master`   //当前所在分支master
            2. `git merge --no-ff -m '修复bug-001' bug-001`   //bug-001分支合并到当前分支master
            3. `git branch -d bug-001`  #删除bug-001
   
         5. 回到dev分支干活, `git checkout dev`
   
         6. 查看工作现场, `git stash list`,  查看现有stash
   
            1. ```
               $ git stash list
               
               @{0}: WIP on master: 049d078 added the index file stash
               
               @{1}: WIP on master: c264051 Revert "added file_size" stash
               
               @{2}: WIP on master: 21d80a5 added number to log
               ```
   
            2. `git stash apply` 可以通过名字指定使用哪个stash，默认用最近的stash（即stash@{0}
   
         7. 恢复工作现场: `git stash pop`,   恢复缓存的工作目录
   
      4. 小结
   
         1. 创建新的bug分支修复bug，然后合并，最后删除bug分支
         2. 手头工作没有完成时
            1. 先 `git stash`工作现场 
            2. 去修复bug
            3. 修复后，再`git stash pop` 恢复工作现场
   
   8. git stash其他操作
   
      1. 移除stash,
         1.  `git stash drop`命令，后面可以跟着stash名字, 如 stash@{0} , `git stash drop stash@{0}`
         2. 或用`git stash clear`命令，删除所有缓存的stash
   
      2. 查看指定stash的diff
         1. `git stash show` 命令，后面可以跟着stash名字
         2. 命令后面添加`-p`或`--patch`可以查看特定stash的全部diff
   
      3. 从stash创建分支
         1. `git stash branch`，这会创建一个新的分支，检出你储藏工作时的所处的提交，重新应用你的工作，如果成功，将会丢弃储藏
   
      4.  暂存未跟踪或忽略的文件
         1. 默认情况下，git stash会缓存下列文件：
            1. 加到暂存区的修改（staged changes）
            2. Git跟踪的但并未添加到暂存区的修改（unstaged changes）
   
         2. 但不会缓存一下文件：
            1. 在工作目录中新的文件（untracked files）
            2. 被忽略的文件（ignored files）
   
         3. git stash命令提供了参数用于缓存上面两种类型的文件。
            1. 使用-u或者--include-untracked可以stash untracked文件。
            2. 使用-a或者--all命令可以stash当前目录下的所有修改
   
   
   

----

#### 分布式 电脑网络结构: 

​	工作电脑(小明)<---推送push/拉取pull-->中央服务器 <--推送push/拉取pull-->工作电脑(小红)

项目经理/项目组代码管理员 工作:

1. 创建仓库`repository`
2. 添加`SSH`公钥

#### 6 - 使用github

1. 创建仓库
   1. 注册github账号. 注册邮箱, 用户名, 

   2. 创建仓库 repository.  (repository: 储藏室, 仓库; 知识库, 智囊团)

      1. 登录, New repository

      2. 新页面, 输入`项目名`, 勾选'readme.md', 点击`create repository, `

         ```
         新页面内容:
          Create a new repository
          Owner, Repository name : test #(项目名)
          Description(optional)
          pulic #共有(选)
          private #私有的(花钱)
          选择: Initialize this repository with:
          1. Add a README file
          2. Add .gitignore
          3. Choose a license
         Create repository #(点击)
         ```

      3. 添加成功后，转到文件列表页, 

         1. 修改 `.gitignore `, 要忽略的文件
      
            *.pyc

2. 添加SSH公钥 步骤: 

   1. github网站:

      1. 账户头像 后的 下拉三角，选择'settings'  
         1. 某电脑与github的仓库交互, 要把这台电脑的 `ssh公钥` 添加到 `github账户`
      2. 选项 `SSH and GPG keys`, 选择之，再点击 `New SSH key` 公钥。

   2. 用户电脑: 生成公钥步骤

      1. ubuntu命令行, 进入用户主目录, 编辑` .gitconfig`文件  修改机器的`git配置`. 代码操作如下: 

         1. `cd`   #回到用户主目录

         2. `vi .gitconfig`  #编辑` .gitconfig`文件  修改机器的`git配置`

            ```
            [user]
            	email = smartli_it@163.com  #注册github的邮箱
            	name = smartliit  #github用户名
            ```

            1. `.gitconfig`文件的作用: 标记 向github推送/拉取的用户

      2. ubuntu命令行, 生成ssh密钥

         1. ```
            ssh-keyg -t rsa -C "smartli_it@163.com" #注册github的邮箱,
            #提示 ssh-key: 未找到命令
            ```

         2. ```
            ssh-keygen -t rsa -C "smartli_it@163.com" #注册github的邮箱
            
            overwrite(y/n) y #输入y
            ```

         3. 主目录下的.ssh文件件，下面有两个文件

            公钥为id_rsa.pub

            私钥为id_rsa

         4. 查看公钥,密钥.  

            1. 进入主目录 `cd ~/.ssh`文件夹, 
            2. id_rsa.pub  #公钥文件
            3. id_rsa  #私钥文件
            4. 查看公钥 ，复制之
               1. `cat id_rsa.pub`

   3. 回到 `浏览器` 黏贴至key, 填写Title `smart`(自己定义)

      1. 点击 `Add SSH key`

   

3. git clone项目 步骤:

   1. 浏览器
      1.  github首页, 再进入 `项目仓库` 的页面, 选中`test`项目
      2. 选中Code下拉列表, 选Clone>SSH
         1. 复制 git 地址 `git@github.com:smartliit/test.git`
   2. 个人电脑端
      1. linux 命令行   `git clone git@github.com:sartliit/test.git`,  克隆出错
         1. 接方案(linux 命令行) 步骤: 
            1. `eval '$(ssh-agent -s)'`
            2. `ssh -add`
      2. 命令行中 粘贴仓库中 内容
         1. linux命令行   `git clone git@github.com:sartliit/test.git`

4. 个人电脑端开发步骤: 

   1. 项目克隆到本地 后，创建 `smart分支`. 自己的分支smart, 

      1. 创建自己的 `开发分支`, 不要在 `master分支`上开发

      2. ```
         git checkout -b smart #开发分支smart, 自己的开发分支
         git branch  #查看所有的分支
         #每天在自己的 `smart分支`上开发, 
         ```

   2. 创建 code.txt并提交 版本 (在自己的分支上创建文件)

      1. touch code.txt  //smart分支上创建文件code.txt
      2. vi code.txt   //编辑code.txt
      3. git add code.txt   //工作区提交到stage
      4. git commit -m '版本1'   // stage的改变提交到本地仓库

   3. git push origin 推送到github网站的仓库

      1. 个人电脑ubuntu命令行: 推送分支， 把该分支上的 `所有本地` 提交到远程库

         1. 推送时指定 本地分支. 这样，git 把该分支推送到远程库对应的 远程分支
         2. 方法: `git push origin smart`  #本地smart分支推送至远程库, smart 本地分支名

      2. 推送前, github网站的仓库

         1. 推送前: 

            1. 例: 1branch

            2. 分支列表  

               ```
               Default branch
               master Updated an hour ago by smartliit
               ```

      3. 推送后, github网站的仓库

         1. 列 2 branchs

            ```
            Default branch
            
            Your branches
            
            Active branches

         2. 分支页面

            ```
            smart(1 minute ago)
            ......
            ```

   4. 小结

      1. `git branch 自己的分支名`
      2. 自己的分支上创建文件
      3. `git push origin 自己的分支名`

5. 本地分支smart 跟踪 服务器(github网站)分支smart

   1. 语法: git branch --set-upstream-to=origin/远程分支名称  本地分支名称

      ```
      git branch --set-upstream-to=origin/smart smart
      ```

6. 从服务器(github网站)分支smart 拉取代码

   1. 语法: git pull origin 分支名

      `git pull origin smart`  #远程主机（origin）, 远程分支名smart, 

      1. 把远程分支smart上的代码 下载 并 合并 到本地所在分支

7. 小结: 

   1. 服务器(github网站)上建项目 test
   2. `git clone git@github.com:sartliit/test.git`  从服务器(github网站)clone项目
   3. `git branch -b smart`  创建自己分支smart
   4. `git push origin smart`  推送自己的分支
   5. `git pull origin smart` 从服务器拉取代码


#### 7 git工作流程

1. 项目经理 工作内容
   1. 搭建框架
      1. 几个应用
      2. 用到的模型类
   2. 项目框架代码 放到服务器
2. 普通员工 工作内容
   1. 自己电脑, 生成SSH公钥,  给项目经理. 他添加它到 服务器
   2. 自己下载 代码 到自己电脑 (每个组员的项目代码的 地址  是项目经理给的)
   3. 自己在自己电脑创建 本地 `dev分支`,  在自己电脑的 `dev分支` 中 开发(编写代码)
   4. 自己在自己电脑上开发完代码后, push到远程的 dev分支 上
3. 分支 用途 不同
   1. master: 保存 `发布项目代码`, V1.0, V2.0
   2. dev: 保存 `开发过程代码` 





