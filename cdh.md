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
- 安装Oracle JDK: 

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
- 安装Mysql5.7

```shell
open https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/
# vim /etc/yum.repos.d/mysql-community.repo

# Enable to use MySQL 5.7
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/6/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

#yum repolist enabled | grep mysql
#yum install mysql-community-server
#service mysqld start
```
- 安装Cloudera Manager Server和 Agent

```shell
open https://www.cloudera.com/documentation/enterprise/release-notes/topics/cm_vd.html#cmvd_topic_1
```
![](/assets/WX20180913-174424.png)

```
下载一下3个文件到/opt目录
```
![](/assets/WX20180913-174759.png)

### 



