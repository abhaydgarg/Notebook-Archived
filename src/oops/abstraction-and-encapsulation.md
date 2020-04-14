# Abstraction and Encapsulation

Abstraction says, separate the implementation detail (implementation of behavior) which focuses on how to do from the detail which focuses on what to do (behavior itself).

For example: In Programming Languages

Data types in programming languages is the best data abstraction example, when we make use of data types ,we don't care how the data are stored, but only what operations are provided and what properties are supported. int, string, double, array, list.

For example: In DBMS

Database Management Systems have abstractions of Table and View. Every DBMS software may store underlying data differently. But to users, Table is always represented as Rows and Columns. This allows performing high level operations like query (declarative statements) on this data abstraction.

Encapsulation says, hide it by packing of data and functions into a single component which is called class. It provides mechanism for restricting access to some of the class components.

## How abstraction is different from encapsulation

When we talk about abstraction then sometimes we say that it is process where we hide implementation detail and show only which is relevant to outside world. It sounds like encapsulation because of HIDE word. But it is not, both are completely different process in OOPS.

| Abstraction | Encapsulation |
| :--- | --- |
| It is a process of separation. | It is process of hiding. |
| It must have at least 2 classes to implement the process, as describe below. | It is restricted to single class where programmer can code single class with two layers. as describe below. |
| **[1] Abstract class or Interface** = Tells to other classes in a system that What i do. Abstract classes and interfaces are also called Abstract Data Type (ADT) - about Behavior or Contract. | **[1] Class internally **= Variables and methods that are not accessible to other classes in system. What i let other to see. |
| **[2] Simple or concrete class** = extends or implements abstract class or interface as an agreement of contract which state that concrete class have to make it all happen which abstract or interface tell about What they do. How it is done is a responsibility of simple class. code Behavior or Contract. | **[2] Class externally** = Variables and methods that are accessible to other classes in system. What i let not other to see. |
| Achieved by Abstract classes, Interfaces and simple class (concrete class). | Achieved by Private, Public and Protected keywords. |

## Why use abstraction

Main purpose is to achieve flexible code (new addition done without changing a code in a many classes). As we know, while implementing abstraction in programming languages, we need two elements, one is Interface or abstract class and another is concrete class. The interface or abstract class defines the CONTRACT or in other words must have behavior which have to be implemented by concrete class. Coding based on abstraction also known as Design By Contract + Depend on abstraction (contract, behavior) than implementation that means the coding has to be done based on "contract" nothing more and nothing less.

The question arises here:

_**Why do we need abstract classes or interfaces in programming to be used while coding abstraction, however programmer can write code without interfaces and still achieve the goal?**_

1. System has a certain behavior which programmer code with help of programming language. And the system can only be useful to the user if programmer accurately code what system does. The programmer, when write a program, their mind mostly focus on implementation. And sometimes they may just missed to code the certain aspect of system's behavior. So by introducing abstract classes or interfaces in programming, compiler does not let the programmer to miss the "must have behavior" of system by generating compile-time error. Think of it as facility or extra helping tool given by programming language to programmer to write code which is more close to contract or behavior.
2. After an year or any new programmer, when see the code or diagram of class structure, the programmer can easily figure out that what certain type of behavior of system the particular code is achieving.

## Important thing about the CONTRACT

**Non changeable.** The contract or behavior, once planned must not be altered. It must be fixed. This is a very important and first step towards flexible system. If changed then concrete classes which had implemented the contract have to be changed and may be any other classes in a system which may lead to huge change of code and and testing of already written code. This is not what flexible design says. Therefore, before start coding system behavior, programmer must understand the system's behavior and planned it perfectly which also includes the place for new additions in system's behavior in future without changing a lot of classes.

## So what we say when we mention word contract in OOPS

The well planned behavior of the system is actually a contract that is introduced in coding practice by programming language through elements called interfaces and abstract classes.

See the OCP and DIP design principles which are based on idea of an abstraction. It also shows why programmer should write code which depends on the abstraction or interfaces or contract and that contract must be well planned and fixed.
