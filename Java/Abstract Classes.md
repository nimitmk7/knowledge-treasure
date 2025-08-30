Abstract classes cannot be instantiated on their own, its meant to be inherited by other classes.

It can have: 
1. Abstract methods
2. Concrete methods
3. Fields(Attributes)
4. Constructors

# Basic Syntax

```Java
abstract class ClassName {
    // fields
    // constructors
    // abstract methods (no body)
    // concrete methods (normal methods)
}
```

## Example
```Java
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void makeSound(); // abstract method

    void sleep() { // concrete method
        System.out.println(name + " is sleeping...");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
    }

    @Override
    void makeSound() {
        System.out.println(name + " says: Woof!");
    }
}
```

```Java
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        dog.makeSound(); // Buddy says: Woof!
        dog.sleep();     // Buddy is sleeping...
    }
}
```

> 🔥 **Important**: You **must** override all abstract methods in the first **concrete** subclass.

# Use Case
1. To provide a **common base** for related classes.
2. To **enforce a contract** (like interfaces) but also **provide shared behavior**.
3. To **prevent** someone from creating incomplete objects.

# Rules
- If a class has **even one** abstract method ➔ the class **must** be abstract.
- Abstract classes **can** have constructors (they run when subclass objects are created).
- **Cannot instantiate** abstract classes directly:

```Java
Animal a = new Animal(); // ❌ Error
```

# Partial Implementation

An abstract class can **partially implement** an interface:

```Java
interface Flyable {
    void fly();
}

abstract class Bird implements Flyable {
    void eat() {
        System.out.println("Bird eats seeds.");
    }
    // fly() not implemented yet
}

class Sparrow extends Bird {
    @Override
    public void fly() {
        System.out.println("Sparrow flies high.");
    }
}
```

### **🧠 Quick Mental Model**

- **Interface** = “What can it do?” (pure behavior)
- **Abstract Class** = “What is it + what can it do?” (base behavior + identity)

