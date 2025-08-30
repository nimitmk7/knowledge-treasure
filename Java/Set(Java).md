```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Set<String> countries = new HashSet<>();

        countries.add("USA");
        countries.add("India");
        countries.add("USA"); // duplicate, ignored

        System.out.println(countries); // [USA, India] (order not guaranteed)
    }
}
```

- HashSet: Fast, unordered.
- LinkedHashSet: Keeps insertion order.
- TreeSet: Sorted order (uses Red-Black Tree).