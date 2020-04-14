# Liskov Substitution Principle

Derived types must be completely substitutable for their base types.

If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program.

## Why use LSP?

Figure out if doing wrong inheritance - is-a relation: If substitutions cannot be made then class hierarchies would be a mess. Whenever a subclass instance is passed as parameter to any method, surprises might occur.

## How to achieve LSP?

Figure out same type of classes which has "is-a" relationship between them and apply inheritance.

```java
class Bird {
  public void fly(){}
  public void eat(){}
}
class Crow extends Bird {}
class Ostrich extends Bird{
  fly(){
    throw new UnsupportedOperationException();
  }
}

public BirdTest{
  public static void main(String[] args){
    List<Bird> birdList = new ArrayList<Bird>();
    birdList.add(new Bird());
    birdList.add(new Crow());
    birdList.add(new Ostrich());
    letTheBirdsFly ( birdList );
  }
  static void letTheBirdsFly ( List<Bird> birdList ){
    for ( Bird b : birdList ) {
      b.fly();
    }
  }
}
```

### How to fix the issue?

In the above scenario we can factor out the fly feature into- Flight and NonFlight birds.

```java
class Bird{
  public void eat(){}
}
class FlightBird extends Bird{
  public void fly()()
}
class NonFlight extends Bird{}
```
