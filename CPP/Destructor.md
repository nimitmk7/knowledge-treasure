A **destructor** in C++ is a special member function that is automatically invoked when an object is destroyed (goes out of scope or is explicitly deleted). Its primary purpose is to release any resources or memory that were acquired during the lifetime of the object, ensuring proper cleanup.

### **Key Characteristics of a Destructor:**

- **Name:** A destructor has the same name as the class but is preceded by a tilde (`~`). For example, the destructor for a class `Car` would be `~Car()`.
- **No return type:** It does not return a value and does not accept any parameters.
- **Called automatically:** When an object goes out of scope (for stack-allocated objects) or when `delete` is called (for dynamically allocated objects), the destructor is invoked.
- **One per class:** There can only be one destructor in a class, and it cannot be overloaded.

### **Why Use Destructors?**

Destructors are mainly used for **resource management**:

- **Memory cleanup:** If the class dynamically allocates memory (using `new`), the destructor ensures that memory is properly released (using `delete`).
- **Closing resources:** If the class manages file handles, network connections, or other resources, the destructor ensures they are closed properly when the object is no longer needed.

### **Example of a Destructor in C++**

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    string brand;

    // Constructor
    Car(string b) {
        brand = b;
        cout << "Car " << brand << " created!" << endl;
    }

    // Destructor
    ~Car() {
        cout << "Car " << brand << " destroyed!" << endl;
    }
};

int main() {
    Car myCar("Toyota");  // Constructor is called
    // Destructor will be called automatically when myCar goes out of scope
    return 0;
}
```

### **Output:**

```
Car Toyota created!
Car Toyota destroyed!
```

### **Important Notes About Destructors:**

1. **Automatic Cleanup:** When `myCar` goes out of scope at the end of the `main` function, the destructor is called automatically, cleaning up any resources (though in this case, no dynamic memory is allocated).
    
2. **Destructors in Inheritance:**
    
    - If a derived class has a destructor, the **base class destructor** is called first (after the derived class's destructor completes).
    - If the base class destructor is **virtual**, it ensures that the derived class destructor is called when deleting an object of a derived class through a base class pointer.

### **Example of Destructor in Inheritance**

```cpp
class Vehicle {
public:
    Vehicle() {
        cout << "Vehicle Created!" << endl;
    }

    virtual ~Vehicle() {  // Virtual Destructor
        cout << "Vehicle Destroyed!" << endl;
    }
};

class Car : public Vehicle {
public:
    Car() {
        cout << "Car Created!" << endl;
    }

    ~Car() {  // Destructor of the derived class
        cout << "Car Destroyed!" << endl;
    }
};

int main() {
    Vehicle* vehicle = new Car();  // Create object of derived class through base pointer
    delete vehicle;  // Calls both Car and Vehicle destructors
    return 0;
}
```

### **Output:**

```
Vehicle Created!
Car Created!
Car Destroyed!
Vehicle Destroyed!
```

In this case, since the destructor in the **Vehicle** class is **virtual**, it ensures that the **Car** destructor is also called when the object is deleted, even though we are using a base class pointer.

---

### **Key Points About Destructors:**

1. **Automatic Call:** Destructor is called automatically when an object goes out of scope or is deleted.
2. **Virtual Destructors:** Make base class destructors virtual to ensure proper cleanup when deleting derived objects through base class pointers.
3. **No Parameters:** A destructor cannot take parameters or return anything.

Would you like to explore **virtual destructors** or **memory management** further?