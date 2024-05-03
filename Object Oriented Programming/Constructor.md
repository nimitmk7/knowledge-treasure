In class-based, object-oriented programming, a **constructor** is a special type of function called to create an object. 

It prepares the new object for use, often accepting arguments that the constructor uses to set required member variables.

A constructor resembles an instance method, but it differs from a method in that it **has no explicit return type**, it is not implicitly inherited and it usually has different rules for scope modifiers. 

Constructors often **have the same name as the declaring class**. They have the task of initializing the object's data members and of establishing the invariant of the class, failing if the invariant is invalid. So obviously, it is the first method called when an object is initialized.

A properly written constructor leaves the resulting object in a **valid state**. 

> [!WARNING]
Immutable objects must be initialized in a constructor.


## Example

```java
public class Student {
  String firstName;
  String lastName;
  int age;
  
  //constructor
  public Student(String firstName, String lastName, int age){
      this.firstName = firstName;
      this.lastName = lastName;
      this.age = age;
  }
  
  public static void main(String args[]) {
    Student myStudent = new Student("Ihechikara", "Abba", 100);
    System.out.println(myStudent.age);
  }

}
```

## Types

1. Default constructor
2. No-argument constructor
3. Parametrized constructor

