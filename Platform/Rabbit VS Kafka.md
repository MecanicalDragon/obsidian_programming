| RabbitMQ                                                                                                                                                                | Kafka                                                                                                                                                                                                                             |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Push-system. Brokers push messages to subscribers.                                                                                                                      | Pull-system. Consumers fetch messages from brokers.                                                                                                                                                                               |
| Knows if msg has been delivered to a subscriber.                                                                                                                        | Doesn’t care whether msg has been delivered or not.                                                                                                                                                                               |
| *SQS*. Every subscriber has its own queue to read from. So, if the number of consumers is big, this will take a lot of resources to maintain a queue for each consumer. | *Distributed commit log*. Topic is common for all consumers. This fact makes Kafka a better choice when the number of consumers is huge.                                                                                          |
| Messages are pushed to subscribers one at a time. Sub can configure a number of concurrently processed messages and send acks back in batch.                            | Next message will be processed only when the previous one is handled. Batch consuming is allowed too, but the next message in a batch will be processed only after the previous one acked.                                        |
| Guarantees order of pushing messages but doesn’t guarantee order of handling until consumer is configured to process only one message at time, that hits performance.   | Messages will be handled in the same order they were published for every partition.                                                                                                                                               |
| Since every subscriber has its own queue, it can use a PriorityQueue with its specific preferences.                                                                     | All subscribers read from a single topic and have their own offsets in it.                                                                                                                                                        |
| Message will be deleted after consuming.                                                                                                                                | Messages won’t be deleted and can be retrieved later.                                                                                                                                                                             |
| RabbitMQ maintains global order of the whole queue but offers no ways to maintain that order during the parallel processing of that queue.                              | Kafka cannot offer global ordering of the topic, but it offers ordering on the partition level. So, if you only need ordering of related messages then Kafka offers both ordered message delivery and ordered message processing. |
| Multiple applications cannot read the same queue and receive all messages – they need their own queues for it.                                                          | Multiple applications can subscribe to the same topic-partition and receive all messages in publication order.                                                                                                                    |
| Can reconfigure the cluster dynamically.                                                                                                                                | To reconfigure the cluster you must restart it.                                                                                                                                                                                   |