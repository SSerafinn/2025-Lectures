# Building a Xylophone App in Flutter
### Flutter Packages, Audio Playback, and Cleaner Code with Functions

---

## 1. Flutter Packages

So far, everything we've built has used widgets that come built into Flutter. But what if you need functionality that Flutter doesn't provide out of the box — like playing audio?

This is where **Flutter packages** come in. A package is a bundle of pre-written code made by the Flutter community that you can plug into your project to add new features without building them from scratch.

All Flutter packages are hosted on **[pub.dev](https://pub.dev)** — the official package registry for Dart and Flutter. You can search for any package there, read its documentation, and find out exactly how to install and use it.

### How to Add a Package

1. Find the package on [pub.dev](https://pub.dev)
2. Copy the package name and version
3. Add it under `dependencies:` in your `pubspec.yaml`
4. Run `flutter pub get` to download it
5. Import it in your `.dart` file and use it

> **📖 Reference:** [Using packages – Flutter docs](https://docs.flutter.dev/packages-and-plugins/using-packages)

---

## 2. Adding the Audio Player Package

For our Xylophone app we need to play audio files when the user taps a key. We'll use the [`audioplayers`](https://pub.dev/packages/audioplayers) package for this.

### Step 1 — Update `pubspec.yaml`

```yaml
dependencies:
  flutter:
    sdk: flutter
  cupertino_icons: ^1.0.8
  audioplayers: ^6.0.0
```

After saving, run `flutter pub get` in the terminal.

### Step 2 — Import the Package

```dart
import 'package:audioplayers/audioplayers.dart';
```

### Step 3 — Register Your Sound Files

Place your audio files in a `sounds/` folder, then register it in `pubspec.yaml`:

```yaml
flutter:
  assets:
    - sounds/
```

---

## 3. Setting Up the App

We start with a `StatefulWidget` and a single test button to make sure audio playback works before building the full UI.

```dart
import 'package:flutter/material.dart';
import 'package:audioplayers/audioplayers.dart';

void main() {
  return runApp(
    MaterialApp(home: Scaffold(body: SafeArea(child: XylophonePage()))),
  );
}

class XylophonePage extends StatefulWidget {
  const XylophonePage({super.key});

  @override
  State<XylophonePage> createState() => _XylophonePageState();
}

class _XylophonePageState extends State<XylophonePage> {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: ElevatedButton(
        onPressed: () async {
          final player = AudioPlayer();
          await player.play(AssetSource('sounds/note1.wav'));
        },
        child: Text('Play'),
      ),
    );
  }
}
```

> **💡 `async` / `await`:** Playing audio is an **asynchronous** operation — it takes a moment to load and play the file. Marking a function as `async` and using `await` before `player.play()` tells Dart to wait for the audio to finish loading before moving on. This prevents the app from crashing or skipping the sound.

---

## 4. Extracting a `playSound()` Function

Right now our audio logic lives directly inside `onPressed`. As we build more buttons, duplicating those two lines for every key would be messy. Instead, let's extract it into its own function:

```dart
void playSound() async {
  final player = AudioPlayer();
  await player.play(AssetSource('sounds/note1.wav'));
}
```

Then call it from the button:

```dart
onPressed: () {
  playSound();
},
```

> **💡 Why extract functions?** This is the **DRY principle** — Don't Repeat Yourself. Instead of writing the same logic in multiple places, we write it once in a function and call that function wherever we need it. If the logic ever needs to change, we only update it in one place.

---

## 5. Function Parameters — Making `playSound()` Dynamic

Right now `playSound()` always plays `note1.wav`. We need it to play a different note depending on which key is tapped. We do this by adding a **parameter** to the function:

```dart
void playSound(int soundNumber) async {
  final player = AudioPlayer();
  await player.play(AssetSource('sounds/note$soundNumber.wav'));
}
```

Now each button passes its own note number:

```dart
// Button 1
onPressed: () { playSound(1); }

// Button 2
onPressed: () { playSound(2); }
```

> **💡 Parameters vs. Arguments:**
> - A **parameter** is the variable declared in the function definition — `int soundNumber`
> - An **argument** is the actual value passed when calling the function — `playSound(2)`
>
> We use string interpolation `'sounds/note$soundNumber.wav'` to dynamically build the file path — the same technique we used in the Dice App with `'images/dice$diceNumber.png'`.

---

## 6. Styling — Full Screen Keys

A real xylophone fills the screen with keys from top to bottom. We use `Expanded` inside a `Column` with `crossAxisAlignment: CrossAxisAlignment.stretch` to make each button fill the full width and share the screen height equally.

```dart
Widget build(BuildContext context) {
  return Center(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        Expanded(
          child: Padding(
            padding: const EdgeInsets.all(2.0),
            child: ElevatedButton(
              style: ElevatedButton.styleFrom(
                backgroundColor: Color.fromARGB(255, 255, 72, 72),
              ),
              onPressed: () { playSound(1); },
              child: Text(''),
            ),
          ),
        ),
        // ... repeat for notes 2–7
      ],
    ),
  );
}
```

> **💡 `crossAxisAlignment: CrossAxisAlignment.stretch`** forces each child of the `Column` to stretch to the full width of the screen — the same concept we covered in the Portfolio App lesson.

> **💡 `Expanded` inside a `Column`** works the same way it does inside a `Row` — each key takes an equal share of the available vertical space, making the layout responsive across all screen sizes.

---

## 7. Reducing Redundancy — Widget Builder Functions

Looking at the full app, we notice that all 7 keys follow the exact same structure — only the `color` and `soundNumber` are different. Writing that same `Expanded → Padding → ElevatedButton` tree seven times is repetitive and hard to maintain.

The solution is a **widget builder function** — a function that takes parameters and returns a widget:

```dart
Widget buildKey({required Color color, required int soundNumber}) {
  return Expanded(
    child: Padding(
      padding: const EdgeInsets.all(2.0),
      child: ElevatedButton(
        style: ElevatedButton.styleFrom(backgroundColor: color),
        onPressed: () { playSound(soundNumber); },
        child: Text(''),
      ),
    ),
  );
}
```

> **💡 Named parameters** use `{curly braces}` and the `required` keyword. Unlike positional parameters, named parameters can be passed in any order and make the function call much more readable — you can clearly see what each argument means.

### The Result — Clean, Readable Code

```dart
Widget build(BuildContext context) {
  return Center(
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: [
        buildKey(color: Color.fromARGB(255, 255, 72,  72),  soundNumber: 1),
        buildKey(color: Color.fromARGB(255, 255, 154, 72),  soundNumber: 2),
        buildKey(color: Color.fromARGB(255, 255, 231, 72),  soundNumber: 3),
        buildKey(color: Color.fromARGB(255, 139, 255, 72),  soundNumber: 4),
        buildKey(color: Color.fromARGB(255, 72,  255, 215), soundNumber: 5),
        buildKey(color: Color.fromARGB(255, 118, 72,  255), soundNumber: 6),
        buildKey(color: Color.fromARGB(255, 255, 72,  151), soundNumber: 7),
      ],
    ),
  );
}
```

Before `buildKey()`, adding a new note required copying ~10 lines of code. Now it's a single line. This is the power of **refactoring** — restructuring code to be cleaner without changing what it does.

---

## Summary

| Concept | Key Takeaway | Docs |
|---|---|---|
| Flutter Packages | Pre-built code from `pub.dev` that extends Flutter's functionality | [→](https://pub.dev) |
| `pubspec.yaml` dependencies | Declare packages under `dependencies:` then run `flutter pub get` | [→](https://docs.flutter.dev/packages-and-plugins/using-packages) |
| `audioplayers` | Package for playing audio from assets or network sources | [→](https://pub.dev/packages/audioplayers) |
| `async` / `await` | Handles asynchronous operations like loading and playing audio | [→](https://dart.dev/language/async) |
| Function parameters | Pass values into a function to make it dynamic and reusable | [→](https://dart.dev/language/functions#parameters) |
| Named parameters | `{required Type name}` — makes function calls more readable | [→](https://dart.dev/language/functions#named-parameters) |
| Widget builder functions | Functions that return widgets — reduces repetition in `build()` | [→](https://docs.flutter.dev/perf/best-practices) |
| `CrossAxisAlignment.stretch` | Stretches children to fill the full width of a `Column` | [→](https://api.flutter.dev/flutter/rendering/CrossAxisAlignment.html) |

---

## Challenge 🎵

1. **Note labels** — Add the note name (Do, Re, Mi, Fa, Sol, La, Ti) as the `Text` label inside each key.
2. **Key width variation** — Real xylophone keys get shorter from top to bottom. Can you use the `flex` property on each `Expanded` to simulate this effect?
3. **Simultaneous notes** — Right now each tap creates a new `AudioPlayer` instance. What happens if you tap two keys very quickly? Research how to handle overlapping audio using the `audioplayers` package.
