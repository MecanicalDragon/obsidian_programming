Schema Registry provides a centralized repository for managing and validating schemas for topic messages, and (de)serialization of the data over the network. Producers and Consumers to Kafka topics can use schemas to ensure data consistency and compatibility as schemas evolve. Schema Registry provides data validation, compatibility, versioning, and evolution.

Schema Registry is a service that connects to Kafka as a client and stores schemas to `_schemas` topic. Once registered, the schema can be shared and reused across different systems and applications.

When producer sends data to message broker, before the dispatch itself Schema Registry ensures that the data is valid and compatible with the expected schema for the topic. Then schema id is included in the message header. When consumer receives the message, it fetches the schema from Schema Registry by the schema id from the header and deserializes it using provided schema.

At its core, Schema Registry has two main parts:
- A REST service for validating, storing, and retrieving Avro\JSON\Protobuf schemas
- Serializers and deserializers that plug into the Kafka clients to handle schema storage and retrieve for Kafka messages across the three formats

A schema defines the structure of message data. It defines allowed data types, their format, relationships, and any constraints or rules that apply to the data.

When to use:
- ensures data validation among multiple services
- ensures data consistency and compatibility during frequent data changes