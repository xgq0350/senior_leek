docker安装etcd
创建网络
docker network create app-tier --driver bridge
创建etcd容器
docker run --it --name Etcd-server --network app-tier --publish 2379:2379 --publish 2380:2380  --env ALLOW_NONE_AUTHENTICATION=yes  --env ETCD_ADVERTISE_CLIENT_URLS=http://etcd-server:2379 bitnami/etcd:latest
进入容器
docker exec --it Etcd-server

Etcd介绍
gRPC server： 用来处理客户端的连接，也用来处理集群其他节点之间的通信，都是采用protobuf来传输数据。
raft共识算法：用来保证集群一致性
wal（write ahead log）：预写式日志实现事务日志的标准方法，两段式提交。执行写操作前先写日志，跟 mysql中 redo_log 类似，wal 实现的是顺序写，而若按照 B+ 树写，则涉及到多次 io 以及 随机写。
snapshot 快照数据：用于其他节点（新加入节点或从节点数据落后太多）同步主节点数据从而达到一致性地状态，类似 redis 中主从复制中 rdb 数据恢复。流程：leader生成 snapshot，leader向follower发送snapshot，follower接收并应用snapshot。
boltdb是一个单机的支持事务的 kv 存储，etcd 的事务是基于 boltdb 的事务实现的。boltdb 为每一个 key 都创建一个索引（B+树），该 B+ 树存储了 key 所对应的版本数据，所以可以通过这个B+树索引出历史信息

Etcd命令使用
增加或者修改key        PUT key val
查找key   GET key
范围查找，范围是左闭右开区间，key按字典序查找
GET keyfrom keyend
前缀索引  GET --prefix key
删除key  DEL key
监听，用来实现监听和推送服务
WATCH key
WATCH --prefix key
当监听的key发生变化（增删改）时，终端会输出这个变化的操作，注意不响应查找操作