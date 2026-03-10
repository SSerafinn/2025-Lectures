# Building a Portfolio App in Flutter
### A Practical Introduction to Layouts, Widgets, and Styling

---

## 1. Starting with a Stateless Widget

Every Flutter app starts with a root widget. Since our portfolio page doesn't need to change or react to user input, we use a [`StatelessWidget`](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html) — it's immutable and perfect for static UI like a profile card.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        backgroundColor: Colors.lightBlueAccent,
        body: Container(),
      ),
    );
  }
}
```

> **Recall:** A `StatelessWidget` is appropriate here because the portfolio content — your name, photo, contact info — doesn't change while the app is running.

---

## 2. The `Container` Widget

Think of [`Container`](https://api.flutter.dev/flutter/widgets/Container-class.html) as Flutter's equivalent of a `<div>` in HTML. It's a versatile box that can hold a single child and be styled with color, size, padding, and margin.

> **📖 Reference:** [Flutter Basic Widgets](https://docs.flutter.dev/ui/widgets/basics)

**Key sizing behavior to remember:**
- A `Container` **with no children** tries to be as **large** as possible (unless constrained)
- A `Container` **with children** sizes itself to **fit its children**
- You can override this with explicit `width` and `height` values

```dart
body: Container(
  color: Colors.white,
  child: Text('Hello!'),
),
```

> **Important:** `Container` only accepts a **single child**. If you need multiple children side by side or stacked, you'll need `Row` or `Column` — covered in Section 4.

---

## 3. `SafeArea`, Padding, and Margins

### SafeArea

[`SafeArea`](https://api.flutter.dev/flutter/widgets/SafeArea-class.html) is a widget that automatically adds padding to avoid system UI intrusions — like the iPhone notch, status bar, or bottom navigation bar. Wrapping your body in `SafeArea` ensures your content is never hidden behind hardware or OS elements.

```dart
body: SafeArea(
  child: Container(...),
),
```

### EdgeInsets — Controlling Spacing

Flutter uses [`EdgeInsets`](https://api.flutter.dev/flutter/painting/EdgeInsets-class.html) for both `padding` (inner spacing) and `margin` (outer spacing). There are several constructors depending on how specific you need to be:

| Constructor | Use Case | Example |
|---|---|---|
| `EdgeInsets.all(v)` | Same on all sides | `EdgeInsets.all(16.0)` |
| `EdgeInsets.symmetric(h, v)` | Horizontal + vertical separately | `EdgeInsets.symmetric(horizontal: 20, vertical: 50)` |
| `EdgeInsets.fromLTRB(l,t,r,b)` | Full control per side | `EdgeInsets.fromLTRB(10, 20, 10, 0)` |
| `EdgeInsets.only(left: v)` | One specific side | `EdgeInsets.only(left: 15)` |

```dart
body: SafeArea(
  child: Container(
    height: 100.0,
    width: 100.0,
    margin: EdgeInsets.symmetric(horizontal: 20.0, vertical: 50.0),
    color: Colors.white,
    child: Text('Hello!', textAlign: TextAlign.center),
  ),
),
```

---

## 4. Multiple Children — `Row` and `Column`

When you need to display more than one widget, `Container` alone won't cut it. Flutter provides two core multi-child layout widgets:

- [`Column`](https://api.flutter.dev/flutter/widgets/Column-class.html) — stacks children **vertically** (top to bottom)
- [`Row`](https://api.flutter.dev/flutter/widgets/Row-class.html) — arranges children **horizontally** (left to right)

### Key Properties

| Property | What It Does |
|---|---|
| `mainAxisAlignment` | Controls spacing **along** the main axis (vertical for Column, horizontal for Row) |
| `crossAxisAlignment` | Controls alignment **across** the main axis |
| `mainAxisSize` | Set to `MainAxisSize.min` to shrink-wrap around children |
| `verticalDirection` | Set to `VerticalDirection.up` to reverse the order of children |

```dart
body: SafeArea(
  child: Column(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      Container(height: 100, width: 100, color: Colors.white,    child: Text('Container 1')),
      Container(height: 100, width: 100, color: Color(0xFF9900FF), child: Text('Container 2')),
      Container(height: 100, width: 100, color: Color(0xFFFF0011), child: Text('Container 3')),
    ],
  ),
),
```

> **Think of it this way:**
> - `mainAxisAlignment` is like `justify-content` in CSS Flexbox
> - `crossAxisAlignment` is like `align-items` in CSS Flexbox

---

## 5. `CrossAxisAlignment` and `SizedBox`

### Stretching Children to Fill Width

If you want a child to stretch across the full width of a `Column`, use:

```dart
crossAxisAlignment: CrossAxisAlignment.stretch,
```

This is equivalent to setting `width: double.infinity` on each child — but cleaner and more idiomatic.

### `SizedBox` for Spacing

[`SizedBox`](https://api.flutter.dev/flutter/widgets/SizedBox-class.html) is the Flutter way to add fixed spacing between elements — similar to adding `margin-bottom` to individual elements in CSS.

```dart
Column(
  children: [
    Container(height: 100, width: 100, color: Colors.white,     child: Text('Container 1')),
    Container(height: 100, width: 100, color: Color(0xFF9900FF), child: Text('Container 2')),
    SizedBox(height: 80.0),   // gap between Container 2 and 3
    Container(height: 100, width: 100, color: Color(0xFFFF0011), child: Text('Container 3')),
  ],
),
```

> **Why `SizedBox` over margin?** Using `SizedBox` for spacing between items in a list is a common Flutter convention — it keeps each widget's own margin/padding clean and predictable.

---

## 6. Building the Portfolio — `CircleAvatar` and `pubspec.yaml`

Now let's start building the actual portfolio UI. We'll add a profile photo using [`CircleAvatar`](https://api.flutter.dev/flutter/material/CircleAvatar-class.html).

### Setting Up Your Image Asset

Before you can use a local image in Flutter, you need to register it in `pubspec.yaml`:

1. Create an `images/` folder in your project root
2. Place your profile photo inside it (e.g., `profile.jpg`)
3. Open `pubspec.yaml` and add the asset path:

```yaml
flutter:
  assets:
    - images/profile.jpg
```

4. Save and run `flutter pub get`

### Using `CircleAvatar`

```dart
CircleAvatar(
  radius: 50,
  backgroundImage: AssetImage('images/profile.jpg'),
),
```

### Adding Name and Title

```dart
Column(
  children: [
    CircleAvatar(
      radius: 50,
      backgroundImage: AssetImage('images/profile.jpg'),
    ),
    Text(
      'Sera Dawgs',
      style: TextStyle(
        fontSize: 40,
        color: Colors.white,
        fontWeight: FontWeight.bold,
      ),
    ),
  ],
),
```

---

## 7. Adding a Custom Font

Custom fonts give your app a distinct personality. Here's how to add one:

1. Create a `fonts/` folder in your project root
2. Place your font file inside (e.g., `Roketto.ttf`)
3. Register it in `pubspec.yaml`:

```yaml
flutter:
  fonts:
    - family: Roketto
      fonts:
        - asset: fonts/Roketto.ttf
```

4. Use it in your `TextStyle`:

```dart
Text(
  'Sera Dawgs',
  style: TextStyle(
    fontFamily: 'Roketto',
    fontSize: 40,
    color: Color.fromARGB(255, 73, 179, 255),
  ),
),
Text(
  'The Developer',
  style: TextStyle(
    fontSize: 15,
    color: Color.fromARGB(255, 73, 179, 255),
  ),
),
```

> **Reference:** [Using custom fonts – Flutter docs](https://docs.flutter.dev/cookbook/design/fonts)

---

## 8. The `Icon` Widget and Contact Cards

Flutter comes with a built-in library of Material Design icons accessible through the [`Icons`](https://api.flutter.dev/flutter/material/Icons-class.html) class — no external packages needed.

> **Reference:** [Icons class](https://api.flutter.dev/flutter/material/Icons-class.html) — browse the full icon catalog here.

We'll use icons to build contact info rows. Each row is a `Container` wrapping a `Row` of an `Icon` and a `Text`:

```dart
Container(
  color: Colors.white,
  margin: EdgeInsets.symmetric(vertical: 10, horizontal: 30),
  padding: EdgeInsets.all(15),
  child: Row(
    children: [
      Icon(Icons.phone, color: Color.fromARGB(255, 73, 179, 255)),
      SizedBox(width: 10.0),
      Text(
        '0999 678 9111',
        style: TextStyle(fontSize: 15, color: Color.fromARGB(255, 73, 179, 255)),
      ),
    ],
  ),
),
Container(
  color: Colors.white,
  margin: EdgeInsets.symmetric(vertical: 10, horizontal: 30),
  padding: EdgeInsets.all(15),
  child: Row(
    children: [
      Icon(Icons.email, color: Color.fromARGB(255, 73, 179, 255)),
      SizedBox(width: 10.0),
      Text(
        'seradawgs@flutter.dev',
        style: TextStyle(fontSize: 15, color: Color.fromARGB(255, 73, 179, 255)),
      ),
    ],
  ),
),
```

---

## 9. Flutter `Card` Widget

The [`Card`](https://api.flutter.dev/flutter/material/Card-class.html) widget is a Material Design surface with a subtle shadow (elevation) and rounded corners. It's a cleaner, more polished alternative to a plain `Container` for displaying grouped information.

> **Important:** `Card` has **no built-in padding**. You need to wrap its child in a `Padding` widget manually.

> **Reference:** [Card class](https://api.flutter.dev/flutter/material/Card-class.html)

### Replacing `Container` with `Card`

```dart
Card(
  color: Colors.white,
  margin: EdgeInsets.symmetric(vertical: 10, horizontal: 30),
  child: Padding(                     // ← always add Padding inside Card
    padding: const EdgeInsets.all(8.0),
    child: Row(
      children: [
        Icon(Icons.phone, color: Color.fromARGB(255, 73, 179, 255)),
        SizedBox(width: 10.0),
        Text(
          '0999 678 9111',
          style: TextStyle(fontSize: 15, color: Color.fromARGB(255, 73, 179, 255)),
        ),
      ],
    ),
  ),
),
Card(
  color: Colors.white,
  margin: EdgeInsets.symmetric(vertical: 10, horizontal: 30),
  child: Padding(
    padding: const EdgeInsets.all(8.0),
    child: Row(
      children: [
        Icon(Icons.email, color: Color.fromARGB(255, 73, 179, 255)),
        SizedBox(width: 10.0),
        Text(
          'seradawgs@flutter.dev',
          style: TextStyle(fontSize: 15, color: Color.fromARGB(255, 73, 179, 255)),
        ),
      ],
    ),
  ),
),
```

### `Container` vs. `Card` — When to Use Which?

| | `Container` | `Card` |
|---|---|---|
| Shadow / Elevation | None by default | Built-in |
| Rounded Corners | Manual via `decoration` | Built-in |
| Built-in Padding | Via `padding` property | Must add manually |
| Best For | General-purpose boxes | Grouped, elevated content |

---

## Summary

| Concept | Key Takeaway | Docs |
|---|---|---|
| `StatelessWidget` | Use for UI that never changes | [Link](https://api.flutter.dev/flutter/widgets/StatelessWidget-class.html) |
| `Container` | Single-child box for styling and sizing | [Link](https://api.flutter.dev/flutter/widgets/Container-class.html) |
| `SafeArea` | Keeps content away from OS/hardware UI | [Link](https://api.flutter.dev/flutter/widgets/SafeArea-class.html) |
| `EdgeInsets` | Controls padding and margin spacing | [Link](https://api.flutter.dev/flutter/painting/EdgeInsets-class.html) |
| `Column` / `Row` | Multi-child vertical / horizontal layouts | [Link](https://api.flutter.dev/flutter/widgets/Column-class.html) |
| `SizedBox` | Fixed spacing between elements | [Link](https://api.flutter.dev/flutter/widgets/SizedBox-class.html) |
| `CircleAvatar` | Displays a circular image or initials | [Link](https://api.flutter.dev/flutter/material/CircleAvatar-class.html) |
| `pubspec.yaml` | Registers assets and fonts for the app | [Link](https://docs.flutter.dev/ui/assets/assets-and-images) |
| `Icons` | Built-in Material Design icon library | [Link](https://api.flutter.dev/flutter/material/Icons-class.html) |
| `Card` | Elevated, rounded surface for grouped content | [Link](https://api.flutter.dev/flutter/material/Card-class.html) |
