---
title: hadoop
author: yu
date: '2022-10-15'
slug: []
categories:
  - ~
tags: []
description: ~
featured_image: ~
---
# 修改hadoop-env.sh
```
export JAVA_HOME=JAVA地址
```

# 修改core-site.xml
Hadoop的核心配置文件，目的是配置HDFS地址、端口号、已经临时文件目录。
```
<!-- 默认节点(写自己主机的ip)端口,端口默认为9000 -->
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://hadoop01:9000</value>
</property>
<!-- hdfs的临时文件的目录***这个要记好，后面初始化错误可能会用到***  -->
<property>
   <name>hadoop.tmp.dir</name>
   <value>/opt/hadoop/tmp</value>
</property>
<!-- 其他机器的root用户可访问 -->
<property>
   <name>hadoop.proxyuser.root.hosts</name>
   <value>*</value>
</property>
<!-- 其他root组下的用户都可以访问 -->
<property>
        <name>hadoop.proxyuser.root.groups</name>
        <value>*</value>
</property>

```

# 修改hdfs-site.xml
设置HDFS的NameNode和DataNode两大进程。
```
<!-- 设置数据块应该被复制的份数(和集群机器数量相等,目前只有一个填1) -->
<property>
  <name>dfs.replication</name>
  <value>3</value>
</property>
<!-- namenode备用节点，目前只有一个填主机的IP地址  -->
<property>
  <name>dfs.namenode.secondary.http-address</name>
  <value>hadoop02:50090</value>
</property>

```

# 修改mapred-site.xml
MapReduce的核心配置文件，用于指定MapReduce的运行框架。
默认没有该文件，要将文件mapred-site.xml.template改名为mapred-site.xml得到该文件
```
<!-- mapreduce的工作模式:yarn -->
<property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
</property>
<!-- mapreduce的工作地址 -->
<property>
        <name>mapreduce.jobhistory.address</name>
        <value>hadoop01:10020</value>
</property>
<!-- web页面访问历史服务端口的配置 -->
<property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>hadoop01:19888</value>
</property>

```

# 修改yarn-site.xml
YARN框架的核心配置文件
```
<!-- reducer获取数据方式 -->
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
<!-- 指定YARN的ResourceManager的地址,值为主机名 -->
<property>
    <name>yarn.resourcemanager.hostname</name>
    <value>hadoop01</value>
</property>
<!-- 日志聚集功能使用 -->
<property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
</property>
<!-- 日志保留时间设置7天 -->
<property>
    <name>yarn.log-aggregation.retain-seconds</name>
    <value>604800</value>
</property>

<property> 
    <name>yarn.resourcemanager.webapp.address.rm1</name>  
    <value>hadoop01:8088</value> 
</property>  

<property> 
    <name>yarn.resourcemanager.webapp.address.rm2</name>  
    <value>hadoop02:8088</value> 
</property>
```

# 修改slaves
写入集群内的主机名

# 配置操作系统环境变量（每个节点都做）
1. 返回用户目录即hadoop本身目录
2. 编辑.bash_profile
```
vi ~/.bash_profile
```
3. 追加代码
```
#HADOOP
export HADOOP_HOME=**hadoop文件地址**
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```
4. 重启环境变量
```
source ~/.bash_profile
```

# 复制配置到其他节点
scp -r /opt/hadoop root@slave0:/opt
scp -r /opt/hadoop root@slave1:/opt
