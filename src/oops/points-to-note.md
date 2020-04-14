# Points to note

## What is a rule of coding Inheritance, Aggregation and Composition

Rule is simple - If relation says it is should be composite then implement it through composition and if relation says it should be aggregate then implement it using aggregation. It just depends upon the what the relation between the classes says to programmer. For inheritance, If relation is a pure "is-a" relation and conform to LSP then just go for inheritance way. Let the relation decide.

## How to code aggregation?

> Aggregation = How to code weak dependency between two classes? = Loose Coupling

The way object of Engine class is given to Car class also telling that how programmer can implement weak dependency between two classes. The Engine object is coming from outside the Car class so it is example of weak dependency.

```java
public class Car {
    private String make;
    private int year;
    private Engine engine;

    public Car(String make, int year, Engine engine) {
     this.make = make;
     this.year = year;
     // the engine object is created outside and is passed as argument to Car
     // constructor. When this Car object is destroyed, the engine is still
     // available to objects other than Car. If the instance of Car is garbage
     // collected the associated instance of Engine may not be garbage
     // collected (if it is still referenced by other objects)
     this.engine = engine;
    }
}
```

## How to code composition?

> Composition = How to code strong dependency between two classes?

The way object of Engine class is created in Car class also telling that how programmer can implement strong dependency between two classes. The Engine object is created inside the Car class so it is example of strong dependency.

```java
public class Car {
    private String make;
    private int year;
    private Engine engine;

 public Car(String make, int year, int engineCapacity, int engineSerialNumber) {
     this.make=make;
     this.year=year;
     // we create the engine using parameters passed in Car constructor
     // only the Car instance has access to the engine instance
     // when Car instance is garbage collected, the engine instance is garbage collected too
     engine = new Engine(engineCapacity, engineSerialNumber);
}
```

## When to use static variable

Static variable is used when you want to share data among all objects of the class. When updated then the updated value be available for all other objects as well. As static variable can be shared,these are often called as class variable.

## When to use static class

* Use a static class as a "factory". It usually doesn't have any state associated with it, all it does is produce other classes.
* For bundling helper class which contains helper functions, that you want to be available globally in program as globally available class.
* In situation when you want to have only one resource globally available in a system and can be used anywhere such as database connection class.
