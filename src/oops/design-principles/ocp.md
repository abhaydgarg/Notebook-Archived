# Opened Closed Principle

Software entities like classes, modules and functions should be open for extension but closed for modifications.

The Open Close Principle states that the design and writing of the code should be done in a way that new functionality should be added with minimum changes in the existing code. The design should be done in a way to allow the adding of new functionality as new classes, keeping as much as possible existing code unchanged.

This principle is for building software libraries where other programmers can extend library without changing the core or already written classes.

## Why use OCP?

A clever application design and the code writing part should take care of the frequent changes that are done during the development and the maintaining phase of an application. Usually, many changes are involved when a new functionality is added to an application. Those changes in the existing code should be minimized, since it's assumed that the existing code is already unit tested and changes in already written code might affect the existing functionality in an unwanted manner.

## How to achieve OCP?

1. Allow the classes to depend on the abstractions, there by new features can be added by creating new extension or concrete class out of the abstraction.
2. Conform to rule of good abstraction that states non changeable contract. Plan abstraction or contract carefully so it must be fixed and unchangeable in future. Because if contract or abstraction is changed then we have to change the implementation in already existed concrete classes which uses the abstraction, which clearly violates the OCP principle.
3. Involves = two elements, one is single abstract class or multiple interfaces and second is one or more concrete classes that implement the contract.

Let us go through the below example to understand the principle and the underlying benefits: Suppose for a certain requirement you would need to write a class for calculating the fare that a person is charged with. So, you write the following two classes.

```java
public class Bus {

    public double CostPerMile;
    public double MilesTravelled;
    public int NoOfSeatsBooked;
}

public class FareCalculator {

    public double Fare(Bus bus) {

        double fare = 0;
        fare = bus.CostPerMile * bus.MilesTravelled * bus.NoOfSeatsBooked;
        return fare;
    }
}
```

Everything is fine in the above code. FareCalculator class would return the computed fare for Bus properly.

But the above design is a clear violation of OCP: Let's say, the new requirement says that the FareCalculator class should include fare calculation for a new vehicle – Truck also. And, you know that the calculation would differ in both the cases. In case of Bus, the calculation depends on the number of seats booked and in case of Truck, it depends on the Weight of Goods that truck would be carrying.

So, you would modify FareCalculator class as follows:

```java
public class FareCalculator {

    public double BusFare(Bus bus) {

        double fare = 0;
        fare = bus.CostPerMile * bus.MilesTravelled * bus.NoOfSeatsBooked;
        return fare;
    }

    public double TruckFare(Truck truck) {

        double fare = 0;
        fare = truck.CostPerMile * truck.MilesTravelled * truck.GoodsWeight;
        return fare;
    }
}

public class Bus {

    public double CostPerMile;
    public double MilesTravelled;
    public int NoOfSeatsBooked;
}

public class Truck {

    public double CostPerMile;
    public double MilesTravelled;
    public int GoodsWeight;
}
```

Definitely your Code would work fine and delivers the results accurately.

But, there are numerous design problems with the above modification:

If the requirement is to compute fare for one more vehicle say- Car etc, you would be making a change in the existing FareCalculator class to implement the new requirement. So, every time you need to make sure that the new function added in the class has not broken any older functinality present already in the class.

So, in the above design, its very clear that the class is not really closed for modifcation and also not open for extension. In the real world, the class may not be having just a few lines of code and in actual would be having hundreds or thousands lines of code. So, anything that you are modifying in an existing class in order to implement a new functionality lead to enormous development and testing efforts. For every new functionality to be added, you need to make sure that nothing has been broken with respect to the functinality already present in the class.

### You can overcome the said problems by following OCP

You create a Abstract class Vehicle as follows

```java
public abstract class Vehicle {
  public double Fare();
}
```

And, then write Bus and Truck class extending the Vehicle class and implementing the Fare function as follows:

```java
public class Bus extends Vehicle {

    public double CostPerMile;
    public double MilesTravelled;
    public int NoOfSeatsBooked;

    public double Fare() {

        return CostPerMile * MilesTravelled * NoOfSeatsBooked;
    }
}

public class Truck extends Vehicle {

    public double CostPerMile;
    public double MilesTravelled;
    public int GoodsWeight;

    public double Fare() {

        return CostPerMile * MilesTravelled * GoodsWeight;
    }
}
```

Finally, you write FareCalculator as follows:

```java
public class FareCalculator {

    public double Fare(Vehicle vehicle) {

        double fare = 0;
        fare = vehicle.Fare();
        return fare;
    }
}
```

In the above design, the FareCalculator and all other classes are absolutely closed for modifications. With the usage of Abstract class Vehicle, now, even if the requirement is to compute fare for a new vehicle, you can easily do it by writing a new class and extending the Vehicle class in it. By doing so, you would not really need to make modifications in any of the existing classes and you will always extend the existing classes to implement any new functionality.
