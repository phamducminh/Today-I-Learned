# Singleton Design Pattern

## Naive Implementation

```java
public class Singleton {
    
    // private static instance
    private static Singleton sInstance;

    // private constructor for prevent outside initialization
    private Singleton() {
    }

    // static getter
    public static Singleton getInstance() {
        if (sInstance == null) {
            sInstance = new Singleton();
        }
        return sInstance;
    }
}
```

## And Multithread World Comes

_When your application runs multithread, your code can be paused at any line within any time. So maybe two threads can run into two initialization blocks_

How to avoid:

### Double Check Locking Design Pattern

```java
public static Singleton getInstance() {
    if (sInstance == null) {
        // block so other threads cannot come into while initialize
        synchronized (Singleton.class) {
            // recheck again. maybe another thread has initialized before
            if (sInstance == null) {
                sInstance = new Singleton();
            }
        }
    }
    return sInstance;
}
```

You know what **above code won’t work for Java**. I'm not jokking, many of you (including me) have implemented like that over, and over, and over again.

Here is why, please read those link below carefully:
* [The "Double-Checked Locking is Broken" Declaration](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html)
* [Double-checked locking](https://en.wikipedia.org/wiki/Double-checked_locking)

How to fix: simply use **volatile** keyword. And here is a true version of Double Check Locking pattern:

```java
public class Singleton {

    // adding volatile keyword here
    private static volatile Singleton sInstance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        // using local variable for storing volatile object
        // for increasing performance
        Singleton result = sInstance;
        if (result == null) {
            synchronized (Singleton.class) {
                result = sInstance;
                if (result == null) {
                    sInstance = result =  new Singleton();
                }
            }
        }
        return result;
    }
}
```

### Static Block Initialization (Class Loader)

```java
public class Singleton {

    private Singleton() {
    }

    // create a static nested class
    public static class Helper {
        public static Singleton searchBox = new Singleton();
    }

    public static Singleton getsInstance() {
        return Helper.searchBox;
    }

}
```

Java ensures thread-safe property for static class loader. So when one thread is initialized object using class Helper, no other threads can come and create another object. Above method also ensures lazy initialization property.

### Enum

```java
public enum Singleton {
    
    // and just call Singleton.INSTANCE
    // no private constructor. no getInstance
    INSTANCE;
```

> This approach is functionally equivalent to the public field approach, except that it is more concise, provides the serialization machinery for free, and provides an ironclad guarantee against multiple instantiation, even in the face of sophisticated serialization or reflection attacks. While this approach has yet to be widely adopted, **a single-element enum type is the best way to implement a singleton.**

#### Serialization problem:
Reference about this problem in the link [at the end of this note](singleton-pattern.md#Inspiration). I won't tell it here for brevity.

---
## Inspiration

Inspired by [Huỳnh Quang Thảo](https://blog.androidcafe.in/@huynhquangthao) and his article about Singleton Design Pattern on Medium. You can check it [here](https://blog.androidcafe.in/singleton-design-pattern-2c63dfcfccf2). 