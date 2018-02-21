# Apache Kafka

![img](https://cdn.thenewstack.io/media/2017/02/e515ee15-kafka-fini.jpg)

Apache Kafka is fast becoming the preferred messaging infrastructure for dealing with contemporary, data-centric workloads such as Internet of Things, gaming, and online advertising. The ability to ingest data at a lightening speed makes it an ideal choice for building complex data processing pipelines.

## Why Kafka?

Before the introduction of Apache Kafka, Message Oriented Middleware (MOM) such as [Apache Qpid](https://qpid.apache.org/), [RabbitMQ](https://www.rabbitmq.com/), [Microsoft Message Queue](https://msdn.microsoft.com/en-us/library/ms711472(v=vs.85).aspx), and [IBM MQ Series](http://www.ibm.com/software/products/en/ibm-mq) were used for exchanging messages across various components. While these products are good at implementing the publisher/subscriber pattern (Pub/Sub), they are not specifically designed for dealing with large streams of data originating from thousands of publishers. Most of the MOM software have a broker that exposes [Advanced Message Queuing Protocol ](https://www.amqp.org/)(AMQP) protocol for asynchronous communication.

Kafka is designed from the ground up to deal with millions of firehose-style events generated in rapid succession. It guarantees low latency, “at-least-once”, delivery of messages to consumers. Kafka also supports retention of data for offline consumers, which means that the data can be processed either in real-time or in offline mode.

Expanding further on the persistence and retention, Kafka is designed to be a distributed commit log. Much like relational databases, it can provide a durable record of all transactions that can be played back to recover the state of a system. The key thing to understand is that the data is stored durably in an order that can be read deterministically. Due to the distributed design, Kafka provides redundancy, which ensures high availability of data even when one of the servers faces disruption.

This architecture makes Kafka the gateway for all things data. Multiple event sources can concurrently send data to a Kafka cluster, which will reliably gets delivered to multiple destinations.

## Key Concepts and Terminology

Apache Kafka uses a slightly different nomenclature than the traditional Pub/Sub systems. Let’s explore the terminology to understand it better.

![img](https://cdn.thenewstack.io/media/2017/02/5648a9e9-kafka-arch.png)

**Message** — In Kafka, messages represent the fundamental unit of data. Each message is a key/value pair. Irrespective of the data type, Kafka always converts messages into byte arrays.

**Producers** — Producers map to the publishers or the writers of the Pub/Sub architecture. They are the source that generate messages which get ingested into the system. In the context of Kafka, the producer is often referred as the client. It is important to note that the client is source of data and not to be confused with the consumer.

**Consumers** — Consumers are the subscribers or readers that receive the data. They are at the other side of the Kafka infrastructure. Unlike subscribers in MOM, Kafka consumers are stateful, which means they are responsible for remembering the cursor position, which is called as an **offset**. The consumer is also a client of Kafka cluster. Each consumer may belong to a consumer group, which will be introduced in the later sections.

> The fundamental difference between Message Oriented Middleware and Kafka is that the clients will never receive messages automatically. They have to explicitly ask for a message when they are ready to handle.

![img](https://cdn.thenewstack.io/media/2017/02/768c57c2-kafka-prod-cons.png)

**Topics** — Topics represent the logical collection of messages that belong to a group. They are similar to the topics in MOM. The data sent by the producers is stored in topics. Consumers subscribe to a specific topic that they are interested in.

**Partition** — Partitions are unique to Apache Kafka that are not seen the traditional message queuing systems. Each topic is split into one or more partitions. While sending data, producers don’t mention the partition but consumers are aware of the available partitions. Kafka may use the message key to automatically group similar messages into a partition. This scheme enables Kafka to dynamically scale the messaging infrastructure.

![img](https://cdn.thenewstack.io/media/2017/02/2208989d-kafka-log.png)

Partitions are redundantly distributed across the Kafka cluster. Messages are written to one partition but copied to at least two more partitions maintained on different brokers with the cluster.

The concept of partitions and consumer groups allows horizontal scalability of the system.

**Consumer Groups** — As mentioned earlier, consumers belong to at least one consumer group, which is typically associated with a topic. Each consumer within the group is mapped to one or more partitions of the topic. Kafka will guarantee that a message is only read by a single consumer in the group. Kafka will also ensure that all the messages belonging to the same topic are delivered to all the registered consumers of a group.

Each consumer will read from a partition while tracking the offset. If a consumer that belongs to a specific consumer group goes offline, Kafka can assign the partition to an existing consumer. Similarly, when a new consumer joins the group, it balances the association of partitions with the available consumers.

It is possible for multiple consumer groups to subscribe to the same topic. For example, in the IoT use case, a consumer group might receive messages for real-time processing through an Apache Storm cluster. A different consumer group may also receive messages from the same topic for storing them in HBase for batch processing.

The concept of partitions and consumer groups allows horizontal scalability of the system.

**Broker** — Each Kafka instance belonging to a cluster is called a broker. Its primary responsibility is to receive messages from producers, assigning offsets, and finally committing the messages to the disk. Based on the underlying hardware, each broker can easily handle thousands of partitions and millions of messages per second.

The partitions in a topic may be distributed across multiple brokers. This redundancy ensures the high availability of messages.

**Cluster** — A collection of Kafka broker forms the cluster. One of the brokers in the cluster is designated as a controller, which is responsible for handling the administrative operations as well as assigning the partitions to other brokers. The controller also keeps track of broker failures.

### Topic indexing & retention

Kafka is an ordered and indexed (by offset) log of data. Unlike a queue which doesn’t provide the ability to traverse the timeline of events, Kafka lets you traverse its message history by index. It may not be apparent at first blush, but this lets you develop a whole new class of applications.

Consider the case when you configure your Kafka topic to retain messages for some X period of time. Any new application that comes online can start processing from the oldest retained message, meaning they have a pre-determined way to read in X amount of history and bringing their state up to par with other components in the system. Also, more generally, this is a really convenient way to bootstrap new systems. With a traditional queue once a message has been delivered to its currently configured listeners, that message disappears.

![Several consumers accessing a shared Apache Kafka topic, all at different offset](http://share.ryandaigle.com/kafka-topic.png)

Additionally, Kafka itself can manage the last delivered offset per consuming system. This relieves consuming applications from the burden of having to know from which point to start consuming (new apps start at the beginning, existing apps start at their last index) and provides a very clean error recovery mechanism.

By exposing the mechanics of an ordered and indexed log, Kafka provides a level of application flexibility not possible with a simple queue.

### Compaction

Kafka not only exposes a flexible retention property, it also introduces the concept of log compaction. All messages in Kafka have a key. Compaction specifies that only the most recent value for a key is kept. So what exactly does this provide us?

![An Apache Kafka compacted topic automatically prunes duplicate entries](http://share.ryandaigle.com/kafka-topic-compacted.png)

This is where the more database-like properties of Kafka come into play. By efficiently storing a large amount of key-based data, and being able to efficiently advance through it, we now have something akin to a distributed index. Multiple consuming systems can process through a single optimized view of all their relevant data. This is an important use case for Spreedly since our primary datastore is a Riak distributed database which doesn’t provide efficient record-iteration functionality. Even if you’re not using a distributed store yourself, having the native ability to automatically compact some dataset is a useful primitive to have available.

### Operational flexibility

This is a less a direct comparison to something Kafka provides vs. traditional message queues, but is worth calling out. Kafka provides an incredible level of operational flexibility. Multiple topics can be configured on the same Kafka instance, each with their own distinct retention, compaction, security, partitioning, access, and replication settings. You can really pack a lot of very specific topic usage onto the same Kafka instance. This is a great quality, since once you have a Kafka instance provisioned, you can slice and dice to the needs of each class of application.

![Independently administer and configure different topics of the same Apache Kafka instance](http://share.ryandaigle.com/kafka-topic-isolation.png)

## The Role of ZooKeeper

Most of the contemporary distributed orchestration systems such as [Kubernetes](http://www.thenewstack.io/tag/Kubernetes) and [Swarm](http://www.thenewstack.io/tag/Swarm) rely on a distributed key/value pair for maintaining the global state of the cluster. [Consul](https://www.consul.io/), [etcd](https://github.com/coreos/etcd), and even [Redis](https://redis.io/) are used for service discovery and cluster state management. Apache Kafka was designed much before these lightweight services are built.

Like of most of the other Java-based distributed systems such as [Apache Hadoop](http://hadoop.apache.org/), Kafka uses [Apache ZooKeeper](https://zookeeper.apache.org/) as the distributed configuration store. It forms the backbone of Kafka cluster that continuously monitors the health of the brokers. When new brokers get added to the cluster, ZooKeeper will start utilizing it by creating topics and partitions on it.

Apart from cluster management, initial versions of Kafka used ZooKeeper for storing the partition and offset information for each consumer. Starting from 0.10, that information has moved to an internal Kafka topic.

