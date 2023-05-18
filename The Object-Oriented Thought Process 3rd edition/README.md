# Chapter 1 - Introduction to Object-Oriented Concepts


* object-oriented (OO) software development has been around since the early 1960s.

* When systems are working fine, in most cases you should not change them, at least not simply for the sake of change.

* Object Wrappers : A wrapper is a class that encapsulates (wraps) the functionality of another class or program.

## Procedural Versus Object-Oriented Programming

* Procedural programming is a programming paradigm, derived from structured programming, based on the concept of the procedure call. Procedures, also known as *routines*, *subroutines*, or *functions*, simply contain a series of computational steps to be carried out.

* Object-oriented programming (OOP) is a programming paradigm based on the concept of "objects", which may contain data, in the form of fields, often known as *attributes*; and code, in the form of procedures, often known as *methods*.

> In OO design, the attributes and behaviors are contained within a single object, whereas in procedural, or structured design, the attributes and behaviors are normally separated.

* Data Hiding : In OO terminology, data is referred to as *attributes*, and behaviors are referred to as *methods*. Restricting access to certain attributes and/or methods is called *data hiding*.

## What Is an Object?

* An object is an instance of a class. Objects are the basic building blocks of an OO system.

### Object data

* An object's data is referred to as its *attributes*. The data stored within an object represents the state of the object. 

e.g. A bank account object might have attributes such as account number, balance, and interest rate.

### Object behavior

* An object's behavior is referred to as its *methods*. The methods of an object represent the operations that can be performed on the object.

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

* A class is a blueprint or template that describes the details of an object. (think of a cookie cutter)

* A class is a logical grouping of objects that have common properties and behaviors.

`public` vs `private` vs `protected` vs `default`:

https://stackoverflow.com/questions/215497/in-java-what-is-the-difference-between-public-default-protected-and-private

## Encapsulation and Data Hiding

* Encapsulation is the process of combining data and behavior into a single package called an object.

* Data hiding is the process of hiding the internal data of an object from the outside world.

* Encapsulation and data hiding are closely related. In fact, encapsulation is often referred to as data hiding.

### Interface

* An interface is a collection of abstract methods that has no implementation. An interface is a contract between a class and the outside world.


## Inheritance

* Inheritance is the process by which one class acquires the properties of another class. The class that acquires the properties of another class is called the *subclass*, and the class from which the subclass is derived is called the *superclass*.

* Inheritance is a powerful feature of OO programming because it allows you to reuse existing code without having to rewrite it.

## Abstraction

* Abstraction is the process of hiding the internal details of an object from the outside world. Only the functionality that is necessary to the user should be made available.

* Abstraction is closely related to encapsulation and data hiding.


Abstraction vs Encapsulation: 

https://stackoverflow.com/questions/742341/difference-between-abstraction-and-encapsulation

## Is-a Relationships

* Inheritance is often referred to as an *is-a* relationship. For example, a checking account is a bank account, a savings account is a bank account, and a certificate of deposit (CD) is a bank account. In each case, the subclass inherits the properties of the superclass.

## Polymorphism

* Polymorphism is the ability to take more than one form. In OO programming, polymorphism refers to the ability of an object to provide different behaviors to its users depending on its state.

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

* Composition is the process of combining two or more objects into a more complex object. For example, a car is composed of an engine, a transmission, and a body.

* Composition is often referred to as a *has-a* relationship. For example, a car has an engine, a transmission, and a body.

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


* Knowing the difference between the interface and implementation
* Thinking more abstractly
* Giving the user the minimal interface possible

## Knowing the difference between the interface and implementation

* The interface of a class is the set of public methods that it provides. The implementation is the code that is executed when each of those methods is called.

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

