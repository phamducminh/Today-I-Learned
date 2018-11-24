# How do I simplify mode comparison

## Assumption

Suppose you have a list of modes like this

```java
public static final int MODE_A = 0;
public static final int MODE_B = 1;
public static final int MODE_C = 2;
public static final int MODE_D = 3;
public static final int MODE_E = 4;
```

What would you do when you want to compare an integer with each mode, or couple of modes at the same time?

```java
if (mode == MODE_A) {
    // do something
}
```

Easy huh :) How about:

```java
if (mode == MODE_A && mode == MODE_B) {
    // do something
}
```

Still no problem. How about:

```java
if (mode == MODE_A
        && mode == MODE_B
        && mode == MODE_C
        && mode == MODE_D) {
    // do something
}
```

Yikes!!! If you have to repeat this check over and over and over again, then why don't define a utility method for this.

## Solution

```java
boolean equalAny(int mode, int... modes) {
    if (modes == null)
        return false;
    for (int modeItem : modes) {
        if (mode == modeItem) {
            return true;
        }
    }
    return false;
}
```

Here I define an **_equalAny_** method with 2 parameters: a mode to check and an array of modes to compare.

```java
// How to use
if (equalAny(mode, MODE_A)) {}
if (equalAny(mode, MODE_A, MODE_B)) {}
if (equalAny(mode, MODE_A, MODE_B, MODE_C)) {}
```

Quite simple, right :)

**Expend method**:

```java
boolean notEqualAll(int mode, int... modes) {
    return !equalAny(mode, modes);
}
```

I believe you can easily understand function above. Belive me :)

---
# Inspiration

Inspired by a conversation with [Hieu Rocker](https://github.com/rockerhieu).

