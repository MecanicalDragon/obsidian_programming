DHCP


Ассемблер с чатом жпт:
https://chatgpt.com/share/6893e114-0c58-8001-a389-1d412d10e203

---
False Sharing:
https://alidg.me/blog/2020/4/24/thread-local-random#false-sharing
@Contended MESI

`x & mask` эквивалентно `x % m` **только если `m = 2^k` и `mask = m - 1`**.  
Если `m` — не степень двойки, то `& mask` ≠ `% m`
`private static final long mask = (1L << 48) - 1; // то есть m = 2^48`

---
Про Кассандру и колоночные базы:
https://www.bigdataschool.ru/wiki/cassandra
Про редис:
[Redis Topologies: Replicated setup, Sentinel, Cluster, Standalone + Envoy](https://blog.devgenius.io/redis-topologies-d9e16a7fa8e0)

shared and exclusive locks in postgres

CICD - ключи с асинхронными сборками против родитель-наследник и поступенчатой сборки
CICD agent and CICD worker

Kafka \ AVRO \ Protobuf \ thrift
Ktor
gRPC
Vasm
Postgres tsvector
Proto: type, wiring, value
Trunk-based VS gitflow
Алгоритмическая задача про рюкзак с товарами

Database structures:
**https://www.youtube.com/watch?v=W_v05d_2RTo&ab_channel=ByteByteGo

