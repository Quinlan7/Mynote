# mongodb集群搭建

*使用的版本mongoDB7.0.2  mongosh2.0.2*

> [基本的mongodb和mongosh安装](https://blog.csdn.net/miserable_world/article/details/134272052?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22134272052%22%2C%22source%22%3A%22miserable_world%22%7D)

## 一、前置知识

### 说明

​	https://docs.mongodb.com/manual/sharding/

​	`分片(sharding)`是指`将数据拆分,将其分散存在不同机器的过程`，有时也用`分区(partitioning)`来表示这个概念,将数据分散在不同的机器上，不需要功能强大的大型计算机就能存储更多的数据，处理更大的负载。

​	分片目的是通过分片能够增加更多机器来应对不断的增加负载和数据，还不影响应用运行。

​	MongoDB支持`自动分片`,可以摆脱手动分片的管理困扰，集群自动切分数据做负载均衡。MongoDB分片的基本思想就是将集合拆分成多个块，这些快分散在若干个片里，每个片只负责总数据的一部分，应用程序不必知道哪些片对应哪些数据，甚至不需要知道数据拆分了，所以在分片之前会运行一个路由进程，mongos进程，这个路由器知道所有的数据存放位置，应用只需要直接与mongos交互即可。mongos自动将请求转到相应的片上获取数据，从应用角度看分不分片没有什么区别。

### 架构

![image-20240116124823509](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401161248684.png)

- **Shard:**	用于存储实际的数据块，实际生产环境中一个shard server角色可由几台机器组个一个replica set承担，防止主机单点故障

- **Config Server**:mongod实例，存储了整个 ClusterMetadata。

- **Query Routers**: 前端路由，客户端由此接入，且让整个集群看上去像单一数据库，前端应用可以透明使用。
- **Shard Key**: 片键，设置分片时需要在集合中选一个键,用该键的值作为拆分数据的依据,这个片键称之为(shard key)，片键的选取很重要,片键的选取决定了数据散列是否均匀。

### 仲裁节点

> 在某些情况下（例如有一个主节点和一个从节点，但由于成本约束无法添加另一个从节点），你可以在副本集中添加一个仲裁节点。仲裁节点没有数据集的副本，并且不能成为主节点。然而，仲裁节点可以参与[主节点选举](https://docs.mongodb.com/manual/core/replica-set-elections/#replica-set-elections)。一个仲裁节点只有 `1` 票选举权。

## 二、集群规划

> 实验环境：
>
> 十一台服务器：172.16.4.71-81
>
> ​							glnode01-11

```markdown
# 1.集群规划
replsetname: replshard1
- Shard Server 1：glnode01:27017
- Shard Repl1   1：glnode02:27017
- Shard Repl2   1：glnode03:27017

replsetname: replshard2
- Shard Server 2：glnode04:27017
- Shard Repl1   2：glnode05:27017
- Shard Repl2   2：glnode06:27017

replsetname: replshard3
- Shard Server 3：glnode10:27017
- Shard Repl1   3：glnode11:27017

replsetname: replconfig
- Config Server ：glnode07:27017
- Config Server ：glnode08:27017
- Config Server ：glnode09:27017

mongos:
- mongos1 :glnode07:27018
- mongos2 :glnode08:27018
- mongos3 :glnode09:27018

```



## 三、实操

> 注：
>
> 1. 本文章配置方式均选择通过配置文件配置，而非在启动命令上配置的形式
> 2. 用到的脚本文件均在附录给出

### 1. 基础安装

关于安装见文章==文章链接==

1. 先按mongodb安装方式，在一个服务器上安装（我这里选择的是glnode07）
2. 把mongodb和mongosh分发到另外是一台机器上

```
# cd /export/AutoShell/
# ./07to11s.sh /export/servers/mongoDB7.0.2 /export/servers
# ./07to11s.sh /export/servers/mongosh2.0.2/ /export/servers
```

3. 创建软连接

在所有机器上执行以下命令，可以使用xshell的工具选项卡

![image-20240116124937988](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202401161249061.png)

```
# ln -sv mongoDB7.0.2 mongodb
```

4. 设置环境变量并使其生效

同样在所有机器上执行

```
[root@glnode07 servers]# echo "PATH=$PATH:/export/servers/mongodb/bin">/etc/profile.d/mongodb.sh
[root@glnode07 servers]# cat /etc/profile.d/mongodb.sh 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/export/servers/Hadoop3.3.4/bin:/export/servers/Hadoop3.3.4/sbin:/usr/java/default/bin:/export/servers/Hive3.1.2/bin:/export/servers/mongoDB7.0.2/bin:/root/bin:/export/servers/mongodb/bin
[root@glnode07 servers]# source /etc/profile.d/mongodb.sh 
```

5. 创建数据和日志目录并增加权限

```
[root@glnode07 servers]# mkdir -p /home/mongodb/data/{db,logs}
[root@glnode07 servers]# chmod +777 /home/mongodb/data/{db,logs}
```

### 2. 配置config server副本集

1. 配置bin目录下的 mongod.conf文件

以下配置在 glnode07、glnode08、glnode09三台服务器上配置

> 注：
>
> 1. 这是新版的yaml格式配置文件，也可以使用原版的propert格式的配置文件，但是不能混合使用

```yaml
systemLog:
 destination: file
 path: /home/mongodb/data/logs/mongodb.log
 logAppend: true
storage:
 dbPath: /home/mongodb/data/db
net:
 bindIp: 0.0.0.0
 port: 27017
 maxIncomingConnections: 5000
processManagement:
 fork: true
sharding:
 clusterRole: configsvr
replication:
 replSetName: replconfig
```

可以通过以下命令分发

```
[root@glnode07 ~]# /export/AutoShell/1toN.sh "08,09" /export/servers/mongodb/bin/mongod.conf /export/servers/mongodb/bin/
```

2. 启动数据库

以下命令在 glnode07、glnode08、glnode09三台服务器上运行

```
[root@glnode07 bin]# ./mongod -f ./mongod.conf 
```

3. 服务器副本集初始化

以下内容在这三台服务器任意一台服务器执行即可

```
[root@glnode07 bin]# /export/servers/mongosh2.0.2/bin/mongosh
test> use admin                
admin> config = {_id:"replconfig",members:[             
{_id:0,host:"172.16.4.77:27017"},
{_id:1,host:"172.16.4.78:27017"},
{_id:2,host:"172.16.4.79:27017"},]
}
admin> rs.initiate(config);
```

初始化完成后，可以看到提示符变为

```
replconfig [direct: other] admin>
```

你可能好奇为什么副本集群中有个 other 角色，其实这是因为还没有初始化完成，等初始化完成就会出现对应的角色名。

```
replconfig [direct: primary] admin>
```

4. 查看副本集配置状态

```
replconfig [direct: other] admin> rs.status()
{
  set: 'replconfig',
  date: ISODate("2024-01-15T13:19:58.415Z"),
  myState: 1,
  term: Long("1"),
  syncSourceHost: '',
  syncSourceId: -1,
  configsvr: true,
  heartbeatIntervalMillis: Long("2000"),
  majorityVoteCount: 2,
  writeMajorityCount: 2,
  votingMembersCount: 3,
  writableVotingMembersCount: 3,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1705324797, i: 1 }), t: Long("1") },
    lastCommittedWallTime: ISODate("2024-01-15T13:19:57.831Z"),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1705324797, i: 1 }), t: Long("1") },
    appliedOpTime: { ts: Timestamp({ t: 1705324797, i: 1 }), t: Long("1") },
    durableOpTime: { ts: Timestamp({ t: 1705324797, i: 1 }), t: Long("1") },
    lastAppliedWallTime: ISODate("2024-01-15T13:19:57.831Z"),
    lastDurableWallTime: ISODate("2024-01-15T13:19:57.831Z")
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1705324785, i: 1 }),
  electionCandidateMetrics: {
    lastElectionReason: 'electionTimeout',
    lastElectionDate: ISODate("2024-01-15T13:18:56.535Z"),
    electionTerm: Long("1"),
    lastCommittedOpTimeAtElection: { ts: Timestamp({ t: 1705324725, i: 1 }), t: Long("-1") },
    lastSeenOpTimeAtElection: { ts: Timestamp({ t: 1705324725, i: 1 }), t: Long("-1") },
    numVotesNeeded: 2,
    priorityAtElection: 1,
    electionTimeoutMillis: Long("10000"),
    numCatchUpOps: Long("0"),
    newTermStartDate: ISODate("2024-01-15T13:18:56.584Z"),
    wMajorityWriteAvailabilityDate: ISODate("2024-01-15T13:18:57.102Z")
  },
  members: [
    {
      _id: 0,
      name: '172.16.4.77:27017',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 720,
      optime: { ts: Timestamp({ t: 1705324797, i: 1 }), t: Long("1") },
      optimeDate: ISODate("2024-01-15T13:19:57.000Z"),
      lastAppliedWallTime: ISODate("2024-01-15T13:19:57.831Z"),
      lastDurableWallTime: ISODate("2024-01-15T13:19:57.831Z"),
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: 'Could not find member to sync from',
      electionTime: Timestamp({ t: 1705324736, i: 1 }),
      electionDate: ISODate("2024-01-15T13:18:56.000Z"),
      configVersion: 1,
      configTerm: 1,
      self: true,
      lastHeartbeatMessage: ''
    },
    {
      _id: 1,
      name: '172.16.4.78:27017',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 72,
      optime: { ts: Timestamp({ t: 1705324795, i: 1 }), t: Long("1") },
      optimeDurable: { ts: Timestamp({ t: 1705324795, i: 1 }), t: Long("1") },
      optimeDate: ISODate("2024-01-15T13:19:55.000Z"),
      optimeDurableDate: ISODate("2024-01-15T13:19:55.000Z"),
      lastAppliedWallTime: ISODate("2024-01-15T13:19:57.831Z"),
      lastDurableWallTime: ISODate("2024-01-15T13:19:57.831Z"),
      lastHeartbeat: ISODate("2024-01-15T13:19:56.584Z"),
      lastHeartbeatRecv: ISODate("2024-01-15T13:19:57.592Z"),
      pingMs: Long("0"),
      lastHeartbeatMessage: '',
      syncSourceHost: '172.16.4.77:27017',
      syncSourceId: 0,
      infoMessage: '',
      configVersion: 1,
      configTerm: 1
    },
    {
      _id: 2,
      name: '172.16.4.79:27017',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 72,
      optime: { ts: Timestamp({ t: 1705324795, i: 1 }), t: Long("1") },
      optimeDurable: { ts: Timestamp({ t: 1705324795, i: 1 }), t: Long("1") },
      optimeDate: ISODate("2024-01-15T13:19:55.000Z"),
      optimeDurableDate: ISODate("2024-01-15T13:19:55.000Z"),
      lastAppliedWallTime: ISODate("2024-01-15T13:19:57.831Z"),
      lastDurableWallTime: ISODate("2024-01-15T13:19:57.831Z"),
      lastHeartbeat: ISODate("2024-01-15T13:19:56.584Z"),
      lastHeartbeatRecv: ISODate("2024-01-15T13:19:57.589Z"),
      pingMs: Long("0"),
      lastHeartbeatMessage: '',
      syncSourceHost: '172.16.4.77:27017',
      syncSourceId: 0,
      infoMessage: '',
      configVersion: 1,
      configTerm: 1
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1705324797, i: 1 }),
    signature: {
      hash: Binary.createFromBase64("AAAAAAAAAAAAAAAAAAAAAAAAAAA=", 0),
      keyId: Long("0")
    }
  },
  operationTime: Timestamp({ t: 1705324797, i: 1 })
}
```

启动成功！

### 3. 配置replshard1副本集

1. 配置bin目录下的 mongod.conf文件

以下配置在 glnode01、glnode02、glnode03三台服务器上配置

```yaml
systemLog:
 destination: file
 path: /home/mongodb/data/logs/mongodb.log
 logAppend: true
storage:
 dbPath: /home/mongodb/data/db
net:
 bindIp: 0.0.0.0
 port: 27017
 maxIncomingConnections: 5000
processManagement:
 fork: true
sharding:
 clusterRole: shardsvr
replication:
 replSetName: replshard1
```

可以通过以下命令分发

```
[root@glnode01 ~]# /export/AutoShell/1toN.sh "02,03" /export/servers/mongodb/bin/mongod.conf /export/servers/mongodb/bin/
```

2. 启动数据库

以下命令在 glnode01、glnode02、glnode03三台服务器上运行

```
[root@glnode01 bin]# ./mongod -f ./mongod.conf 
```

3. 服务器副本集初始化

以下内容在这三台服务器任意一台服务器执行即可

```
[root@glnode01 bin]# /export/servers/mongosh2.0.2/bin/mongosh
test> use admin                
admin> config = {_id:"replshard1",members:[             
{_id:0,host:"172.16.4.71:27017"},
{_id:1,host:"172.16.4.72:27017"},
{_id:2,host:"172.16.4.73:27017"},]
}
admin> rs.initiate(config);
```

4. 查看副本集配置状态

```
replconfig [direct: other] admin> rs.status()
```

### 3. 配置replshard2副本集

1. 配置bin目录下的 mongod.conf文件

以下配置在 glnode04、glnode05、glnode06三台服务器上配置

```yaml
systemLog:
 destination: file
 path: /home/mongodb/data/logs/mongodb.log
 logAppend: true
storage:
 dbPath: /home/mongodb/data/db
net:
 bindIp: 0.0.0.0
 port: 27017
 maxIncomingConnections: 5000
processManagement:
 fork: true
sharding:
 clusterRole: shardsvr
replication:
 replSetName: replshard2
```

可以通过以下命令分发

```
[root@glnode04 ~]# /export/AutoShell/1toN.sh "05,06" /export/servers/mongodb/bin/mongod.conf /export/servers/mongodb/bin/
```

2. 启动数据库

以下命令在 glnode04、glnode05、glnode06三台服务器上运行

```
[root@glnode04 bin]# ./mongod -f ./mongod.conf 
```

3. 服务器副本集初始化

以下内容在这三台服务器任意一台服务器执行即可

```
[root@glnode01 bin]# /export/servers/mongosh2.0.2/bin/mongosh
test> use admin                
admin> config = {_id:"replshard2",members:[             
{_id:0,host:"172.16.4.74:27017"},
{_id:1,host:"172.16.4.75:27017"},
{_id:2,host:"172.16.4.76:27017"},]
}
admin> rs.initiate(config);
```

4. 查看副本集配置状态

```
replconfig [direct: other] admin> rs.status()
```

### 4. 配置replshard3副本集

1. 配置bin目录下的 mongod.conf文件

以下配置在 glnode10、glnode11、glnode12三台服务器上配置

```yaml
systemLog:
 destination: file
 path: /home/mongodb/data/logs/mongodb.log
 logAppend: true
storage:
 dbPath: /home/mongodb/data/db
net:
 bindIp: 0.0.0.0
 port: 27017
 maxIncomingConnections: 5000
processManagement:
 fork: true
sharding:
 clusterRole: shardsvr
replication:
 replSetName: replshard3
```

可以通过以下命令分发

```
[root@glnode10 ~]# /export/AutoShell/1toN.sh "11" /export/servers/mongodb/bin/mongod.conf /export/servers/mongodb/bin/
```

2. 启动数据库

以下命令在 glnode04、glnode05、glnode06三台服务器上运行

```
[root@glnode10 bin]# ./mongod -f ./mongod.conf 
```

3. 服务器副本集初始化

以下内容在这三台服务器任意一台服务器执行即可

```
[root@glnode10 bin]# /export/servers/mongosh2.0.2/bin/mongosh
test> use admin                
admin> config = {_id:"replshard3",members:[             
{_id:0,host:"172.16.4.80:27017"},
{_id:1,host:"172.16.4.81:27017"}]
}
admin> rs.initiate(config);
```

4. 查看副本集配置状态

```
replconfig [direct: other] admin> rs.status()
```

### 5. 配置路由服务器 mongos

1. 配置bin目录下的 mongod.conf文件

以下配置在 glnode07、glnode08、glnode09三台服务器上配置，==注意端口号==

> mongos只是一个路由服务器的作用，它本身不存储任何数据，所以不用指定对应的dbpath，也无法配置为副本集。
>
> 那么可能大家会有疑问，如果这个节点的 mongos 宕机了，我们的业务不就连接不上了吗？
>
> 我们可以启用多个 mongos 服务，连接同一个 configserver 就可以，然后在我们的代码中同时连接多个 mongos，后面会有示例。

```yaml
systemLog:
 destination: file
 path: /home/mongodb/data/logs/mongos.log
 logAppend: true
net:
 bindIp: 0.0.0.0
 port: 27018
 maxIncomingConnections: 5000
processManagement:
 fork: true
sharding:
 configDB: replconfig/172.16.4.77:27017,172.16.4.78:27017,172.16.4.79:27017
```

可以通过以下命令分发

```
[root@glnode07 ~]# /export/AutoShell/1toN.sh "08,09" /export/servers/mongodb/bin/mongos.conf /export/servers/mongodb/bin/
```

2. 启动数据库

以下命令在 glnode07、glnode08、glnode09三台服务器上运行

```
[root@glnode07 bin]# ./mongos -f ./mongos.conf 
```

3. 添加分片服务器

以下内容在这三台服务器任意一台服务器执行即可

```
[root@glnode10 bin]# /export/servers/mongosh2.0.2/bin/mongosh localhost:27018
[direct: mongos] test> use admin                
[direct: mongos] admin> db.runCommand({addshard:"replshard1/172.16.4.71:27017,172.16.4.72:27017,172.16.4.73:27017"})
[direct: mongos] admin>
db.runCommand({addshard:"replshard2/172.16.4.74:27017,172.16.4.75:27017,172.16.4.76:27017"})
[direct: mongos] admin>
db.runCommand({addshard:"replshard3/172.16.4.80:27017,172.16.4.81:27017"})
```

4. 查看副本集配置状态

```json
[direct: mongos] admin> sh.status()
shardingVersion
{ _id: 1, clusterId: ObjectId("65a530c092f149f1c8dd0eb1") }
---
shards
[
  {
    _id: 'replshard1',
    host: 'replshard1/172.16.4.71:27017,172.16.4.72:27017,172.16.4.73:27017',
    state: 1,
    topologyTime: Timestamp({ t: 1705372649, i: 1 })
  },
  {
    _id: 'replshard2',
    host: 'replshard2/172.16.4.74:27017,172.16.4.75:27017,172.16.4.76:27017',
    state: 1,
    topologyTime: Timestamp({ t: 1705372661, i: 1 })
  },
  {
    _id: 'replshard3',
    host: 'replshard3/172.16.4.80:27017,172.16.4.81:27017',
    state: 1,
    topologyTime: Timestamp({ t: 1705372670, i: 2 })
  }
]
---
active mongoses
[ { '7.0.2': 1 } ]
---
autosplit
{ 'Currently enabled': 'yes' }
---
balancer
{
  'Currently enabled': 'yes',
  'Currently running': 'no',
  'Failed balancer rounds in last 5 attempts': 0,
  'Migration Results for the last 24 hours': 'No recent migrations'
}
---
databases
[
  {
    database: { _id: 'config', primary: 'config', partitioned: true },
    collections: {
      'config.system.sessions': {
        shardKey: { _id: 1 },
        unique: false,
        balancing: true,
        chunkMetadata: [ { shard: 'replshard1', nChunks: 1 } ],
        chunks: [
          { min: { _id: MinKey() }, max: { _id: MaxKey() }, 'on shard': 'replshard1', 'last modified': Timestamp({ t: 1, i: 0 }) }
        ],
        tags: []
      }
    }
  }
]

```

### 6. 测试

1. 对指定数据库分片

```
db.runCommand( { enablesharding :"shardingTest"});    #开启shardingTest库分片功能
```

2. 对指定表分片并指定分片字段

```
db.runCommand( { shardcollection : "shardingTest.demo",key : {_id:"hashed"} } )    #指定数据库里需要分片的集合tables和片键_id
```

查看分片信息

```
[direct: mongos] admin> db.runCommand({listshards:1})
{
  shards: [
    {
      _id: 'replshard1',
      host: 'replshard1/172.16.4.71:27017,172.16.4.72:27017,172.16.4.73:27017',
      state: 1,
      topologyTime: Timestamp({ t: 1705372649, i: 1 })
    },
    {
      _id: 'replshard2',
      host: 'replshard2/172.16.4.74:27017,172.16.4.75:27017,172.16.4.76:27017',
      state: 1,
      topologyTime: Timestamp({ t: 1705372661, i: 1 })
    },
    {
      _id: 'replshard3',
      host: 'replshard3/172.16.4.80:27017,172.16.4.81:27017',
      state: 1,
      topologyTime: Timestamp({ t: 1705372670, i: 2 })
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1705373080, i: 2 }),
    signature: {
      hash: Binary.createFromBase64("AAAAAAAAAAAAAAAAAAAAAAAAAAA=", 0),
      keyId: Long("0")
    }
  },
  operationTime: Timestamp({ t: 1705373080, i: 2 })
}
```

3. 插入数据测试

```
[direct: mongos] admin> use shardingTest
switched to db shardingTest
[direct: mongos] shardingTest> for (var i = 1; i <= 100000; i++) db.demo.save({_id:i,"test1":"testval1"+i});
```

4. 查看分片情况

省去了很多信息

```
[direct: mongos] shardingTest> db.demo.stats()
{
  ok: 1,
  capped: false,
  shards: {
    replshard3: {
      size: 1312761,
      count: 33755,
      avgObjSize: 38,
      numOrphanDocs: 0,
      storageSize: 1097728,
      freeStorageSize: 589824,
      capped: false,
      nindexes: 2,
      indexBuilds: [],
      totalIndexSize: 2801664,
      indexSizes: { _id_: 1167360, _id_hashed: 1634304 },
      totalSize: 3899392,
      scaleFactor: 1
    },
    replshard1: {
      size: 1288823,
      count: 33143,
      avgObjSize: 38,
      numOrphanDocs: 0,
      storageSize: 1040384,
      freeStorageSize: 528384,
      capped: false,
      nindexes: 2,
      indexBuilds: [],
      totalIndexSize: 2748416,
      indexSizes: { _id_: 1150976, _id_hashed: 1597440 },
      totalSize: 3788800,
      scaleFactor: 1
    },
    replshard2: {
      size: 1287311,
      count: 33102,
      avgObjSize: 38,
      numOrphanDocs: 0,
      storageSize: 1040384,
      freeStorageSize: 528384,
      capped: false,
      nindexes: 2,
      indexBuilds: [],
      totalIndexSize: 2785280,
      indexSizes: { _id_: 1167360, _id_hashed: 1617920 },
      totalSize: 3825664,
      scaleFactor: 1
    }
  },
  sharded: true,
  size: 3888895,
  count: 100000,
  numOrphanDocs: 0,
  storageSize: 3178496,
  totalIndexSize: 8335360,
  totalSize: 11513856,
  indexSizes: { _id_: 3485696, _id_hashed: 4849664 },
  avgObjSize: 38,
  ns: 'shardingTest.demo',
  nindexes: 2,
  scaleFactor: 1
}
```

这种方式也可以

```
[direct: mongos] shardingTest> db.demo.getShardDistribution()
Shard replshard2 at replshard2/172.16.4.74:27017,172.16.4.75:27017,172.16.4.76:27017
{
  data: '1.22MiB',
  docs: 33102,
  chunks: 2,
  'estimated data per chunk': '628KiB',
  'estimated docs per chunk': 16551
}
---
Shard replshard3 at replshard3/172.16.4.80:27017,172.16.4.81:27017
{
  data: '1.25MiB',
  docs: 33755,
  chunks: 2,
  'estimated data per chunk': '640KiB',
  'estimated docs per chunk': 16877
}
---
Shard replshard1 at replshard1/172.16.4.71:27017,172.16.4.72:27017,172.16.4.73:27017
{
  data: '1.22MiB',
  docs: 33143,
  chunks: 2,
  'estimated data per chunk': '629KiB',
  'estimated docs per chunk': 16571
}
---
Totals
{
  data: '3.7MiB',
  docs: 100000,
  chunks: 6,
  'Shard replshard2': [
    '33.1 % data',
    '33.1 % docs in cluster',
    '38B avg obj size on shard'
  ],
  'Shard replshard3': [
    '33.75 % data',
    '33.75 % docs in cluster',
    '38B avg obj size on shard'
  ],
  'Shard replshard1': [
    '33.14 % data',
    '33.14 % docs in cluster',
    '38B avg obj size on shard'
  ]
}

```



## 四、springboot连接mongodb集群

yaml格式配置文件

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://172.16.4.77:27018,172.16.4.78:27018,172.16.4.79:27018/trafficData
      max-idle: 100
      min-idle: 10
      max-active: 150
      max-wait: 1000
```

> SpringBoot Data 包中自带连接池的设置，不需要像很多教程中的设置，自己编写一个mongodb连接池的工具类。
>
> 这样可以同时连接三个mongos服务，当其中一个宕机时，也可以稳定

## 五、常见问题

### 1. 在分片集群上创建分片字段时报错

MongoServerError: Please create an index that starts with the proposed shard key before sharding the collection. 

```
[direct: mongos] admin> db.runCommand( { shardcollection : "trafficData.K3index",key : {_id:"hashed"} } )
MongoServerError: Please create an index that starts with the proposed shard key before sharding the collection. 
```

> 问题原因：当集合中已有数据时，再创建分片信息，会产生这个错误
>
> 解决方法：
>
> 1. 清空集合再创建分片信息
> 2. 显式的创建索引
>
> ```
> // 在 trafficData 数据库中的 K3index 集合上为 _id 字段创建哈希索引
> db.runCommand({
>   createIndexes: "K3index",
>   indexes: [
>     {
>       key: { _id: "hashed" },
>       name: "_id_hashed_index"
>     }
>   ]
> })
> ```

### 2. 当启动副本集群后某个节点转台一直是 startup

错误原因：无法建立通信

解决方法：检查防火墙设置







## 六、附录

### 1. 1toN.sh脚本源代码

```shell
#!/bin/bash

if [ $# -ne 3 ]
then
  printf "Usage:                                                     \n"
  printf "    1toN.sh \"01,04-05,08-12\" SrcFile TargetDir           \n"
  printf "该命令会将本机的SrcFile文件(夹)传输至01,04,05,08,09,10,11, \n"
  printf "12主机的TargetDir路径下                                    \n"
  printf "                                                           \n"
  printf "PS:                                                        \n"
  printf "    + 目标主机编号应使用双引号，源文件(夹)和目标路径不需要 \n"
  exit
fi

array=(${1//,/ })
for arra in ${array[@]}
do
  if (( ${#arra} == 2 ))
  then
    # do somethings
    echo "#################### glnode${arra} ####################"
    # scp -r ${2} root@glnode${arra}:${3}>/export/AutoShell/1toN.log
    script /export/AutoShell/scp.log -q -c "scp -r ${2} root@glnode${arra}:${3}"
    echo "#################### glnode${arra} ####################"
    echo 
  else
    arr=(${arra[@]})
    start=`echo ${arra:0:2}|bc`
    end=`echo ${arra:3:2}|bc`
    while (($start <= $end))
    do
      ar=`printf "%02d" $start`
      start=$(($start+1))
      # do somethings
      echo "#################### glnode${ar} ####################"
      # scp -r ${2} root@glnode${ar}:${3}>/export/AutoShell/1toN.log
      script /export/AutoShell/scp.log -q -c "scp -r ${2} root@glnode${ar}:${3}"
      echo "#################### glnode${ar} ####################"
      echo
    done
  fi
done


db.runCommand( { shardcollection : "trafficData.K3index",key : {_id:"hashed"} } )
```





### 2. 07to11s.sh 脚本代码

```shell
if [ $# -ne 2 ]
then
	echo -e "usage:\n    1to11.sh srcFile targetDir\n\n  srcFile: 需要被分发的文件(夹)\n  targetDir: 被分发的主机的目的路径\n\n  该命令将源文件分发至处本机外的11台主机\n"
	exit;
fi

for i in "01" "02" "03" "04" "05" "06" "08" "09" "10" "11" "12" 
do 
	echo ==================== "glnode"$i ====================
	
	# 检查目标目录是否存在，如果不存在则创建
   	ssh root@glnode${i} "[ -d ${2} ] || mkdir -p ${2}"

  	# 使用 scp 命令将源文件（或文件夹）复制到目标主机的指定目录
	scp -r $1 root@glnode${i}:${2}
	# echo scp -r $1 root@glnode${i}:${2}
done

```

