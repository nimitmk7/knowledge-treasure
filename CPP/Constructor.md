In C++, **constructors** play a crucial role in object-oriented programming, especially in inheritance. Constructors are special member functions that are automatically called when an object is created.

### **Types of Constructors in C++**

1. **Default Constructor**
2. **Parameterized Constructor**
3. **Copy Constructor**
4. **Constructor Overloading**
5. **Constructors in Inheritance**

---

### **1. Default Constructor**

A constructor with no parameters that initializes default values.

```cpp
#include <iostream>
using namespace std;

class Car {
public:
    Car() {
        cout << "Default Constructor Called" << endl;
    }
};

int main() {
    Car myCar;  // Default Constructor automatically called
    return 0;
}
```

---

### **2. Parameterized Constructor**

A constructor that accepts arguments to initialize class members.

```cpp
class Car {
public:
    string brand;
    int year;

    Car(string b, int y) {  // Parameterized Constructor
        brand = b;
        year = y;
    }

    void display() {
        cout << "Brand: " << brand << ", Year: " << year << endl;
    }
};

int main() {
    Car car1("Toyota", 2022);
    car1.display();
    return 0;
}
```

---

### **3. Copy Constructor**

A constructor that creates a new object by copying an existing object.

```cpp
class Car {
public:
    string brand;
    Car(string b) {
        brand = b;
    }

    // Copy Constructor
    Car(const Car &c) {
        brand = c.brand;
        cout << "Copy Constructor Called" << endl;
    }

    void display() {
        cout << "Brand: " << brand << endl;
    }
};

int main() {
    Car car1("Honda");
    Car car2 = car1;  // Copy Constructor Called
    car2.display();
    return 0;
}
```

---

### **4. Constructor Overloading**

C++ allows multiple constructors in the same class with different parameter lists.

```cpp
class Car {
public:
    string brand;
    int year;

    Car() {  // Default Constructor
        brand = "Unknown";
        year = 0;
    }

    Car(string b, int y) {  // Parameterized Constructor
        brand = b;
        year = y;
    }

    void display() {
        cout << "Brand: " << brand << ", Year: " << year << endl;
    }
};

int main() {
    Car car1;
    Car car2("Ford", 2021);

    car1.display();
    car2.display();
    return 0;
}
```

---

### **5. Constructors in Inheritance**

When a derived class is created, **base class constructors are called first**, followed by the derived class constructor.

#### Example

```cpp
class Vehicle {
public:
    Vehicle() {
        cout << "Vehicle Constructor Called" << endl;
    }
};

class Car : public Vehicle {
public:
    Car() {
        cout << "Car Constructor Called" << endl;
    }
};

int main() {
    Car myCar;  // Output: Vehicle Constructor → Car Constructor
    return 0;
}
```

---

### **Order of Constructor Execution**

1. Base class constructor
2. Derived class constructor

---

### **Parameterized Constructors in Inheritance**

Derived classes can call base class constructors using **initializer lists**.

```cpp
class Vehicle {
public:
    Vehicle(string type) {
        cout << type << " Vehicle Created" << endl;
    }
};

class Car : public Vehicle {
public:
    Car(string brand) : Vehicle("Car") {  // Calling Base Class Constructor
        cout << brand << " Car Created" << endl;
    }
};

int main() {
    Car myCar("Toyota");
    return 0;
}
```

---

### **Summary**

|Constructor Type|Description|When Used|
|---|---|---|
|Default|No parameters|Automatic Initialization|
|Parameterized|Accepts arguments|Custom Initialization|
|Copy|Copies one object into another|Cloning objects|
|Overloaded|Multiple constructors|Different Initialization Patterns|
|Inheritance|Calls base class constructors|Parent-Child Relationship|

---

Would you like to know how **destructors** work in C++ or how to use **virtual functions** with constructors?