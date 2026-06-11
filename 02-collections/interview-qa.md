# Why do we need Collections when arrays already exist?
- Collections were introduced to overcome the limitations of arrays. Arrays have fixed size, limited utility methods, and support only indexed data access. Collections provide dynamic resizing, ready-made data structures like List, Set, Queue, and Map, along with powerful APIs for searching, sorting, insertion, deletion, and traversal.

- Different collection implementations are optimized for different use cases. For example, ArrayList provides fast random access, LinkedList supports efficient insertions/deletions, HashSet ensures uniqueness, and Queue supports FIFO operations.

- Collections also improve developer productivity by providing reusable and optimized data structures instead of implementing them manually.

# What are the main interfaces in the Java Collections Framework?
The main interfaces in the Java Collections Framework are:
1. **Collection**: The root interface for most collection types, providing basic methods like add, remove, clear, size, and contains.
2. **List**: An ordered collection that allows duplicate elements and provides positional access. Implementations include ArrayList, LinkedList, and Vector.
3. **Set**: A collection that does not allow duplicate elements. Implementations include HashSet, LinkedHashSet, and TreeSet.
4. **Queue**: A collection used to hold multiple elements prior to processing, typically following FIFO order. Implementations include LinkedList, PriorityQueue, and ArrayDeque.
5. **Map**: An object that maps keys to values, where each key is unique. Implementations include HashMap, LinkedHashMap, TreeMap, and Hashtable.

# What is the difference between Collection and Collections?
- **Collection**: It is the root interface in the Java Collections Framework that represents a group of objects, known as elements. It provides basic methods for adding, removing, and querying elements. It is extended by other interfaces like List, Set, and Queue.
- **Collections**: It is a utility class that consists of static methods that operate on or return collections. It provides methods for sorting, searching, reversing, and performing other operations on collections. It also includes methods for creating synchronized or unmodifiable collections.

# Why is Map not a subtype of Collection?
- Map is part of the Java Collections Framework, but it does not extend the Collection interface because Collection is designed to store individual elements, whereas Map stores data in key-value pairs.
- Due to this fundamental difference, operations and behaviors are different. For example, Map supports methods like put(key, value) and get(key), which are not applicable to standard collections like List or Set. Therefore, Map was designed as a separate hierarchy.

# How does HashSet ensure uniqueness of elements?
- HashSet uses a hash table to store its elements. When an element is added to the HashSet, it calculates the hash code of the element and determines the bucket where it should be stored. If there is already an element in that bucket, it uses the equals() method to check if the new element is equal to the existing one. If they are equal, the new element is not added, ensuring uniqueness. If they are not equal, the new element is added to the bucket, allowing for multiple elements with different hash codes.

# If two objects have same hashCode, are they equal?
- No. Two objects having the same hashCode does not necessarily mean they are equal. Different objects can generate the same hashCode, leading to hash collisions. In such cases, equals() is used to determine actual equality. Therefore, while hashCode is used for efficient storage and retrieval in hash-based collections, it does not guarantee object equality. The equals() method must be implemented correctly to ensure that objects are compared based on their content rather than just their hash code.

# If two objects are equal, do they have the same hashCode?
- Yes. If two objects are considered equal according to the equals() method, they must have the same hashCode. This is a fundamental contract in Java: if obj1.equals(obj2) is true, then obj1.hashCode() must be equal to obj2.hashCode(). However, the reverse is not necessarily true; two objects can have the same hashCode but not be equal according to the equals() method. This is because hashCode is not required to be unique, and different objects can produce the same hash code, leading to hash collisions.

# Why is it important to override both equals() and hashCode() methods?
- It is important to override both equals() and hashCode() methods to maintain the general contract for the hashCode method, which states that equal objects must have equal hash codes. If you override equals() without overriding hashCode(), you may violate this contract, leading to unexpected behavior when your objects are used in hash-based collections like HashSet or HashMap. For example, if two objects are considered equal but have different hash codes, they may be stored in different buckets in a HashSet, allowing duplicates to exist. Therefore, to ensure that your objects behave correctly in collections that rely on hashing, you should always override both methods together.

# What is the difference between List, Set, and Map?
- **List**: An ordered collection that allows duplicate elements. It maintains the insertion order and provides positional access to elements. Examples include ArrayList and LinkedList.
- **Set**: A collection that does not allow duplicate elements. It is useful when you need to ensure uniqueness of elements. Examples include HashSet, LinkedHashSet, and TreeSet. Ordering behavior in Set depends on implementation:
    - HashSet does not maintain any order.
    - LinkedHashSet maintains insertion order.
    - TreeSet maintains sorted order.
- **Map**: An object that maps keys to values, where each key is unique. It is useful when you need to associate values with unique keys. Examples include HashMap, LinkedHashMap, TreeMap, and Hashtable.

# What is the difference between ArrayList and LinkedList?
- **ArrayList**: It is a resizable array implementation of the List interface. It provides fast random access to elements but can be slower for insertions and deletions, especially in the middle of the list, as it requires shifting elements.
- **LinkedList**: It is a doubly-linked list implementation of the List interface. It provides efficient insertions and deletions, especially in the middle of the list, as it does not require shifting elements. However, it can be slower for random access compared to ArrayList.

# What is the difference between HashSet and TreeSet?
- **HashSet**: It is a hash table-based implementation of the Set interface. It does not maintain any order of elements and allows null values. It provides constant-time performance for basic operations like add, remove, and contains.
- **TreeSet**: It is a tree-based implementation of the Set interface. It maintains elements in sorted order (natural ordering or via a Comparator) and does not allow null values. It provides logarithmic time performance for basic operations like add, remove, and contains.

# What is the difference between HashMap and TreeMap?
- **HashMap**: It is a hash table-based implementation of the Map interface. It does not maintain any order of keys and allows one null key and multiple null values. It provides constant-time performance for basic operations like get, put, and remove.
- **TreeMap**: It is a tree-based implementation of the Map interface. It maintains keys in sorted order (natural ordering or via a Comparator) and does not allow null keys. It provides logarithmic time performance for basic operations like get, put, and remove.

# What is the difference between Iterator and ListIterator?
- **Iterator**: It is a simple interface that allows you to traverse a collection in a forward direction. It provides methods like hasNext(), next(), and remove(). It can be used with any collection that implements the Collection interface.
- **ListIterator**: It is an extension of the Iterator interface that allows bidirectional traversal of a list. It provides additional methods like hasPrevious(), previous(), add(), and set(). It can only be used with List implementations.

# What is the difference between Comparable and Comparator?
- **Comparable**: It is an interface that defines a natural ordering for objects of a class. A class that implements Comparable must override the compareTo() method to define the natural ordering. It is used when you want to sort objects based on their natural order.
- **Comparator**: It is an interface that defines a custom ordering for objects of a class. A class that implements Comparator must override the compare() method to define the custom ordering. It is used when you want to sort objects based on a specific attribute or criteria that is different from their natural order.

# When does ConcurrentModificationException occur?
- ConcurrentModificationException occurs when a collection is modified while it is being iterated over using an iterator that does not support concurrent modifications. This typically happens when you try to add, remove, or modify elements in a collection while iterating through it using a for-each loop or an iterator. The fail-fast behavior of the iterator detects the modification and throws the exception to prevent unpredictable behavior.

# What is modification count (modCount) in Java collections?
- Modification count (modCount) is an internal variable used by Java collections to keep track of the number of times a collection has been structurally modified (e.g., adding or removing elements). It is used by fail-fast iterators to detect concurrent modifications. When an iterator is created, it captures the current modCount value. If the collection is modified while the iterator is in use, the modCount value changes, and the iterator detects this change and throws a ConcurrentModificationException to prevent unpredictable behavior.

# What is the difference between fail-fast and fail-safe iterators?
- **Fail-fast iterators**: These iterators throw a ConcurrentModificationException if the underlying collection is modified while iterating. They are not thread-safe and are designed to fail quickly to prevent unpredictable behavior. Examples include iterators of ArrayList and HashMap.
- **Fail-safe iterators**: These iterators do not throw ConcurrentModificationException if the underlying collection is modified while iterating. They are thread-safe and work on a copy of the collection, allowing modifications without affecting the iteration. Examples include iterators of CopyOnWriteArrayList and ConcurrentHashMap.

# When should we use fail-safe iterators?
- Fail-safe iterators should be used in scenarios where you need to allow concurrent modifications to the collection while iterating over it. This is particularly useful in multi-threaded environments where multiple threads may be modifying the collection simultaneously. Fail-safe iterators provide a way to safely iterate over a collection without worrying about ConcurrentModificationException, as they work on a copy of the collection. However, they can have performance implications due to the overhead of copying the collection, so they are best used when read operations are more frequent than write operations.

# What is the difference between synchronized and concurrent collections?
- **Synchronized collections**: These collections are thread-safe by synchronizing access to the collection. They use synchronized methods or blocks to ensure that only one thread can access the collection at a time. Examples include Vector and Hashtable.
- **Concurrent collections**: These collections are designed for concurrent access and provide better performance in multi-threaded environments. They use advanced techniques like lock-free algorithms and fine-grained locking to allow multiple threads to access the collection simultaneously without blocking. Examples include ConcurrentHashMap and CopyOnWriteArrayList.

# What is the difference between ArrayList and Vector?
- **ArrayList**: It is a resizable array implementation of the List interface. It is not synchronized, which means it is not thread-safe. It provides better performance in single-threaded environments due to the lack of synchronization overhead.
- **Vector**: It is a resizable array implementation of the List interface. It is synchronized, which means it is thread-safe. However, it can have performance issues in multi-threaded environments due to the overhead of synchronization. It is considered a legacy class and is generally not recommended for new code.

# What is the difference between HashMap and Hashtable?
- **HashMap**: It is a hash table-based implementation of the Map interface. It is not synchronized, which means it is not thread-safe. It allows one null key and multiple null values. It provides better performance in single-threaded environments due to the lack of synchronization overhead.
- **Hashtable**: It is a hash table-based implementation of the Map interface. It is synchronized, which means it is thread-safe. It does not allow null keys or null values. It is considered a legacy class and is generally not recommended for new code due to its performance issues in multi-threaded environments.

# What is the difference between LinkedHashMap and TreeMap?
- **LinkedHashMap**: It is a hash table-based implementation of the Map interface that maintains the insertion order of keys. It allows one null key and multiple null values. It provides constant-time performance for basic operations like get, put, and remove.
- **TreeMap**: It is a tree-based implementation of the Map interface that maintains keys in sorted order (natural ordering or via a Comparator). It does not allow null keys but allows multiple null values. It provides logarithmic time performance for basic operations like get, put, and remove.

# What is the difference between PriorityQueue and ArrayDeque?
- **PriorityQueue**: It is a queue implementation that orders its elements based on their natural ordering or by a Comparator provided at the time of creation. It does not allow null elements and is not thread-safe. It provides logarithmic time performance for basic operations like add, remove, and peek.
- **ArrayDeque**: It is a resizable array implementation of the Deque interface that allows elements to be added or removed from both ends. It does not allow null elements and is not thread-safe. It provides constant-time performance for basic operations like add, remove, and peek at both ends of the deque.

# What is the difference between HashSet and LinkedHashSet?
- **HashSet**: It is a hash table-based implementation of the Set interface that does not maintain any order of elements. It allows null values and provides constant-time performance for basic operations like add, remove, and contains.
- **LinkedHashSet**: It is a hash table-based implementation of the Set interface that maintains the insertion order of elements. It allows null values and provides constant-time performance for basic operations like add, remove, and contains while maintaining the order of elements.

# What is the difference between TreeSet and LinkedHashSet?
- **TreeSet**: It is a tree-based implementation of the Set interface that maintains elements in sorted order (natural ordering or via a Comparator). It does not allow null values and provides logarithmic time performance for basic operations like add, remove, and contains.
- **LinkedHashSet**: It is a hash table-based implementation of the Set interface that maintains the insertion order of elements. It allows null values and provides constant-time performance for basic operations like add, remove, and contains while maintaining the order of elements. It does not maintain sorted order like TreeSet.

# What is the difference between HashMap and LinkedHashMap?
- **HashMap**: It is a hash table-based implementation of the Map interface that does not maintain any order of keys. It allows one null key and multiple null values. It provides constant-time performance for basic operations like get, put, and remove.
- **LinkedHashMap**: It is a hash table-based implementation of the Map interface that maintains the insertion order of keys. It allows one null key and multiple null values. It provides constant-time performance for basic operations like get, put, and remove while maintaining the order of keys. It does not maintain sorted order like TreeMap.

# What is the difference between HashMap and ConcurrentHashMap?
- **HashMap**: It is a hash table-based implementation of the Map interface that is not synchronized, which means it is not thread-safe. It allows one null key and multiple null values. It provides better performance in single-threaded environments due to the lack of synchronization overhead.
- **ConcurrentHashMap**: It is a hash table-based implementation of the Map interface that is designed for concurrent access. It uses advanced techniques like lock-free algorithms and fine-grained locking to allow multiple threads to access the map simultaneously without blocking. It does not allow null keys or null values and provides better performance in multi-threaded environments compared to HashMap.

# What is the difference between ArrayList and CopyOnWriteArrayList?
- **ArrayList**: It is a resizable array implementation of the List interface that is not synchronized, which means it is not thread-safe. It provides better performance in single-threaded environments due to the lack of synchronization overhead.
- **CopyOnWriteArrayList**: It is a thread-safe variant of ArrayList that creates a new copy of the underlying array for every modification (add, set, remove). It allows concurrent read operations without blocking but can have performance issues in scenarios with frequent modifications due to the overhead of copying the array. It is ideal for scenarios where read operations are more frequent than write operations.

# What is the difference between HashSet and CopyOnWriteArraySet?
- **HashSet**: It is a hash table-based implementation of the Set interface that does not maintain any order of elements. It allows null values and provides constant-time performance for basic operations like add, remove, and contains. It is not thread-safe and can lead to unpredictable behavior if accessed by multiple threads concurrently.
- **CopyOnWriteArraySet**: It is a thread-safe variant of HashSet that creates a new copy of the underlying array for every modification (add, remove). It allows concurrent read operations without blocking but can have performance issues in scenarios with frequent modifications due to the overhead of copying the array. It is ideal for scenarios where read operations are more frequent than write operations and when you need to ensure thread safety without external synchronization.

# What is the difference between HashMap and IdentityHashMap?
- **HashMap**: It is a hash table-based implementation of the Map interface that uses the equals() method to compare keys for equality. It allows one null key and multiple null values. It provides constant-time performance for basic operations like get, put, and remove.
- **IdentityHashMap**: It is a hash table-based implementation of the Map interface that uses the == operator to compare keys for identity rather than equality. It does not allow null keys or null values. It provides constant-time performance for basic operations like get, put, and remove but is not suitable for general-purpose use due to its unique key comparison behavior. It is typically used in scenarios where you need to maintain a mapping based on object identity rather than logical equality.

# What is the difference between HashSet and IdentityHashSet?
- **HashSet**: It is a hash table-based implementation of the Set interface that uses the equals() method to compare elements for equality. It allows null values and provides constant-time performance for basic operations like add, remove, and contains. It is not thread-safe and can lead to unpredictable behavior if accessed by multiple threads concurrently.
- **IdentityHashSet**: It is a hash table-based implementation of the Set interface that uses the == operator to compare elements for identity rather than equality. It does not allow null values and provides constant-time performance for basic operations like add, remove, and contains but is not suitable for general-purpose use due to its unique element comparison behavior. It is typically used in scenarios where you need to maintain a set based on object identity rather than logical equality.
