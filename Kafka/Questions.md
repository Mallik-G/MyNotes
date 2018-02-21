Questions

#### Why is Kafka so fast?

Kafka is fast because it avoids copying buffers in-memory (Zero Copy), and streams data to immutable logs instead of using random access.

#### How fast is Kafka usage growing?

When you consider Kafka is six years old, and over 1⁄3 of fortune 500 companies use Kafka, then the only answer is fast, very fast.

#### How is Kafka getting used?

Kafka is used to feed data lakes like Hadoop, and to feed real-time analytics systems like Flink, Storm and Spark Streaming.

#### Where does Kafka fit in the Big Data Architecture?

Kafka is a data stream that fills up Big Data’s data lakes.

#### How does Kafka relate to real-time analytics?

Kafka feeds data to real-time analytics systems like Storm, Spark Streaming, Flink, and Kafka Streaming.

#### Who uses Kafka?

The top ten travel companies, 7 of top ten banks, 8 of top ten insurance companies, 9 of top ten telecom companies, LinkedIn, Microsoft, Netflix and many more companies.

#### How does Kafka decouple streams of data?

It decouple streams of data by allowing multiple consumer groups that can each control where in the topic partition they are. The producers don’t know about the consumers. Since the Kafka broker delegates the log partition offset (where the consumer is in the record stream) to the clients (Consumers), the message consumption is flexible. This allows you to feed your high-latency daily or hourly data analysis in Spark and Hadoop and the same time you are feeding microservices real-time messages, sending events to your CEP system and feeding data to your real-time analytic systems.

#### What are some common use cases for Kafka?

Kafka feeds data to real-time analytics systems like Storm, Spark Streaming, Flink, and Kafka Streaming. It also gets used for log aggregation, feeding events to CEP systems, and commit log for in-memory [microservices](http://cloudurable.com/blog/high-speed-microservices/index.html).

#### What is an ISR?

An ISR is an in-sync replica. If a leader fails, an ISR is picked to be a new leader.

#### How does Kafka scale consumers?

Kafka scales consumers by partition such that each consumer gets its share of partitions. A consumer can have more than one partition, but a partition can only be used by one consumer in a consumer group at a time. If you only have one partition, then you can only have one consumer.

#### What are leaders? Followers?

Leaders perform all reads and writes to a particular topic partition. Followers replicate leaders.

#### How does Kafka perform failover for consumers?

If a consumer in a consumer group dies, the partitions assigned to that consumer is divided up amongst the remaining consumers in that group.

#### How does Kafka perform failover for Brokers?

If a broker dies, then Kafka divides up leadership of its topic partitions to the remaining brokers in the cluster.

#### What is a consumer group?

A consumer group is a group of related consumers that perform a task, like putting data into Hadoop or sending messages to a service. Consumer groups each have unique offsets per partition. Different consumer groups can read from different locations in a partition.

#### Does each consumer group have its own offset?

Yes. The consumer groups have their own offset for every partition in the topic which is unique to what other consumer groups have.

#### When can a consumer see a record?

A consumer can see a record after the record gets fully replicated to all followers.

#### What happens if there are more consumers than partitions?

The extra consumers remain idle until another consumer dies.

#### What happens if you run multiple consumers in many threads in the same JVM?

Each thread manages a share of partitions for that consumer group.

#### Can producers occasionally write faster than consumers?

Yes. A producer could have a burst of records, and a consumer does not have to be on the same page as the consumer.

#### What is the default partition strategy for producers without using a key?

Round-Robin

#### What is the default partition strategy for Producers using a key?

Records with the same key get sent to the same partition.

#### What picks which partition a record is sent to?

The Producer picks which partition a record goes to.

#### How would you prevent a denial of service attack from a poorly written consumer?

Use Quotas to limit the consumer’s bandwidth.

#### What is the default producer durability (acks) level?

All. Which means all ISRs have to write the message to their log partition.

#### What happens by default if all of the Kafka nodes go down at once?

Kafka chooses the first replica (not necessarily in ISR set) that comes alive as the leader as `unclean.leader.election.enable=true` is default to support availability.

#### Why is Kafka record batching important?

Optimized IO throughput over the wire as well as to the disk. It also improves compression efficiency by compressing an entire batch.

#### What are some of the design goals for Kafka?

To be a high-throughput, scalable streaming data platform for real-time analytics of high-volume event streams like log aggregation, user activity, etc.

#### What are some of the new features in Kafka as of June 2017?

Producer atomic writes, performance improvements and producer not sending duplicate messages.

#### What is the different message delivery semantics?

There are three message delivery semantics: at most once, at least once and exactly once.

#### What are three ways Kafka can delete records?

Kafka can delete older records based on time or size of a log. Kafka also supports log compaction for record key compaction.

#### What is log compaction good for?

Since Log compaction retains last known value it is a full snapshot of the latest records it is useful for restoring state after a crash or system failure for an in-memory service, a persistent data store, or reloading a cache. It allows downstream consumers to restore their state.

#### What is the structure of a compacted log? Describe the structure.

With a compacted log, the log has head and tail. The head of the compacted log is identical to a traditional Kafka log. New records get appended to the end of the head. All log compaction works at the tail of the compacted log.

After compaction, do log record offsets change? No.

#### What is a partition segment?

Recall that a topic has a log. A topic log is broken up into partitions and partitions are divided into segment files which contain records which have keys and values. Segment files allow for divide and conquer when it comes to log compaction. A segment file is part of the partition. As the log cleaner cleans log partition segments, the segments get swapped into the log partition immediately replacing the older segment files. This way compaction does not require double the space of the entire partition as additional disk space required is just one additional log partition segment.