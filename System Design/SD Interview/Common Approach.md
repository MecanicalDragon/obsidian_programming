**Functional requirements**.
Ask the exact scope. Ask everything you can imagine about functionality.
1. Who and how will use this service? What kind of requirements do they have? For what purpose do they need this service?
2. Ask about users:
	1. How many daily active users do we expect?
	2. What is the read/write request ratio?
	3. What about load peaks?
3. Ask about stored data:
	1. What kind of data clients dispatch and retrieve? What kind of data do we want to store?
	2. How much and how long do we want to keep data?
	3. What are expected write-to-read data delay and query latency? 
4. If we need to collect data:
	1. What data do we want to collect?
	2. How frequently do we need to collect it?
	3. Do we need to provide results immediately or delay is ok?
	4. Are we interested in the precision of this data or approximate results are ok?

**Non-functional requirements**.
Ask about numbers:
1. Total users
2. Daily Active Users / Minute Active Users
3. Total data storage
4. Requests per day/minute
5. Scalability, availability, durability
6. Congestion and bandwidth
7. Budget

**Clarify API**, juxtapose it with supposed functionality.
   
**Define data model and database**.

**Back-of-the-envelope estimation**.
1. Calculate approximate data storage requirements for all types of data.
2. Calculate *RPS* and *QPS* (HTTP requests and DB queries per second). 86400 seconds in a day, that is `~9*10^4`. Take into account or calculate:
	1. *MAU* and *DAU* (monthly and daily active users)
	2. read-write ratio 
	3. required traffic and network bandwidth
	4. load peaks
3. **Example**: let's say we have 150M DAU; 30% post 3 tweets in a day, load peek doubles the load.
	1. `4.5 * 10^7 (users) * 3 (tweets) * 2 (load peak) / 9 * 10^4 (secs in a day)`
	2. `(a * b) : c = (a : c) * b; a : (b * c) = (a : b) : c` ([[Распределительные свойства|welcome here]])
	3. Shift all numbers to the left and all powers of ten to the right.
	4. `(4.5 * 3 * 2 * 10^7 / 10^4) / 9` or `(4.5 * 3 * 2 * 10^7 / 9) / 10^4`
	5. Division to `10^x` is just the subtraction of `x` from the sum of other powers.
	6. `3 * 10^3 = 3000 RPS` at the peak load.

**High-Level Design**.
Solve the problem from end to end.

**Detailed Design**.
1. How should we partition the data to distribute it among multiple databases? What issues could it cause?
2. How will we handle hot cases like users who post a lot or follow lots of people?
3. Since users’ timeline will contain the most recent (and relevant) posts, should we try to store our data so that it is optimized for scanning the latest tweets?
4. Are we protected from service failures and downtimes?
5. How much and at which layer should we introduce cache to speed things up?
6. What components need better load balancing?

**Identify and resolve bottlenecks**.
1. Is there any single point of failure in our system? What are we doing to mitigate it?
2. Do we have enough data replicas to still serve users if we lose a few servers?
3. Do we have enough copies of different services running such that a few failures will not cause a total system shutdown?
4. How are we monitoring the performance of our service? Do we get alerts whenever critical components fail or their performance degrades?

---
- [System Design Interview](https://www.educative.io/courses/grokking-the-system-design-interview/B8nMkqBWONo)
- [Advanced System Design](https://www.educative.io/courses/grokking-adv-system-design-intvw?aid=5082902844932096&utm_source=google&utm_medium=cpc&utm_campaign=grokking-row&utm_term=system%20design%20interview%20prep&utm_campaign=Grokking+System+Design+-+ROW&utm_source=adwords&utm_medium=ppc&hsa_acc=5451446008&hsa_cam=9501300853&hsa_grp=97374374860&hsa_ad=523657604610&hsa_src=g&hsa_tgt=dsa-296059857999&hsa_kw=system%20design%20interview%20prep&hsa_mt=b&hsa_net=adwords&hsa_ver=3&gclid=CjwKCAiAm7OMBhAQEiwArvGi3LYOEU8xdow-z9oCE-XS8BZrZSbUMWv4ObiTkKp3ntEdwNIq95aPPhoC0JcQAvD_BwE)
