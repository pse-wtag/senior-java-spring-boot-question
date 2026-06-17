# CICD Senior Technical Interview Framework

---

## 📋 Table of Contents
- [1.0. CICD](#10-cicd)
  - [Q1.1. What is CICD?](#q11-what-is-cicd?)
  - [Q1.2. What is blue-green deployment?](#q12-what-is-handle-distributed-transaction-in-microservices)
  - [Q1.3. Explain CQRS](#q13-explain-cqrs)
  - [Q1.4. What is the outbox pattern?](#q14-what-is-the-outbox-pattern)
  - [Q1.5. Our Spring Boot app is getting slow and timing out under heavy traffic. How would you investigate the root cause, and what strategies would you use to fix it?](#q15-our-spring-boot-app-is-getting-slow-and-timing-out-under-heavy-traffic-how-would-you-investigate-the-root-caus-and-what-strategies-would-you-use-to-fix-it)

---

## 1.0. Microservies

### Q1.1. Give me some Microservice Design Pattern?
#### Target Answer
1. APi Gateway
2. Database per service
3. Saga Pattern
4. Outbox pattern
5. CQRS

### Q1.2. How to handle distributed transaction in microservice?
#### Target Answer
- Use the SAGA pattern
- Use the Outbox pattern

### Q1.3. Explain CQRS (Command Query Responsibility Segregation)
#### Target Answer
- Separated data modification (commands) from data retrievals (Queries)

### Q1.4. What is the outbox pattern?
#### Target Answer
- It is a design pattern used in distributed systems (like microservices) to reliably update a database and send event notifications without losing data.

### Q1.5. Our Spring Boot app is getting slow and timing out under heavy traffic. How would you investigate the root cause, and what strategies would you use to fix it?
#### Target Answer
- **`Investigate first`**: "I wouldn't guess. I’d use APM tools or logs to see if the delay is in the database or the code, and check for blocked threads."
- **`Fix Database Issues`**: "I'd check for missing indexes or the N+1 problem (where one query turns into dozens)."
- **`Optimize Performance`**: "I’d add caching (like Redis) for frequent reads or use asynchronous processing for slow tasks."
- **`Add Safety`**: "I’d implement Circuit Breakers (Resilience4j) so that if a part of the system is slow, it doesn't crash the whole application."