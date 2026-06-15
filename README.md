# senior-spring-boot-question

## 📋 Table of Contents
- [1.0. Stream API](#10-stream-api)
  - [1.1. What is Functional Interface in Java](#11-what-is-functional-interface-in-java)
- [2.0. Concurrency, Parallelism & Async](#10-Concurrency-Parallelism-Async)

---

## 1.0. Stream API

### 1.1. What is Functional Interface in Java
- It must have exactly one abstract method.
### 1.2. How to add more than one method in Functional Interface
- Can have any number of default methods since they are not abstract and already implemented
```java
@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t);

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }
}
```
### 1.3. Types of Functional Interfaces
1. Function
2. Supplier
3. Consumer
4. Predicate
5. UnaryOperator
6. BinaryOperatorperator
### 1.4. Difference between Supplier vs Consumer
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
### 1.5. Difference between map and flatmap in Streams?
map(): transform each element in the streams.
```java
.map(r ->)
```
flatmap(): flattens nested structures.
```java
.flatmap(r -> )
```
### 1.6. Java Stream Exercises
1. Find the name that appears more than 2 time
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
2. Find all the names that start with the letter 'j/J', Male and the age above 18 and return the name in uppercase
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
## 2.0. Concurrency, Parallelism & Async
### 4.1. Concurrency
- Definition: Concurrency means multiple tasks are in progress at the same time, but they are not executing simultaneously; instead, they take turns so rapidly that it creates the illusion of parallel work.

- Context Switching: This is the underlying process (also called time-slicing) where a single CPU core rotates between tasks by assigning them tiny time slots, pausing them, saving their current state, and instantly moving to the next.

![Concurrent Diagram]()
