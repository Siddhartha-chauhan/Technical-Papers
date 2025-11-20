# Apache Kafka

Apache Kafka is a distributed event‑streaming platform designed for
building real‑time data pipelines and high‑performance streaming
applications. It allows systems to publish, subscribe, store and process
continuous streams of records at scale. Kafka is built to handle massive
volumes of data with low latency and high fault tolerance by
distributing workloads across a cluster of brokers. It uses a commit
log‑based storage mechanism that ensures durability and ordering of
messages. Kafka is widely used in industries for log aggregation,
real‑time analytics, messaging, event sourcing and integrating
microservices.

## Key Characteristics

-   Distributed, scalable and fault‑tolerant architecture
-   High‑throughput message ingestion and processing
-   Persistent storage using distributed commit logs
-   Real‑time event streaming with low latency
-   Support for pub‑sub and event‑driven architectures
-   Horizontal scaling across multiple brokers

## Core Components

1.  **Producers** -- Send data to Kafka topics\
2.  **Consumers** -- Read data from topics\
3.  **Brokers** -- Kafka servers that store and manage data\
4.  **Topics** -- Logical channels that store event streams\
5.  **Partitions** -- Subdivisions of topics enabling parallel
    consumption\
6.  **Zookeeper/Kraft** -- Used for cluster coordination and metadata
    management

## Advantages of Apache Kafka

1.  Extremely high throughput suitable for big‑data environments\
2.  Ability to store large amounts of streaming data reliably\
3.  Real‑time processing with integrations like Kafka Streams and Flink\
4.  Scalability across clusters without downtime\
5.  Strong durability guarantees due to distributed logs

## Example Use Case

A large e‑commerce company can use Kafka to collect clickstream data
from millions of users. Producers publish events such as product views
or cart updates, and consumers like recommendation systems or analytics
engines process this data in real time to personalize the user
experience and detect trends.

## References

-  https://youtu.be/QkdkLdMBuL0?si=RiFL-ckOGUOqm0Lu
