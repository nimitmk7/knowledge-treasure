> "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

In other words, the DRY principle encourages developers to write modular, reusable code and avoid duplicating the same functionality in multiple places.

It encourages us to minimize redundancy and write code that does one thing well, making our lives (and the lives of those who maintain our code) much easier.

# Examples
## Example 1: Avoid Code Duplication

Consider you have a system where you need to calculate taxes for different types of products. A non-DRY approach might look like this:

![[Pasted image 20250225122802.png]]

This code is repetitive and cumbersome. Let's DRY it up:
![[Pasted image 20250225122826.png]]
Here, a single function handles all product types, and the tax rate is passed as a parameter, embodying the DRY principle.

## Example 2: Using Decorators for Cross-cutting Concerns

Suppose you need to log function calls in multiple functions. Instead of writing the logging code in each function, use a decorator:

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8951aa2e-764c-4d60-9071-c77bac18f3be_1374x762.png)

In this example, the `logger` decorator is applied to any function that needs logging, significantly reducing repetitive logging code.
# Why is DRY important ?

- **Reduced Code Duplication**: By avoiding repeated code, you reduce the overall size of your codebase, making it easier to understand and maintain.
    
- **Improved Code Reusability**: DRY code is more modular and flexible, allowing you to reuse functionality across your application.
    
- **Easier Bug Fixing**: With a single, authoritative representation of code, you can fix bugs in one place, rather than searching for multiple instances of the same code.
    
- **Improved Consistency**: When you have duplicated code, it's easy for inconsistencies to creep in. By centralizing the logic in a single location, you ensure that the behavior remains consistent across the entire application.
    
- **Faster Development**: By leveraging existing code, you can speed up your development process and focus on new features and functionality.

# How to Apply the DRY Principle
Now that we understand the benefits of the DRY principle, let's explore some practical ways to apply it in your software development projects.

- **Identify Repetitive Code:** Identify areas in your codebase where you have repeated code. Look for patterns, similar logic, or functionality that appears in multiple places.
    
- **Extract Common Functionality:** Extract the common functionality into reusable components, such as functions, classes, or modules.
    
- **Use Inheritance and Composition:** By creating a hierarchy of classes or composing objects together, you can avoid duplicating code and promote code reuse.
    
- **Leverage Libraries and Frameworks:** Instead of reinventing the wheel, leverage libraries and frameworks to avoid writing repetitive code. However, be cautious not to overuse external dependencies, as they can introduce complexity and maintenance overhead.
    
- **Refactor Regularly:** Applying the DRY principle is an ongoing process. As your codebase evolves, it's important to regularly review and refactor your code to eliminate any new instances of duplication.
# When not to use the DRY Principle

The DRY principle is a guideline, not a hard-and-fast rule.

There are situations where it might not be the best approach:

1. **Premature Abstraction**: Trying to apply DRY too early in the development process might lead to over-engineering. If requirements are likely to change, you might abstract code that ends up getting discarded or significantly reworked.
    
2. **Performance-critical code**: In some cases, duplicating code can be faster than calling a reusable function, especially if the function has a high overhead or is not inlined.
    
3. **Sacrificing Readability**: If the duplicated code is very simple and easy to understand, it might be better to leave it as is, rather than creating a complex abstraction.
    
4. **One-time usage**: If a piece of code is only used once, it might not be worth extracting into a reusable function.
    
5. **Legacy code or technical debt**: In cases where you're working with legacy code or technical debt, it might be more practical to duplicate code temporarily, rather than trying to refactor the entire system.
    
6. **Debugging and testing**: In some cases, duplicating code can make it easier to debug and test, as it allows for more isolation and control.


