**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

## Problem
Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the `Truck` class.

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

Great news, right? But how about the code? 

At present, most of your code is coupled to the `Truck` class. Adding `Ships` into the app would require making changes to the entire codebase.

Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

## Solution
The Factory Method pattern suggests that you replace direct object construction calls (using the `new` operator) with calls to a special _factory_ method. 

Don’t worry: the objects are still created via the `new` operator, but it’s being called from within the factory method. Objects returned by a factory method are often referred to as _products._

![[Pasted image 20250225152713.png | “Subclasses can alter the class of objects being returned by the factory method”]]

However, consider this: now you can override the factory method in a subclass and change the class of products being created by the method.

There’s a slight limitation though: subclasses may return different types of products only if these products have a common base class or interface. Also, the factory method in the base class should have its return type declared as this interface.

![[Pasted image 20250226151104.png | All products must follow the same interface.]]

For example, both `Truck` and `Ship` classes should implement the `Transport` interface, which declares a method called `deliver`. Each class implements this method differently: trucks deliver cargo by land, ships deliver cargo by sea. The factory method in the `RoadLogistics` class returns truck objects, whereas the factory method in the `SeaLogistics` class returns ships.

![[Pasted image 20250226151149.png | As long as all product classes implement a common interface, you can pass their objects to the client code without breaking it.]]

The code that uses the factory method (often called the _client_ code) doesn’t see a difference between the actual products returned by various subclasses. The client treats all the products as abstract `Transport`. The client knows that all transport objects are supposed to have the `deliver` method, but exactly how it works isn’t important to the client.

![[Pasted image 20250226151438.png]]
# Examples

Let's consider a **notification system** where we want to send messages via **Email** or **SMS**.

### **Example: Factory Method in Java**

Let's consider a **notification system** where we want to send messages via **Email** or **SMS**.

#### **Step 1: Define a Common Interface**

```java
// Step 1: Define an interface for Notification
interface Notification {
    void notifyUser();
}
```

#### **Step 2: Create Concrete Implementations**

```java
// Step 2: Concrete class for Email Notification
class EmailNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("Sending an Email Notification...");
    }
}

// Step 2: Concrete class for SMS Notification
class SMSNotification implements Notification {
    @Override
    public void notifyUser() {
        System.out.println("Sending an SMS Notification...");
    }
}
```

#### **Step 3: Create the Factory Method**

```java
// Step 3: Create a Factory Class
class NotificationFactory {
    public static Notification createNotification(String type) {
        if (type.equalsIgnoreCase("EMAIL")) {
            return new EmailNotification();
        } else if (type.equalsIgnoreCase("SMS")) {
            return new SMSNotification();
        }
        return null;
    }
}
```

#### **Step 4: Use the Factory Method**

```java
// Step 4: Client code
public class FactoryMethodExample {
    public static void main(String[] args) {
        Notification notification = NotificationFactory.createNotification("EMAIL");
        notification.notifyUser();  // Output: Sending an Email Notification...
    }
}
```

---

# Applicability
1. Use the Factory Method when ==you don’t know beforehand the exact types and dependencies of the objects== your code should work with.
	1. The Factory Method separates product construction code from the code that actually uses the product. Therefore it’s easier to extend the product construction code independently from the rest of the code.
	2. For example, to add a new product type to the app, you’ll only need to create a new creator subclass and override the factory method in it.
2. Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.
3. Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.


# Implementation

1. Make all products follow same interface. Interface should declare methods that make sense in every product.
2. Add an empty factory method inside the creator class. The return type of the method should match the common  product interface. 
3. In the creator’s code find all references to product constructors. One by one, replace them with calls to the factory method, while extracting the product creation code into the factory method.
	1. At this point, the code of the factory method may look pretty ugly. It may have a large `switch` statement that picks which product class to instantiate. But don’t worry, we’ll fix it soon enough.
4. Now, create a set of creator subclasses for each type of product listed in the factory method. Override the factory method in the subclasses and extract the appropriate bits of construction code from the base method.
5. If there are too many product types and it doesn’t make sense to create subclasses for all of them, you can reuse the control parameter from the base class in subclasses.
6. If, after all of the extractions, the base factory method has become empty, you can make it abstract. If there’s something left, you can make it a default behavior of the method.







