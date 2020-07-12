1. #### Mysql集群环境

   

   | 服务器用途         | ip地址         |
   | ------------------ | -------------- |
   | master0节点        | 192.168.255.70 |
   | master0-slave1节点 | 192.168.255.71 |
   | master0-slave2节点 | 192.168.255.72 |
   | master1节点        | 192.168.255.80 |
   | master1-slave1节点 | 192.168.255.81 |
   | master1-slave2节点 | 192.168.255.82 |

   #### 1、搭建mysq集群服务

   集群搭建参考Mysql上作业，此处不占用文档说明

   #### 2、配置c_order数据源规则

   ![image-20200713011144385](C:\Users\myang\AppData\Roaming\Typora\typora-user-images\image-20200713011144385.png)

   

   此处c_order是公司端查询比较多，所以根据company_id将同一个公司的id集中在一个数据库中，而数据源中的表是根据user_id进行分表的。

   #### 3、配置读写分离

   ![image-20200713011315012](C:\Users\myang\AppData\Roaming\Typora\typora-user-images\image-20200713011315012.png)

配置了两个主节点，master0和slave1,数据节点配置为master0和master1,而读节点配置为slave01,slave02,slave11,slave22