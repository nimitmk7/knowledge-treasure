
# Creating Objects
In C++, defining classes and creating objects is similar to Java but with some differences in syntax and flexibility. Here’s a breakdown:

### **Class Definition in C++**

A class in C++ is defined using the `class` or `struct` keyword. It contains **data members (fields)** and **member functions (methods)**.

#### **Example: Basic Class Syntax**

```cpp
#include <iostream>
using namespace std;

class Car {
public:  // Access specifier (public allows access from outside)
    string brand;
    int year;

    // Constructor
    Car(string b, int y) {
        brand = b;
        year = y;
    }

    // Member function
    void display() {
        cout << "Brand: " << brand << ", Year: " << year << endl;
    }
};

int main() {
    // Creating objects of the class
    Car car1("Toyota", 2022);
    Car car2("Honda", 2020);

    // Calling a member function
    car1.display();
    car2.display();

    return 0;
}
```

### **Key Differences from Java**

1. **No `class` filename restriction**: Unlike Java, C++ does not require the class name to match the filename.
2. **No `public static void main`**: The `main()` function is standalone in C++.
3. **Constructors**: Defined explicitly, unlike Java, where default constructors are auto-generated.
4. **No `new` required for stack allocation**: Objects can be created directly without using `new`, reducing memory management complexity.

---

### **Creating Objects in C++**

There are two ways to create objects in C++:

1. **Stack Allocation (Automatic Storage)**
    
    ```cpp
    Car car1("BMW", 2021); // No need for `new`
    ```
    
    - Automatically destroyed when the function exits.
    - No need for manual `delete`.
2. **Heap Allocation (Dynamic Memory)**
    
    ```cpp
    Car* car2 = new Car("Audi", 2023);
    car2->display();
    delete car2; // Required to free memory
    ```
    
    - Uses `new` to allocate memory.
    - Requires `delete` to avoid memory leaks.
    - Access members using `->` instead of `.`

# Example

```CPP
#include <iostream>
using namespace std;

class Car {
public:  // Access specifier (public allows access from outside)
    string brand;
    int year;

    // Constructor
    Car(string b, int y) {
        brand = b;
        year = y;
    }

    // Member function
    void display() {
        cout << "Brand: " << brand << ", Year: " << year << endl;
    }
};

int main() {
    // Creating objects of the class
    Car car1("Toyota", 2022);
    Car car2("Honda", 2020);

    // Calling a member function
    car1.display();
    car2.display();

    return 0;
}

```
