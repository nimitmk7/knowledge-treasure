**Java Collections Framework** is a set of **classes and interfaces** designed to handle groups of objects like:
- Lists
- Sets
- Maps
- Queues

It **abstracts** away the low-level details of managing arrays, resizing, sorting, searching, etc.

| **Interface** | **Description**            | **Common Implementations**      |
| ------------- | -------------------------- | ------------------------------- |
| **List**      | Ordered, allows duplicates | ArrayList, LinkedList           |
| **Set**       | No duplicates allowed      | HashSet, LinkedHashSet, TreeSet |
| **Queue**     | First-In-First-Out (FIFO)  | LinkedList, PriorityQueue       |
| **Map**       | Key-Value pairs            | HashMap, LinkedHashMap, TreeMap |

# [[List(Java)]]
# [[Set(Java)]]
# [[Map(Java)]]

# [[Queue(Java)]]

# Utilities

| **Utility Method**           | **Purpose**                                |
| ---------------------------- | ------------------------------------------ |
| `Collections.sort()`         | Sort elements (ascending order by default) |
| `Collections.reverse()`      | Reverse the order of elements              |
| `Collections.shuffle()`      | Randomly shuffle elements                  |
| `Collections.binarySearch()` | Fast search in sorted lists                |
| `Collections.max() / min()`  | Find largest or smallest element           |
| `Collections.frequency()`    | Count occurrences of an element            |
| `Collections.copy()`         | Copy elements from one list to another     |
| `Collections.fill()`         | Replace all elements with a value          |

## Sorting

1. Ascending Order:
```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 3, 9, 1);
        Collections.sort(numbers);
        System.out.println(numbers); // [1, 3, 5, 9]
    }
}
```

2. Descending Order:

```Java
Collections.sort(numbers, Collections.reverseOrder());
System.out.println(numbers); // [9, 5, 3, 1]
```

3. Custom sorting(e.g. by string length):

```Java
List<String> words = Arrays.asList("apple", "dog", "banana");
words.sort(Comparator.comparing(String::length));
System.out.println(words); // [dog, apple, banana]
```

## Reversing
```Java
Collections.reverse(numbers);
System.out.println(numbers); // Reversed order
```
## Shuffling

```Java
Collections.shuffle(numbers);
System.out.println(numbers); // Random order each time
```

## Binary Search
```Java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
int index = Collections.binarySearch(nums, 3);
System.out.println(index); // 2 (0-based index)
```

## Finding max and min

```Java
int maxNum = Collections.max(nums);
int minNum = Collections.min(nums);

System.out.println("Max: " + maxNum); // 5
System.out.println("Min: " + minNum); // 1
```

## Frequency of an element

```Java
List<String> colors = Arrays.asList("red", "blue", "red", "green", "blue");
int freq = Collections.frequency(colors, "red");
System.out.println(freq); // 2
```

## Copy elements

```Java
List<String> dest = new ArrayList<>(Arrays.asList("","","","")); // Same size
List<String> source = Arrays.asList("A", "B", "C", "D");

Collections.copy(dest, source);
System.out.println(dest); // [A, B, C, D]
```

## Fill elements
```Java
Collections.fill(dest, "Z");
System.out.println(dest); // [Z, Z, Z, Z]
```

## Contains 

If you **just want to know whether an element exists**, use contains():
```Java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

        if (names.contains("Bob")) {
            System.out.println("Bob found!");
        } else {
            System.out.println("Bob not found.");
        }
    }
}
```

## IndexOf and LastIndexOf

```Java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Bob");

int firstIndex = names.indexOf("Bob");
System.out.println(firstIndex); // 1

int lastIndex = names.lastIndexOf("Bob");
System.out.println(lastIndex); // 3
```

> 🔥 **indexOf()** ➔ first occurrence.
> 🔥 **lastIndexOf()** ➔ last occurrence.


