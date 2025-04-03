C++ supports **five types of inheritance**, allowing you to build hierarchies and reuse code effectively. These are:

1. **Single Inheritance**
2. **Multiple Inheritance**
3. **Multilevel Inheritance**
4. **Hierarchical Inheritance**
5. **Hybrid (Virtual) Inheritance**

---

### **1. Single Inheritance**

- A derived class inherits from only **one** base class.
- This is the most common type of inheritance.

#### **Example**

```cpp
#include <iostream>
using namespace std;

class Vehicle {
public:
    void move() {
        cout << "Vehicle is moving" << endl;
    }
};

class Car : public Vehicle {  // Single inheritance
public:
    void honk() {
        cout << "Car is honking" << endl;
    }
};

int main() {
    Car myCar;
    myCar.move(); // Inherited from Vehicle
    myCar.honk(); // Car's own function
    return 0;
}
```

---

### **2. Multiple Inheritance**

- A derived class inherits from **more than one** base class.
- This can cause **ambiguity issues** if both base classes have methods with the same name.

#### **Example**

```cpp
class Engine {
public:
    void startEngine() {
        cout << "Engine started" << endl;
    }
};

class Wheels {
public:
    void rotateWheels() {
        cout << "Wheels are rotating" << endl;
    }
};

class Car : public Engine, public Wheels {  // Multiple Inheritance
public:
    void drive() {
        cout << "Car is driving" << endl;
    }
};

int main() {
    Car myCar;
    myCar.startEngine();
    myCar.rotateWheels();
    myCar.drive();
    return 0;
}
```

---

### **3. Multilevel Inheritance**

- A derived class acts as a base class for another derived class (like a chain).
- Forms a **hierarchy**.

#### **Example**

```cpp
class Vehicle {
public:
    void move() {
        cout << "Vehicle is moving" << endl;
    }
};

class Car : public Vehicle {
public:
    void honk() {
        cout << "Car is honking" << endl;
    }
};

class SportsCar : public Car {  // Multilevel inheritance
public:
    void turboBoost() {
        cout << "SportsCar is boosting!" << endl;
    }
};

int main() {
    SportsCar myCar;
    myCar.move();      // From Vehicle
    myCar.honk();      // From Car
    myCar.turboBoost(); // From SportsCar
    return 0;
}
```

---

### **4. Hierarchical Inheritance**

- Multiple classes inherit from **the same base class**.
- Useful when multiple derived classes share common functionality.

#### **Example**

```cpp
class Vehicle {
public:
    void move() {
        cout << "Vehicle is moving" << endl;
    }
};

class Car : public Vehicle {
public:
    void honk() {
        cout << "Car is honking" << endl;
    }
};

class Bike : public Vehicle {
public:
    void ringBell() {
        cout << "Bike is ringing bell" << endl;
    }
};

int main() {
    Car myCar;
    myCar.move();  // Inherited from Vehicle
    myCar.honk();

    Bike myBike;
    myBike.move();  // Inherited from Vehicle
    myBike.ringBell();
    
    return 0;
}
```

---

### **5. Hybrid (Virtual) Inheritance**

- A combination of **two or more types** of inheritance.
- Often uses **virtual base classes** to prevent the **diamond problem**.

#### **Diamond Problem**

When a class is derived from two classes that share a common base, it may inherit duplicate members.  
**Solution:** Use `virtual` base classes.

#### **Example**

```cpp
class Vehicle {
public:
    void move() {
        cout << "Vehicle is moving" << endl;
    }
};

class Engine : virtual public Vehicle {};  // Virtual inheritance
class Wheels : virtual public Vehicle {};  // Virtual inheritance

class Car : public Engine, public Wheels {};

int main() {
    Car myCar;
    myCar.move(); // No ambiguity due to virtual inheritance
    return 0;
}
```

---

### **Summary Table**

|Inheritance Type|Description|Example|
|---|---|---|
|**Single**|One base class, one derived class|`class Car : public Vehicle {};`|
|**Multiple**|Inherits from multiple base classes|`class Car : public Engine, public Wheels {};`|
|**Multilevel**|Inheritance chain (grandparent → parent → child)|`class SportsCar : public Car {};`|
|**Hierarchical**|Multiple derived classes from a single base|`class Bike : public Vehicle {};`|
|**Hybrid (Virtual)**|Combination of multiple inheritance types|`class Car : public Engine, public Wheels {};` (with `virtual` base)|

Would you like a deeper dive into **virtual inheritance** or **constructor behavior in inheritance**? 🚀