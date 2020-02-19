# How to use **synchronized** keyword

Suppose you have class Foo and a method foo like this

```java
class Foo() {
    void foo() {

    }
}
```

## Explanation

In all cases lock is taken on current instance of Foo.

1. `synchronized (Foo.this) {...}`

is used when code is written inside another class, can be an anonymous class inside Foo. In order to refer to instance of Foo.

2. `synchronized (Foo.class) {...}`

is used when the functions are static. 

3. `synchronized (this) {...}`

is used inside any member function of Foo. Which points to it's instance.

4. `synchronized void foo() (...)`

is used when entire method need to be synchronized. it is equivalent to

```java
void foo() { 
    synchronized (this) {...}
}
```

5. `static synchronized foo() {...}`

it is equivalent to

```java
static void foo() {
    synchronized (Foo.class) {
        // ...
    }
}
```

## Conclusion

* **Static synchronized functions synchronize on the class while non-static synchronized functions synchronize on the instance.**

* When a thread has entered a synchronized instance method then no other thread can enter any of synchronized instance methods of the same instance

* When a thread entered a synchronized static method then no other thread can enter any of synchronized static methods of the same class
