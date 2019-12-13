---
title: ubuntu-mariaDb
date: 2017-07-18 15:31:21
tags:
categories: LINUX

---

#   数据库常用用法
W3C教程 :   https://www.w3cschool.cn/mariadb/
1、创建/删除 数据库：CREATE  DATABASE  数据库名 / drop database 数据库名;
2、建表/删除表 ： h_id为主键   /DROP TABLE  table_name ;
       
    MariaDB [person]> CREATE TABLE home_recycler(
      h_id INT NOT NULL AUTO_INCREMENT,
      h_title VARCHAR(100) NOT NULL,
      h_describe VARCHAR(100) NOT NULL,
      h_imgurl VARCHAR(40) NOT NULL,
      PRIMARY KEY(h_id)
     );
    Query OK, 0 rows affected (0.29 sec)
3、 插入数据

    MariaDB [person]> INSERT INTO home_recycler(h_title,h_describable,h_imgurl) VALUES('1','D','m');

   删除数据,删除全部表的数据就不需要条件了
      
      DELETE FROM home_recycler WHERE product_id=133;

   

注意：插入数据乱码： 在命令行也是插入数据也是中文乱码，解决方法
          https://my.oschina.net/u/1011130/blog/864540
          重启数据库：http://www.cnblogs.com/anseey/archive/2013/04/28/3049785.html
          



# 、  Ubuntu数据库安装
  更新系统

    $ sudo apt update
    $ sudo apt upgrade

 


 安装MariaDB：


     $ sudo apt install mariadb-server

 

 登陆MariaDB命令行


     $ sudo mysql -u root -p

  


 参考：http://blog.topspeedsnail.com/archives/6323
           https://linux.cn/article-6210-1.html

_ _ _

# 、  数据库操作
1.  登录 ：mysql -u root -p  
_ _ _


2. 数据库导出 导入
       在网上查了下，导出命令 千遍一律 都是这样的 mysqldump -u username -p dbname > filename.sql ，按照这个操作，语法错误，   在我的惯性思维中，觉得是命令行登录数据库后再导出，其实是不需要登录的，首先Win + r 输入cmd,进入命令行 输入 mysqldump，如果看到下面这个
       
                   C:\Users\Administrator>mysqldump'
                   mysqldump' 不是内部或外部命令，也不是可运行的程序或批处理文件。
-  说明需要配置环境变量了，类似我的就需要 D:\install\Mariadb\bin；加入path路径后面
   然后输入mysqldump ，

       C:\Users\Administrator>mysqldump
       Usage: mysqldump [OPTIONS] database [tables]
       OR     mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]
       OR     mysqldump [OPTIONS] --all-databases [OPTIONS]
       For more options, use mysqldump --help 


- 然后主题来了

        mysqldump -u root -p person > "C:\mysql.sql"
root是用户名 ， person是数据库，>后面是存放位置 ，标准格式是这样的  mysqldump -u username -p dbname > filename.sql
    
       然后导入：mysql -u username -p dbname < filename.sql
    
       但是我在vps导入后数据库为空
    

参考： https://www.youtube.com/watch?v=2hsOk0XcYC4
           https://john-dugan.com/dump-and-restore-mysql-databases-in-windows/
 http://wqss.2008.blog.163.com/blog/static/912428082010102092548409/    

_ _ _

#  MariaDB 配置远程访问
       有时候在家也要整整这些玩意儿，代码可以用github同步，每次数据库数据还倒腾来倒腾去的 ，如果在vps直接访问就最好了

1.    找到默认配置文件 : 目的是要找到 my.cnf  ， 可以先 " cd /" ，到根目录下  ，输入下面命令
        
           root@TrustingDevoted-VM:/# find -name "my.cnf"
           ./etc/mysql/my.cnf

2.修改my.cnf， 找到【mysqld】 ” bind-address		= 127.0.0.1“ ，前面加个" # "就可以了， 教程说 “skip-networking”前面也需要修改，但是我这里没有就作罢了


3 授予权限 ，这里授予的是183.129.133.0/24段的ip
       
       GRANT ALL PRIVILEGES ON *.* TO 'root'@'183.129.133.%' IDENTIFIED BY '123456' WITH GRANT OPTION;

 4   给远程主机给予连接用户，查询已经存在的远程用户
          
           SELECT User, Host FROM mysql.user WHERE Host <> 'localhost';
                                 
                | User      | Host               |
                +-----------+--------------------+
                | user_name | %                  |
                | root      | 127.0.0.1          |
                | root      | 183.129.133.%      |
                | root      | ::1                |
                | root      | trustingdevoted-vm |
                +-----------+--------------------+


 "user_name" 是给的所有用户授予的 ,"root  暂时不知道怎么删除，     | 183.129.133.%"是刚才添加的，然后 sudo reboot 重启下

5   本地远程登录 

          mysql -h66.168.1.11 -uroot -p123456
    
     注意中间没有空格, -h 后面yourVpsIp ,-u后面 用户名， -p 后面密码

 参考：https://mariadb.com/kb/zh-cn/configuring-mariadb-for-remote-client-access/
       http://blog.csdn.net/ithomer/article/details/6976148
       http://www.jianshu.com/p/9c175e9293e2
       
       
## 　安装Navicat
https://www.jianshu.com/p/42a33b0dda9c