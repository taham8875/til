# Chapter 17 - Smells and Heuristics

## Comments

### C1: Inappropriate Information

Examples of inappropriate information to be included in the comments:

- Change histories (use version control)
- Metadata like authors
- Last modified dates (seriously why someone would do that?)

### C2: Obsolete Comment

Comments can be old and obsolete, if you found and obsolete comment, delete it or update it as soon as possible.

### C3: Redundant Comment

A comment is redundant if it describes something that is already clear from the code itself.

```java
i++; // increment i
```

Another example is this javadoc that says nothing more than the method signature:

```java
/**
 * @param list
 * @return the size of the list
 * @throws Exception
 */
public int size(List list) throws Exception {
    /// ...
}
```

Comments should says what the code cannot say itself.

### C4: Poorly Written Comment

Choose the words carefully, use correct grammar and punctuation. Write it well and make it worth to read.

### C5: Commented-Out Code

When you see commented-out code, delete it. Don't worry. Source control will remember it for you.

## Environment

### E1: Build Requires More Than One Step

Building a project should be a single trivial operation.

### E2: Tests Require More Than One Step

You should be able to run all your unit test with a single command.

## Functions

### F1: Too Many Arguments

No argument is best, one is good, two is acceptable, three is questionable, and four or more is unacceptable.

### F2: Output Arguments

Output arguments are counterintuitive. Readers expect arguments to be inputs, not outputs. If your function must change the state of something, have it change the state of the object it is called on.

### F3: Flag Arguments

Boolean arguments loudly declare that the function does more than one thing. They are confusing.

### F4: Dead Function

If a function is never called, consider deleting it.

## General

### G1: Multiple Languages in One Source File

### G2: Obvious Behavior Is Unimplemented

Following the principle of least surprise, any function or class should implement the behavior that another programmer would expect.

For example, consider a function that translate the name of a day to an enum:

```java
Day day = Day.fromName("Monday");
```

We would expect the string Monday to be translated to the enum `Day.MONDAY`. We also expecting handling common abbreviations and to ignore the case of the string.

When an obvious behavior is unimplemented, readers and users of the code can no longer depend on their intuition about the naming, they lose their trust in the original author and fall back on reading the details of the code.

### G3: Incorrect Behavior at the Boundaries

Don't rely on your intuition, write a test for every boundary condition and every corner case.

### G4: Overridden Safeties

Chernobyl melted down because the plant manager overrode each of the safety mechanisms one by one. The safeties were making it inconvenient to run an experiment. The result was that the experiment did not get run, and the world saw it’s first major civilian nuclear catastrophe

Don't ignore compiler warnings, ignoring them may get the build to succeed but may end you with countless hours of debugging. Turning of failing tests is even worse.

### G5: Duplication

Find and eliminate duplication wherever you can.

### G8: Too Much Information

Concentrate on keeping interfaces very tight and very small. Help keep coupling low by limiting information.

Well defined modules have very small interfaces that allow you to do a lot with a little.

Limit what you expose at the interface of the class on methods, the fewer methods a class has, the better, the fewer variables a function has, the better, the fewer instance variables a class has, the better.

### G9: Dead Code

Dead code is code that isn't executed. You find it in the body of an if statement that checks for a condition the never happens. You find it in a catch block of a try the never throws, in switch statements that never occur or in utility methods that are never called.

### G10: Vertical Separation

Variables and function should be defined close to where they are used. Local variables should be declared just above their first usage. Private functions should be declared just below their first usage.

### G11: Inconsistency

If you are doing something in a certain way, do all similar things in the same way.

### G15: Selector Arguments

It is hard to deal with function with dangling `false` argument at the end of a function call, what does it mean? What will happen it it were `true`?

Functions with such arguments does more than one thing, they are confusing and should be avoided. This is a lazy way to avoid splitting a large function into several smaller functions, consider:

```java
public int calculateWeeklyPay(boolean overtime) {
    int tenthRate = getTenthRate();
    int tenthsWorked = getTenthsWorked();
    int straightTime = Math.min(400, tenthsWorked);
    int overTime = Math.max(0, tenthsWorked - straightTime);
    int straightPay = straightTime * tenthRate;
    double overtimeRate = overtime ? 1.5 : 1.0 * tenthRate;
    int overtimePay = (int)Math.round(overTime*overtimeRate);
    return straightPay + overtimePay;
}
```

It is very bad having to remember what `calculateWeeklyPay(false)` means, it is better to break it into several readable functions:

```java
public int straightPay() {
    return getTenthsWorked() * getTenthRate();
}

public int overTimePay() {
    int overTimeTenths = Math.max(0, getTenthsWorked() - 400);
    int overTimePay = overTimeBonus(overTimeTenths);
    return straightPay() + overTimePay;
}

private int overTimeBonus(int overTimeTenths) {
    double bonus = 0.5 * getTenthRate() * overTimeTenths;
    return (int) Math.round(bonus);
}
```

Of course selector arguments are not only booleans, they can be enums, integers, strings, etc.

### G16: Obscured Intent

We want the code to be as expressive as possible, bad naming, run-on expressions, Hungarian notation and magic numbers all obscure the intent of the code.

### G17: Misplaced Responsibility

One of the most important decisions a software developer can make is where to put code. For example, where should the `PI` constant go, `Math`, `Circle`, `Trigonometry` class?

The principle of least surprise comes into play here. Code should be place where the reader would naturally expect it to be. (Personally I find `Math` is the most intuitive place)

### G19: Use Explanatory Variables

### G20: Function Names Should Say What They Do

Look at this code

```java
Date newDate = date.add(5);
```

What this functions do? does it add 5 days, 5 months, 5 years? Is the `date` instance changed or does the function just return a new `Date` without changing the old one? *You can't tell from the call what the function does.*

If the function adds five days to the date an changes the date, then it should be called `addDaysTo` or `increaseByDays`. If on the other hand, the function returns a new date that is five days later but not changes the date instance, it should be called `daysLater` or `daysSince`.

If you have to look to the implementation or the documentation of the function to know what is does, then you should work to find a better name or refactor the functionality so it can be placed in functions with better names.

### G25: Replace Magic Numbers with named Constants

Magic numbers are numbers that are used without explanation. They are bad because they are hard to understand, they require the reader to remember what they mean, and they are a sign that the code should be refactored.

For example, the number 86400 should be hidden behind the constant `SECONDS_PER_DAY`. If you are printing 55 lines per page, then the constant 55 should be hidden behind `LINES_PER_PAGE`. constant.

Some constant are so easy to recognize that they don't always need a named constant to hide behind as long as they are used in a self explanatory context, for example:

```java
double milesWalked = feetWalked/5280.0;
int dailyPay = hourlyRate * 8;
double circumference = radius * Math.PI * 2;
```

Do we really need the constants `FEET_PER_MILE`, `WORK_HOURS_PER_DAY`, and `TWO` in the above examples? Clearly no. They numbers in that context are self explanatory.

Constant like 3.141592653589793 are also well known and easy recognizable, however, the chances of error are great because errors can happen (can you spot the error in 3.1415927535890793?) also people may use different values like 3.14, 3.14159, 3.142, 22/7, etc. So it is better to hide it behind `Math.PI`.

The term magic numbers does not apply only to numbers, they also apply to any token that has a value that is not self descriptive, for example:

```java
assertEquals(7777, Employee.find(“John Doe”).employeeNumber());
```

There are two magic numbers here, the first one is obviously 7777 and second one is the string “John Doe”.

It turns out that these values are for well known test data created by the team, for better readability of the code, the first one should be hidden behind a constant like `HOURLY_EMPLOYEE_ID` and the second one should be hidden behind a constant like `HOURLY_EMPLOYEE_NAME`.

### G26: Be precise, Not Lazy

When you make decisions in you code, make sure that you are making it precisely, know why you have made it and how will deal with any expectations. Don't be lazy about the decisions you make.

Examples for lazy not precise decisions:

- Expecting the first match to be the only match to a query
- Using floating point numbers for money
- Avoid locks or transactions management
- Making all variables protected by default

Examples for precise decisions:

- When you decide to call a function that might returns null, make sure you check for null before using the result
- Make sure that you code checks for all possible matches in a query not only the first one
- If you need to deal with currency, use integers and deal with the rounding properly
- If there are any chance of concurrent updates, make sure you use locking mechanism

### G28: Encapsulate Conditionals

Boolean values are hard enough to understand

Extract functions that explain the intent of the condition

For example:

```java
if (shouldBeDeleted(timer))
```

is preferable to

```java
if (timer.hasExpired() && !timer.isRecurrent())
```

### G29: Avoid Negative Conditionals

Negative conditions are harder to understand than positive ones. For example:

```java
if (buffer.shouldCompact())
```

is preferable to

```java
if (!buffer.shouldNotCompact())
```

### G30: Functions Should Do One Thing

Functions should do one thing. They should do it well. They should do it only.

### G36: Avoid Transitive Navigation

In general we don't want a single module to know much about its collaborators, we don't want something like `a.getB().getC().doSomething()`.

This is called the Law of Demeter. Some people call it Writing Shy code, In either cases it comes down to making sure that modules know only about their immediate collaborators and do not know the navigation map of the whole system.

## Names

### N1: Choose Descriptive Names

### N3: Use Standard Nomenclature Where Possible

Names are easier to understand if they are based on existing convention or usage, for example, if you are using the decorator pattern, you should use the suffix `Decorator` for the decorator classes. For example `AutoFocusDecorator`, `WideAngleDecorator`, etc.

Patterns are example of standard, there are another patterns, like in java, a function the converts objects to string representation are often names `toString`, it is better to follow common conventions rather than inventing your own.

### N5: Use Long Names for Long Scopes

The longer the scope of the name, the longer and more precise the name should be.

Names like `i` is fine if only the scope is small (like 5 lines) and is in a loop (where `i` is a common convention for loop counters), but if the scope is larger, the name should be more descriptive.

### N6: Avoid Encodings

Such things like Hungarian notation.

### N7: Names Should Describe Side-Effects

Names should describe everything that a function, variable or class does, don't hide side effects with a name. For example:

```java
public ObjectOutputStream getOos() throws IOException {
    if (m_oos == null) {
        m_oos = new ObjectOutputStream(m_socket.getOutputStream());
    }
    return m_oos;
}
```

This function does a bit more that get an oos, it creates it if there hasn't been created, thus, a better name is `createOrReturnOos`.

## Tests

### T1: Insufficient Tests

How many tests should be in the test suite? Unfortunately, a metric many programmers use is the seems like enough.

A test suite should test everything that could possibly break, the tests are insufficient so long as there are conditions that have not been covered by the tests.

### T2: Use a Coverage Tool

Coverage tools makes it easy to find methods, classes or modules that are insufficiently tested, most IDEs give you a visual indicator marking lines that are covered in green an lines that are not in red, making it easy and quick to find `if` or `catch` statements whose bodies have not been covered.

### T3: Don't Skip Trivial Tests

### T4: An Ignored Test Is a Question about an Ambiguity

Sometimes we are uncertain about a behavioral detail because the requirements are unclear. We can express our question about the requirements as a test that is commented out, or as a test that annotated with `@Ignore`. Which you choose depends upon whether the ambiguity is about something that would compile or not.

### T5: Test Boundary Conditions

### T6: Exhaustively Test Near Bugs

Bugs tends to congregate, when you find a bug in a function, it is wise to do and exhaustive test of the function, you will probably find that the bug was not alone.

### T7: Patterns of Failure Are Revealing

Sometimes you can diagnose a problem by finding patterns in the ways the test fail. This is another argument for making the test cases as complete as possible. Complete tests expose the patterns.

For example suppose you notices that all tests with input larger than five characters fails, or what if a negative numbers passed to the second argument in a function fails, Sometimes just seeing the pattern of red and green on the test report is enough to spark the "Aha" moment leads to a solution.

### T9: Tests Should Be Fast

Slow tests that test won't run, when things get tight, slow tests will be dropped of the suit, do what you can do to make the tests run fast.
