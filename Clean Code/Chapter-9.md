# Chapter 9 - Unit Tests

around 30 years ago, no one has heard of Test Driver Development. For the vast majority of us, unit tests were short snippets of code that we wrote to make sure our program (worked) and threw away.

Nowadays, we would write tests that make sure that every nook and cranny of that code works as expected. And make sure once written that these tests are convenient for anyone who works with code. And would sure that tests and code are checked together in the same source package.

The agile and TDD have encouraged many programmers to write automated unit test, but in the mad rush to add testing to our discipline, many programmers missed some of the more subtle and important points of writing good tests.

## The Three Laws of TDD

**First Law**: You may not write production code until you have written a failing unit test.

**Second Law**: You may not write more of a unit test than is sufficient to fail, and not compiling is failing.

**Third Law**: You may not write more production code than is sufficient to pass the currently failing test.

Note how the testing and production code is written together, with tests that are just a few seconds ahead of the production code0

If we work this way, we will write dozens of tests every day, hundreds of tests every month, and thousands of tests every year. If we work this way, those tests will cover virtually all of our production code. The sheer bulk of those tests, which can rival the size of the production code itself, can present a daunting management problem.

## Keeping Tests Clean

Some teams think that it doesn't worth it to follow the clean code practices in tests, or as quality as the production code is, they play it "quick and dirty", Their variables did not have to be well named, their test functions did not need to be short and descriptive. Their test code did not need to be well designed and thoughtfully partitioned. So long as the test code worked, and so long as it covered the production code, it was good enough.

What this team did not realize was that having dirty tests is equivalent to, if not worse than, having no tests. The problem that tests must change as the production code evolves, the dirtier the test, the harder they are to change. The more tangled the test code is, the more likely it is that you will spend more time cramming new tests into the suite than you will writing new production code. As you modify the production code, old tests start to fail, and the mess in the test code makes it hard to get those tests to pass again. **So tests become viewed as an ever increasing debts**

From release to release the cost of maintaining the test suite rose, eventually it will become the single biggest complain form the developers. When managers asks why their estimates goes too large, developer will blame the tests, and in the end they will be forced to discard the entire suite.

But without tests they lost the abilities to make sure that changes to their codebase worked as expected and change in one part will not break the other parts in the system, number of bugs increased and they started to fear making changes, they stopped cleaning the production code because they feared the changes will make harm than good, they production code started to rot.

In the end they were left with no tests, tangled and bug-riddled production code, frustrated customers.

The moral of this story is simple: **Test code is as important as production code.** It is not a second-class citizen. It requires thought, design, and care. It must be kept as clean as production code.

### Test enable the -ilities

If you don't keep your test clean, you will lose them, and without them, you make your codebase less flexible, if you have tests, you don't fear making a change to your code. Without change every change is a possible bug.

So having an automated suite of unit tests that cover the production code is the key to keeping your design and architecture as clean as possible. Tests enable all the -ilities, because tests enable change.

## Clean Tests

What makes a clean test? Three things, readability, readability, and readability. Readability is perhaps even more important in test than it is in production code.

What makes a test readable, the same thing that makes any code readable: **clarity, simplicity, and density of expression.**

### Domain-Specific Testing Language

Rather than using the APIs the programmers use to manipulate the system, we build up a set of functions and utilities that make use of those apis and make the tests more convenient to write and easier to read. These functions and utils become a specialized api used by the tests, they are a *testing language* that programmer use to write their tests and to help who will read the tests later on.

### A Dual Standard

The nature of the dual standard that there are things that you might never do in a production environment that are perfectly fine in a test environment, usually they involve issues of memory or CPU efficiency, but *never* involve issues of clarity.

For example:

```java
public String getState() {
    String state = "";
    state += heater ? "H" : "h";
    state += blower ? "B" : "b";
    state += cooler ? "C" : "c";
    state += hiTempAlarm ? "H" : "h";
    state += loTempAlarm ? "L" : "l";
    return state;
}
```

`StringBuffer`s are a bit ugly. Even in production code I will avoid them if the cost is small; and you could argue that the cost of the code is very small. However, this application is clearly an embedded real-time system, and it is likely that computer and memory resources are very constrained. The test environment, however, is not likely to be constrained at all. That is the mean of dual standard.

### One Assert per Test

There are a school of thought that says that every function in the test should have only one an only one assert statement, this rule might seem cruel, but it forces you to create many small tests, each of which focuses on one behavior. But a better rule that minimize the number of asserts as much as possible, a better rule is in the following subsection.

### Single Concept per Test

Perhaps a better rule is that we just want to test a single concept in each test function. We don't want a function that tests one thing after another although it can be broke down to a multiple functions.

It is not the number of the asserts in one test that causes the problem, Rather it is the fact that there is more than one concept being tested, so probably the best rule is that you should minimize the number of asserts per concept and just test one concept per test function.

## F.I.R.S.T

Clean tests follow five other important rules:

1. **Fast**: Tests should be fast, if tests are slow, then you won't run them frequently, and if you don't run them frequently, they won't help you find the problems quickly and the codebase will start to rot.

2. **Independent**: Tests should not depend on each other nor set up conditions for the next test, you should be able to write them independently and in any order you like, when tests depend on each other, it make the diagnosis difficult.

3. **Repeatable**: Tests should be repeatable in any environment, you should be able to run the tests in the production environment, in the development environment, in the testing environment, on you laptop while riding home on the train without a network. If your tests are not repeatable, then you have an excuse for why they fails. You also will not be able to run the tests when the environment is not available.

4. **Self-Validating**: The tests should have a boolean output. Either they pass or fail. You should not have to read through the test logs to tell whether the tests pass. You should not have to manually compare text files to see whether the tests pass, if they are not self validating, then failure can become subjective and running the tests requires a long manual evaluation.

5. **Timely**: The tests should be written just before the production code that makes them pass. If you write tests after the production code, then you may find the production code to be hard to test. You may decide that some tests are not worth writing because the production code is too hard
