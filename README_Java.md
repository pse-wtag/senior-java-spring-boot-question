# Java Senior Technical Interview Framework

---

## 📋 Table of Contents
- [1.0. Stream API & Functional Programming](#10-stream-api--functional-programming)
  - [Q1.1. What is a Functional Interface in Java?](#q11-what-is-a-functional-interface-in-java)
  - [Q1.2. How do you add more than one method to a Functional Interface?](#q12-how-do-you-add-more-than-one-method-to-a-functional-interface)
  - [Q1.3. What are the core types of Functional Interfaces provided in Java?](#q13-what-are-the-core-types-of-functional-interfaces-provided-in-java)
  - [Q1.4. What is the difference between a Supplier and a Consumer?](#q14-what-is-the-difference-between-a-supplier-and-a-consumer)
  - [Q1.5. What is the difference between map() and flatMap() in Streams?](#q15-what-is-the-difference-between-map-and-flatmap-in-streams)
  - [Q1.6. Stream Exercise 1: Element Frequency Counter](#q16-stream-exercise-1-element-frequency-counter)
  - [Q1.7. Stream Exercise 2: Object Pipeline Filtering & Transformation](#q17-stream-exercise-2-object-pipeline-filtering--transformation)
- [2.0. Concurrency, Parallelism & Async](#20-concurrency-parallelism--async)
  - [Q2.1. What is Concurrency and how does Context Switching work?](#q21-what-is-concurrency-and-how-does-context-switching-work)
  - [Q2.2. What is Parallelism and what are its core architectural risks?](#q22-what-is-parallelism-and-what-are-its-core-architectural-risks)
  - [Q2.3. What is Asynchronous Programming and how does it prevent blocking?](#q23-what-is-asynchronous-programming-and-how-does-it-prevent-blocking)
- [3.0. Multi-Threading Failures & States](#30-multi-threading-failures--states)
  - [Q3.1. What is a Deadlock?](#q31-what-is-a-deadlock)
  - [Q3.2. What is a Livelock and how does its system impact differ from a Deadlock?](#q32-what-is-a-livelock-and-how-does-its-system-impact-differ-from-a-deadlock)
  - [Q3.3. What is a Race Condition and what concurrency mechanics resolve it?](#q33-what-is-a-race-condition-and-what-concurrency-mechanics-resolve-it)
- [🎯 Candidate Final Tally Matrix](#-candidate-final-tally-matrix)

---

## 1.0. Stream API & Functional Programming

### Q1.1. What is a Functional Interface in Java?
#### Target Answer
A Functional Interface is an interface that possesses **exactly one abstract method**. It serves as the formal structural target contract for lambda expressions and method references introduced in Java 8+.

### Q1.2. How do you add more than one method to a Functional Interface?
#### Target Answer
You can include multiple additional methods by defining them as **`default` methods**. Because this option supply a concrete runtime implementation block, do not violate the interface's structural single abstract method constraint. Adding the `@FunctionalInterface` annotation prompts the compiler to enforce this design format.

```java
@FunctionalInterface
public interface Predicate<T> {

    // The single abstract method contract
    boolean test(T t);

    // Default method (permitted because it provides an implementation)
    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }
}
```


### Q1.3. What are the core types of Functional Interfaces provided in Java?
#### Target Answer
1. Function
2. Supplier
3. Consumer
4. Predicate
5. UnaryOperator
6. BinaryOperatorperator

### Q1.4. What is the difference between a Supplier and a Consumer?
#### Target Answer
Supplier: Does not accept any argument but returns a result R.
```java
public interface Consumer<T> {
    void accept(T t);
}
```
Consumer: Accepts an argument T but returns no result void.
```java
public interface Supplier<R> {
    R get();
}
```

### Q1.5. What is the difference between map() and flatMap() in Streams?
#### Targer Answer
- **`map()`**:
  - Used for object transformation
  - 1 to 1
- **`flatmap()`**
  - Used for structural flattening
  - 1 to Many

### Q1.6. Stream Exercise 1: Element Frequency Counter
#### Find the name that appears more than 2 times
#### Target Answer
- Explain Process
  - Hint: Use .filter(Objects::nonNull)
  - Hint: Need to create a Map<String, Long>
  - Hint: Use Collectors.groupBy()
  - Hint: Use Collectors.counting()
- If using for loops
  - Explain imperative (How to do it and what to do) vs declarative programming (what to do) use Stream apis
```java
List<String> names = new ArrayList<>();
void main() {
    var frequencyCounter = addingNames().stream()
            .filter(Objects::nonNull)
            .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

    List<String> namesThatAppearsMoreThanTwoTimes = frequencyCounter.entrySet()
            .stream()
            .filter(mapOfNameAndCount -> mapOfNameAndCount.getValue() > 2)
            .map(Map.Entry::getKey)
            .toList();
    IO.println(frequencyCounter);
    IO.println(namesThatAppearsMoreThanTwoTimes);
}

List<String> addingNames() {
    names.add(null);
    names.add("John");
    names.add("Paul");
    names.add("Sam");
    names.add("Dora");
    names.add("John");
    names.add("Paul");
    names.add("Smith");
    names.add("John");
    names.add("Paul");
  return names;
}
```
### Q1.7. Stream Exercise 2: Object Pipeline Filtering & Transformation
#### Find all the names that start with the letter 'j/J', Male and the age above 18 and return the name in uppercase
#### Target Answer
```java
void main() {
    List<String> namesStartWithLetterJAndIsAbove18AndMale = populateNames().stream()
            .filter(person -> person.name.toLowerCase().startsWith("j"))
            .filter(person -> Gender.MALE.equals(person.gender))
            .filter(person -> person.age > 18)
            .map(Person::uppercaseName)
            .toList();
    IO.println(namesStartWithLetterJAndIsAbove18AndMale);
}

public List<Person> populateNames() {
    return List.of(
            new Person("John", Gender.MALE, 30),
            new Person("jane", Gender.FEMALE, 25),
            new Person("Paul", Gender.MALE, 10),
            new Person("jamile", Gender.MALE, 19),
            new Person("jamie", Gender.MALE, 10),
            new Person("Smith", Gender.OTHER, 30)
    );
}

record Person(String name, Gender gender, Integer age) {
    public Person {
        Objects.requireNonNull(gender);
        if (name.isBlank()) {
            throw new IllegalArgumentException("Name must not be null");
        }
        if (age < 0) {
            throw new IllegalArgumentException("Age must be positive");
        }
    }
    public String uppercaseName() {
        return name().toUpperCase();
    }
}

enum Gender {
  MALE,
  FEMALE,
  OTHER
}
```
---

## 2.0. Concurrency, Parallelism & Async

### Q2.1. What is Concurrency and how does Context Switching work?
#### Target Answer
- Concurrency means multiple tasks are in progress at the same time, but they are not executing simultaneously; instead, they take turns so rapidly that it creates the illusion of parallel work.
- **`Context Switching`**: This is the underlying process (also called time-slicing) where a single CPU core rotates between tasks by assigning them tiny time slots, pausing them, saving their current state, and instantly moving to the next.

### Q2.2. What is Parallelism and what are its core architectural risks?
#### Target Answer
- **`Definition`**: Parallelism is the execution of multiple tasks at the exact same moment across separate CPU cores without any turn-taking, allowing true simultaneous processing.
- **`Requirements & Use Cases`**: It requires multiple physical CPU cores and independent, CPU-bound tasks (e.g., video encoding, machine learning, or image processing) to achieve linear speed gains.
- **`The Challenges`**: Parallelism introduces race conditions when multiple cores modify shared memory at the same time. Resolving this requires complex synchronization primitives (like mutexes or semaphores), which can cause performance bottlenecks like lock contention and introduce subtle, hard-to-debug errors.

### Q2.3. What is Asynchronous Programming and how does it prevent blocking?
#### Target Answer
- Asynchronous programming is a software design, not a hardware feature. It solves one main problem: How can a single thread do multiple things without wasting time sitting idle?
- Instead of freezing (blocking) while waiting for a slow task to finish (like fetching data from a database), the thread drops off the request, leaves a "note" (a callback) on what to do next, and immediately jumps to other work. When the slow task finally finishes, a mechanism called the Event Loop picks up that note and tells the thread to finish the job.

## 3.0. Multi-Threading Failures & States

### Q3.1. What is a Deadlock?
#### Target Answer
- **``Definition``**: A deadlock is a situation in a multi-threaded environment where two or more threads are permanently blocked because each thread is waiting for a resource or lock held by another thread in the cycle.
- **``Impact``**: Because none of the threads can release their resources until they get the ones they are waiting for, the affected parts of the application stall or fail completely.

### Q3.2. What is a Livelock and how does its system impact differ from a Deadlock?
#### Target Answer
- **``Definition``**: Livelock is a concurrency problem similar to a deadlock, but instead of freezing or waiting indefinitely, the involved threads continuously change their states in response to each other without making any actual forward progress.
- **``Impact``**: The threads remain active and trapped in an endless cycle of status updates, completely preventing them from executing or completing their intended tasks.

### Q3.3. What is a Race Condition and what concurrency mechanics resolve it?
#### Target Answer
- **``Definition``**: A race condition occurs when multiple threads attempt to modify a shared resource simultaneously without proper synchronization.
- **``Impact``**: Because threads run independently, their operations can overlap in unpredictable ways, resulting in inconsistent data and unexpected application behavior.
- Use synchronized methood or block to avoid

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
