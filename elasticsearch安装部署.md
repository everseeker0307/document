# elasticsearch安装部署

## 安装

1. rpm安装
```
rpm -ivh elasticsearch-6.1.3.rpm
```

2. 配置  
/etc/elasticsearch/elasticsearch.yml 修改部分参数:
```
network.host: 10.201.0.31
http.port: 9200
```

3. 启动
```
systemctl start elasticsearch
```

## 重要配置的修改

1. 指定集群名字和节点名字

```
# 默认集群名字为 elasticsearch 。如果不更改，可能会出现某些服务器或者笔记本加入了集群这种意外。
cluster.name: elasticsearch_glsc    
# 默认节点名是启动的时候随机产生，每次启动都会变化。如果不指定，查询日志时无法通过节点名过滤。
node.name: glsc_001
```

2. 指定路径

```
# 默认情况下，数据、日志、插件在安装目录下；如果重新安装不小心覆盖，会丢失所有数据；建议更改。
# 注意：自定义的目录需要文件拥有者为 elasticsearch ，参考命令: chown -R elasticsearch:elasticsearch /opt/elasticsearch_glsc
path.data: /opt/elasticsearch_glsc/data
path.logs: /opt/elasticsearch_glsc/logs
```

3. 最小主节点数

```
# master(主节点)拥有集群的最高权限，可以决定索引的创建，分片的移动等，必须唯一，否则数据的完整性得不到保证。
# 这个参数指定了当最少有几个节点意见一致时，可以选举 master ，一般配置为 master 候选节点个数/2+1。
# 比如一共10个节点，配置为6，能保证可以选举出唯一的 master ；如果配置为5，可能会选举出2个 master 。
# 如果你有3个候选 master 节点，和100个data节点，法定数就是2，你只要数数那些可以做master的节点数就可以了。
# 如果有2个节点就麻烦了。如果配置为2，意味着如果一个节点挂掉，整个集群就不可用；如果配置为1，那么可能出现2个 master 。
# 所以建议节点数至少为3个，然后参数配置为( master 候选节点个数/2+1)即可。
discovery.zen.minimum_master_nodes: 2
```

动态增加或者减少节点数时，需要修改 discovery.zen.minimum_master_nodes 的值。不用修改配置文件并重启集群以让配置生效，可以通过调用 api 的方式:
```
PUT /_cluster/settings
{
    "persistent" : {
        "discovery.zen.minimum_master_nodes" : 2
    }
}
```

4. 使用单播替代组播

Elasticsearch 默认被配置为使用单播发现，以防止节点无意中加入集群。只有在同一台机器上运行的节点才会自动组成集群。

使用单播，你应该为 Elasticsearch 提供一些它应该去尝试连接的节点列表；否则，它无法自动查找所属的集群。当一个节点联系到单播列表中的成员时，它就会得到整个集群所有节点的状态，然后它会联系 master 节点，并加入集群，因此，你也没有必要将集群中所有成员加入到尝试列表中。
```
discovery.zen.ping.unicast.hosts: ["10.201.0.28", "10.201.0.29", "10.201.0.31"]
```
