SOLID principles are five software development principles which are guidelines to follow when building software so that it is easier to scale and maintain. 

Some of these principles may look similar but they are not targeting **the same goal**. It is possible to satisfy one principle while violating the other, even though they are alike.

> [!NOTE]
> I will be using the word “**Class”** but note that it can also apply to a **Function**, **Method** or **Module** in this article.

# Single Responsibility(S)
> “A class should have a single responsibility.”

![[Pasted image 20250225114247.png]]
If a Class has many responsibilities, it increases the possibility of bugs because making changes to one of its responsibilities, could affect the other ones without you knowing.

**Goal**: This principle aims to separate behaviors so that if bugs arise as a result of your change, it won’t affect other unrelated behaviors.

**Code Example**:
![[Pasted image 20250225120218.png]]
This class violates the principle because it has multiple responsibilities: **authentication**, **profile management**, and **email notifications**.

If you need to change the way user authentication is handled, you might inadvertently affect the email notification logic, or vice versa.

To adhere to the SRP, we can split this class into three separate classes, each with a single responsibility:

![[Pasted image 20250225120303.png]]
Now, each class has a single, well-defined responsibility. Changes to user authentication won't affect the email notification logic, and vice versa, improving maintainability and reducing the risk of unintended side effects.
# Open Closed(O)

> “Classes should be open for extension, but closed for modification”.
![[Pasted image 20250225114534.png]]

**Goal:** This principle aims to extend a Class’s behaviour without changing the existing behaviour of that Class. This is to avoid causing bugs wherever the Class is being used.

**Code example**:
Let's say you have a `ShapeCalculator` class that calculates the area and perimeter of different shapes like **rectangles** and **circles**.

![[Pasted image 20250225120439.png]]

If we want to add support for a new shape, like a **triangle**, we would have to modify the `calculate_area` and `calculate_perimeter` methods, violating the Open/Closed Principle.

To adhere to the principle, we can create an abstract base class for shapes and separate concrete classes for each shape type:

![[Pasted image 20250225120500.png]]
By introducing an abstraction (`Shape` class) and separating the concrete implementations (`Rectangle` and `Circle` classes), we can add new shapes without modifying the existing code.

The `ShapeCalculator` class can now work with any shape that implements the `Shape` interface, allowing for easy extensibility.

# Liskov Substitution(L)

> “If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program.”

![[Pasted image 20250225114909.png]]
When a **child** Class cannot perform the same actions as its **parent** Class, this can cause bugs.

If you have a Class and create another Class from it, it becomes a **parent** and the new Class becomes a **child.** The **child** Class should be able to do everything the **parent** Class can do. This process is called **Inheritance**.

The **child** Class should be able to process the same requests and deliver the same result as the **parent** Class or it could deliver a result that is of the same type.

The picture shows that the **parent** Class delivers Coffee(it could be any type of coffee). It is acceptable for the **child** Class to deliver Cappucino because it is a specific type of Coffee, but it is NOT acceptable to deliver Water.

If the **child** Class doesn’t meet these requirements, it means the **child** Class is changed completely and violates this principle.

**Goal**: This principle aims to enforce consistency so that the parent Class or its child Class can be used in the same way without any errors.

**Code example:**
Let's consider a scenario where we have a base class `Vehicle` and two derived classes `Car` and `Bicycle`.

Without following the LSP, the code might look like this:

![[Pasted image 20250225120554.png]]

In this example, the `Bicycle` class violates the LSP because it provides an implementation for the `start_engine` method, which doesn't make sense for a bicycle.

If we try to substitute a `Bicycle` instance where a `Vehicle` instance is expected, it might lead to unexpected behavior or errors.

To adhere to the LSP, we can restructure the code as follows:

![[Pasted image 20250225120628.png]]
Here, we've replaced the `start_engine` method with a more general `start` method in the base class `Vehicle`. The `Car` class implements the `start` method to start the engine, while the `Bicycle` class implements the `start` method to indicate that the rider is pedaling.

Now, instances of `Car` and `Bicycle` can be safely substituted for instances of `Vehicle` without any unexpected behavior or errors.
# Interface Segregation(I)

> Clients should not be forced to depend on methods that they do not use.

![[Pasted image 20250225115424.png]]
When a Class is required to perform actions that are not useful, it is wasteful and may produce unexpected bugs if the Class does not have the ability to perform those actions.

A Class should perform only actions that are needed to fulfil its role. Any other action should be removed completely or moved somewhere else if it might be used by another Class in the future.

**Goal**: ==This principle aims at splitting a set of actions into smaller sets so that a Class executes ONLY the set of actions it requires.==
 
**Code Example**
Let's consider a scenario where we have a media player application that supports different types of media files, such as audio files (MP3, WAV) and video files (MP4, AVI).

Without applying the ISP, we might have a single interface like this:
![[Pasted image 20250225120733.png]]
In this case, any class that implements the `MediaPlayer` interface would be forced to implement all the methods, even if it doesn't need them.

For example, an audio player would have to implement the `play_video`, `stop_video`, and `adjust_video_brightness` methods, even though they are not relevant for audio playback.

To adhere to the ISP, we can segregate the interface into smaller, more focused interfaces:
![[Pasted image 20250225120813.png]]

Now, we can have separate implementations for audio and video players:

![[Pasted image 20250225120841.png]]

By segregating the interfaces, each class only needs to implement the methods it actually requires. This not only makes the code more maintainable but also prevents clients from being forced to depend on methods they don't use.

If we need a class that supports both audio and video playback, we can create a new class that implements both interfaces:
![[Pasted image 20250225120913.png]]
# Dependency Inversion(D)

> High-level modules should not depend on low-level modules. Both should depend on the abstraction.
> Abstractions should not depend on details. Details should depend on abstractions.

![[Pasted image 20250225120030.png]]
Firstly, let’s define the terms used here more simply

**High-level Module(or Class)**: Class that executes an action with a tool.

**Low-level Module (or Class)**: The tool that is needed to execute the action

**Abstraction**: Represents an interface that connects the two Classes.

**Details**: How the tool works

This principle says a Class should not be fused with the tool it uses to execute an action. Rather, it should be fused to the interface that will allow the tool to connect to the Class.

It also says that both the Class and the interface should not know how the tool works. However, the tool needs to meet the specification of the interface.

**Goal**: This principle aims at reducing the dependency of a high-level Class on the low-level Class by introducing an interface.

**Code Example:**
Let's consider a example where we have a `EmailService` class that sends emails using a specific email provider (e.g., Gmail).

![[Pasted image 20250225122010.png]]

In this example, the `EmailService` class directly depends on the `GmailClient` class, a low-level module that implements the details of sending emails using the Gmail API.

This violates the DIP because the high-level `EmailService` module is tightly coupled to the low-level `GmailClient` module.

To adhere to the DIP, we can introduce an abstraction (interface) for email client. 

![[Pasted image 20250225122126.png]]

Now, the `EmailService` class depends on the `EmailClient` abstraction, and the low-level email client implementations (`GmailClient` and `OutlookClient`) depend on the abstraction.

This follows the principle, resulting in a more flexible and extensible design.



# References
1. [SOLID principles in pictures](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)
2. [SOLID principles explained with code](https://blog.algomaster.io/p/solid-principles-explained-with-code)
3. 