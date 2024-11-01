# Chapter 11 - Systems

## How Would You Build a City?

Could you manage all the details yourself, probably not. Even managing an existing city is too much for one person. Yet, cities work most of the time, they work because cities have group of people that manage particular parts of the city, the water system, the power system, traffic, law enforcement, and so on.

Some of these people are responsible for the big picture, while other people focus on the details.

Cities also worked because they have evolved appropriate levels of abstraction and modularity that make it possible for the individuals and the components they manage to work effectively together even without understanding the big picture.

Clean code helps as achieve cleanliness at the lower levels of abstraction, in this chapter, we will consider how to be clean at higher levels of abstraction, the *system* level.

## Separate Constructing a System from Using It

First, consider that *construction* is very different process than *use*.

> Software systems should separate the startup process, when the application objects are constructed and the dependencies are "wired" together, from the runtime logic that takes over after startup.

The startup process is a concern that any application must address, and separation of concerns is one of the oldest and most important design techniques in our craft.

Unfortunately, most applications do not separate this concern, for example

```java
public Service getService() {
    if (service == null) {
        service = new MyServiceImpl(...);
    }
    return service;
}
```

This is the *lazy initialization/evaluation* idiom, and it has several pros. We don’t incur the overhead of construction unless we actually use the object, and our startup times can be faster as a result. We also ensure that `null` is never returned.

However, we now have a hard coded dependency on `MyServiceImpl` and everything it constructor requires (removed from the code snippet and replace with `...`), we can't compile without resolving this dependency, even if we never use an object of this type at runtime.

Testing can be a problem, if `MyServiceImpl` is a heavyweight object, wer need to make sure than an appropriate params get passed to the service field before this method is called during a unit test, because we have a construction logic mixed with the runtime logic, we should test all the execution paths of this method. Having both of this responsibilities in the same method violates the single responsibility principle.

We should modularize the construction process separately rom the normal runtime logic and we should make sure that we have a global consistent strategy for resolving our major dependencies.

### Separation of Main

One way to separate construction from use is simply to move all aspects of construction to `main`, or modules called by `main`, and to design the rest of the system assuming that all objects have been constructed and wired up appropriately.

### Dependency Injection

Dependency injection means in a simple term that an object should not take the responsibility for instantiating dependencies itself, instead, it should pass this responsibility to another "authority" that can be configured to "inject" the correct dependencies. Thereby inverting the control

True dependency injection goes one step further, the class takes no direct steps into resolving its dependencies, it is completely passive, instead, it provides setter methods or constructor arguments that are used to inject the dependencies.

## Scaling Up

It is a myth that we can get systems “right the first time.” Instead, we should implement only today's stories, then refactor and expand the system to implement new stories tomorrow. This is the essence of iterative and incremental agility
