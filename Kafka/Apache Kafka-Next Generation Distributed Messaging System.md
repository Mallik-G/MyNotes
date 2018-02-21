# Apache Kafka: Next Generation Distributed Messaging System

In today’s world, **data is the main ingredient** of internet applications and typically encompasses the following :

- Page visits and clicks
- User activities
- Events corresponding to logins
- Social networking activities such as likes, shares and comments
- Application-specific metrics (e.g. logs, page load time, performance etc.)

This **data can be used to run analytics in real time** serving various purposes, some of which are:

- Delivering advertisements
- Tracking abnormal user behaviors
- Displaying search based on relevance
- Showing recommendations based on previous activities

**Problem: **Collecting all the data is not easy as data is generated from various sources in different formats

**Solution:** One of the ways to solve this problem is to use a messaging system. Messaging systems provide a seamless integration between distributed applications with the help of messages.

[![apache-kafka-next-generation-distributed-messaging-system](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/apache-kafka.png)](https://www.edureka.co/apache-kafka)

**Apache Kafka :**

Apache Kafka is a distributed publish subscribe messaging system which was originally developed at LinkedIn and later on became a part of the Apache project. Kafka is fast, agile, scalable and distributed by design.

**Kafka Architecture and Terminology :**

[![kafka-cluster](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/kafka-cluster.jpg)](https://www.edureka.co/apache-kafka)

**Topic : **A stream of messages belonging to a particular category is called a topic

**Producer : **A producer can be any application that can publish messages to a topic

**Consumer : **A consumer can be any application that subscribes to topics and consumes the messages

**Broker :** Kafka cluster is a set of servers, each of which is called a broker

Kafka is scalable and allows creation of multiple types of clusters.

- **Single Node Single Broker Cluster**
- **Single Node Multiple Broker Cluster**
- **Multiple Nodes Multiple Broker Cluster**

[![single-node-single-broker](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/single-node-single-broker.jpg)](https://www.edureka.co/apache-kafka)**Single Node Single Broker**

**What’s the role of ZooKeeper ?**

Each Kafka broker coordinates with other Kafka brokers using ZooKeeper. Producers and Consumers are notified by the ZooKeeper service about the presence of new brokers or failure of the broker in theKafka system.

[![single-node-multiple-brokers](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/single-node-multiple-brokers.jpg)](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/single-node-multiple-brokers.jpg)

​                                                         **Single Node Multiple Brokers**

[![multiple-node-multiple-broker](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/multiple-node-multiple-broker.jpg)](https://www.edureka.co/apache-kafka)

**                                                         Multiple Nodes Multiple Brokers**

**Kafka @ LinkedIn**

![lnewsfeed-kafka-linkedin](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/lnewsfeed.png)

**                                                        LinkedIn Newsfeed is powered by Kafka**

[![lrecommendations](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/lrecommendations.png)](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/lrecommendations.png)                                           **LinkedIn recommendations are powered by Kafka**

[![lnotifications](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/lnotifications.png)](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/lnotifications.png)

**                                       LinkedIn notifications are powered by Kafka**

**Note: **Apart from this, LinkedIn uses Kafka for many other tasks like log monitoring, performance metrics, search improvement, among others.

**Who else uses Kafka ?**

**DataSift:** DataSift uses Kafka as a collector of monitoring events and to track users’ consumption of data streams in real time

**Wooga:** Wooga uses Kafka to aggregate and process tracking data from all their Facebook games (hosted at various providers) in a central location

**Spongecell:** Spongecell uses Kafka to run its entire analytics and monitoring pipeline driving both real time and ETL applications

**Loggly :** Loggly is the world’s most popular cloud-based log management. It uses Kafka for log collection.

**Comparative Study: Kafka vs. ActiveMQ vs. RabbitMQ**

Kafka has a more efficient storage format.On an average, each message has an overhead of 9 bytes in Kafka, versus 144 bytes in ActiveMQ

[![kafka-storage-cmp1](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/cmp1.png)](https://www.edureka.co/apache-kafka)In both ActiveMQ and RabbitMQ, brokers maintain delivery state of every message by writing to disk but in the case of Kafka, there is no disk write, hence making it faster.

[![kafka-storage-cmp2](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2015/11/cmp2.png)](https://www.edureka.co/apache-kafka)

With the wide adoption of Kafka in production, it looks to be a promising solution for solving real world problems. 