# senior-spring-boot-question

## 📋 Table of Contents
- [1.0. Stream API](#10-stream-api)
  - [1.1. What is Functional Interface in Java](#11-what-is-functional-interface-in-java)

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
