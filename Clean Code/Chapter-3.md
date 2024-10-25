# Chapter 3 - Functions

## Small

The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that. Functions should not be 100 lines long. They should be 20 lines long. Or even shorter!

## Do One Thing

Functions should do one thing. They should do it well. They should do it only.

## One Level of Abstraction per Function

To make sure our functions are doing "one thing", we need to make sure that the statements within our function are all at the same level of abstraction. Mixing levels of abstraction within a function is always confusing.

## Switch Statements

Switch statements always do N things. They can be hidden by abstracting them into a factory or a strategy.

## Function Arguments

The ideal number of arguments for a function is zero (niladic). Next comes one (monadic), followed closely by two (dyadic). Three arguments (triadic) should be avoided where possible. More than three (polyadic) requires very special justification - and then shouldn't be used anyway.

Arguments are hard, they take a lot of conceptual power.

### Flag Arguments

Flag arguments are ugly. Passing a boolean into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. It does one thing if the flag is true and another if the flag is false!

If this is the case, separate the function into two functions and name them accordingly.

### Argument Object

You can use an argument object when a function seems to need more than two or three arguments.

```java
public class Circle {
    public void makeCircle(double x, double y, double radius) {
        // ...
    }
}
```

```java

public class Circle {
    public void makeCircle(Point center, double radius) {
        // ...
    }
}
```

## Have No Side Effects

Side effects are lies, your function promises to do one thing, but it also does other *hidden* things.

Consider the following function the matches the username and password.

```java
public class UserValidator {
    private Cryptographer cryptographer;

    public boolean checkPassword(String userName, String password) {
        User user = UserGateway.findByName(userName);
        if (user != User.NULL) {
            String codedPhrase = user.getPhraseEncodedByPassword();
            String phrase = cryptographer.decrypt(codedPhrase, password);
            if ("Valid Password".equals(phrase)) {
                Session.initialize();
                return true;
            }
        }
        return false;
    }
}
```

This side effect here is calling `Session.initialize()`. The `checkPassword` function by its name says that it checks the password, but it also initializes the session. Developer how executes this function runs the risk of erasing the existing session data when he thinks he is just checking the password.

If you must do this, rename your function to indicate that the function does more than just check the password, for example, `checkPasswordAndInitializeSession`. But note that violates "Do One Thing" rule. So it is better to separate the function into two functions.

## Output Arguments

Arguments are most naturally interpreted as inputs to the function, for example:

```java
appendFooter(s);
```

Does this functions appends `s` as the footer to something? Or does it append a footer to `s`? Is `s` an input or an output?

If you see the declaration of the function as:

```java
public void appendFooter(StringBuffer report)
```

This clarifies that it does but it required a check of the declaration, anything the forces you to check the function signature should be avoided.

In days before object-oriented programming, output arguments were occasionally used to allow a function to return multiple values. But that is no longer necessary. In the object-oriented world, `this` object can be used as output arguments. In other words, it would be better for `appendFooter` to be invoked as that:

```java
report.appendFooter();
```

## Command Query Separation

Functions should either do something or answer something, but not both. Either your function should change the state of an object, or it should return some information about that object. Doing both often leads to confusion.

For example, consider this function:

```java
public boolean set(String attribute, String value);
```

This function sets the value of a named attribute and returns true if it is successful and false if no such attribute exists. This leads to off statements like this:

```java
if (set("username", "unclebob"))...
```

Imagine reading that, what that means? Is it asking if the `username` attribute was previously set to `unclebob`? Or is it setting the `username` attribute to `unclebob` and doing something if so?

The author intended `set` ti be verb to execute something, but in the context of `if` statement it may *feel* like an adjective. So someone may read this as "If the username is previously set to unclebob" and not "set the username to unclebob".

To solve this you may rename the `set` function to `setAndCheckIfExists` or but this violates the "Do One Thing" rule. So it is better to separate the function into two functions.

```java
if (attributeExists("username")) {
    setAttribute("username", "unclebob");
}
```

## Prefer Exceptions to Returning Error Codes

Returning error codes from command functions violates the rule of command query separation.

```java
if (deletePage(page) == E_OK)
```

if doesn't suffer from verb/adjective confusion but does lead to deeply nested structures. When you return error code, you create the problem that the caller must deal with the error immediately. This can lead to a lot of nested `if` statements.

```java
if (deletePage(page) == E_OK) {
    if (registry.deleteReference(page.name) == E_OK) {
        if (configKeys.deleteKey(page.name.makeKey()) == E_OK) {
            logger.log("page deleted");
        } else {
            logger.log("configKey not deleted");
        }
    } else {
        logger.log("deleteReference from registry failed");
    }
} else {
    logger.log("delete failed");
    return E_ERROR;
}
```

on the other hand, if you use exceptions instead of returned code errors, then the error handling code can be separated from the happy path code.

```java
try {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
} catch (Exception e) {
    logger.log(e.getMessage());
}
```

### Extract Try/Catch Blocks

If you have a lot of try/catch blocks, you should extract the bodies of the try and catch blocks out into functions of their own.

```java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    } catch (Exception e) {
        logger.log(e.getMessage());
    }
}

private void deletePageAndAllReferences(Page page) {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}
```

## Error Handling is One Thing

Functions should do one thing. Error handling is one thing. Thus, a function that handles errors should do nothing else.

So if the word `try` exists in a function, it should be the very first word in the function and that there should be nothing after the `catch/finally` blocks.
