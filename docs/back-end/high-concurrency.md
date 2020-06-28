
## Message Queues
* [Why Message Queues? - Github/doocs/advanced-java](https://github.com/doocs/advanced-java/blob/master/docs/high-concurrency/why-mq.md).
  * Pros: 解耦 (decoupled)、异步 (async)、削峰 (cut-peak-traffic);
  * Cons: 系统可用性降低 (availablity), 系统复杂度提高 (complexity), 一致性问题 (consistency);
  * It compares ActiveMQ, RabbitMQ, RocketMQ, and Kafka. 
* Kafka
  * [Apache Kafka - Fundamentals](https://www.tutorialspoint.com/apache_kafka/apache_kafka_fundamentals.htm) gives an abstract description without an example. Not very clear.
  * [Kafka architecture (Mar 2020)](https://data-flair.training/blogs/kafka-architecture/) explains the architecture with slightly more details. Furthermore, [Kafka topics (Feb 2020)](https://data-flair.training/blogs/kafka-topic-architecture/), from the same website, gives a more detailed explanation.
  * [ZooKeeper in Kafka (Feb 2020)](https://data-flair.training/blogs/zookeeper-in-kafka/) - Since brokers are stateless, Kafka uses ZooKeeper to manage and coordiante them. Also, since Kafka partitions have in-sync replicas (ISR), ZooKeeper will elect new leader if the previous leader fails.
* [ ] RabbitMQ - 
* [ ] ZooKeeper