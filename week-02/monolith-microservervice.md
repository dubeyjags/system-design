### Monolith

Ek hi codebase + ek hi deployment + ek hi database
```
Frontend
   ↓
Single Backend
   ↓
Single DB
```

✔ Simple
❌ Scale karna mushkil
❌ Ek bug → pura system down

### Microservice

Application ko chhote independent services me tod dena
```
User Service  
Feed Service  
Like Service  
Chat Service  
```
Apna code  
Apna DB  
Independent deploy

### Twitter-like Service
API Gateway
```
User Service
Profile Service
Feed Service
Like Service
Comment Service
Follow Service
Chat Service
Notification Service
```

### Stateless + Horizontal Scaling

Stateless = Server session store nahi karta.

### ASG (Auto Scaling Group)
traffice increase par server jyada and kam kar sake

### Application Level Joins

Microservice me:

No SQL JOIN across DB.

Instead:

```
Service A fetch
Service B fetch
Merge in code
```

### Single DB vs Multiple DB

Single DB:

✔ Easy
❌ Tight coupling

Multiple DB:

✔ Independent scaling
✔ Isolation
❌ Complex

Microservices → Multiple DB best.

### Sync vs Async Tasks

```
Sync:
Wait for response.
Async:
Fire and forget.
Example:
Send email → async
```

### Simple Queue Service
Producer → Queue → Consumer
```
Used for:
Notifications
Email
Analytics  

```

### Throttle
Limit processing speed.  
Prevent overload.

### Fanout
One event → multiple consumers.  
Example:
```
New tweet:
→ Feed Service
→ Notification
→ Analytics
```
### Dead Letter Queue (DLQ)

Failed messages → DLQ.  
Debug later.  
Prevents infinite retry.


### Rule #53 in Microservice
Never share database between services.  
Each service owns its data.

### PostgreSQL Bytea

In PostgreSQL  
BYTEA datatype:  
Store binary data.  
``` 
Example:
Images, files.
Not recommended for large media.
```

### Queue Infrastructure

Components:
```
Producer
Queue Broker
Consumer
DLQ
Monitoring
```
#### Tools:
- Apache Kafka  
- RabbitMQ  
- Amazon SQS