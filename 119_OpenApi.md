# OpenApi

# vim

参考： https://gitee.com/juedui0769/MyDocs/blob/master/Linux/Linux998_vim.md

centos7上，还没有vim，`yum install vim`，安装一个。

https://www.cnblogs.com/songlen/p/6883522.html，本文讲解vim如何与外界复制粘贴。

https://blog.csdn.net/j_sure/article/details/41014641，这篇是个补充，但是，在我的电脑上操作失败。

对于vim与外界的复制粘贴，还得花费点精力研究一下，先暂停！



## 环境准备

### docker

docker安装，参考： <https://gitee.com/juedui0769/MyDocs/blob/master/temp/temp921__centos7__docker_ce__.md>

- 镜像的配置，需要特别关注！

看上面的笔记，安装docker时，花费了不少精力；需要把之前的centos6更换为centos7，…… centos7中的命令已经和centos6完全不同了，……！



docker常用命令：

```sh
sudo systemctl start docker
sudo systemctl status docker
```



### JDK

不知为何很讨厌在linux上安装JDK！记录下安装过程，为未来增添信心！

```sh
# F:\Downloads_old_02\jdk-8u191-linux-x64.tar.gz
# 使用"FileZilla"上传到'192.168.3.20'上

tar -zxvf /home/wxg/jdk-8u191-linux-x64.tar.gz
mv ./jdk1.8.0_191 /opt/

vim /etc/profile

# 20181227, JDK
export JAVA_HOME=/opt/jdk1.8.0_191
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH

source /etc/profile

java -version
```

JDK的安装，可以参考 https://jingyan.baidu.com/article/5225f26bafc0f3e6fb090871.html

### Redis

这一年，倒腾的东西太多，记得的很少。Redis的安装，我之前有做笔记，安装有点麻烦，需要下载源码，再编译…… ，感觉有点烦，不如使用 docker ，估计会简单方便很多！

https://hub.docker.com/_/redis/ ， 这个是 docker 上的 redis 页面，可以参考！

还可以参考，https://blog.csdn.net/weixin_38956287/article/details/80423607，这篇博文，来快速体验 docker redis。

```sh
cd /home/wxg
mkdir docker-redis
cd docker-redis

docker search redis

docker pull redis

Digest: sha256:bf65ecee69c43e52d0e065d094fbdfe4df6e408d47a96e56c7a29caaf31d3c35
Status: Downloaded newer image for redis:latest

docker images

mkdir data

docker run --name myredis -p 6379:6379 -v $PWD/data:/data -d redis

docker ps

docker exec -it myredis redis-cli

> keys *
> set name 123
> get name
> exit
```



#### 启动多个redis

```sh
docker run --name myredis -p 6379:6379 -v $PWD/data:/data -d redis
docker run --name myredis1 -p 7001:6379 -v $PWD/data1:/data -d redis
docker run --name myredis2 -p 7002:6379 -v $PWD/data2:/data -d redis
docker run --name myredis3 -p 7003:6379 -v $PWD/data3:/data -d redis
docker run --name myredis4 -p 7004:6379 -v $PWD/data4:/data -d redis
docker run --name myredis5 -p 7005:6379 -v $PWD/data5:/data -d redis
```

以上，只是启动了多个redis，并没有集群。

#### docker，redis，集群

网上搜索了一些资源，但是看起来太复杂，暂时，没有找到解决方案，暂时放弃在docker中集群redis（理解了之后再说）。

### redis 集群

参考： <https://gitee.com/juedui0769/RedisStudy/blob/master/docs/redis999_Redis%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97-%E7%89%882.md>





### RocketMQ

安装和使用参考： <https://gitee.com/juedui0769/MyDocs/blob/master/temp/temp936__%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97__rocketmq__%E4%BD%BF%E7%94%A8%E8%AE%B0%E5%BD%95_.md> ， 这个比较是之前记录的，还好，有个笔记，这次可以节省很多精力。

http://rocketmq.apache.org/docs/quick-start/ ，这个是官方的文档。

```sh
# F:\Downloads_old_02\rocketmq-all-4.3.0-bin-release.zip
# 使用"FileZilla"上传到'192.168.3.20'上

# unzip命令没有，yum install -y unzip
cd /home/wxg
unzip rocketmq-all-4.3.0-bin-release.zip
mv rocketmq-all-4.3.0-bin-release /opt/

cd /opt

mv rocketmq-all-4.3.0-bin-release rocketmq-all-4.3.0

cd ./rocketmq-all-4.3.0/bin

cp runserver.sh runserver.sh.bak
cp runbroker.sh runbroker.sh.bak

vim runserver.sh

JAVA_OPT="${JAVA_OPT} -server -Xms4g -Xmx4g -Xmn2g -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"

# 将上面的内容修改成下面的内容

JAVA_OPT="${JAVA_OPT} -server -Xms512m -Xmx512m -Xmn512m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"

vim runbroker.sh

JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g"
JAVA_OPT="${JAVA_OPT} -XX:MaxDirectMemorySize=15g"

# 将上面的内容修改成下面的内容

JAVA_OPT="${JAVA_OPT} -server -Xms512m -Xmx512m -Xmn512m
JAVA_OPT="${JAVA_OPT} -XX:MaxDirectMemorySize=1g"

# 启动

nohup sh ./mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log

nohup sh ./mqbroker -n localhost:9876 &
tail -f ~/logs/rocketmqlogs/broker.log 

# 停止 ， 貌似应该用 root 用户？？
sh bin/mqshutdown broker
sh bin/mqshutdown namesrv

```

日志如下：

nameserver

```
[root@localhost bin]# tail -f ~/logs/rocketmqlogs/namesrv.log 
2018-12-27 15:38:38 INFO main - tls.client.keyPath = null
2018-12-27 15:38:38 INFO main - tls.client.keyPassword = null
2018-12-27 15:38:38 INFO main - tls.client.certPath = null
2018-12-27 15:38:38 INFO main - tls.client.authServer = false
2018-12-27 15:38:38 INFO main - tls.client.trustCertPath = null
2018-12-27 15:38:39 INFO main - Using OpenSSL provider
2018-12-27 15:38:39 INFO main - SSLContext created for server
2018-12-27 15:38:39 INFO NettyEventExecutor - NettyEventExecutor service started
2018-12-27 15:38:39 INFO main - The Name Server boot success. serializeType=JSON
2018-12-27 15:38:39 INFO FileWatchService - FileWatchService service started


2018-12-27 15:39:39 INFO NSScheduledThread1 - --------------------------------------------------------
2018-12-27 15:39:39 INFO NSScheduledThread1 - configTable SIZE: 0



2018-12-27 15:41:11 INFO NettyServerCodecThread_1 - NETTY SERVER PIPELINE: channelRegistered 127.0.0.1:47190
2018-12-27 15:41:11 INFO NettyServerCodecThread_1 - NETTY SERVER PIPELINE: channelActive, the channel[127.0.0.1:47190]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new topic registered, AUTO_CREATE_TOPIC_KEY QueueData [brokerName=localhost.localdomain, readQueueNums=8, writeQueueNums=8, perm=7, topicSynFlag=0]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new topic registered, BenchmarkTest QueueData [brokerName=localhost.localdomain, readQueueNums=1024, writeQueueNums=1024, perm=6, topicSynFlag=0]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new topic registered, OFFSET_MOVED_EVENT QueueData [brokerName=localhost.localdomain, readQueueNums=1, writeQueueNums=1, perm=6, topicSynFlag=0]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new topic registered, localhost.localdomain QueueData [brokerName=localhost.localdomain, readQueueNums=1, writeQueueNums=1, perm=7, topicSynFlag=0]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new topic registered, SELF_TEST_TOPIC QueueData [brokerName=localhost.localdomain, readQueueNums=1, writeQueueNums=1, perm=6, topicSynFlag=0]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new topic registered, DefaultCluster QueueData [brokerName=localhost.localdomain, readQueueNums=16, writeQueueNums=16, perm=7, topicSynFlag=0]
2018-12-27 15:41:11 INFO RemotingExecutorThread_1 - new broker registered, 172.17.0.1:10911 HAServer: 172.17.0.1:10912


2018-12-27 15:42:11 INFO RemotingExecutorThread_4 - new topic registered, RMQ_SYS_TRANS_HALF_TOPIC QueueData [brokerName=localhost.localdomain, readQueueNums=1, writeQueueNums=1, perm=6, topicSynFlag=0]
```

broker

```
[root@localhost bin]# tail -f ~/logs/rocketmqlogs/broker.log 
2018-12-27 15:41:10 WARN main - Load default transaction message hook service: TransactionalMessageServiceImpl
2018-12-27 15:41:10 WARN main - Load default discard message hook service: DefaultTransactionalMessageCheckListener
2018-12-27 15:41:11 INFO FileWatchService - FileWatchService service started
2018-12-27 15:41:11 INFO PullRequestHoldService - PullRequestHoldService service started
2018-12-27 15:41:11 INFO brokerOutApi_thread_1 - register broker to name server localhost:9876 OK
2018-12-27 15:41:11 INFO main - Start transaction service!
2018-12-27 15:41:11 INFO main - The broker[localhost.localdomain, 172.17.0.1:10911] boot success. serializeType=JSON and name server is localhost:9876
2018-12-27 15:41:20 INFO BrokerControllerScheduledThread1 - dispatch behind commit log 0 bytes
2018-12-27 15:41:20 INFO BrokerControllerScheduledThread1 - Slave fall behind master: 0 bytes
2018-12-27 15:41:21 INFO brokerOutApi_thread_2 - register broker to name server localhost:9876 OK



2018-12-27 15:41:51 INFO brokerOutApi_thread_3 - register broker to name server localhost:9876 OK
2018-12-27 15:42:11 INFO TransactionalMessageCheckService - create new topic TopicConfig [topicName=RMQ_SYS_TRANS_HALF_TOPIC, readQueueNums=1, writeQueueNums=1, perm=RW-, topicFilterType=SINGLE_TAG, topicSysFlag=0, order=false]
2018-12-27 15:42:11 INFO brokerOutApi_thread_4 - register broker to name server localhost:9876 OK
2018-12-27 15:42:20 INFO BrokerControllerScheduledThread1 - dispatch behind commit log 0 bytes
2018-12-27 15:42:20 INFO BrokerControllerScheduledThread1 - Slave fall behind master: 0 bytes
2018-12-27 15:42:21 INFO brokerOutApi_thread_1 - register broker to name server localhost:9876 OK
2018-12-27 15:42:51 INFO brokerOutApi_thread_2 - register broker to name server localhost:9876 OK
```

#### NameServer

https://blog.csdn.net/mr253727942/article/details/52637126 ， 这篇博文介绍的比较详细，可以参考。

- 早期，RocketMq是使用 zookeeper 来做命名服务的，后来，为向轻量级转化，改为使用自己的 nameServer 组件。

#### broker

https://blog.csdn.net/binzhaomobile/article/details/73332463，这篇文章中的图示，让我明白了boker的含义。值得阅读！





## 整理

### crt-parent

```
filters
	|- filter-dev-env.properties
```

`gateway.signUrl=` 是做什么用的呢？



### gateway

```
src/main/resources
	|- props
		|- openapi-cfg-redis.properties
	|- spring
		|- spring-context-redis.xml
```

### apiinterface

redis.properties

redis.xml



### apiservice



### apipub



### apisub



### apiconsole



## 编译

```sh
mvn clean package -Pdev
```

## 运行

### gateway











# End