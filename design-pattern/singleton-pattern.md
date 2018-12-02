# Singleton Design Pattern

## Implementation

### 1. Eager initialization

* If the program will always need an instance, or if the cost of creating the instance is not too large in terms of time/resources, the programmer can switch to eager initialization, which **always creates an instance when the class is loaded into the JVM.**

```java
public class Singleton {

    // Static member holds only one instance of the Singleton class
    private static Singleton sInstance = new Singleton();

    // Singleton prevents any other class from instantiating
    private Singleton() {
    }

    // static getter
    public static Singleton getInstance() {
        return sInstance;
    }
}
```

* **Static block initialization**

The main advantage of using the static block here is that it supports the options for exception handling for the instantiation of the singleton class.

```java
public class Singleton {

    // Static member holds only one instance of the Singleton class
    private static Singleton sInstance;
    static { 
        try {
            sInstance = new Singleton();
        } catch (IOException e) {
            throw new RuntimeException("An error occurred!", e);
        }
    }
    // Singleton prevents any other class from instantiating
    private Singleton() {
    }

    // static getter
    public static Singleton getInstance() {
        return sInstance;
    }
}
```
### 2. Lazy initialization

```java
public class Singleton {
    
    // private static instance
    private static Singleton sInstance;

    // private constructor for prevent outside initialization
    private Singleton() {
    }

    // static getter
    public static Singleton getInstance() {
        if (null == sInstance) {
            sInstance = new Singleton();
        }
        return sInstance;
    }
}
```

_When your application runs multithread, your code can be paused at any line within any time. So maybe two threads can run into two initialization blocks_

In the multithreading environment to prevent each thread to create another instance of singleton object and thus creating concurrency issue we will need to use locking mechanism. This can be achieved by synchronized keyword.

```java
public static synchronized Singleton getInstance() {
    if (null == sInstance) {
        sInstance = new Singleton();
    }
    return sInstance;
}
```

Every time the getInstance() is called it gives us an additional overhead.

**HOW TO AVOID THIS**

* **Double Check Locking Design Pattern**

```java
public static Singleton getInstance() {
    if (null == sInstance) {
        // block so other threads cannot come into while initialize
        synchronized (Singleton.class) {
            // recheck again. maybe another thread has initialized before
            if (null == sInstance) {
                sInstance = new Singleton();
            }
        }
    }
    return sInstance;
}
```

You know what **above code won’t work for Java**. I'm not jokking, many of you (including me) have implemented like that over, and over, and over again.

Here is why, please read those link below carefully:
- [The "Double-Checked Locking is Broken" Declaration](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html)
- [Double-checked locking](https://en.wikipedia.org/wiki/Double-checked_locking)

**How to fix:** simply use **volatile** keyword. And here is a true version of Double Check Locking pattern:

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

Note the local variable "`result`", which seems unnecessary. The effect of this is that in cases where `sInstance` is already initialized (i.e., most of the time), the volatile field is only accessed once (due to "`return result;`" instead of "`return sInstance;`"), which can improve the method's overall performance by as much as 25 percent.

* **Initialization-on-demand holder idiom**

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

Java ensures thread-safe property for static class loader. So when one thread is initialized object using class Helper, no other threads can come and create another object. 

Above method also ensures lazy initialization property. Static instance field is initialized no earlier than SingletonHolder class is loaded and initialized. SingletonHolder class in its turn is loaded and initialized no earlier than it is first referenced. Finally SingletonHolder class is first referenced no earlier than getSingletonInstance() method is called. And this is exactly what we need.

* **Enum**

```java
public enum Singleton {
    
    // and just call Singleton.INSTANCE
    // no private constructor. no getInstance
    INSTANCE;
```

> This approach is functionally equivalent to the public field approach, except that it is more concise, provides the serialization machinery for free, and provides an ironclad guarantee against multiple instantiation, even in the face of sophisticated serialization or reflection attacks. While this approach has yet to be widely adopted, **a single-element enum type is the best way to implement a singleton.**

## Serialization problem:

Serialization allows storing the object in some data store and re-create it later on. However when we serialize a singleton class and invoke deserialization multiple times, we can end up with multiple objects of the singleton class.

In order to make serialization and singleton work properly, we have to introduce readResolve() method in the singleton class. readResolve() method lets developer control what object should be returned  on deserialization.

```java
// This method is called immediately after an object of this class is deserialized.
protected Object readResolve() {
    // Instead of the object we’re on, return the class variable singleton
    return sInstance;
}
```
 
## Cloning Problem

Although we have taken enough precaution to make the Singleton object work as singleton, Cloning the object can still copy it and result into duplicate object. The clone of the singleton object can be constructed using clone() method of the object. Hence it is advisable to overload clone() method of Object class and throw CloneNotSupportedException exception.

```java
public Object clone() throws CloneNotSupportedException {
   throw new CloneNotSupportedException();
}
```

---
## Inspiration

* Inspired by [Huỳnh Quang Thảo](https://blog.androidcafe.in/@huynhquangthao) and his article about Singleton Design Pattern on Medium. You can check it [here](https://blog.androidcafe.in/singleton-design-pattern-2c63dfcfccf2).

* Also reference [Singleton Design Pattern](https://javarevealed.wordpress.com/tag/singleton-and-serialization/).