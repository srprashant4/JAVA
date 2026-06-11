# ConcurrentModificationException in Java

## What is it?

`ConcurrentModificationException` is an exception that occurs when a collection is modified while it is being iterated in a way that the iterator does not expect.

A common example is removing elements from a list while using a for-each loop.

---

## Example

```java
List<String> names = new ArrayList<>();

names.add("Alice");
names.add("Bob");
names.add("Charlie");

for (String name : names) {
    if (name.equals("Alice")) {
        names.remove(name);
    }
}
```

This code throws:

```text
java.util.ConcurrentModificationException
```

---

## Why does it happen?

The enhanced for-loop (`for-each`) uses an `Iterator` internally.

The above code is roughly equivalent to:

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    String name = iterator.next();

    if (name.equals("Alice")) {
        names.remove(name);
    }
}
```

The iterator expects the collection to remain unchanged while it is traversing it.

When `names.remove(name)` is called, the list changes directly without informing the iterator.

The iterator notices that the collection was modified behind its back and throws a `ConcurrentModificationException`.

---

## How to fix it

Use the iterator's `remove()` method instead:

```java
Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    String name = iterator.next();

    if (name.equals("Alice")) {
        iterator.remove();
    }
}
```

Since the iterator performs the removal itself, it can keep its internal state consistent.

---

## Real-world analogy

Imagine you are reading a book using a bookmark.

* The book is the collection.
* The bookmark is the iterator.

If someone tears out a page while you are reading, your bookmark may no longer point to the correct location.

Similarly, when the collection changes unexpectedly, the iterator can no longer safely continue and throws an exception.

---

## Key Takeaways

* The enhanced for-loop uses an iterator internally.
* Modifying a collection directly during iteration can cause `ConcurrentModificationException`.
* Use `iterator.remove()` when removing elements during iteration.
* The exception is a fail-fast mechanism that helps detect bugs early.
