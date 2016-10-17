title: svn命令行
summary: svn搭建与使用
authors: Tyler
publish_date: 2016-10-017
tags: command line 

# 了解更多使你更自由---svn搭建与使用

##1 Mac 下创建svn 仓库

    $ svnadmin create /Users/MyMac/workspace/svn/test_svn_local

创建后发现多了这些文件


    $ls

    README.txt conf db format hooks locks

##2 修改配置文件

    $vim conf/senserve.conf

修改anon-access=read(若none表示需要用户登录，read表示匿名登录为只读），auth-access=write, password-db=passwd, authz-db=zuthz
 '#'表示的是注释
 去掉＃ 和空格

添加用户

打开passwd文件

在［user]下面顶格写 root=root 这里的配置的帐号和密码 前面的帐号 后面的为密码格式如下

    [user]

    root=root

前面是用户后面是密码

配置组

打开authz配置权限

    [groups]

    mygroup = root, admin

文件后面加上用户的配置组的文件夹

    [/]

    @mygroup = rw

##3 启动服务器

    $ svnserve -d -r ~/workspace/svn/test_svn_local

##4 导入文件

    $ svn import /Users/MyMac/workspace/svn/test_svn_code svn://localhost/svn/test_svn_local/code/hehe --username=root --password=root -m "解释文本”

查看文件是否导入成功

    $ svn ls svn://localhost/svn/test\_svn\_local

##5 检出仓库

    $ svn checkout svn://localhost/svn/test_svn_local/code --username=root --password=root ./demo

##6 添加一个文件并进行一次提交
创建文件test.txt
    $ touch test.txt
将文件添加到svn
    $ svn add test.txt
提交到远程
    $ svn commit -m " some message"

##7 修改文件并提交
在文件后增加一些文本
    $ echo "something" >> test.txt
查看区别
    $ svn diff

    Index:test.txt
    --- test.txt	(revision 4)
    +++ test.txt	(working copy)
    @@ -0,0 +1 @@
    +something
查看当前工作区的状态
    $ svn status
    M       test.txt
提交修改
    $ svn ci -m "add something to test.txt"

#8 查看日志
    $ svn log

#9 版本回退
##9.1当修改没有提交到远程时

回退文件

    $ svn revert filename

回退文件夹 加上-R参数 表递归 recursive

    $ svn revert -R dirname

这有点类似于git中的checkout 

##9.2 当修改已经提交到远程时
###9.2.1 首先更新代码为最新

    $ svn update

###9.2.2 找出要会滚的版本号

    $ svn log test.txt
    ------------------------------------------------------------------------
    r5 | root | 2016-10-17 22:03:17 +0800 (一, 17 10 2016) | 1 line

    add something to test.txt
    ------------------------------------------------------------------------
    r4 | root | 2016-10-17 21:52:13 +0800 (一, 17 10 2016) | 1 line

    add a test.txt
    ------------------------------------------------------------------------
查看两个版本号之间的详细差别

    $ svn diff -r 5:4
    Index: test.txt
    ===================================================================
    --- test.txt	(revision 5)
    +++ test.txt	(revision 4)
    @@ -1 +0,0 @@
    -something



### 9.2.3 回滚并提交

回滚到版本4

    $ svn merge -r 5:4 test.txt

确认是否回退到正常的版本

    $ svn diff
    Index: test.txt
    ===================================================================
    --- test.txt	(revision 5)
    +++ test.txt	(working copy)
    @@ -1 +0,0 @@
    -something

提交到远程服务器

    $ svn ci -m "Revert version from 5 to 4,because of ..."







 
