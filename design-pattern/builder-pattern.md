# Use BUILDERS when faced with many constructors

A good choice when designing classes whose constructors or static factories would have more than a handful of parameters.

## Implementation

```java
public final class Coordinate {
    final int x;
    final int y;

    public static Builder builder() {
        return new Builder();
    }

    private Coordinate(Builder builder) {
        this.x = builder.x;
        this.y = builder.y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }
    
    public static final class Builder {
        int x;
        int y;

        private Builder() {

        }

        public Builder setX(int x) {
            this.x = x;
            return this;
        }

        public Builder setY(int y) {
            this.y = y;
            return this;
        }

        public Coordinate build() {
            return new Coordinate(this);
        }
    }
}
```

**_Calling the builder_**

```java
Coordinate.builder().setX(x).setY(y).build());
```

## Some thoughts

* Using builder pattern will create immutable object, **threadsafe** but **consume memory**.
* Variable x,y of Coordinate should declare final.
* Constructor of Coordinate should have private access.
* No need naming Builder class as `CoordinateBuilder`, only `Builder` is enough meaning.
* Constructor of Build should have private access too, and it can only be initialized through `static Builder builder()` method of Coordinate class.

## How to mutate immutable object which created by Builder

Coming soon...

---

## Inspiration

Inspired by a conversation with [Hieu Rocker](https://github.com/rockerhieu).