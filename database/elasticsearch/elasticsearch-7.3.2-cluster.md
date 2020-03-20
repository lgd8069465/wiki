[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@elastic](https://www.elastic.co/cn/)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[elasticsearch-7.3.2-linux-x86_64.tar.gz](https://www.elastic.co/cn/downloads/past-releases/elasticsearch-7-3-2)

~~~
mysql         -> databases -> tables      -> rows       -> columns   ->index   -> primary key
elasticsearch -> indices   -> types       -> documents  -> fields

[root@docker opt]# vi elasticsearch-7.3.2/config/elasticsearch.yml
cluster.name: my-application                                                          # 集群名称
node.name: node-1                                                                     # 当前节点名称
path.data: /home/data/es/my-application                                               # 数据目录
path.logs: /home/logs/es/my-application                                               # 日志目录
network.host: 0.0.0.0                                                                 # 监听地址
http.port: 9200                                                                       # 端口
http.cors.enabled: true                                                               # 开启跨域访问
http.cors.allow-origin: "*"                                                           # 允许所有域访问
discovery.seed_hosts: ["192.168.192.128","192.168.192.129","192.168.192.134"]         # 集群ip/hostanme
cluster.initial_master_nodes: ["node-1"]                                              # 初始化指定节点为master,不写会自动随机选举为master
discovery.zen.minimum_master_nodes: 1                                                 # master最少数量
transport.tcp.port: 9300                                                              # 集群节点相互通信端口

[root@docker opt]# curl '192.168.192.128:9200/_cat/'
=^.^=
/_cat/allocation
/_cat/shards
/_cat/shards/{index}
/_cat/master
/_cat/nodes
/_cat/tasks
/_cat/indices
/_cat/indices/{index}
/_cat/segments
/_cat/segments/{index}
/_cat/count
/_cat/count/{index}
/_cat/recovery
/_cat/recovery/{index}
/_cat/health
/_cat/pending_tasks
/_cat/aliases
/_cat/aliases/{alias}
/_cat/thread_pool
/_cat/thread_pool/{thread_pools}
/_cat/plugins
/_cat/fielddata
/_cat/fielddata/{fields}
/_cat/nodeattrs
/_cat/repositories
/_cat/snapshots/{repository}
/_cat/templates
[root@docker opt]# curl 192.168.192.128:9200/_cat/master?v
id                     host            ip              node
7Yw_5og-StmiPpXWEeld_Q 192.168.192.128 192.168.192.128 node-1
[root@docker opt]# curl 192.168.192.128:9200/_cat/nodes?v
ip              heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
192.168.192.134           34          84   0    0.03    0.34     0.55 dim       -      node-3
192.168.192.129           34          96   1    0.08    1.27     1.30 dim       -      node-2
192.168.192.128           40          97   1    0.10    0.27     0.64 dim       *      node-1
[root@docker opt]# curl 192.168.192.128:9200/_cat/count?v
epoch      timestamp count
1584439633 10:07:13  0
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/health?v
epoch      timestamp cluster status node.total node.data shards pri relo init unassign pending_tasks max_task_wait_time active_shards_percent
1584439824 10:10:24  my-application   green           3         3      0   0    0    0        0             0                  -                100.0%
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/fielddata?v
id host ip node field size
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/tasks?v
action                         task_id                       parent_task_id                type      start_time    timestamp running_time ip              node
cluster:monitor/tasks/lists    7Yw_5og-StmiPpXWEeld_Q:113028 -                             transport 1584493305792 01:01:45  818.8ms      192.168.192.128 node-1
cluster:monitor/tasks/lists[n] 7Yw_5og-StmiPpXWEeld_Q:113029 7Yw_5og-StmiPpXWEeld_Q:113028 direct    1584493306004 01:01:46  607.3ms      192.168.192.128 node-1
cluster:monitor/tasks/lists[n] MrDkg153RtG1k2PWXp0BZA:55822  7Yw_5og-StmiPpXWEeld_Q:113028 transport 1584493306135 01:01:46  1.9s         192.168.192.134 node-3
cluster:monitor/tasks/lists[n] 6vIQzkmbRP-lTSKk6eM2FA:55364  7Yw_5og-StmiPpXWEeld_Q:113028 transport 1584493306155 01:01:46  1.5s         192.168.192.129 node-2
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/pending_tasks?v
insertOrder timeInQueue priority source
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/plugins?v
name component version
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/nodeattrs?v
node   host            ip              attr              value
node-3 192.168.192.134 192.168.192.134 ml.machine_memory 1907818496
node-3 192.168.192.134 192.168.192.134 ml.max_open_jobs  20
node-3 192.168.192.134 192.168.192.134 xpack.installed   true
node-2 192.168.192.129 192.168.192.129 ml.machine_memory 1907818496
node-2 192.168.192.129 192.168.192.129 ml.max_open_jobs  20
node-2 192.168.192.129 192.168.192.129 xpack.installed   true
node-1 192.168.192.128 192.168.192.128 ml.machine_memory 1907818496
node-1 192.168.192.128 192.168.192.128 xpack.installed   true
node-1 192.168.192.128 192.168.192.128 ml.max_open_jobs  20
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/repositories?v
id type
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/templates?v
name                        index_patterns                order      version
.watch-history-10           [.watcher-history-10*]        2147483647
.ml-anomalies-              [.ml-anomalies-*]             0          7030299
.triggered_watches          [.triggered_watches*]         2147483647
.monitoring-es              [.monitoring-es-7-*]          0          7000199
.watches                    [.watches*]                   2147483647
.ml-state                   [.ml-state*]                  0          7030299
.monitoring-alerts-7        [.monitoring-alerts-7]        0          7000199
.ml-meta                    [.ml-meta]                    0          7030299
.monitoring-kibana          [.monitoring-kibana-7-*]      0          7000199
.data-frame-notifications-1 [.data-frame-notifications-*] 0          7030299
.ml-config                  [.ml-config]                  0          7030299
.logstash-management        [.logstash]                   0
.monitoring-beats           [.monitoring-beats-7-*]       0          7000199
.data-frame-internal-1      [.data-frame-internal-1]      0          7030299
.monitoring-logstash        [.monitoring-logstash-7-*]    0          7000199
.ml-notifications           [.ml-notifications]           0          7030299
[root@docker opt]#
[root@docker opt]# curl 192.168.192.128:9200/_cat/indices?v
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size
[root@docker opt]#

~~~

[elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)

[elasticsearch-analysis-pinyin](https://github.com/medcl/elasticsearch-analysis-pinyin)
