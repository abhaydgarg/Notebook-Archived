# Testing

## Unit testing

A unit test focuses on a single “unit of code” – usually a function in an object or module. By making the test specific to a single function, the test should be simple, quick to write, and quick to run. This means you can have many unit tests, and more unit tests means more bugs caught. They are especially helpful if you need to change your code: When you have a set of unit tests verifying your code works, you can safely change the code and trust that other parts of your program will not break.

A unit test should be isolated from dependencies – for example, no network access and no database access. There are tools that can replace these dependencies with fakes you can control. This makes it trivial to test all kinds of scenarios that would otherwise require a lot of setup. For example, imagine having to set up an entire database just to run a test. Ugh, no thanks.

The basic pieces of a unit test are there: Individual tests, which test one thing, and they are isolated from each other. You could use scripts like this to build rudimentary unit tests for your code. But using an actual unit testing tool such as Mocha or Jasmine will make it easier to write tests, and they have other helpful features such as better reporting when tests fail (which makes it easier to find out what went wrong)

Some think that any automated test is a unit test. This is not true. There are different types of automated tests, and each type has its own purpose.

Here are three of the most common types of automated tests:

- **Unit tests**: A single piece of code (usually an object or a function) is tested, isolated from other pieces
- **Integration tests**: Multiple pieces are tested together, for example testing database access code against a test database
- **Acceptance tests (also called Functional tests)**: Automatic testing of the entire application, for example using a tool like Selenium to automatically run a browser.

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
