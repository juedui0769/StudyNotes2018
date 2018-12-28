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

#### redis 安装

参考： <https://gitee.com/juedui0769/RedisStudy/blob/master/docs/redis999_Redis%E5%85%A5%E9%97%A8%E6%8C%87%E5%8D%97-%E7%89%882.md> ，这个文档是之前学习Redis时记录下来的，里面有Redis安装的相关介绍。但是，那是之前在 centos6 上安装redis时的记录，现在 centos7 上，也许有不同的情况发生。

```sh
# F:\Downloads_old_02\redis-4.0.11.tar.gz
# 上传到服务器

tar -zxf redis-4.0.11.tar.gz

cd redis-4.0.11

make

make[3]: gcc: Command not found
/bin/sh: cc: command not found

yum install gcc

Installed:
  gcc.x86_64 0:4.8.5-36.el7

# 需要回到上层目录，将'redis-4.0.11'删除，重新解压，再执行'make'
cd ..
rm -rf redis-4.0.11
tar -zxf redis-4.0.11.tar.gz

# F:\Downloads_old_02\tcl8.6.1-src.tar.gz
# 上传到服务器

tar -zxf tcl8.6.1-src.tar.gz
cd tcl8.6.1

cd unix
sudo ./configure
sudo make
sudo make install

cd /home/wxg/redis-4.0.11
make test

\o/ All tests passed without errors!

make install

```

按照上面的步骤，安装 redis 完成。

```sh
redis-server --port 6379

redis-cli -h 127.0.0.1 -p 6379
keys *
set foo 123
get foo

redis-cli SHUTDOWN
```

#### redis 集群

参考 http://www.redis.cn/topics/cluster-tutorial.html 中“搭建并使用Redis集群”一节，

##### 启动节点（集群前）

上一节中，redis，的安装，不能忽略；

这节是“准备”环境，创建6个目录，存放redis的配置文件。（如何配置，在本节描述）

```sh
# 创建目录
cd /home/wxg
mkdir redis-cluster
cd redis-cluster

mkdir 7000 7001 7002 7003 7004 7005

```

下面是一个最少选项的集群配置文件

```sh
port 7000
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
```

```sh
# 复制、修改配置文件
cd /home/wxg/redis-4.0.11
cp redis.conf ~/redis-cluster/

cd ~/redis-cluster/
vim redis.conf

port 7000
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes

# 复制到子目录中去
cp ./redis.conf ./7000/
cp ./redis.conf ./7001/
cp ./redis.conf ./7002/
cp ./redis.conf ./7003/
cp ./redis.conf ./7004/
cp ./redis.conf ./7005/

# 除了'7000'，其他目录下的'redis.conf'文件都要修改'port'
vim ./7001/redis.conf
...

# 启动
# 要进入到各个子目录下，再执行启动命令，因为会生成很多文件
cd 7000
nohup redis-server ./redis.conf &

cd ../7001
nohup redis-server ./redis.conf &

cd ../7002
nohup redis-server ./redis.conf &

cd ../7003
nohup redis-server ./redis.conf &

cd ../7004
nohup redis-server ./redis.conf &

cd ../7005
nohup redis-server ./redis.conf &

# ps
ps -ef | grep redis

wxg      15719  1673  0 13:11 pts/1    00:00:00 redis-server 127.0.0.1:7000 [cluster]
wxg      15724  1673  0 13:11 pts/1    00:00:00 redis-server 127.0.0.1:7001 [cluster]
wxg      15728  1673  0 13:12 pts/1    00:00:00 redis-server 127.0.0.1:7002 [cluster]
wxg      15732  1673  0 13:12 pts/1    00:00:00 redis-server 127.0.0.1:7003 [cluster]
wxg      15736  1673  0 13:12 pts/1    00:00:00 redis-server 127.0.0.1:7004 [cluster]
wxg      15741  1673  0 13:12 pts/1    00:00:00 redis-server 127.0.0.1:7005 [cluster]
wxg      15746  1673  0 13:12 pts/1    00:00:00 grep --color=auto redis

```

~~调整，配置文件~~



##### 安装“ruby”

这仍然是，集群前的准备工作。但是这里的“ruby”安装方法不可取，不要自己编译安装，会很麻烦的！！！

```sh
# F:\Downloads\ruby-2.6.0.tar.gz
# 上传到服务器

tar -zxf ruby-2.6.0.tar.gz
cd ruby-2.6.0

sudo ./configure
sudo make
sudo make install

ruby -v

ruby 2.6.0p0 (2018-12-25 revision 66547) [x86_64-linux]
```

##### “ruby”的卸载

```sh
cd ~/redis-4.0.11/src
ls | grep redis-tri

./redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 \
127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005

# 以上命令运行失败，原因是没有 ruby 没有 redis
#（ruby不太懂，就是ruby没有安装redis依赖的意思吧）
# 参考： https://www.jianshu.com/p/c38369097448， 这里提到的问题，正式我遇到的问题。

# 先弄好，gem
# 参考： https://gems.ruby-china.com/
gem sources -l
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

## 插曲： openssl， 并重新编译 ruby
yum install openssl

sudo ./configure
sudo make
sudo make install

## 以上方式失败，使用'RVM'安装ruby

# 首先，得卸载 ruby ，貌似很麻烦
# 参考： https://ruby-china.org/topics/4419 ， https://ruby-china.org/topics/4416
which ruby
whereis ruby

find / -name ruby

# 删除
rm -rf /usr/lib64/ruby
rm -rf /usr/lib64/gems
rm -rf /usr/share/ruby
rm -rf /usr/local/bin/ruby
rm -rf /usr/local/include/ruby-2.6.0
rm -rf /usr/local/lib/ruby
rm -rf /usr/local/share/doc/ruby

# gem
find / -name gem

rm -rf /usr/share/locale/gem
rm -rf /usr/local/bin/gem

# gems
find / -name gems

rm -rf /usr/share/gems


# 再来添加'gem'镜像源
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
gem sources -l

```

##### 安装RVM

推荐使用”rvm“来管理ruby，参考 https://ruby-china.org/wiki/rvm-guide， 安装这篇文章介绍的步骤安装，https://rvm.io/ 官网上也是这样写的，所以，不要有抗拒感或怀疑心（不过，还是按照官网的来，`recv-keys`可能不同）。

```sh
# gpg, gpg2 , centos7上都有安装的
man gpg
man gpg2

gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

gpg: keyserver receive failed: Syntax error in URI
# 以上，官网复制的命令，执行失败，改为使用以下

gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3

# 上面命令执行的非常慢，改为参考： https://rvm.io/rvm/install

gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

# 这条命令执行非常快，不知是否有之前命令的功劳？执行结果如下：
gpg: requesting key D39DC0E3 from hkp server keys.gnupg.net
gpg: requesting key 39499BDB from hkp server keys.gnupg.net
gpg: key D39DC0E3: "Michal Papis (RVM signing) <mpapis@gmail.com>" not changed
gpg: key 39499BDB: public key "Piotr Kuczynski <piotr.kuczynski@gmail.com>" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 2
gpg:               imported: 1  (RSA: 1)
gpg:              unchanged: 1

# 继续
\curl -sSL https://get.rvm.io | bash -s stable

# 等待一会，执行结果如下：
Downloading https://github.com/rvm/rvm/archive/1.29.6.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.6/1.29.6.tar.gz.asc
gpg: Signature made Thu 13 Dec 2018 11:09:53 PM CST using RSA key ID 39499BDB
gpg: Good signature from "Piotr Kuczynski <piotr.kuczynski@gmail.com>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 7D2B AF1C F37B 13E2 069D  6956 105B D0E7 3949 9BDB
GPG verified '/home/wxg/.rvm/archives/rvm-1.29.6.tgz'
Installing RVM to /home/wxg/.rvm/
    Adding rvm PATH line to /home/wxg/.profile /home/wxg/.mkshrc /home/wxg/.bashrc /home/wxg/.zshrc.
    Adding rvm loading line to /home/wxg/.profile /home/wxg/.bash_profile /home/wxg/.zlogin.
Installation of RVM in /home/wxg/.rvm/ is almost complete:

  * To start using RVM you need to run `source /home/wxg/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.

# 继续
source ~/.bashrc
source ~/.bash_profile

# https://ruby-china.org/wiki/ruby-mirror ，这个链接上的地址是最新的，
echo "ruby_url=https://cache.ruby-china.com/pub/ruby" > ~/.rvm/user/db

# 继续

rvm list known

rvm install 2.5.3 --disable-binary

# 以上命令执行后，一直没动静，仔细看才发现要输入当前用户密码，
# 只是，输出没有换行，所以不那么显眼！ 以上遭遇要注意一下！

# 以上命令的安装过程非常慢，安装结果如下：

ruby-2.5.3 - #removing old rubygems........
ruby-2.5.3 - #installing rubygems-2.7.8.....................................
ruby-2.5.3 - #gemset created /home/wxg/.rvm/gems/ruby-2.5.3@global
ruby-2.5.3 - #importing gemset /home/wxg/.rvm/gemsets/global.gems................................................................
ruby-2.5.3 - #generating global wrappers.......
ruby-2.5.3 - #gemset created /home/wxg/.rvm/gems/ruby-2.5.3
ruby-2.5.3 - #importing gemsetfile /home/wxg/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.5.3 - #generating default wrappers.......
ruby-2.5.3 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.5.3 - #complete 
Ruby was built without documentation, to build it run: rvm docs generate-ri

# 继续
rvm list
rvm use 2.5.3
rvm use 2.5.3 --default

# 卸载，使用如下命令
rvm remove 2.5.3

# ruby, gem
ruby -v
gem -v

# https://ruby-china.org/wiki/rvm-guide ，还有其他的内容，
# 但是我只是为redis集群而来，其他的就不看了。
```

##### 搭建集群

```sh
# gem install redis，首先安装依赖，似乎不用再配置'gem'的镜像了？

gem install redis
gem list

# 还要再次启动所有的六个 redis 节点
cd /home/wxg/redis-cluster/7000
nohup redis-server ./redis.conf &
cd ../7001
nohup redis-server ./redis.conf &
cd ../7002
nohup redis-server ./redis.conf &
cd ../7003
nohup redis-server ./redis.conf &
cd ../7004
nohup redis-server ./redis.conf &
cd ../7005
nohup redis-server ./redis.conf &

ps -ef | grep redis

# 再次回到'src'
cd ~/redis-4.0.11/src
ls | grep redis-tri

./redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 \
127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005

# 这次顺序执行上面的命令，输出如下：

>>> Creating cluster
>>> Performing hash slots allocation on 6 nodes...
Using 3 masters:
127.0.0.1:7000
127.0.0.1:7001
127.0.0.1:7002
Adding replica 127.0.0.1:7004 to 127.0.0.1:7000
Adding replica 127.0.0.1:7005 to 127.0.0.1:7001
Adding replica 127.0.0.1:7003 to 127.0.0.1:7002
>>> Trying to optimize slaves allocation for anti-affinity
[WARNING] Some slaves are in the same host as their master
M: cd1aa60291b2faabcbf6364f12af8768f1dffbab 127.0.0.1:7000
   slots:0-5460 (5461 slots) master
M: 27dd79c08c69981f718557ff77f1b2e81252f563 127.0.0.1:7001
   slots:5461-10922 (5462 slots) master
M: ecad2ee73c9afc708790b50522cdde26e979b44a 127.0.0.1:7002
   slots:10923-16383 (5461 slots) master
S: b120daa0296ebef447cd963d20e7500c98610b9f 127.0.0.1:7003
   replicates cd1aa60291b2faabcbf6364f12af8768f1dffbab
S: 0354a6a9f3cc13c041d3f1698a0a99dc71ba1943 127.0.0.1:7004
   replicates 27dd79c08c69981f718557ff77f1b2e81252f563
S: 9198063362daef4f7cc138dd27e70e7d938744a1 127.0.0.1:7005
   replicates ecad2ee73c9afc708790b50522cdde26e979b44a
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join...
>>> Performing Cluster Check (using node 127.0.0.1:7000)
M: cd1aa60291b2faabcbf6364f12af8768f1dffbab 127.0.0.1:7000
   slots:0-5460 (5461 slots) master
   1 additional replica(s)
S: 9198063362daef4f7cc138dd27e70e7d938744a1 127.0.0.1:7005
   slots: (0 slots) slave
   replicates ecad2ee73c9afc708790b50522cdde26e979b44a
S: 0354a6a9f3cc13c041d3f1698a0a99dc71ba1943 127.0.0.1:7004
   slots: (0 slots) slave
   replicates 27dd79c08c69981f718557ff77f1b2e81252f563
M: 27dd79c08c69981f718557ff77f1b2e81252f563 127.0.0.1:7001
   slots:5461-10922 (5462 slots) master
   1 additional replica(s)
M: ecad2ee73c9afc708790b50522cdde26e979b44a 127.0.0.1:7002
   slots:10923-16383 (5461 slots) master
   1 additional replica(s)
S: b120daa0296ebef447cd963d20e7500c98610b9f 127.0.0.1:7003
   slots: (0 slots) slave
   replicates cd1aa60291b2faabcbf6364f12af8768f1dffbab
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

# 从上面可以看出，7000,7001,7002 是 Master
# 7003，7004,7005 是 slave

# 测试一下
redis-cli -p 7000

# 以上命令是错误的，缺少 '-c' 参数
redis-cli -c -p 7000
keys *
set foo 123
get foo

redis-cli -c -p 7005
get foo
```

至此，redis-集群终于告捷！`ruby`的安装有点复杂，而且，下载有点慢，下次如果再到新环境上部署，可以试试直接安装`ruby2.5.3`，然后配置"ruby-china"的gem镜像，应该比安装`rvm`要快一些，`rvm`的安装太慢了，编译的也很慢。

另外，如果有时间，可以研究一下`src/redis-trib.rb`文件的代码，看是否能搞懂，或者研究一下`Jedis`，或者其他的Java版的redis集群写法，redis集群的重点应该是在“槽”的分配上！

##### 集群相关命令

在 http://www.redis.cn/topics/cluster-tutorial.html 上，还提到了其他的好用的命令

```sh
# 集群重新分片，如下：
# 你只需要指定集群中其中一个节点的地址， redis-trib 就会自动找到集群中的其他节点。
./redis-trib.rb reshard 127.0.0.1:7000

# 查询-目标节点
redis-cli -p 7000 cluster nodes | grep myself

# 在重新分片结束后你可以通过如下命令检查集群状态
./redis-trib.rb check 127.0.0.1:7000

# consistency-test.rb ， （我没有执行）
# ruby consistency-test.rb

# 列出集群中的所有主节点
redis-cli -p 7000 cluster nodes | grep master

# 让这个主节点崩溃
# 可以向节点发送"DEBUG SEGFAULT"命令，
redis-cli -p 7002 debug segfault

# 添加一个新节点
# 先启动一个新的节点，比如 7006 ，配置及启动方式同之前的节点一样
# 然后使用如下命令添加到集群中
# 第一个参数是新节点的地址, 
# 第二个参数是任意一个已经存在的节点的IP和端口
./redis-trib.rb add-node 127.0.0.1:7006 127.0.0.1:7000

# 可以在 client 端使用下面的命令查看
redis 127.0.0.1:7006> cluster nodes

# 添加一个从节点
# 这种情况下系统会在其他的复制集中的主节点中随机选取一个作为这个从节点的主节点。
./redis-trib.rb add-node --slave 127.0.0.1:7006 127.0.0.1:7000

# 你可以通过下面的命令指定主节点:
./redis-trib.rb add-node --slave --master-id \
3c3a0c74aae0b56170ccb03a76b60cfe7dc1912e 127.0.0.1:7006 127.0.0.1:7000

# 移除一个节点
# 第一个参数是任意一个节点的地址,第二个节点是你想要移除的节点地址。
./redis-trib del-node 127.0.0.1:7000 `<node-id>`

```

##### 127.0.0.1

```
# bind 127.0.0.1
```

##### 重建集群

```sh
[wxg@localhost src]$ ./redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 \
> 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005
>>> Creating cluster
[ERR] Node 127.0.0.1:7000 is not empty. Either the node already knows other nodes (check with CLUSTER NODES) or contains some key in database 0.
```

我是把`bind 127.0.0.1`屏蔽了，看来，还需要把之前的数据都清除掉啊？！

```sh
rm -rf appendonly.aof dump.rdb nodes.conf nohup.out 

# 删除之后，目录下就只剩下 redis.conf 这个一个文件了。
```

依次启动六个节点。

再，集群

```sh

./redis-trib.rb create --replicas 1 192.168.3.20:7000 192.168.3.20:7001 \
192.168.3.20:7002 192.168.3.20:7003 192.168.3.20:7004 192.168.3.20:7005

# 上面的指定IP地址的命令，执行失败，只能执行下面的命令

./redis-trib.rb create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 \
127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005

```

关闭防火墙，

```sh
systemctl stop firewalld
# systemctl disable firewalld
systemctl stop iptables

# disable enable start stop status
```



关闭防火墙，远程客户端，终于可以访问了，但是不能操作，提示信息如下：



> (error) DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.

```sh
vim redis.conf

protected-mode no
```

##### 允许其他主机访问

我按照上面的将`bind 127.0.0.1`注释掉，并且将`protected-mode no`设置为`no`，但是，我在另一台机器上访问时，还是出现问题，比如：

```sh
# 在192.168.3.25上访问
redis-cli -h 192.168.3.26 -c -p 7000
# 能够连接上，没有提出 refused ，但是
get foo
# 以上命令却提示，7002 refused ，很烦……
```

网罗……

```sh
# 看到网上有人说，可以 bind 0.0.0.0
bind 0.0.0.0
# 也有人说，绑定多个IP地址，空格隔开
bind 127.0.0.1 192.168.3.26
```

但是，我修改成 `bind 127.0.0.1 192.168.3.26`，却得到如下的提示：

```sh
18550:C 28 Dec 17:48:35.932 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
18550:C 28 Dec 17:48:35.933 # Redis version=4.0.11, bits=64, commit=00000000, modified=0, pid=18550, just started
18550:C 28 Dec 17:48:35.933 # Configuration loaded
18550:M 28 Dec 17:48:35.935 # You requested maxclients of 10000 requiring at least 10032 max file descriptors.
18550:M 28 Dec 17:48:35.935 # Server can't set maximum open files to 10032 because of OS error: Operation not permitted.
18550:M 28 Dec 17:48:35.935 # Current maximum open files is 4096. maxclients has been reduced to 4064 to compensate for low ulimit. If you need higher maxclients increase 'ulimit -n'.
18550:M 28 Dec 17:48:35.936 # Creating Server TCP listening socket 192.168.3.26:7005: bind: Cannot assign requested address
```

`Cannot assign requested address` ，翻译一下是： 无法分配请求的地址。

显然，不能将本机的局域网IP（192.168.3.26），bind，到这里。

改为bind 0.0.0.0

```sh
bind 0.0.0.0
```

这一次，终于，可以在其他机器上访问了，哎呀！不容易啊！



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

cd /opt/rocketmq-all-4.3.0/bin

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
cd F:\wxg111_openapi\crt-parent
mvn clean package -Pdev
```

## 运行

### mysql

```sh
mysql -uroot -p

use mysql;
grant all privileges  on *.* to root@'%' identified by "password";
flush privileges;

select host,user from user;
```

因为，我在 `192.168.3.20` 上的`crt-apigateway`无法访问`192.168.3.26`上的mysql，所以，做上面的修改。

### gateway

编译，打包（将zip包上传，里面包含lib，再修改时，只需传递jar，除非lib中的jar有修改），上传到 服务器，

start.sh

```sh

#!/bin/bash

if [ "`ps -ef |grep crt-apigateway.jar|grep -v grep |awk '{print $2}'`" ];then
 for pid in `ps -ef |grep crt-apigateway.jar|grep -v grep |awk '{print $2}'`;do
   kill -9 $pid
 done
fi

java -Xms512M -Xmx512M -Xmn512M -Xss256K -Djava.nio.channels.spi.SelectorProvider=sun.nio.ch.EPollSelectorProvider -XX:+DisableExplicitGC -XX:SurvivorRatio=1 -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSParallelRemarkEnabled -XX:+UseCMSCompactAtFullCollection -XX:CMSFullGCsBeforeCompaction=0 -XX:+CMSClassUnloadingEnabled -XX:LargePageSizeInBytes=128M -XX:+UseFastAccessorMethods -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=70 -XX:SoftRefLRUPolicyMSPerMB=0 -jar crt-apigateway.jar 8081 &>8081.log &

```

stop.sh

```sh
#!/bin/bash

if [ "`ps -ef |grep crt-apigateway.jar|grep -v grep |awk '{print $2}'`" ];then
 for pid in `ps -ef |grep crt-apigateway.jar|grep -v grep |awk '{print $2}'`;do
   kill -9 $pid
 done
fi
```

启动成功后的，输出信息如下：

```sh
tail: 8081.log: file truncated
Java HotSpot(TM) 64-Bit Server VM warning: UseCMSCompactAtFullCollection is deprecated and will likely be removed in a future release.
Java HotSpot(TM) 64-Bit Server VM warning: CMSFullGCsBeforeCompaction is deprecated and will likely be removed in a future release.
Java HotSpot(TM) 64-Bit Server VM warning: MaxNewSize (524288k) is equal to or greater than the entire heap (524288k).  A new max generation size of 524224k will be used.
2018-12-28 18:30:22.153 1932 [main] INFO  c.c.o.utils.proporty.AppConfig - Loading properties file from class path resource [props/openapi-cfg-jdbc.properties]
2018-12-28 18:30:22.158 1937 [main] INFO  c.c.o.utils.proporty.AppConfig - Loading properties file from class path resource [props/openapi-cfg-app.properties]
2018-12-28 18:30:22.158 1937 [main] INFO  c.c.o.utils.proporty.AppConfig - Loading properties file from class path resource [props/openapi-cfg-redis.properties]
2018-12-28 18:30:22.159 1938 [main] INFO  c.c.o.utils.proporty.AppConfig - Loading properties file from class path resource [props/openapi-cfg-mq.properties]
2018-12-28 18:30:24.060 3839 [main] INFO  c.a.druid.pool.DruidDataSource - {dataSource-1} inited
2018-12-28 18:30:28.512 8291 [main] INFO  c.c.openapi.apigateway.StartServer - 服务启动完成~~~~监听端口：8081
```







# End