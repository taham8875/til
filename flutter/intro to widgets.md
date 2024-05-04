# Flutter Widgets

Widgets are the basic building blocks of a Flutter app's user interface. Each widget is an immutable declaration of part of the user interface. Everything in a Flutter app is a widget, including the app itself.

Widgets serves many purposes in a Flutter app, for example:
- There are widgets that provide layout related functionality, such as `Padding`, `Row`, and `Column`.
- There are widgets that provide structural elements, such as `Button`, `TextField`, and `Image`.
- There are widgets that provide stylistic elements, such as `TextStyle`, `Colors`, and `Theme`.

> To follow this tutorial, create a new flutter project with vscode command (click `Ctrl+Shift+P` and type `Flutter: New Project`) and the choose the `Application` choice, it create a new flutter counter app.


# Stateless Widgets

let's say the in the newly created flutter project, we need the text `'You have pushed the button this many times:'` to have a red background color, we can wrap it in `DecoratedBox` widget and set the `decoration` property to `BoxDecoration` with `color` property set to `Colors.red`.

before:
```dart
children: <Widget>[
const Text(
    'You have pushed the button this many times:',
),
```

After:
```dart
children: <Widget>[
const DecoratedBox(
    decoration: BoxDecoration(
      color: Colors.red,
    ),
    child: Text(
      'You have pushed the button this many times:',
    ),
),
// ...
```

let's say we want to add a padding around the text, we can wrap the `Text` widget in a `Padding` widget and set the `padding` property to `EdgeInsets.all(8.0)`.

```dart
children: <Widget>[
const DecoratedBox(
    decoration: BoxDecoration(
      color: Colors.red,
    ),
    child: Padding(
      padding: EdgeInsets.all(8.0),
      child: Text(
        'You have pushed the button this many times:',
      ),
    ),
),
// ...
```

This process of adding widgets together to create a new widget is called **composition**. Flutter provides a rich set of widgets that can be composed together to create complex user interfaces.

Let's create a new widget called `DogName` that displays the name of a dog in a blue background with a padding of 8.0 around it.

```dart
class DogName extends StatelessWidget {
  final String name;

  const DogName({super.key, required this.name});

  @override
  Widget build(BuildContext context) {
    return DecoratedBox(
      decoration: 
      const BoxDecoration(
        color: Colors.red,
      ),
      child: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Text(name),
      ),
    );
  }
}
```

Now we can use the `DogName` widget in the `MyHomePage` widget.

```dart
children: <Widget>[
  const DogName(name: 'Fido'),
  // ...
],
```

# Stateful Widgets

Stateful widgets are widgets that can change over time. It provides a mutable configuration info and a state object that can be changed over time and trigger a rebuild of the UI.


The new flutter app flutter initiated is a counter app, however, let's implement our new counter using a stateful widget.

```dart
class ItemCounter extends StatefulWidget {
  final String name; // Immutable property

  const ItemCounter({super.key, required this.name});

  @override
  State<ItemCounter> createState() => _ItemCounterState();
}

class _ItemCounterState extends State<ItemCounter> {
  int count = 0;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        Text(widget.name),
        Text('$count'),
        ElevatedButton(
          onPressed: () {
            setState(() {
              count++;
            });
          },
          child: const Text('Increment'),
        ),
      ],
    );
  }
}
```

Ass we see, a stateful widget consists of two main parts: 

1. **Immutable Configuration:** This includes properties that never change once the widget is created like `name` (immutable property). These properties define the initial state of the widget but remain constant throughout its lifecycle.

2. **Mutable State:** This is the data that can change over time, influencing how the widget appears or behaves. `_ItemCounterState` maintains the count variable


Now we can use the `ItemCounter` widget in the `MyHomePage` widget.

```dart
children: <Widget>[
    // ...
    const ItemCounter(name: 'Fido'),
    // ...
],
```
**How Stateful Widgets Work?**

When a stateful widget is created, Flutter calls its `createState` method, which returns a corresponding state object.

This state object is then responsible for managing the mutable state and triggering UI updates when changes occur.

**The Role of `setState()`**

In Flutter, UI updates are triggered through the `setState()` method. When you call `setState()`, Flutter knows that the state of the widget has changed, and it schedules a rebuild of the widget's subtree. Inside `setState()`, you update the mutable state variables.


# When to use keys

Keys are used to uniquely identify widgets. They are useful when you need to maintain state across rebuilds, such as when reordering a list in a todo app.

When you reorder items in your flutter app, flutter checks if it maps the old element tree items to widget tree items correctly, it compares each item in the old list to the new list and see if they are of the same type and have the same key.

When to key is not used, flutter will figure out the the order didn't change since the todo items are of the same type, resulting in wrong functionality.