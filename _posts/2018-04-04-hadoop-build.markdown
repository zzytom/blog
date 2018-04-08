---
layout: post
title: hadoop单机版搭建
date: 2018-04-04 17:32:20 +0300
description: 
img: hadoop-st.png # Add image post (optional)添加图像帖子(可选)
tags: [hadoop]
---
&#160; &#160; &#160; &#160;安装版本为2.7.5,系统为centos 7  
解压到/usr/hadoop/hadoop-2.7.5目录下  
在/usr/hadoop下创建tmp和hdfs文件夹  
修改/usr/hadoop/hadoop-2.7.5/etc/hadoop下的部分配置文件  
修改core-site.xml文件添加内容如下   
```xml
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
修改hdfs-site.xml文件  
```xml
<configuration>
<property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/data/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/data/hadoop/tmp/dfs/data</value>
    </property>
</configuration>
```
修改yarn-site.xml文件  
```xml
<configuration>
   <property>
       <name>yarn.nodemanager.aux-services</name>
       <value>mapreduce_shuffle</value>
   </property>
</configuration>
```
如果报找不到JAVA_HOME去修改hadoop-evn.sh文件中的配置填好jdk路径
```
  export JAVA_HOME=${}
```
修改mapred-site.xml文件,此文件原文件名为mapred-site.xml.template,为了启动yarn
```xml
<configuration>
   <property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
   </property>
</configuration>
```
修改/etc/profile文件
```
# set hadoop path
export HADOOP_HOME=/usr/hadoop/hadoop-2.7.5
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
```
然后执行source /ect/profile 重新加载配置文件  
启动hadoop  
启动：分别启动HDFS和MapReduce

命令如下：start-dfs.sh start-mapreted.sh

命令如下：stop-dfs.sh  stop-mapreted.sh

第二种方式

全部启动或者全部停止

启动：

命令：start-all.sh

启动顺序：NameNode，DateNode，SecondaryNameNode，JobTracker，TaskTracker

 
停止：

命令：stop-all.sh

关闭顺序性：JobTracker，TaskTracker，NameNode，DateNode，SecondaryNameNode  
如果报 Unable to load native-hadoop library for your platform... using builtin-java classes where applicable  
说明http://dl.bintray.com/sequenceiq/sequenceiq-bin版本不对去该网址下载分别解压到/hadoop-2.7.5/lib和/navtive下