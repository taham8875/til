# Chapter 6 - Objects and Data Structures

The reason we keep out variable private, that we don't anyone else lto *depend* on them, we want to keep the freedom to change their type or implementation whenever we want.

Why, then, do so many developers automatically add getters and setters to their objects, exposing their private variables as if they were public?

## Data Abstraction

Consider the difference between code 1 and code 2 below, both represents the data of point in a 2D plane. One exposes its implementation and the other completely hides it.

```java
public class Point {
    public double x;
    public double y;
}
```

```java
public interface Point {
    double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```

The beautiful thing about code 2 is that there is no way to tell whether it is implemented using rectangular or Polar coordinates. I might be neither!

Teh interface represents more than a data structure, the methods enforce and access policy, you cana read the individual coordinates independently, but you can set them only as an atomic operation.

Code 1 on the other hand, is very clearly implemented in rectangular coordinates. And it enforces us to manipulate the coordinates independently. That exposes the implementation. It also will expose the implementation if we just make the fields private and add getters and setters.

Hiding implementation is not just matter of putting a layer of functions between the variables. Hiding implementation is about abstractions! Not just hiding the data behind dumb getters and setters. Rather its exposes abstract interfaces that allow its users to manipulate the essence of the data, without having to know its implementation.

Consider code 3 vs code 4 below, the first uses concrete terms to communicate the fuel level of a car, the second does so with the abstraction of the percentage, in the concrete case you are just sure that it is just accessors to the data, in the second you have no clue about the implementation.

```java
public interface Vehicle {
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}
```

```java
public interface Vehicle {
    double getPercentFuelRemaining();
}
```

In both of the above the second one is preferable, we don't want to expose the details of the data, we just want to expose the abstraction.

This is not accomplished by using interfaces and/or getters and setters. Serious thought need to be put into the best way to represent the data that an object contains. The worst option is to blindly add getters and setters.

## Data/Object Anti-Symmetry

These two examples show the difference between objects and data structures, objects hide their data behind abstractions and expose functions that operate on that data. Data structures expose their data and have no meaningful functions.

They are both complementary.

consider the following snippet:

```java
public class Square {
    public Point topLeft;
    public double side;
}

public class Rectangle {
    public Point topLeft;
    public double height;
    public double width;
}

public class Circle {
    public Point center;
    public double radius;
}

public class Geometry {
    public final double PI = 3.141592653589793;

    public double area(Object shape) throws NoSuchShapeException {
        if (shape instanceof Square) {
            Square s = (Square) shape;
            return s.side * s.side;
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.height * r.width;
        } else if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return PI * c.radius * c.radius;
        }
        throw new NoSuchShapeException();
    }
}
```

Object oriented programmers will complain that it is procedural, and they are right!

Consider what will happen if we added a `perimeter()` function to the `Geometry` class, the shape classes won't be affected, and any other class depends on these shapes will not be affected.

On the other hand, if i want to add a new shape, I must change the all functions in the `Geometry` class to deal with it.

Notice that the two conditions are opposed.

Now let's consider the following oop code

```java
public class Square implements Shape {
    private Point topLeft;
    private double side;

    public double area() {
        return side * side;
    }
}

public class Rectangle implements Shape {
    private Point topLeft;
    private double height;
    private double width;

    public double area() {
        return height * width;
    }
}

public class Circle implements Shape {
    private Point center;
    private double radius;
    public final double PI = 3.141592653589793;

    public double area() {
        return PI * radius * radius;
    }
}
```

Again, you can see the complimentary nature of these two definitions. They are virtually opposites.

This exposes the difference between objects and data structures.

> Procedural code (code using data structures) makes it easy to add new functions without changing the existing data structures. OO code, on the other hand, makes it easy to add new classes without changing existing functions.

The complement is also true:

> Procedural code makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.

So things that are hard for OO are easy for procedural, and things that are hard for procedural are easy for OO.

In any complex systems there will be times when you want to add new data types rather than new functions. For these cases objects and OO are most appropriate. On the other hand, there will be times when you want to add new functions as opposed to data types. In that case procedural code and data structures will be more appropriate.

## The Law of Demeter

The law of Demeter can be stated simply as "Only talk to your friends". The fundamental notion is that a given object should assume as little as possible about the structure or properties of anything else (including its subcomponents).

It suggests that a method should only interact with:

1. its own members (fields or methods)
2. object its creates
3. parameters passed to it
4. object held in variables it directly holds

```java
public void ObjectO::MethodM(Class1 arg1, Class2 arg2) {
    // can refer to features in the object O itself
    System.out.println(self.attribute);

    // can refer to features of the parameters of MethodM
    System.out.println(arg1.attribute);
    System.out.println(arg2.attribute);

    // can refer to features of any object created in MethodM
    Class3 obj3 = new Class3();
    System.out.println(obj3.attribute);

    // can refer to features of direct components of O
    System.out.println(self.component.attribute);
}
```

Again, talk to your friends, don't talk to strangers.

### Train Wrecks

This kind of code is called a train wreck, because it looks like a bunch of train cars all strung together.

```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath();
```

This is too much knowledge for one function to know.

The goal of the Law of Demeter is to *tell an object what to do, not how to do it*. not *ask for data and do the work itself*.

So the above code should be refactored to:

```java
BufferedOutputStream bos = ctxt.createScratchFileStream(classFileName);
```

That seems like a reasonable thing for an object to do! This allows `ctxt` to hide its internals and prevents the current function from having to violate the Law of Demeter by navigating through objects it shouldn’t know about.

Another simple example is refactoring the following code that violates the Law of Demeter:

```java
class Address {
    private String zipCode;

    public Address(String zipCode) {
        this.zipCode = zipCode;
    }

    public String getZipCode() {
        return zipCode;
    }
}

class Customer {
    private Address address;

    public Customer(Address address) {
        this.address = address;
    }

    public Address getAddress() {
        return address;
    }
}

class Order {
    private Customer customer;

    public Order(Customer customer) {
        this.customer = customer;
    }

    public void printShippingZipCode() {
        // Violates the Law of Demeter
        String zipCode = customer.getAddress().getZipCode();
        System.out.println("Shipping zip code: " + zipCode);
    }
}
```

The above code violates the Law of Demeter because the `Order` class is directly accessing the `zipCode` property of the `Address` class through the `Customer` class. This can be refactored to:

```java
class Address {
    private String zipCode;

    public Address(String zipCode) {
        this.zipCode = zipCode;
    }

    public String getZipCode() {
        return zipCode;
    }
}

class Customer {
    private Address address;

    public Customer(Address address) {
        this.address = address;
    }

    // New method to get zip code directly
    public String getZipCode() {
        return address.getZipCode();
    }
}

class Order {
    private Customer customer;

    public Order(Customer customer) {
        this.customer = customer;
    }

    public void printShippingZipCode() {
        // Now follows the Law of Demeter
        String zipCode = customer.getZipCode();
        System.out.println("Shipping zip code: " + zipCode);
    }
}
```

## Data Transfer Objects

1. **Data Transfer Object (DTO)**:
   - A DTO is a simple data structure with **public variables and no functions**. It holds data but doesn’t perform any actions.
   - DTOs are commonly used for **transferring data**, especially when interacting with databases or reading from external sources (like sockets) or deserialize JSON response in your application.
   - They’re often the first stage in converting raw data from an external source into structured objects that the application can use.

2. **Bean Objects**:
   - The “bean” format is a variation of DTOs where variables are **private** but can be accessed and modified using **getters and setters**.
   - While adding getters and setters might make beans appear more aligned with object-oriented (OO) principles, it doesn’t necessarily offer any functional benefit over simpler DTOs.

3. **Active Record**:
   - Active Record is a **specialized DTO** often used for database tables.
   - They are like DTOs but can include basic **methods like `save` or `find`** to interact with the database.
   - Sometimes, developers mistakenly add business logic (rules for data manipulation) into Active Records, which isn’t ideal because it creates a mix of data structure and functional object.

4. **Best Practice**:
   - Keep the **data structure separate** from business logic.
   - Use Active Records strictly as data containers and create **separate objects** to implement business rules and processing logic.

## Conclusion

Objects expose behavior and hide data. This makes it easy to add new kinds of objects without changing existing behaviors. It also makes it hard to add new behaviors to existing objects. Data structures expose data and have no significant behavior. This makes it easy to add new behaviors to existing data structures but makes it hard to add new data structures to existing functions
