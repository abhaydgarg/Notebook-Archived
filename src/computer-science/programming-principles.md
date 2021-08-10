# Programming principles every programmer must follow

We’ve all heard horror stories about spaghetti code, massive if-else chains, entire programs that can break just by changing one variable, functions that look like they were obfuscated, and so on. That’s what happens when you try to make a shippable product with only a semester of programming experience under your belt.

**Don’t settle for writing code that works. Aim to write code that can be maintained** — not only by yourself, but by anyone else who may end up working on the software at some point in the future. To that end, here are several principles to help you clean up your act.

!!! note "idea"
    Conceptualize design, code and system as a many different **independent** and **well-connected** pieces which form a whole system or unit when arrange or combine together in such a way that they are **cohesive** but at the same time **loosely coupled**. So that whenever you want to **remove** or **update** the certain piece of the system or unit then you can **do it without disturbing the other pieces or with minimal disturbance**.

    > To achieve such a modular, flexible and easy to maintain system, you must follow the following rules properly and these rules can easily be achieved by using combination of various **Design Patterns.**

## KISS

The **“keep it simple, stupid”** principle applies to pretty much all of life, but it’s especially necessary in medium-to-large programming projects.

Simplicity (and avoiding complexity) should always be a key goal. Simple code takes less time to write, has fewer bugs, and is easier to modify.

## DRY

The **“don’t repeat yourself”** principle is crucial for clean and easy-to-modify code. It’s a common coding mistake. When writing code, you want to avoid duplication of data and duplication of logic. If you notice the same chunk of code being written over and over again, you’re breaking this principle.

## Open/Closed

Whether you’re writing objects or modules, you should aim to make your code **open to extension but closed to modification.** This applies to all kinds of projects, but is especially important when releasing a **library or framework** meant for others to use.

Write code that prevents direct modification and encourages extension. This separates core behavior from modified behavior. The benefits? Greater stability (users can’t accidentally break core behavior) and greater maintainability (users only worry about extended code). The open/closed principle is key to making a good **API**.

!!! note "Useful design pattern"
    - Visitor
    - Decorator
    - Middleware
    - Hook system

## Composition > Inheritance

The **“composition over inheritance”** principle states that objects with complex behaviors should do so by containing instances of objects with individual behaviors rather than inheriting a class and adding new behaviors.

Composition is a lot cleaner to write, easier to maintain because each individual behavior is its own class, and you create complex behaviors by combining individual behaviors.

### Two types of compositions

#### Functional composition

Composing independent functions together to form a system or sub-system to achieve specific requirement and feature. Composing means bring multiple independent functions together in such a way that they are well cohesive but at the same time they are loosely-coupled. So that they can be updated, removed and new functions are added without distrubing the system or sub-system.

#### Object composition

Composing independent objects together to form a system or sub-system to achieve specific requirement and feature. Composing means bring multiple independent objects together in such a way that they are well cohesive but at the same time they are loosely-coupled. So that they can be updated, removed and new objects are added without distrubing the system or sub-system.

### Problem in composition

How to connect multiple independent pieces together so that we still get a loosely coupled unit, so that we can update and remove the pieces with minimal hassles later?

#### Minimise Coupling

Coupling between modules/components is their degree of mutual interdependence; lower coupling is better. In other words, coupling is the probability that code unit "B" will "break" after an unknown change to code unit "A". A change in one module usually forces a ripple effect of changes in other modules. A particular module might be harder to reuse and/or test because dependent modules must be included. Developers might be afraid to change code because they aren't sure what might be affected.

!!! note "Useful design pattern"
    - Dependency injection

## Single Responsibility

The **single responsibility** principle says that every class or module in a program should only concern itself with providing one bit of specific functionality. As Robert C. Martin puts it, “A class should have only one reason to change.”

## Separation of Concerns

Separation of concerns is a design principle for separating a computer program into distinct sections, such that each section addresses a separate concern. For example the business logic of the application is a concern and the user interface is another concern. Changing the user interface should not require changes to business logic and vice versa.

When concerns are well-separated, individual sections can be reused, as well as developed and updated independently.

!!! note "Useful design pattern"
    - MVC

## Hide Implementation Details

A software module hides information (i.e. implementation details) by providing an interface, and not leak any unnecessary information.

When the implementation changes, the interface clients are using does not have to change.

!!! note "Useful design pattern"
    - Facade
    - Adapter

## YAGNI

The **“you aren’t gonna need it”** principle is the idea that you should never code for functionality that you may need in the future. Chances are, you won’t need it and it will be a waste of time — and not only that, but it will needlessly increase your code’s complexity.

## Avoid Premature Optimization

The no premature optimization principle is similar to the YAGNI principle. The difference is that YAGNI addresses the tendency to implement behaviors before they’re necessary while this principle addresses the tendency to speed up algorithms before it’s necessary.

The problem with premature optimization is that you can never really know where a program’s bottlenecks will be until after the fact. You can guess, of course, and sometimes you may even be right. But more often than not, you’ll waste valuable time trying to speed up a function that isn’t as slow as you think or doesn’t get called as often as you’d expect.

Reach your milestones as simply as you can, then profile your code to identify true bottlenecks.

## Refactor, Refactor, Refactor

One of the hardest truths to accept as an inexperienced programmer is that code rarely comes out right the first time. It may feel right when you implement that shiny new feature, but as your program grows in complexity, future features may be hindered by how you wrote that early one.

Codebases are constantly evolving. It’s completely normal to revisit, rewrite, or even redesign entire chunks of code — and not just normal, but healthy to do so. You know more about your project’s needs now than when you did at the start, and you should regularly use this newly gained knowledge to refactor old code.

Note that it doesn’t always have to be a big process. Take a page from the Boy Scouts of America, who live by these words: “Leave the campground cleaner than you found it.” If you ever need to check or modify old code, always clean it up and leave it in a better state.

## Clean Code > Clever Code

Speaking of clean code, leave your ego at the door and forget about writing clever code. You know what I’m talking about: the kind of code that looks more like a riddle than a solution and exists solely to show off how smart you are. The truth is, nobody really cares.

One example of clever code is packing as much logic into one line as possible. Another example is exploiting a language’s intricacies to write strange but functional statements. Anything that might cause someone to say “Wait, what?” when poring over your code.

!!! quote ""
    Patterns, let you write code in more efficient and reusable way. It is not just about classic programming patterns but also patterns specific to library, framework, utility etc. For example `Redux patterns`, `react patterns`, `nodejs patterns` etc.

    Do not try to solve imaginary problem, imaginary scenarios and imaginary cases. Understand the context of problem clearly. Clear understanding of problem leads to better solution.

    ...git actually has a simple design, with stable and reasonably well-documented data structures. In fact, I'm a huge proponent of designing your code around the data, rather than the other way around, and I think it's one of the reasons git has been fairly successful. I will, in fact, claim that the difference between a bad programmer and a good one is whether he considers his code or his data structures more important. _Bad programmers worry about the code. Good programmers worry about data structures and their relationships._
