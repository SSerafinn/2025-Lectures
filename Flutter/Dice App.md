# Building a Dice App in Flutter
### A Step-by-Step Guide to Widgets, State, and Interactivity

---

## 1. Project Setup

We start with the standard Flutter boilerplate. Notice the [`Scaffold`](https://api.flutter.dev/flutter/material/Scaffold-class.html) and [`MaterialApp`](https://api.flutter.dev/flutter/material/MaterialApp-class.html) are already doing heavy lifting — the `AppBar`, `backgroundColor`, and `body` are all configured here.

```dart
import 'package:flutter/material.dart';

void main() {
  return runApp(
    MaterialApp(
      home: Scaffold(
        backgroundColor: Color.fromRGBO(213, 128, 252, 1),
        appBar: AppBar(
          elevation: 10.0,
          shadowColor: Colors.black,
          centerTitle: true,
          title: Text('Diceeee'),
          backgroundColor: Color.fromRGBO(255, 255, 255, 1),
        ),
        body: DicePage(),
      ),
    ),
  );
}

class DicePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

> **💡 Note:** `DicePage` is currently a [`StatelessWidget`](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html) — an empty container for now. We'll be building it up progressively.

---

## 2. Displaying Images

### 2.1 Basic Image Widget

Let's add a dice image inside a [`Row`](https://api.flutter.dev/flutter/widgets/Row-class.html):

```dart
class DicePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Image(image: AssetImage('images/dice1.png')),
      ],
    );
  }
}
```

**Problem:** The image is too large and overflows the screen.

### 2.2 Fixed Width — A Static Solution

```dart
Image(image: AssetImage('images/dice1.png'), width: 200)
```

This works, but it's a hardcoded value — not responsive across different screen sizes.

### 2.3 The `Expanded` Widget — A Dynamic Solution

The [`Expanded`](https://api.flutter.dev/flutter/widgets/Expanded-class.html) widget tells a child to fill all available space along the main axis of its parent (in this case, the `Row`). Think of it like `flex: 1` in CSS Flexbox.

```dart
class DicePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Expanded(
          child: Image(image: AssetImage('images/dice1.png')),
        ),
      ],
    );
  }
}
```

### 2.4 Using `flex` to Control Proportions

When multiple `Expanded` widgets share the same parent, you can use the `flex` property to control how much space each one takes — just like `flex-grow` in CSS.

```dart
Expanded(flex: 1, child: Image(image: AssetImage('images/dice1.png'))),
Expanded(flex: 2, child: Image(image: AssetImage('images/dice2.png'))),
```

Here, `dice2` takes up twice as much space as `dice1`.

### 2.5 Shorthand: `Image.asset()`

Flutter provides named constructors for common use cases. Since asset images are used so frequently, there's a dedicated constructor: [`Image.asset()`](https://api.flutter.dev/flutter/widgets/Image/Image.asset.html).

```dart
// Before
Image(image: AssetImage('images/dice1.png'))

// After — cleaner and idiomatic
Image.asset('images/dice1.png')
```

---

## 3. Polishing the Layout with Intention Actions

Android Studio / VS Code offer **intention actions** (the lightbulb 💡 icon) to quickly wrap or unwrap widgets. Using these, we wrap our images in [`Padding`](https://api.flutter.dev/flutter/widgets/Padding-class.html) and center the whole `Row` with a [`Center`](https://api.flutter.dev/flutter/widgets/Center-class.html) widget:

```dart
class DicePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Row(
        children: [
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Image.asset('images/dice1.png'),
            ),
          ),
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: Image.asset('images/dice2.png'),
            ),
          ),
        ],
      ),
    );
  }
}
```

> **🛠 Pro Tip:** Use `Alt + Enter` (Windows/Linux) or `Option + Enter` (Mac) to trigger intention actions on any widget.

---

## 4. Detecting User Gestures

Flutter's [button catalog](https://docs.flutter.dev/ui/widgets#Input) offers many button types. Since we want to detect a tap on an *image* (not a traditional button), we'll use [`GestureDetector`](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html).

The `GestureDetector` widget wraps any widget and exposes gesture callbacks. The one we care about is `onTap()`, which fires whenever the user taps the wrapped widget.

```dart
GestureDetector(
  onTap: () {
    print("Left dice tapped");
  },
  child: Image.asset('images/dice1.png'),
)
```

> **🔍 Common Callbacks in `GestureDetector`:**
> - `onTap` — single tap
> - `onDoubleTap` — two taps in quick succession
> - `onLongPress` — extended press
> - `onPanUpdate` — dragging
>
> See the full list in the [official GestureDetector docs](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html).

---

## 5. Dart Variables and Data Types

Before we wire up the tap to actually change the dice image, let's take a detour into Dart fundamentals.

> **📖 Reference:** [A tour of the Dart language – Variables](https://dart.dev/language/variables)

### 5.1 Declaring Variables

```dart
var name = 'Flutter';       // Type inferred as String
int age = 3;                // Explicit integer
double version = 3.10;      // Floating point
bool isAwesome = true;      // Boolean
String greeting = 'Hello!'; // Explicit String
```

Dart is **strongly typed**, but uses type inference with `var` when the type is obvious from context.

### 5.2 `final` vs `const`

| Keyword | Set At | Can Change? | Example |
|---|---|---|---|
| `var` / typed | Runtime | ✅ Yes | `int score = 0;` |
| `final` | Runtime (once) | ❌ No | `final name = getName();` |
| `const` | Compile time | ❌ No | `const pi = 3.14;` |

> **📖 Reference:** [Dart language – final and const](https://dart.dev/language/variables#final-and-const)

### 5.3 String Interpolation

You can embed variables directly into strings using the `$` prefix — similar to template literals in JavaScript or `{{ }}` in Laravel Blade:

```dart
int diceNumber = 4;
print('images/dice$diceNumber.png'); // → images/dice4.png

// For expressions, wrap in curly braces:
print('Result: ${diceNumber * 2}'); // → Result: 8
```

> This is exactly how we'll dynamically load dice images!

---

## 6. Making the Dice Change Reactively

Now let's connect everything. We create a variable `leftDieNumber` and use string interpolation to load the correct image:

```dart
var leftDieNumber = 1;
// ...
Image.asset('images/dice$leftDieNumber.png')
```

Changing `leftDieNumber` to `3` would load `dice3.png` — simple and elegant!

---

## 7. Stateless vs. Stateful Widgets

> **📖 References:**
> - [StatelessWidget class](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html)
> - [StatefulWidget class](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html)
> - [Adding interactivity to your Flutter app](https://docs.flutter.dev/ui/interactivity)

### What is "State"?

**State** refers to any data that can change over time and that affects what is displayed on screen. Examples:
- A counter value
- Whether a checkbox is checked
- Which dice image is currently showing

### Stateless Widgets

A `StatelessWidget` is **immutable** — once built, its properties cannot change. It's ideal for static UI elements like `AppBar`, `Text` labels, or layout containers that never change.

```
StatelessWidget → Built once → Never changes
```

### Stateful Widgets

A `StatefulWidget` can **rebuild itself** when its internal state changes. Under the hood, it is split into two classes:

1. **The Widget class** — immutable configuration (like the widget's key)
2. **The State class** — holds mutable data and the `build()` method

```dart
class MyWidget extends StatefulWidget {
  const MyWidget({super.key});

  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}
```

> **💡 Shortcut:** In VS Code or Android Studio, type `stful` and press `Enter` to generate a `StatefulWidget` scaffold automatically.

### Why Did We Get an Immutable Warning?

When we added `int leftDieNumber = 6;` as an instance variable to our `StatelessWidget`, the IDE warned us:

```
This class is marked as '@immutable', but one or more of its instance
fields aren't final: DicePage.leftDieNumber
```

This is because a `StatelessWidget` is *not supposed to hold changing data*. The fix is to convert it to a `StatefulWidget`.

---

## 8. Wiring It All Together with `setState()`

> **📖 Reference:** [State.setState method](https://api.flutter.dev/flutter/widgets/State/setState.html)

Here's our fully converted `StatefulWidget`:

```dart
class DicePage extends StatefulWidget {
  const DicePage({super.key});

  @override
  State<DicePage> createState() => _DicePageState();
}

class _DicePageState extends State<DicePage> {
  int leftDieNumber = 1;
  int rightDieNumber = 1;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Row(
        children: [
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: GestureDetector(
                onTap: () {
                  setState(() {
                    leftDieNumber = 3;
                  });
                },
                child: Image.asset('images/dice$leftDieNumber.png'),
              ),
            ),
          ),
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(16.0),
              child: GestureDetector(
                onTap: () {
                  setState(() {
                    rightDieNumber = 3;
                  });
                },
                child: Image.asset('images/dice$rightDieNumber.png'),
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

### Why `setState()`?

Simply changing a variable's value is not enough — Flutter doesn't know it needs to redraw the UI. Calling `setState()` does two things:

1. **Updates the variable** inside the callback
2. **Triggers a rebuild** of the `build()` method, so the UI reflects the new value

```
User taps → setState() called → build() runs again → UI updates ✅
```

> Without `setState()`, the variable changes in memory but the screen stays the same.

---

## 9. Challenge: Add Randomness 🎲

Now that you understand how `setState()` works, try this on your own:

1. Import Dart's `math` library: `import 'dart:math';`
2. Use `Random().nextInt(6) + 1` to generate a number between 1 and 6
3. Call `setState()` on both dice when *either* die is tapped

**Hint:**
```dart
import 'dart:math';

// Inside setState:
leftDieNumber = Random().nextInt(6) + 1;
```

> **📖 Reference:** [dart:math library](https://api.dart.dev/stable/dart-math/dart-math-library.html)

---

## Summary

| Concept | Key Takeaway | Docs |
|---|---|---|
| `Scaffold` | Top-level layout structure for Material apps | [→](https://api.flutter.dev/flutter/material/Scaffold-class.html) |
| `Expanded` | Makes a widget fill available space dynamically | [→](https://api.flutter.dev/flutter/widgets/Expanded-class.html) |
| `Padding` | Adds spacing around a child widget | [→](https://api.flutter.dev/flutter/widgets/Padding-class.html) |
| `Image.asset()` | Shorthand for loading images from the assets folder | [→](https://api.flutter.dev/flutter/widgets/Image/Image.asset.html) |
| `GestureDetector` | Detects taps (and other gestures) on any widget | [→](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html) |
| String Interpolation | `'dice$number.png'` dynamically builds a string | [→](https://dart.dev/language/built-in-types#strings) |
| `StatelessWidget` | Immutable — good for static UI | [→](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html) |
| `StatefulWidget` | Holds mutable state — good for interactive UI | [→](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html) |
| `setState()` | Notifies Flutter to rebuild the UI after a state change | [→](https://api.flutter.dev/flutter/widgets/State/setState.html) |
