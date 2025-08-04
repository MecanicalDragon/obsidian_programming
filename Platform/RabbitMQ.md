**RabbitMQ** complies with *AMQP* (Advanced Message Queuing Protocol), which is an open standard application layer protocol and allows messaging between systems, regardless of message broker vendor or platform used. *JMS* (Java Message Service) defines API, but not message format. AMQP publishes message format specifications in XML that allows it to create message (de)serialization algorithms automatically, also it defines rules of how messages must be routed and stored within the broker to follow the AMQ Model. Message clients using AMQP are completely language agnostic.

RabbitMQ follows the publish/subscribe pattern. Publishers publish messages into exchangers – this is a Rabbit-specific structure that distributes messages among queues and other exchangers. Each exchanger has its own configuration of how to distribute messages based on message’s headers and key, and queues are prioritized according to their own configuration and delete message after delivery. Consumers read messages from queues. This schema has following features:
- If a message should be delivered to several consumers, each of them should have its own queue.
- If a message should be read by a single consumer in a set, they all should read the same queue.

Rabbit uses a push model: each message in a queue triggers a callback on the consumer. Thanks to exchangers, message routing is very flexible, and it’s easy to add new participants in the system.

## Components of AMQP 0.9.1

**Message Queue**. Queue acts as a buffer that stores messages that are consumed later. A queue can also be declared with several attributes during creation. For instance, it can be marked as durable, auto-delete and exclusive.

**Exchanges and Exchange Types**. Channel routes messages to a queue depending on the exchange type and bindings between the exchange and the queue. For a queue to receive messages, it must be bound to at least one exchange. AMQP 0.9.1 brokers should provide the following exchange types:
- **direct exchange** – only one subscriber receives msg, even if there are several of them.
- **fanout exchange** – all subs will receive the msg.
- **topic exchange** – all subs that match RoutingKey or QueuePattern will receive the msg.
- **header exchange** – RoutingKey and X-match attributes, that provided in headers, are used to define recipients.
- **consistent hashing** – allows to achieve consequential handling for messages with the same key in groups.

**Binding** - a relation between a queue and an exchange consisting of a set of rules that the exchange uses to route messages.

**Message and Content** - each message contains a set of headers defining properties such as life duration, durability, and priority.

**Connection** - a network connection between application and AMQP broker, e.g., a TCP/IP socket connection.

**Channel** - a virtual connection inside a connection between two AMQP peers. Message publishing or consuming to or from a queue is performed over a channel (AMQP). A channel is multiplexed, one single connection can have multiple channels.

![[rabbit2.png]]
![[rabbit1.png]]
## Features and Advantages

- Flexible and dynamic routing and scaling (components can be added and removed without cluster restart).
- Messages are delivered in order of their supplement to the queue, but it doesn’t guarantee that handling endings will follow the same order, especially (but not only) if concurrent subscribers exist.
- Subscriber can limit the number of messages-in-process to prevent a situation when it’s overloaded with messages.
- To overcome drawback of point 2 we can limit msg-in-process from point 3 with one.
- It’s possible to add custom components like exchangers with plugins.
- DLQ out of the box provided. Messages with expired TTL, or messages with overfilled queues are routed there.
- RabbitMQ has delivery strategies *Once* and *At Least Once*, but not *Exactly Once*.

[Source](https://habr.com/ru/company/itsumma/blog/416629/), [original source](https://jack-vanlightly.com/blog/2017/12/3/rabbitmq-vs-kafka-series-introduction).
