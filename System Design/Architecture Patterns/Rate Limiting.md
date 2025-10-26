5 ways to implement (*best choice* means that algorithm limits requests and handles bursts better than others):

- **Leaky Bucket.** It is just a FIFO queue with max size of max allowed requests number. Every configured timespan first request is taken for handling. Requests that exceed a queue limit just dropped. This approach smoothens bursts of requests but drops new requests if the queue is filled.

- **Token Bucket** (*best choice*). Bucket is filled with tokens. Each request consumes some amount of tokens to be processed. Main drawback: Burst of requests after some delay may consume all tokens too fast and clutter the system.

- **Fixed Window**. Allows a fixed number of requests every time period. Period should be very small to afford requests burst ignoring. And anyway, near the boundaries of a timespan double requests rate is possible. Also, it drops old overcrowding requests to handle new ones.

- **Sliding Log**. Algorithm keeps timestamps of incoming requests, drops requests if there are too many of them in the configured timespan and removes old out of consideration. The drawback: it is expensive.

- **Sliding Window** (*best choice*). It is a hybrid approach that considers part of requests of the previous window to compute a possibility to handle requests in a current window: if a half of window timespan has passed, 50% of previous window requests is considered. It is good in performance and smoothes request bursts.

[habr](https://habr.com/ru/post/448438/)
