Inheritance is a feature of object-oriented programming that allows you to create new classes based on existing classes. 

The new class, called the **subclass**, inherits all the fields and methods of the existing class, called the **superclass**. This means that you can reuse the code that you have already written in the superclass in the subclass.

## Advantages
There are two main benefits to using inheritance:
1. Code reuse: Inheritance allows you to reuse code that you have already written. This can save you time and effort, and it can also help to make your code more consistent.
2. Extensibility: Inheritance allows you to extend the functionality of an existing class. This can be useful for creating new classes that are tailored to specific needs.

## Example

1. Superclass
```Java
public class Bicycle {
        
    // **the Bicycle class has three _fields_**
    public int cadence;
    public int gear;
    public int speed;
        
    // **the Bicycle class has one _constructor_**
    public Bicycle(int startCadence, int startSpeed, int startGear) {
        gear = startGear;
        cadence = startCadence;
        speed = startSpeed;
    }
        
    // **the Bicycle class has four _methods_**
    public void setCadence(int newValue) {
        cadence = newValue;
    }
        
    public void setGear(int newValue) {
        gear = newValue;
    }
        
    public void applyBrake(int decrement) {
        speed -= decrement;
    }
        
    public void speedUp(int increment) {
        speed += increment;
    }
        
}

```

2. Subclass

``` Java
public class MountainBike extends Bicycle {
        
    // **the MountainBike subclass adds one _field_**
    public int seatHeight;

    // **the MountainBike subclass has one _constructor_**
    public MountainBike(int startHeight,
                        int startCadence,
                        int startSpeed,
                        int startGear) {
        super(startCadence, startSpeed, startGear);
        seatHeight = startHeight;
    }   
        
    // **the MountainBike subclass adds one _method_**
    public void setHeight(int newValue) {
        seatHeight = newValue;
    }   
}
```

