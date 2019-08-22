# Docker下Elasticsearch的使用

> 目录:[https://github.com/dolyw/DockerStudy](https://github.com/dolyw/DockerStudy)

#### Docker下Elasticsearch的使用

当然首先启动Docker，可以去`Docker Hub`: [https://hub.docker.com/_/elasticsearch?tab=tags](https://hub.docker.com/_/elasticsearch?tab=tags)查询下Tag版本

可以先`docker search elasticsearch`查询一下，看下连接有没有问题

然后直接使用命令`docker pull elasticsearch`下载最新版本，可以加上冒号版本号下载对应版本`docker pull elasticsearch:7.3.0`，这里我们使用7.3.0版本

```
PS C:\> docker search elasticsearch
NAME                                 DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
elasticsearch                        Elasticsearch is a powerful open source sear…   3801                [OK]
nshou/elasticsearch-kibana           Elasticsearch-7.1.1 Kibana-7.1.1                104                                     [OK]
itzg/elasticsearch                   Provides an easily configurable Elasticsearc…   67                                      [OK]
mobz/elasticsearch-head              elasticsearch-head front-end and standalone …   47
elastichq/elasticsearch-hq           Official Docker image for ElasticHQ: Elastic…   36                                      [OK]
...
PS C:\> docker pull elasticsearch:7.3.0
7.3.0: Pulling from library/elasticsearch
8ba884070f61: Pull complete
854c9f9b1064: Pull complete
44d43a907bb5: Pull complete
9311c5f24d75: Pull complete
91363c70bdb7: Pull complete
38b4cb8c47ad: Pull complete
c22bd5067efd: Pull complete
Digest: sha256:ba2ef018238cc05e9e44d72228002b4fabe202801951caaa265ce080deb97133
Status: Downloaded newer image for elasticsearch:7.3.0
docker.io/library/elasticsearch:7.3.0
PS C:\>
```

这样就下载完成了，先使用命令`docker images`查询Elasticsearch镜像ID

```
PS C:\> docker images
REPOSITORY          TAG                               IMAGE ID            CREATED             SIZE
tomcat              8.5.43-jdk8-adoptopenjdk-openj9   689bdcef64fe        22 hours ago        339MB
elasticsearch       7.3.0                             bdaab402b220        3 weeks ago         806MB
```

在启动前我们先在自己主机建立ES的配置文件elasticsearch.yml和一个data空目录

```yml
# 设置支持Elasticsearch-Head
http.cors.enabled: true
http.cors.allow-origin: "*"
# 设置集群Master配置信息
cluster.name: myEsCluster
# 节点的名字，一般为Master或者Slave
node.name: master
# 节点是否为Master，设置为true的话，说明此节点为Master节点
node.master: true
# 设置网络，如果是本机的话就是127.0.0.1，其他服务器配置对应的IP地址即可(0.0.0.0支持外网访问)
network.host: 0.0.0.0
# 设置对外服务的Http端口，默认为 9200，可以修改默认设置
http.port: 9200
# 设置节点间交互的TCP端口，默认是9300
transport.tcp.port: 9300
# 手动指定可以成为Master的所有节点的Name或者IP，这些配置将会在第一次选举中进行计算
cluster.initial_master_nodes: ["master"]
```

然后直接用下面命令启动Elasticsearch容器，加-d就表示后台运行，配置文件和空目录如下对应起来

``` 
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -v D:/tools/docker/elasticsearch/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v D:/tools/docker/elasticsearch/data:/usr/share/elasticsearch/data --name es -p 9200:9200 -p 9300:9300 bdaab402b220
```

注：设置-e ES_JAVA_OPTS="-Xms256m -Xmx256m"是因为/etc/elasticsearch/jvm.options默认JVM最大最小内存是2G，读者启动容器后 可用`docker stats`命令查看

可能会需要确认是否共享磁盘，都确认就可以，等一会启动成功浏览器查看: [http://127.0.0.1:9200](http://127.0.0.1:9200)，返回下面的字符，代表启动成功

```json
{
  "name" : "master",
  "cluster_name" : "myEsCluster",
  "cluster_uuid" : "5SZ13bMbSKOx1Nd5iIyruA",
  "version" : {
    "number" : "7.3.0",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "de777fa",
    "build_date" : "2019-07-24T18:30:11.767338Z",
    "build_snapshot" : false,
    "lucene_version" : "8.1.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

#### Docker下Elasticsearch的集群

建立三个配置文件elasticsearch1.yml，elasticsearch2.yml，elasticsearch3.yml和三个data文件夹，data1，data2，data3

* elasticsearch1.yml

```yml
# 设置支持Elasticsearch-Head
http.cors.enabled: true
http.cors.allow-origin: "*"
# 设置集群Master配置信息
cluster.name: myEsCluster
# 节点的名字，一般为Master或者Slave
node.name: master
# 节点是否为Master，设置为true的话，说明此节点为Master节点
node.master: true
# 设置网络，如果是本机的话就是127.0.0.1，其他服务器配置对应的IP地址即可(0.0.0.0支持外网访问)
network.host: 0.0.0.0
# 设置对外服务的Http端口，默认为 9200，可以修改默认设置
http.port: 9500
# 设置节点间交互的TCP端口，默认是9300
transport.tcp.port: 9300
# 手动指定可以成为Master的所有节点的Name或者IP，这些配置将会在第一次选举中进行计算
cluster.initial_master_nodes: ["master"]
# 集群发现节点信息，一般为其他节点IP加交互端口，这里一般填主机IP
discovery.seed_hosts: ["192.168.2.58:9301", "192.168.2.58:9302"]
```

* elasticsearch2.yml

```yml
# 设置集群Slave配置信息
cluster.name: myEsCluster
# 节点的名字，一般为Master或者Slave
node.name: slave1
# 节点是否为Master，设置为true的话，说明此节点为master节点
node.master: false
# 设置对外服务的Http端口，默认为 9200，可以修改默认设置
http.port: 9600
# 设置节点间交互的TCP端口，默认是9300
transport.tcp.port: 9301
# 设置网络，如果是本机的话就是127.0.0.1，其他服务器配置对应的IP地址即可(0.0.0.0支持外网访问)
network.host: 0.0.0.0
# 集群发现节点信息，一般为其他节点IP加交互端口，这里一般填主机IP
discovery.seed_hosts: ["192.168.2.58:9300", "192.168.2.58:9302"]
```

* elasticsearch3.yml

```yml
# 设置集群Slave配置信息
cluster.name: myEsCluster
# 节点的名字，一般为Master或者Slave
node.name: slave2
# 节点是否为Master，设置为true的话，说明此节点为master节点
node.master: false
# 设置对外服务的Http端口，默认为 9200，可以修改默认设置
http.port: 9700
# 设置节点间交互的TCP端口，默认是9300
transport.tcp.port: 9302
# 设置网络，如果是本机的话就是127.0.0.1，其他服务器配置对应的IP地址即可(0.0.0.0支持外网访问)
network.host: 0.0.0.0
# 集群发现节点信息，一般为其他节点IP加交互端口，这里一般填主机IP
discovery.seed_hosts: ["192.168.2.58:9300", "192.168.2.58:9301"]
```

然后启动三个，一个Master，两个Slave

``` 
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -v D:/tools/docker/elasticsearch/elasticsearch1.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v D:/tools/docker/elasticsearch/data1:/usr/share/elasticsearch/data --name es1 -p 9500:9500 -p 9300:9300 bdaab402b220
```

``` 
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -v D:/tools/docker/elasticsearch/elasticsearch2.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v D:/tools/docker/elasticsearch/data2:/usr/share/elasticsearch/data --name es2 -p 9600:9600 -p 9301:9301 bdaab402b220
```

``` 
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -v D:/tools/docker/elasticsearch/elasticsearch3.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v D:/tools/docker/elasticsearch/data3:/usr/share/elasticsearch/data --name es3 -p 9700:9700 -p 9302:9302 bdaab402b220
```

等一会启动成功浏览器查看集群节点信息: [http://127.0.0.1:9500/_cat/nodes?v](http://127.0.0.1:9500/_cat/nodes?v)

```
ip         heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
172.17.0.2           34          95   3    0.69    0.50     0.20 dim       *      master
172.17.0.4           49          95   3    0.69    0.50     0.20 di        -      slave1
172.17.0.3           48          95   3    0.69    0.50     0.20 di        -      slave1
```

* Docker集群就OK了

#### Docker下Elasticsearch-Head的安装

可以先`docker search elasticsearch`查询一下，看下有没有问题，然后直接使用命令`docker pull mobz/elasticsearch-head:5`下载，加上冒号版本号下载对应版本，这里我们使用5版本

下载完成了，直接启动

```
docker run -d --name es-head -p 9100:9100 mobz/elasticsearch-head:5
```

等一会启动成功浏览器查看: [http://127.0.0.1:9100](http://127.0.0.1:9100)，把连接地址改成[http://localhost:9500](http://localhost:9500)，点击连接即可，连接成功，可以看到三个节点的集群信息，就这样Elasticsearch-Head就安装成功了

#### 搭建参考

1. 感谢东北小狐狸的【拆分版】Docker-compose构建Elasticsearch 7.1.0集群: [https://www.cnblogs.com/hellxz/p/docker_es_cluster.html](https://www.cnblogs.com/hellxz/p/docker_es_cluster.html)
2. 感谢belonghuang157405的docker简易搭建ElasticSearch集群: [https://blog.csdn.net/belonghuang157405/article/details/83301937](https://blog.csdn.net/belonghuang157405/article/details/83301937)
3. 感谢ZeroOne01的使用docker安装elasticsearch伪分布式集群以及安装ik中文分词插件: [https://blog.51cto.com/zero01/2285604](https://blog.51cto.com/zero01/2285604)