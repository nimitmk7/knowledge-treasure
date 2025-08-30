Structs are used to **group related data together**. This is similar to how properties are defined in a class in other languages.

A struct defines a collection of fields (variables) that can be of different types.

```Go
// Define a struct named 'Person'  
type Person struct {  
	Name string  
	Age int  
	City string  
	IsAdult bool  
}
```

  -  **ORM and Database Interactions**: Structs are often used to model database tables for Object-Relational Mapping (ORM) and can be generated from SQL schemas
  - **Serialization and Deserialization***: Structs serve as targets for deserializing various data formats like CSV, JSON, and TOML, and for generating data in these formats. Tools exist to convert XML or JSON into Go structs automatically.
  - **Validation**: Struct tags can be used for data validation, allowing developers to define rules directly on struct fields.
  - **Reflection**: The `reflect` package in Go allows runtime introspection of types, including accessing and parsing struct tags. This enables dynamic behavior based on the metadata provided in the tags.

Go also supports **nested structures** and **embedded structures**, allowing for complex data relationships.

# Nested structures

A nested structure in Go is a struct that ==includes another struct as one of its fields==. This allows for the creation of more complex and organized data structures by composing smaller, more focused structs.

```Go
package main

import "fmt"

// Define the Address struct
type Address struct {
	Street  string
	City    string
	ZipCode string
}

// Define the Person struct, which includes the Address struct
type Person struct {
	Name    string
	Age     int
	Address Address // Nested struct
}

func main() {
	// Initialize the nested struct
	person1 := Person{
		Name: "Alice",
		Age:  30,
		Address: Address{ // Initialize the Address struct within Person
			Street:  "123 Main St",
			City:    "Anytown",
			ZipCode: "12345",
		},
	}

	// Access fields of the nested struct
	fmt.Printf("Name: %s\n", person1.Name)
	fmt.Printf("Age: %d\n", person1.Age)
	fmt.Printf("Street: %s\n", person1.Address.Street)
	fmt.Printf("City: %s\n", person1.Address.City)
	fmt.Printf("Zip Code: %s\n", person1.Address.ZipCode)
}
```

# Embedded Structures

**Embedded structures** allow you to include one struct type directly within another struct **without explicitly naming the field**. 

When a struct is embedded anonymously, the fields of the inner (embedded) struct are "promoted" and ==become directly accessible through the outer struct==, as if they were declared directly in the outer struct itself.

```Go
package main

import "fmt"

// Define the Engine struct
type Engine struct {
    Type     string // e.g., "V8", "Electric"
    Horsepower int
}

// Define the Car struct, which embeds the Engine struct anonymously
type Car struct {
    Brand   string
    Model   string
    Engine  // This is an embedded struct (anonymous field of type Engine)
    Year    int
}

func main() {
    // Create a Car instance
    myCar := Car{
        Brand: "Tesla",
        Model: "Model 3",
        Engine: Engine{ // Initialize the embedded Engine struct
            Type:     "Electric",
            Horsepower: 283,
        },
        Year: 2023,
    }

    // Access fields of the embedded Engine struct directly from myCar
    fmt.Printf("Car Brand: %s\n", myCar.Brand)
    fmt.Printf("Car Model: %s\n", myCar.Model)
    fmt.Printf("Engine Type (from embedded struct): %s\n", myCar.Type)         // Direct access
    fmt.Printf("Engine Horsepower (from embedded struct): %d\n", myCar.Horsepower) // Direct access
    fmt.Printf("Car Year: %d\n", myCar.Year)

    fmt.Println("\n--- Another Car ---")

    // You can also initialize an embedded struct inline
    anotherCar := Car{
        Brand: "Ford",
        Model: "Mustang GT",
        Engine: Engine{
            Type:     "V8",
            Horsepower: 450,
        },
        Year: 2022,
    }
    fmt.Printf("Car Brand: %s\n", anotherCar.Brand)
    fmt.Printf("Engine Type: %s\n", anotherCar.Type)
    fmt.Printf("Engine Horsepower: %d\n", anotherCar.Horsepower)
}
```

In this example:

- The `Engine` struct is defined with `Type` and `Horsepower` fields.
- The `Car` struct then includes `Engine` simply by its type name (`Engine`) rather than `engineField Engine`. This is **anonymous embedding**.
- Because `Engine` is embedded, you can access `Type` and `Horsepower` directly through the `myCar` variable (e.g., `myCar.Type`, `myCar.Horsepower`) as if they were fields declared directly within `Car`. If `Engine` were a named field (e.g., `Powertrain Engine`), you would access them as `myCar.Powertrain.Type`.


