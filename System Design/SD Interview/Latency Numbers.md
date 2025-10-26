```
1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns

L1 cache reference || CPU register access   <1 ns
L2 cache reference                           5 ns  10x L1 cache
Branch mispredict                            5 ns
L3 cache reference                          15 ns  3x L2 cache, 30x L1 cache
Mutex lock/unlock                           25 ns
Main memory reference                      100 ns  20x L2 cache, 200x L1 cache
System call                           100-1000 ns
MD5 hash of int64                          200 ns

Process context switching                        1+ us
64 KB copying in memory                          1+ us 
Compress 1 KB with Snzip                          3 us
Send 1 KB over 1 Gbps network                    10 us
Read 1 MB sequentially from memory               10 us
Nginx proxying of regular HTTP request           50 us
Read a 8k page from SSD                      10-100 us
Single datacenter network round trip        100-500 us
Write a 8k page to SSD                     100-1000 us  10x SSD reading
Read 4 KB randomly from 1GB/sec SSD            100+ us

Redis GET including network roundtrip             1 ms
Read 1 MB sequentially from 1GB/sec SSD           1 ms  100x memory
HDD seek time                                     5 ms
Inter-zone network roundtrip                   1-10 ms  10x datacenter roundtrip
Read 1 MB sequentially from 1 Gbps network       10 ms  1000x memory, 10X SSD
Read 1 MB sequentially from HDD                  30 ms  3000x memory, 30X SSD
Reading 1GB sequentially from main memory    10-100 ms
US_east-US_west-Europe network round trip    50-100 ms  10x interzone roundtrip
bcrypt password || TLS handshake             50-500 ms
US-west - Singapore network round trip      250-500 ms  5x intercontinent roundtrip
Read 1 GB sequentially from SSD            100-1000 ms  10x memory

Transfer 1 GB by the network within the same region:    10 sec

QPS handled by MySQL: 1000
QPS handled by key-value store: 10,000
QPS handled by cache server: 100,000–1 M
```
