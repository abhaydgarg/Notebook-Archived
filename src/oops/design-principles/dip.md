# Dependency Inversion Principle

Also called **"Depend on abstractions not implementation"**, this means our dependencies should be interfaces/contracts or abstract classes rather than concrete implementations.

* High-level modules should not depend on low-level modules. Both should depend on abstractions.
* Abstractions should not depend on details. Details should depend on abstractions.

In a bad design the high level class uses directly and depends heavily on the low level classes. In such a case if we want to change the code in low level class then we may need to change code in high level class extensively or may be rewrite it again. In order to avoid such problems we can introduce an abstraction layer between high level classes and low level classes. Since the high level modules contain the complex logic they should not depend on the low level classes so the new abstraction layer should not be created based on low level modules. Low level modules are to be created based on the abstraction layer.

According to this principle the way of designing a class structure is to start from high level modules to the low level modules:

> **High Level Classes --&gt; Abstraction Layer --&gt; Low Level Classes**

## How to achieve DIP?

* Abstraction or contract must be fixed. No change in contract if new changes add up.

* It involves = 3 elements, one is High level classes with complex logic, second is low level classes with concrete implementation and third is single abstract class or multiple interfaces.

```php
<?php
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
         //Here we have a Database class that requires an adapter class to
         // speak to the database. We instantiate the adapter in the
         // constructor and create a hard dependency.
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}

<?php
namespace Database;

class Database
{
    protected $adapter;
    // This code can be re-factored to use Dependency Injection and
    // therefore loosen the dependency. Dependency Injection means loosening
    // our dependencies by controlling and instantiating them elsewhere in
    // the system and provide the dependency to class instead
    // creating inside class which need it.
    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}

<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(AdapterInterface $adapter)
    {
        $this->adapter = $adapter;
    }
}

interface AdapterInterface {}

class MysqlAdapter implements AdapterInterface {}
```

There are 2 benefits to the Database class now depending on an interface rather than a concretion. Depend on abstraction, or in other words **Why use abstraction?**

Consider that you are working in a team and the adapter is being worked on by a colleague. In our first example, we would have to wait for said colleague to finish the adapter before we could properly mock it for our unit tests. Now that the dependency is an interface/contract we can happily mock that interface knowing that our colleague will build the adapter based on that contract.
