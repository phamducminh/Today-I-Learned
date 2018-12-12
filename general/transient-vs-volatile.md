# Difference between transient vs volatile variable or modifier in Java

## Transient

### What

Transient keyword is used in serialization process to prevent any variable from being serialized.

### Why

Provides you some control over serialization process and gives you flexibility to exclude some of object properties from serialization process

### When

* `Transient` keyword can only be applied to fields or member variable. Applying it to method or local variable is compilation error.
* Another important point is that you can declare an variable static and transient at same time and java compiler doesn't complain but doing that doesn't make any sense because transient is to instruct "do not save this field" and static variables are not saved anyway during serialization.
* In similar way you can apply transient and final keyword together to a variable compiler will not complain but you will face another problem of reinitializing a final variable during deserialization.
* Transient variable in java is not persisted or saved when an object gets serialized.

---

## Volatile

### What

The `volatile` keyword in Java is used as an indicator to Java compiler and Thread that do not cache value of this variable and always read it from [main memory](https://javarevisited.blogspot.com/2011/05/java-heap-space-memory-size-jvm.html).

_From Java 5 along with major changes like Autoboxing, Enum, Generics and Variable arguments , Java introduces some change in Java Memory Model (JMM), Which guarantees visibility of changes made from one thread to another also as **"happens-before"** which solves the problem of memory writes that happen in one thread can "leak through" and be seen by another thread._

### How

1. Cannot be used with method or class and it can only be used with a variable.
2. Volatile keyword also guarantees visibility and ordering, after Java 5 write to any volatile variable happens before any read into the volatile variable
3. Use of volatile keyword also prevents compiler or JVM from the reordering of code or moving away them from synchronization barrier.

### When

### Important points on Volatile keyword in Java

1. The volatile keyword in Java is only application to a variable and using volatile keyword with class and method is illegal.

2. volatile keyword in Java guarantees that value of the volatile variable will always be read from main memory and not from Thread's local cache.

    * _Since volatile keyword in Java only synchronizes the value of one variable between Thread memory and `"main"` memory while synchronized synchronizes the value of all variable between thread memory and `"main"` memory and locks and releases a monitor to boot. Due to this reason synchronized keyword in Java is likely to have more overhead than volatile._

3. In Java reads and writes are atomic for all variables declared using Java volatile keyword (including long and double variables).

4. Using the volatile keyword in Java on variables reduces the risk of memory consistency errors because any write to a volatile variable in Java establishes a happens-before relationship with subsequent reads of that same variable.

5. From Java 5 changes to a volatile variable are always visible to other threads. It means that when a thread reads a volatile variable in Java, it sees not just the latest change to the volatile variable but also the side effects of the code that led up the change.

    * _Also from Java 5 writing into a volatile field has the same memory effect as a monitor release, and reading from a volatile field has the same memory effect as a monitor acquire_

6. Reads and writes are atomic for reference variables are for most primitive variables (all types except long and double) even without the use of volatile keyword in Java.

7. An access to a volatile variable in Java never has a chance to block, since we are only doing a simple read or write, so unlike a synchronized block we will never hold on to any lock or wait for any lock.

8. You can not synchronize on the `null` object but your volatile variable in Java could be `null`.

9. Java volatile keyword doesn't mean atomic, its common misconception that after declaring volatile ++ will be atomic, to make the operation atomic you still need to ensure exclusive access using synchronized method or block in Java.

    * _Synchronized obtains and releases the lock on monitor’s Java volatile keyword doesn't require that => Synchronized method affects performance more than a volatile keyword in Java._

10. If a variable is not shared between multiple threads, you don't need to use volatile keyword with that variable.

11. The volatile keyword in Java is a field modifier while synchronized modifies code blocks and methods.

---

## Difference between transient vs volatile variable or modifier in Java

* `transient` keyword is used along with instance variables to exclude them from serialization process. `volatile` keyword can also be used in variables to indicate compiler and JVM that always read its value from main memory and follow **_happens-before_** relationship on visibility of volatile variable among multiple thread
* `transient` keyword can not be used along with `static` keyword but `volatile` can be used along with `static`.
* `transient` variables are initialized with default value during de-serialization and there assignment or restoration of value has to be handled by application code.

---
## References

* [Transient keyword variable in Java](https://javarevisited.blogspot.com/2011/09/transient-keyword-variable-in-java.html)

* [How Volatile in Java works?](https://javarevisited.blogspot.com/2011/06/volatile-keyword-java-example-tutorial.html)

* [Difference between transient vs volatile variable or modifier in Java](http://www.java67.com/2012/11/difference-between-transient-vs-volatile-modifier-variable-java.html#ixzz5YhgJIR3l)

