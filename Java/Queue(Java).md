```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Queue<String> q = new LinkedList<>();

        q.add("First");
        q.add("Second");
        q.add("Third");

        System.out.println(q.poll()); // First (removes and returns)
        System.out.println(q.peek()); // Second (only looks at first element)
    }
}
```

- `LinkedList` as Queue: FIFO behavior.
- `PriorityQueue`: Elements are ordered by natural order or comparator.

