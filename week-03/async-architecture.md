# the async architecture
## Sync (Synchronous)
> Request aayi → sab kaam ek ke baad ek → response baad me.

## Async (Asynchronous)
> Critical kaam pehle karo, baaki background me.

progarming has 2 terms
sync and async

## Basic Amazon Signup Flow
```
User
 ↓
Load Balancer
 ↓
API Server
```

Steps:
- Rate limiting
- Email/password validation
- Password hashing
- Insert into database
- Send welcome email
- Return auth token

```
User
 ↓
Load Balancer
 ↓
API Server
 ↓
Database
```
## Rate Limiting  
Spam users ko stop karne ke liye.  
`Max 5 signup per minute per IP`

Usually Redis me store hota hai.


## Email Sending Problem

Email bhejna slow ho sakta hai:
- SMTP delay
- network issue
- retry needed
Isliye email ko background task bana dete hain.

Async Without Queue (Bad but Simple)

Architecture:

```
User
 ↓
API Server
 ↓
Send Email Service
```
**Flow:**

`API → Email server → SMTP`

Retry aur rate limiting API machine handle karegi.

Example:
```
API machine
  ↓
Email worker
```

### Problem  
Agar API server crash ho gaya:  
`Email task lost`

### solution 1
```
API
 ↓
Broadcast message
 ↓
Server1
Server2
Server3
```
Multiple workers sunte hain.

But problem:

**Duplicate processing.**  
`3 servers → 3 emails sent`

**Deduplication Solution**

Redis locking.  
`SETNX email:user123`

Agar lock mila → process karo.  
Nahi mila → skip.

### Solution 2
#### Solution 2: Message Broker (Best Practice)

Isko bolte hain Queue Based Architecture.
```
API Server
 ↓
Message Broker
 ↓
Worker Servers
```
```
Signup API
 ↓
Send message to queue
 ↓
Queue stores message
 ↓
Worker picks message
 ↓
Send email
```
**Benefits:**

- Retry
- Acknowledgement
- No data loss

**Typical brokers:**
- RabbitMQ
- Apache Kafka
- Amazon SQS


### Solution 3: Database as Queue

Agar queue nahi hai.

Architecture:
```
API
 ↓
Insert Task into DB
 ↓
Worker Servers
```
Table:

tasks
id
type
status
created_at

Flow:

Worker check karta hai:

SELECT * FROM tasks WHERE status = pending

Phir:

1️⃣ Pickup
2️⃣ Perform
3️⃣ Delete

Problem

Race condition.

2 workers same task le sakte hain.

Better Approach
`Pickup → Mark processing`

Example:

status = processing
Aur Better

Use timestamp.

`processing_at`  
Agar worker crash ho gaya:

Task retry ho jayega.

### complete architecture
```
User
 ↓
CDN
 ↓
Load Balancer
 ↓
API Gateway
 ↓
Signup Service
 ↓
Database
 ↓
Message Queue
 ↓
Email Workers
 ↓
Email Provider
```

| Task              | Type  |
| ----------------- | ----- |
| Signup validation | Sync  |
| Password hashing  | Sync  |
| DB insert         | Sync  |
| Send email        | Async |
| Analytics         | Async |
| Notifications     | Async |


## Key System Design Lessons

1️⃣ Critical tasks sync rakho  
2️⃣ Slow tasks async karo  
3️⃣ Queue use karo  
4️⃣ Retry mechanism rakho  
5️⃣ Idempotent workers banao

## One Sentence Summary

> Modern scalable systems me user request fast respond karti hai, aur heavy work background async workers handle karte hain.


# LeetCode-like coding platform

# First Question in System Design

Sabse pehla question:  

> What are we building?    

Agar yeh clear nahi hai to architecture galat ho jayega.  

**Basic Features**  
LeetCode jaisa system:
- Authentication
- Problem list
- Problem detail
- Code submission
- Execution & judging
- Leaderboard
- Discussion

**Functional Requirements**  
System kya karega?
- Login
- Browse problems
- Open problem
- Submit code
- See result
- Check leaderboard
- Discuss

**Non-Functional Requirements**

`Yeh system quality requirements hain.`  
Important ones:  

**Scale**  
- Millions of users.

**Latency**
- Submission result fast aana chahiye.

**Reliability**  
- System crash nahi hona chahiye.

**Observability**
Error kaha hua?  
Monitoring tools:
- logs
- metrics
- alerts

**CAP Theorem**  
Distributed system me 3 cheeze hoti hain:
| Term                | Meaning                |
| ------------------- | ---------------------- |
| Consistency         | Same data everywhere   |
| Availability        | Always responsive      |
| Partition Tolerance | Network failure handle |

Coding platform usually choose:  
`Availability + Partition Tolerance`

**High Level Architecture**

Basic architecture:
```
Client
 ↓
API Server
 ↓
Queue
 ↓
Execution Workers
 ↓
Database
```
Client request karega, workers code run karenge.

## Client Side Design

Client kya karega?

1. Get problem list
GET /problems
2. Get single problem
GET /problem/{id}
3. Submit code
POST /submission
4. Check result

2 methods possible:

**Polling**
Client baar baar request karta hai.  
`GET /submission/status `   
Every few seconds.  
Simple.

**Server Sent Events (SSE)**  
Server push karta hai result.  
Example:  
`submission completed → send event`  
More efficient.  

**Submission Architecture**

Code submission heavy process hai.
```
Flow:

Client
 ↓
API Server
 ↓
Queue
 ↓
Execution Worker
 ↓
Result Store
 ↓
Client gets result
```
**Queue Ka Role**

Queue important hai.  
Queue store karta hai submission tasks. 

*Example tools:*
- Amazon SQS
- Apache Kafka
- Queue Benefits

✔ Async processing  
✔ Load smoothing  
✔ Retry support  
✔ Worker scaling

**Database Design**  

Database store karega:

Tables:
```
Users
Problems
Submissions
Leaderboard
Comments
```

**Observability**

Production me monitoring important hai.

Track:
- request errors
- queue length
- worker failures
- xecution time
Monitoring tools:
- metrics + logs.


## Latency Optimization

Submission result fast chahiye.

Use:

✔ queue workers
✔ horizontal scaling
✔ caching problems

**Full Architecture**  
Final simplified system:
```
Client
 ↓
CDN
 ↓
Load Balancer
 ↓
API Servers
 ↓
Queue
 ↓
Execution Workers
 ↓
Result Store
 ↓
Database
```

## Heartbeat
A heartbeat is a small signal sent periodically to show that a system/service is alive and healthy ❤️
```
Service A ----heartbeat----> Service B
        every 5 seconds
If Service B doesn't receive heartbeat, it assumes:

Service A is down ❌
```
**Common uses:**

Distributed systems  
Load balancers  
Leader election  
Worker monitoring  \

Example tools using heartbeat:  
Kafka consumer groups  
Kubernetes nodes  
RabbitMQ clusters

## Push-based Message Broker
> In a push-based system, the broker pushes messages to consumers automatically.

## RabbitMQ

> RabbitMQ is a message broker used for asynchronous communication between services.
```
User registers
     |
Producer sends message
     |
RabbitMQ Queue
     |
Email Service sends welcome email
```
## Message Broker

A message broker is a system that transfers messages between services.  
Think of it as a post office for applications 📬  
`Service A → Broker → Queue → Service B`
Examples:  
- RabbitMQ  
- Kafka  
- AWS SQS  
- BullMQ (built on Redis)


## BullMQ
> BullMQ is a Redis-based job queue for Node.js.  
> Very popular in Node.js backend systems.

**Common uses:**  
Background jobs  
Email sending  
Video processing  
Notifications  

**Why developers like it:**  
✔ Works well with Node.js  
✔ Uses Redis  
✔ Retry support  
✔ Delayed jobs  
✔ Job scheduling  

## *Is Queue is FIFO*
>Yes, most queues are FIFO.

> But some systems may not strictly guarantee FIFO:

**Example:**
Kafka → partition-level ordering  
RabbitMQ → FIFO per queue  
SQS → FIFO only if FIFO queue used  

## Is Kafka Push or Pull?
> Kafka is Pull-based.  
> Consumers pull messages from Kafka.


## SQS vs Kafka vs BullMQ
> This is a very common system design interview question.

| Feature  | SQS           | Kafka           | BullMQ          |
| -------- | ------------- | --------------- | --------------- |
| Type     | Cloud Queue   | Event Streaming | Job Queue       |
| Scale    | Medium        | Massive         | Small–Medium    |
| Managed  | Yes           | No (unless MSK) | No              |
| Use Case | Microservices | Data pipelines  | Background jobs |
| Tech     | AWS           | JVM ecosystem   | Node.js         |


## Greenfield Project

A Greenfield project means:

Building a system from scratch.
# Interview Questions

## When would you use Kafka instead of RabbitMQ?
**Use RabbitMQ for:**
- Task queues
- Request processing
- Background jobs

**Use Kafka for:**
- Event streaming
- Large scale data pipelines
- Analytics systems







