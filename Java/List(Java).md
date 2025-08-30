### **Ordered collection, allows duplicates**

```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();

        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Mango");

        System.out.println(fruits.get(1)); // Banana
        System.out.println(fruits.size()); // 3

        fruits.remove("Apple");
        System.out.println(fruits); // [Banana, Mango]
    }
}
```

> 🔥 ArrayList is backed by a **dynamic array** (good for random access),
> 🔥 LinkedList is a **doubly linked list** (good for frequent insertion/removal).

