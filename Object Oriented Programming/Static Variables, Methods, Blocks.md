When you declare a variable or a method as static, it **belongs to the class, rather than a specific instance**. 

This means that **only one instance of a static member exists**, even if you create multiple objects of the class, or if you don’t create any. It will be s**hared by all objects**.

## Static Variables
```Java
public class Counter {
  public static int COUNT = 0;
  Counter() {
    COUNT++;
  }
}

public class MyClass {
  public static void main(String[] args) {
    Counter c1 = new Counter();
    Counter c2 = new Counter();
    System.out.println(Counter.COUNT);
  }
}
// Outputs "2"
```

## Static Method
A static method belongs to the class rather than instances. Thus, it **can be called without creating instance of class**. It is used for altering static contents of the class.

1. Static method can not use non-static members (variables or functions) of the class.
2. Static method can not use `this` or `super` keywords.

```java
public class Counter {
  public static int COUNT = 0;
  Counter() {
    COUNT++;
  }

  public static void increment(){
    COUNT++;
  }
}

public class MyClass {
  public static void main(String[] args) {
    Counter.increment();
    Counter.increment();
    System.out.println(Counter.COUNT);
  }
}
// Outputs "2"
```

> [!NOTE] 
> Both static variables and methods can be accessed by non-static instance variables.

## Static Blocks

Static code blocks are used to initialise static variables. These blocks are executed immediately after declaration of static variables.

```Java
public class Saturn {
  public static final int MOON_COUNT;

  static {
    MOON_COUNT = 62;
  }
}

public class Main {
  public static void main(String[] args) {
    System.out.println(Saturn.MOON_COUNT);
  }
}
// Outputs "62"
```

## Static Nested Classes

A class can have static nested class which can be accessed by using outer class name.

```java
public class Outer {

  public Outer() {
  }

  public static class Inner {
    public Inner() {
    }
  }
}
```

In above example, class `Inner` can be directly accessed as a static member of class `Outer`.

```java
public class Main {
  public static void main(String[] args) {
    Outer.Inner inner = new Outer.Inner();
  }
}
```

Read ahead: Builder Pattern

