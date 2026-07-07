# High Level Design (HLD) Interview Questions & Answers

> **500 Most Asked HLD Interview Questions** — Basic to Advanced  
> Format: Question → Answer | **47 JavaScript code snippets**

---

## Table of Contents

| # | Section | Questions |
|---|---------|-----------|
| 1 | HLD Fundamentals & Estimation | Q1–Q51 |
| 2 | Networking & Protocols | Q52–Q101 |
| 3 | Databases & Storage | Q102–Q151 |
| 4 | Caching | Q152–Q201 |
| 5 | Load Balancing & Scaling | Q202–Q251 |
| 6 | Messaging & Event Systems | Q252–Q301 |
| 7 | Distributed Systems | Q302–Q361 |
| 8 | Classic HLD System Design Problems | Q362–Q471 |
| 9 | Advanced HLD & Architecture | Q472–Q607 |

**Total: 607 Questions** | Levels: Basic → Intermediate → Advanced

---

## 1. HLD Fundamentals & Estimation

### Q1. What is High Level Design (HLD)?
**Level:** Basic

**Answer:**
HLD defines **system architecture** — components, data flow, APIs, storage, scaling — without implementation details.

```javascript
const arch={client:'App',gateway:'GW',services:['User','Order'],db:'PG',cache:'Redis'}
```

### Q2. HLD vs LLD?
**Level:** Basic

**Answer:**
HLD: architecture, components, scale. LLD: classes, methods, design patterns.

### Q3. What is system design interview?
**Level:** Basic

**Answer:**
Design scalable, reliable system for given requirements in 45-60 min.

### Q4. Functional requirements?
**Level:** Basic

**Answer:**
What system must do: features, user actions.

### Q5. Non-functional requirements?
**Level:** Basic

**Answer:**
Scale, latency, availability, consistency, durability.

### Q6. Back-of-envelope estimation?
**Level:** Basic

**Answer:**
Quick math: QPS, storage, bandwidth from DAU and usage patterns.

### Q7. QPS calculation?
**Level:** Basic

**Answer:**
QPS = DAU × actions_per_user / 86400. Peak ≈ 2-3× average.

```javascript
const qps=(10_000_000*5)/86400; const peak=qps*3
```

### Q8. Storage estimation?
**Level:** Basic

**Answer:**
Daily data × retention period × replication factor.

```javascript
const tb=(500_000_000*300*365*5*3)/(1024**4)
```

### Q9. Bandwidth estimation?
**Level:** Basic

**Answer:**
Request size × QPS for ingress/egress.

```javascript
const mbps=(50*1024*10000*8)/(1024*1024)
```

### Q10. Latency requirements?
**Level:** Basic

**Answer:**
User-facing: <200ms read, <500ms write typical targets.

### Q11. Availability 99.9% meaning?
**Level:** Basic

**Answer:**
8.76 hours downtime/year. 99.99% = 52.6 min/year.

```javascript
const down=(100-99.9)/100*8760
```

### Q12. Scalability?
**Level:** Basic

**Answer:**
System handles growth by adding resources.

### Q13. Vertical vs horizontal scaling?
**Level:** Basic

**Answer:**
Vertical: bigger machine. Horizontal: more machines (preferred).

```javascript
class Cluster{constructor(){this.i=[]}add(x){this.i.push(x)}}
```

### Q14. Reliability?
**Level:** Basic

**Answer:**
System works correctly even when components fail.

### Q15. Fault tolerance?
**Level:** Basic

**Answer:**
Continue operating despite failures.

### Q16. Durability?
**Level:** Basic

**Answer:**
Data persists after writes survive crashes.

### Q17. Throughput vs latency?
**Level:** Basic

**Answer:**
Throughput: requests/sec. Latency: time per request.

### Q18. P50/P95/P99 latency?
**Level:** Intermediate

**Answer:**
Percentile latencies — P99 = 99% requests faster than value.

### Q19. SLA vs SLO vs SLI?
**Level:** Intermediate

**Answer:**
SLI: metric. SLO: target. SLA: contract with penalties.

### Q20. Capacity planning?
**Level:** Intermediate

**Answer:**
Estimate peak load; provision infra with headroom (30-50%).

### Q21. Bottleneck analysis?
**Level:** Intermediate

**Answer:**
Find slowest component: CPU, disk, network, DB.

### Q22. Trade-off triangle?
**Level:** Basic

**Answer:**
Consistency, Availability, Partition tolerance (CAP).

### Q23. Performance vs cost?
**Level:** Intermediate

**Answer:**
More replicas/cache = higher cost. Right-size for SLO.

### Q24. Modular monolith vs microservices?
**Level:** Intermediate

**Answer:**
Monolith: simpler ops. Microservices: independent scale/deploy.

### Q25. 12-Factor App?
**Level:** Intermediate

**Answer:**
Cloud-native principles: config in env, stateless processes, logs as streams.

### Q26. System design interview steps?
**Level:** Basic

**Answer:**
1) Clarify reqs 2) Estimate 3) API 4) Data model 5) High-level diagram 6) Deep dive 7) Bottlenecks.

### Q27. Clarifying questions to ask?
**Level:** Basic

**Answer:**
Scale? Read/write ratio? Consistency needs? Latency? Geographic distribution?

### Q28. Read-heavy vs write-heavy?
**Level:** Intermediate

**Answer:**
Read-heavy: cache, replicas. Write-heavy: sharding, queues, batching.

### Q29. Stateful vs stateless services?
**Level:** Intermediate

**Answer:**
Stateless: any server handles request (preferred). Stateful: sticky sessions/complex.

### Q30. Multi-region design?
**Level:** Advanced

**Answer:**
Deploy in multiple geos for latency and disaster recovery.

### Q31. Active-active vs active-passive?
**Level:** Advanced

**Answer:**
Active-active: all regions serve traffic. Passive: standby for failover.

### Q32. RPO vs RTO?
**Level:** Intermediate

**Answer:**
RPO: max data loss. RTO: max downtime after failure.

### Q33. Disaster recovery?
**Level:** Intermediate

**Answer:**
Backups, replication, failover runbooks, regular drills.

### Q34. Chaos engineering?
**Level:** Advanced

**Answer:**
Inject failures to validate resilience.

### Q35. Observability pillars?
**Level:** Intermediate

**Answer:**
Metrics, Logs, Traces.

### Q36. Golden signals?
**Level:** Intermediate

**Answer:**
Latency, Traffic, Errors, Saturation (Google SRE).

### Q37. Health checks?
**Level:** Intermediate

**Answer:**
Liveness (alive?) and Readiness (can serve traffic?).

### Q38. Graceful degradation?
**Level:** Intermediate

**Answer:**
Reduce features under load instead of full failure.

### Q39. Thundering herd?
**Level:** Intermediate

**Answer:**
Many clients retry simultaneously. Fix: jitter, backoff.

```javascript
const delay=1000*2**i+Math.random()*1000
```

### Q40. Hot spot problem?
**Level:** Advanced

**Answer:**
Uneven key distribution on shards. Fix: better hashing, salting.

### Q41. Fan-out?
**Level:** Intermediate

**Answer:**
One request triggers many downstream calls.

### Q42. Fan-in?
**Level:** Intermediate

**Answer:**
Aggregate results from many services.

### Q43. Back-of-envelope Twitter QPS?
**Level:** Intermediate

**Answer:**
300M DAU × 2 tweets / 86400 ≈ 7K write QPS avg, ~20K peak.

### Q44. Storage for 5 years tweets?
**Level:** Advanced

**Answer:**
500M tweets/day × 300 bytes × 365 × 5 ≈ 270 TB raw.

### Q45. API design in HLD?
**Level:** Intermediate

**Answer:**
Define key endpoints, request/response, idempotency.

### Q46. Data model in HLD?
**Level:** Intermediate

**Answer:**
Entities, relationships, access patterns drive schema.

### Q47. Entity relationship thinking?
**Level:** Intermediate

**Answer:**
Users, Tweets, Follows — model before deep dive.

### Q48. Single point of failure?
**Level:** Basic

**Answer:**
Component whose failure takes down system. Eliminate via redundancy.

### Q49. Redundancy?
**Level:** Basic

**Answer:**
Duplicate components for failover.

### Q50. Load test?
**Level:** Intermediate

**Answer:**
Simulate peak traffic to find breaking point.

### Q51. Canary deployment?
**Level:** Intermediate

**Answer:**
Roll out to small % traffic first.

### Q52. Blue-green deployment?
**Level:** Intermediate

**Answer:**
Two identical envs; switch traffic atomically.

```javascript
const routes={'GET /users':'list','POST /users':'create'}
```

---

## 2. Networking & Protocols

### Q53. OSI model layers?
**Level:** Basic

**Answer:**
7: Application, Presentation, Session, Transport, Network, Data Link, Physical.

### Q54. TCP vs UDP?
**Level:** Basic

**Answer:**
TCP: reliable, ordered, connection-oriented. UDP: fast, unreliable.

### Q55. HTTP vs HTTPS?
**Level:** Basic

**Answer:**
HTTPS = HTTP + TLS encryption.

### Q56. HTTP/1.1 vs HTTP/2?
**Level:** Intermediate

**Answer:**
HTTP/2: multiplexing, header compression, binary framing.

### Q57. HTTP/3?
**Level:** Advanced

**Answer:**
QUIC over UDP — faster connection establishment.

### Q58. REST principles?
**Level:** Basic

**Answer:**
Stateless, resource-based, standard HTTP methods.

### Q59. GraphQL vs REST?
**Level:** Intermediate

**Answer:**
GraphQL: client specifies fields. REST: fixed endpoints.

### Q60. gRPC?
**Level:** Intermediate

**Answer:**
HTTP/2 + Protocol Buffers. Fast, typed, streaming.

### Q61. WebSocket?
**Level:** Intermediate

**Answer:**
Full-duplex persistent connection for real-time.

### Q62. Long polling?
**Level:** Intermediate

**Answer:**
Server holds request until data available.

### Q63. Server-Sent Events?
**Level:** Intermediate

**Answer:**
Server pushes one-way stream over HTTP.

### Q64. DNS role?
**Level:** Basic

**Answer:**
Resolves domain to IP. Hierarchical distributed system.

### Q65. DNS caching?
**Level:** Intermediate

**Answer:**
TTL-based caching at browser, OS, ISP resolvers.

### Q66. CDN?
**Level:** Basic

**Answer:**
Content Delivery Network — edge caches for static content.

### Q67. CDN cache invalidation?
**Level:** Intermediate

**Answer:**
TTL expiry, purge API, versioned URLs.

### Q68. Reverse proxy?
**Level:** Basic

**Answer:**
Sits before servers: SSL termination, load balance, cache.

### Q69. Forward proxy?
**Level:** Intermediate

**Answer:**
Client-side proxy for filtering, anonymity.

### Q70. API Gateway?
**Level:** Intermediate

**Answer:**
Single entry: auth, routing, rate limit, aggregation.

### Q71. TLS handshake?
**Level:** Intermediate

**Answer:**
Negotiate encryption keys before data transfer.

### Q72. Certificate authority?
**Level:** Basic

**Answer:**
Issues trusted SSL certificates.

### Q73. CORS?
**Level:** Intermediate

**Answer:**
Browser security — cross-origin resource sharing headers.

### Q74. NAT?
**Level:** Intermediate

**Answer:**
Network Address Translation — private IP to public.

### Q75. IP vs MAC address?
**Level:** Basic

**Answer:**
IP: network routing. MAC: local link layer.

### Q76. IPv4 vs IPv6?
**Level:** Basic

**Answer:**
IPv4: 32-bit (4.3B addresses). IPv6: 128-bit.

### Q77. Subnet?
**Level:** Intermediate

**Answer:**
Logical subdivision of IP network.

### Q78. Firewall?
**Level:** Basic

**Answer:**
Filters traffic by rules.

### Q79. VPN?
**Level:** Intermediate

**Answer:**
Encrypted tunnel between networks.

### Q80. Anycast?
**Level:** Advanced

**Answer:**
Same IP announced from multiple locations; route to nearest.

### Q81. BGP?
**Level:** Advanced

**Answer:**
Internet routing protocol between autonomous systems.

### Q82. QUIC benefits?
**Level:** Advanced

**Answer:**
0-RTT reconnect, no head-of-line blocking.

### Q83. Connection pooling?
**Level:** Intermediate

**Answer:**
Reuse TCP connections to reduce handshake overhead.

### Q84. Keep-alive?
**Level:** Basic

**Answer:**
Reuse HTTP connections for multiple requests.

### Q85. Timeout configuration?
**Level:** Intermediate

**Answer:**
Connect, read, write timeouts prevent hung requests.

### Q86. Retry storm?
**Level:** Intermediate

**Answer:**
Failed service causes cascading retries. Use backoff + circuit breaker.

### Q87. Service mesh?
**Level:** Advanced

**Answer:**
Sidecar proxies handle service-to-service networking (Istio, Linkerd).

### Q88. Sidecar pattern?
**Level:** Advanced

**Answer:**
Helper container alongside main app container.

### Q89. Ingress controller?
**Level:** Intermediate

**Answer:**
Kubernetes HTTP routing to services.

### Q90. Load balancer OSI layer?
**Level:** Intermediate

**Answer:**
L4: TCP/UDP. L7: HTTP routing.

### Q91. Sticky sessions?
**Level:** Intermediate

**Answer:**
Same client → same server. Needed for local session state.

### Q92. WebRTC?
**Level:** Advanced

**Answer:**
Peer-to-peer real-time audio/video.

### Q93. mTLS?
**Level:** Advanced

**Answer:**
Mutual TLS — both client and server authenticate.

### Q94. Protocol Buffers?
**Level:** Intermediate

**Answer:**
Binary serialization — smaller, faster than JSON.

### Q95. JSON vs Protobuf trade-off?
**Level:** Intermediate

**Answer:**
JSON: human-readable. Protobuf: performance.

### Q96. HPACK?
**Level:** Advanced

**Answer:**
HTTP/2 header compression.

### Q97. DNS load balancing?
**Level:** Intermediate

**Answer:**
Round-robin DNS returns multiple IPs.

### Q98. GeoDNS?
**Level:** Advanced

**Answer:**
Return IP based on user location.

### Q99. TCP three-way handshake?
**Level:** Basic

**Answer:**
SYN → SYN-ACK → ACK.

### Q100. SYN flood attack?
**Level:** Advanced

**Answer:**
DDoS exhausting connection table. Mitigate: SYN cookies.

### Q101. DDoS mitigation?
**Level:** Intermediate

**Answer:**
CDN, rate limiting, WAF, scrubbing centers.

### Q102. WAF?
**Level:** Intermediate

**Answer:**
Web Application Firewall — blocks malicious HTTP.

```javascript
const idx=new Map(users.map(u=>[u.email,u]))
```

---

## 3. Databases & Storage

### Q103. SQL vs NoSQL?
**Level:** Basic

**Answer:**
SQL: structured, ACID, relations. NoSQL: flexible schema, scale-out.

```javascript
const shard=id=>String(id).split('').reduce((h,c)=>h+c.charCodeAt(0),0)%4
```

### Q104. ACID properties?
**Level:** Basic

**Answer:**
Atomicity, Consistency, Isolation, Durability.

```javascript
class Router{write(s,p){return primary.query(s,p)}read(s,p){return replica.query(s,p)}}
```

### Q105. BASE properties?
**Level:** Intermediate

**Answer:**
Basically Available, Soft state, Eventual consistency.

### Q106. Database indexing?
**Level:** Basic

**Answer:**
B-tree index speeds lookups; slows writes.

### Q107. Primary key?
**Level:** Basic

**Answer:**
Unique row identifier.

### Q108. Foreign key?
**Level:** Basic

**Answer:**
References another table's primary key.

### Q109. Normalization?
**Level:** Intermediate

**Answer:**
Reduce redundancy: 1NF, 2NF, 3NF, BCNF.

### Q110. Denormalization?
**Level:** Intermediate

**Answer:**
Duplicate data for faster reads.

### Q111. Sharding?
**Level:** Intermediate

**Answer:**
Horizontal partition of data across DB instances.

### Q112. Sharding strategies?
**Level:** Intermediate

**Answer:**
Hash-based, range-based, directory-based.

### Q113. Consistent hashing?
**Level:** Advanced

**Answer:**
Minimal key redistribution when nodes added/removed.

### Q114. Replication?
**Level:** Basic

**Answer:**
Copy data to multiple nodes.

### Q115. Leader-follower replication?
**Level:** Intermediate

**Answer:**
Leader handles writes; followers replicate.

### Q116. Multi-leader replication?
**Level:** Advanced

**Answer:**
Multiple writable nodes — conflict resolution needed.

### Q117. Leaderless replication?
**Level:** Advanced

**Answer:**
Quorum reads/writes (Dynamo-style).

### Q118. Read replicas?
**Level:** Intermediate

**Answer:**
Scale reads by routing to replicas.

### Q119. Replication lag?
**Level:** Intermediate

**Answer:**
Follower behind leader — stale reads possible.

### Q120. CAP theorem?
**Level:** Basic

**Answer:**
During partition: choose Consistency or Availability.

### Q121. PACELC?
**Level:** Advanced

**Answer:**
If Partition: A or C. Else: Latency or Consistency.

### Q122. Eventual consistency?
**Level:** Intermediate

**Answer:**
Replicas converge over time without guarantees on when.

### Q123. Strong consistency?
**Level:** Intermediate

**Answer:**
All reads see latest write.

### Q124. Quorum (N, W, R)?
**Level:** Advanced

**Answer:**
N replicas. W write acks. R read nodes. W+R>N for consistency.

### Q125. SQL join types?
**Level:** Basic

**Answer:**
INNER, LEFT, RIGHT, FULL, CROSS.

### Q126. Database connection pool?
**Level:** Intermediate

**Answer:**
Reuse connections; bound max connections.

### Q127. N+1 query problem?
**Level:** Intermediate

**Answer:**
1 query + N per-item queries. Fix: eager load/join.

### Q128. OLTP vs OLAP?
**Level:** Intermediate

**Answer:**
OLTP: transactions. OLAP: analytics/aggregations.

### Q129. Data warehouse?
**Level:** Intermediate

**Answer:**
Columnar store for analytics (Snowflake, BigQuery).

### Q130. ETL vs ELT?
**Level:** Intermediate

**Answer:**
ETL: transform before load. ELT: load raw then transform.

### Q131. Document DB?
**Level:** Intermediate

**Answer:**
JSON documents — MongoDB, CouchDB.

### Q132. Key-value store?
**Level:** Basic

**Answer:**
Simple get/put — Redis, DynamoDB.

### Q133. Wide-column store?
**Level:** Advanced

**Answer:**
Cassandra, HBase — column families.

### Q134. Graph database?
**Level:** Advanced

**Answer:**
Nodes and edges — Neo4j for social graphs.

### Q135. Time-series DB?
**Level:** Advanced

**Answer:**
Optimized for metrics — InfluxDB, TimescaleDB.

### Q136. NewSQL?
**Level:** Advanced

**Answer:**
Distributed SQL with scale-out — CockroachDB, Spanner.

### Q137. Database per service?
**Level:** Intermediate

**Answer:**
Microservices own their DB — no shared schema.

### Q138. Shared database anti-pattern?
**Level:** Intermediate

**Answer:**
Tight coupling between services.

### Q139. Saga for distributed transactions?
**Level:** Advanced

**Answer:**
Choreography or orchestration with compensations.

### Q140. Two-phase commit?
**Level:** Advanced

**Answer:**
Coordinator: prepare → commit. Blocking, not partition-tolerant.

### Q141. Change Data Capture (CDC)?
**Level:** Advanced

**Answer:**
Stream DB changes to other systems.

### Q142. Materialized view?
**Level:** Intermediate

**Answer:**
Precomputed query result stored on disk.

### Q143. Partition key selection?
**Level:** Intermediate

**Answer:**
Choose high-cardinality key with even distribution.

### Q144. Hot partition?
**Level:** Advanced

**Answer:**
One shard gets disproportionate traffic.

### Q145. Global secondary index?
**Level:** Advanced

**Answer:**
Alternate query path — eventual consistency in DynamoDB.

### Q146. LSM vs B-tree?
**Level:** Advanced

**Answer:**
LSM: write-optimized (RocksDB). B-tree: read-optimized.

### Q147. Write amplification?
**Level:** Advanced

**Answer:**
LSM compaction rewrites data multiple times.

### Q148. Read amplification?
**Level:** Advanced

**Answer:**
Multiple SSTable lookups per read in LSM.

### Q149. Vacuum/analyze?
**Level:** Intermediate

**Answer:**
Maintenance for dead tuples and statistics.

### Q150. MVCC?
**Level:** Advanced

**Answer:**
Multi-version concurrency — readers don't block writers.

### Q151. Isolation levels?
**Level:** Advanced

**Answer:**
Read uncommitted, read committed, repeatable read, serializable.

### Q152. Deadlock in DB?
**Level:** Intermediate

**Answer:**
Cycle of lock waits. DB detects and aborts one.

```javascript
class LRU{constructor(c){this.c=c;this.m=new Map()}get(k){}put(k,v){}}
```

### Q153. Optimistic locking DB?
**Level:** Intermediate

**Answer:**
Version column — update fails if version changed.

```javascript
async function getUser(id){const c=await redis.get(`user:${id}`);if(c)return JSON.parse(c);const u=await db.find(id);await redis.setex(`user:${id}`,3600,JSON.stringify(u));return u}
```

---

## 4. Caching

### Q154. What is caching?
**Level:** Basic

**Answer:**
Store frequently accessed data in fast storage.

### Q155. Cache hit vs miss?
**Level:** Basic

**Answer:**
Hit: data in cache. Miss: fetch from origin.

### Q156. Cache eviction policies?
**Level:** Intermediate

**Answer:**
LRU, LFU, FIFO, TTL, random.

```javascript
async function upd(id,d){await db.update(id,d);await cache.del(`user:${id}`)}
```

### Q157. TTL?
**Level:** Basic

**Answer:**
Time To Live — auto-expire cached entries.

### Q158. Cache-aside (lazy loading)?
**Level:** Basic

**Answer:**
App checks cache → on miss, read DB → populate cache.

### Q159. Read-through cache?
**Level:** Intermediate

**Answer:**
Cache handles miss and loads from DB itself.

### Q160. Write-through cache?
**Level:** Intermediate

**Answer:**
Write to cache and DB synchronously.

### Q161. Write-behind (write-back)?
**Level:** Advanced

**Answer:**
Write to cache; async flush to DB.

### Q162. Write-around?
**Level:** Intermediate

**Answer:**
Write directly to DB; cache only on read.

### Q163. Cache invalidation?
**Level:** Intermediate

**Answer:**
Hardest problem — TTL, event-driven purge, version keys.

### Q164. Redis use cases?
**Level:** Basic

**Answer:**
Cache, session store, rate limiter, pub/sub, leaderboard.

### Q165. Memcached vs Redis?
**Level:** Intermediate

**Answer:**
Memcached: simple cache. Redis: data structures, persistence.

### Q166. CDN as cache?
**Level:** Basic

**Answer:**
Edge caches static assets close to users.

### Q167. Browser cache?
**Level:** Basic

**Answer:**
Cache-Control, ETag headers.

### Q168. Application cache?
**Level:** Intermediate

**Answer:**
In-process (Caffeine) or distributed (Redis).

### Q169. Distributed cache?
**Level:** Intermediate

**Answer:**
Shared across app instances — Redis cluster.

### Q170. Cache stampede?
**Level:** Advanced

**Answer:**
Many requests miss simultaneously. Fix: lock, early expiry.

### Q171. Cache warming?
**Level:** Intermediate

**Answer:**
Pre-populate cache before traffic spike.

### Q172. Negative caching?
**Level:** Intermediate

**Answer:**
Cache 'not found' to prevent repeated DB hits.

### Q173. Cache key design?
**Level:** Intermediate

**Answer:**
Include all params affecting result: `user:123:profile:v2`.

### Q174. Cache consistency?
**Level:** Advanced

**Answer:**
Trade-off: TTL vs event invalidation vs write-through.

### Q175. Multi-level cache?
**Level:** Advanced

**Answer:**
L1 local + L2 Redis — check L1 first.

### Q176. Redis data structures?
**Level:** Intermediate

**Answer:**
String, Hash, List, Set, Sorted Set, Stream.

### Q177. Redis persistence?
**Level:** Intermediate

**Answer:**
RDB snapshots + AOF append log.

### Q178. Redis cluster?
**Level:** Advanced

**Answer:**
Hash slots across nodes; auto-failover.

### Q179. Cache penetration?
**Level:** Advanced

**Answer:**
Queries for non-existent keys. Fix: bloom filter.

### Q180. Cache avalanche?
**Level:** Advanced

**Answer:**
Many keys expire together. Fix: jittered TTL.

### Q181. Bloom filter?
**Level:** Advanced

**Answer:**
Probabilistic 'definitely not in set' check.

### Q182. CDN dynamic content?
**Level:** Intermediate

**Answer:**
Cache with short TTL or edge compute.

### Q183. Varnish/Squid?
**Level:** Intermediate

**Answer:**
HTTP reverse proxy caches.

### Q184. HTTP caching headers?
**Level:** Intermediate

**Answer:**
Cache-Control, ETag, Last-Modified, Expires.

### Q185. Conditional GET?
**Level:** Intermediate

**Answer:**
If-None-Match → 304 Not Modified.

### Q186. Cache coherency protocols?
**Level:** Advanced

**Answer:**
MESI for CPU caches; invalidation for distributed.

### Q187. Write-through latency?
**Level:** Intermediate

**Answer:**
Higher write latency but consistent cache.

### Q188. Write-behind risk?
**Level:** Advanced

**Answer:**
Data loss if cache crashes before flush.

### Q189. Geographic cache?
**Level:** Advanced

**Answer:**
Regional Redis replicas near users.

### Q190. Cache for feed?
**Level:** Intermediate

**Answer:**
Precompute and cache user feed pages.

### Q191. Counter cache?
**Level:** Intermediate

**Answer:**
Denormalized count column updated on change.

### Q192. Query result cache?
**Level:** Intermediate

**Answer:**
Cache expensive aggregation results.

### Q193. Session cache?
**Level:** Intermediate

**Answer:**
Store session in Redis for stateless app servers.

### Q194. Rate limit cache?
**Level:** Intermediate

**Answer:**
Token bucket counters in Redis.

### Q195. Pub/sub via Redis?
**Level:** Intermediate

**Answer:**
Real-time notifications channel.

### Q196. Cache monitoring?
**Level:** Intermediate

**Answer:**
Hit rate, eviction rate, memory usage metrics.

### Q197. Cache size planning?
**Level:** Intermediate

**Answer:**
Hot data set size × object size × overhead.

### Q198. Serialization for cache?
**Level:** Intermediate

**Answer:**
JSON, MessagePack, or binary for smaller payloads.

### Q199. Immutable cache keys?
**Level:** Intermediate

**Answer:**
Version in key for cache busting on schema change.

### Q200. Partial cache update?
**Level:** Intermediate

**Answer:**
Update single field in Redis hash vs full invalidation.

### Q201. Cache for search?
**Level:** Intermediate

**Answer:**
Cache popular queries and autocomplete suggestions.

### Q202. Reverse proxy cache?
**Level:** Intermediate

**Answer:**
Nginx proxy_cache for HTTP responses.

### Q203. Cache aside code flow?
**Level:** Intermediate

**Answer:**
```javascript
async function getUser(id) {
  const cached = await redis.get(`user:${id}`);
  if (cached) return JSON.parse(cached);
  const user = await db.findById(id);
  await redis.setex(`user:${id}`, 3600, JSON.stringify(user));
  return user;
}
```

### Q204. When NOT to cache?
**Level:** Intermediate

**Answer:**
Strong consistency required, data changes constantly, low read ratio.

---

## 5. Load Balancing & Scaling

### Q205. What is load balancing?
**Level:** Basic

**Answer:**
Distribute traffic across multiple servers.

```javascript
class RR{constructor(s){this.s=s;this.i=0}next(){return this.s[this.i++%this.s.length]}}
```

### Q206. Round robin?
**Level:** Basic

**Answer:**
Sequential request to each server.

### Q207. Weighted round robin?
**Level:** Intermediate

**Answer:**
Servers get traffic proportional to weight.

```javascript
class WRR{constructor(s){this.e=s.flatMap(x=>Array(x.w).fill(x.h))}next(){return this.e[this.i++%this.e.length]}}
```

### Q208. Least connections?
**Level:** Intermediate

**Answer:**
Route to server with fewest active connections.

```javascript
class LC{pick(){return this.s.reduce((a,b)=>a.c<b.c?a:b)}}
```

### Q209. IP hash?
**Level:** Intermediate

**Answer:**
Hash client IP for sticky routing.

### Q210. Consistent hashing LB?
**Level:** Advanced

**Answer:**
Minimal redistribution on server add/remove.

```javascript
class CH{get(k){/* hash ring */}}
```

### Q211. L4 load balancer?
**Level:** Intermediate

**Answer:**
Routes by IP/port — fast, no HTTP awareness.

### Q212. L7 load balancer?
**Level:** Intermediate

**Answer:**
Routes by URL, headers, cookies — smarter.

### Q213. Health check in LB?
**Level:** Basic

**Answer:**
Remove unhealthy servers from pool.

### Q214. Auto scaling?
**Level:** Intermediate

**Answer:**
Add/remove instances based on CPU, QPS, queue depth.

### Q215. Horizontal pod autoscaler?
**Level:** Intermediate

**Answer:**
Kubernetes scales pods by metrics.

### Q216. Vertical pod autoscaler?
**Level:** Advanced

**Answer:**
Adjusts CPU/memory requests per pod.

### Q217. Scale up triggers?
**Level:** Intermediate

**Answer:**
CPU > 70%, latency P99 > SLO, queue backlog.

### Q218. Scale down cautiously?
**Level:** Intermediate

**Answer:**
Cooldown period to avoid flapping.

### Q219. Stateless scaling?
**Level:** Basic

**Answer:**
Any instance handles any request — easiest to scale.

### Q220. Database scaling?
**Level:** Intermediate

**Answer:**
Read replicas, sharding, connection pooling.

### Q221. Read/write splitting?
**Level:** Intermediate

**Answer:**
Writes to primary, reads to replicas.

### Q222. Microservices scaling?
**Level:** Intermediate

**Answer:**
Scale hot services independently.

### Q223. Queue-based scaling?
**Level:** Intermediate

**Answer:**
Workers scale with queue depth.

### Q224. Cell-based architecture?
**Level:** Advanced

**Answer:**
Independent deployment cells for blast radius.

### Q225. Sharding for scale?
**Level:** Intermediate

**Answer:**
Partition data when single DB exceeds capacity.

### Q226. Elasticity?
**Level:** Basic

**Answer:**
Scale in/out automatically with demand.

### Q227. Over-provisioning?
**Level:** Intermediate

**Answer:**
Extra capacity for spikes — costly but safe.

### Q228. Performance testing at scale?
**Level:** Intermediate

**Answer:**
Load test at 2× expected peak.

### Q229. Bottleneck at scale?
**Level:** Intermediate

**Answer:**
Usually DB, then network, then CPU.

### Q230. Connection limit?
**Level:** Intermediate

**Answer:**
DB and LB have max connections — pool carefully.

### Q231. NGINX vs HAProxy?
**Level:** Intermediate

**Answer:**
Both L7 LB. NGINX also reverse proxy/static. HAProxy high perf LB.

### Q232. AWS ALB vs NLB?
**Level:** Intermediate

**Answer:**
ALB: L7 HTTP. NLB: L4 high throughput low latency.

### Q233. Global load balancing?
**Level:** Advanced

**Answer:**
Route users to nearest healthy region.

### Q234. DNS-based GSLB?
**Level:** Advanced

**Answer:**
GeoDNS returns regional IP.

### Q235. Anycast LB?
**Level:** Advanced

**Answer:**
BGP routes to nearest PoP.

### Q236. Session affinity?
**Level:** Intermediate

**Answer:**
Sticky sessions for stateful apps.

### Q237. Drain connections?
**Level:** Intermediate

**Answer:**
Graceful removal: stop new, finish in-flight.

### Q238. Rolling update?
**Level:** Intermediate

**Answer:**
Replace instances incrementally.

### Q239. Autoscaling lag?
**Level:** Intermediate

**Answer:**
Instances take minutes to boot — pre-warm for known spikes.

### Q240. Predictive scaling?
**Level:** Advanced

**Answer:**
ML forecasts traffic; scale proactively.

### Q241. Cost at scale?
**Level:** Intermediate

**Answer:**
Reserved instances, spot for batch, right-sizing.

### Q242. Multi-AZ deployment?
**Level:** Intermediate

**Answer:**
Instances across availability zones.

### Q243. Multi-region active-active?
**Level:** Advanced

**Answer:**
All regions serve; data sync challenge.

### Q244. Partition tolerance at scale?
**Level:** Advanced

**Answer:**
Design for network splits between nodes.

### Q245. Bulkhead pattern?
**Level:** Advanced

**Answer:**
Isolate resources so one failure doesn't drain all.

### Q246. Noisy neighbor?
**Level:** Advanced

**Answer:**
One tenant consumes disproportionate resources. Isolate tenants.

### Q247. Rate limiting for scale?
**Level:** Intermediate

**Answer:**
Protect backend from overload.

### Q248. Backpressure?
**Level:** Intermediate

**Answer:**
Slow producers when consumers overwhelmed.

### Q249. Async processing for scale?
**Level:** Intermediate

**Answer:**
Offload heavy work to queues.

### Q250. Batching writes?
**Level:** Intermediate

**Answer:**
Aggregate writes to reduce DB load.

### Q251. Compression at scale?
**Level:** Intermediate

**Answer:**
gzip/brotli reduces bandwidth.

### Q252. Pagination at scale?
**Level:** Intermediate

**Answer:**
Cursor-based avoids OFFSET cost.

```javascript
class Bus{constructor(){this.l={}}on(e,fn){(this.l[e]??=[]).push(fn)}emit(e,d){(this.l[e]||[]).forEach(fn=>fn(d))}}
```

### Q253. Data locality?
**Level:** Advanced

**Answer:**
Process data near storage to reduce network.

```javascript
class MQ{constructor(){this.q=[]}enq(m){this.q.push(m)}}
```

### Q254. Edge computing?
**Level:** Advanced

**Answer:**
Compute at CDN edge for low latency.

```javascript
class IC{constructor(){this.s=new Set()}consume(m){if(this.s.has(m.id))return;this.s.add(m.id)}}
```

### Q255. Serverless scaling?
**Level:** Intermediate

**Answer:**
Auto-scales per invocation; cold start trade-off.

### Q256. Kubernetes HPA formula?
**Level:** Advanced

**Answer:**
replicas = ceil(currentReplicas × (currentMetric / targetMetric)).

---

## 6. Messaging & Event Systems

### Q257. Message queue purpose?
**Level:** Basic

**Answer:**
Decouple producers and consumers; async processing.

### Q258. Kafka?
**Level:** Intermediate

**Answer:**
Distributed log; high throughput; retention; replay.

### Q259. RabbitMQ?
**Level:** Intermediate

**Answer:**
Traditional message broker; exchanges and queues.

### Q260. SQS?
**Level:** Intermediate

**Answer:**
AWS managed queue; at-least-once delivery.

### Q261. Pub/Sub pattern?
**Level:** Basic

**Answer:**
Publishers send to topic; subscribers receive.

### Q262. Point-to-point?
**Level:** Intermediate

**Answer:**
One consumer per message (competing consumers).

### Q263. At-least-once delivery?
**Level:** Intermediate

**Answer:**
Message may be delivered multiple times — idempotent consumers.

### Q264. At-most-once delivery?
**Level:** Intermediate

**Answer:**
May lose messages; no retry.

### Q265. Exactly-once?
**Level:** Advanced

**Answer:**
Hardest — Kafka transactions + idempotent producer.

### Q266. Message ordering?
**Level:** Intermediate

**Answer:**
Single partition preserves order in Kafka.

### Q267. Consumer group?
**Level:** Intermediate

**Answer:**
Kafka consumers share partitions.

### Q268. Dead letter queue?
**Level:** Intermediate

**Answer:**
Failed messages after max retries.

### Q269. Backpressure in queues?
**Level:** Intermediate

**Answer:**
Queue depth triggers scale or throttle.

### Q270. Event-driven architecture?
**Level:** Intermediate

**Answer:**
Services communicate via events not direct calls.

### Q271. Event notification vs event-carried state?
**Level:** Intermediate

**Answer:**
Notification: ref only. Carried: full payload.

### Q272. CQRS with events?
**Level:** Advanced

**Answer:**
Commands write; events update read model.

### Q273. Event sourcing?
**Level:** Advanced

**Answer:**
Store state changes as event log.

### Q274. Idempotent consumer?
**Level:** Intermediate

**Answer:**
Process same message twice safely — use dedup key.

### Q275. Message schema?
**Level:** Intermediate

**Answer:**
Avro/Protobuf with schema registry.

### Q276. Kafka partitions?
**Level:** Intermediate

**Answer:**
Parallelism unit; key determines partition.

### Q277. Kafka retention?
**Level:** Intermediate

**Answer:**
Keep messages for days regardless of consumption.

### Q278. Kafka replay?
**Level:** Advanced

**Answer:**
Re-process historical events.

### Q279. RabbitMQ exchanges?
**Level:** Intermediate

**Answer:**
Direct, fanout, topic, headers.

### Q280. Priority queue?
**Level:** Intermediate

**Answer:**
High-priority messages processed first.

### Q281. Delayed messages?
**Level:** Intermediate

**Answer:**
Schedule delivery after delay.

### Q282. Transactional outbox?
**Level:** Advanced

**Answer:**
Atomic DB write + event publish.

### Q283. Inbox pattern?
**Level:** Advanced

**Answer:**
Dedup incoming messages by ID.

### Q284. Saga choreography?
**Level:** Advanced

**Answer:**
Services react to events; no central coordinator.

### Q285. Saga orchestration?
**Level:** Advanced

**Answer:**
Central orchestrator directs steps.

### Q286. Compensating transaction?
**Level:** Advanced

**Answer:**
Undo step in saga on failure.

### Q287. Webhook delivery?
**Level:** Intermediate

**Answer:**
HTTP POST on event; retry with backoff.

### Q288. Push vs pull messaging?
**Level:** Intermediate

**Answer:**
Push: broker sends. Pull: consumer polls.

### Q289. Long polling SQS?
**Level:** Intermediate

**Answer:**
Reduces empty poll costs.

### Q290. Fan-out SNS+SQS?
**Level:** Intermediate

**Answer:**
SNS broadcasts to multiple SQS queues.

### Q291. Stream processing?
**Level:** Advanced

**Answer:**
Kafka Streams, Flink process events in real-time.

### Q292. Windowing?
**Level:** Advanced

**Answer:**
Tumbling, sliding, session windows for aggregations.

### Q293. CEP?
**Level:** Advanced

**Answer:**
Complex Event Processing — detect patterns.

### Q294. Message size limits?
**Level:** Intermediate

**Answer:**
Large payloads → store in S3, send reference.

### Q295. Poison message?
**Level:** Intermediate

**Answer:**
Always fails processing → DLQ.

### Q296. Ordering vs parallelism trade-off?
**Level:** Intermediate

**Answer:**
More partitions = more parallelism, less ordering.

### Q297. Kafka vs RabbitMQ?
**Level:** Intermediate

**Answer:**
Kafka: log, replay, high volume. RabbitMQ: routing, task queues.

### Q298. ZeroMQ?
**Level:** Advanced

**Answer:**
Lightweight messaging library.

### Q299. NATS?
**Level:** Advanced

**Answer:**
Cloud-native messaging; simple pub/sub.

### Q300. Pulsar?
**Level:** Advanced

**Answer:**
Multi-tenancy, geo-replication messaging.

### Q301. Event bus in monolith?
**Level:** Intermediate

**Answer:**
In-process pub/sub decouples modules.

### Q302. Change notifications?
**Level:** Advanced

**Answer:**
DB trigger or CDC → event stream.

```javascript
class CB{constructor(){this.f=0;this.s='CLOSED'}async call(fn){if(this.s==='OPEN')throw Error();try{return await fn()}catch(e){if(++this.f>=5)this.s='OPEN';throw e}}}
```

### Q303. Notification service design?
**Level:** Intermediate

**Answer:**
Queue per channel; workers send email/SMS/push.

```javascript
async function retry(fn,n=3){for(let i=0;i<n;i++){try{return await fn()}catch(e){await new Promise(r=>setTimeout(r,2**i*1000))}}}
```

### Q304. Log aggregation?
**Level:** Intermediate

**Answer:**
Fluentd/Logstash → Elasticsearch.

```javascript
class Idem{constructor(){this.c=new Map()}async go(k,fn){if(this.c.has(k))return this.c.get(k);const r=await fn();this.c.set(k,r);return r}}
```

### Q305. Metrics pipeline?
**Level:** Intermediate

**Answer:**
StatsD/Prometheus → Grafana.

```javascript
class Reg{constructor(){this.s={}}add(n,i){(this.s[n]??=[]).push(i)}get(n){const l=this.s[n];return l[Math.floor(Math.random()*l.length)]}}
```

### Q306. Trace propagation?
**Level:** Advanced

**Answer:**
Inject trace ID in messages for distributed tracing.

### Q307. Backpressure Kafka?
**Level:** Advanced

**Answer:**
Pause consumer when processing slow.

### Q308. Consumer lag monitoring?
**Level:** Intermediate

**Answer:**
Alert when lag exceeds threshold.

---

## 7. Distributed Systems

### Q309. Distributed system challenges?
**Level:** Basic

**Answer:**
Partial failure, latency, consistency, clock skew.

### Q310. Fallacies of distributed computing?
**Level:** Intermediate

**Answer:**
Network reliable, latency zero, bandwidth infinite, etc.

### Q311. Network partition?
**Level:** Intermediate

**Answer:**
Nodes can't communicate but keep running.

### Q312. Split brain?
**Level:** Advanced

**Answer:**
Two leaders during partition — use quorum/fencing.

### Q313. Consensus algorithms?
**Level:** Advanced

**Answer:**
Raft, Paxos — agree on value across nodes.

### Q314. Raft leader election?
**Level:** Advanced

**Answer:**
Majority vote; term numbers prevent stale leaders.

### Q315. Zookeeper?
**Level:** Advanced

**Answer:**
Coordination service: config, leader election, locks.

### Q316. Distributed lock?
**Level:** Advanced

**Answer:**
Redis Redlock, Zookeeper ephemeral nodes.

### Q317. Fencing token?
**Level:** Advanced

**Answer:**
Monotonic token prevents stale lock holder writes.

### Q318. Vector clock?
**Level:** Advanced

**Answer:**
Track causality without synchronized clocks.

### Q319. Lamport timestamp?
**Level:** Advanced

**Answer:**
Logical clock ordering events.

### Q320. Happens-before?
**Level:** Advanced

**Answer:**
Causal ordering guarantee.

### Q321. Byzantine fault tolerance?
**Level:** Advanced

**Answer:**
Nodes may lie or behave arbitrarily — costly.

### Q322. Gossip protocol?
**Level:** Advanced

**Answer:**
Nodes randomly exchange state — eventual consistency.

### Q323. Consistent hashing ring?
**Level:** Advanced

**Answer:**
Nodes and keys on ring; walk clockwise.

### Q324. Virtual nodes?
**Level:** Advanced

**Answer:**
More even distribution on consistent hash ring.

### Q325. Hinted handoff?
**Level:** Advanced

**Answer:**
Dynamo: temporarily store for down node.

### Q326. Merkle tree sync?
**Level:** Advanced

**Answer:**
Compare hash trees to find differing replicas.

### Q327. Anti-entropy?
**Level:** Advanced

**Answer:**
Background replica synchronization.

### Q328. Read repair?
**Level:** Advanced

**Answer:**
Fix stale replica on read.

### Q329. Sloppy quorum?
**Level:** Advanced

**Answer:**
W+R may not exceed N during failure — tunable.

### Q330. CRDT?
**Level:** Advanced

**Answer:**
Conflict-free Replicated Data Types — merge without coordination.

### Q331. Operational transformation?
**Level:** Advanced

**Answer:**
Collaborative editing conflict resolution.

### Q332. Distributed tracing?
**Level:** Intermediate

**Answer:**
Jaeger, Zipkin — trace requests across services.

### Q333. Correlation ID?
**Level:** Intermediate

**Answer:**
Unique ID propagated across all service calls.

### Q334. Service discovery?
**Level:** Intermediate

**Answer:**
Consul, Eureka — services find each other.

### Q335. DNS service discovery?
**Level:** Intermediate

**Answer:**
SRV records for service endpoints.

### Q336. Kubernetes service discovery?
**Level:** Intermediate

**Answer:**
Cluster DNS resolves service names.

### Q337. Circuit breaker?
**Level:** Intermediate

**Answer:**
Stop calling failing service; periodic retry.

### Q338. Bulkhead?
**Level:** Advanced

**Answer:**
Isolate thread pools per dependency.

### Q339. Timeout and retry?
**Level:** Intermediate

**Answer:**
Set timeouts; retry with exponential backoff + jitter.

### Q340. Idempotency keys?
**Level:** Intermediate

**Answer:**
Same key = same result; safe retries.

### Q341. Distributed transaction alternatives?
**Level:** Advanced

**Answer:**
Saga, outbox, eventual consistency — avoid 2PC.

### Q342. Clock skew?
**Level:** Advanced

**Answer:**
Nodes have different times — use logical clocks.

### Q343. NTP?
**Level:** Intermediate

**Answer:**
Network Time Protocol synchronizes clocks.

### Q344. Leap second problem?
**Level:** Advanced

**Answer:**
Time jumps cause issues — use logical clocks for ordering.

### Q345. CAP in practice?
**Level:** Intermediate

**Answer:**
Most systems choose AP with eventual consistency.

### Q346. PACELC examples?
**Level:** Advanced

**Answer:**
Dynamo AP+Latency. Spanner CP+Consistency.

### Q347. Microservices communication?
**Level:** Intermediate

**Answer:**
Sync REST/gRPC or async events.

### Q348. API composition?
**Level:** Intermediate

**Answer:**
Gateway aggregates multiple service calls.

### Q349. BFF pattern?
**Level:** Intermediate

**Answer:**
Backend For Frontend — tailored API per client.

### Q350. Strangler fig?
**Level:** Advanced

**Answer:**
Gradually replace monolith by routing to new services.

### Q351. Database saga example?
**Level:** Advanced

**Answer:**
Order → Payment → Inventory with compensations.

```javascript
async function fanW(t,f){await Promise.all(f.map(u=>tl.add(u,t)))}
```

### Q352. Leader election use cases?
**Level:** Advanced

**Answer:**
Single writer, scheduled jobs, failover.

```javascript
async function fanR(u,fs){const t=await Promise.all(fs.map(getTweets));return t.flat().sort((a,b)=>b.ts-a.ts)}
```

### Q353. Split-brain prevention?
**Level:** Advanced

**Answer:**
Quorum, STONITH (shoot the other node).

### Q354. Distributed cache invalidation?
**Level:** Advanced

**Answer:**
Pub/sub broadcast invalidation events.

### Q355. Geo-replication?
**Level:** Advanced

**Answer:**
Multi-region data copies — latency vs consistency.

### Q356. Multi-master conflict?
**Level:** Advanced

**Answer:**
Last-write-wins, version vectors, CRDTs.

### Q357. Failure detection?
**Level:** Advanced

**Answer:**
Heartbeats, phi accrual failure detector.

### Q358. Graceful shutdown distributed?
**Level:** Intermediate

**Answer:**
Deregister from LB, drain, then stop.

### Q359. Chaos monkey?
**Level:** Advanced

**Answer:**
Randomly terminate instances to test resilience.

### Q360. Blast radius?
**Level:** Advanced

**Answer:**
Limit impact scope — cells, regions, shards.

### Q361. Dependency graph?
**Level:** Intermediate

**Answer:**
Map service deps to understand cascade failures.

---

## 8. Classic HLD System Design Problems

### Q362. Design URL Shortener?
**Level:** Intermediate

**Answer:**
API: shorten/redirect. Base62 encoding. Counter or hash. SQL/NoSQL for mapping. Redis cache hot URLs. Analytics async.

```javascript
class Short{constructor(){this.m=new Map();this.n=1}shorten(u){const c=(this.n++).toString(36);this.m.set(c,u);return c}}
```

### Q363. URL shortener — hash collision?
**Level:** Intermediate

**Answer:**
Check DB on collision; append salt and retry.

```javascript
class Paste{create(c,ttl=3600){const id=Math.random().toString(36).slice(2);this.store.set(id,{c,exp:Date.now()+ttl*1000});return id}}
```

### Q364. URL shortener — scale?
**Level:** Intermediate

**Answer:**
Read-heavy: cache + CDN. Billions of URLs: shard by hash.

### Q365. Design Pastebin?
**Level:** Intermediate

**Answer:**
Store text blob in object storage (S3). Metadata in DB. TTL expiry job.

### Q366. Design Twitter/X?
**Level:** Advanced

**Answer:**
Post tweet, timeline, follow, search. Fan-out on write vs read. Redis cache. Kafka events.

```javascript
class RL{allow(id){const n=Date.now();const h=(this.m.get(id)||[]).filter(t=>n-t<this.w);if(h.length>=this.max)return false;h.push(n);this.m.set(id,h);return true}}
```

### Q367. Twitter fan-out on write?
**Level:** Advanced

**Answer:**
Precompute follower timelines on tweet — fast read, slow write for celebrities.

### Q368. Twitter fan-out on read?
**Level:** Advanced

**Answer:**
Merge tweets on read — slow read, simple write. Hybrid for celebrities.

```javascript
class Trie{insert(w){let n=this.root;for(const c of w){n.children??={};n=n.children[c]??={};}}}
```

### Q369. Design Instagram?
**Level:** Advanced

**Answer:**
Photo upload to S3, metadata DB, feed generation, CDN for images.

### Q370. Design Facebook News Feed?
**Level:** Advanced

**Answer:**
Ranking ML model, fan-out hybrid, cache per user feed.

```javascript
class LB{update(u,s){this.m.set(u,(this.m.get(u)||0)+s)}top(n){return[...this.m].sort((a,b)=>b[1]-a[1]).slice(0,n)}}
```

### Q371. Design WhatsApp/Messenger?
**Level:** Advanced

**Answer:**
WebSocket connections, message queue, E2E encryption, delivery receipts.

### Q372. Design YouTube?
**Level:** Advanced

**Answer:**
Upload → transcode pipeline (workers) → CDN. Metadata DB. Recommendation system.

### Q373. Design Netflix?
**Level:** Advanced

**Answer:**
CDN (Open Connect), microservices, recommendation, adaptive bitrate streaming.

### Q374. Design Uber/Lyft?
**Level:** Advanced

**Answer:**
Geo-index (QuadTree/Geohash), matching service, real-time location via WebSocket.

```javascript
class Snowflake{gen(){return((Date.now()-this.epoch)<<22)|(this.mid<<12)|this.seq++}}
```

### Q375. Uber surge pricing?
**Level:** Advanced

**Answer:**
Demand/supply ratio per geo cell; dynamic multiplier.

### Q376. Design Airbnb?
**Level:** Advanced

**Answer:**
Search (Elasticsearch), booking with inventory lock, payments, reviews.

```javascript
class CM{send(u,m){this.c.get(u)?.send(JSON.stringify(m))}}
```

### Q377. Design Amazon/e-commerce?
**Level:** Advanced

**Answer:**
Catalog, cart, inventory, order, payment, search, recommendations.

```javascript
const near=(ds,lat,lng,r)=>ds.filter(d=>dist(lat,lng,d.lat,d.lng)<=r)
```

### Q378. Design payment system?
**Level:** Advanced

**Answer:**
Idempotency, ledger (double-entry), PCI compliance, fraud detection.

```javascript
const surge=(d,s)=>s===0?3:Math.min(3,1+d/s*0.5)
```

### Q379. Design notification system?
**Level:** Intermediate

**Answer:**
Multi-channel (email/SMS/push), template engine, preference, queue workers.

### Q380. Design rate limiter?
**Level:** Intermediate

**Answer:**
Token bucket/sliding window in Redis. Middleware checks before handler.

```javascript
class Hold{hold(s,seat,u){const k=`${s}:${seat}`;this.h.set(k,{u,exp:Date.now()+300000});setTimeout(()=>this.h.delete(k),300000)}}
```

### Q381. Design distributed cache?
**Level:** Intermediate

**Answer:**
Consistent hashing, replication, cache-aside, eviction LRU.

### Q382. Design search autocomplete?
**Level:** Intermediate

**Answer:**
Trie in memory + popular queries cache. Debounce client.

### Q383. Design web crawler?
**Level:** Advanced

**Answer:**
URL frontier queue, politeness delay, dedup bloom filter, distributed workers.

### Q384. Design Typeahead?
**Level:** Intermediate

**Answer:**
Prefix index, ranking by popularity, cache top queries.

### Q385. Design Google Drive?
**Level:** Advanced

**Answer:**
Chunked upload, metadata DB, block storage, sync via version vectors.

### Q386. Design Dropbox sync?
**Level:** Advanced

**Answer:**
Block-level diff, metadata server, conflict copies.

```javascript
class NSvc{async notify(u,m,p){if(p.email)await email.send(u.email,m)}}
```

### Q387. Design ticket booking (BookMyShow)?
**Level:** Intermediate

**Answer:**
Seat map, temporary lock (Redis TTL), payment, confirm booking.

### Q388. Design stock ticker?
**Level:** Intermediate

**Answer:**
Pub/sub real-time prices, WebSocket to clients, time-series DB.

### Q389. Design leaderboard?
**Level:** Intermediate

**Answer:**
Redis Sorted Set — O(log N) score updates.

### Q390. Design metrics monitoring?
**Level:** Advanced

**Answer:**
Agents collect → Kafka → time-series DB → Grafana alerts.

### Q391. Design logging system?
**Level:** Advanced

**Answer:**
App → Fluentd → Kafka → Elasticsearch → Kibana.

### Q392. Design email service?
**Level:** Intermediate

**Answer:**
SMTP queue, template rendering, bounce handling, spam filter.

### Q393. Design SMS gateway?
**Level:** Intermediate

**Answer:**
Provider API integration, rate limits, delivery status webhook.

### Q394. Design API rate limiter distributed?
**Level:** Intermediate

**Answer:**
Redis INCR with TTL per window or token bucket Lua script.

### Q395. Design distributed job scheduler?
**Level:** Advanced

**Answer:**
Leader election + time wheel + worker queue (Celery/K8s jobs).

### Q396. Design Zoom/video conferencing?
**Level:** Advanced

**Answer:**
SFU/MCU media servers, signaling WebSocket, TURN/STUN NAT traversal.

### Q397. Design online judge (LeetCode)?
**Level:** Advanced

**Answer:**
Submit code → queue → sandboxed worker → result stored.

### Q398. Design Yelp?
**Level:** Intermediate

**Answer:**
Geo search (Elasticsearch geo_query), reviews, business profiles.

### Q399. Design Quora?
**Level:** Intermediate

**Answer:**
Q&A, feed, voting, search, notification on answers.

### Q400. Design Reddit?
**Level:** Advanced

**Answer:**
Subreddits, posts, comments tree, voting, hot ranking algorithm.

### Q401. Design Pinterest?
**Level:** Advanced

**Answer:**
Image pins, boards, graph DB for recommendations, CDN.

### Q402. Design LinkedIn?
**Level:** Advanced

**Answer:**
Professional graph, feed, job matching, messaging, skill endorsements.

### Q403. Design Tinder?
**Level:** Intermediate

**Answer:**
Geo matching, swipe queue, real-time match notification.

### Q404. Design food delivery (DoorDash)?
**Level:** Advanced

**Answer:**
Order flow, dispatch matching, real-time tracking, ETA.

### Q405. Design ride-sharing ETA?
**Level:** Advanced

**Answer:**
Map service + traffic data + ML model.

### Q406. Design Spotify?
**Level:** Advanced

**Answer:**
Music streaming CDN, playlist DB, recommendation, offline cache.

### Q407. Design Slack?
**Level:** Advanced

**Answer:**
Channels, real-time messaging WebSocket, search, file sharing.

### Q408. Design GitHub?
**Level:** Advanced

**Answer:**
Git object storage, PR review, CI integration, search code.

### Q409. Design Stack Overflow?
**Level:** Intermediate

**Answer:**
Q&A voting, reputation, search (Elasticsearch), moderation.

### Q410. Design rate-limited API platform?
**Level:** Intermediate

**Answer:**
API keys, tier quotas, Redis counters, 429 responses.

### Q411. Design multi-tenant SaaS?
**Level:** Advanced

**Answer:**
Tenant isolation: row-level or schema-per-tenant or DB-per-tenant.

### Q412. Design ad click aggregator?
**Level:** Advanced

**Answer:**
Stream processing (Flink) aggregates clicks per minute.

### Q413. Design CDN?
**Level:** Advanced

**Answer:**
Edge PoPs, origin pull, cache rules, purge API, geo routing.

### Q414. Design DNS system?
**Level:** Advanced

**Answer:**
Root → TLD → authoritative. Recursive resolver. Anycast.

### Q415. Design distributed ID generator?
**Level:** Advanced

**Answer:**
Snowflake: timestamp + machine ID + sequence.

### Q416. Snowflake ID structure?
**Level:** Advanced

**Answer:**
41-bit timestamp + 10-bit machine + 12-bit sequence = 64-bit.

### Q417. Design UUID vs Snowflake?
**Level:** Intermediate

**Answer:**
UUID: random, no ordering. Snowflake: time-sortable, coordination needed.

### Q418. Design chat system (WhatsApp scale)?
**Level:** Advanced

**Answer:**
Persistent connections, message store (Cassandra), delivery acks, groups.

### Q419. Design news aggregator?
**Level:** Intermediate

**Answer:**
RSS crawl, dedup, categorize, personalized feed.

### Q420. Design recommendation system?
**Level:** Advanced

**Answer:**
Collaborative filtering + content-based + ML ranking pipeline.

### Q421. Design fraud detection?
**Level:** Advanced

**Answer:**
Rule engine + ML model on transaction stream; block in real-time.

### Q422. Design billing system?
**Level:** Advanced

**Answer:**
Usage metering, invoice generation, payment retry, dunning.

### Q423. Design config service?
**Level:** Advanced

**Answer:**
Versioned config, watch API (like etcd/Consul), rollout by percentage.

### Q424. Design feature flag service?
**Level:** Intermediate

**Answer:**
Evaluate flags per user/context; SDK polls or streams updates.

### Q425. Design A/B testing platform?
**Level:** Advanced

**Answer:**
Consistent user bucketing, metrics collection, statistical analysis.

### Q426. Design analytics pipeline?
**Level:** Advanced

**Answer:**
Events → Kafka → stream/batch processing → data warehouse.

### Q427. Design real-time dashboard?
**Level:** Advanced

**Answer:**
WebSocket push + stream aggregation + time-series DB.

### Q428. Design IoT platform?
**Level:** Advanced

**Answer:**
Device MQTT ingest → stream processing → alerts + storage.

### Q429. Design blockchain (high level)?
**Level:** Advanced

**Answer:**
Distributed ledger, consensus, immutable chain, smart contracts.

### Q430. Design game leaderboard global?
**Level:** Advanced

**Answer:**
Geo-sharded Redis sorted sets + periodic merge.

### Q431. Design parking lot (HLD)?
**Level:** Intermediate

**Answer:**
Sensors → API → DB + mobile app + payment integration.

### Q432. Design weather service?
**Level:** Intermediate

**Answer:**
Aggregate station data, cache forecasts, CDN for maps.

### Q433. Design calendar app (Google)?
**Level:** Advanced

**Answer:**
Event CRUD, recurrence (RRULE), sharing, sync conflict resolution.

### Q434. Design map/location service?
**Level:** Advanced

**Answer:**
Tile CDN, geocoding API, routing graph, real-time traffic.

### Q435. Design search engine?
**Level:** Advanced

**Answer:**
Crawl → index (inverted) → rank → serve. Billion-page scale.

### Q436. Design Wikipedia?
**Level:** Intermediate

**Answer:**
CDN static, edit API, revision history, search.

### Q437. Design online bookstore?
**Level:** Intermediate

**Answer:**
Catalog, search, cart, inventory, recommendations.

### Q438. Design pharmacy delivery?
**Level:** Advanced

**Answer:**
Prescription verification, inventory, compliance, delivery tracking.

### Q439. Design hospital management?
**Level:** Advanced

**Answer:**
Patient records (HIPAA), appointments, billing, EHR integration.

### Q440. Design banking system?
**Level:** Advanced

**Answer:**
Accounts, transactions (ACID), fraud, regulatory audit trail.

### Q441. Design stock exchange?
**Level:** Advanced

**Answer:**
Order matching engine, low latency, market data feed.

### Q442. Design visa application system?
**Level:** Intermediate

**Answer:**
Workflow states, document upload S3, notification, audit.

### Q443. Design voting system?
**Level:** Advanced

**Answer:**
Anonymous ballot, tamper-proof audit log, one vote per identity.

### Q444. Design parking sensor network?
**Level:** Intermediate

**Answer:**
IoT MQTT → ingest → real-time availability API.

### Q445. Design meme generator service?
**Level:** Basic

**Answer:**
Template CDN, text overlay API, share link generation.

### Q446. Design URL preview/unfurl?
**Level:** Intermediate

**Answer:**
Fetch URL, parse OG tags, cache preview card.

### Q447. Design distributed search?
**Level:** Advanced

**Answer:**
Shard inverted index by term/hash; scatter-gather queries.

### Q448. Design global chat?
**Level:** Advanced

**Answer:**
Region-local brokers, cross-region replication for DMs.

### Q449. Design coupon platform?
**Level:** Intermediate

**Answer:**
Code generation, redemption limits, fraud detection.

### Q450. Design loyalty points?
**Level:** Intermediate

**Answer:**
Ledger for points earn/burn; expiry job; idempotent transactions.

### Q451. Design multi-language i18n?
**Level:** Intermediate

**Answer:**
String DB, CDN cache per locale, fallback chain.

### Q452. Design webhook platform?
**Level:** Intermediate

**Answer:**
Event subscription, signed delivery, retry DLQ, delivery logs.

```javascript
async function hook(url,p){for(let i=0;i<3;i++){try{const r=await fetch(url,{method:'POST',body:JSON.stringify(p)});if(r.ok)return}catch(e){}await sleep(2**i*1000)}}
```

### Q453. Design CI/CD system?
**Level:** Advanced

**Answer:**
Git webhook → build workers → artifact store → deploy pipeline.

```javascript
class Tenant{static id=null;static set(i){this.id=i}}
```

### Q454. Design container orchestration?
**Level:** Advanced

**Answer:**
K8s: scheduler, controller, etcd state, CNI networking.

### Q455. Design serverless platform?
**Level:** Advanced

**Answer:**
Event trigger → cold/warm container → auto-scale → billing per ms.

### Q456. Design API marketplace?
**Level:** Advanced

**Answer:**
Developer portal, API keys, usage metering, billing.

### Q457. Design compliance audit log?
**Level:** Advanced

**Answer:**
Append-only immutable log; WORM storage; retention policy.

### Q458. Design password manager?
**Level:** Advanced

**Answer:**
Zero-knowledge encryption, client-side encrypt, vault sync.

### Q459. Design two-factor auth service?
**Level:** Intermediate

**Answer:**
TOTP generation, SMS/email OTP, backup codes, rate limit.

### Q460. Design OAuth provider?
**Level:** Advanced

**Answer:**
Authorization code flow, token endpoint, refresh tokens, scopes.

### Q461. Design SSO enterprise?
**Level:** Advanced

**Answer:**
SAML/OIDC federation, IdP integration, session management.

### Q462. Design content moderation?
**Level:** Advanced

**Answer:**
ML classifier + human review queue + appeal workflow.

### Q463. Design DDoS protection service?
**Level:** Advanced

**Answer:**
Anycast scrubbing, rate limit, challenge, blackhole routing.

### Q464. Design blockchain wallet?
**Level:** Advanced

**Answer:**
Key management HSM, transaction signing, balance sync.

### Q465. Design ML model serving?
**Level:** Advanced

**Answer:**
Model registry, A/B versions, GPU inference servers, feature store.

### Q466. Design data lake?
**Level:** Advanced

**Answer:**
S3 raw zone → processed zone → catalog (Glue) → query (Athena).

### Q467. Design real-time bidding (ads)?
**Level:** Advanced

**Answer:**
<100ms bid request → ML score → auction → win notification.

### Q468. HLD interview tip?
**Level:** Basic

**Answer:**
Start simple monolith → identify bottlenecks → add cache/queue/shard.

### Q469. HLD — when to shard?
**Level:** Intermediate

**Answer:**
When single DB exceeds ~1-2TB or write QPS exceeds single node.

### Q470. HLD — when to cache?
**Level:** Basic

**Answer:**
Read QPS high, data tolerates staleness, repeated queries.

### Q471. HLD — when to queue?
**Level:** Basic

**Answer:**
Async work, spike absorption, decouple services.

---

## 9. Advanced HLD & Architecture

### Q472. Microservices vs monolith decision?
**Level:** Advanced

**Answer:**
Start monolith; split when team scale or independent scaling needed.

```javascript
class GW{use(m){this.m.push(m)}async handle(r){for(const m of this.m)await m(r);return this.route(r)}}
```

### Q473. Domain-Driven Design in HLD?
**Level:** Advanced

**Answer:**
Bounded contexts map to services; ubiquitous language.

```javascript
class Bloom{add(i){for(let k=0;k<3;k++)this.bits[this.hash(i,k)]=1}has(i){for(let k=0;k<3;k++)if(!this.bits[this.hash(i,k)])return false;return true}}
```

### Q474. Event storming?
**Level:** Advanced

**Answer:**
Workshop to discover domain events and aggregates.

```javascript
class SWRL{allow(k){const n=Date.now();const h=(this.m.get(k)||[]).filter(t=>n-t<this.w);if(h.length>=this.l)return false;h.push(n);this.m.set(k,h);return true}}
```

### Q475. API gateway patterns?
**Level:** Advanced

**Answer:**
Routing, auth, rate limit, response aggregation, protocol translation.

```javascript
class FF{on(n,ctx){return this.f[n]?.users?.includes(ctx.uid)||hash(ctx.uid)%100<this.f[n]?.pct}}
```

### Q476. Service mesh benefits?
**Level:** Advanced

**Answer:**
mTLS, observability, traffic management without app code changes.

```javascript
const bucket=(uid,exp)=>hash(`${uid}:${exp}`)%100
```

### Q477. Sidecar proxy?
**Level:** Advanced

**Answer:**
Envoy alongside each pod handles network concerns.

```javascript
class Analytics{track(e){this.b.push({...e,ts:Date.now()});if(this.b.length>=100)this.flush()}}
```

### Q478. Kubernetes architecture?
**Level:** Advanced

**Answer:**
Control plane (API server, scheduler, etcd) + worker nodes (kubelet, pods).

### Q479. Pod vs container?
**Level:** Intermediate

**Answer:**
Pod is K8s smallest deploy unit; may have multiple containers.

### Q480. StatefulSet?
**Level:** Advanced

**Answer:**
K8s controller for stateful apps with stable network ID.

### Q481. Helm charts?
**Level:** Intermediate

**Answer:**
K8s package manager for templated deployments.

### Q482. Infrastructure as Code?
**Level:** Intermediate

**Answer:**
Terraform/CloudFormation — reproducible infra.

### Q483. GitOps?
**Level:** Advanced

**Answer:**
Git as source of truth; ArgoCD syncs cluster to repo.

### Q484. Multi-cloud strategy?
**Level:** Advanced

**Answer:**
Avoid vendor lock-in; complexity cost. Active-active rare.

### Q485. Hybrid cloud?
**Level:** Advanced

**Answer:**
On-prem + cloud burst for peak capacity.

### Q486. Edge vs cloud?
**Level:** Advanced

**Answer:**
Edge: low latency local. Cloud: scale and managed services.

### Q487. Data mesh?
**Level:** Advanced

**Answer:**
Decentralized data ownership per domain with shared platform.

### Q488. FinOps?
**Level:** Intermediate

**Answer:**
Cloud cost optimization: right-sizing, reserved, spot.

### Q489. Zero trust security?
**Level:** Advanced

**Answer:**
Never trust, always verify — mTLS, identity per request.

### Q490. Defense in depth?
**Level:** Advanced

**Answer:**
Multiple security layers: WAF, network, auth, encryption.

### Q491. Encryption at rest vs transit?
**Level:** Intermediate

**Answer:**
Rest: disk encryption. Transit: TLS.

### Q492. KMS?
**Level:** Advanced

**Answer:**
Key Management Service — centralized encryption keys.

### Q493. HSM?
**Level:** Advanced

**Answer:**
Hardware Security Module — tamper-proof key storage.

### Q494. PII handling?
**Level:** Advanced

**Answer:**
Encrypt, tokenize, minimize collection, audit access, GDPR delete.

### Q495. GDPR right to erasure?
**Level:** Advanced

**Answer:**
Delete user data across all systems — track data lineage.

### Q496. HIPAA compliance?
**Level:** Advanced

**Answer:**
PHI encryption, access logs, BAA with vendors.

### Q497. PCI DSS?
**Level:** Advanced

**Answer:**
Card data isolation, tokenization, no storage of CVV.

### Q498. SOC 2?
**Level:** Advanced

**Answer:**
Security audit framework for SaaS.

### Q499. Penetration testing?
**Level:** Advanced

**Answer:**
Simulated attacks to find vulnerabilities.

### Q500. Blue team vs red team?
**Level:** Advanced

**Answer:**
Red attacks; blue defends. Purple collaborates.

### Q501. SIEM?
**Level:** Advanced

**Answer:**
Security Information and Event Management — aggregate security logs.

### Q502. WAF rules?
**Level:** Intermediate

**Answer:**
SQL injection, XSS, rate limit, geo block.

### Q503. OAuth2 flows?
**Level:** Advanced

**Answer:**
Authorization code (web), client credentials (service), PKCE (mobile).

### Q504. JWT structure?
**Level:** Intermediate

**Answer:**
Header.Payload.Signature — stateless auth.

### Q505. Token refresh strategy?
**Level:** Intermediate

**Answer:**
Short access token + long refresh token rotation.

### Q506. Rate limit algorithms?
**Level:** Intermediate

**Answer:**
Token bucket, leaky bucket, sliding window, fixed window.

### Q507. Global consistency models?
**Level:** Advanced

**Answer:**
Strong (Spanner), eventual (Dynamo), causal (vector clocks).

### Q508. Spanner TrueTime?
**Level:** Advanced

**Answer:**
GPS + atomic clocks for global consistent timestamps.

### Q509. Google Spanner architecture?
**Level:** Advanced

**Answer:**
Paxos per shard; TrueTime for external consistency.

### Q510. Dynamo paper key ideas?
**Level:** Advanced

**Answer:**
Consistent hashing, vector clocks, quorum, gossip.

### Q511. MapReduce?
**Level:** Advanced

**Answer:**
Distributed batch processing: map → shuffle → reduce.

### Q512. Spark vs MapReduce?
**Level:** Advanced

**Answer:**
Spark in-memory; faster iterative processing.

### Q513. Lambda architecture?
**Level:** Advanced

**Answer:**
Batch layer + speed layer + serving layer.

### Q514. Kappa architecture?
**Level:** Advanced

**Answer:**
Single stream processing path — simpler than Lambda.

### Q515. Data lakehouse?
**Level:** Advanced

**Answer:**
Delta Lake/Iceberg — ACID on object storage.

### Q516. Feature store?
**Level:** Advanced

**Answer:**
Centralized ML features for training and serving.

### Q517. Model drift detection?
**Level:** Advanced

**Answer:**
Monitor prediction distribution vs training.

### Q518. A/B test statistical significance?
**Level:** Advanced

**Answer:**
Chi-square or t-test; sufficient sample size.

### Q519. Real-time personalization?
**Level:** Advanced

**Answer:**
Stream features + online ML inference <50ms.

### Q520. Cost-performance Pareto?
**Level:** Advanced

**Answer:**
Optimize cost at given latency/availability target.

### Q521. Technical debt in architecture?
**Level:** Advanced

**Answer:**
Document trade-offs; plan evolution path.

### Q522. Architecture decision record (ADR)?
**Level:** Intermediate

**Answer:**
Document context, decision, consequences.

### Q523. C4 model?
**Level:** Intermediate

**Answer:**
Context, Container, Component, Code diagrams.

### Q524. TOGAF?
**Level:** Advanced

**Answer:**
Enterprise architecture framework.

### Q525. Well-Architected Framework?
**Level:** Advanced

**Answer:**
AWS: Operational Excellence, Security, Reliability, Performance, Cost.

### Q526. Site Reliability Engineering?
**Level:** Advanced

**Answer:**
SRE: error budgets, toil reduction, automation.

### Q527. Error budget?
**Level:** Advanced

**Answer:**
Allowed downtime derived from SLO; balance velocity vs reliability.

### Q528. Toil?
**Level:** Advanced

**Answer:**
Manual repetitive ops work — automate away.

### Q529. Runbook?
**Level:** Intermediate

**Answer:**
Step-by-step incident response procedure.

### Q530. Postmortem blameless?
**Level:** Intermediate

**Answer:**
Learn from incidents without blaming individuals.

### Q531. MTTR vs MTBF?
**Level:** Intermediate

**Answer:**
MTTR: mean time to repair. MTBF: mean time between failures.

### Q532. Capacity headroom?
**Level:** Intermediate

**Answer:**
Run at 70% max —留 buffer for spikes and failures.

### Q533. Platform engineering?
**Level:** Advanced

**Answer:**
Internal developer platform abstracts infra complexity.

### Q534. Developer experience (DX)?
**Level:** Advanced

**Answer:**
Self-service deploy, good docs, fast feedback loops.

### Q535. API-first design?
**Level:** Intermediate

**Answer:**
Design API contract before implementation.

### Q536. Contract testing?
**Level:** Advanced

**Answer:**
Pact — consumer-driven contract verification.

### Q537. Backward compatible evolution?
**Level:** Intermediate

**Answer:**
Additive schema changes; never remove required fields.

### Q538. Schema registry?
**Level:** Advanced

**Answer:**
Central Avro/Protobuf schema versioning.

### Q539. Multi-tenancy noisy neighbor?
**Level:** Advanced

**Answer:**
Per-tenant rate limits and resource quotas.

### Q540. Tenant data isolation?
**Level:** Advanced

**Answer:**
Row-level security or dedicated schema/DB per tenant.

### Q541. Compliance data residency?
**Level:** Advanced

**Answer:**
Store EU user data in EU region only.

### Q542. Cross-region data sync?
**Level:** Advanced

**Answer:**
Async replication; conflict resolution on write.

### Q543. Active-active database?
**Level:** Advanced

**Answer:**
CockroachDB, Spanner — distributed consensus.

### Q544. Read-your-writes consistency?
**Level:** Advanced

**Answer:**
User sees own writes immediately — session stickiness or quorum.

### Q545. Monotonic reads?
**Level:** Advanced

**Answer:**
Successive reads never go backward in time.

### Q546. Causal consistency?
**Level:** Advanced

**Answer:**
Causally related ops seen in order.

### Q547. Session consistency?
**Level:** Advanced

**Answer:**
Consistency guarantees within a client session.

### Q548. DynamoDB design?
**Level:** Advanced

**Answer:**
Single-table design, GSI for alternate access patterns.

### Q549. Single-table design?
**Level:** Advanced

**Answer:**
Multiple entity types in one table with composite keys.

### Q550. Access pattern first design?
**Level:** Advanced

**Answer:**
Design schema around queries not entities.

### Q551. Hot partition DynamoDB?
**Level:** Advanced

**Answer:**
Write sharding with suffix keys.

### Q552. Global tables?
**Level:** Advanced

**Answer:**
Multi-region DynamoDB replication.

### Q553. Aurora architecture?
**Level:** Advanced

**Answer:**
Shared storage layer; compute nodes scale independently.

### Q554. Neon/serverless Postgres?
**Level:** Advanced

**Answer:**
Separate compute and storage; scale to zero.

### Q555. Vitess?
**Level:** Advanced

**Answer:**
MySQL sharding middleware used by YouTube.

### Q556. TiDB?
**Level:** Advanced

**Answer:**
Distributed HTAP MySQL-compatible.

### Q557. ClickHouse?
**Level:** Advanced

**Answer:**
Columnar OLAP for real-time analytics.

### Q558. Elasticsearch architecture?
**Level:** Advanced

**Answer:**
Index → shards → replicas. Inverted index search.

### Q559. Inverted index?
**Level:** Intermediate

**Answer:**
Term → document IDs mapping for full-text search.

### Q560. TF-IDF?
**Level:** Advanced

**Answer:**
Term frequency-inverse document frequency ranking.

### Q561. BM25?
**Level:** Advanced

**Answer:**
Improved TF-IDF ranking used in Elasticsearch.

### Q562. Search relevance tuning?
**Level:** Advanced

**Answer:**
Boost fields, function scores, learning to rank.

### Q563. Federated search?
**Level:** Advanced

**Answer:**
Query multiple indexes/services; merge results.

### Q564. GraphQL federation?
**Level:** Advanced

**Answer:**
Compose multiple GraphQL subgraphs.

### Q565. BFF for mobile vs web?
**Level:** Advanced

**Answer:**
Different aggregation and payload sizes per client.

### Q566. Mobile offline-first?
**Level:** Advanced

**Answer:**
Local DB sync; conflict resolution on reconnect.

### Q567. CRDT for collaborative editing?
**Level:** Advanced

**Answer:**
Yjs, Automerge — merge concurrent edits.

### Q568. WebSocket at scale?
**Level:** Advanced

**Answer:**
Connection registry in Redis; pub/sub for cross-server messaging.

### Q569. MQTT for IoT scale?
**Level:** Advanced

**Answer:**
Lightweight pub/sub; millions of device connections.

### Q570. TimescaleDB?
**Level:** Advanced

**Answer:**
Postgres extension for time-series.

### Q571. Prometheus architecture?
**Level:** Advanced

**Answer:**
Pull metrics; TSDB storage; PromQL; Alertmanager.

### Q572. OpenTelemetry?
**Level:** Advanced

**Answer:**
Unified metrics, logs, traces instrumentation.

### Q573. eBPF observability?
**Level:** Advanced

**Answer:**
Kernel-level tracing without app instrumentation.

### Q574. Service level objectives example?
**Level:** Intermediate

**Answer:**
99.9% of requests < 200ms over 30 days.

### Q575. Capacity denial of wallet?
**Level:** Advanced

**Answer:**
Auto-scale without budget caps causes huge bills — set limits.

### Q576. Architecture review checklist?
**Level:** Advanced

**Answer:**
Scale, failure modes, security, cost, operability, data flow.

### Q577. Build vs buy?
**Level:** Intermediate

**Answer:**
Buy for commodity (email, payments); build for core differentiator.

### Q578. Vendor lock-in mitigation?
**Level:** Advanced

**Answer:**
Abstraction layers, open formats, multi-cloud where justified.

### Q579. Legacy modernization?
**Level:** Advanced

**Answer:**
Strangler fig, API wrapper, incremental migration.

### Q580. Mainframe integration?
**Level:** Advanced

**Answer:**
MQ/message bridge between modern and legacy.

### Q581. EAI/ESB?
**Level:** Advanced

**Answer:**
Enterprise Service Bus — centralized integration (older pattern).

### Q582. iPaaS?
**Level:** Advanced

**Answer:**
Integration Platform as a Service — Zapier, MuleSoft.

### Q583. Webhook vs polling integration?
**Level:** Intermediate

**Answer:**
Webhook: push on event. Polling: pull periodically.

### Q584. ETL pipeline orchestration?
**Level:** Advanced

**Answer:**
Airflow, Dagster — DAG of data jobs.

### Q585. Data quality checks?
**Level:** Advanced

**Answer:**
Great Expectations — validate schema and values.

### Q586. Lineage tracking?
**Level:** Advanced

**Answer:**
Track data origin and transformations — Apache Atlas.

### Q587. Privacy by design?
**Level:** Advanced

**Answer:**
Minimize data, encrypt, purpose limitation from start.

### Q588. Threat modeling STRIDE?
**Level:** Advanced

**Answer:**
Spoofing, Tampering, Repudiation, Info disclosure, DoS, Elevation.

### Q589. Attack surface reduction?
**Level:** Advanced

**Answer:**
Fewer public endpoints, network segmentation.

### Q590. Immutable infrastructure?
**Level:** Advanced

**Answer:**
Replace servers not patch — cattle not pets.

### Q591. Cattle vs pets?
**Level:** Intermediate

**Answer:**
Pets: unique, nursed. Cattle: disposable, identical.

### Q592. Disaster recovery tiers?
**Level:** Advanced

**Answer:**
Tier 1: hours RTO. Tier 4: minutes; fully redundant.

### Q593. Backup 3-2-1 rule?
**Level:** Intermediate

**Answer:**
3 copies, 2 media types, 1 offsite.

### Q594. Ransomware protection?
**Level:** Advanced

**Answer:**
Immutable backups, air gap, restore drills.

### Q595. Business continuity planning?
**Level:** Advanced

**Answer:**
Identify critical systems; define RPO/RTO per tier.

### Q596. Chaos engineering tools?
**Level:** Advanced

**Answer:**
Chaos Monkey, Gremlin, Litmus.

### Q597. Game days?
**Level:** Advanced

**Answer:**
Scheduled disaster simulation exercises.

### Q598. Progressive delivery?
**Level:** Advanced

**Answer:**
Canary + feature flags + automated rollback.

### Q599. Flag-driven rollback?
**Level:** Advanced

**Answer:**
Disable feature flag instead of redeploy on incident.

### Q600. Dark launches?
**Level:** Advanced

**Answer:**
Deploy code disabled by flag; test in production safely.

### Q601. Shadow traffic?
**Level:** Advanced

**Answer:**
Mirror prod traffic to new system without affecting users.

### Q602. Load test in production?
**Level:** Advanced

**Answer:**
Synthetic traffic or tiny % real — careful!

### Q603. Capacity benchmarking?
**Level:** Advanced

**Answer:**
Find RPS per core; extrapolate with overhead.

### Q604. Power of two choices?
**Level:** Advanced

**Answer:**
Pick least loaded of two random servers — near-optimal LB.

### Q605. Join-absorb-stabilize?
**Level:** Advanced

**Answer:**
New node joins hash ring; absorbs keys; stabilizes.

### Q606. SLA 99.999 (five nines)?
**Level:** Advanced

**Answer:**
5.26 minutes downtime/year — very expensive.

### Q607. HLD complete checklist?
**Level:** Basic

**Answer:**
Reqs, estimates, API, schema, diagram, deep dives, failure, monitor, security, cost.
