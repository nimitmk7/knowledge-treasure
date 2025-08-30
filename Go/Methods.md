A **method** in Go is a function that is attached to a specific **receiver**(type) argument (Example : **struct**). It defines the behaviour of the type and it should use the state of the type . The output of a method is dependent on the type of the receiver . 

Methods are also known as **receiver functions**. Here, the receiver can be of **struct** type or **non-struc**t type .

**Syntax**:
```Go
func (t ReceiverType) FunctionName(Parameters...) ReturnTypes...
```

> [!INFO] Different methods with the same name with a different receiver are allowed in Golang.

In Go, you are also allowed to create a method with a **pointer** receiver.  
With the help of a pointer receiver, if any change is made in the method, it will reflect in the caller which is not possible with the value receiver methods.

```Go
    type Counter struct {
        count int
    }

    func (c *Counter) Increment() { // Pointer receiver
        c.count++
    }

```



