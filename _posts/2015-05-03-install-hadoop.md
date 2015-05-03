---
layout: post
title: Hadoop学习（一）Hadoop2.6.0伪分布式安装与配置
tags:
    - 分布式
    - hadoop
    - 大数据
---

## 环境 ##
#### 准备Ubuntu 14.04 LTS  并更新apt ####
1. 执行`sudo apt-get update`	
2. 可更改数据源进行update 


#### 设置静态IP地址 ####
必须设子静态IP，为以后的集群，分布式，HA等做铺垫	

1. 编辑interfaces文件	
`sudo vi /etc/network/interfaces`   
2. 写入如下:	
`auto eth0	
iface eth0 inet static	
address 192.168.1.100 	#IP地址		
gateway 192.168.1.1 	#网关		
netmask 255.255.255.0	
network 192.168.1.0	
broadcast 192.168.1.255		
`	

#### 修改HostName ####
1. 编辑hostname文件
`sudo vi /etc/hostname `	
2. 写入`ubuntu`		
3. 那么ubuntu就是本机器的主机名，可以`hostname`进行查看


#### IP与HostName绑定 ####
1. 执行`sudo vi /etc/hosts`	
2. 写入如下：	
`192.168.1.100 hadoop-yarn.jasonsoso.com hadoop-yarn`	
3. 那么访问‘hadoop-yarn.jasonsoso.com’则访问IP为‘192.168.1.100’的机器


#### 创建hadoop系统用户 ####
如果你安装 Ubuntu 的时候不是用的 hadoop 用户，那么最好增加一个名为 hadoop 的用户，密码可设置为 123456 (密码随意指定)。	

1. 创建新用户	
`sudo useradd -m hadoop -s /bin/bash`	
2. 修改密码		
`sudo passwd hadoop`	
3. 为 hadoop 用户增加管理员权限，方便部署		
`sudo adduser hadoop sudo`	



#### Sun JDK 7 ####
详情请参考 [ubuntu下配置java环境](http://www.cnblogs.com/fnng/archive/2013/01/30/2883815.html "ubuntu下配置java环境") 和 [HOW TO INSTALL ORACLE JAVA 7 IN DEBIAN VIA REPOSITORY](http://www.webupd8.org/2012/06/how-to-install-oracle-java-7-in-debian.html "HOW TO INSTALL ORACLE JAVA 7 IN DEBIAN VIA REPOSITORY")

#### 安装SSH server、配置SSH无密码登陆 ####

1. 执行安装		
`sudo apt-get install openssh-server`
2. 登陆本机		
`ssh localhost`		
3. 配置SSH无密码登陆	
`sudo ssh-keygen -t rsa		# 生成密钥	
sudo ssh-copy-id ubuntu@localhost	# 拷贝密钥到某台机器		
sudo ssh localhost	#进行无密码登陆	
`


## hadoop2.6.0安装 ##
1. 下载hadoop2.6.0	
	官网hadoop下载http://hadoop.apache.org/releases.html
2. 解压tar包	
	`sudo tar -zxvf ./hadoop-2.6.0.tar.gz -C /opt/hadoop  # 解压到/opt/hadoop中`	
	则hadoop的安装根目录为：`/opt/hadoop/hadoop-2.6.0`
3. 修改文件权限
	`sudo chown -R hadoop:hadoop ./hadoop-2.6.0`

## 配置 ##
1. 配置环境变量	
下面`#set Hadoop`才是真正的hadoop配置，而`#set java environment`是必须的java环境变量，`#set findbugs`、`#set ant`、`#PROTOBUF`和`#set maven environment`是编译hadoop代码必须的，现在没有编译hadoop源码，只需要java环境和hadoop环境足矣。		 		
执行`sudo vi /etc/profile`修改profile文件			
添加如下：	
- `#set java environment	
JAVA_HOME=/usr/java/jdk1.7.0_60		
export JRE_HOME=/usr/java/jdk1.7.0_60/jre		
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH	
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH		`	

- `#set findbugs	
export LD_LIBRARY_PATH=/usr/local/lib/		
export FINDBUGS_HOME=/usr/local/findbugs-3.0.0		
export PATH=$PATH:$FINDBUGS_HOME/bin	`	

- `#set ant	
ANT_HOME=/usr/local/apache-ant-1.9.4	
export PATH=$PATH:$ANT_HOME/bin	`	

- `#PROTOBUF	
export PROTOC_HOME=/usr/local/protobuf-2.5.0	
export PATH=$PATH:$PROTOC_HOME/bin	
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PROTOC_HOME/lib`	
	
- `#set maven environment		
M2_HOME=/usr/maven/apache-maven-3.0.5	
export MAVEN_OPTS="-Xms256m -Xmx512m"	
export PATH=$M2_HOME/bin:$PATH	`	

- ` #set Hadoop		
export HADOOP_HOME=/opt/hadoop/hadoop-2.6.0		
export HADOOP_INSTALL=$HADOOP_HOME	
export HADOOP_MAPRED_HOME=$HADOOP_HOME	
export HADOOP_COMMON_HOME=$HADOOP_HOME	
export HADOOP_HDFS_HOME=$HADOOP_HOME	
export HADOOP_YARN_HOME=$HADOOP_HOME	
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native	
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin	
`


2. hadoop-env.sh

		export JAVA_HOME=/usr/java/jdk1.7.0_60		
		export HADOOP_HOME=/opt/hadoop/hadoop-2.6.0


3. yarn-env.sh

		export JAVA_HOME=/usr/java/jdk1.7.0_60		
		export HADOOP_HOME=/opt/hadoop/hadoop-2.6.0


4. mapred-env.sh

		export JAVA_HOME=/usr/java/jdk1.7.0_60		
		export HADOOP_HOME=/opt/hadoop/hadoop-2.6.0


5. core-site.xml

		<property>
			<name>fs.default.name</name>
			<value>hdfs://hadoop-yarn.dragon.org:8020</value>
		</property>

		<property>
				<name>hadoop.tmp.dir</name>
				<value>/opt/modules/hadoop-2.2.0/data/tmp</value>
		</property>


6. hdfs-site.xml

		<property>		
				<name>dfs.replication</name>
				<value>1</value>
		</property>
		<property>
                <name>dfs.permissions</name>
                <value>false</value>
        </property>

7. yarn-site.xml

		<property>
			<name>yarn.nodemanager.aux-services</name>
			<value>mapreduce_shuffle</value>
		</property>
	

8. mapred-site.xml

		<property>	 	        		
			<name>mapreduce.framework.name</name>
			<value>yarn</value>
		</property>


## 启动 ##
1. 启动HDFS(NameNode、DataNode、SecondaryNameNode)
	
	- NameNode 格式化	
		`bin/hdfs namenode -format`

	- 启动NameNode	
		`sbin/hadoop-daemon.sh start namenode`

	- 启动DataNode		
		`sbin/hadoop-daemon.sh start datanode`

	- 启动SecondaryNameNode		
		`sbin/hadoop-daemon.sh start secondarynamenode`

2. 启动YARN(ResourceManager、NodeManager)
	- 启动ResourceManger		
		`sbin/yarn-daemon.sh start resourcemanager`
	- 启动NodeManager
		`sbin/yarn-daemon.sh start nodemanager`


3. 启动历史服务器	

		sbin/mr-jobhistory-daemon.sh start historyserver
	
4. 启动方式

		sbin/start-dfs.sh
		sbin/start-yarn.sh
		sbin/start-all.sh



## 实例与测试 ##

启动后，用命令`jps`检查是否启动

![](http://7u2ivm.com1.z0.glb.clouddn.com/@/2015/start-all.png)



访问HDFD的NameNode url `http://hadoop-yarn.jasonsoso.com:50070/`

![](http://7u2ivm.com1.z0.glb.clouddn.com/@/2015/50070.png)

访问HDFD的Secondary NameNode  url `http://hadoop-yarn.jasonsoso.com:50090/`

![](http://7u2ivm.com1.z0.glb.clouddn.com/@/2015/50090.png)









