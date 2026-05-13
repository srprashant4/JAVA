# HashMap Internal Working in Java
- HashMap is a widely used data structure in Java that implements the Map interface. It stores key-value pairs and allows for fast retrieval of values based on their corresponding keys. The internal working of HashMap involves several components, including hashing, buckets, and collision resolution.
- HashMap falls under the map interface and is part of the Java Collections Framework. It is not synchronized, which means it is not thread-safe. However, it provides a high-performance implementation for single-threaded applications. Map does not fall under the collections interface, but it is a part of the Java Collections Framework as it provides a way to store and manipulate key-value pairs.
- HashMap internally uses an array of buckets. Each bucket stores nodes containing key-value pairs.

    When a key-value pair is inserted:
        - hashCode() is calculated for the key
        - Hashing logic determines the bucket index
        - If bucket is empty, entry is inserted directly
        - If bucket already contains entries, collisions are handled using LinkedList (or Red-Black Tree in Java 8+ after a threshold)
        - equals() is used to check whether the key already exists
        - If key exists → value is updated
        - Else → new node is added

Average time complexity is O(1), though worst case can degrade during heavy collisions.

## Hashing
- When a key-value pair is added to a HashMap, the key is processed through a hash function to generate a hash code. This hash code is then used to determine the index of the bucket where the key-value pair will be stored. The hash function is designed to distribute keys uniformly across the buckets to minimize collisions.

```java
int hashCode = key.hashCode();
int bucketIndex = hashCode % bucketArray.length;
```

## Buckets
- A HashMap uses an array of buckets to store key-value pairs. Each bucket can hold multiple key-value pairs in case of collisions. When a key-value pair is added to a bucket, it is stored as a linked list or a tree structure (in Java 8 and later) to handle collisions efficiently.

## Collision Resolution
- Collisions occur when two different keys produce the same hash code and are assigned to the same bucket. To resolve collisions, HashMap uses chaining, where each bucket can store multiple key-value pairs. In Java 8 and later, if the number of key-value pairs in a bucket exceeds a certain threshold, the linked list is converted into a balanced tree (Red-Black Tree) to improve performance.

```java
if (bucket.size() > TREEIFY_THRESHOLD) {
    // Convert linked list to tree
    bucket.treeify();
}
```

## Performance
- The performance of HashMap is generally O(1) for get and put operations, assuming a good hash function that distributes keys uniformly. However, in the worst case (when many keys collide), the performance can degrade to O(n) due to the need to traverse the linked list or tree in the bucket.
- To mitigate this, it is important to choose a good hash function and ensure that the load factor (the number of key-value pairs per bucket) is kept low by resizing the HashMap when necessary.

```java
if (size > loadFactor * bucketArray.length) {
    resize();
}
```

## Conclusion
- HashMap is a powerful and efficient data structure for storing key-value pairs in Java. Understanding its internal working, including hashing, buckets, and collision resolution, can help developers use it effectively and optimize performance in their applications.