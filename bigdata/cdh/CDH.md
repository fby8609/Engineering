# CDH5.13 安装部署

## 环境

```
Centos6.8 X86_64
CDH5.13

```

## 集群架构及角色分配

![](/assets/WX20180913-150645.png)  
![](/assets/WX20180913-151016.png)

## 端口分布

![](/assets/WX20180913-152903.png)

## 准备工作

* 安装系统依赖

```shell
yum install -y cyrus-sasl-gssapi cyrus-sasl-plain fuse fuse-libs gcc init-functions libxslt mod_ssl MySQL-python openssl openssl-devel perl portmap psmisc python-devel python-psycopg2 python-setuptools sqlite swig zlib gcc-c++ automake libtool flex bison pkgconfig gcc-c++ boost-devel libevent-devel zlib-devel Python-devel ruby-devel crypto-utils openssl openssl-devel glibc-static libstdc++-static ncurses-devel sqlite-devel readline-devel tk-devel gcc make cyrus-sasl-plain  cyrus-sasl-devel  cyrus-sasl-gssapi tkinter tcl-devel tk-devel bzip2-devel snappy
```

* 安装Oracle JDK: 

```shell
open http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html

#tar -zxvf jdk-8u181-linux-x64.tar.gz -C /usr/java/
#touch /etc/profile.d/java.sh
java.sh
JAVA_HOME=/usr/java/jdk1.8.0_181
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib
JRE_HOME=$JAVA_HOME/jre
```

* 安装Mysql5.7

```shell
安装Mysql7.5
# cd /opt; wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
# sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
# yum update
# sudo yum install mysql-server
# sudo systemctl start mysqld
修改配置文件
#sudo service mysqld stop
#vim /etc/my.cnf
[mysqld]
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
# symbolic-links = 0

key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space. Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your system
#and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

# For MySQL version 5.1.8 or later. For older versions, reference MySQL documentation for configuration help.
binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

sql_mode=STRICT_ALL_TABLES

添加开机启动
#sudo /sbin/chkconfig mysqld on
#sudo /sbin/chkconfig --list mysqld

初始化安全配置
#sudo /usr/bin/mysql_secure_installation

下载JDBC驱动
open http://www.mysql.com/downloads/connector/j/5.1.html
# sudo mkdir -p /usr/share/java/
# sudo cp mysql-connector-java-5.1.31/mysql-connector-java-5.1.31-bin.jar /usr/share/java/mysql-connector-java.jar

创建数据库
mysql> create database database DEFAULT CHARACTER SET utf8;
mysql> grant all on database.* TO 'user'@'%' IDENTIFIED BY 'password';
```

![](/assets/WX20180913-194533.png)

* 安装Cloudera Manager Server和 Agent

```shell
open https://www.cloudera.com/documentation/enterprise/release-notes/topics/cm_vd.html#cmvd_topic_1
```

![](/assets/WX20180913-174424.png)

```
下载一下3个文件到/opt目录
```

![](/assets/WX20180913-174759.png)

```shell
下载cms manager
#wget https://archive.cloudera.com/cm5/cm/5/cloudera-manager-el6-cm5.13.1_x86_64.tar.gz
创建manager目录
#mkdir -p /opt/cloudera-manager
#tar xzf cloudera-manager*.tar.gz -C /opt/cloudera-manager
创建cms必要启动用户
#useradd --system --home=/opt/cloudera-manager/cm-5.3.1/run/cloudera-scm-server --no-create-home --shell=/bin/false --comment "Cloudera SCM User" cloudera-scm
创建cms本地数据目录
#/var/lib/cloudera-scm-server
#chown cloudera-scm:cloudera-scm /var/lib/cloudera-scm-server
更改agent配置文件
# vim /opt/cloudera-manager/cm-5.13.1/etc/cloudera-scm-agent/config.ini
server_host=cms-manager
server_port=7182
创建日志及配置目录
# mkdir -p /var/log/{cloudera-scm-headlamp,cloudera-scm-firehose,cloudera-scm-alertpublisher,cloudera-scm-eventserver}
# mkdir -p /var/lib/{cloudera-scm-headlamp,cloudera-scm-firehose,cloudera-scm-alertpublisher,cloudera-scm-eventserver,cloudera-scm-server}
#chown -R cloudera-scm:cloudera-scm /var/log/cloudera-scm-headlamp
创建CDH包目录
#mkdir -p /opt/cloudera/parcel-repo
#chown username:groupname /opt/cloudera/parcel-repo
在每个集群节点创建parcels目录
#mkdir -p /opt/cloudera/parcels
#chown username:groupname /opt/cloudera/parcels

下载JDBC驱动
open http://www.mysql.com/downloads/connector/j/5.1.html
# sudo mkdir -p /usr/share/java/
# sudo cp mysql-connector-java-5.1.31/mysql-connector-java-5.1.31-bin.jar /usr/share/java/mysql-connector-java.jar
# sudo cp /usr/share/java/mysql-connector-java-5.1.47-bin.jar /opt/cloudera-manager/cm-5.13.1/share/cmf/lib

数据库初始化
# ./scm_prepare_database.sh   mysql cm -h ops-test-004 -uroot -p'1qaz@WSX3edc$RFV' --scm-host ops-test-004 root  '1qaz@WSX3edc$RFV'

启动server 和 agent

#sudo $CMF_DEFAULTS/etc/init.d/cloudera-scm-server start 
#sudo $CMF_DEFAULTS/etc/init.d/cloudera-scm-agent start 

echo never > /sys/kernel/mm/transparent_hugepage/defrag
echo never > /sys/kernel/mm/transparent_hugepage/enabled

访问 http://127.0.0.1:7180
```

hive /opt/cloudera/parcels/CDH-5.13.1-1.cdh5.13.1.p0.2/lib/hive/lib

### 



