# Jargon

## Propagation

### Continuation passing style _[CPS]_

A result propagates from method to method, up the call stack. So if `a()` calls `b()`, which calls `c()`, which calls `d()`, and if `d()` returns a result, the result will propagate from `d` to `c` to `b` to `a`. It simply means that **a result is propagated by passing it to another function (the callback), instead of directly returning it to the caller.**

### Exception propagation

An exception propagates from method to method, up the call stack, until it's caught. So if `a()` calls `b()`, which calls `c()`, which calls `d()`, and if `d()` throws an exception, the exception will propagate from `d` to `c` to `b` to `a`, unless one of these methods catches the exception.

## Syntactic sugar

Syntactic sugar is a term to describe a feature in a language that lets you do something more easily/with less typing involved, but doesn’t really add any new functionality to the language that it didn’t already have.

For example; whilst you can write `x = x + 5;` to add `5` to the variable `x`, most languages also allow you to just write `x += 5;` which does the same thing.

## Kernel

![Kernel](assets/Kernel_Layout.svg.png)

A kernel is the central part of an operating system. It can be thought of as the program which controls all other programs on the computer. When the computer starts, it goes through some initialization (booting) function, such as checking memory. It is responsible for assigning and unassigning memory space which allows software to run.

It provides services so programs can request the use of the network card, the disk or other piece of hardware (the kernel forwards the request to special programs called device drivers which control the hardware), manages the file system and sets interrupts for the CPU to enable multitasking.

> Generaly you can say it as Operating System.

## Throughput

> The amount of material or items passing through a system or process.

In general terms, throughput is the maximum rate at which something can be processed.

How much data can be transferred from one location to another in a given amount of time. It is used to measure the performance of hard drives and RAM, as well as Internet and network connections.

For example, a hard drive that has a maximum transfer rate of 100 Mbps has twice the throughput of a drive that can only transfer data at 50 Mbps. Similarly, a 54 Mbps wireless connection has roughly 5 times as much throughput as a 11 Mbps connection.

## Bottleneck

Earlier computers were fed programs and data for processing while they were running. Von Neumann came up with the idea behind the stored program computer, our standard model, which is also known as the von Neumann architecture. In the von Neumann architecture, programs and data are held in memory; the processor and memory are separate and data moves between the two. In that configuration, latency is unavoidable.

Furthermore, in recent years, processor speeds have increased significantly. Memory improvements, on the other hand, have mostly been in density – the ability to store more data in less space – rather than transfer rates. As speeds have increased, the processor has spent an increasing amount of time idle, waiting for data to be fetched from memory. No matter how fast a given processor can work, in effect it is limited to the rate of transfer allowed by the bottleneck. Often, a faster processor just means that it will spend more time idle.

A simple example might be a program that relies on network communications to transmit or receive data when the network connection is limited to a speed insufficient to keep the program fully busy at all times, thus leaving other resources idle waiting for data.

> Simple words - What makes program run slow OR Where program takes lot of time.

## segfault

> Segmentation fault - Related to memory.

Segmentation fault occurs:

- when a process is trying to access non-existed memory address. **Dangling Reference** (pointer) problem means that trying to access an object or variable whose contents have already been deleted from memory.
- when a process is trying to access read-only memory address which is being used by other process.
- when attempting to write read-only memory.

> In simple words: segmentation fault is the operating system sending a signal to the program saying that it has detected an illegal memory access and is prematurely terminating the program to prevent memory from being corrupted.

## Race condition or Data race

In multihtreading programming, when two threads access the same location in memory at the same time, and at least one of the accesses is a write. In the situation the "reader" thread may get the old value or the new value, depending on which thread "wins the race."

Because the thread scheduling algorithm can swap between threads at any time, you don't know the order in which the threads will attempt to access the shared data. Therefore, the result of the change in data is dependent on the thread scheduling algorithm, i.e. both threads are "racing" to access/change the data.

## Polling vs Interrupts I/O

We have many external devices attached to the CPU like a mouse, keyboard, scanner, printer, etc. These devices also need CPU attention.

A computer must have a way of detecting or knowing the arrival of any type of input in low level harware. There are two ways that this can happen, known as polling and interrupts. Both of these techniques allow the CPU to deal with low level hardware and that are not related to the process it is currently running.

> Polling and Interrupt let CPU stop what it is currently doing and respond to the more important task.

### Polling I/O

> CPU keeps on checking I/O devices at regular interval whether it needs CPU service.

Polling is the simplest way for an I/O device to communicate with the CPU. The process of periodically checking status of the I/O device by CPU to see if it is time for the next I/O operation,is called polling. The I/O device simply puts the information in a Status register, and the CPU must come and get the information.

This is an inefficient method and much of the processors time is wasted on unnecessary polls.

_Compare this method to a teacher continually asking every student in a class, one after another, if they need help. Obviously the more efficient method would be for a student to inform the teacher whenever they require assistance._

### Interrupts I/O

> The I/O device interrupts the CPU and tell CPU that it need CPU service.

An interrupt is a signal to the CPU from a I/O device that requires attention.

A device controller puts an interrupt signal on the bus when it needs CPU’s attention when CPU receives an interrupt, It saves its current state and invokes the appropriate interrupt handler using the interrupt vector (addresses of OS routines to handle various events). When the interrupting device has been dealt with, the CPU continues with its original task as if it had never been interrupted.

## File descriptor

In simple words, when you open a file, the operating system creates an entry to represent that file and store the information about that opened file. So if there are 100 files opened in your OS then there will be 100 entries in OS (somewhere in kernel). These entries are represented by integers like (...100, 101, 102....). This entry number is the file descriptor. So it is just an integer number that uniquely represents an opened file in operating system. If your process opens 10 files then your Process table will have 10 entries for file descriptors.

Similarly when you open a network socket, it is also represented by an integer and it is called Socket Descriptor.

## CPU bound vs I/O bound

If most part of the process life is spent in cpu, then it is cpu bound process. An example of CPU bound application would be a service that calculates SHA-1 checksums. Majority of the time is spent crunching the hash - doing large amount of bitwise xors and shifts for the input string.

If most part of the lifetime of a process is spent in I/O state, then the process is a I/O bound process. Majority of the time is spent waiting for network, filesystem and perhaps database I/O to complete.

**In other way of looking it:** CPU bound means the program is bottlenecked by the CPU, or central processing unit. I/O bound means the program is bottlenecked by I/O, or input/output, such as reading or writing to disk, network, etc.

> bottleneck - thing that makes your program go slower.

In general, when optimizing computer programs, one tries to seek out the bottleneck and eliminate it. Like if it CPU that slow down the performance or it is I/O device throughput. Knowing that your program is CPU bound you try to improve the speed of CPU by using fastest in the market, so that one doesn't unnecessarily optimize something else.

## System call

A system call is the programmatic way in which a program requests a service from the kernel. This may include hardware-related services (for example, accessing a hard disk drive, printer), creation and execution of new processes, and communication with integral kernel services such as process scheduling.

## Tuple

Contains multiple entities of different nature.

Set of data with different types. Tuple contains a mixture of data types.

## Toolchain

In software, a toolchain is a **set of programming tools** that is used to perform a complex software development task or to create a software product

A set of tools such as compiler, loader, linker etc which are used to build executable program from source code. It may include — but not limited to — tools for parsing, checking, compiling, linking, configuring, packaging and deploying a software to the target platform(s).

## Boilerplate

Boilerplate code or just boilerplate are sections of **code that is included in many places** with little or no alteration.

!!! note
    When we say this package is a **react boilerpalte**, then we means this package or code can we used multiple times to create react project.

## Benchmark

> Measure, Evaluate by comparison with a pre-defined standard.

The objectives of benchmarking are (1) to determine what and where improvements are called for, (2) to analyze how other organizations achieve their high performance levels, and (3) to use this information to improve performance.

## Profiling

> Record and analyse

Program profiling or software profiling is a form of analysis that measures, for example, memory or time complexity of a program, the usage of particular instructions, or the frequency and duration of function calls. Most commonly, profiling information serves to aid program optimization.

## Stub

A stub is a function that has the expected signature (i.e. name and accepted arguments), but an incomplete implementation.

A stub function is put in place so that code that calls the function can be tested before the called function is fully written.

## Canonical _(standard - according to rule)_

This term makes sense when there is an entity that can be represented in multiple, acceptable ways. A canonical form is just a choice among all possible representations. For example, consider the following strings:

- (800) 532 0789
- +1–800–532–0789
- +1.800.532.0789

From our shared life experience we know that they represent the same phone number. The first one does not contain the country code, but from context you can infer that it is from the US.

For a computer program born yesterday, though, they are completely different. If we were to store it on a database just to display back to other humans, choosing a format might not matter. But if we wanted to search them, or call them, or make any other automated operation, it's valuable that we stick to a single representation.

The choice of a format, and what will be the canonical one (standard one), depend on the use you plan for the data. For example, the format should be suited to the application. If they are to be sorted and searched, you may want one that uses constant-size strings, like “0018005320789”. Note that the country code should not be presented with zeros, but it is acceptable in a canonical form!

## Canonical Path

The whole point of making anything canonicalis so that you can compare two things. For example, both `../../here/bar/x` and `./test/../../bar/x` may refer to the same location, but you can't do a textual comparison on the two paths. However, if you turn them into their canonical representation, they both become `../bar/x`, and we see that they actually refer to the same thing.

A good way to define a canonical path will be: `the shortest absolute path` (short, in the meaning of string-length).

This is an example of the difference between an absolute path and a canonical path:

absolute path: `C:\abc\..\abc\file.txt` = Absolute path means you can reach file/folder by many ways.

canonical path: `C:\abc\file.txt` = It means there is only one path to reach file/folder.

Consider these filenames:

`C:\temp\file.txt` - This is a path, an absolute path, and a canonical path.

`.\file.txt` - This is a path. It's neither an absolute path nor a canonical path.

`C:\temp\myapp\bin\..\\..\file.txt` - This is an absolute path. It's not a canonical path.

A canonical path is always an absolute path.

Converting from a path to a canonical path makes it absolute (usually tack on the current working directory so e.g. `./file.txt` becomes `c:/temp/file.txt`). The canonical path of a file just purifies the path, removing and resolving stuff like `..\`and resolving symlinks (on unixes).

## TDD

TDD or Test-Driven Development is a process for when you write and run your tests. Following it makes it possible to have a very high test-coverage. Test-coverage refers to the percentage of your code that is tested automatically, so a higher number is better. TDD also reduces the likelihood of having bugs in your tests, which can otherwise be difficult to track down.

The TDD process consists of the following steps:

1. Start by writing a test
2. Run the test and any other tests. At this point, your newly added test should fail. If it doesn’t fail here, it might not be testing the right thing and thus has a bug in it.
3. Write the minimum amount of code required to make the test pass
4. Run the tests to check the new test passes
5. Optionally refactor your code
6. Repeat from 1

It can take some effort to learn well, but spending the time can pay off big. TDD projects often get a code-coverage of 90-100%, which means maintaining the code and adding new features is easy. This is because you have a large set of tests, so you can trust your code and changes work, and didn’t break any other code either.

Some think you must use the “xUnit style” testing tools to use the TDD process. This is not the case – TDD works great with unit tests, but you can apply it to other testing methods as well. It also does not require any specific tool or syntax.

The most difficult thing about TDD for many developers is the fact you have to write your tests before writing code.

## BDD

BDD – Behavior-Driven Development – is perhaps the biggest source of confusion. When applied to automated testing, BDD is a set of best practices for writing great tests. BDD can, and should be, used together with TDD and unit testing methods.

One of the key things BDD addresses is implementation detail in unit tests. A common problem with poor unit tests is they rely too much on how the tested function is implemented. This means if you update the function, even without changing the inputs and outputs, you must also update the test. This is a problem because it makes doing changes tedious.

Behavior-Driven Development addresses this problem by showing you how to test. You should not test implementation, but instead behavior.

Let me show you an example of what happens when you think of implementation, and when you think of behavior:

```js
suite('Counter', function() {
  test('tick increases count to 1', function() {
    var counter = new Counter();

    counter.tick();

    assert.equal(counter.count, 1);
  });
});
```

This is a unit test of an imaginary counter object. We test that after calling tick, the value should be 1, which sounds like it makes sense. But there’s a problem in the test. The test is completely dependent on the fact that the counter starts at 0. So in other words, this test is relying on two things.

1. Counter starts at 0
2. Ticking increments by 1

The fact the counter starts at 0 is an implementation detail that’s irrelevant to the behavior of the `tick()` function. Therefore it should not have any bearing on the test. The only reason we wrote the test like this is because we were thinking of the implementation, not of the behavior.

BDD suggests to test behaviors, so instead of thinking of how the code is implemented, we spend a moment thinking of what the scenario is. Typically you phrase BDD tests in the form of “it should do something”. So when ticking a counter, it should increment count by one.

The important part here is thinking of the scenario, rather than the implementation, can lead you to design a better test.

```js
describe('Counter', function() {
  it('should increase count by 1 after calling tick', function() {
    var counter = new Counter();
    var expectedCount = counter.count + 1;

    counter.tick();

    assert.equal(counter.count, expectedCount);
  });
});
```

In this version of the test, which uses Mocha’s BDD style functions, we removed the implementation detail. Instead of relying on the counter starting at 0, we are comparing against counter.count + 1 which makes much more sense in terms of testing against behavior.

Sometimes your requirements change. Let’s imagine that for some reason the Counter has to start at some other value. Before, we would have to change the test to accommodate that, but with the BDD-variant there is no need to do so.

> Unit Testing gives you the **what**. Test-Driven Development gives you the **when**. Behavior Driven-Development gives you the **how**.
