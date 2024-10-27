# Chapter 8 - Boundaries

We seldom control all the software we use. We usually have to interact with third-party libraries, databases, or services. We have to interact with these external systems through APIs, we usually use open source libraries, we depend on other teams in the company to produce components or subsystems for us. Somehow we must cleanly integrate this foreign code with our own.

## Using Third-Party Code

There is a natural tension between the provider of an interface and the user of an interface. Providers of the third party packages and frameworks striver for broad applicability so they can work in many environments and appeal to wide audiences, users on the other hand, want in interface that is focused on their particular need. This tension can cause problems at the boundaries of our systems.

Let's take `java.util.Map` as an example. It is a very general interface that is used in many different contexts. This power and flexibility is useful, but it can also be a problem, for example you may build a map and pass it around. Out intention the none of the recipients of the map delete anything in the map, but there are the `clear` method so any user can delete it. Or maybe our design convention is that only particular types of objects can be stored in the map, but maps do not reliably constrain the types of objects placed within them. Any determined user can add items of any type to any map.

the following snippet is the methods of the `Map` interface:

```text
• clear() void – Map
• containsKey(Object key) boolean – Map
• containsValue(Object value) boolean – Map
• entrySet() Set – Map
• equals(Object o) boolean – Map
• get(Object key) Object – Map
• getClass() Class<? extends Object> – Object
• hashCode() int – Map
• isEmpty() boolean – Map
• keySet() Set – Map
• notify() void – Object
• notifyAll() void – Object
• put(Object key, Object value) Object – Map
• putAll(Map t) void – Map
• remove(Object key) Object – Map
• size() int – Map
• toString() String – Object
• values() Collection – Map
• wait() void – Object
• wait(long timeout) void – Object
• wait(long timeout, int nanos) void – Object
```

If our app need a map of sensors:

```java
Map sensors = new HashMap();
```

Then, when someone need to access the sensors, they can do this:

```java
Sensor s = (Sensor) sensors.get(sensorId);
```

He do that over and over again thorough the code, the client of the code carries the responsibility of getting an `Object` from the map and cast it to a `Sensor`. This works but not a clean code.

The reliability of the code can be improved by using generics:

```java
Map<Sensor> sensors = new HashMap<Sensor>();
Sensor s = sensors.get(sensorId);
```

However, this does not solve all our problems that `Map<Sensors>` provides more capability than we need or want.

A cleaner way to use the map might look like this:

```java
public class Sensors {
  private Map sensors = new HashMap();
  
  public Sensor getById(String id) {
    return (Sensor) sensors.get(id);
  }
}
```

The interface of the map is hidden behind the `Sensors` class and able to evolve with very little impact on the rest of the application.

The interface is also tailored and constrained to the needs of the application, it results in code that it easier to understand, maintain and hard to misuse. The `Sensors` class can enforce design and business rules.

We are not suggesting that every use of map be encapsulated in this form. Rather, we are advising you not to pass maps (or any other interface at a boundary) around your system. If you use a boundary interface like map, keep it inside the class, or close family of classes, where it is used. Avoid returning it from, or accepting it as an argument to, public APIs.

## Exploring and Learning Boundaries

Third party code helps us get more functionality delivered in less time, where do we start when we want to use a third party code?

It is not our job to test the third party code, but writing tests for it may be helpful.

Suppose it is not clear how to use our third party library, we might spend day or two reading the documentation and deciding how to use it, then we start writing our code to use the third party code and see if it does what we expect. No surprise, we might end up with long debugging sessions trying to figure out if the bug is in our code or the third party code.

Learning the third party code is hard, integrating the third party code is hard too, doing both in the same time is doubly harder.

What if we can take another approach, instead of experimenting the new stuff in the production code, we could write some tests to explore our understanding of the third party code, such tests are called **learning tests**.

In learning tests we call the third party code the same way we expect to call it in the production code, we check our understanding of the api and we just focus on what we want from the api

## Learning Test Are better than Free

The learning tests end up costing nothing. We had to learn the API anyway, and writing those tests was an easy and isolated way to get that knowledge. The learning tests were precise experiments that helped increase our understanding.

Not only are learning tests free, they have a positive return on investment. When there are new releases of the third-party package, we run the learning tests to see whether there are behavioral differences.

Whether you need the learning provided by the learning tests or not, a clean boundary should be supported by a set of outbound tests that exercise the interface the same way the production code does. Without these boundary tests to ease the migration, we might be tempted to stay with the old version longer than we should.

## Using Code That Does Not Yet Exist

There is another kind of boundary, one that separates the known from the unknown, sometimes in places where our knowledge drop off the edge, sometimes what is on the other side of boundary in unknowable, sometimes we choose to look no farther than the boundary.

## Clean Boundaries

Change in one thing that happen at boundaries, good software designs accommodate changes without huge investments and rework, when we use code that is out of our control, special care must be taken to protect our investment and make sure future changes are not too costly.

**Code at the boundaries need clear separation and tests that define expectations.**

We manage third-party boundaries by having very few places in the code that refer to them. We may wrap them as we did with map, or we may use an adapter to convert from our perfect interface to the provided interface. Either way our code speaks to us better, promotes internally consistent usage across the boundary, and has fewer maintenance points when the third-party code changes.
