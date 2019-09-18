# Factory Method aka "Named Constructor"

## Things that you've seen before

```java
Integer.valueOf("44");
Integer.valueOf(44);
```

more

```java
Executors.newSingleThreadExecutor();
```

even more

```java
Glide.with(contexts)
```

Those are the example of factory methods. It wraps the way we call constructor to create new object.

## Advantages

* ### We can replace this method body without breaking the existing code

* ### Comfortable naming

Which of the following example is more readable?

```java
Executors.newSingleThreadExecutor();

//or

new FinalizableDelegatedExecutorService(new ThreadPoolExecutor(1, 1, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>()));
```

* ### Not restricted to class — can be in a separate file.

You can implement rich constructors if you want. But the class file size explodes. With Factory methods, you can move them elsewhere (refer to Executors.java).

---

## Inspiration

Inspired by [Maciej Najbar](https://medium.com/@MaciejNajbar) and his post about [Factory method — design pattern 🏭](https://medium.com/facademy/factory-method-design-pattern-d71a6f57fa6).