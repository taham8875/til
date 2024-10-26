# Chapter 7 - Error Handling

## Use Exceptions Rather Than Return Codes

Returning error codes is a bad practice. It makes the code harder to read and understand. It also makes the code more verbose and harder to maintain and add layers of indentation trying to execute code in else/if levels. Instead of returning error codes, you should use exceptions.

## Write Your Try-Catch-Finally Statement First

One of the most interesting things about exceptions is that they define a scope within your program. When you execute code in the `try` portion of a `try-catch-finally` statement, you are stating that execution can abort at any point and then resume at the `catch`.

In a way, `try` blocks are like transactions. Your `catch` has to leave your program in a consistent state, no matter what happens in the `try`. For this reason it is good practice to start with a `try-catch-finally` statement when you are writing code that could throw exceptions. This helps you define what the user of that code should expect, no matter what goes wrong with the code that is executed in the `try`.

## Provide Context with Exceptions

There are many ways to classify errors. We can classify them by their source: Did they come from one component or another? Or their type: Are they device failures, network failures, or programming errors? However, when we define exception classes in an application, our most important concern should be *how they are caught*

This is poor exception classificaiton:

```java
ACMEPort port = new ACMEPort(12);
try {
  port.open();
} catch (DeviceResponseException e) {
  reportPortError(e);
  logger.log("Device response exception", e);
} catch (ATM1212UnlockedException e) {
  reportPortError(e);
  logger.log("Unlock exception", e);
} catch (GMXError e) {
  reportPortError(e);
  logger.log("GMX error", e);
} finally {
  ...
}
```

There are a lot of duplication, and we should not be surprised, in most exception handling situations, the work we do is pretty standard regardless of the exception reason.

We simplify the code by wrapping the api we are calling and make sure that it returns a common exception type:

```java
LocalPort port = new LocalPort(12);
try {
  port.open();
} catch (PortDeviceFailure e) {
  reportError(e);
  logger.log(e.getMessage(), e);
} finally {
  ...
}
```

Our `LocalPort` class wraps the `ACMEPort` class and translates the exceptions into a common exception type. This way, the client code does not have to know about the `ACMEPort` class or the exceptions it throws.

```java
public class LocalPort {
  private ACMEPort innerPort;

  public LocalPort(int portNumber) {
    innerPort = new ACMEPort(portNumber);
  }

  public void open() {
    try {
      innerPort.open();
    } catch (DeviceResponseException e) {
      throw new PortDeviceFailure(e);
    } catch (ATM1212UnlockedException e) {
      throw new PortDeviceFailure(e);
    } catch (GMXError e) {
      throw new PortDeviceFailure(e);
    }
  }
}
```

Wrappers like the one we defined for `ACMEPort` can be very useful. In fact, wrapping third-party APIs is a best practice. When you wrap a third-party API, you minimize your dependencies upon it: You can choose to move to a different library in the future without much penalty. Wrapping also makes it easier to mock out third-party calls when you are testing your own code. You are not tightly bound to the vendor’s API.

## Define the Normal Flow

Don't use the exception handling for the normal flow of your program. Exceptions should be exceptional. If you are using exceptions for the normal flow of your program, you are doing it wrong.

consider the following code:

```java
try {
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
} catch (MealExpensesNotFound e) {
    m_total += getMealPerDiem();
}
```

In this business, if meals are expensed, they become part of the total. If they are not expensed, the employee gets a per diem. This is not an exceptional circumstance; it is a normal flow of control. The exceptions should not define the logic. The code should be written without exceptions:

```java
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

and change the `ExpenseReportDAO` to return a `MealExpenses` object that returns a per diem when there are no expenses.

```java
public class PerDiemMealExpenses implements MealExpenses {
    public int getTotal() {
        // return the per diem default
    }
}
```

## Don't Return Null

Most of things that invites errors is returning `null`. If you return `null`, you have to check for it and anytime you don't check for it you have the probability of getting a `NullPointerException`.

```
public void registerItem(Item item) {
    if (item != null) {
        ItemRegistry registry = peristentStore.getItemRegistry();
        if (registry != null) {
            Item existing = registry.getItem(item.getID());
            if (existing.getBillingPeriod().hasRetailOwner()) {
                existing.register(item);
            }
        }
    }
}
```

If you see this code, you might say that the problem no checking for null in the second line of that nested if statement, but not, the real problem that there is *too many* null checks.

If you tempted to return `null` from a method, consider throwing an exception or returning a special case object instead. If you are calling a null-returning method from a third-party API, consider wrapping that method with a method that either throws an exception or returns a special case object.

In many cases, special values are easy solution, imagine that you have this code:

```java
List<Employee> employees = getEmployees();
if (employees != null) {
    for (Employee e : employees) {
        totalPay += e.getPay();
    }
}
```

Right now, `getEmployees` can return `null`, but does it have to? If we change `getEmployee` so that it returns an empty list, we can clean up the code:

```java
List<Employee> employees = getEmployees();
for (Employee e : employees) {
    totalPay += e.getPay();
}
```

Fortunately, Java has `Collections.emptyList()`, and it returns a predefined immutable list that we can use for this purpose:

```java
public List<Employee> getEmployees() {
    if (/* we have no employees */) {
        return Collections.emptyList();
    }
}
```

Doing this will minimize `NullPointerException` and make the code cleaner.

## Don't Pass Null

Returning `null` from methods is bad, but passing `null` into methods is worse. Unless you are working with an API which expects you to pass `null`, you should avoid passing `null` in your code whenever possible.

```java
public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
        return (p2.x - p1.x);
    }
}
```

What will happen if someone passes `null` as an argument?

```java
calculator.xProjection(null, new Point(12, 13));
```

The code will throw a `NullPointerException`.

To fix this we can create a new type of exception and throw it when `null` is passed as an argument:

```java
public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
        if (p1 == null || p2 == null) {
            throw InvalidArgumentException("Invalid argument for MetricsCalculator.xProjection");
        }
        return (p2.x - p1.x);
    }
}
```

Is this better? It might be a little better than a `null` pointer exception, but remember, we have to define a handler for `InvalidArgumentException`. What should the handler do? Is there any good course of action? It is an additional effort to handle this exception.

There is another alternative, We could use a set of assertions:

```java
public class MetricsCalculator {
    public double xProjection(Point p1, Point p2) {
        assert p1 != null : "p1 should not be null";
        assert p2 != null : "p2 should not be null";
        return (p2.x - p1.x);
    }
}
```

It is a good documentation but it does not solve the problem. If someone passes a `null` we will get a runtime error.

In most programming languages there is no good way to deal with a `null` that is passed by a caller accidentally. Because this is the case, the rational approach is to forbid passing `null` by default. When you do, you can code with the knowledge that a `null` in an argument list is an indication of a problem, and end up with far fewer careless mistakes.
