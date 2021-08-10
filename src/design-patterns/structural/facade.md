# Facade

It **provides a simplified interface** to a library, a framework, or any other complex set of classes.

The intent of the Façade is to provide a high-level interface (properties and methods) that makes a subsystem or toolkit easy to use for the client.

## Applicability

1. Use the Facade pattern when you need to have a limited but straightforward interface to a complex subsystem.

2. Another area where Façades are used is in refactoring. Suppose you have a confusing or messy set of legacy objects that the client should not be concerned about. You can hide this code behind a Façade. The Façade exposes only what is necessary and presents a cleaner and easy-to-use interface.
