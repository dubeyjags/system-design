# System Design = Bade software system ko scalable aur fast banane ka plan.

### DNS Resolution = Domain ko IP address me convert karna  
`google.com → 142.250.xxx.xxx`  
### Scale Up (Vertical Scaling): Server ki power increase karna (RAM, CPU, Storage badhana).  
Server ko powerful banana:
```
8GB RAM → 32GB RAM
2 CPU → 8 CPU
```
### Scale Out (Horizontal Scaling): Load handle karne ke liye multiple servers add karna.
Servers badhana:
```\
User → Server1
User → Server2
User → Server3
```

### Consistency: System me har server par data same hona.  
Data har server par same hona chahiye  

### Pre-Warming: Expected traffic ke liye servers ya cache ko pehle se ready karna.
Server ya cache ko pehle se ready karna.
```
Sale start hone wali hai:
Traffic aayega.
To pehle se servers start kar do.
```

### Bottleneck: System ka slowest component jo performance limit karta hai.
`User → Fast Server → Slow Database`

### Load Balancer: Incoming traffic ko multiple servers me distribute karne wala system.
Users ko different servers me distribute karta hai.  
`Speed fast = Crash kam`
```
User1 → Server1
User2 → Server2
User3 → Server3
```
### Stateless System: Server user ka session ya data store nahi karta; har request independent hoti hai.
Server user ka data store nahi karta.
`Easy scaling`
Example:

```
Login info → Database me stored
Server kuch yaad nahi rakhta.
```

### Stateful System: Server user ka session ya state store karta hai.

Server user ka data ya session store karta hai.
```
User login → Session Server1 me save
```
### Availability: System kitna time accessible aur working rehta hai (uptime).
System kitna time chalta hai.

```
99.9% uptime
Bahut kam downtime.
```

### Resource PlanningResource Planning: Expected traffic ke hisab se servers aur resources ka estimation.

Kitne servers chahiye?

```
1000 users → 1 server
1 lakh users → 10 servers
```

### Spike: Traffic ka sudden increase. 

Sudden traffic increase.
```
10 users → 10,000 users
```
### Auto Scaling: Traffic ke hisab se servers automatically increase ya decrease hona. 

Automatic server increase/decrease.
```
100 users → 2 servers
10000 users → 10 servers
```
### Proxy: Client aur internet ke beech ka intermediary server. 

User → Proxy → Internet

`Proxy middle me hota hai.
`
### Reverse Proxy: User requests ko backend servers tak route karne wala server. 
User → Reverse Proxy → Server  
`User → Nginx → Node.js`  
Reverse proxy server ko hide karta hai.

### Trade-off: Ek cheez improve karne ke liye dusri cheez sacrifice karna.
Ek cheez gain karne ke liye dusri sacrifice.
```
Fast system → Expensive
Cheap system → Slow
```
### VPC (Virtual Private Cloud): Cloud me private network jo secure aur isolated hota hai. 
Private network cloud me
```AWS me private network.```
Public internet se hidden.  
Secure.

### Single Point Failure 
Ek cheez fail → pura system band.

### Caching: Frequently used data ko fast access ke liye temporary storage me rakhna.
Data temporary memory me store.
```
Database se 1 sec
Cache se 10 ms
```
### TTL (Time To Live): Cache ya data kitne time tak valid rahega. 
Cache kitne time tak rahega.

### TTL Jitter: Cache expiration time ko random banana taaki server overload na ho.
TTL ko random banana.
```
4.5 min
5.2 min
5.7 min
```

### Probability: Kisi event ke hone ka chance ya likelihood. 
Chance calculation.
```1 million user me```
1000 online = probability.

System planning me use hota hai.

### Mutex: Ek time par ek hi process ko resource access karne ki permission dene ka mechanism.
Ek time par ek hi process.  
`2 log ek account se paisa nikal rahe.`

### Stale While Revalidate: Old cache data turant show karna aur background me fresh data update karna. 
Old cache dikhao + background me update.  
`Cache show karo instantly. Background me update.`

## Thunder Herd Problem
Bahut saare users ya processes ek hi time par ek hi resource ko access karne ki koshish karein → system overload ho jata hai.  
Cache expire → sab users DB hit
### Solution
#### TTL Jitter
Sab cache same time expire na ho.  
```
User1 TTL = 300 sec
User2 TTL = 320 sec
User3 TTL = 280 sec
```

#### Caching + Lock (Mutex)
Ek hi request DB ko hit kare.  
Baaki wait kare.
```
User1 → DB call
User2 → wait
User3 → wait
```

#### Stale While Revalidate
Old cache dikhao.  
Background me update karo.  
User ko fast response milega.

#### Rate Limiting
Ek user ko limited request.   
`1 sec = 10 request max`

#### Queue System
Sab request queue me.
`User → Queue → Server`


atlassian load balancer case styudy
traffic patter netflis/youtube/hotstar
