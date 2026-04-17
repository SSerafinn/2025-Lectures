# Building a Quiz App in Flutter
### Introducing Object-Oriented Programming — Classes, Encapsulation, and Abstraction

---

## 1. Project Setup

We start with the provided starter code. Notice the UI is already scaffolded — the question text, True button, False button, and a placeholder for the score keeper are all in place. Our job is to wire everything up with logic and OOP principles.

```dart
import 'package:flutter/material.dart';

void main() => runApp(const Quizzler());

class Quizzler extends StatelessWidget {
  const Quizzler({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        backgroundColor: Colors.grey.shade900,
        body: const SafeArea(
          child: Padding(
            padding: EdgeInsets.symmetric(horizontal: 10.0),
            child: QuizPage(),
          ),
        ),
      ),
    );
  }
}

class QuizPage extends StatefulWidget {
  const QuizPage({super.key});

  @override
  State<QuizPage> createState() => _QuizPageState();
}

class _QuizPageState extends State<QuizPage> {
  @override
  Widget build(BuildContext context) {
    return Column(
      mainAxisAlignment: MainAxisAlignment.spaceBetween,
      crossAxisAlignment: CrossAxisAlignment.stretch,
      children: <Widget>[
        Expanded(
          flex: 5,
          child: Padding(
            padding: EdgeInsets.all(10.0),
            child: Center(
              child: Text(
                'This is where the question text will go.',
                textAlign: TextAlign.center,
                style: TextStyle(fontSize: 25.0, color: Colors.white),
              ),
            ),
          ),
        ),
        Expanded(
          child: Padding(
            padding: const EdgeInsets.all(15.0),
            child: TextButton(
              style: TextButton.styleFrom(
                backgroundColor: Colors.green,
                shape: const RoundedRectangleBorder(
                  borderRadius: BorderRadius.zero,
                ),
              ),
              onPressed: () {},
              child: const Text('True', style: TextStyle(color: Colors.white, fontSize: 20.0)),
            ),
          ),
        ),
        Expanded(
          child: Padding(
            padding: const EdgeInsets.all(15.0),
            child: TextButton(
              style: TextButton.styleFrom(
                backgroundColor: Colors.red,
                shape: const RoundedRectangleBorder(
                  borderRadius: BorderRadius.zero,
                ),
              ),
              onPressed: () {},
              child: const Text('False', style: TextStyle(fontSize: 20.0, color: Colors.white)),
            ),
          ),
        ),
        // Score keeper row goes here
      ],
    );
  }
}
```

---

## 2. The Score Keeper — Introducing Lists

Before wiring up the quiz logic, we need a way to display whether the player answered correctly. We'll use a `Row` of icons at the bottom of the screen.

Here is what a static row of icons looks like:

```dart
Row(
  children: [
    Icon(Icons.check, color: Colors.lightGreenAccent),
    Icon(Icons.close, color: Colors.redAccent),
    Icon(Icons.close, color: Colors.redAccent),
    Icon(Icons.close, color: Colors.redAccent),
  ],
)
```

> **💡 `children` vs `child`:** Some widgets only take a `child` (singular) — like `Padding`, `Center`, and `Expanded` — because they hold one widget. Widgets like `Row` and `Column` take `children` (plural) because they hold a **list** of widgets. A list is simply a way of grouping multiple items of the same type together.

---

## 3. Dart Lists — A Quick Review

A **List** in Dart is an ordered collection of items. All items in a typed list must share the same type.

```dart
// A list of strings
List<String> fruits = ['Apple', 'Banana', 'Mango'];

// Accessing by index (starts at 0)
print(fruits[0]); // Apple
print(fruits[2]); // Mango

// Adding an item
fruits.add('Durian');

// Getting the length
print(fruits.length); // 4
```

> **📖 Reference:** [Dart language — Lists](https://dart.dev/language/collections#lists)

### From Static to Dynamic — A `List<Widget>` Score Keeper

Instead of hardcoding the icons in the `Row`, we declare an empty `List<Widget>` and populate it as the player answers:

```dart
List<Widget> scoreKeeper = [];
```

> **💡 Why `<Widget>`?** Without the type annotation, Dart treats the list as `dynamic` — meaning it could hold anything. Using `List<Widget>` tells Dart this list will only ever hold widgets, which keeps our code safe and predictable.

We add an icon to the list dynamically inside `setState()` when the True button is pressed:

```dart
Expanded(
  child: Padding(
    padding: const EdgeInsets.all(15.0),
    child: ElevatedButton(
      style: ElevatedButton.styleFrom(
        backgroundColor: Colors.green,
        foregroundColor: Colors.white,
        shape: const RoundedRectangleBorder(
          borderRadius: BorderRadius.zero,
        ),
      ),
      onPressed: () {
        setState(() {
          scoreKeeper.add(
            Icon(Icons.check, color: Colors.lightGreenAccent),
          );
        });
      },
      child: const Text('True', style: TextStyle(fontSize: 20.0)),
    ),
  ),
),
```

Then pass the list directly to the `Row`:

```dart
Row(children: scoreKeeper),
```

---

## 4. Displaying Questions

Now let's display actual quiz questions. We store them in a `List<String>`:

```dart
List<String> questions = [
  'You can lead a cow down stairs but not up stairs.',
  'Approximately one quarter of human bones are in the feet.',
  'A slug\'s blood is green.',
];
```

We also need to track which question the player is on:

```dart
int questionNumber = 0;
```

Display the current question by indexing into the list:

```dart
Text(
  questions[questionNumber],
  textAlign: TextAlign.center,
  style: TextStyle(fontSize: 25.0, color: Colors.white),
),
```

**Challenge:** Increment `questionNumber` by 1 every time the player taps a button.

```dart
// Answer:
setState(() {
  scoreKeeper.add(Icon(Icons.check, color: Colors.lightGreenAccent));
  questionNumber++;
});
```

---

## 5. Checking Answers

We add a parallel `List<bool>` that holds the correct answer for each question:

```dart
List<bool> answers = [false, true, true];
```

Inside `onPressed`, we get the correct answer for the current question and check if it matches what the player picked:

```dart
setState(() {
  bool correctAnswer = answers[questionNumber];

  if (correctAnswer == true) {
    print('Tama!');
  } else {
    print('Mali Po');
  }

  scoreKeeper.add(Icon(Icons.check, color: Colors.lightGreenAccent));
  questionNumber++;
});
```

---

## 6. Dart `if` / `else` — A Quick Refresher

An `if/else` statement runs code conditionally — based on whether a boolean expression evaluates to `true` or `false`.

```dart
int score = 85;

if (score >= 90) {
  print('Excellent!');
} else if (score >= 75) {
  print('Passed!');
} else {
  print('Try again.');
}
```

> **💡 Common comparison operators:**
> - `==` equal to
> - `!=` not equal to
> - `>` / `<` greater / less than
> - `>=` / `<=` greater / less than or equal to

> **📖 Reference:** [Dart language — Branches](https://dart.dev/language/branches)

---

## 7. Creating the `Question` Class

Right now we have two separate lists — one for question text, one for answers — linked only by their index. This is fragile. What happens if we add a question but forget to add its matching answer?

The better approach is to **group related data together using a class**.

### Creating `question.dart`

Create a new file in your `lib/` folder called `question.dart`:

```dart
class Question {
  String questionText;
  bool questionAnswer;

  Question({required this.questionText, required this.questionAnswer});
}
```

> **💡 What is a class?** A class is a **blueprint** for creating objects. It defines what properties (data) an object of that type will have.

> **💡 What is a constructor?** A constructor is a special function that runs when you create a new object. It initializes the object's properties with the values you pass in. `{required this.questionText}` is Dart shorthand that automatically assigns the passed value to the property.

### Creating a Single Object

```dart
Question q1 = Question(
  questionText: 'You can lead a cow down stairs but not up stairs.',
  questionAnswer: false,
);

print(q1.questionText);   // You can lead a cow...
print(q1.questionAnswer); // false
```

Notice that both pieces of data — the text and the answer — are now associated together in one object. No more tracking two separate lists!

### Challenge: Create a `List<Question>` Instead

```dart
// Answer:
List<Question> questionBank = [
  Question(questionText: 'You can lead a cow down stairs but not up stairs.', questionAnswer: false),
  Question(questionText: 'Approximately one quarter of human bones are in the feet.', questionAnswer: true),
  Question(questionText: 'A slug\'s blood is green.', questionAnswer: true),
];
```

Update the references in the app to use the new list:

```dart
// Question text
questionBank[questionNumber].questionText

// Correct answer
bool correctAnswer = questionBank[questionNumber].questionAnswer;
```

---

## 8. OOP in Dart — Classes, Objects, and Methods

Before we go further, let's ground ourselves in the theory behind what we just built.

### Class
A **class** is a blueprint or template. It defines what an object looks like and what it can do.

```dart
class Dog {
  String name;
  String breed;

  Dog({required this.name, required this.breed});

  void bark() {
    print('$name says: Woof!');
  }
}
```

### Object
An **object** is a specific instance created from a class.

```dart
Dog d1 = Dog(name: 'Bantay', breed: 'Aspin');
Dog d2 = Dog(name: 'Choco', breed: 'Labrador');

d1.bark(); // Bantay says: Woof!
d2.bark(); // Choco says: Woof!
```

### Method
A **method** is a function that belongs to a class. It defines behaviors the object can perform — like `bark()` above.

> **💡 Analogy:** Think of a class as a **cookie cutter** and objects as the **cookies** made from it. Each cookie has the same shape (structure) but can have different fillings (data).

> **📖 Reference:** [Dart language — Classes](https://dart.dev/language/classes)

---

## 9. Abstraction — Creating `QuizBrain`

Our `main.dart` is getting crowded. It's handling UI, question data, and answer logic all at once. This is where **Abstraction** comes in — each part of your code should have a single, well-defined responsibility.

We move all quiz logic into a dedicated class: `QuizBrain`.

### Creating `quiz_brain.dart`

```dart
import 'question.dart';

class QuizBrain {
  List<Question> questionBank = [
    Question(questionText: 'You can lead a cow down stairs but not up stairs.', questionAnswer: false),
    Question(questionText: 'Approximately one quarter of human bones are in the feet.', questionAnswer: true),
    Question(questionText: 'A slug\'s blood is green.', questionAnswer: true),
  ];
}
```

> **⚠️ Notice the import error:** The class shows an error on `Question` because it doesn't know what that is yet. We fix it by importing `question.dart` at the top — as shown above.

### Using `QuizBrain` in `main.dart`

Import the class and create an object — notice the object name starts with a **lowercase** letter by convention:

```dart
import 'package:quiz_app/quiz_brain.dart';

QuizBrain quizBrain = QuizBrain();
```

Now instead of using the raw `questionBank` list directly, we refer to it through the `quizBrain` object:

```dart
quizBrain.questionBank[questionNumber].questionText
```

> **💡 Abstraction** means giving each class a defined role. `main.dart` handles the UI. `QuizBrain` handles the quiz data. Neither needs to know the internal details of the other.

---

## 10. Encapsulation — Why Direct Access is Dangerous

Now that `questionBank` lives inside `QuizBrain`, notice that nothing is stopping someone from doing this in `main.dart`:

```dart
// Cheating — directly changing an answer!
quizBrain.questionBank[questionNumber].questionAnswer = true;

bool correctAnswer = quizBrain.questionBank[questionNumber].questionAnswer;
```

If we run this, even a `false` answer will return `true` because we changed it directly. This is a problem — `main.dart` should have no business modifying the question data.

This is where **Encapsulation** comes in.

### Making Properties Private

In Dart, prefixing a property name with `_` makes it **private** — accessible only from within the same file:

```dart
import 'question.dart';

class QuizBrain {
  final List<Question> _questionBank = [
    Question(questionText: 'You can lead a cow down stairs but not up stairs.', questionAnswer: false),
    Question(questionText: 'Approximately one quarter of human bones are in the feet.', questionAnswer: true),
    Question(questionText: 'A slug\'s blood is green.', questionAnswer: true),
  ];
}
```

Now if we go back to `main.dart` and try to access `_questionBank` or modify the answers — it no longer works. The data is protected.

### Exposing Data Safely Through Methods

Since `main.dart` can no longer tap into `_questionBank` directly, we create **public methods** inside `QuizBrain` that safely expose only what is needed:

```dart
String getQuestionText(int questionNumber) {
  return _questionBank[questionNumber].questionText;
}
```

Now in `main.dart` instead of:

```dart
quizBrain.questionBank[questionNumber].questionText  // ❌ no longer works
```

We call:

```dart
quizBrain.getQuestionText(questionNumber)  // ✅ safe and controlled
```

**Challenge:** Fix the remaining error in `main.dart` where we get the correct answer. Create a matching method in `QuizBrain`.

```dart
// Answer — add this to quiz_brain.dart:
bool getQuestionAnswer(int questionNumber) {
  return _questionBank[questionNumber].questionAnswer;
}
```

---

## 11. Moving `questionNumber` into `QuizBrain`

There is still one issue — `questionNumber` lives in `main.dart`, which means the UI controls how the quiz progresses. That logic belongs in `QuizBrain`. Let's move it there and make it private too:

```dart
import 'question.dart';

class QuizBrain {
  int _questionNumber = 0;

  final List<Question> _questionBank = [
    Question(questionText: 'You can lead a cow down stairs but not up stairs.', questionAnswer: false),
    Question(questionText: 'Approximately one quarter of human bones are in the feet.', questionAnswer: true),
    Question(questionText: 'A slug\'s blood is green.', questionAnswer: true),
    Question(questionText: 'The tongue is the strongest muscle in the human body.', questionAnswer: false),
    Question(questionText: 'A group of flamingos is called a flamboyance.', questionAnswer: true),
    Question(questionText: 'Honey never spoils.', questionAnswer: true),
    Question(questionText: 'The Great Wall of China is visible from space.', questionAnswer: false),
    Question(questionText: 'Octopuses have three hearts.', questionAnswer: true),
    Question(questionText: 'Humans share 50% of their DNA with bananas.', questionAnswer: true),
    Question(questionText: 'Lightning never strikes the same place twice.', questionAnswer: false),
    Question(questionText: 'A day on Venus is longer than a year on Venus.', questionAnswer: true),
    Question(questionText: 'Goldfish have a memory span of only three seconds.', questionAnswer: false),
    Question(questionText: 'The Eiffel Tower grows taller in summer.', questionAnswer: true),
    Question(questionText: 'Bats are blind.', questionAnswer: false),
    Question(questionText: 'Water can boil and freeze at the same time.', questionAnswer: true),
  ];

  void nextQuestion() {
    if (_questionNumber < _questionBank.length - 1) {
      _questionNumber++;
    }
  }

  String getQuestionText() {
    return _questionBank[_questionNumber].questionText;
  }

  bool getQuestionAnswer() {
    return _questionBank[_questionNumber].questionAnswer;
  }

  bool isFinished() {
    return _questionNumber >= _questionBank.length - 1;
  }

  void reset() {
    _questionNumber = 0;
  }
}
```

Update `main.dart` to use the new methods — no more `questionNumber` in the UI layer:

```dart
quizBrain.getQuestionText()
quizBrain.getQuestionAnswer()
quizBrain.nextQuestion()
```

---

## 12. Inheritance — How Flutter Widgets Work

You may have noticed that every widget we write says either `extends StatelessWidget` or `extends StatefulWidget`. This is **Inheritance** in action.

> **💡 Inheritance** allows a new class to acquire the properties and methods of an existing class. The new class is the **subclass**; the existing class is the **superclass**.

### Kitchen Analogy

```
Staff (superclass)
  └── properties: name, salary
  └── methods: clockIn(), clockOut()

Chef (subclass of Staff)
  └── inherits: name, salary, clockIn(), clockOut()
  └── adds: cookMeal(), createMenu()

Waiter (subclass of Staff)
  └── inherits: name, salary, clockIn(), clockOut()
  └── adds: takeOrder(), serveFood()
```

In Flutter:

```dart
// Flutter's StatefulWidget is like "Staff" — it provides
// the framework plumbing every widget needs.

class QuizPage extends StatefulWidget { ... }
// QuizPage is like "Chef" — it inherits everything from
// StatefulWidget and adds its own build() on top.
```

> **📖 Reference:** [Dart language — Extend a class](https://dart.dev/language/extend)

---

## 13. Polymorphism — The `build()` Method

> **💡 Polymorphism** means "many forms." It allows subclasses to provide their own specific implementation of a method defined in the superclass.

Every widget in Flutter overrides `build()` — the same method name, but each produces a completely different UI:

```dart
// Quizzler's build() — returns a MaterialApp
class Quizzler extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(...);
  }
}

// _QuizPageState's build() — returns a Column
class _QuizPageState extends State<QuizPage> {
  @override
  Widget build(BuildContext context) {
    return Column(...);
  }
}
```

The `@override` annotation tells Dart: *"I am intentionally replacing the superclass version of this method with my own implementation."*

> **💡 In short:** Inheritance says *"I get `build()` from my parent."* Polymorphism says *"But I'm going to implement it my own way."*

---

## 14. Score Keeper Logic — `checkAnswer()`

Now let's clean up the button logic by extracting it into a method inside `_QuizPageState`:

```dart
void checkAnswer(bool userPickedAnswer) {
  bool correctAnswer = quizBrain.getQuestionAnswer();

  if (userPickedAnswer == correctAnswer) {
    scoreKeeper.add(Icon(Icons.check, color: Colors.lightGreenAccent));
  } else {
    scoreKeeper.add(Icon(Icons.close, color: Colors.redAccent));
  }

  quizBrain.nextQuestion();
}
```

Both buttons become clean and symmetrical:

```dart
// True button
onPressed: () {
  setState(() { checkAnswer(true); });
},

// False button
onPressed: () {
  setState(() { checkAnswer(false); });
},
```

---

## 15. Final Challenge — End of Quiz Alert with `rflutter_alert`

When the quiz runs out of questions, we show a popup using the [`rflutter_alert`](https://pub.dev/packages/rflutter_alert) package. When the user taps **Restart**, the quiz resets from the beginning.

### Add to `pubspec.yaml`

```yaml
dependencies:
  flutter:
    sdk: flutter
  rflutter_alert: ^2.0.7
```

Run `flutter pub get`.

### Import in `main.dart`

```dart
import 'package:rflutter_alert/rflutter_alert.dart';
```

### Update `checkAnswer()`

```dart
void checkAnswer(bool userPickedAnswer) {
  bool correctAnswer = quizBrain.getQuestionAnswer();

  if (userPickedAnswer == correctAnswer) {
    scoreKeeper.add(Icon(Icons.check, color: Colors.lightGreenAccent));
  } else {
    scoreKeeper.add(Icon(Icons.close, color: Colors.redAccent));
  }

  if (quizBrain.isFinished()) {
    Alert(
      context: context,
      title: 'Quiz Finished!',
      desc: 'You\'ve completed all the questions.',
      buttons: [
        DialogButton(
          onPressed: () {
            setState(() {
              quizBrain.reset();
              scoreKeeper = [];
            });
            Navigator.pop(context);
          },
          child: Text(
            'Restart',
            style: TextStyle(color: Colors.white, fontSize: 20),
          ),
        ),
      ],
    ).show();
  } else {
    quizBrain.nextQuestion();
  }
}
```

---

## Summary

| Concept | Key Takeaway | Docs |
|---|---|---|
| `List<T>` | An ordered, typed collection of items | [→](https://dart.dev/language/collections#lists) |
| `if` / `else` | Runs code conditionally based on a boolean expression | [→](https://dart.dev/language/branches) |
| Class & Constructor | Blueprint for objects; constructor initializes properties | [→](https://dart.dev/language/classes) |
| Object | A specific instance created from a class | [→](https://dart.dev/language/classes) |
| Abstraction | Each class has one defined responsibility | [→](https://dart.dev/language/classes) |
| Encapsulation | `_` prefix makes properties private and protected | [→](https://dart.dev/language/libraries) |
| Inheritance | Subclass acquires properties and methods from a superclass | [→](https://dart.dev/language/extend) |
| Polymorphism | Subclasses override a parent method with their own version | [→](https://dart.dev/language/extend#overriding-members) |
| `rflutter_alert` | Package for customizable alert dialogs | [→](https://pub.dev/packages/rflutter_alert) |

---

## Challenge 🧠

1. **Score display** — Show the player's final score (e.g. `'You got 11/15 correct!'`) inside the alert dialog when the quiz ends.
2. **Question shuffle** — Add a `shuffle()` method to `QuizBrain` that randomizes the order of questions at the start of each new game.
3. **New topic** — Create a brand new quiz class (e.g. `ScienceQuiz`) with its own 15 questions, and let the player choose which quiz to take from the home screen.
