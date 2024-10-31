# Chapter 10 - Classes

## Class Organization

Following the standard java convention, a class should begin with a list of variables. Public static constants, if any, should come first. Then private static variables, followed by private instance variables. There is seldom a good reason to have public variables.

Public functions should follow the list of variables, we like to put the private utils called by a public function right after the public function itself. This follows the stepdown rule and helps the program read like a newspaper article.

### Encapsulation

We like to keep our variables and utility functions private, but we're not religious about it. Sometimes we need to relax encapsulation for the sake of testability. In that case, we make the variable package protected.

## Classes Should Be Small

The first rule of classes that they should be small. The second rule of classes is that they should be smaller than that.

In classes, we count how small a class if by counting the responsibilities.

The name of a class should describe what responsibilities if fulfills, in fact, naming is probably the first way of helping determine class size. If we cannot derive a concise name for a class, then it's likely too large. The more ambiguous the class name, the more likely it has too many responsibilities. For example, `Processor`, `Manager`, `Super`, or `Data` often hint at unfortunate aggregation of responsibilities.

We should be also able to write a brief description of the class in about 25 words, without using the words "if", "and", "or", or "but".

### The Single Responsibility Principle

The Single Responsibility Principle (SRP) states that a class or module should have one, and only one, reason to change. This principle gives us both a definition of responsibility, and a guideline for the class size. Class should have one responsibility, one reason to change.

SRP is one of the most important concepts is OO design. It is also one of the simple concepts to understand, however SRP is the most abused class design principle, we regularly encounter classes that have more than one responsibility.

Getting software to work and making software clean are two very different activities. Don't mix them. We focus of getting our code working more than the organization and cleanliness of the code.

### Cohesion

Classes should have a small number of instance variables, each of the methods of a class should manipulate one or more of those variables. In general, the more variables a method manipulates the more cohesive that method is to its class. A class in which each variable is used by each method is maximally cohesive.

In general it is neither advisable nor possible to create a maximally cohesive class. However, we should attempt to create classes that have high cohesion. When cohesion is high, it means that the methods and variables of the class are co-dependent and hang together as a logical whole.

### Maintaining Cohesion Results in Many Small Classes

The act of breaking large functions into smaller functions often causes a proliferation of small classes.

Consider a large function with many variables declared within it, let's  say you want to extract one small part of that function into a separate function. However, the code you want to extract uses four of the variables declared in the function. Must you pass all of those variables as arguments to the new function?

Not at all~ If we promoted those four variables to instance variables of the class, then we could extract the code without passing any variables at all, great! it would be easy to break the function into smaller functions.

Unfortunately, this also means that our classes lose cohesion because they accumulate more and more instance variables that exist solely to allow a few functions to share them. But wait! If there are a few functions that want to share certain variables, doesn’t that make them a class in their own right? Of course it does. When classes lose cohesion, split them!

So breaking a large function into smaller functions often gives us the opportunity to split the large class into smaller classes.
