1. **搭建一个MySQL高可用架构集群环境（\**4台主机，\**1主、2从、1 MHA）**

2. 1. 首先实现一主两从的同步复制功能（采用半同步复制机制）

   在主库服务器上，修改/etc/my.cnf配置文件

   添加配置

   log-bin=/var/log/mysql-bin.log

   server-id=1

   `注意：修改之后启动出现问题，提示权限不足，需要给日志文件所在的文件夹授权，chown mysql.mysql -R ***`

   ![image-20200620150351063](C:\Users\myang\AppData\Roaming\Typora\typora-user-images\image-20200620150351063.png)

   使用show master status，得到主库上当前的二进制日志名和偏移量。这个操作的目的是从库启动之后，从某一个节点复制数据

   `有些库时不需要同步到其他节点上，所以在配置文件中配置需要过滤掉的数据库，binlog-ignore-db=mysql`

   主库进行授权：

   `grant replication slave on *.* to 'root'@'%' identified by '123456'`

   `grant all privileges on *.* to 'root'@'%' identified by 'root'`

   `flush privileges`

   

   从库配置

   server-id=101

   read-only=1

   ` change master to master_host='xxx',master_port=3306,master_user='root',master_password='123456',master_log_file='mysqld-bin.000003',master_log_pos=''`  

   #### 遇到的问题

   - Slave_IO_Running: No，解决方法：如何定位为题？查看mysql的日志，cat /var/log/mysqld.log,发现两台主机的uuid相同。查询命令找此auto.cnf修改uuid即可，find / -name auto.cnf

     ![image-20200620210941497](C:\Users\myang\AppData\Roaming\Typora\typora-user-images\image-20200620210941497.png)

   1. 然后采用MHA实现主机出故障，从库能自动切换功能

   2. MHA高可用搭建后，在主库新建商品表进行效果测试

   3. 在拉勾业务中职位表相当于电商系统的商品表，投递表相当于电商系统的订单表。职位表我们采用垂直拆分方法分为position（职位描述表）和 position_detail（职位详情表），表结构结构如下：

   4. **position：id(int)、name(varchar)、salary(varchar)、city(varchar)**

   5. 

   6. **position_detail：id(int)、pid(int)、description(text)**

   7. 作业需要提交集群环境搭建手册和效果演示视频

      手册：包含环境软件版本和架构介绍、环境安装过程、操作的问题和注意事项等。

      视频：仅录制环境介绍和效果演示。












--------------

---------



1. **搭建一个MySQL高可用架构集群环境（\**4台主机，\**1主、2从、1 MHA）**

2. 1. 首先实现一主两从的同步复制功能（采用半同步复制机制）

````
grant replication slave on *.* to 'root'@'%' identified by '123456'

grant all privileges on *.* to 'root'@'%' identified by 'root'

flush privileges
````

1. 1. 然后采用MHA实现主机出故障，从库能自动切换功能

   2. MHA高可用搭建后，在主库新建商品表进行效果测试

   3. 在拉勾业务中职位表相当于电商系统的商品表，投递表相当于电商系统的订单表。职位表我们采用垂直拆分方法分为position（职位描述表）和 position_detail（职位详情表），表结构结构如下：

   4. **position：id(int)、name(varchar)、salary(varchar)、city(varchar)**

   5. 

   6. **position_detail：id(int)、pid(int)、description(text)**

   7. 作业需要提交集群环境搭建手册和效果演示视频

      手册：包含环境软件版本和架构介绍、环境安装过程、操作的问题和注意事项等。

      视频：仅录制环境介绍和效果演示。

2. \----------------------------------------------------------------------------

3. 作业资料说明：

4. 

5. 1、提供资料：说明文档、验证及讲解视频。

6. 2、讲解内容包含：题目分析、实现思路、环境介绍。

7. 3、说明文档包含：

8. l 环境软件版本、架构介绍

9. l 环境安装过程（各个配置加注释）

10. l 操作过程中遇到的问题

11. l 操作注意事项

12. 4、效果视频验证：

13. l 集群环境

14. \* 4台

15. \* 1主，2从，1MHA

16. 一主两从的半同步复制功能

17. MHA实现主机出故障，从库能自动切换功能

18. l 环境介绍

19.  \* 介绍涉及的各个软件的版本

20.  \* 介绍各个机器对应角色&作用&ip地址

21. l 主库新建商品表

22. l 添加数据，演示半同步复制

23. ​    \* 先查询主从库数据

24. ​    \* 添加后，再次查询主从库数据

25. ​    \* 查看log，是否显示半同步

26. l 主机出故障，从库能自动切换功能

27.   \* 关闭 主服务器，查看MHA服务器是否正常切换

28. App1:mysql Master failower 之前主ip(之前主ip:3306) to 切换后ip(切换后ip:3306) succeeded

29.   \* 在切换后的主节点机器上查看状态展示效果

30.   \* 在切换后的主节点机器数据库添加数据，分别查询主从库数据一致

31. \------------------------------------------------------------------------------------------------------------