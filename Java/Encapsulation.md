✅ **Encapsulation** = **Hiding internal data** of an object and **exposing only selected operations** to the outside world.

Think of it like:
- Making the variables **private** (hidden).
- Providing **public getters/setters** to access and modify them.

> 🔥 “Data hiding + Controlled access” = **Encapsulation**

| **Step** | **What you do**                           | **Why you do it**                          |
| -------- | ----------------------------------------- | ------------------------------------------ |
| 1        | Make fields private                       | So nobody outside can access them directly |
| 2        | Provide public getter methods             | To allow read access                       |
| 3        | Provide public setter methods             | To allow controlled write access           |
| 4        | Optionally, add validation inside setters | To protect data integrity                  |

```Java
public class Person {
    // Step 1: Make fields private
    private String name;
    private int age;

    // Step 2: Public getter for name
    public String getName() {
        return name;
    }

    // Step 3: Public setter for name
    public void setName(String name) {
        this.name = name;
    }

    // Step 2: Getter for age
    public int getAge() {
        return age;
    }

    // Step 3 & 4: Setter with validation
    public void setAge(int age) {
        if (age > 0) { // Validation: no negative age allowed
            this.age = age;
        } else {
            System.out.println("Invalid age");
        }
    }
}
```

