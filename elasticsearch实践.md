# Elasticsearch实践

## `Elasticsearch.yaml`配置

```yaml
##################### Elasticsearch Configuration Example ##################### 
#
# 只是挑些重要的配置选项进行注释,其实自带的已经有非常细致的英文注释了!
# https://www.elastic.co/guide/en/elasticsearch/reference/current/modules.html
#
################################### Cluster ################################### 
# 代表一个集群,集群中有多个节点,其中有一个为主节点,这个主节点是可以通过选举产生的,主从节点是对于集群内部来说的. 
# es的一个概念就是去中心化,字面上理解就是无中心节点,这是对于集群外部来说的,因为从外部来看es集群,在逻辑上是个整体,你与任何一个节点的通信和与整个es集群通信是等价的。 
# cluster.name可以确定你的集群名称,当你的elasticsearch集群在同一个网段中elasticsearch会自动的找到具有相同cluster.name的elasticsearch服务. 
# 所以当同一个网段具有多个elasticsearch集群时cluster.name就成为同一个集群的标识. 
 
# cluster.name: elasticsearch 
 
#################################### Node ##################################### 
# https://www.elastic.co/guide/en/elasticsearch/reference/5.1/modules-node.html#master-node
# 节点名称同理,可自动生成也可手动配置. 
# node.name: node-1
 
# 允许一个节点是否可以成为一个master节点,es是默认集群中的第一台机器为master,如果这台机器停止就会重新选举master. 
# node.master: true 
 
# 允许该节点存储数据(默认开启) 
# node.data: true 
 
# 配置文件中给出了三种配置高性能集群拓扑结构的模式,如下： 
# 1. 如果你想让节点从不选举为主节点,只用来存储数据,可作为负载器 
# node.master: false 
# node.data: true 
# node.ingest: false
 
# 2. 如果想让节点成为主节点,且不存储任何数据,并保有空闲资源,可作为协调器 
# node.master: true 
# node.data: false
# node.ingest: false 
 
# 3. 如果想让节点既不称为主节点,又不成为数据节点,那么可将他作为搜索器,从节点中获取数据,生成搜索结果等 
# node.master: false 
# node.data: false 
# node.ingest: true (可不指定默认开启)
 
# 4. 仅作为协调器 
# node.master: false 
# node.data: false
# node.ingest: false
 
# 监控集群状态有一下插件和API可以使用: 
# Use the Cluster Health API [http://localhost:9200/_cluster/health], the 
# Node Info API [http://localhost:9200/_nodes] or GUI tools # such as <http://www.elasticsearch.org/overview/marvel/>, 
# <http://github.com/karmi/elasticsearch-paramedic>, 
# <http://github.com/lukas-vlcek/bigdesk> and 
# <http://mobz.github.com/elasticsearch-head> to inspect the cluster state. 
 
# A node can have generic attributes associated with it, which can later be used 
# for customized shard allocation filtering, or allocation awareness. An attribute 
# is a simple key value pair, similar to node.key: value, here is an example: 
# 每个节点都可以定义一些与之关联的通用属性，用于后期集群进行碎片分配时的过滤
# node.rack: rack314 
 
# 默认情况下，多个节点可以在同一个安装路径启动，如果你想让你的es只启动一个节点，可以进行如下设置
# node.max_local_storage_nodes: 1 
 
#################################### Index #################################### 
# 设置索引的分片数,默认为5 
#index.number_of_shards: 5 
 
# 设置索引的副本数,默认为1: 
#index.number_of_replicas: 1 
 
# 配置文件中提到的最佳实践是,如果服务器够多,可以将分片提高,尽量将数据平均分布到大集群中去
# 同时,如果增加副本数量可以有效的提高搜索性能 
# 需要注意的是,"number_of_shards" 是索引创建后一次生成的,后续不可更改设置 
# "number_of_replicas" 是可以通过API去实时修改设置的 
 
#################################### Paths #################################### 
# 配置文件存储位置 
# path.conf: /path/to/conf 
 
# 数据存储位置(单个目录设置) 
# path.data: /path/to/data 
# 多个数据存储位置,有利于性能提升 
# path.data: /path/to/data1,/path/to/data2 
 
# 临时文件的路径 
# path.work: /path/to/work 
 
# 日志文件的路径 
# path.logs: /path/to/logs 
 
# 插件安装路径 
# path.plugins: /path/to/plugins 
 
#################################### Plugin ################################### 
# 设置插件作为启动条件,如果一下插件没有安装,则该节点服务不会启动 
# plugin.mandatory: mapper-attachments,lang-groovy 
################################### Memory #################################### 
# 当JVM开始写入交换空间时（swapping）ElasticSearch性能会低下,你应该保证它不会写入交换空间 
# 设置这个属性为true来锁定内存,同时也要允许elasticsearch的进程可以锁住内存,linux下可以通过 `ulimit -l unlimited` 命令 
# bootstrap.mlockall: true 
 
# 确保 ES_MIN_MEM 和 ES_MAX_MEM 环境变量设置为相同的值,以及机器有足够的内存分配给Elasticsearch 
# 注意:内存也不是越大越好,一般64位机器,最大分配内存别才超过32G 
 
############################## Network And HTTP ############################### 
# 设置绑定的ip地址,可以是ipv4或ipv6的,默认为0.0.0.0 
# network.bind_host: 192.168.0.1 
 
# 设置其它节点和该节点交互的ip地址,如果不设置它会自动设置,值必须是个真实的ip地址 
# network.publish_host: 192.168.0.1 
 
# 同时设置bind_host和publish_host上面两个参数 
# network.host: 192.168.0.1 
 
# 设置节点间交互的tcp端口,默认是9300 
# transport.tcp.port: 9300 
 
# 设置是否压缩tcp传输时的数据，默认为false,不压缩
# transport.tcp.compress: true 
 
# 设置对外服务的http端口,默认为9200 
# http.port: 9200 
 
# 设置请求内容的最大容量,默认100mb 
# http.max_content_length: 100mb 
 
# 使用http协议对外提供服务,默认为true,开启 
# http.enabled: false 
 
###################### 使用head等插件监控集群信息，需要打开以下配置项 ###########
# http.cors.enabled: true
# http.cors.allow-origin: "*"
# http.cors.allow-credentials: true
 
################################### Gateway ################################### 
# gateway的类型,默认为local即为本地文件系统,可以设置为本地文件系统 
# gateway.type: local 
 
# 下面的配置控制怎样以及何时启动一整个集群重启的初始化恢复过程 
# (当使用shard gateway时,是为了尽可能的重用local data(本地数据)) 
 
# 一个集群中的N个节点启动后,才允许进行恢复处理 
# gateway.recover_after_nodes: 1 
 
# 设置初始化恢复过程的超时时间,超时时间从上一个配置中配置的N个节点启动后算起 
# gateway.recover_after_time: 5m 
 
# 设置这个集群中期望有多少个节点.一旦这N个节点启动(并且recover_after_nodes也符合), 
# 立即开始恢复过程(不等待recover_after_time超时) 
# gateway.expected_nodes: 2
 
 ############################# Recovery Throttling ############################# 
# 下面这些配置允许在初始化恢复,副本分配,再平衡,或者添加和删除节点时控制节点间的分片分配 
# 设置一个节点的并行恢复数 
# 1.初始化数据恢复时,并发恢复线程的个数,默认为4 
# cluster.routing.allocation.node_initial_primaries_recoveries: 4 
 
# 2.添加删除节点或负载均衡时并发恢复线程的个数,默认为2 
# cluster.routing.allocation.node_concurrent_recoveries: 2 
 
# 设置恢复时的吞吐量(例如:100mb,默认为0无限制.如果机器还有其他业务在跑的话还是限制一下的好) 
# indices.recovery.max_bytes_per_sec: 20mb 
 
# 设置来限制从其它分片恢复数据时最大同时打开并发流的个数,默认为5 
# indices.recovery.concurrent_streams: 5 
# 注意: 合理的设置以上参数能有效的提高集群节点的数据恢复以及初始化速度 
 
################################## Discovery ################################## 
# 设置这个参数来保证集群中的节点可以知道其它N个有master资格的节点.默认为1,对于大的集群来说,可以设置大一点的值(2-4) 
# discovery.zen.minimum_master_nodes: 1 
# 探查的超时时间,默认3秒,提高一点以应对网络不好的时候,防止脑裂 
# discovery.zen.ping.timeout: 3s 
 
# For more information, see 
# <http://elasticsearch.org/guide/en/elasticsearch/reference/current/modules-discovery-zen.html> 
 
# 设置是否打开多播发现节点.默认是true. 
# 当多播不可用或者集群跨网段的时候集群通信还是用单播吧 
# discovery.zen.ping.multicast.enabled: false 
 
# 这是一个集群中的主节点的初始列表,当节点(主节点或者数据节点)启动时使用这个列表进行探测 
# discovery.zen.ping.unicast.hosts: ["host1", "host2:port"] 
 
# Slow Log部分与GC log部分略,不过可以通过相关日志优化搜索查询速度 
 
################  X-Pack ###########################################
# 官方插件 相关设置请查看此处
# https://www.elastic.co/guide/en/x-pack/current/xpack-settings.html
# 
############## Memory(重点需要调优的部分) ################ 
# Cache部分: 
# es有很多种方式来缓存其内部与索引有关的数据.其中包括filter cache 
 
# filter cache部分: 
# filter cache是用来缓存filters的结果的.默认的cache type是node type.node type的机制是所有的索引内部的分片共享filter cache.node type采用的方式是LRU方式.即:当缓存达到了某个临界值之后，es会将最近没有使用的数据清除出filter cache.使让新的数据进入es. 
 
# 这个临界值的设置方法如下：indices.cache.filter.size 值类型：eg.:512mb 20%。默认的值是10%。 
 
# out of memory错误避免过于频繁的查询时集群假死 
# 1.设置es的缓存类型为Soft Reference,它的主要特点是据有较强的引用功能.只有当内存不够的时候,才进行回收这类内存,因此在内存足够的时候,它们通常不被回收.另外,这些引用对象还能保证在Java抛出OutOfMemory异常之前,被设置为null.它可以用于实现一些常用图片的缓存,实现Cache的功能,保证最大限度的使用内存而不引起OutOfMemory.在es的配置文件加上index.cache.field.type: soft即可. 
 
# 2.设置es最大缓存数据条数和缓存失效时间,通过设置index.cache.field.max_size: 50000来把缓存field的最大值设置为50000,设置index.cache.field.expire: 10m把过期时间设置成10分钟. 
# index.cache.field.max_size: 50000 
# index.cache.field.expire: 10m 
# index.cache.field.type: soft 
 
# field data部分&&circuit breaker部分： 
# 用于fielddata缓存的内存数量,主要用于当使用排序,faceting操作时,elasticsearch会将一些热点数据加载到内存中来提供给客户端访问,但是这种缓存是比较珍贵的,所以对它进行合理的设置. 
 
# 可以使用值：eg:50mb 或者 30％(节点 node heap内存量),默认是：unbounded #indices.fielddata.cache.size： unbounded 
# field的超时时间.默认是-1,可以设置的值类型: 5m #indices.fielddata.cache.expire: -1 
 
# circuit breaker部分: 
# 断路器是elasticsearch为了防止内存溢出的一种操作,每一种circuit breaker都可以指定一个内存界限触发此操作,这种circuit breaker的设定有一个最高级别的设定:indices.breaker.total.limit 默认值是JVM heap的70%.当内存达到这个数量的时候会触发内存回收
 
# 另外还有两组子设置： 
#indices.breaker.fielddata.limit:当系统发现fielddata的数量达到一定数量时会触发内存回收.默认值是JVM heap的70% 
#indices.breaker.fielddata.overhead:在系统要加载fielddata时会进行预先估计,当系统发现要加载进内存的值超过limit * overhead时会进行进行内存回收.默认是1.03 
#indices.breaker.request.limit:这种断路器是elasticsearch为了防止OOM(内存溢出),在每次请求数据时设定了一个固定的内存数量.默认值是40% 
#indices.breaker.request.overhead:同上,也是elasticsearch在发送请求时设定的一个预估系数,用来防止内存溢出.默认值是1 
 
# Translog部分: 
# 每一个分片(shard)都有一个transaction log或者是与它有关的预写日志,(write log),在es进行索引(index)或者删除(delete)操作时会将没有提交的数据记录在translog之中,当进行flush 操作的时候会将tranlog中的数据发送给Lucene进行相关的操作.一次flush操作的发生基于如下的几个配置 
#index.translog.flush_threshold_ops:当发生多少次操作时进行一次flush.默认是 unlimited #index.translog.flush_threshold_size:当translog的大小达到此值时会进行一次flush操作.默认是512mb 
#index.translog.flush_threshold_period:在指定的时间间隔内如果没有进行flush操作,会进行一次强制flush操作.默认是30m #index.translog.interval:多少时间间隔内会检查一次translog,来进行一次flush操作.es会随机的在这个值到这个值的2倍大小之间进行一次操作,默认是5s 
#index.gateway.local.sync:多少时间进行一次的写磁盘操作,默认是5s
```
## 安装
### jdk
```bash
rpm -ivh jre-8u161-linux-x64.rpm
```
### elasticsearch
```
tar -xvf elasticsearch-6.2.2.tar.gz
```

## 集群


## 启动

### 查找进程关闭、启动
#### 查找进程
```bash
ps -ef | grep elastic
kill -9 进程号
```

#### 启动
```bash
elasticsearch -d
```

#### 问题
```bash
[4] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max number of threads [1024] for user [es] is too low, increase to at least [4096]
[3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[4]: system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk
```
修改`vim /etc/security/limits.conf`
添加
```
es soft nofile 65536
es hard nofile 131072
es soft nproc 4096
es hard nproc 4096
es soft memlock unlimited
es hard memlock unlimited
```

#### 问题
```bash
[2] bootstrap checks failed
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2]: system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk
```
解决办法
```
sysctl -w vm.max_map_count=262144
```
为了让配置永久生效
`vim /etc/sysctl.conf`
在文末添加以下内容
```
vm.max_map_count=262144
```
Centos6不支持SecComp，而ES6默认bootstrap.system_call_filter为true
禁用：在elasticsearch.yml中配置bootstrap.system_call_filter为false，注意要在Memory下面: 
取消bootstrap.memory_lock的注释，添加bootstrap.system_call_filter 配置
```
bootstrap.memory_lock: false 
bootstrap.system_call_filter: false
```

#### 问题
```bash
max virtual memory areas vm.max_map_count [65530] is too low, increase to at least
```
修改`vim /etc/security/limits.conf`
添加
```
es soft memlock unlimited
es hard memlock unlimited
```

## 创建用户
elasticsearc不允许root帐户运行，所以单独创建一个
```bash
groupadd es
useradd es -g es
passwd es
```

## 命令
### 基本命令
#### cat
``` bash
curl -XGET "ip:port/_cat"
```
如果经常在命令行环境下工作，cat API 对你会非常有用。用 Linux 的 cat 命令命名，这些 API 也就设计成像 linux 命令行工具一样工作了。

#### 查看nodes
```bash
curl -XGET '192.168.0.17:9200/_cat/nodes'
192.168.0.15 27 22 0 0.20 0.14 0.08 mdi * b6
192.168.0.17 26 42 3 0.14 0.12 0.07 di  - b8
```



## 文档
Elasticsearch 使用 JavaScript Object Notation（或者 JSON）作为文档的序列化格式。JSON 序列化为大多数编程语言所支持，并且已经成为 NoSQL 领域的标准格式。 它简单、简洁、易于阅读。

### 索引
#### 创建索引
索引模板
`PUT /_template/my_logs `

```json
{
  "template": "logstash-*",
  "order":    1,
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "_default_": {
      "_all": {
        "enabled": false
      }
    }
  },
  "aliases": {
    "last_3_months": {}
  }
}
```
* 创建一个名为 my_logs 的模板。
* 将这个模板应用于所有以 logstash- 为起始的索引。
* 这个模板将会覆盖默认的 logstash 模板，因为默认模板的 order 更低。
* 限制主分片数量为 1 。
* 为所有类型禁用 _all 域。
* 添加这个索引至 last_3_months 别名中。

### 搜索
`GET /megacorp/employee/_search?q=last_name:Smith`
#### 查询表达式
* 查询所有
```
GET /_search    //所有索引，所有type下的所有数据都搜索出来
{
  "query": {
    "match_all": {}
  }
}
GET /test_index/_search    //指定一个index，搜索其下所有type的数据
{
  "query": {
    "match_all": {}
  }
}
GET /test_index,test_index2/_search    //同时搜索两个index下的数据
{
  "query": {
    "match_all": {}
  }
}
GET /*1,*2/_search    //按照通配符去匹配多个索引
{
  "query": {
    "match_all": {}
  }
}
GET /test_index/test_type/_search    //搜索一个index下指定的type的数据
{
  "query": {
    "match_all": {}
  }
}
GET /test_index/test_type,test_type2/_search    //可以搜索一个index下多个type的数据
{
  "query": {
    "match_all": {}
  }
}
GET /test_index,test_index2/test_type,test_type2/_search    //搜索多个index下的多个type的数据
{
  "query": {
    "match_all": {}
  }
}
GET /_all/test_type,test_type2/_search    //可以代表搜索所有index下的指定type的数据
{
  "query": {
    "match_all": {}
  }
}
```

* match
```
GET /_search
{
  "query": {
    "match": {
      "title": "elasticsearch"
    }
  }
}
```

* multi match
```
GET /_search
{
  "query": {
    "multi_match": {
      "query": "elasticsearch",
      "fields": ["title","content"]
    }
  }
}
```

* range query
```
GET /company/employee/_search
{
  "query": {
    "range": {
      "age": {
        "gte": 30
      }
    }
  }
}
```

* term query
```
//term 查询被用于精确值 匹配，这些精确值可能是数字、时间、布尔或者那些 not_analyzed 的字符串
//term 查询对于输入的文本不 分析 ，所以它将给定的值进行精确查询
GET /_search
{
  "query": {
    "term": {
      "title":"test hello"
    }
  }
}
```

* terms query
```
//terms 查询和 term 查询一样，但它允许你指定多值进行匹配。
//如果这个字段包含了指定值中的任何一个值，那么这个文档满足条件：
{ "terms": { "tag": [ "search", "full_text", "nosql" ] }}
```

* 定制排序
查询的默认情况是按照_score排序的，然而某些情况下，可能没有有用的_score，比如说filter或constant_score

```
GET _search
{
  "query": {
    "bool": {
      "filter": {
        "term": {
          "author_id": 110
        }
      }
    }
  }
}
GET _search
{
  "query": {
    "constant_score": {
      "filter": {
        "term": {
          "author_id": 110
        }
      }
    }
  }
}
```

* 定制排序规则
```
GET /company/employee/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "range":{
          "age":{
            "gte":30
          }
        }
      }
    }
  },
  "sort": [
    {
      "join_date": {
        "order": "asc"
      }
    }
  ]
}
```

##