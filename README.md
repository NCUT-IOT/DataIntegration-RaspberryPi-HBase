# DataIntegration-RaspberryPi-Hbase

Happybase介绍

# 1、Happybase是连接hbase的Python库，官方介绍如下：

HappyBase is a developer-friendly Python library to interact with Apache HBase. HappyBase is designed for use in standard HBase setups, and offers application developers a Pythonic API to interact with HBase. Below the surface, HappyBase uses the Python Thrift library to connect to HBase using its Thrift gateway, which is included in the standard HBase 0.9x releases.

# 2、安装happybase和thrift

安装happybase：sudo pip install happybase
安装thrift： sudo pip install thrift(可能安装不成功，需要进入hdp的hbase目录下找到thrift 编译安装，具体过程可以百度，资料挺多)
注：安装thrift这一步可以跳过，具体行不行我没有尝试，happybase官方文档描述如下：Generating and installing the HBase Thrift Python modules (using thrift --gen py on the .thrift file) is not necessary, since HappyBase bundles pregenerated versions of those modules.


测试是否安装成功
进入Python环境：import happybase
如果没有报错则安装成功。


# 3、Happybase操作hbase使用简介

（1）首先在hdp集群开启Hadoop平台的HadoopMaster的thrift服务，命令如下：
hbase thrift start
如果想关闭终端之后，thrift服务继续运行，可以用daemon模式运行：
nohup hbase thrift start &

（2）建立连接
import happybase
connection = happybase.Connection('somehost')

（3）建立连接之后查看可以使用的表：
print connection.tables()

（4）创建表：
connection.create_table(
    'mytable',
    {'cf1': dict(max_versions=10),
     'cf2': dict(max_versions=1, block_cache_enabled=False),
     'cf3': dict(),  # use defaults
    })

（5）获取一个table实例
table = connection.table('mytable')
（6）存储数据：Hbase里 存储的数据都是原始的字节字符串
cloth_data = {'cf1:content': u'牛仔裤', 'cf1:price': '299', 'cf1:rating': '98%'}
hat_data = {'cf1:content': u'鸭舌帽', 'cf1:price': '88', 'cf1:rating': '99%'}
shoe_data = {'cf1:content': u'耐克', 'cf1:price': '988', 'cf1:rating': '100%'}
author_data = {'cf2:name': u'LiuLin', 'cf2:date': '2017-03-09'}

table.put(row='www.test1.com', data=cloth_data)
table.put(row='www.test2.com', data=hat_data)
table.put(row='www.test3.com', data=shoe_data)
table.put(row='www.test4.com', data=author_data)
（7）扫描一个table里的数据
 全局扫描一个table
for key, value in table.scan():
    print key, value


# 4、Happybase文档：
Installation guide — HappyBase 1.1.0 documentation  https://happybase.readthedocs.io/en/latest/installation.html
