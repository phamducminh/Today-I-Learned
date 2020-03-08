# Serialization and Deserialization

## Note

1. If a parent class has implemented Serializable interface then child class doesn’t need to implement it but vice-versa is not true.

2. Only non-static data members are saved via Serialization process.

3. Static data members and transient data members are not saved via Serialization process. So, if you don’t want to save value of a non-static data member then make it transient.

4. Constructor of object is never called when an object is deserialized.
5. Associated objects must be implementing Serializable interface.

---

## References

* [Serialization and Deserialization in Java with Example](https://www.geeksforgeeks.org/serialization-in-java/)