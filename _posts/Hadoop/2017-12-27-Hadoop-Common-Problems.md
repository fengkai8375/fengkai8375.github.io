---
layout: post
title: "Hadoop配置和运行中常见问题"
date: 2017-12-27 09:00:00 +0800 
categories: Hadoop
tag: Hadoop
---
* content
{:toc}

#### hadoop配置顺序

0. 安装配置JDK  *(not openjdk)*
1. 下载解压
2. 配置环境变量，并 ```source /etc/profile```
3. 修改配置文件
4. 配置免密登录，启动 ```sshd```
4. 格式化hdfs
5. 启动hdfs和yarn


#### 环境变量

```shell
# java
export JAVA_HOME=/usr/java/jdk1.8.0_111
export JRE_HOME=/usr/java/jdk1.8.0_111/jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
# hadoop
export HADOOP_HOME=/usr/hadoop  
export PATH=${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin${PATH}  
export HADOOP_MAPRED_HOME=${HADOOP_HOME}  
export HADOOP_COMMON_HOME=${HADOOP_HOME}  
export HADOOP_HDFS_HOME=${HADOOP_HOME}  
export YARN_HOME=${HADOOP_HOME}  
export HADOOP_COMMON_LIB_NATIVE_DIR=${HADOOP_HOME}/lib/natvie    
export HADOOP_OPTS="-Djava.library.path=${HADOOP_HOME}/lib:${HADOOP_HOME}/lib/native" 

```

<!-- more -->

#### 运行hadoop服务

```shell
sbin/start-all.sh

# 或

sbin/start-dfs.sh
sbin/start-yarn.sh
```

#### 查看当前运行的服务

```shell
jps
```

#### 文件操作
基本同linux文件操作

```shell
hadoop fs -mkdir input  # 创建文件夹

hadoop fs -rm -r input  # 删除文件夹

hdfs dfs -put ./input/* input  # 上传文件，也可用 hadoop fs

hdfs dfs -cat input/file1.txt # 查看文件，也可用 hadoop fs
```

#### 常用端口

1. 8088
2. 8030
3. 8032
4. 9000
5. 50070
6. 50010
7. 8042

#### 使用方式

##### 本地测试

```
尽量不要用Windows！
```

1. 安装JDK
2. 下载配置hadoop
3. 下载配置eclipse
4. 下载hadoop eclipse插件
5. 配置map/reduce location
6. Run As Configuration

##### 集群运行

1. 将本地测试好的程序打成jar包

    ```shell
    jar -cvf yourname.jar -C bin/ . 
    # 导出成 Jar/Runnable Jar容易出问题，所以最好是手动打包
    ```

2. 上传jar包到集群namenode服务器上

3. 运行 

    ```shell
    hadoop jar word_count.jar yourMainClass param1 param2
    ```

#### eclipse连不上hdfs或读取错误

这是权限问题，可通过修改 ```hdfs-site.xml```修改

```xml
<property>
    <name>dfs.permission</name>
    <value>false</value>
</property>
```

通过```chmod/chown```修改hdfs文件系统权限的方法有时不管用。


#### jibsplitmapinfo.xml does not exist


可能跟hadoop或eclipse插件的版本有关，找不到正确的hdfs所致，2.7.3下暂时无解。


#### datenode起不来
windows下容易出问题，不折腾它。Linux下可以通过重新格式namenode来解决

#### 格式化

```shell
hdfs namenode -fromat
```

执行之前先stop-all，删除namnode目录下的current文件夹，格式化完成后再start-all


#### 端口访问不了

关闭主机、客户机的```iptables```或```firewalld```和```selinux```


#### win下一直auth as Administrator (simple)

在程序中加两行
```java
conf.set("mapreduce.framework.name", "yarn");
conf.set("yarn.resourcemanager.address", "master:8032");
```

#### eclipse中以某个用户的身份运行
```shell
-DHADOOP_USER_NAME=root
```

#### datanode已启动但live node为0

1. stop-all

2. 关闭防火墙

3. 确保各个节点间可以互相ssh

4. 关闭安全模式
    ```shell
    hadoop dfsadmin -safemode leave
    ```

5. 清空namenode和datanode上的hdfs,logs和tmp文件夹

6. 重新格式化namenode

7. 确保/etc/hosts中各个节点正确配置

8. start-all

#### eclipes中no job jar set
没有指定jar file，需要生成一个jar指定路径
```java
File jarFile = EJob.createTempJar("bin");
job.setJar(jarFile.toString());
```

