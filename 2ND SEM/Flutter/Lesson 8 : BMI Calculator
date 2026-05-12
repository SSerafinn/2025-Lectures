# BMI Calculator App
---

## Overview

In this handout, you will build a **BMI Calculator** app — a two-screen Flutter application where the user inputs their gender, height, weight, and age, taps Calculate, and is taken to a results screen showing their BMI score and health interpretation.

This guide will walk you through the app **step by step**. Read each section carefully before writing any code.

---

## What You Will Learn

| Concept | Description |
|---|---|
| **Flutter Themes** | Apply a consistent color palette and text style across the entire app |
| **Custom Widget Classes** | Build reusable widgets as their own classes, not just helper methods |
| **Multi-Screen Navigation** | Move between screens using `Navigator.push()` |
| **`Slider` Widget** | A draggable input for selecting height |
| **`GestureDetector` for selection** | Toggle between Male and Female cards |
| **Separate file structure** | Keep UI, logic, and constants in their own files |

---

## Project File Structure

Before writing any code, set up your project files:

```
lib/
├── main.dart              ← App entry, theme, and route to InputPage
├── input_page.dart        ← Screen 1 — gender, height, weight, age inputs
├── results_page.dart      ← Screen 2 — BMI score and interpretation
├── calculator_brain.dart  ← BMI calculation logic
└── constants.dart         ← Colors, text styles, reused values
```

Create each file now in your `lib/` folder before proceeding.

---

## Part 1 — Constants and Theme (`constants.dart` and `main.dart`)

### Why Constants?

When building a styled app, you will use the same colors and text styles in many places. Instead of typing `Color(0xFF1D1E33)` every time you need the background color, you declare it once as a constant and reference it by name everywhere. This means if the color ever changes, you update it in one place and the entire app updates.

> **💡 `const`** declares a compile-time constant — a value that never changes and is determined before the app even runs. It is more memory-efficient than `final` for simple values like colors and numbers.

### `constants.dart`

```dart
import 'package:flutter/material.dart';

// Colors
const kBackgroundColor    = Color(0xFF111328);
const kCardColor          = Color(0xFF1D1E33);
const kActiveCardColor    = Color(0xFF1D1E33);
const kInactiveCardColor  = Color(0xFF111328);
const kBottomBarColor     = Color(0xFFEB1555);
const kSliderActiveColor  = Color(0xFFEB1555);
const kSliderInactiveColor= Color(0xFF8D8E98);

// Numbers
const kBottomBarHeight    = 60.0;

// Text Styles
const kLabelTextStyle = TextStyle(
  fontSize: 18.0,
  color: Color(0xFF8D8E98),
  letterSpacing: 1.5,
);

const kNumberTextStyle = TextStyle(
  fontSize: 50.0,
  fontWeight: FontWeight.w900,
  color: Colors.white,
);

const kTitleTextStyle = TextStyle(
  fontSize: 50.0,
  fontWeight: FontWeight.bold,
  color: Colors.white,
);

const kResultTextStyle = TextStyle(
  fontSize: 22.0,
  fontWeight: FontWeight.bold,
  color: Color(0xFF24D876),
);

const kBMITextStyle = TextStyle(
  fontSize: 100.0,
  fontWeight: FontWeight.bold,
  color: Colors.white,
);

const kBodyTextStyle = TextStyle(
  fontSize: 22.0,
  color: Colors.white,
);
```

---

### Flutter Themes — `main.dart`

A **theme** in Flutter is a set of visual properties — colors, fonts, button styles — applied globally to the entire app through `MaterialApp`. Instead of styling every widget individually, you define the look of your app once in the theme and Flutter applies it automatically to all matching widgets.

Think of it like a CSS stylesheet for your entire app.

```dart
import 'package:flutter/material.dart';
import 'input_page.dart';
import 'constants.dart';

void main() => runApp(const BMICalculator());

class BMICalculator extends StatelessWidget {
  const BMICalculator({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        // App-wide background color
        scaffoldBackgroundColor: kBackgroundColor,

        // AppBar style
        appBarTheme: AppBarTheme(
          backgroundColor: kBackgroundColor,
          titleTextStyle: TextStyle(
            fontSize: 20.0,
            fontWeight: FontWeight.bold,
            letterSpacing: 2.0,
            color: Colors.white,
          ),
          centerTitle: true,
          elevation: 0,
        ),

        // Slider colors
        sliderTheme: SliderThemeData(
          thumbColor: kSliderActiveColor,
          activeTrackColor: kSliderActiveColor,
          inactiveTrackColor: kSliderInactiveColor,
          overlayColor: kSliderActiveColor.withOpacity(0.2),
          thumbShape: RoundSliderThumbShape(enabledThumbRadius: 15.0),
          overlayShape: RoundSliderOverlayShape(overlayRadius: 28.0),
        ),
      ),
      home: InputPage(),
    );
  }
}
```

> **💡 `ThemeData`** is the class that holds all theme configuration. You only write it once inside `MaterialApp` and it applies to the entire widget tree below it. This is how professional Flutter apps maintain visual consistency without repeating style code everywhere.

---

## Part 2 — Reusable Widget Classes

### Custom Widget Classes vs. Helper Methods

In the Xylophone app, we created **helper methods** like `buildKey()` that returned widgets. That is fine for simple cases, but as components get more complex, a better approach is to create a dedicated **widget class**.

A custom widget class is a `StatelessWidget` (or `StatefulWidget`) with a **constructor** — so you can pass data into it and reuse it with different configurations anywhere in the app. This is OOP applied directly to UI components.

> **💡 Key difference:**
> - Helper method → function inside `_State` class, can only be used there
> - Custom widget class → standalone widget, can be imported and used anywhere in the project

---

### The `ReusableCard` Widget

Many parts of the UI are dark rounded cards. Instead of writing `Container` with `BoxDecoration` every time, we build a `ReusableCard` class once.

Create this inside `input_page.dart` (or its own file if you prefer):

```dart
class ReusableCard extends StatelessWidget {
  final Color colour;
  final Widget? cardChild;
  final VoidCallback? onPress;

  const ReusableCard({
    super.key,
    required this.colour,
    this.cardChild,
    this.onPress,
  });

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: onPress,
      child: Container(
        margin: EdgeInsets.all(8.0),
        decoration: BoxDecoration(
          color: colour,
          borderRadius: BorderRadius.circular(10.0),
        ),
        child: cardChild,
      ),
    );
  }
}
```

> **💡 `VoidCallback?`** is a Dart type for a function that takes no arguments and returns nothing. The `?` makes it nullable — the card works even without a tap handler. This is the same pattern as `onPressed: null` disabling a button.

---

### The `GenderCard` Widget

The Male and Female cards share the same structure — an icon and a label — but differ in content. We make a `GenderCard` widget:

```dart
class GenderCard extends StatelessWidget {
  final IconData genderIcon;
  final String genderLabel;

  const GenderCard({
    super.key,
    required this.genderIcon,
    required this.genderLabel,
  });

  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Icon(genderIcon, size: 80.0, color: Colors.white),
        SizedBox(height: 15.0),
        Text(genderLabel, style: kLabelTextStyle),
      ],
    );
  }
}
```

---

### The `RoundIconButton` Widget

The `+` and `−` buttons for weight and age are circular icon buttons. We build them as a reusable class:

```dart
class RoundIconButton extends StatelessWidget {
  final IconData icon;
  final VoidCallback onPressed;

  const RoundIconButton({
    super.key,
    required this.icon,
    required this.onPressed,
  });

  @override
  Widget build(BuildContext context) {
    return RawMaterialButton(
      onPressed: onPressed,
      shape: CircleBorder(),
      fillColor: Color(0xFF4C4F5E),
      constraints: BoxConstraints.tightFor(width: 56.0, height: 56.0),
      child: Icon(icon, color: Colors.white),
    );
  }
}
```

> **💡 `RawMaterialButton`** gives full control over shape and fill color — it's more flexible than `ElevatedButton` when you need a non-rectangular button.

---

## Part 3 — Input Page (`input_page.dart`)

### State Variables

The input page tracks five pieces of state:

```dart
// Gender selection — 0 = none, 1 = male, 2 = female
int selectedGender = 0;

// Height in cm
int height = 170;

// Weight in kg
int weight = 60;

// Age in years
int age = 25;
```

---

### Gender Selection Logic

When the user taps a gender card, we update `selectedGender` and change the card's color to show which one is active:

```dart
// Helper to determine card color based on selection
Color getGenderCardColor(int gender) {
  return selectedGender == gender ? kActiveCardColor : kInactiveCardColor;
}
```

Inside `setState()`:

```dart
// When Male is tapped
onPress: () {
  setState(() { selectedGender = 1; });
},

// When Female is tapped
onPress: () {
  setState(() { selectedGender = 2; });
},
```

---

### The Full Input Page Layout

```dart
import 'package:flutter/material.dart';
import 'constants.dart';
import 'results_page.dart';
import 'calculator_brain.dart';

// Paste your ReusableCard, GenderCard, RoundIconButton classes here

class InputPage extends StatefulWidget {
  const InputPage({super.key});

  @override
  State<InputPage> createState() => _InputPageState();
}

class _InputPageState extends State<InputPage> {
  int selectedGender = 0;
  int height = 170;
  int weight = 60;
  int age = 25;

  Color getGenderCardColor(int gender) {
    return selectedGender == gender ? kActiveCardColor : kInactiveCardColor;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('BMI CALCULATOR')),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.stretch,
        children: [

          // ── Gender Row ──────────────────────
          Expanded(
            child: Row(
              children: [
                Expanded(
                  child: ReusableCard(
                    colour: getGenderCardColor(1),
                    onPress: () => setState(() => selectedGender = 1),
                    cardChild: GenderCard(
                      genderIcon: Icons.male,
                      genderLabel: 'MALE',
                    ),
                  ),
                ),
                Expanded(
                  child: ReusableCard(
                    colour: getGenderCardColor(2),
                    onPress: () => setState(() => selectedGender = 2),
                    cardChild: GenderCard(
                      genderIcon: Icons.female,
                      genderLabel: 'FEMALE',
                    ),
                  ),
                ),
              ],
            ),
          ),

          // ── Height Card ─────────────────────
          Expanded(
            child: ReusableCard(
              colour: kCardColor,
              cardChild: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text('HEIGHT', style: kLabelTextStyle),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    crossAxisAlignment: CrossAxisAlignment.baseline,
                    textBaseline: TextBaseline.alphabetic,
                    children: [
                      Text(height.toString(), style: kNumberTextStyle),
                      Text('cm', style: kLabelTextStyle),
                    ],
                  ),
                  Slider(
                    value: height.toDouble(),
                    min: 120.0,
                    max: 220.0,
                    onChanged: (double newValue) {
                      setState(() { height = newValue.round(); });
                    },
                  ),
                ],
              ),
            ),
          ),

          // ── Weight and Age Row ───────────────
          Expanded(
            child: Row(
              children: [
                // Weight Card
                Expanded(
                  child: ReusableCard(
                    colour: kCardColor,
                    cardChild: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Text('WEIGHT', style: kLabelTextStyle),
                        Text(weight.toString(), style: kNumberTextStyle),
                        Row(
                          mainAxisAlignment: MainAxisAlignment.center,
                          children: [
                            RoundIconButton(
                              icon: Icons.remove,
                              onPressed: () => setState(() => weight--),
                            ),
                            SizedBox(width: 10.0),
                            RoundIconButton(
                              icon: Icons.add,
                              onPressed: () => setState(() => weight++),
                            ),
                          ],
                        ),
                      ],
                    ),
                  ),
                ),
                // Age Card
                Expanded(
                  child: ReusableCard(
                    colour: kCardColor,
                    cardChild: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        Text('AGE', style: kLabelTextStyle),
                        Text(age.toString(), style: kNumberTextStyle),
                        Row(
                          mainAxisAlignment: MainAxisAlignment.center,
                          children: [
                            RoundIconButton(
                              icon: Icons.remove,
                              onPressed: () => setState(() => age--),
                            ),
                            SizedBox(width: 10.0),
                            RoundIconButton(
                              icon: Icons.add,
                              onPressed: () => setState(() => age++),
                            ),
                          ],
                        ),
                      ],
                    ),
                  ),
                ),
              ],
            ),
          ),

          // ── Calculate Button ─────────────────
          GestureDetector(
            onTap: () {
              CalculatorBrain calc = CalculatorBrain(
                height: height,
                weight: weight,
              );
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => ResultsPage(
                    bmiResult: calc.calculateBMI(),
                    resultText: calc.getResult(),
                    interpretation: calc.getInterpretation(),
                  ),
                ),
              );
            },
            child: Container(
              margin: EdgeInsets.only(top: 10.0),
              height: kBottomBarHeight,
              color: kBottomBarColor,
              child: Center(
                child: Text(
                  'CALCULATE',
                  style: TextStyle(
                    fontSize: 22.0,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                    letterSpacing: 2.0,
                  ),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

---

## Part 4 — BMI Logic (`calculator_brain.dart`)

This class applies **abstraction** — the results page never knows *how* BMI is calculated. It just asks `CalculatorBrain` for the result.

```dart
import 'dart:math';

class CalculatorBrain {
  final int height;
  final int weight;

  CalculatorBrain({required this.height, required this.weight});

  double _bmi = 0;

  String calculateBMI() {
    _bmi = weight / pow(height / 100, 2);
    return _bmi.toStringAsFixed(1);
  }

  String getResult() {
    if (_bmi >= 25) return 'OVERWEIGHT';
    if (_bmi >= 18.5) return 'NORMAL';
    return 'UNDERWEIGHT';
  }

  String getInterpretation() {
    if (_bmi >= 25) {
      return 'You have a higher than normal body weight. Try to exercise more.';
    } else if (_bmi >= 18.5) {
      return 'You have a normal body weight. Good job!';
    } else {
      return 'You have a lower than normal body weight. Try to eat more.';
    }
  }
}
```

> **💡 `dart:math`** is a built-in Dart library that provides mathematical functions like `pow()` (raise a number to a power). Import it at the top of the file with `import 'dart:math';`.

> **💡 `toStringAsFixed(1)`** formats a `double` to a fixed number of decimal places. `(28.4123).toStringAsFixed(1)` returns `'28.4'`.

---

## Part 5 — Multi-Screen Navigation

### How `Navigator` Works

Flutter uses a **navigation stack** to manage screens — like a stack of cards. The screen on top is what the user sees.

- `Navigator.push()` — adds a new screen on top of the stack (go forward)
- `Navigator.pop()` — removes the top screen (go back)

```dart
// Going forward to ResultsPage
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => ResultsPage(
      bmiResult: calc.calculateBMI(),
      resultText: calc.getResult(),
      interpretation: calc.getInterpretation(),
    ),
  ),
);

// Going back from ResultsPage
Navigator.pop(context);
```

> **💡 `MaterialPageRoute`** wraps the destination screen and provides the platform-appropriate transition animation (slide on Android, slide up on iOS).

---

## Part 6 — Results Page (`results_page.dart`)

The results page receives three pieces of data from the input page through its constructor:

```dart
import 'package:flutter/material.dart';
import 'constants.dart';

class ResultsPage extends StatelessWidget {
  final String bmiResult;
  final String resultText;
  final String interpretation;

  const ResultsPage({
    super.key,
    required this.bmiResult,
    required this.resultText,
    required this.interpretation,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('BMI CALCULATOR')),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        crossAxisAlignment: CrossAxisAlignment.stretch,
        children: [

          // Title
          Padding(
            padding: EdgeInsets.all(15.0),
            child: Text('Your Result', style: kTitleTextStyle),
          ),

          // Result Card
          Expanded(
            child: Container(
              margin: EdgeInsets.all(15.0),
              decoration: BoxDecoration(
                color: Color(0xFF1D1E33),
                borderRadius: BorderRadius.circular(10.0),
              ),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                crossAxisAlignment: CrossAxisAlignment.center,
                children: [
                  Text(resultText, style: kResultTextStyle),
                  Text(bmiResult, style: kBMITextStyle),
                  Padding(
                    padding: EdgeInsets.symmetric(horizontal: 20.0),
                    child: Text(
                      interpretation,
                      textAlign: TextAlign.center,
                      style: kBodyTextStyle,
                    ),
                  ),
                ],
              ),
            ),
          ),

          // Re-Calculate Button
          GestureDetector(
            onTap: () => Navigator.pop(context),
            child: Container(
              margin: EdgeInsets.only(top: 10.0),
              height: kBottomBarHeight,
              color: kBottomBarColor,
              child: Center(
                child: Text(
                  'RE-CALCULATE',
                  style: TextStyle(
                    fontSize: 22.0,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                    letterSpacing: 2.0,
                  ),
                ),
              ),
            ),
          ),
        ],
      ),
    );
  }
}
```

---

## Build Order Checklist

Follow this order to avoid errors:

- [ ] **Step 1** — Create all 5 files in `lib/`
- [ ] **Step 2** — Write `constants.dart` — colors and text styles
- [ ] **Step 3** — Write `main.dart` — theme and app entry
- [ ] **Step 4** — Write `calculator_brain.dart` — BMI logic
- [ ] **Step 5** — Write the custom widget classes (`ReusableCard`, `GenderCard`, `RoundIconButton`) in `input_page.dart`
- [ ] **Step 6** — Write the `InputPage` UI in `input_page.dart`
- [ ] **Step 7** — Write `results_page.dart`
- [ ] **Step 8** — Run the app and test all inputs and both screens

---

## Key Concepts Summary

| Concept | Key Takeaway |
|---|---|
| **`ThemeData`** | Defines a global visual style applied to the entire app |
| **`constants.dart`** | Centralizes colors, numbers, and text styles for reuse |
| **Custom widget class** | A `StatelessWidget` with a constructor — more reusable than a helper method |
| **`VoidCallback`** | A function type that takes no arguments and returns nothing |
| **`Slider`** | A draggable widget for selecting a value within a range |
| **`RawMaterialButton`** | A fully customizable button — useful for non-rectangular shapes |
| **`Navigator.push()`** | Adds a new screen on top of the navigation stack |
| **`Navigator.pop()`** | Removes the current screen and returns to the previous one |
| **`MaterialPageRoute`** | Wraps a screen for navigation with a transition animation |
| **`dart:math`** | Built-in library for math operations like `pow()` |
| **`toStringAsFixed(n)`** | Formats a double to `n` decimal places |
| **`CalculatorBrain`** | Applies abstraction — keeps BMI logic out of the UI layer |

---

> 💡 **Note from your instructor:** Follow the handout section by section. Build one part at a time and run the app frequently to catch errors early. If you get stuck, re-read the explanation above the code — the *why* is just as important as the *what*. See you next session!
