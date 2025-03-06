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
## 2. Thread-safe singleton

This approach is similar to lazy initialization but also ensures that the singleton is thread-safe.

This is achieved by making the `getInstance()` method **synchronized** ensuring only one thread can execute this method at a time.

When a thread enters the synchronized method, it acquires a lock on the class object. Other threads must wait until the method is executed.

![[Pasted image 20250225144333.png]]

If calling the `getInstance()` method isn’t causing substantial overhead, this approach is straightforward and effective.

But, using `synchronized` can decrease performance, which can be a bottleneck if called frequently.

## 3. Double-checked locking

This approach minimizes performance overhead from synchronization by only synchronizing when the object is first created.

It uses the `volatile` keyword to ensure that changes to the instance variable are immediately visible to other threads.

![[Pasted image 20250225144448.png]]

> [!INFO] The Volatile keyword
> In Java, the `volatile` keyword is a modifier applied to variables, ensuring that changes made to the variable by one thread are immediately visible to other threads. It addresses the issue of thread caching, where each thread might maintain a local copy of a variable, leading to inconsistencies when multiple threads access and modify the same variable concurrently.
> 
> When a variable is declared `volatile`, the Java Virtual Machine (JVM) is instructed to always read the variable's value from main memory and write changes directly back to main memory, bypassing thread-local caches.

- If the first check (`instance == null)` passes, we synchronize on the class object.
    
- We check the same condition one more time because multiple threads may have passed the first check.
    
- The instance is created only if both checks pass.

## 4. Eager Initilization

In this method, we rely on the JVM to create the singleton instance when the class is loaded. The JVM guarantees that the instance will be created before any thread access the instance variable.

This implementation is one of the simplest and inherently thread-safe without needing explicit synchronization.

![[Pasted image 20250225145230.png]]

- `static` variable ensures there's only one instance shared across all instances of the class.
- `final` prevents the instance from being reassigned after initialization.

This approach is suitable if your application always creates and uses the singleton instance, or the overhead of creating it is minimal.

While it is inherently thread-safe, it could potentially waste resources if the singleton instance is never used by the client application.

## 5. Bill Pugh Singleton

This implementation uses a static inner helper class to hold the singleton instance. The inner class is not loaded into memory until it's referenced for the first time in the `getInstance()` method.

It is thread-safe without requiring explicit synchronization.

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f667e03-738a-4721-91e7-5f0efbe73341_3240x1536.png)


- When the `getInstance()` method is called for the first time, it triggers the loading of the SingletonHelper class.
    
- When the inner class is loaded, it creates the INSTANCE of BillPughSingleton.
    
- The `final` keyword ensures that the INSTANCE cannot be reassigned.
    

The Bill Pugh Singleton implementation, while more complex than Eager Initialization provides a perfect balance of lazy initialization, thread safety, and performance, without the complexities of some other patterns like double-checked locking.

## 6. Enum Singleton

  
In this method, the singleton is declared as an enum rather than a class.

Java ensures that only one instance of an enum value is created, even in a multithreaded environment.

The Enum Singleton pattern is the most robust and concise way to implement a singleton in Java.

![[Pasted image 20250225145858.png]]

Many Java experts, including **[Joshua Bloch](https://en.wikipedia.org/wiki/Joshua_Bloch)**, recommend Enum Singleton as the best singleton implementation in Java.

However, it's not always suitable, especially if you need to extend a class or if lazy initialization is a strict requirement.

## 7. Static Block Initialization

This is similar to eager initialization, but the instance is created in a static block.

It provides the ability to handle exceptions during instance creation, which is not possible with simple eager initialization.

![[Pasted image 20250225145952.png]]
- The static block is executed when the class is loaded by the JVM.
    
- If an exception occurs, it's wrapped in a RuntimeException.
    

It is thread safe but not lazy-loaded, which might be a drawback if the initialization is resource-intensive or time-consuming.

# Real-World Examples
Singleton is useful in scenarios like:

- **Managing Shared Resources** (database connections, thread pools, caches, configuration settings)
    
- **Coordinating System-Wide Actions** (logging, print spoolers, file managers)
    
- **Managing State (**user session, application state**)**
    

#### Specific Examples:

- **Logger Classes**: Many logging frameworks use the Singleton pattern to provide a global logging object. This ensures that log messages are consistently handled and written to the same output stream.
    
- **Database Connection Pools**: Connection pools help manage and reuse database connections efficiently. A Singleton can ensure that only one pool is created and used throughout the application.
    
- **Cache Objects**: In-memory caches are often implemented as Singletons to provide a single point of access for cached data across the application.
    
- **Thread Pools:** Thread pools manage a collection of worker threads. A Singleton ensures that the same pool is used throughout the application, preventing resource overuse.
    
- **File System**: File systems often use Singleton objects to represent the file system and provide a unified interface for file operations.
    
- **Print Spooler**: In operating systems, print spoolers manage printing tasks. A single instance coordinates all print jobs to prevent conflicts.

# Pros and Cons of Singleton Design Pattern
![[Pasted image 20250225150130.png]]

It's important to note that the Singleton pattern should be used judiciously, as it introduces global state and can make testing and maintenance more challenging. 

Consider alternative approaches like **dependency injection** when possible to promote loose coupling and testability.


# References
2. https://refactoring.guru/design-patterns/singleton
3. https://blog.algomaster.io/p/singleton-design-pattern
