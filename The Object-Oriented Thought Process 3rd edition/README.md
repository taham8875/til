# Chapter 1 - Introduction to Object-Oriented Concepts

- object-oriented (OO) software development has been around since the early 1960s.

- When systems are working fine, in most cases you should not change them, at least not simply for the sake of change.

- Object Wrappers : A wrapper is a class that encapsulates (wraps) the functionality of another class or program.

## Procedural Versus Object-Oriented Programming

- Procedural programming is a programming paradigm, derived from structured programming, based on the concept of the procedure call. Procedures, also known as _routines_, _subroutines_, or _functions_, simply contain a series of computational steps to be carried out.

- Object-oriented programming (OOP) is a programming paradigm based on the concept of "objects", which may contain data, in the form of fields, often known as _attributes_; and code, in the form of procedures, often known as _methods_.

> In OO design, the attributes and behaviors are contained within a single object, whereas in procedural, or structured design, the attributes and behaviors are normally separated.

- Data Hiding : In OO terminology, data is referred to as _attributes_, and behaviors are referred to as _methods_. Restricting access to certain attributes and/or methods is called _data hiding_.

## What Is an Object?

- An object is an instance of a class. Objects are the basic building blocks of an OO system.

### Object data

- An object's data is referred to as its _attributes_. The data stored within an object represents the state of the object.

e.g. A bank account object might have attributes such as account number, balance, and interest rate.

### Object behavior

- An object's behavior is referred to as its _methods_. The methods of an object represent the operations that can be performed on the object.

e.g. A bank account object might have methods such as deposit, withdraw, and calculate interest. Also the getter and setter methods.

For example, in java we can create a class called `BankAccount` and create an object of that class called `myAccount` as follows:

```java
public class BankAccount {

    // Attributes
    private int accountNumber;
    private double balance;
    private double interestRate;

    // constructor
    public BankAccount(int accountNumber, double balance, double interestRate) {
        this.accountNumber = accountNumber;
        this.balance = balance;
        this.interestRate = interestRate;
    }

    // getter and setter methods
    public int getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public double getInterestRate() {
        return interestRate;
    }

    public void setInterestRate(double interestRate) {
        this.interestRate = interestRate;
    }

    // methods
    public void deposit(double amount) {
        balance += amount;
    }

    public void withdraw(double amount) {
        balance -= amount;
    }

    public void calculateInterest() {
        double interest = balance * interestRate;
        return interest;
    }
}
```

## What Is a Class?

- A class is a blueprint or template that describes the details of an object. (think of a cookie cutter)

- A class is a logical grouping of objects that have common properties and behaviors.

`public` vs `private` vs `protected` vs `default`:

https://stackoverflow.com/questions/215497/in-java-what-is-the-difference-between-public-default-protected-and-private

## Encapsulation and Data Hiding

- Encapsulation is the process of combining data and behavior into a single package called an object.

- Data hiding is the process of hiding the internal data of an object from the outside world.

- Encapsulation and data hiding are closely related. In fact, encapsulation is often referred to as data hiding.

### Interface

- An interface is a collection of abstract methods that has no implementation. An interface is a contract between a class and the outside world.

## Inheritance

- Inheritance is the process by which one class acquires the properties of another class. The class that acquires the properties of another class is called the _subclass_, and the class from which the subclass is derived is called the _superclass_.

- Inheritance is a powerful feature of OO programming because it allows you to reuse existing code without having to rewrite it.

## Abstraction

- Abstraction is the process of hiding the internal details of an object from the outside world. Only the functionality that is necessary to the user should be made available.

- Abstraction is closely related to encapsulation and data hiding.

Abstraction vs Encapsulation:

https://stackoverflow.com/questions/742341/difference-between-abstraction-and-encapsulation

## Is-a Relationships

- Inheritance is often referred to as an _is-a_ relationship. For example, a checking account is a bank account, a savings account is a bank account, and a certificate of deposit (CD) is a bank account. In each case, the subclass inherits the properties of the superclass.

## Polymorphism

- Polymorphism is the ability to take more than one form. In OO programming, polymorphism refers to the ability of an object to provide different behaviors to its users depending on its state.

For example, suppose you have an array of three shapes: a `circle`, a `square`, and a `triangle`. Each shape has a method called `area()` that calculates the area of the shape. The `area()` method is polymorphic because it provides different behavior for each shape.

Here is an example of polymorphism in java:

```java
public abstract class Shape {
    double area;
    public abstract double getArea();
}

public class Circle extends Shape{
    private double radius;
    public Circle(double radius) {
        this.radius = radius;
    }
    @Override
    public double getArea() {
        area = Math.PI * radius * radius;
        return area;
    }
}


public class Square extends Shape {
    private double side;

    public Square(double side) {
        this.side = side;
    }

    @Override
    public double getArea() {
        area = side * side;
        return area;
    }
}


```

You can create an array of shapes and call the `area()` method on each shape as follows:

```java
public class Main {
    public static void main(String[] args) {
        Shape[] shapes = new Shape[2];
        shapes[0] = new Circle(5);
        shapes[1] = new Square(5);
        for (Shape shape : shapes) {
            System.out.println(shape.getArea());
        }
    }
}
```

## Composition

- Composition is the process of combining two or more objects into a more complex object. For example, a car is composed of an engine, a transmission, and a body.

- Composition is often referred to as a _has-a_ relationship. For example, a car has an engine, a transmission, and a body.

Example of composition in java:

```java
public class Engine {
    private String model;
    private int horsepower;

    public Engine(String model, int horsepower) {
        this.model = model;
        this.horsepower = horsepower;
    }

    public String getModel() {
        return model;
    }

    public int getHorsepower() {
        return horsepower;
    }
}

public class Transmission {
    private String model;
    private int gears;

    public Transmission(String model, int gears) {
        this.model = model;
        this.gears = gears;
    }

    public String getModel() {
        return model;
    }

    public int getGears() {
        return gears;
    }
}

public class Car {
    private Engine engine;
    private Transmission transmission;

    public Car(Engine engine, Transmission transmission) {
        this.engine = engine;
        this.transmission = transmission;
    }

    public Engine getEngine() {
        return engine;
    }

    public Transmission getTransmission() {
        return transmission;
    }

}
```

```java
public class Main {
    public static void main(String[] args) {
        Engine engine = new Engine("V8", 400);
        Transmission transmission = new Transmission("Automatic", 5);
        Car car = new Car(engine, transmission);
        System.out.println("car.engine.model = " + car.getEngine().getModel() + "\ncar.engine.horsepower = " + car.getEngine().getHorsepower());
        System.out.println("car.transmission.model = " + car.getTransmission().getModel() + "\ncar.transmission.gears = " + car.getTransmission().getGears());
    }
}
```

# Chapter 2: How to Think in Terms of Objects

Three important things you can do to develop a good sense of the OO thought process:

- Knowing the difference between the interface and implementation
- Thinking more abstractly
- Giving the user the minimal interface possible

## Knowing the difference between the interface and implementation

- The interface of a class is the set of public methods that it provides. The implementation is the code that is executed when each of those methods is called.

For an example, consider a real life car and a car object in java. The interface of a car is the steering wheel, the gas pedal, and the brake pedal. The implementation is the engine, the transmission, and the brakes mechanism.

A change in the implementation should have no impact on the driver, whereas a change to the interface might.

### The interface

The services presented to an end user comprise the interface. The interface is the set of public methods that a class provides. The interface is the contract between a class and its users. The interface is the only thing that the user needs to know about a class.

As a rule of thumb, the interface to a class should contain only what the user needs to know. In the car example, the user needs to know how to steer the car, how to accelerate, and how to brake. The user does not need to know how the engine works, how the transmission works, or how the brakes work.

### The implementation

The implementation details are hidden from the user. One goal regarding the implementation should be kept in mind: A change to the implementation should not require a change to the user’s code.

In the car example, the user should be able to drive the car regardless of whether the car has a gasoline engine or an electric motor. The user should be able to drive the car regardless of whether the car has a manual transmission or an automatic transmission. The user should be able to drive the car regardless of whether the car has disc brakes or drum brakes.

### Minimual interface

Although perhaps extreme, one way to determine the minimalist interface is to initially provide the user no public interfaces. Of course, the class will be useless; however, this forces the user to come back to you and say, “Hey, I need this functionality.” Then you can negotiate. Thus, you add interfaces only when it is requested. Never assume that the user needs something.

## Thinking more abstractly

One of the main advantages of OO programming is that classes can be reused. In general, reusable classes tend to have interfaces that are more abstract than concrete. Concrete interfaces tend to be very specific, whereas abstract interfaces are more general. However, simply stating that a highly abstract interface is more useful than a highly concrete interface, although often true, is not always the case.

To illustrate the difference between an abstract and a concrete interface, let’s create a taxi object. It is much more useful to have an interface such as “drive me to the airport” than to have separate interfaces such as “turn right,”“turn left,”“start,”“stop,” and so on.

## Giving the user the minimal interface possible

Provide the user with as little
knowledge of the inner workings of the class as possible.

Give the users only what they absolutely need. In effect, this means the class has as few interfaces as possible.

# Chapter 3: Advanced Object-Oriented Concepts

## Constructors

- A constructor is a special method that is called when an object is instantiated (i.e., created). The constructor is used to initialize the object.

- A constructor has the same name as the class and no return type. If there is a return type, then the compiler will treat the constructor as a method.

```java
// Constructor
public Cabbie() {
/* code to construct the object */
}

// Method
public void Cabbie() {
/* code to construct the object */
}
```

### When is a constructor called?

- A constructor is called when an object is instantiated (i.e., created). The constructor is used to initialize the object.

```java
Cabbie myCabbie = new Cabbie();
```

### What is inside a constructor?

- A constructor is used to initialize the object. The constructor is used to set the initial values of the object’s instance variables, for example, if you have a stopwatch object, you might want to initialize the stopwatch to zero.

```java
public class Stopwatch {
    private int elapsedTime;
    private int previousTime;
    private boolean isRunning;

    public Stopwatch() {
        elapsedTime = 0;
        previousTime = 0;
        isRunning = false;
    }
}
```

### The default constructor

If you do not provide a constructor, then the compiler will provide a default constructor.

The only action that a default constructor takes is to call the constructor of its superclass. In many cases, the superclass will be part of the language framework, like the `Object` class in Java. For example, if a constructor is not provided for the Cabbie class, the following default constructor is inserted

```java
public Cabbie() {
    super();
}
```

**Providing a Constructor**

The rule of thumb is that you should always provide a constructor, even if you do not plan on doing anything inside it. You can provide a constructor with nothing in it and then add to it later. Although there is technically nothing wrong with using the default constructor provided by the compiler, it is always nice to know exactly what your code looks like.

It is not surprising that maintenance becomes an issue here. If you depend on the default constructor and then maintenance is performed on the class that added another constructor, then the default constrictor is not created. In short, the default constructor is only added if you don’t include one.As soon as you include just one, the default constructor is not included.

### Using multiple constructors

- You can have more than one constructor in a class. This is called overloading the constructor. The constructors must have different signatures. The signature of a constructor is the number and type of the parameters. (Not the return type.)

```java
public class Cabbie {
    private String name;
    private int numPassengers;

    public Cabbie(String name) {
        this.name = name;
        numPassengers = 0;
    }

    public Cabbie(String name, int numPassengers) {
        this.name = name;
        this.numPassengers = numPassengers;
    }
}
```

### Overloading methods

- You can have more than one method in a class with the same name. This is called overloading the method. The methods must have different signatures. The signature of a method is the number and type of the parameters. (Not the return type.)

```java
// different parameter list
public void getCab (String cabbieName);
// different parameter list
public void getCab (int numberOfPassengers);
```

### The concept of scope

- Scope refers to the visibility of a variable. A variable can be visible to the entire class, or it can be visible only to a method. The scope of a variable is determined by where it is declared.

the state of the object is represented by attributes.There are three types of attributes :

- Local attributes
- Object attributes
- Class attributes

### Local attributes

- Local attributes are declared inside a method and visible only to that method. Local attributes are not visible to any other method or class.

```java
public class Number {
    public method1() {
        int count;
    }
    public method2() {

    }
}
```

The method `method1` contains a local variable called `count`.This integer is accessible only inside `method1`.The method `method2` has no idea that the integer `count` even exists.

Note that scope is delineated by the curly braces `{}`.

For more fun with scope, consider the following example:

```java
public class Number {
    public method1() {
        int count;
    }
    public method2() {
        int count;
    }
}
```

Although both methods have a variable called `count`, they are two different variables. The variable `count` in `method1` is not visible to `method2`, and vice versa.

### Object attributes

There are many design situations in which an attribute must be shared by several methods within the same object.

```java
public class Number {
    private int count;
    public method1() {
        count = 1;
    }
    public method2() {
        count = 2;
    }
}
```

You can play some interesting games with scope. Consider the following code:

```java
public class Number {
    private int count;
    public method1() {
        int count;
    }
    public method2() {
        int count;
    }
}
```

In this case, there are actually three totally separate memory locations with the name of count for each object.The object owns one copy, and `method1()` and `method2()` each
have their own copy.

To access the object variable from within one of the methods, say `method1()`, you can use a pointer called `this` in the C-based languages:

```java
public method1() {
    int count;
    this.count = 1;
}
```

Notice that there is some code that looks a bit curious:

```java
this.count = 1;
```

The selection of the word `this` as a keyword is perhaps unfortunate. However, we must live with it.The use of the `this` keyword directs the compiler to access the object variable `count` and not the local variables within the method bodies.

_Note_

The keyword `this` is a reference to the current object.

### Class attributes

As mentioned earlier, it is possible for two or more objects to share attributes. In Java, C#, and C++, you do this by making the attribute static:

```java
public class Number {
    private static int count;
    public method1() {
    }
    public method2() {
    }
}
```

There are many valid uses for class attributes, for example, to keep track of the number of objects created from a class.

```java
public class Number {
    private static int count;
    public Number() {
        count++;
    }
    public static int getCount() {
        return count;
    }
}
```

The method `getCount()` is a static method. It is a method that is not associated with any object. It is associated with the class itself. You can call the method without creating an object:

```java
int count = Number.getCount();
```

However, you must be aware of potential synchronization problems.

## Operator Overloading

- Operator overloading is a feature of some programming languages that allows the programmer to redefine the meaning of an operator when applied to operands of a user-defined type.

- Operator overloading is syntactic sugar. It allows you to write code that is easier to read and understand.

- Operator overloading is supported in C++, however, it is not supported in Java or C#.

For example, the `+` operator is used to add two numbers together. However, it can also be used to concatenate two strings together in java, java itself has overloaded the `+` operator, but it does not allow operator overloading.

You may read more from here

https://stackoverflow.com/questions/3559563/why-doesnt-java-need-operator-overloading

## Multiple Inheritance

- Multiple inheritance is a feature of some object-oriented computer programming languages in which an object or class can inherit characteristics and features from more than one parent object or parent class.

In some OO languages, such as C++, you can.

However, this situation falls into a category similar to operator overloading. Multiple inheritance is a very powerful technique, and in fact, some problems are quite difficult to solve without it. Multiple inheritance can even solve some problems quite elegantly. However, multiple inheritance can significantly increase the complexity of a system, both for the programmer and the compiler writers.

As with operator overloading, the designers of Java and .NET decided that the increased complexity of allowing multiple inheritance far outweighed its advantages, so they eliminated it from the language. In some ways, the Java and .NET language construct of interfaces compensates for this; however, the bottom line is that Java and .NET do not allow conventional multiple inheritance

There are a good discussion on stackoverflow about this topic

https://stackoverflow.com/questions/2515477/why-is-there-no-multiple-inheritance-in-java-but-implementing-multiple-interfac

## Classes and References

From Effective C++:

> The problem with complex data structures and objects is that they might contain references. Simply making a copy of the reference does not copy the data structures or the object that it references. In the same vein, when comparing objects, simply comparing a pointer to another pointer only compares the references—not what they point to.

### Deep copy vs shallow copy

A deep copy is when all the references are followed and new copies are created for all referenced objects. There might be many levels involved in a deep copy. For objects with references to many objects, which in turn might have references to even more objects, the copy itself can create significant overhead. A shallow copy would simply copy the reference and not follow the levels.

The following figure shows the difference between deep copy and shallow copy

![deep copy vs shallow copy](./assets/deep_copy_vs_shallow_copy.jpg)

# Chapter 4: The Anatomy of a Class

Our sample class

```java
public class Cabbie {
    // attributes
    private static string companyName = "Blue Cab Company";
    private string name;
    private Cab myCab;

    // constructors
    // default constructor
    public Cabbie() {
        name = null;
        myCab = null;
    }

    // constructor with parameters
    public Cabbie(string name, String cabSerialNumber) {
        this.name = name;
        this.myCab = new Cab(cabSerialNumber);
    }

    // accessors methods
    public string getName() {
        return name;
    }

    public void setName(string name) {
        this.name = name;
    }

    public Cab getMyCab() {
        return myCab;
    }

    public void setMyCab(Cab myCab) {
        this.myCab = myCab;
    }

    public static string getCompanyName() {
        return companyName;
    }

    // other methods
    public void giveDestination(){
        private void turnRight() {
            // code to turn right
        }
        private void turnLeft() {
            // code to turn left
        }
    }
}

```

## The name of the class

the name must be descriptive. The choice of a name is important because it provides information about what the class does and how it interacts within larger systems.

The name is also important when considering language constraints. For example, in Java, the public class name must be the same as the filename. If these names do not match, the application won’t compile.

## Comments

Comments are important. They are the only way to document the code. The comments should be clear and concise.

There are three types of comments:

- Block comments `/* */`
- Line comments `//`
- Javadoc comments `/** */`

I like this video, it explains why you shouldn't comment your code, and when two use comments

[Don't Write Comments](https://youtu.be/Bf7vDBBOBUA)

## Attributes

Attributes are the variables that are associated with the class, they represent the state of the object. They are also called instance variables or object variables. Attributes are the data that the class manipulates. Attributes are declared within the class body, but outside of any method bodies.

Note the keword `private` before the attributes, this is called access modifier, it specifies the visibility of the attribute. There are four access modifiers in Java:

- `public` - The attribute is visible to all classes.
- `private` - The attribute is visible only to the class that declares it.
- `protected` - The attribute is visible to the class that declares it and to any subclasses and subpackage.
- `default` - The attribute is visible to the class that declares it and to any classes in the same package.

`public` vs `private` vs `protected` vs `default`:

https://stackoverflow.com/questions/215497/in-java-what-is-the-difference-between-public-default-protected-and-private

Also note the keyword `static` before the attribute, this is called class attribute, it is associated with the class itself, not with any object. There is only one copy of a class attribute, regardless of how many objects are created from the class. Class attributes are declared with the keyword `static`. A case where you might use a class attribute is when you want to keep track of the number of objects that have been created from the class, so whenever the constructor is called, you increment the class attribute.

> **Hide as Much Data as Possible** All the attributes in this example are private. This is in keeping with the design principle of keeping the interface design as minimal as possible. The only way to access these attributes is through the method interfaces provided (which we explore later in this chapter).

## Constructors

Constructors are special methods that are called when an object is created. Constructors are used to initialize the attributes of the object. Constructors are declared with the same name as the class. Constructors do not have a return type, not even void. Constructors are called with the `new` keyword.

This Cabbie class contains two constructors. We know they are constructors because they have the same name as the class: Cabbie. The first constructor is the default constructor:

```java
public Cabbie() {
    name = null;
    myCab = null;
}
```

Technically, this is not a default constructor provided by the system. Recall that the compiler will provide an empty default constructor if you do not code any constructor for a class. By definition, the reason it is called a default constructor here is because it is a constructor with no arguments, which, in effect, overrides the compiler’s default constructor.

If you provide a constructor with arguments, the system will not provide a default constructor. Although this can seem complicated, the rule is that the compiler’s default constructor is included only if you provide no constructors in your code.

Note that coding no constructor is bad practice. It is better to provide a default constructor, even if it is empty.

Another use of the construction instead of initializing the attributes in the declaration is to validate the values of the attributes. For example, if the name of the cabbie must be at least two characters long, you can validate this in the constructor:

```java
public Cabbie(string name, String cabSerialNumber) {
    if (name.length() < 2) {
        throw new IllegalArgumentException("Name must be at least two characters long");
    }
    this.name = name;
    this.myCab = new Cab(cabSerialNumber);
}
```

Multiple constructors are called constructor overloading. The constructors must have different signatures. The signature of a method is the name of the method and the number and type of its parameters. The signature of the default constructor is `Cabbie()`, and the signature of the constructor with parameters is `Cabbie(string, String)`.

## Accessor methods

Also called as getters and setters, they are used to access the attributes of the class. This gives the programmer the ability to control how the attributes are accessed. For example, you may not want any fellow programmers to be able to change the name of the company, so you would not provide a setter for the company name attribute.

## Public interface methods and private implementation methods

Usually, the public interface methods are the methods that are called by other classes, and the private implementation methods are the methods that are called by the public interface methods. The programmer may want to provide a public interface but hide the implementation details. For example, You may want to provide a public method to update the password of a user if he forgets it, but you don't want to expose the encryption details of how the password is encrypted.

```java
public void updatePassword(string newPassword) {
    String encryptedPassword = encrypt(newPassword);
    this.password = encryptedPassword;
}

private String encrypt(string password) {
    // code to encrypt the password
}
```

# Chapter 5 - Class Design Guidelines

## Modeling Real World Systems

## Ideyntifying the Public Interfaces

The goal is to keep the public interface to a minimum.

“the interface of a well-designed object describes the services that the client wants accomplished.” If a class does not provide a useful service to a user, it should not have been built in the first place.

### The Minimum Public Interface

Providing the minimum public interface makes the class as concise as possible. The goal is to provide the user with the exact interface to do the job right. No more, no less.

### Hiding the Implementation

## Designing Robust Constructors (and Perhaps Destructors)

Constructor should put an object into an initial, safe state. This includes issues such as attribute initialization and memory management. You also need to make sure the object is constructed properly in the default condition. It is normally a good idea to provide a constructor to handle this default situation.

### Designing Error Handling Into a Class

It is not a good idea to ignore potential errors. It is better to design error handling into the class.

The general rule that the application should never crash. If an error occurs, the application should fix it and continue, or at minimum, exit gracefully without losing anydata.

### Documenting a Class Using Comments

You must have heard the blablabla hundreds of time, but watch [this](https://youtu.be/Bf7vDBBOBUA)

Good design is practically impossible without good documentation practices

### Building Objects with the Intent to Cooperate

We can safely say that almost no class lives in isolation. In most cases, there is little reason to build a class if it is not going to interact with other classes, unless the class will be used only once. This is a fact in the life of a class. A class will service other classes; it will request the services of other classes, or both.

When designing a class, make sure you are aware of how other objects will interact with it.

## Designing With Reuse in Mind

## Designing With Extensibility in Mind

What Attributes and Methods Can Be Static?

Static methods promote strong coupling to classes. You cannot abstract a static method. You cannot mock a static method or static class. You cannot provide a static interface. The only time it is reasonable to use static classes (within application development—framework development is a bit different) is if you're working with some sort of helper class or extension method that does not produce side effects. For example, _a static class to add numbers is fine. A static class that interacts with a database or a web service is not._

### Make Names Descriptive

And be consistent.

### Abstract Out Nonportable Code

If you are designing a system that must use nonportable (native) code (that is, the code will run only on a specific hardware platform), you should abstract this code out of the class.

For example, instead of memorizing that to make windows beep you need to `System.out.println("\007");` you can create a class that does that for you, and you can call it `Beep` or `Bell` or whatever you want. This way, if you need to change the way you make the beep, you only need to change it in one place.

```java
public class Beep {
    public static void beep() {
        System.out.println("\007");
    }
}

public class Main {
    public static void main(String[] args) {
        Beep.beep();
    }
}
```

### Providing a Way to Copy and Compare Objects

Chapter 3 discussed the issue of copying and comparing objects. It is important to understand how objects are copied and compared. You might not want, or expect, a simple bitwise copy or compare operation. You must make sure that your class behaves as expected, and this means you have to spend some time designing how objects are copied and compared.

### Keeping the Scope as Small as Possible

Keeping the scope as small as possible goes hand-in-hand with abstraction and hiding the implementation. The idea is to localize attributes and behaviors as much as possible. In this way, maintaining, testing, and extending a class are much easier. Using interfaces is a great way to enforce this.

For example, instead of doing this:

```java
public class Math {
    int temp = 0;
    public void swap(int a, int b) {
        temp = a;
        a = b;
        b = temp;
    }
}
```

You can do this:

```java
public class Math {
    public void swap(int a, int b) {
        int temp = a;
        a = b;
        b = temp;
    }
}
```

## Designing With Maintainability in Mind

One way to promote maintainability is to reduce interdependent code—that is, changes in one class have no impact or minimal impact on other classes.

> Highly coupled Classes
> Classes that are highly dependent on one another are considered highly coupled. Thus, if a change made to one class forces a change to another class, these two classes are considered highly coupled. Classes that have no such dependencies have a very low degree of coupling.

### Using Iteration in the Development Process

Don’t write all the code at once! Create the code in small increments and then build and test it at each step. A good testing plan quickly uncovers any areas where insufficient interfaces are provided.

### Testing the Interface

The minimal implementations of the interface are often called _stubs_. By using _stubs_, you can test the interfaces without writing any real code. In the following example, rather than connecting to an actual database, _stubs_ are used to verify that the interfaces are working properly (from the user’s perspective—remember that interfaces are meant for the user). Thus, the implementation is not necessary at this point. In fact, it might cost valuable time and energy to complete the implementation yet because the design of the interface will affect the implementation, and the interface is not yet complete.

Here is a code example that uses an internal array to simulate a working database:

```java
public class DataBaseReader {
    private String db[] = { "Record1","Record2","Record3","Record4","Record5"};
    private booleanDBOpen = false;
    private int pos;
    public void open(String Name){
        DBOpen = true;
    }
    public void close(){
        DBOpen = false;
    }
    public void goToFirst(){
        pos = 0;
    }
    public void goToLast(){
        pos = db.length - 1;
    }
    public int howManyRecords(){
        int numOfRecords = db.length;
        return numOfRecords;
    }
    public String getRecord(int key){
        /* DB Specific Implementation */
        return db[key];
    }
    public String getNextRecord(){
        /* DB Specific Implementation */
        return db[pos++];
    }
}
```

## Using Object Persistence

Persistence the concept of maintaining the state of an object. When you run a program, if you don’t save the object in some manner, the object dies, never to be recovered. These transient objects might work in some applications, but in most business systems, the state of the object must be saved for later use.

The object may be serialized and saved to :

- Flat files
- Relational databases
- NoSQL databases

Serialization is the process of translating a data structure or object state into a format that can be stored or transmitted and reconstructed later.

Serialization and deserialization must use the same specifications. It is sort of like an encryption algorithm. If one object encrypts a string, the object that wants to decrypt it must use the same encryption algorithm.


# Chpater 6 - Designing with Objects

Object-oriented (OO) design has been touted as a robust and flexible software development approach. The truth is that you can create both good and bad OO designs just as easily as you can create both good and bad non-OO designs. Don’t be lulled into a false sense of security just because you are using a state-of-the-art design methodology. You must pay attention to the overall design and invest the proper amount of time and effort to create the best possible product.

In the previous chapter, we concentrated on designing good classes. This chapter focuses on designing good systems. A system can be defined as classes that interact with each other. Proper design practices have evolved throughout the history of software development, and there is no reason you should not take advantage of the blood, sweat, and tears of your software predecessors, whether they used OO technologies or not.

Taking advantage of previous efforts is not limited to design practices; you can even incorporate existing legacy code in your object-oriented designs. In many cases, you can take code, which may have been working well for years, and literally wrap it in your objects. The wrapping is discussed later in the chapter.

## Design Guidelines

Generally, a solid OO design process includes the following steps:

1. Doing the proper analysis
2. Developing a statement of work that describes the system
3. Gathering the requirements from this statement of work
4. Developing a prototype for the user interface
5. Identifying the classes
6. Determining the responsibilities of each class
7. Determining how the various classes interact with each other
8. Creating a high-level model that describes the system to be built

Note that Most of these practices are not specific to OO. They apply to software development in general.


> The Ongoing Design Process
> Despite the best intentions and planning, in all but the most trivial cases, the design is an ongoing process. Even after a product is in testing, design changes will pop up. It is up to the project manager to draw the line that says when to stop changing a product and adding features. I like to call this Version 1.



the reasons to identify requirements early and keep design changes to a minimum are as follows:

- The cost of a requirement/design change in the design phase is relatively small.
- The cost of a design change in the implementation phase is significantly higher.
- The cost of a design change after the deployment phase is astronomical when compared to the first item.

After the software is released, problems that have not been caught and fixed prior to release become much more expensive. To illustrate, consider the dilemma automobile companies face when they are confronted with a recall. If a defect in the automobile is identified and fixed before it is shipped (ideally before it is manufactured), it is much cheaper than if all delivered automobiles have to be recalled and fixed one at a time. Not only is this scenario very expensive, but it damages the reputation of the company. In an increasingly competitive market, high-quality software, support services, and reputation are the competitive advantages that will keep a company in business.

The following sections provide brief summaries of the items listed previously as being part of the  design process. 

### Performing the Proper Analysis

The users must work hand in hand with the developers at all stages. In the analysis phase, the users and the developers must do the proper research and analysis to determine the statement of work, the requirements of the project, and whether to actually do the project.

During the analysis phase, there must not be any hesitation to 
terminate the project if a valid reason exists to do so. Too many times, pet project status or some 
political inertia keeps a project going, regardless of the obvious warning signs that cry out for 
project cancellation.

### Developing a Statement of Work

The statement of work (SOW) is a document that describes the system.

The SOW should give anyone who reads it a complete, high level understanding of the system. Regardless of how it is written, the SOW must represent the complete system and be clear about how the system will look and feel.

The SOW contains everything that must be known about the system. Many customers create a request for proposal (RFP) for distribution, which is similar to the statement of work. A customer creates an RFP that completely describes the system the customer wants built and releases it to multiple vendors. The vendors then use this document, along with whatever analysis they need to do, to determine whether they should bid on the project, and if so, what price to charge.

### Gathering the Requirements

The requirements document describes what the users want the system to do. Even though the level of detail of the requirements document does not need to be of a highly technical nature, the requirements must be specific enough to represent the true nature of the user’s needs for the end product.

Whereas the SOW is a document written in paragraph (even narrative) form, the requirements are usually represented as a summary statement or presented as _bulleted items_. Each individual bulleted item represents one specific requirement of the system.

In many ways, these requirements are the most important part of the system. The SOW might contain irrelevant material; however, the requirements are the final representation of the system that must be implemented.

### Developing a Prototype

One of the best ways to make sure users and developers understand the system is to create 
a prototype. A prototype is a working model of the system. It does not have to be a complete. It may be a simulated GUI, created by using IDE or on the whiteboard.


### Identifying the Classes

After the requirements are documented, the process of identifying classes can begin. From the requirements, one straightforward way of identifying classes is to highlight all the nouns. These tend to represent objects, such as people, places, and things.

Don't get fussy about getting all the classes right the first time. You will have plenty of time to refine the classes as you go through the design process.

### Determining the Responsibilities of Each Class

You need to determine the responsibilities of each class you have identified. This includes the data that the class must store and what operations the class must perform. For example, an Employee object would be responsible for calculating payroll and transferring the money to the appropriate account. It might also be responsible for storing the various payroll rates and the account numbers of various banks.

### Determining How the Classes Interact

Most classes do not exist in isolation. One class can send a message to another class when it needs information from that class, or if it wants the other class to do something for it.

### Creating a Class Model to Describe the System

The class model is a graphical representation of the system. It shows the classes and how they interact with each other. UML is the most popular notation for creating class models.

## Object Wrappers

### Wrapping Structured Code

Object oriented programming is not a separate paradigm from structured programming. When you write a program that uses an object-oriented programming language and are using sound object-oriented design techniques, you are also using structured programming techniques. There is no way around this.

The goal is to wrap structured code in objects.

### Wraping Nonportable Code

remember the `'\007'` example

### Wrapping Existing Classes

Software developers often utilize code written by someone else. Perhaps the code was purchased from a vendor or even written internally within the same organization. In many of these cases, the code cannot be changed. Perhaps the individual who wrote the code is no longer with the organization, or the vendor cannot perform maintenance updates, and so on. This is where the true power of wrappers emerges.

The idea is to take an existing class and alter its implementation or interface by wrapping it inside a new class—just like we did for the structured code and nonportable code. The difference in this case is that, rather than putting an object-oriented face to the code, we are altering its implementation or interface.

Why would we want to do this? Well, the answer lies with both the implementation and the interface.

Consider the database example that we used in Chapter 2. Our goal was to provide the same interface for the developers regardless of which database they were using. In fact, if we need to support another database, our goal would remain the same—to make the transition to the new database transparent to the user.

Also, remember our earlier discussion about creating middleware to provide an interface between objects and relational databases. As developers, we want to use objects. Thus, we want functionality that will allow us to persist objects to a database. What we don’t want to have to do is write SQL code for every single object transaction performed to a relational database. This is where we can consider middleware to be a wrapper, and many object-relational mapping products are available.