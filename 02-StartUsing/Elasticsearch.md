# Tomcat

> 目录:[https://github.com/dolyw/DockerStudy](https://github.com/dolyw/DockerStudy)

#### `Docker`下`Elasticsearch`的安装

当然首先启动`Docker`，然后去`Docker Hub`: [https://hub.docker.com/_/elasticsearch?tab=tags](https://hub.docker.com/_/elasticsearch?tab=tags)查询下Tag版本

可以先`docker search elasticsearch`查询一下，看下连接有没有问题

然后直接使用命令`docker pull elasticsearch`下载最新版本，可以加上版本号下载对应版本`docker pull elasticsearch:7.3.0`，这里我们使用`7.3.0`版本

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

这样就下载完成了，先使用命令`docker images`查询`Elasticsearch`镜像ID

```
PS C:\> docker images
REPOSITORY          TAG                               IMAGE ID            CREATED             SIZE
tomcat              8.5.43-jdk8-adoptopenjdk-openj9   689bdcef64fe        22 hours ago        339MB
elasticsearch       7.3.0                             bdaab402b220        3 weeks ago         806MB
```

直接用命令`docker run -d --name es -p 9200:9200 -p 9300:9300 bdaab402b220`启动`Elasticsearch`容器

启动失败，记录一下

```
ERROR: [1] bootstrap checks failed
[1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
{"type": "server", "timestamp": "2019-08-16T09:16:21,507+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "stopping ..."  }
{"type": "server", "timestamp": "2019-08-16T09:16:21,540+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "stopped"  }
{"type": "server", "timestamp": "2019-08-16T09:16:21,541+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "closing ..."  }
{"type": "server", "timestamp": "2019-08-16T09:16:21,555+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "closed"  }
{"type": "server", "timestamp": "2019-08-16T09:16:21,557+0000", "level": "INFO", "component": "o.e.x.m.p.NativeController", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "Native controller process has stopped - no new native processes can be started"  }
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
{"type": "server", "timestamp": "2019-08-16T09:19:35,554+0000", "level": "INFO", "component": "o.e.e.NodeEnvironment", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "using [1] data paths, mounts [[/ (overlay)]], net usable_space [53.6gb], net total_space [58.8gb], types [overlay]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:35,557+0000", "level": "INFO", "component": "o.e.e.NodeEnvironment", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "heap size [1007.3mb], compressed ordinary object pointers [true]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:35,565+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "node name [4b3b20c7e066], node ID [HrDiSkjbQYqvTJMNSi1hqg], cluster name [docker-cluster]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:35,565+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "version[7.3.0], pid[1], build[default/docker/de777fa/2019-07-24T18:30:11.767338Z], OS[Linux/4.9.184-linuxkit/amd64], JVM[Oracle Corporation/OpenJDK 64-Bit Server VM/12.0.1/12.0.1+12]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:35,566+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "JVM home [/usr/share/elasticsearch/jdk]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:35,566+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "JVM arguments [-Xms1g, -Xmx1g, -XX:+UseConcMarkSweepGC, -XX:CMSInitiatingOccupancyFraction=75, -XX:+UseCMSInitiatingOccupancyOnly, -Des.networkaddress.cache.ttl=60, -Des.networkaddress.cache.negative.ttl=10, -XX:+AlwaysPreTouch, -Xss1m, -Djava.awt.headless=true, -Dfile.encoding=UTF-8, -Djna.nosys=true, -XX:-OmitStackTraceInFastThrow, -Dio.netty.noUnsafe=true, -Dio.netty.noKeySetOptimization=true, -Dio.netty.recycler.maxCapacityPerThread=0, -Dlog4j.shutdownHookEnabled=false, -Dlog4j2.disable.jmx=true, -Djava.io.tmpdir=/tmp/elasticsearch-11874788328507458153, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=data, -XX:ErrorFile=logs/hs_err_pid%p.log, -Xlog:gc*,gc+age=trace,safepoint:file=logs/gc.log:utctime,pid,tags:filecount=32,filesize=64m, -Djava.locale.providers=COMPAT, -Des.cgroups.hierarchy.override=/, -Dio.netty.allocator.type=unpooled, -XX:MaxDirectMemorySize=536870912, -Des.path.home=/usr/share/elasticsearch, -Des.path.conf=/usr/share/elasticsearch/config, -Des.distribution.flavor=default, -Des.distribution.type=docker, -Des.bundled_jdk=true]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,503+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [aggs-matrix-stats]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,503+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [analysis-common]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,503+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [data-frame]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,504+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [flattened]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,504+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [ingest-common]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,504+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [ingest-geoip]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,505+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [ingest-user-agent]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,505+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [lang-expression]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,505+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [lang-mustache]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,505+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [lang-painless]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,506+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [mapper-extras]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,506+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [parent-join]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,506+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [percolator]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,506+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [rank-eval]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,507+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [reindex]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,507+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [repository-url]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,510+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [transport-netty4]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,519+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [vectors]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,520+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-ccr]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,520+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-core]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,521+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-deprecation]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,521+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-graph]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,521+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-ilm]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,521+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-logstash]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,522+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-ml]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,522+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-monitoring]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,522+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-rollup]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,522+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-security]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,523+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-sql]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,523+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-voting-only-node]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,523+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "loaded module [x-pack-watcher]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:37,524+0000", "level": "INFO", "component": "o.e.p.PluginsService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "no plugins loaded"  }
{"type": "server", "timestamp": "2019-08-16T09:19:41,731+0000", "level": "INFO", "component": "o.e.x.s.a.s.FileRolesStore", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "parsed [0] roles from file [/usr/share/elasticsearch/config/roles.yml]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:42,377+0000", "level": "INFO", "component": "o.e.x.m.p.l.CppLogMessageHandler", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "[controller/101] [Main.cc@110] controller (64 bit): Version 7.3.0 (Build ff2f774f78ce63) Copyright (c) 2019 Elasticsearch BV"  }
{"type": "server", "timestamp": "2019-08-16T09:19:42,960+0000", "level": "DEBUG", "component": "o.e.a.ActionModule", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "Using REST wrapper from plugin org.elasticsearch.xpack.security.Security"  }
{"type": "server", "timestamp": "2019-08-16T09:19:43,584+0000", "level": "INFO", "component": "o.e.d.DiscoveryModule", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "using discovery type [zen] and seed hosts providers [settings]"  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,684+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "initialized"  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,685+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "starting ..."  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,865+0000", "level": "INFO", "component": "o.e.t.TransportService", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "publish_address {172.17.0.2:9300}, bound_addresses {0.0.0.0:9300}"  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,873+0000", "level": "INFO", "component": "o.e.b.BootstrapChecks", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "bound or publishing to a non-loopback address, enforcing bootstrap checks"  }
ERROR: [1] bootstrap checks failed
[1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
{"type": "server", "timestamp": "2019-08-16T09:19:44,881+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "stopping ..."  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,910+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "stopped"  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,910+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "closing ..."  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,932+0000", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "closed"  }
{"type": "server", "timestamp": "2019-08-16T09:19:44,934+0000", "level": "INFO", "component": "o.e.x.m.p.NativeController", "cluster.name": "docker-cluster", "node.name": "4b3b20c7e066",  "message": "Native controller process has stopped - no new native processes can be started"  }
```

#### `Docker`下`Elasticsearch`的集群

直接用下面三个命令启动三个`Elasticsearch`容器

```
docker run -d --name es1 -p 9200:9200 -p 9300:9300 bdaab402b220
docker run -d --name es2 -p 9201:9200 -p 9301:9300 bdaab402b220
docker run -d --name es3 -p 9202:9200 -p 9302:9300 bdaab402b220
```

结果

```
PS C:\> docker run -d --name es1 -p 9200:9200 -p 9300:9300 bdaab402b220
9cdcf754433647f5142e7537d3d57e69377c7ec38caeab92e5bc900f93d92720
PS C:\> docker run -d --name es2 -p 9201:9200 -p 9301:9300 bdaab402b220
0d5c688cadde11764522cfbc8623651423834688cfe40f958303e249e1c59fdb
PS C:\> docker run -d --name es3 -p 9202:9200 -p 9302:9300 bdaab402b220
354f398ccf70b0aa779c7ef4c2a2edcdf1dd114d036685d6981e98ffb8638bab
```

使用命令`Elasticsearch`查看运行的容器
