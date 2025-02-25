In software development, we often require classes that can only have **one object**. For example: thread pools, caches, loggers etc.

Creating more than one objects of these could lead to issues such as incorrect program behavior, overuse of resources, or inconsistent results.

This is where **Singleton Design Pattern** comes into play.

**Singleton** is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

## Problem
The Singleton pattern solves two problems at the same time, violating the _Single Responsibility Principle_(S in SOLID):

1. **Ensure the class has just a single instance**. 
	1.  Why would anyone want to control how many instances a class has? ==The most common reason for this is to control access to some shared resource==—for example, a database or a file.
	2. Here’s how it works: imagine that you created an object, but after a while decided to create a new one. ==Instead of receiving a fresh object, you’ll get the one you already created==.
	3. Note that this behavior is impossible to implement with a regular constructor since a constructor call **must** always return a new object by design.
	4. ![[Pasted image 20250225143204.png]]
2. **Provide a global access point to that instance**
	1. Remember those global variables that you (all right, me) used to store some essential objects? While they’re very handy, they’re also very unsafe since any code can potentially overwrite the contents of those variables and crash the app.
	2. Just like a global variable, the Singleton pattern lets you access some object from anywhere in the program. However, it also protects that instance from being overwritten by other code.

# Real-World Analogy

The government is an excellent example of the Singleton pattern. A country can have only one official government. Regardless of the personal identities of the individuals who form governments, the title, “The Government of X”, is a global point of access that identifies the group of people in charge.

# Implementation
To implement the singleton pattern, 

1. We must prevent external objects from creating instances of the singleton class. 
	1. ==Only the singleton class should be permitted to create its own objects==
2. We need to provide a method for external objects to access the singleton object.

This can be achieved by making the **constructor private** and providing a **static method** for external objects to access it.

![[Pasted image 20250225143711.png]]
The `instance` class variable holds the one and only instance of the Singleton class.

The `Singleton()` constructor is declared as private, preventing external objects from creating new instances.

The `getInstance()` method is a static class method, making it accessible to the external world.

## 1. Lazy implementation
This approach creates the singleton instance only when it is needed, saving resources if the singleton is never used in the application.

![[Pasted image 20250225144025.png]]
It checks whether the instance already exists, and creates a new instance if it doesn’t exist, otherwise returns the created one.

> [!WARNING] 
> This implementation is not **thread-safe**. If multiple threads call `getInstance()` simultaneously when `instance` is null, it's possible to  create multiple instances.



## 2.
# References
2. https://refactoring.guru/design-patterns/singleton
3. https://blog.algomaster.io/p/singleton-design-pattern
