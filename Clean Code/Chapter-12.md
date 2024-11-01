# Chapter 12 - Emergence

## Getting Clean via Emergent Design

There are four simple rules that you could follow that would help you create good design as you work, a software design is simple if it follows these rules:

1. Runs all the tests
2. Contains no duplication
3. Expresses the intent of the programmer
4. Minimizes the number of classes and methods

## Simple Design Rule 1: Runs All the Tests

Systems that aren't testable aren't verifiable, a system that cannot be verified should never be deployed

Fortunately, making our systems testable pushes us toward a design where our classes are small and single purpose, it is just easier to test a class the follows the single responsibility principle. The more tests are written the more we push toward things that are simpler to test.

## Simple Design Rule 2-4: Refactoring

Once we have tests, we are empowered to keep our code and classes clean, we do this by incrementally refactoring our code. The fact that we have tests eliminates the fear that cleaning up the code will break it.

## No Duplication

Duplication is the enemy of a well designed system, it represents additional work, additional risk, and additional unnecessary complexity. Duplication can be seen in many forms, like lines of code that looks like the same, or duplication of implementation details, for example:

```java
int size() {}
boolean isEmpty() {}
```

We could have separated the implementation of these to methods, we can eliminate this duplication by tying `isEmpty` to `size`:

```java
boolean isEmpty() {
    return 0 == size();
}
```

## Expressive

It is easy to write code that we understand, because of while writing it we had a deep understand of the problem, but other maintainers of the code may not have the same understanding, so we should write code that is expressive, that expresses the intent of the programmer.

The majority of work in software development is in long term maintenance, so try to express your intent as clearly as possible.

You can express yourself by choosing good name, keeping functions and classes small or by using standard nomenclature. Design patterns for example are largely about communication, and expressiveness, by using standard pattern names, for example `Command` or `Visitor`, you implicitly communicate the structure of your code to other developers.

Well written unit tests are also expressive, a primary goal of tests is to acts as a documentation, when someone reads the test, they should understand the intent of the class it is testing.

But the most important way to be expressive is to try, all often we get our code working and then move on to the next problem without giving sufficient thought to how we make the code easier for the next person to read.

## Minimal Classes and Methods

This rule suggests that we keep our function and class count low.

Our goal is to keep the overall system small while we are also keeping our functions and classes small, however that rule is the lowest priority of the four rules of a simple design, so if there are a trade off, having tests, eliminating duplication, and expressing intent are more important than minimizing the number of classes and methods.
