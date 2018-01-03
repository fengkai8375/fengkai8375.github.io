---
layout: post
title: "Hadoop常用配置文件"
date: 2017-12-28 09:00:00 +0800 
categories: Hadoop
tag: Hadoop
---
* content
{:toc}

Hadoop常用的几个配置文件：

### hadoop-env.sh

```shell
# 只修改下面一项即可
export JAVA_HOME=/usr/java/jdk1.8.0_111

# export JAVA_HOME=${JAVA_HOME} 不行
```

<!-- more -->

### core-site.xml
```xml
<configuration>
<property>

  <name>hadoop.tmp.dir</name>

  <value>/usr/hadoop/tmp</value>

  <description>A base for other temporary directories.</description>

  </property>

  <property>

  <name>fs.default.name</name>

  <value>hdfs://ubuntu:9000</value>

 </property>
</configuration>
```

### hdfs-site.xml
```xml
<configuration>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>file:///usr/hadoop/dfs/namenode</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>file:///usr/hadoop/dfs/datanode</value>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>

    <property>
        <name>dfs.nameservices</name>
        <value>hadoop-cluster1</value>
    </property>
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>ubuntu:50090</value>
    </property>
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>
    <property>
      <name>dfs.permissions</name>
       <value>false</value>
    </property>
</configuration>
```

### mapred-site.xml
```xml
<configuration>
<property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
                <final>true</final>
        </property>

    <property>
        <name>mapreduce.jobtracker.http.address</name>
        <value>ubuntu:50030</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>ubuntu:10020</value>
    </property>
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>ubuntu:19888</value>
    </property>
        <property>
                <name>mapred.job.tracker</name>
                <value>http://ubuntu:9001</value>
        </property>
</configuration>
```


### yarn-site.xml
```xml
<configuration>

<!-- Site specific YARN configuration properties -->
<property>
                <name>yarn.resourcemanager.hostname</name>
                <value>ubuntu</value>
        </property>

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.resourcemanager.address</name>
        <value>ubuntu:8032</value>
    </property>
    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>ubuntu:8030</value>
    </property>
    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>ubuntu:8031</value>
    </property>
    <property>
        <name>yarn.resourcemanager.admin.address</name>
        <value>ubuntu:8033</value>
    </property>
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>ubuntu:8088</value>
    </property>
</configuration>
```

