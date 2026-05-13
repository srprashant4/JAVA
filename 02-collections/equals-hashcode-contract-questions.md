# equals() and hashCode() Contract Questions
1. What is the contract between equals() and hashCode() in Java?
- The contract states that if two objects are considered equal according to the equals() method, then they must have the same hash code as returned by the hashCode() method. This means that if obj1.equals(obj2) is true, then obj1.hashCode() must be equal to obj2.hashCode(). However, the reverse is not necessarily true; two objects can have the same hash code but not be equal.

2. Why is it important to override both equals() and hashCode() methods when using objects in collections like HashMap or HashSet?
- It is important to override both equals() and hashCode() methods because collections like HashMap and HashSet rely on these methods to determine object equality and to manage the storage of objects. If you only override equals() without overriding hashCode(), you may violate the contract, leading to unexpected behavior when storing objects in these collections. For example, if two objects are considered equal but have different hash codes, they may be stored in different buckets in a HashMap, causing retrieval issues.

3. What happens if you override equals() but not hashCode() in a class that is used as a key in a HashMap?
- If you override equals() but not hashCode(), you may violate the contract between the two methods. This can lead to inconsistent behavior when using the class as a key in a HashMap. For instance, if two objects are considered equal by the equals() method but have different hash codes, they may be stored in different buckets in the HashMap. As a result, when you try to retrieve a value using one of the keys, it may not be found because the HashMap will look in the wrong bucket.

4. What is the size of the map for the below example?

```java
Map<String, Integer> map = new HashMap<>();
map.put("A", 1);
map.put(new String("A"), 2);
System.out.println(map.size());
```

- The size of the map will be 1 even though two different String objects (one in SCP and one in heap) are used as keys. This is because both "A" and new String("A") are considered equal according to the equals() method, and they have the same hash code. Therefore, when the second put operation is executed, it will update the value associated with the key "A" rather than adding a new entry to the map.

5. What is the size of the set for the below example?

```java
class Employee {
    int id;

    Employee(int id) {
        this.id = id;
    }
}

public class Main {
    public static void main(String[] args) {
    Employee e1 = new Employee(1);
    Employee e2 = new Employee(1);

    HashSet<Employee> set = new HashSet<>();

    set.add(e1);
    set.add(e2);

    System.out.println(set.size());
    }
}
```
- The size of the set will be 2 because the Employee class does not override the equals() and hashCode() methods. As a result, the default implementation from the Object class is used, which considers e1 and e2 as different objects (since they are different instances in memory). Therefore, both e1 and e2 will be added to the HashSet, resulting in a size of 2.
- The Object class's default implementation of equals() compares memory addresses, and the default hashCode() generates a hash code based on the object's memory address. Since e1 and e2 are different objects in memory, they are considered unequal, and both are added to the set.
- Also hashCode() will generate different hash codes for e1 and e2, which further confirms that they are treated as distinct objects in the HashSet. This is because the default hashCode() implementation in the Object class typically returns a hash code based on the memory address of the object, and since e1 and e2 are different instances, they will have different memory addresses and thus different hash codes.
- This is why proper overriding of equals() and hashCode() is crucial when using custom objects in collections like HashSet, as it ensures that the collection can correctly identify and manage duplicate entries based on the logical equality of the objects rather than their memory addresses.
```java
    @Override
    public boolean equals(Object o) {
        Employee e = (Employee) o;
        return this.id == e.id;
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
```
- By overriding equals() and hashCode() in the Employee class, we can ensure that two Employee objects with the same id are considered equal and have the same hash code. This would result in a size of 1 for the HashSet when both e1 and e2 are added, as they would be treated as duplicates based on their logical equality.

6. Why does equals() in the above example work differently for String objects compared to Employee objects?
- The equals() method works differently for String objects compared to Employee objects because the String class in Java overrides the equals() method to compare the contents of the strings rather than their memory addresses. In contrast, the Employee class does not override equals(), so it uses the default implementation from the Object class, which compares memory addresses. As a result, two different String objects with the same content are considered equal, while two different Employee objects with the same id are not considered equal unless we override equals() in the Employee class to compare their ids.

7. What will the below print and why?

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);

Integer x = 200;
Integer y = 200;

System.out.println(x == y);
```
- The first print statement will output `true` because Integer values between -128 and 127 are cached by the Java Integer class. When you assign the value 100 to both `a` and `b`, they reference the same cached Integer object, so `a == b` returns true.
- The second print statement will output `false` because Integer values outside the range of -128 to 127 are not cached. When you assign the value 200 to both `x` and `y`, they reference different Integer objects in memory, so `x == y` returns false.
- Wrapper classes like Integer use internal caching for values between -128 and 127. Therefore, autoboxed Integers within this range may point to the same cached object, causing == to return true. Outside this range, separate objects are created, leading to == returning false. This behavior is specific to wrapper classes and does not apply to custom objects like Employee unless we implement caching ourselves.

