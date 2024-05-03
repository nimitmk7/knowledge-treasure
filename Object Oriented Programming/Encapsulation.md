In object-oriented programming (OOP), encapsulation is ==the practice of bundling data and the methods that operate on that data into a single unit==.

We often often use this concept to hide an object’s internal representation or state from the outside. This is called **information hiding**.

Encapsulation = Abstraction + Information Hiding

## Example

``` Java
// Java Program to demonstrate
// Java Encapsulation
 
// Person Class
class Person {
    // Encapsulating the name and age
    // only approachable and used using
    // methods defined
    private String name;
    private int age;
 
    public String getName() { return name; }
 
    public void setName(String name) { this.name = name; }
 
    public int getAge() { return age; }
 
    public void setAge(int age) { this.age = age; }
}
 
// Driver Class
public class Main {
    // main function
    public static void main(String[] args)
    {
        // person object created
        Person person = new Person();
        person.setName("John");
        person.setAge(30);
 
        // Using methods to get the values from the
        // variables
        System.out.println("Name: " + person.getName());
        System.out.println("Age: " + person.getAge());
    }
}

```

## Access modifiers

**Public**: Public members can be accessed from anywhere in the program.
**Protected**: Protected members can be accessed from within the same package
and by subclasses of the class in which they are declared.
**Private**: Private members can only be accessed from within the class in which they are declared.
**Default** (package-private): Default members can be accessed from within the same package.
![[Pasted image 20240502232440.png]]
