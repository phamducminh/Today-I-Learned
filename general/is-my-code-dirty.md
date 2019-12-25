# Is My Code Dirty?

If you have been through any item in the list below, then your code smell **dirty**

1. Use `Ctrl + C` and `Ctrl + V`

2. The name of class has differ semantically

```Kotlin
class Car {
    calculateTax(car:Car)
}
```

3. Subclass and superclass differ semantically

```Kotlin
// smell dirty
class Bike:Car{}
```

Subclass IS-A Superclass :thumbsup: otherwise :thumbsdown:

4. Function has more than one responsibility

5. Can not substitute an implementation with others

6. Implementing interfaces in classes for feature-specific use cases (IS-A fails), then it is not architected well

7. One class know a lot about each other internal inplementation

8. Hard to change to other 3rd party library

9. Code does not have comments and it is not readable

---
# Inspiration

Inspired by [Gaurav]https://medium.com/@gaurav.khanna) and his opinions about dirty code at [Is my Code dirty?](https://medium.com/@gaurav.khanna/is-my-code-dirty-f476b6cac0d4)