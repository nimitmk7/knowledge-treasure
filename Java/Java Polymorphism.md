**Polymorphism** literally means “**many forms**.” In Java, it allows a **single entity** (like a method or object) to **behave differently** based on its actual type at runtime.

# Types

| **Type**                  | **Meaning**                                      | **When it Happens**      |
| ------------------------- | ------------------------------------------------ | ------------------------ |
| **Compile-time (Static)** | Method Overloading                               | During compilation       |
| **Runtime (Dynamic)**     | Method Overriding + Upcasting + Dynamic Dispatch | During program execution |

# 1. Compile-time: Method overloading

**Method Overloading** = Same method name but different signatures (different parameters).

```Java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

- Java decides **at compile time** which add() to call based on parameters.

# 2. Runtime Polymorphism: Method Overriding + Dynamic Dispatch

**Method Overriding** = Subclass **provides its own version** of a method defined in the superclass.

```Java
class Animal {
    void makeSound() {
        System.out.println("Some generic sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}
```

## Upcasting + Dynamic Method Dispatch

You can **treat** a subclass object **as a parent type**, but **at runtime**, the **subclass version** of the method will be called.

```Java
public class Main {
    public static void main(String[] args) {
        Animal myAnimal = new Dog(); // Upcasting
        myAnimal.makeSound();        // Output: Bark

        myAnimal = new Cat();
        myAnimal.makeSound();        // Output: Meow
    }
}
```

> Even though `myAnimal` is declared as `Animal`, the actual method that runs is based on the real object (Dog or Cat) at **runtime**.

# Why is Polymorphism important ?

1. Reduces code duplication
2. Makes code **extensible** (easy to add new classes without modifying existing ones)
3. Enables **flexibility**: you can write generic code that works on superclasses or interfaces, but actually runs specific implementations.



