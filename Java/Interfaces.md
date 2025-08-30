An **interface** is like a **contract**: it defines **what** a class can do, but **not how** it does it.

- It **only** contains **method signatures** (declarations) and **constants** (fields that are public static final).
- Classes that **implement** the interface must **provide concrete implementations** of its methods.

# Basic Syntax

```Java
interface InterfaceName {
    // method signatures
    void methodOne();
}
```

A class implements the interface using the implements keyword:

```Java
class ClassName implements InterfaceName {
    public void methodOne() {
        // implementation
    }
}
```

## Example

```Java
interface Animal {
    void eat();
    void sleep();
}

class Dog implements Animal {
    public void eat() {
        System.out.println("Dog eats bones.");
    }

    public void sleep() {
        System.out.println("Dog sleeps in kennel.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();
        d.sleep();
    }
}
```

> [!tip] 
> **All methods** in an interface are **implicitly** public abstract even if you don’t write it.

# Important Points

1. They **can’t have method bodies**(except for default and static methods)
2. **Fields are automatically** public static final.
3. A class can **implement multiple interfaces**.
4. You **must** implement **all** methods unless your class is abstract.

## Multiple Inheritance

Java **does not support multiple inheritance of classes**, but a class **can implement multiple interfaces**:

```Java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    public void fly() {
        System.out.println("Duck flies");
    }

    public void swim() {
        System.out.println("Duck swims");
    }
}
```

# Default and static methods

interfaces can have:
- **Default methods**: Methods with a body.
- **Static methods**: Utility methods belonging to the interface.

```Java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle starting...");
    }

    static void honk() {
        System.out.println("Vehicle honking...");
    }
}

class Car implements Vehicle {
    // inherits start() method
}

public class Main {
    public static void main(String[] args) {
        Car c = new Car();
        c.start();           // default method
        Vehicle.honk();       // static method
    }
}
```

- Default methods in interfaces ==provide a default implementation for a method,==allowing interfaces to evolve without breaking existing implementations.
- When a class implements an interface with a default method, the class has the option to either use the default implementation provided by the interface or override it with its own custom implementation. 

This feature offers flexibility: classes can inherit the default behavior or customize it according to their specific needs.

# Difference with Abstract Class

| **Aspect**           | **Interface**                         | **Abstract Class**                     |
| -------------------- | ------------------------------------- | -------------------------------------- |
| Methods              | Only abstract (except default/static) | Abstract + Concrete methods allowed    |
| Fields               | public static final only              | Any type (private, protected, etc.)    |
| Multiple Inheritance | Can implement multiple interfaces     | Can extend only one abstract class     |
| Constructors         | No                                    | Yes                                    |
| Usage                | Behavior contract                     | Base class with partial implementation |

# Best Practices

- Use interfaces to define **capabilities** (e.g., Runnable, Serializable).
- Prefer **“program to an interface”** rather than a concrete class (for flexibility).
- Keep interfaces focused and small — **Single Responsibility Principle**.
- Don’t overuse default methods unless absolutely needed.

