# Kafka学习

![img](https://static001.geekbang.org/infoq/7f/7fec11f02e8f922220bb079fffb26906.png)

## 1.什么是kafka

1. Apache Kafka 是一个开源 **消息** 系统，由 Scala 写成。是由 Apache 软件基金会开发的 一个开源消息系统项目。
2. Kafka 最初是由 LinkedIn 公司开发，用作 LinkedIn 的活动流（Activity Stream）和运营数据处理管道（Pipeline）的基础，现在它已被多家不同类型的公司作为多种类型的数据管道和消息系统使用。
3. **Kafka 是一个分布式消息队列**。Kafka 对消息保存时根据 Topic 进行归类，发送消息 者称为 Producer，消息接受者称为 Consumer，此外 kafka 集群有多个 kafka 实例组成，每个 实例(server)称为 broker。
4. 无论是 kafka 集群，还是 consumer 都依赖于 **Zookeeper** 集群保存一些 meta 信息， 来保证系统可用性。

### 1.1 kafka的特性

**消息系统**：kafka 与传统的消息中间件都具备系统解耦、冗余存储、流量削峰、缓冲、异步通信、扩展性、可恢复性等功能。。

**存储系统**：kafka 把消息持久化到磁盘，相比于其他基于内存存储的系统而言，有效的降低了消息丢失的风险

**流式处理平台**：kafka 为流行的流式处理框架提供了可靠的数据来源，还提供了一个完整的流式处理框架



### 1.2 kafka基本概念

![img](https://static001.geekbang.org/infoq/59/595a2a9e9ec5361c6167e76ab77fe7d6.png)

#### 概念一：生产者（Producer）与消费者（Consumer）

对于 Kafka 来说客户端有两种基本类型：**生产者**（Producer）和 **消费者**（Consumer）。除此之外，还有用来做数据集成的 Kafka Connect API 和流式处理的 **Kafka Streams** 等高阶客户端，但这些高阶客户端底层仍然是生产者和消费者 API，只不过是在上层做了封装。

- **Producer** ：消息生产者，就是向 Kafka broker 发消息的客户端；
- **Consumer** ：消息消费者，向 Kafka broker 取消息的客户端；

#### 概念二：Broker 和集群（Cluster）

一个 Kafka 服务器也称为 **Broker**，它接受生产者发送的消息并存入磁盘；Broker 同时服务消费者拉取分区消息的请求，返回目前已经提交的消息。使用特定的机器硬件，一个 Broker 每秒可以处理成千上万的分区和百万量级的消息。

![img](https://static001.geekbang.org/infoq/05/05a4e26d510e07d5b00ec9feaf35d755.png)

#### 概念三：主题（Topic）与分区（Partition）

![img](https://static001.geekbang.org/infoq/0a/0a5755ecfc6b2b79f6e58aa152fc245e.png)

在 Kafka 中，消息以 **主题**（**Topic**）来分类，每一个主题都对应一个「**消息队列**」，这有点儿类似于数据库中的表。

### 1.3 Kafka工作流程

![img](https://static001.geekbang.org/infoq/bc/bc4253e612f382855a5c6eee6f09e784.png)

#### 1.3.1 生产方式分析

**1.写入方式** 

producer 采用推（push）模式将消息发布到 broker，每条消息都被追加（append）到分区（patition）中，属于顺序写磁盘（顺序写磁盘效率比随机写内存要高，保障 kafka 吞吐率）

**2.分区**

消息发送时都被发送到一个 topic，其本质就是一个目录，而 topic 是由一些 Partition Logs(分区日志)组成，其组织结构如下图所示：

![img](https://static001.geekbang.org/infoq/bd/bdab635370bee00e39225fb70f8f8511.png)

我们可以看到，每个 Partition 中的消息都是 **有序** 的，生产的消息被不断追加到 Partition log 上，其中的每一个消息都被赋予了一个唯一的 **offset** 值。

**1）分区的原因**

1. 方便在集群中扩展，每个 Partition 可以通过调整以适应它所在的机器，而一个 topic 又可以有多个 Partition 组成，因此整个集群就可以适应任意大小的数据了；
2. 可以提高并发，因为可以以 Partition 为单位读写了。

**2）分区的原则**

1. 指定了 patition，则直接使用；
2. 未指定 patition 但指定 key，通过对 key 的 value 进行 hash 出一个 patition；
3. patition 和 key 都未指定，使用轮询选出一个 patition。

```java
DefaultPartitioner 类 
public int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster) { 
  List<PartitionInfo> partitions = cluster.partitionsForTopic(topic); 
  int numPartitions = partitions.size(); 
  if (keyBytes == null) {
    int nextValue = nextValue(topic); 
    List<PartitionInfo> availablePartitions = cluster.availablePartitionsForTopic(topic);
    if (availablePartitions.size() > 0) { 
    int part = Utils.toPositive(nextValue) % availablePartitions.size(); 
    return availablePartitions.get(part).partition();
     } else { 
     // no partitions are available, give a non-available partition 
     return Utils.toPositive(nextValue) % numPartitions; 
     } 
    } else { 
    // hash the keyBytes to choose a partition 
    return Utils.toPositive(Utils.murmur2(keyBytes)) % numPartitions; 
    }
 }
```

**3.副本**

同 一 个 partition 可 能 会 有 多 个 replication （ 对 应 `server.properties` 配 置 中 的 `default.replication.factor=N`）。没有 replication 的情况下，一旦 broker 宕机，其上所有 patition 的数据都不可被消费，同时 producer 也不能再将数据存于其上的 patition。引入 replication 之后，同一个 partition 可能会有多个 replication，而这时需要在这些 replication 之间选出一 个 leader，producer 和 consumer 只与这个 leader 交互，其它 replication 作为 follower 从 leader 中复制数据。



**4.写入流程**

producer 写入消息流程如下：

![img](https://static001.geekbang.org/infoq/6d/6d03a91b6298e975e70b0f3e7844b37e.png)

#### 1.3.2 Broker保存消息

**1.存储方式**

理上把 topic 分成一个或多个 patition（对应 `server.properties` 中的 `num.partitions=3` 配 置），每个 patition 物理上对应一个文件夹（该文件夹存储该 patition 的所有消息和索引文 件），如下：

```bash
[root@hadoop102 logs]$ ll 
drwxrwxr-x. 2 demo demo 4096 8 月 6 14:37 first-0 
drwxrwxr-x. 2 demo demo 4096 8 月 6 14:35 first-1 
drwxrwxr-x. 2 demo demo 4096 8 月 6 14:37 first-2 

[root@hadoop102 logs]$ cd first-0 
[root@hadoop102 first-0]$ ll 
-rw-rw-r--. 1 demo demo 10485760 8 月 6 14:33 00000000000000000000.index 
-rw-rw-r--. 1 demo demo 219 8 月 6 15:07 00000000000000000000.log 
-rw-rw-r--. 1 demo demo 10485756 8 月 6 14:33 00000000000000000000.timeindex 
-rw-rw-r--. 1 demo demo 8 8 月 6 14:37 leader-epoch-checkpoint
```

**2.存储策略**

无论消息是否被消费，kafka 都会保留所有消息。有两种策略可以删除旧数据：



- 基于时间：log.retention.hours=168
- 基于大小：log.retention.bytes=1073741824



需要注意的是，因为 Kafka 读取特定消息的时间复杂度为 O(1)，即与文件大小无关， 所以这里删除过期文件与提高 Kafka 性能无关。

**3.Zookeeper存储结构**

![img](https://static001.geekbang.org/infoq/a8/a8de8937ab9006fb9617345313f24838.png)

注意：producer 不在 zk 中注册，消费者在 zk 中注册。

### 1.4 kafka消费过程分析

kafka 提供了两套 consumer API：高级 Consumer API 和低级 Consumer API。

#### 1.4.1 高级API

**1）高级 API 优点**

- 高级 API 写起来简单
- 不需要自行去管理 offset，系统通过 zookeeper 自行管理。
- 不需要管理分区，副本等情况，系统自动管理。
- 消费者断线会自动根据上一次记录在 zookeeper 中的 offset 去接着获取数据（默认设置 1 分钟更新一下 zookeeper 中存的 offset）
- 可以使用 group 来区分对同一个 topic 的不同程序访问分离开来（不同的 group 记录不同的 offset，这样不同程序读取同一个 topic 才不会因为 offset 互相影响）

**2）高级 API 缺点**

- 不能自行控制 offset（对于某些特殊需求来说）
- 不能细化控制如分区、副本、zk 等

#### 1.4.2 低级API

**1）低级 API 优点**

- 能够让开发者自己控制 offset，想从哪里读取就从哪里读取。
- 自行控制连接分区，对分区自定义进行负载均衡
- 对 zookeeper 的依赖性降低（如：offset 不一定非要靠 zk 存储，自行存储 offset 即可， 比如存在文件或者内存中）

**2）低级 API 缺点**

- 太过复杂，需要自行控制 offset，连接哪个分区，找到分区 leader 等。

#### 1.4.3 消费者组

![img](https://static001.geekbang.org/infoq/7b/7b866bf0a1deef9a394a3a7ad340ad14.png)

消费者是以 consumer group 消费者组的方式工作，由一个或者多个消费者组成一个组， 共同消费一个 topic。每个分区在同一时间只能由 group 中的一个消费者读取，但是多个 group 可以同时消费这个 partition。在图中，有一个由三个消费者组成的 group，有一个消费者读取主题中的两个分区，另外两个分别读取一个分区。某个消费者读取某个分区，也可以叫做某个消费者是某个分区的拥有者。

#### 1.4.4 消费方式

consumer 采用 pull（拉）模式从 broker 中读取数据。

pull 模式不足之处是，如果 kafka 没有数据，消费者可能会陷入循环中，一直等待数据 到达。为了避免这种情况，我们在我们的拉请求中有参数，允许消费者请求在等待数据到达 的“长轮询”中进行阻塞（并且可选地等待到给定的字节数，以确保大的传输大小）。



## 2.安装kafka



## 学习资料

https://xie.infoq.cn/article/0d832da5558aff98529af397e