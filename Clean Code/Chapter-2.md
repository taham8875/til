# Chapter 2 - Meaningful Names

## Use Intention-Revealing Names

Choose good names the reveal intent. The name of a variable, function, or class should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

```java
int d; // elapsed time in days
```

The name `d` just tells us nothing about what the variable is for. A better name would be:

- `int elapsedTimeInDays;`
- `int daysSinceCreation;`
- `int daysSinceModification;`
- `int fileAgeInDays;`

Bad names make it hard to understand and change the code, what is the purpose of the following code snippet?

```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x);
  return list1;
}
```

Note that it is a simple code, no complex expression,, ,there are only three variables and two constants and the logic is pretty straightforward, no fancy classes or polymorphic methods.

1. What kinds of things are in `theList`?
2. What is the meaning of the zeroth subscript of an item in `theList`?
3. What is the meaning of the value 4?
4. What would i do with the list that is returned?

Let's refactor the code with meaningful names:

```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>();
  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE] == FLAGGED)
      flaggedCells.add(cell);
  return flaggedCells;
}
```

Notice that the code is not changed, only the names are changed. The code is now self-explanatory, no need for comments.

We can go further and make a simple class for the cells instead of using an array of integers and include an intention-revealing function (called `isFlagged`) to hide the magic numbers.

```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<Cell>();
  for (Cell cell : gameBoard)
    if (cell.isFlagged())
      flaggedCells.add(cell);
  return flaggedCells;
}
```

With this change, it is not difficult to understand what's going on.

## Avoid Disinformation

Avoid leaving false clues that obscure the meaning of the code, we should avoid words whose entrenched meanings vary from our intended meaning. For example, `hp`, `aix`, `sco` are not good names for variables because they are names of unix platforms or variants, even if your are coding a hypotenuse and `hp` is a good name for it, it is better to use `hypotenuse` instead of `hp` because it could be disinformation.

Don't refer to a grouping of accounts as an `accountList` unless it's actually a `List`. The word `List` means something specific to programmers. If the container holding the accounts is not actually a `List`, it may lead to false assumptions. Name it `accountGroup` or `bunchOfAccounts` or just `accounts`. (And no need to include the `List` word at all, we are have LSPs for that)

Beware of using names which vary in small ways, it is easy to misread `XYZControllerForEfficientHandlingOfStrings` as `XYZControllerForEfficientStorageOfStrings`. The names are different by only one word and can be easily confused.

## Make Meaningful Distinctions

Here is the case, you are trying to satisfy the compiler, you can't name two variables with the same name, so you add the noisy words to make them different, like `ProductInfo` and `ProductData`. The names are different, but they are not meaningful. The difference between `Info` and `Data` is not a distinction that registers in the mind. `Info` and `Data` are indistinct noise words like `a`, `an`, and `the`.

Bad names:

- `copyChars(a1, a2);`
- `productInfo();` and `productData();` (what is the difference?)
- `getActiveAccount();` and `getActiveAccountData();` (what is the difference?)
- `NameString` and `Name` Why? we have IDEs that can tell us the type of the variable, we don't need to include the type in the name.

Good names:

- `copyChars(source, destination);`
- `retrieveCustomer();` and `retrieveCustomerAddress();`

## Use Pronounceable Names

Our brains are good with words, why don't we make a use for that, also programming is a social activity, image you have a `genymdhms` variable which generates a date with year, month, day, hour, minute, and second, and you tell your coworker (gen why emm dee aich emm ess).

convert

```java
class DtaRcrd102 {
  private Date genymdhms;
  private Date modymdhms;
  private final String pszqint = "102";
}
```

to

```java
class Customer {
  private Date generationTimestamp;
  private Date modificationTimestamp;
  private final String recordId = "102";
}
```

## Use Searchable Names

Single-letter names and numeric constants have a particular problem in that they are not easy to locate across a body of text.

Imagine searching for `MAX_CLASSES_PER_STUDENT` and finding it vs searching for the `7` number across the code base.

Bad code

```java
for (int j = 0; j < 34; j++) {
  s += (t[j] * 4) / 5;
}
```

Good code

```java
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;
for (int j = 0; j < NUMBER_OF_TASKS; j++) {
  int realTaskDays = taskEstimate[j] * realDaysPerIdealDay;
  int realTaskWeeks = (realDays / WORK_DAYS_PER_WEEK);
  sum += realTaskWeeks;
}
```

## Avoid Encodings

Avoid encoding information in names, it is redundant and unnecessary, we have types and classes for that.

### Hungarian Notation

Hungarian Notation was necessary in the old days of programming, when the IDEs were not as powerful and the compilers was not able to type check before compilation, but now it is not necessary.

Modern IDEs and LSPs make it easy to find the type of a variable, so there is no need to include the type in the name.

Nowadays, HN can be distracting, there is a trend toward smaller classes and shorter functions so that people can easily see the point of declaration of each variable they are using.

Also HN can be misleading when the type of a variable updates but not the variable name, consider this code change:

```java
- String phoneString;
+ PhoneNumber phoneString; // name not changed when type changed!
```

New developer may be confused by the name `phoneString` when it is actually a `PhoneNumber`.

### Member Prefixes

avoid prefixing class members with the `m_`

```java
class Part {
  private String m_dsc; // The textual description
  void setName(String name) {
    m_dsc = name;
  }
}
```

It is unnecessary, we have IDEs that can tell us the type of the variable, we don't need to add clutter and noise to the code, it will be a marker of an older codebase.

```java
class Part {
  private String description;
  void setDescription(String description) {
    this.description = description;
  }
}
```

### Avoid Mental Mapping

Reader shouldn't have to mentally translate your names into another names they already know. This problem arises when you choose to use neither problem domain terms nor solution domain terms.

Avoid using single letter variables for anything other than simple loops, like `i`, `j`, `k` for loops, but for anything else, use meaningful names.

One difference between a smart programmer and a professional programmer is that the professional understands that clarity is king. Professionals use their powers for good and write code that others can understand.

### Class Names

Class names and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`. Avoid words like `Manager`, `Processor`, `Data`, or `Info` in the name of a class. A class name should not be verb.

### Method Names

Methods should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`. Accessors, mutators, and predicates should be named for their value and prefixed with `get`, `set`, and `is`.

```java
String name = employee.getName();
customer.setName("Martin");
if (paycheck.isPosted())...
```

When constructors are overloaded, use static factory methods with names that describe the arguments.

```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

is generally better than

```java
Complex fulcrumPoint = new Complex(23.0);
```

Consider enforcing their use by making the corresponding constructors private.

## Don't be Cute

If names are so clever, they will be only memorable and recognizable for people who share yours sense of humor, just don't do it.

- `whack()` is a bad name for a function that kills a process, use `kill()`.
- `HolyHandGrenade` is a bad name for a class that handles configuration files, use `Configuration`.
- `eatMyShorts()` is a bad name for a function that throws an exception, use `throwException()`.

## Pick One Word per Concept

Pick one word for one abstract concept and stick with it, it is confusing to have `fetch`, `retrieve`, and `get` as equivalent methods of different classes.

## Don't Pun

Don't use the same word for two purposes, using the same word for two different purposes is essentially a pun.

If you have an add method, that sums two numbers, don't name a method that adds an element to a collection `add`, use `insert` or `append` instead.

## Use Solution Domain Names

Remember that the people who read your code will be programmers, so go ahead and use computer science (CS) terms, algorithm names, pattern names, math terms, and so forth. It is not wise to draw every name from the problem domain because you have to run back and forth to the customer asking them what every name means.

The name `AccountVisitor` is better means a great deal to a programmer who is familiar with the Visitor pattern. What programmer would not know what a `JobQueue` was? Or a `PriorityQueue` is?

## Use Problem Domain Names

When there is no "programmer-eese" for what you're doing, use the name from the problem domain. At least the programmer who maintains your code can ask a domain expert what it means.

## Add Meaningful Context

There are a few names that are meaningful in and of themselves, most are not, instead you need to place names in context for future readers by enclosing them i well names classes, functions or namespaces, when all else fails, then prefixing the name may be necessary as a last resort.

Imagine that you have variables named `firstName`, `lastName`, `street`, `houseNumber`, `city`, `state`, and `zipcode`. Taken together it is pretty clear that they form an address. But what if you just saw the state variable being used alone in a method? Would you automatically infer that it was part of an address? or may you understand that it is a state of a process or a state of a machine?

You can add context by using prefixes: `addressFirstName`, `addressLastName`, `addressStreet`, `addressHouseNumber`, `addressCity`, `addressState`, and `addressZipCode`.

But a better solution is to create a class called `Address` and use it to hold the address fields. Then even the compiler will know that these variables belongs to a bigger concept.

Consider the method in the following code. Do the variables need a more meaningful context? The function name provides only part of the context; the algorithm provides the rest. Once you read through the function, you see that the three variables, `number`, `verb`, and `pluralModifier`, are part of the “guess statistics” message. Unfortunately, the context must be inferred. When you first look at the method, the meanings of the variables are opaque.

```java
private void printGuessStatistics(char candidate, int count) {
  String number;
  String verb;
  String pluralModifier;
  if (count == 0) {
    number = "no";
    verb = "are";
    pluralModifier = "s";
  } else if (count == 1) {
    number = "1";
    verb = "is";
    pluralModifier = "";
  } else {
    number = Integer.toString(count);
    verb = "are";
    pluralModifier = "s";
  }
  String guessMessage = String.format(
    "There %s %s %s%s", verb, number, candidate, pluralModifier
  );
  print(guessMessage);
}
```

The function is too long, to split the function into smaller pieces we need to create a `GuessStatisticsMessage` class and move the variables and the method to it.

```java
class GuessStatisticsMessage {
  private String number;
  private String verb;
  private String pluralModifier;

  public String make(char candidate, int count) {
    createPluralDependentMessageParts(count);
    return String.format(
      "There %s %s %s%s", verb, number, candidate, pluralModifier
    );
  }

  private void createPluralDependentMessageParts(int count) {
    if (count == 0) {
      thereAreNoLetters();
    } else if (count == 1) {
      thereIsOneLetter();
    } else {
      thereAreManyLetters(count);
    }
  }

  private void thereAreManyLetters(int count) {
    number = Integer.toString(count);
    verb = "are";
    pluralModifier = "s";
  }

  private void thereIsOneLetter() {
    number = "1";
    verb = "is";
    pluralModifier = "";
  }

  private void thereAreNoLetters() {
    number = "no";
    verb = "are";
    pluralModifier = "s";
  }
}
```

## Don't Add Gratuitous Context

In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with `GSD`, like `GSDAccountAddress`, `GSDCustomerName`, and `GSDGasPump`. The `GSD` prefix is pointless and just adds noise. The application is called “Gas Station Deluxe,” so it is pretty clear that the classes are part of it.

If types `G` and pressed the `ctrl + space` key for autocompletion, good luck, the IDE will show you every single class in the application.

Shorter names are generally better than longer ones, so as long as they are clear, no need to add more context into an already clear name.
