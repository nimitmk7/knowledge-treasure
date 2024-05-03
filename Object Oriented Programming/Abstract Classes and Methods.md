An _abstract class_ is a class that is declared `abstract`—it may or may not include abstract methods. Abstract classes **cannot be instantiated**, but they **can be subclassed.**

An _abstract method_ is a method that is declared without an implementation (without braces, and followed by a semicolon), like this:

```Java
abstract void moveTo(double deltaX, double deltaY);
```


If a class includes abstract methods, then the class itself _must_ be declared `abstract`, as in:

```Java
public abstract class GraphicObject {
   // declare fields
   // declare nonabstract methods
   abstract void draw();
}
```

## Abstract Classes and Interfaces

- Abstract classes are similar to interfaces. 
	- You cannot instantiate them, and they may contain a mix of methods declared with or without an implementation. 
- However, with abstract classes, you can declare fields that are not static and final, and define public, protected, and private concrete methods. 
	- With interfaces, all fields are automatically public, static, and final, and all methods that you declare or define (as default methods) are public.
- You can extend only one class, whether or not it is abstract.
	- But you can implement any number of interfaces.

