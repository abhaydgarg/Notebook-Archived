# Decorator

It lets you **attach new behaviors to objects by placing these objects inside special wrapper.**

Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality. It does not use inheritance or sub classing to extend the functionality of class.

You want to add behavior or state to individual objects at run-time. Inheritance is not feasible because it is static and applies to an entire class.

The ability to add new behavior at runtime is accomplished by a Decorator object which ‘wraps itself’ around the original object. Multiple decorators can add or override functionality to the original object.

Decorators provide flexibility to statically typed languages by allowing runtime changes as opposed to inheritance which takes place at compile time.

## Problem

Imagine that you’re working on a notification library which lets other programs notify their users about important events.

The initial version of the library was based on the  `Notifier`  class that had only a few fields, a constructor and a single  `send`  method. The method could accept a message argument from a client and send the message to a list of emails that were passed to the notifier via its constructor. A third-party app which acted as a client was supposed to create and configure the notifier object once, and then use it each time something important happened.

At some point, you realize that users of the library expect more than just email notifications. Many of them would like to receive an SMS about critical issues. Others would like to be notified on Facebook and, of course, the corporate users would love to get Slack notifications.

How hard can that be? You extended the  `Notifier`  class and put the additional notification methods into new subclasses. Now the client was supposed to instantiate the desired notification class and use it for all further notifications.

But then someone reasonably asked you, “Why can’t you use several notification types at once? If your house is on fire, you’d probably want to be informed through every channel.”

You tried to address that problem by creating special subclasses which combined several notification methods within one class. However, it quickly became apparent that this approach would bloat the code immensely, not only the library code but the client code as well.

Extending a class is the first thing that comes to mind when you need to alter an object’s behavior. However, inheritance has several serious caveats that you need to be aware of.

- Inheritance is static. You can’t alter the behavior of an existing object at runtime. You can only replace the whole object with another one that’s created from a different subclass.
- Subclasses can have just one parent class. In most languages, inheritance doesn’t let a class inherit behaviors of multiple classes at the same time.

## Applicability

1. Use the Decorator pattern when you need to be able to assign extra behaviors to objects at runtime without breaking the code that uses these objects.

2. Use the pattern when it’s awkward or not possible to extend an object’s behavior using inheritance. Many programming languages have the final keyword that can be used to prevent further extension of a class. For a final class, the only way to reuse the existing behavior would be to wrap the class with your own wrapper, using the Decorator pattern.

## Code

```js
function Sale(price) {
  this.price = (price > 0) || 100;
  this.decorators_list = [];
}

Sale.prototype.decorate = function (decorator) {
  this.decorators_list.push(decorator);
};

Sale.prototype.getPrice = function () {
  var price = this.price,
    i,
    max = this.decorators_list.length,
    name;
  for (i = 0; i < max; i += 1) {
    name = this.decorators_list[i];
    price = Sale.decorators[name].getPrice(price);
  }
  return price;
};

Sale.decorators = {};

Sale.decorators.fedtax = {
  getPrice: function (price) {
    return price + price * 5 / 100;
  }
};

Sale.decorators.quebec = {
  getPrice: function (price) {
    return price + price * 7.5 / 100;
  }
};

Sale.decorators.money = {
  getPrice: function (price) {
    return "$" + price.toFixed(2);
  }
};

//# Client
var sale = new Sale(100);       // the price is 100 dollars
sale = sale.decorate('fedtax'); // add federal tax
sale = sale.decorate('quebec'); // add provincial tax
sale = sale.decorate('money');  // format like money
sale.getPrice();

// In another scenario, the buyer could be in a province
// that doesn't have a provincial tax,
// so you can do:
var sale = new Sale(100);       // the price is 100 dollars
sale = sale.decorate('fedtax'); // add federal tax
sale.getPrice();
```
