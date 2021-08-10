# Relationship types = Dependency type

## Association

Association is a relationship between two classes. In other words, association defines the multiplicity (see below) between classes. You may be aware of one-to-one, one-to-many, many-to-one, many-to-many all these words define an association between objects.

## Generalization or Inheritance (IS A)

The Generalization relationship ("is a") indicates that one of the two related classes (the subclass) is considered to be a specialized form of the other (the super type) and superclass is considered as 'Generalization' of subclass. In practice, this means that any instance of the subtype is also an instance of the superclass.

It is implemented using inheritance where parent and child classes are of same type (LSP design principle) or in other words when class A has all the same feature of B and you want to add some more features. So you simply extend B and add the new features.

## (HAS A) Association - Aggregation

The composition "has-a" relationship is of "weak-type". Weak meaning that one can exists without the other. Aggregation can occur when a one class is a container of other classes, but where the contained classes do not have a strong life cycle dependency on the container--essentially, if the container is destroyed, then classes contained within are not. Or in other words, the contained object can exist without the existence of container object.

Example: Engine and Wheel class can exist independently without Car class.

## (HAS A) Association - Composition

The composition "has-a" relationship is of "strong-type". Strong meaning that one can't exists without the other. Composition usually has a strong life cycle dependency between instances of the container class and instances of the contained class(es): if the container is destroyed, normally every instance that it contains is destroyed as well. In other words, the contained object cannot exist without the existence of container object.

Example: Leg class and Hand class cannot exist without Person class.
