---
layout: post
title: Setup HBase for Cloudera
comments: true
redirect_from: "/2013/10/01/setuphbase/"
permalink: setup-hbase-cloudera
categories: HBase shell cloudera
---

Follow these steps

### Install `hbase` on all the machines
~~~sh
sudo yum install hbase
~~~

### Install `hbase-master` and `zookeper-server` on your master machine
~~~sh
sudo yum install zookeper-server
sudo yum install hbase-master
~~~
zookeeper-server automatically installs the base zookeper package also.
For hbase to start it needs to have zookeeper 

### Install `hbase-region` server and zookeeper in all your slave machines
~~~sh
sudo yum install zookeeper
sudo yum install hbase-regionserver
~~~

### Modifying the HBase Configuration

To enable *pseudo-distributed* mode, you must first make some configuration changes. Open `/etc/hbase/conf/hbase-site.xml` in your editor of choice, and insert the following XML properties between the `<configuration>` and `</configuration>` tags. Be sure to replace `myhost` with the hostname of your HDFS NameNode (as specified by `fs.default.name` or `fs.defaultFS` in your `hadoop/conf/core-site.xml` file). You may also need to change the port number from the default `8020` to another value.
~~~xml
<property>
  <name>hbase.cluster.distributed</name>
  <value>true</value>
</property>
<property>
  <name>hbase.rootdir</name>
  <value>hdfs://myhost:8020/hbase</value>
</property>
~~~
### Configuring for Distributed Operation

After you have decided which machines will run each process, you can edit the configuration so that the nodes may locate each other. In order to do so, you should make sure that the configuration files are synchronized across the cluster. Cloudera strongly recommends the use of a configuration management system to synchronize the configuration files, though you can use a simpler solution such as rsync to get started quickly.

The only configuration change necessary to move from pseudo-distributed operation to fully-distributed operation is the addition of the ZooKeeper Quorum address in hbase-site.xml. Insert the following XML property to configure the nodes with the address of the node where the ZooKeeper quorum peer is running:
~~~xml
<property>
  <name>hbase.zookeeper.quorum</name>
  <value>mymasternode</value>
</property>
~~~

### Creating the `/hbase` Directory in HDFS

Before starting the HBase Master, you need to create the /hbase directory in HDFS. The HBase master runs as hbase:hbase so it does not have the required permissions to create a top level directory.

To create the `/hbase` directory in HDFS:
~~~sh
sudo -u hdfs hadoop fs -mkdir /hbase
sudo -u hdfs hadoop fs -chown hbase /hbase
~~~

### Starting the ZooKeeper Server
To start ZooKeeper after a fresh install:
~~~sh
sudo service zookeeper-server init
sudo service zookeeper-server start
~~~

### Starting the HBase Master
On Red Hat and SLES systems (using .rpm packages) you can now start the HBase Master by using the included service script:
~~~sh
sudo service hbase-master start
~~~

### Starting the HBase Region Server:
~~~sh
sudo service hbase-regionserver start
~~~

### Accessing HBase by using the HBase Shell
After you have started HBase, you can access the database by using the HBase Shell:
~~~sh
hbase shell
~~~

Hope this blog helped you in setting up HBase. Please let me know if you have any questions.

For further reference :
[Cloudera blog](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH4/4.2.0/CDH4-Installation-Guide/cdh4ig_topic_20_2.html)















