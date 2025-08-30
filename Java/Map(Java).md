```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Map<String, Integer> ages = new HashMap<>();

        ages.put("Alice", 25);
        ages.put("Bob", 30);

        System.out.println(ages.get("Alice")); // 25

        for (String key : ages.keySet()) {
            System.out.println(key + " -> " + ages.get(key));
        }
    }
}
```

- HashMap: Fast lookup, no order.
- LinkedHashMap: Maintains insertion order.
- TreeMap: Sorted by keys.

