The word polymorphism means having many forms. Polymorphism allows us to perform a single action in different ways. 

## Method Overloading(Compile time)
When there are multiple functions with the same name but different parameters then these functions are said to be **overloaded**. Functions can be overloaded by changes in the number of arguments or/and a change in the type of arguments.

### Example: Different argument type

``` Java
// Java Program for Method overloading
// By using Different Types of Arguments

// Class 1
// Helper class
class Helper {

	// Method with 2 integer parameters
	static int Multiply(int a, int b)
	{
		// Returns product of integer numbers
		return a * b;
	}

	// Method 2
	// With same name but with 2 double parameters
	static double Multiply(double a, double b)
	{
		// Returns product of double numbers
		return a * b;
	}
}

// Class 2
// Main class
class GFG {
	// Main driver method
	public static void main(String[] args)
	{
		// Calling method by passing
		// input as in arguments
		System.out.println(Helper.Multiply(2, 4));
		System.out.println(Helper.Multiply(5.5, 6.3));
	}
}

**Output**

8
34.65
```

### Example: Different number of arguments

```Java
// Java program for Method Overloading
// by Using Different Numbers of Arguments

// Class 1
// Helper class
class Helper {

	// Method 1
	// Multiplication of 2 numbers
	static int Multiply(int a, int b)
	{

		// Return product
		return a * b;
	}

	// Method 2
	// // Multiplication of 3 numbers
	static int Multiply(int a, int b, int c)
	{

		// Return product
		return a * b * c;
	}
}

// Class 2
// Main class
class GFG {

	// Main driver method
	public static void main(String[] args)
	{

		// Calling method by passing
		// input as in arguments
		System.out.println(Helper.Multiply(2, 4));
		System.out.println(Helper.Multiply(2, 7, 3));
	}
}

**Output**

8
42
```

## Method Overriding(Run time)

Method overriding is ==a feature in object-oriented programming that allows a subclass to implement a method that is already defined in a superclass==. 

The subclass's implementation replaces the superclass's implementation, but the overriding method must have the same name, parameters, and return type as the overridden method.

```Java
// Java Program for Method Overriding

// Class 1
// Helper class
class Parent {

	// Method of parent class
	void Print()
	{

		// Print statement
		System.out.println("parent class");
	}
}

// Class 2
// Helper class
class subclass1 extends Parent {

	// Method
	void Print() { System.out.println("subclass1"); }
}

// Class 3
// Helper class
class subclass2 extends Parent {

	// Method
	void Print()
	{

		// Print statement
		System.out.println("subclass2");
	}
}

// Class 4
// Main class
class GFG {

	// Main driver method
	public static void main(String[] args)
	{

		// Creating object of class 1
		Parent a;

		// Now we will be calling print methods
		// inside main() method

		a = new subclass1();
		a.Print();

		a = new subclass2();
		a.Print();
	}
}

```
