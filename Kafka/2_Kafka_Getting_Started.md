# Kafka Summary

**What is Apache Kafka?**

- A publisher-subscriber based messaging system in the Hadoop ecosystem
- Each node in the cluster is called a "broker"
- Written in Scala and Java 1.6
- Queues are known as "topics"
- Topics can be partitioned, generally based on the number of consumers of the topic.  
- Each message in a partition has a unique sequence number associated with it called an offset.  
- ZooKeeper serves as the coordination interface between the kafka broker and its consumers.  
- Elects one broker as the leader of a partition, and all the writes and reads must go to the leader partition.
- Kafka does not have any concept of a master node and treats all the brokers as peers.  

**ZooKeeper Basics**

- ZooKeeper is a configuration and coordination manager used in many Hadoop products. 
- znodes:  shared, hierarchical namespace of data registers much like a filesystem
- runs, generally, on port 2181
- config/server.properties
  - lists zookeeper connection info
  - lists the broker.id for the node/namespace
  - lists the logs dir
- zoo.cfg

**Kafka Topics**

- by default a topic (queue) has one partition and a replication factor of 1.  
- `kafka-list-topic.sh` actually queries zookeeper for its replication factor, partitions, and leader info.  
- **Leader:**  a randomly-selected node for a specific portion of the partitions and is responsible for all reads and writes for the partition.  
- **isr:**  in-sync replicas:  replicas that are currently alive and in sync with the leader.  Basically, the non-leader nodes
- When a new leader needs to be elected this causes significant read-write load in zookeeper and should be avoided.  
- Kafka tries to ensure that lead replicas for topics are equally distributed among nodes but that breaks down when a node is taken offline. 

**Paritioning and Replication**

- The producer determines how the messages are partitioned.  The broker stores the messages in the same order as they arrive.  The number of partitions can be configured for each topic within the broker.  
- Replication was introduced in Kafka 0.8.
  - ensures better durability of messages and high availability of the cluster.  
  - guarantees that the message will be published and consumed even in case of broker failure 

**Useful Kafka Commands**

`# create a topic`

`/bin/kafka-create-topic.sh --zookeeper localhost:2181 --replica 1 --partition 1 --topic <name>`



`# create a topic with 4 partitions and a replication factor of 2`

`/bin/kafka-create-topic.sh --zookeeper localhost:2181 --replication-factor 2 --partition 4 --topic <name>`



`# have a producer send some messages`

`/bin/kafka-console-producer.sh --broker-list localhose:9092 --topic <name>`



`# starting a consumer`

`bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic <name> --from-beginning`



`#list all topics with leader, replication, and partition information`

`./kafka-list-topic.sh --zookeeper localhost:2181`

 

`#see information for only one topic`

`./kafka-list-topic.sh --zookeeper localhost:2181 --topic <name>`

 

`#show a topic's "lag"`

`/opt/kafka/bin/kafka-run-class.sh kafka.tools.ConsumerOffsetChecker --group <name> --zkconnect <host>:2181`

 

`#show just the topic's "lag"`

`/opt/kafka/bin/kafka-run-class.sh kafka.tools.ConsumerOffsetChecker --group <name> --zkconnect <host>:2181 | awk '{print $6}' | tail -n 3`

 

`#start Kafka on one node, with its correct configuration (supervisord is the right way to do this)`

`./kafka-server-start.sh ../config/server.properties`

 

`#stop Kafka on one node (supervisord is the right way to do this)`

`./kafka-server-stop.sh`

 

`#look at Kafka/ZooKeeper supervisord configuration entries`

`cat /etc/supervisord.conf`

 

`tail -f /var/log/supervisor/supervisord.log `



**Startup/Shutdown**

Since Kafka depends on ZooKeeper it's best, IMHO, to ensure each service is managed with something like [supervisord](http://supervisord.org/).  Having said that, we'll look at the native commands:  

`#shutdown a node`

`bin/kafka-run-class.sh kafka.admin.ShutdownBroker --zookeeper <host:port> --broker <brokerid>`



`#startup a node`

`bin/kafka-server-start.sh config/server.properties `



**Various Logs**

`#view supervisord configs`

`cat /etc/supervisord.conf`

 

`#supervisord logs`

`cat /var/log/supervisor/supervisord.log`

 

`#if only zk is not running, check its logs`

`tail -f /var/home/user/log/zookeeper.out`

 

`#if kafka process is not running, check these logs`

`less /var/home/user/kafka/logs/supervisor-kafka.log`

`less /var/home/user/log/kafka.out`

`less /var/home/user/logs/server.log`

`less /var/home/user/state-change.log`

