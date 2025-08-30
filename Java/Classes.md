# Declaration

A **class** in Java is a blueprint for creating objects. It can contain:

- **Fields (attributes)**: Variables to hold data/state.
- **Methods**: Functions that define behavior.
## Class Declaration
```Java
public class ClassName {
    // fields (attributes)
    // methods (behavior)
}
```

## Defining Attributes
These are variables defined inside the class but outside methods. They can have access modifiers, data types, and default values. 

```Java
public class Car {
    String color;         // instance variable
    int year;             // instance variable
    static int wheels = 4; // static variable shared by all instances
}
```

## Creating Methods

Methods define the behavior of the class. Java methods have:

- A **return type** (`void` if it returns nothing)
- A **method name**
- **Parameters** (optional)
- A **method body**

```Java
public class Car {
    String color;
    int year;

    // Method to display car info
    void displayInfo() {
        System.out.println("Color: " + color + ", Year: " + year);
    }

    // Method with parameters
    void setDetails(String c, int y) {
        color = c;
        year = y;
    }
}
```

# Object creation
To use a class and create an object, you instantiate it with `new`.

``` Java
public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();            // creating object
        myCar.setDetails("Red", 2022);    // calling method
        myCar.displayInfo();              // output: Color: Red, Year: 2022
    }
}
```

# Constructor
A **constructor** is a special method that is **automatically called when an object is created**. Its job is to initialize the object’s state (i.e., set up the values of its attributes).

1. Same name as class
2. No return type, not even `void`.
3. Can be **overloaded** (you can have multiple constructors with different parameter lists).

## Types

### Default

If you don’t define an explicit constructor, Java provides a default one.

```Java
public class Dog {
    String name;

    // default constructor
    Dog() {
        name = "Buddy";
    }
}
```

### Parameterized

```Java
public class Dog {
    String name;

    // parameterized constructor
    Dog(String dogName) {
        name = dogName;
    }
}
```

**Example**:

```Java
public class Main {
    public static void main(String[] args) {
        Dog d1 = new Dog();              // uses default constructor
        Dog d2 = new Dog("Charlie");     // uses parameterized constructor
    }
}
```

## Overloading
You can define multiple constructors with different parameter lists:

```Java
public class Book {
    String title;
    int pages;

    Book() {
        title = "Untitled";
        pages = 0;
    }

    Book(String t) {
        title = t;
        pages = 100;
    }

    Book(String t, int p) {
        title = t;
        pages = p;
    }
}
```
## Call another constructor from one constructor

You can call one constructor from another using this(...) (must be the **first line** in the constructor):

```Java
public class Student {
    String name;
    int age;

    Student() {
        this("Unknown", 0); // calls the other constructor
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

> [!INFO] Best Practices
> 1. Always initialize all required fields inside constructors.
> 2. Favor parameterized constructors for meaningful default values.
> 3. Use this() to reduce duplicate initialization logic.




 













