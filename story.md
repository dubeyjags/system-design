## App System
Sab kuch simple hai:  `Users → Server → Database`

### 1️⃣ Monolith hai.
`Ek codebase > Ek server > Ek DB  `  
Is stage par kya zarurat hoti hai?
```
✔ Basic authentication (login/signup)
✔ Simple API
✔ Single DB
```
Koi scaling nahi chahiye.

### 2️⃣ Users Badhne Lage

Ab 100 se 10,000 users ho gaye.

Problem: `Server slow > High CPU > Requests delay`

Yaha kaun kaam aata hai?

```
👉 Scale Up
Server RAM/CPU badha do.
```

Lekin ek limit hoti hai.

### 3️⃣ Traffic Spike

Suddenly app viral ho gayi.

Problem: `Server crash > Downtime`

Solution:

👉 Scale Out (Horizontal Scaling)

`Users → Load Balancer → Server1
                       → Server2
                       → Server3
`  
Yaha kaam aata hai:
```
Load Balancer
Stateless system
Horizontal scaling
```

### 4️⃣Session Problem

`User Server1 me login karta hai
Agli request Server2 me chali gayi`

User logout 😵

Yaha ka solution:
```
✔ Stateless system
✔ JWT
✔ Redis session store
```
### 5️⃣ Database Slow

Ab sab servers same DB ko hit kar rahe hain.

DB bottleneck ban gaya.

Yaha kaam aata hai:
```
✔ Database Replication
✔ Read Replica
✔ Caching (Redis)
```
```
Write → Primary DB
Read → Replica DB
```

### 6️⃣ Celebrity Problem

Ek famous user ka post viral.

1M users ek hi post open kar rahe.

Problem:
```
Same DB row hit  
Hotspot  
Server overload  
```
Yaha kaam aata hai:
```
✔ Caching
✔ CDN
✔ Counter caching
✔ Consistent hashing
```
### 7️⃣ Thundering Herd
```
Cache expire ho gaya.  
Sab users DB ko hit kar rahe.  
System crash.
```
Solution:
```
✔ TTL Jitter
✔ Mutex lock
✔ Stale While Revalidate
```
### 8️⃣ Microservices Ki Zarurat

App badi ho gayi:

Profile
```
Feed 
Likes
Chat
Notification 
Monolith complex ho gaya.
```
### 9️⃣ Services Kaise Baat Karein?

Direct HTTP call karte hain:

Service A → Service B

Problem:
```
Tight coupling
Slow
Failure cascade
```
Solution:

👉 Event Driven Architecture

`Producer → Queue → Consumer`  
Use:
```
Kafka
RabbitMQ
SQS
```

### 1️⃣0️⃣ Distributed System Problems

Ab multiple nodes hain.  
Network partition ho gaya.  
Problem:
```
👉 Split Brain
2 leaders ban gaye.
```
Solution:
```
✔ Quorum
✔ Leader election
✔ Odd number nodes
✔ etcd / ZooKeeper
```
### 1️⃣1️⃣ Data Scaling

Users millions ho gaye.

`Single DB slow.`

Solution:
```
✔ Sharding
✔ Consistent Hashing
✔ Multiple databases
```

### 1️⃣2️⃣ Multi Region

Ab app global ho gayi.

Problem:
```
Latency
Consistency
```
Yaha ka trade-off:  
Strong consistency vs Availability  
`CAP Theorem apply hota hai.`



| Problem                | Concept                    |
| ---------------------- | -------------------------- |
| Server slow            | Scale up                   |
| Server crash           | Scale out                  |
| DB slow                | Replication                |
| Same data hit          | Caching                    |
| Cache expire overload  | TTL jitter                 |
| Viral user             | Celebrity problem solution |
| Service tight coupling | Queue                      |
| Data huge              | Sharding                   |
| Node partition         | Quorum                     |
| Need reliability       | Leader election            |
| Overload               | Rate limiting              |
| Heavy task             | Async queue                |
| Failed messages        | Dead Letter Queue          |


### System Design ka real matlab:
```
Jab system grow karta hai, har stage par naye problems aate hain.
Har problem ka ek specific pattern solution hota hai.
```

Final Wisdom

1️⃣ Pehle simple rakho  
2️⃣ Jab problem aaye tab solution add karo  
3️⃣ Over-engineering mat karo  
4️⃣ Har cheez ka trade-off hota hai  
5️⃣ Microservice tabhi jab zarurat ho  