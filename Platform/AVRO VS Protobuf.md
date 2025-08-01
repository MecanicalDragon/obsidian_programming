AVRO and PB are both binary object serialization languages (OSL) that allow to store structured data. Binary format makes them more compact and faster in (de)serialization comparing to json and xml formats. Both formats have schemas that describe how to interprete stored data.

| AVRO                                                                            | ProtoBuf                                                                                               |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Schema in JSON                                                                  | Schema in `.proto` file                                                                                |
| Schema can be embedded in the data or fetched separately from SchemaRegistry    | Schema is required beforehand both on the dispatcher and receiver sides                                |
| Schema evolution is very agile                                                  | Schema evolution is less agile but still good                                                          |
| No need in generated code. Data can be (de)serialized with libraries in runtime | Controllers and dtos must be generated beforehand with `protoc` but no libraries are needed in runtime |
| Best with Kafka and SchemaRegistry                                              | Best with gRPC                                                                                         |
|                                                                                 | 10-30% faster                                                                                          |

