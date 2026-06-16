# Spring Boot Senior Technical Interview Framework

---

## 📋 Table of Contents
- [1.0. Rest API](#10-rest-api)
  - [Q1.1. What is the types of HTTP Methods in Spring?](#q11-what-is-the-type-of-http-methods-in-spring)
  - [Q1.2. What does idempotent mean?](#q12-what-does-idempotent-mean)
  - [Q1.3. Http Method which is not idempotent?](#q13-http-method-which-is-not-idempotent)
  - [Q1.4. @RestController vs @Controller?](#q14-restcontroller-vs-controller)
- [2.0. @Transactional](#20-transactional)
  - [Q2.1. What is the purpose of @Transactional?](#q21-what-is-the-purpose-of-transactional)
  - [Q2.2. How @Transactional works under the hood?](#q22-how-transactional-works-under-the-hood)
  - [Q2.3. Where @Transactional Should Live?](#q23-where-transactional-should-live)
- [3.0. Hibernate](#30-hibernate)
  - [Q3.1. What is an ORM?](#q31-what-is-an-orm)
  - [Q3.2. What are the different types of mappings in Hibernate?](#q32-what-are-the-different-types-of-mappings-in-hibernate)
  - [Q3.3. What is N + 1 Queries](#q33-what-is-n-1-queries)
  - [Q3.4. How to avoid N + 1 Queries](#q34-how-to-avoid-n-1-queries)
---

## 1.0. Rest API

### Q1.1. What is the types of HTTP Methods in Spring?
1. GetMapping
2. PostMapping
3. PutMapping
4. DeletMapping
```java
@RestController
public StudentController {

// 1.
@PostMapping

// 2.
@GetMapping

// 3.
@PutMapping

//4.
@DeleteMapping
}
```

### Q1.2. What does idempotent mean?
#### Target Answer
- API design property where making the same request multiple times produces the exact same server state and outcome as making it a single time

### Q1.3. Http Method which is not idempotent?
#### Target Answer
- PostMapping
- PatchMapping

### Q1.4. @RestController vs @Controller?
#### Target Answer
| Feature | ```@Controller ``` | ```@RestController``` |
| :--- | :--- | :---: |
| **`Composition`**| Standard @Component specialization | A Combination of @Controller + @ResponseBody |
| **`Data Serialization`**| Requires manually adding @ResponseBody to specific methods to send direct data | Automatic, every method implicitly includes @ResponseBody |

- **`@ResponseBody`**: Convert the return value of method into a **JSON** or **XML** response

---

## 2.0. @Trasactional

### Q2.1. What is the purpose of @Transactional?
### Target Answer
- Transaction os a logical unit of work.
- **``Single Logical Unit & Rollback``**: A transaction treats multiple database steps as a single all-or-nothing job; if even one step fails, a rollback instantly undoes everything to keep your data safe.
- **``ACID``**: To guarantee this level of reliability, transactions are strictly governed by ACID properties: Atomicity (all-or-nothing execution), Consistency (ensuring the system stays valid), Isolation (preventing concurrent tasks from interfering with each other), and Durability (ensuring committed data survives system crashes).

### Q2.2. How @Transactional works under the hood?
### Targer Answer
- **``Core Concept``**:
    - Use the Proxy Design Pattern with Spring AOP
    - Defines a transactional boundary by wrapping service class in dynamic proxy object
- **``Under-the-hood``**: external client invokes service hit the proxy wrapper first
    - **``Interception & Start``**: proxy intercepts the call, borrows a DB connection and start transaction ``` connection.setAutoCommit(false) ```
    - **``Delegation``**: Proxy forward the call to your real method to execute business logic
    - **``Commit or rollback``**: If method successful proxy call ``` commit() ```. If runtime exception, proxy catches it and tiggers a ``` rollback ```
- **``Golden Rule``**:
    - Spring proxies can only intercept public methods.

### Q2.3. Where @Transactional Should Live?
#### Target Answer
- On service layer method that define a business operation

---
## 3.0. Hibernate

### Q3.1. What is an ORM?
#### Target Answer

### Q3.2. What are the different types of mappings in Hibernate?
#### Target Answer

### Q3.3. What is N + 1 Queries?
#### Target Answer

### Q3.4. How to avoid N + 1 Queries
#### Targer Answer


## 🎯 Candidate Final Tally Matrix
| Question | Answer | Status ✅ ❌ | Mark |
| :--- | :--- | :---: | :---: 
| **Q1.1** | |  |  |
| **Q1.2** | |  |  |
| **Q1.3** | |  |  |
| **Q1.4** | |  |  |
| **Q2.1** | |  |  |
| **Q2.2** | |  |  |
| **Q2.3** | |  |  |
| **Q3.1** | |  |  |
| **Q3.2** | |  |  |
| **Q3.3** | |  |  |
| **Q3.4** | |  |  |