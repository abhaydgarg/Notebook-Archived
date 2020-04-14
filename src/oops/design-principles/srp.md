# Single Responsibility Principle

A class and interface should have one and only one responsibility.

## Why use SRP?

Less fragile: When a class has more than one reason to be changed, it is more fragile. A change in one location might lead to some unexpected behavior in totally other places. Low coupling: More responsibilities lead to higher coupling. The couplings are the responsibilities. Higher coupling leads to more dependencies, which is harder to maintain. Maintainability: It's obvious that it is much easier to maintain a small single purpose class, then a big monolithic one.

## How to achieve SRP?

Its simple - Create separate class out of existing class if existing class has more than one responsibility.
