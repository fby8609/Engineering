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


### 



