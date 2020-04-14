# Polymorphism

Ability to take many forms.

## Types of polymorphism

Parametric polymorphism or Method overloading or Static polymorphism or Early binding or Compile time binding: It is having two or more methods in the same class with the same name but different number or type of arguments.

## Why is it called static polymorphism or early binding or compile time binding?

```java
class Test {

   public int add(int no1, int no2) {
     return no1 + no2;
   }

   public int add(int no1, int no2, int no3) {
     return no1 + no2 + no3;
   }
}

class Program {

   public static void main(string[] args) {
     Test tst = new Test ();
     int sum1 = tst.add(10,20); //1
     }
}
```

In above statement number //1 compiler is aware at compile time that it need to call function add(int no1, int no2), hence it is called early binding and it is fixed so called static binding.

## Why use method overloading?

Flexibility: If you are working on a mathematics program, for example, you could use overloading to create several "multiply" methods, each of which multiplies a different number of type of argument: the simplest "multiply(int a, int b)" multiplies two integers; the more complicated method "multiply(double a, int b, int c)" multiplies one double by two integers -- Which would you rather remember: one method name called multiply, with 3 overloads, or 3 different method names: For example, multiplyInteger - multiplyDouble - multiplyIntAndDouble

- Method overriding or Dynamic polymorphism or Late binding or Run time binding: When a child class redefines the same method of a parent class, with the same type and number of parameters.

## Why is it called dynamic polymorphism or late binding or run time binding?

```java
class Car {

    public int speedLimit() {
        return 100;
    }
}

class Ford extends Car {

    public int speedLimit() {
        return 150;
    }
    public static void main(String args[]) {
      Car obj = new Ford(); //1
      int num= obj.speedLimit();
      System.out.println("Speed Limit is: "+num);
    }
}
```

Output = 150, see line //1, at runtime, which version of the method will be invoked is based on the type of actual object stored in the 'obj' variable which is of Ford class and not on the type of the 'obj' variable which is Car class.

## Why use method overriding?

Power to change the behavior through different implementation in subclass: Complete implementation can be replaced by using same method name and arguments. And which method will be executed will be based on the object of class which call it.
