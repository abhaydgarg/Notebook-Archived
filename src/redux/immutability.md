# Immutability

## What does immutability stand for

1. Don’t mutate data, and if you have to – create a clone and mutate it.
2. Reuse unchanged parts.

## Why immutability

Imagine that, you have a object and you either add new property or update the existing property or delete any property. The object has changed. Now if later in your code, you want to check if the object has changed or not. How would you do that? You need to iterate through all of its fields and somehow compare their values against some memorised pattern — but this approach is super expensive from computational point of view (remember that objects can be nested!). Of course, you might invent a system that would log all performed changes — but contrary to our theoretical single object, real-world applications contain multiple objects with deeply nested structures containing arrays or functions. Maintaining such history-writing mechanism would quickly become an unpleasant nightmare.

So what would happen, if we forbid all changes — or using another words, make mutations illegal?

Using immutability, when we want to alter the object. We first create a clone of a object and then alter the clone. In this way we have now two objects. Here is interesting part, you want to know if object has changed or not. All you need is to compare the reference of these two objects using `===`. Simple and extremely fast and there is no need to iterate all the properties of a object.

> We can observe the chnages efficiently.

### Benefits

#### Predictability

If the references differ, it indicates that some change has been done. If we are still left with the old reference, we can be sure that nothing have changed. Clear procedure. Nothing more, nothing less.

#### Change tracking

Imagine using single data object in various modules of the application — or even sending it to some 3rd party library. After a while, can you be certain that the contents of this object haven’t changed? Can you even be sure that it’s the same object as before? Using immutability, you can just compare the references (===) before and after the operation — if something have changed and we still play with the rules, we can expect them to differ.

#### Change history

Imagine that you need to implement functionality of undo and redo in your application. First thing that comes to mind is a monstrous, complex system of tracking all actions with possibility of rewinding (and replaying) them on your application’s data object.

With immutability, you can save and retain any number of history entries in form of separate (immutable) objects and just replace current state with them. Need to undo? Just bring back whole previous object with entire dataset and voila — it’s done. Need to redo? Again, restore the state saved while undoing. It’s so simple.

#### Thread safety

Given that by definition an immutable object cannot be changed, you will not have any synchronization issues when using them. No matter which thread is accessing the version of an object, it is guaranteed to have the same state it originally had. If one thread needs a new version or altered version of that object, it must create a new one therefore any other threads will still have the original object. This leads to simpler, more thread safe code.

## Cloning

The problem with cloning is – you have to waste dozen CPU cycles to create a clone of something that exists, and then waste dozen CPU cycles to let Garbage Collector consumes and digests no more used old variables.

You will have 2 (maybe more) copies of your data in the memory while you are “managing” it. Double memory consumption.

## Structural sharing

> Persistent data structures - enforces a constraint that all operations will return a newer version of that data structure and keep the original structure intact, instead of updating the original structure in-place.

Structural sharing - While creating a clone, you do not recreate the part of the object which has not changed but reuse it. If in object we have another object which has not changed then while cloning we do not recreate nested object but reusue it (reference) in a clone object.

[Check out this video](https://www.youtube.com/watch?v=Wo0qiGPSV-s)
