# Spring Boot Senior Technical Interview Framework

---

## 📋 Table of Contents
- [1.0. Rest API](#10-rest-api)
  - [Q1.1. What is the types of HTTP Methods in Spring?](#q11-what-is-the-type-of-http-methods-in-spring)
  - [Q1.2. What does idempotent mean?](#q12-what-does-idempotent-mean)
  - [Q1.3. Http Method which is not idempotent?](#q13-http-method-which-is-not-idempotent)
  - [Q1.4. @RestController vs @Controller?](#q14-restcontroller-vs-controller)
- [2.0. @Transactional](#20-Transactional)
  - [Q2.1. What is the purpose of @Transactional?](#q21-what-is-the-purpose-of--transactional)
  - [Q2.2. What is Parallelism and what are its core architectural risks?](#q22-what-is-parallelism-and-what-are-its-core-architectural-risks)
  - [Q2.3. What is Asynchronous Programming and how does it prevent blocking?](#q23-what-is-asynchronous-programming-and-how-does-it-prevent-blocking)
- [3.0. Multi-Threading Failures & States](#30-multi-threading-failures--states)
  - [Q3.1. What is a Deadlock?](#q31-what-is-a-deadlock)
  - [Q3.2. What is a Livelock and how does its system impact differ from a Deadlock?](#q32-what-is-a-livelock-and-how-does-its-system-impact-differ-from-a-deadlock)
  - [Q3.3. What is a Race Condition and what concurrency mechanics resolve it?](#q33-what-is-a-race-condition-and-what-concurrency-mechanics-resolve-it)
- [🎯 Candidate Final Tally Matrix](#-candidate-final-tally-matrix)

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



## 🎯 Candidate Final Tally Matrix
| Question | Answer | Status ✅ ❌ | Mark |
| :--- | :--- | :---: | :---: 
| **Q1.1** | |  |  |
| **Q1.2** | |  |  |
| **Q1.3** | |  |  |
| **Q1.4** | |  |  |
| **Q1.5** | |  |  |
| **Q1.6** | |  |  |
| **Q1.7** | |  |  |
| **Q2.1** | |  |  |
| **Q2.2** | |  |  |
| **Q2.3** | |  |  |
| **Q3.1** | |  |  |
| **Q3.4** | |  |  |
| **Q3.3** | |  |  |
