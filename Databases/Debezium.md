Debezium is a kind of plugin for kafka-connect that enables Change Data Capture (CDC) from [[MySQL]] database. The changes are streamed to [[Kafka]] for downstream EPA processing.   
  
Kafka Connect - a framework for building and running distributed data pipelines that reads or writes data from/to the external system. There are 2 types of connectors:  
- **source** - reads data from the database  
- **sink** - writes data to the database

### Config properties for performance optimization

`poll.interval.ms: 100` - interval of polling binlog changes from [[MySQL]]  
`max.batch.size: 16384` - max size of the [[MySQL]] binlog changes batch   
`max.queue.size: 65536` - size of the queue of updates waiting for processing in Kafka-connect  
`producer.override.compression.type: lz4` - compression of Kafka messages  
`producer.override.linger.ms: 100` - time waited to collect kafka messages before pushing them all to Kafka in a batch  
`producer.override.batch.size: 1048576` - max batch size of messages pushed to Kafka  
`value.converter.schemas.enable`, `key.converter.schemas.enable` - remove schemas from messages, reducing their size from 9 to 1 kb  
`tasks.max` - parallelism of binlog reading. We need this property to be `1` for consistency
