**Inheritance** allows one class (child/subclass) to **acquire the properties and methods** of another class (parent/superclass).

# Basic Syntax

```Java
class Parent {

    // fields and methods

}

class Child extends Parent {

    // inherits fields and methods from Parent

}
```

## Example

```Java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();  // inherited
        d.bark(); // own method
    }
}
```

# Method overriding

If a subclass **redefines a method** from the parent class, it overrides it:

```Java
class Animal {
    void sound() {
        System.out.println("Some sound");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow");
    }
}
```

- Use `@Override` to ensure you’re actually overriding.

# Super keyword
It is used by the subclass to use the parent’s class method or constructor

```Java
class Animal {
    Animal() {
        System.out.println("Animal constructor");
    }

    void display() {
        System.out.println("Animal display");
    }
}

class Dog extends Animal {
    Dog() {
        super(); // calls Animal's constructor
        System.out.println("Dog constructor");
    }

    void display() {
        super.display(); // calls Animal's display
        System.out.println("Dog display");
    }
}
```

# Access Control

| **Modifier** | **Accessible in subclass?**            |
| ------------ | -------------------------------------- |
| public       | ✅ Yes                                  |
| protected    | ✅ Yes (even if in a different package) |
| _default_    | ✅ Yes (only in same package)           |
| private      | ❌ No                                   |

> [!INFO] Best Pratice 
> - Prefer protected for members that subclasses might need to access.
> - Keep classes loosely coupled. Don’t inherit just for reuse — use interfaces or composition where appropriate.
> - Use inheritance for **“is-a”** relationships, not **“has-a”** (composition is better there).

