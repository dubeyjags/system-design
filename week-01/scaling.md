#### Database Scaling: Database ki capacity badhana taaki zyada data aur traffic handle ho sake (vertical ya horizontal scaling).
#### ASG (Auto Scaling Group): Servers ka group jo traffic ke hisab se automatically increase ya decrease hota hai.
#### Database Replication: Database ki copies multiple servers par maintain karna availability aur performance improve karne ke liye.
```
Primary DB → Replica DB → Replica DB
```
#### Quorum: Minimum number of nodes jo operation ko valid banane ke liye agree karein.
```
3 nodes → 2 agree → success
```
#### Consistent Hashing: Data ko servers me evenly distribute karne ka technique jisme server add/remove hone par minimum data move hota hai.
```
- Cache systems
- Distributed DB
```

#### Cluster: Multiple machines ka group jo ek system ki tarah kaam karta hai.
`Server1 + Server2 + Server3 = Cluster`
#### Leader Election: Distributed system me ek node ko leader choose karne ka process.
```
- Writes handle karta hai
- Coordination karta hai
```
#### Sharding: Database ko chhote parts me divide karke different servers par store karna.
```
- Users 1-1000 → DB1
- Users 1001-2000 → DB2
```
#### Point in Time Recovery: Database ko kisi specific time par restore karna.
`10:00 AM backup restore.`

#### Split Brain Problem: Jab cluster ke nodes ek dusre se disconnect ho jayein aur multiple leaders ban jayein.
```
- Data conflict
- Inconsistent data
```
#### Fencing Token: Unique increasing number jo ensure karta hai ki sirf latest process hi resource access kare.
```
- Old processes blocked.
- Used in distributed locks.
```

#### Majority Token: Operation tab valid hota hai jab majority nodes agree karein.  
`5 nodes → 3 agree → success`  
#### ETCD: Distributed key-value store jo configuration aur leader election manage karta hai.
```
- Kubernetes
- Distributed systems
```
#### Apache ZooKeeper: Distributed coordination system jo configuration, synchronization aur leader election manage karta hai.
#### Round Robin: Load balancing method jisme requests sequentially servers me distribute hoti hain.  
`Server1 → Server2 → Server3 → Server1`

#### Sticky Session: Same user ki requests hamesha same server par jati hain.
`Useful for session-based apps.`
#### AWS CloudWatch: Monitoring service jo servers, logs aur metrics track karta hai.
```
- CPU usage
- Memory
- Errors
```
#### Celebrity Problem: Jab ek popular resource par bahut zyada traffic aata hai aur system overload ho jata hai.
`Famous user profile.`

#### Hash Function: Input ko fixed-size value me convert karne wala function.
`user123 → A45F9D`
- Sharding
- Caching
#### Hotspot Problem: Jab system ka ek part ya data bahut zyada requests receive karta hai.
#### Cross-Shard Join: Jab data multiple shards me ho aur query ko multiple shards se data combine karna pade.
`Slow operation.`
#### Single Region Critical Write: Important writes sirf ek region me karna taaki consistency maintain rahe.
`Payment system.`
