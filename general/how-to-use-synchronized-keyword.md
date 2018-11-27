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

2. `synchronized (this) {...}`

is used inside any member function of Foo. Which points to it's instance.

3. `synchronized void foo() (...)`

is used when entire method need to be synchronized. it is equivalent to

```java
void foo() { 
    synchronized (this) {...}
}
```

4. `static synchronized foo() {...}`

it is equivalent to

```java
static void foo() {
    synchronized (Foo.class) {
        // ...
    }
}
```

## Conclusion

**Static synchronized functions synchronize on the class while non-static synchronized functions synchronize on the instance.**