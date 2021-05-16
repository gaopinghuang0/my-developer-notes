
## Message Queues
* [Why Message Queues? - Github/doocs/advanced-java](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/why-mq.md).
  * Pros: 解耦 (decoupled)、异步 (async)、削峰 (cut-peak-traffic);
  * Cons: 系统可用性降低 (availability), 系统复杂度提高 (complexity), 一致性问题 (consistency);
  * It compares ActiveMQ, RabbitMQ, RocketMQ, and Kafka. 
* Kafka
  * [Apache Kafka - Fundamentals](https://www.tutorialspoint.com/apache_kafka/apache_kafka_fundamentals.htm) gives an abstract description without an example. Not very clear.
  * [Kafka architecture (Mar 2020)](https://data-flair.training/blogs/kafka-architecture/) explains the architecture with slightly more details. Furthermore, [Kafka topics (Feb 2020)](https://data-flair.training/blogs/kafka-topic-architecture/), from the same website, gives a more detailed explanation.
  * [ZooKeeper in Kafka (Feb 2020)](https://data-flair.training/blogs/zookeeper-in-kafka/) - Since brokers are stateless, Kafka uses ZooKeeper to manage and coordiante them. Also, since Kafka partitions have in-sync replicas (ISR), ZooKeeper will elect new leader if the previous leader fails.
  * [When do the messages published in Kafka get deleted?](https://www.quora.com/When-do-the-messages-published-in-Kafka-get-deleted)
    * Messages are not deleted when consumed. New messages are appended to the end of the queue (broker). Consumers use offsets to locate messages.
    * By default, if your topic comes with the default configuration, the data is deleted after a week by Kafka from the receive timestamp.
    * If your topic is log compacted, the data is kept indefinitely. From time to time Kafka will delete all the messages that share the same key, except the most recent one.
* [ ] RabbitMQ - 
* [ ] ZooKeeper

## Load balancing
* 一致性哈希 or [Consistent hashing (Apr 2018)](https://yikun.github.io/2016/06/09/%E4%B8%80%E8%87%B4%E6%80%A7%E5%93%88%E5%B8%8C%E7%AE%97%E6%B3%95%E7%9A%84%E7%90%86%E8%A7%A3%E4%B8%8E%E5%AE%9E%E8%B7%B5/))
* [ ] Many different requirements for load balancing
  * Balance the number of persistent links
  * Balance the traffic throughput
  * Balance the response time
  * ...
* [ ] Different algorithms
  * Round-robin
  * Weighted Round robin
  * Consistent hashing
  * Policy based ?? (I don't know its name, but we can define some policy, such as choose the server based on the prefix of the search query). This is related to autocomplete system design.
  * IP