---
layout: post
title: Elasticsearch 安装配置
categories: Elasticsearch
description: Elasticsearch 笔记
keywords: elasticsearch, java
---

## 1.安装
  - 1.1 安装JDK并配置环境变量，版本建议1.8以上
  - 1.2 从5.0开始，ElasticSearch提高了安全级别，不允许采用root帐号启动，所以我们要添加一个用户。
	- 1.2.1 先创建用户组，如bigdata
		[root@hadoop ~]# groupadd bigdata
	- 1.2.2 创建用户,如es
	    [root@hadoop ~]# useradd es
		[root@hadoop ~]# passwd es
	- 1.2.3 将es用户添加到bigdata用户组
		[root@hadoop ~]# usermod -G bigdata es
	- 1.2.4 设置sudo权限
		[root@hadoop ~]# visudo
		找到root ALL=(ALL) ALL一行，添加es用户，如下:
		## Allow root to run any commands anywhere
		root    ALL=(ALL)       ALL
		es      ALL=(ALL)       ALL
		之后便可以通过su(switch user)命令切换到不同的用户。	
	- 1.2.5 下载elasticsearch安装包，解压
		更改elasticsearch-6.1.1文件夹以及内部文件的所属用户为es, 
		用户组组为bigdata，-R表示递归执行目录内所有文件。
		[es@hadoop ~]$ sudo chown -R es:bigdata /opt/elasticsearch-6.6.0

## 2. 配置参数调整
	- 2.1 修改elasticsearch.yml文件的network.host和http.port
		# Set the bind address to a specific IP (IPv4 or IPv6):
		#
		network.host: 192.168.80.131
		#
		# Set a custom port for HTTP:
		#
		http.port: 9200
	- 2.2 切换到root用户，修改/etc/sysctl.conf
		[es@hadoop ~]$ su 
		[root@hadoop elasticsearch-6.6.0]# vi /etc/sysctl.conf
		并添加内容：
		vm.max_map_count=262144
	    执行命令sysctl -p 使之生效
	- 2.3 修改文件/etc/security/limits.conf
		* hard nofile 65536
		* soft nofile 65536
		* soft nproc 2048
		* hard nproc 4096
		# End of file
		
## 3. 进入es安装目录，执行。-d 参数表示后台后台执行。随后即可在浏览器9200端口验证是否正常启动。
	[es@node1 elasticsearch-6.6.0]$ bin/elasticsearch -d
		
注意：Elasticsearch通过HTTP协议请求默认采用9200端口，TCP请求采用9300端口。
	因此，在JAVA实际应用中，除非明确采用HTTP请求，基本都是9300端口。
	
## 4. Kibana安装
   实际应用过程中，个人更喜欢在Kibana工作栏中进行ES的查询何调试。
   安装过程比较简单，略过。

	
