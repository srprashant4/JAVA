# Fail-Fast vs Fail-Safe Iterators in Java

## Overview

Java collections use different strategies when a collection is modified during iteration.

These strategies are commonly known as:

* Fail-Fast
* Fail-Safe

---

## Fail-Fast Iterator

A fail-fast iterator immediately throws an exception if it detects that the collection was modified while it was iterating.

Example collections:

* ArrayList
* HashMap
* HashSet

Example:

```java
List<String> names = new ArrayList<>();

names.add("Alice");
names.add("Bob");

for (String name : names) {
    names.remove(name);
}
```

Result:

```text
ConcurrentModificationException
```

### Why?

The iterator is traversing the live collection.

If the collection changes unexpectedly, the iterator fails immediately to prevent incorrect behavior.

### Characteristics

* Iterates over the actual collection.
* Detects unexpected modifications.
* Throws `ConcurrentModificationException`.
* Helps catch programming mistakes early.

---

## Fail-Safe Iterator

A fail-safe iterator works on a snapshot (copy) of the collection instead of the live collection.

Example:

```java
CopyOnWriteArrayList<String> names =
        new CopyOnWriteArrayList<>();

names.add("Alice");
names.add("Bob");
names.add("Charlie");

for (String name : names) {
    if (name.equals("Alice")) {
        names.remove(name);
    }
}
```

No exception is thrown.

---

## How does CopyOnWriteArrayList work?

When an iterator is created, it takes a snapshot of the current data.

Snapshot:

```text
[Alice, Bob, Charlie]
```

If an element is removed:

```java
names.remove("Alice");
```

A new internal array is created:

```text
[Bob, Charlie]
```

The iterator continues reading from the original snapshot:

```text
[Alice, Bob, Charlie]
```

Since the iterator is not reading the live collection, no exception occurs.

---

## Real-world analogy

Imagine taking a photograph of a group of people.

The photograph represents the snapshot used by the iterator.

Even if people leave the group later, the photograph remains unchanged.

The iterator continues reading the photograph instead of looking at the real group.

---

## Trade-offs

### Fail-Fast

Advantages:

* Detects bugs quickly.
* More memory efficient.

Disadvantages:

* Cannot safely modify collections during iteration.

### Fail-Safe

Advantages:

* Safe to iterate while modifying the collection.
* Useful in concurrent environments.

Disadvantages:

* Extra memory is required.
* Modifications create new copies.
* Write operations are expensive.

---

## When to Use CopyOnWriteArrayList

Good for:

* Many readers, few writers
* Event listener lists
* Configuration data
* Read-heavy concurrent applications

Not good for:

* Frequent insertions
* Frequent removals
* Large collections with heavy write traffic

---

## Summary

| Feature                       | Fail-Fast       | Fail-Safe            |
| ----------------------------- | --------------- | -------------------- |
| Iterates over                 | Live collection | Snapshot             |
| Modification during iteration | Exception       | Allowed              |
| Example                       | ArrayList       | CopyOnWriteArrayList |
| Memory usage                  | Lower           | Higher               |
| Write performance             | Better          | Worse                |

### Rule of Thumb

Use normal collections such as `ArrayList` by default.

Use `CopyOnWriteArrayList` only when reads are frequent and modifications are relatively rare.
