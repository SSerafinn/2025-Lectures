# C++ Programming Refresher — Live Coding Lecture Guide
**Course:** CPE | **Format:** Live Coding Walkthrough | **Duration:** ~2–2.5 hrs

---

## Lecture Structure Overview

| # | Topic | Type | Est. Time |
|---|-------|------|-----------|
| 1 | Variables & Data Types | Demo → Try It | 20 min |
| 2 | Operators | Demo → Try It | 20 min |
| 3 | Conditionals | Demo → Try It | 25 min |
| 4 | Loops | Demo → Try It | 25 min |
| 5 | Functions | Demo → Try It | 30 min |
| 6 | Wrap-up Challenge | Student Activity | 15 min |

---

## Setup Reminder (Before Class)
- Have your IDE open (Code::Blocks, VS Code + g++, or online: cpp.sh)
- Start with a blank `main.cpp` — build from scratch live
- Tell students: *"Open your own editor. Type along. Don't just watch."*

---

## Topic 1 — Variables & Data Types
**Learning goal:** Declare and print variables of different types.

### Instructor demo code
Type this live, explaining each line as you go:

```cpp
#include <iostream>
using namespace std;

int main() {
    // Integer — whole numbers
    int age = 20;

    // Double — decimal numbers
    double gwa = 1.75;

    // Char — a single character
    char section = 'A';

    // String — text (needs the header or using namespace std)
    string name = "Maria";

    // Bool — true or false
    bool isEnrolled = true;

    cout << "Name: " << name << endl;
    cout << "Age: " << age << endl;
    cout << "GWA: " << gwa << endl;
    cout << "Section: " << section << endl;
    cout << "Enrolled: " << isEnrolled << endl;

    return 0;
}
```

### Talking points while typing
- "Notice `int` stores no decimal — try storing `1.75` in an `int` and see what happens."
- "A `char` uses single quotes. A `string` uses double quotes. Mixing them is a common error."
- "Try changing `isEnrolled` to `false` — what does cout print? Why?"

### Try it (student turn, ~5 min)
> Declare variables for your own student record: name, ID number, year level, GWA, and whether you passed last semester. Print all of them.

---

## Topic 2 — Operators
**Learning goal:** Use arithmetic, comparison, and logical operators correctly.

### Instructor demo code
Continue in the same file or start fresh:

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, b = 3;

    // Arithmetic
    cout << "Sum: "       << a + b  << endl;
    cout << "Difference: "<< a - b  << endl;
    cout << "Product: "   << a * b  << endl;
    cout << "Quotient: "  << a / b  << endl;   // integer division!
    cout << "Remainder: " << a % b  << endl;

    // What happens with double division?
    double x = 10.0, y = 3.0;
    cout << "Double div: " << x / y << endl;

    // Comparison (returns 1 for true, 0 for false)
    cout << "a > b?  " << (a > b)  << endl;
    cout << "a == b? " << (a == b) << endl;
    cout << "a != b? " << (a != b) << endl;

    // Logical
    bool hasFees = true;
    bool hasGrades = false;
    cout << "Can enroll? " << (hasFees && hasGrades) << endl;
    cout << "Any issue?  " << (hasFees || hasGrades) << endl;

    return 0;
}
```

### Talking points while typing
- "Watch `10 / 3`. It gives `3`, not `3.33`. Integer division truncates — this trips up everyone."
- "To fix it: make at least one operand a `double`. Show `10.0 / 3`."
- "The `%` operator is incredibly useful — show how `n % 2 == 0` detects even numbers."

### Try it (student turn, ~5 min)
> Given a student's total score (`int totalScore = 385`) out of 500, compute and print: the percentage score, whether it's above 75%, and whether the student passed AND has no incomplete grades (`bool noInc = true`).

---

## Topic 3 — Conditionals
**Learning goal:** Use `if/else if/else` and `switch` to control program flow.

### Instructor demo code — grade classifier

```cpp
#include <iostream>
using namespace std;

int main() {
    double grade;
    cout << "Enter grade: ";
    cin >> grade;

    if (grade >= 90) {
        cout << "Excellent — Dean's Lister material!" << endl;
    } else if (grade >= 80) {
        cout << "Good — keep it up." << endl;
    } else if (grade >= 75) {
        cout << "Passed." << endl;
    } else {
        cout << "Below passing. See your instructor." << endl;
    }

    return 0;
}
```

> Run it several times with different inputs: 95, 82, 75, 60.

### Add a switch — year level label

```cpp
    int yearLevel;
    cout << "Enter year level (1-4): ";
    cin >> yearLevel;

    switch (yearLevel) {
        case 1: cout << "Freshman" << endl; break;
        case 2: cout << "Sophomore" << endl; break;
        case 3: cout << "Junior" << endl; break;
        case 4: cout << "Senior" << endl; break;
        default: cout << "Invalid year level." << endl;
    }
```

### Talking points while typing
- "What happens if I remove the `break` statements? Live demo — show fall-through."
- "When do we use `switch` vs `if-else`? Switch is cleaner for fixed discrete values."
- "Ask the class: what would happen if I entered `grade = 75`? Which branch runs?"

### Try it (student turn, ~7 min)
> Write a program that asks for a student's GWA and prints:
> - "Summa Cum Laude" if GWA is 1.00–1.20
> - "Magna Cum Laude" if 1.21–1.45
> - "Cum Laude" if 1.46–1.75
> - "Good Standing" otherwise
>
> (Reminder: in PH grading, lower GWA = better.)

---

## Topic 4 — Loops
**Learning goal:** Use `for`, `while`, and `do-while` to repeat instructions.

### Instructor demo code — for loop

```cpp
#include <iostream>
using namespace std;

int main() {
    // Print numbers 1 to 5
    for (int i = 1; i <= 5; i++) {
        cout << "Count: " << i << endl;
    }

    return 0;
}
```

### Extend live — compute sum of N numbers

```cpp
    int n = 5;
    int sum = 0;

    for (int i = 1; i <= n; i++) {
        sum += i;
    }

    cout << "Sum 1 to " << n << " = " << sum << endl;
```

### While loop — input validation

```cpp
    int attempts = 0;
    int password;

    while (attempts < 3) {
        cout << "Enter PIN: ";
        cin >> password;
        if (password == 1234) {
            cout << "Access granted." << endl;
            break;
        }
        attempts++;
        cout << "Wrong PIN. Attempt " << attempts << " of 3." << endl;
    }

    if (attempts == 3) {
        cout << "Account locked." << endl;
    }
```

### Talking points while typing
- "Point to the three parts of `for (init; condition; update)` and name each one."
- "When do we prefer `while`? When we don't know how many times upfront."
- "What is an infinite loop? Remove `attempts++` and show it — then Ctrl+C to stop."

### Try it (student turn, ~7 min)
> Write a program that:
> 1. Asks the user how many subjects they have
> 2. Loops that many times, asking for each subject's grade
> 3. After the loop, prints the average grade

---

## Topic 5 — Functions
**Learning goal:** Write reusable functions with parameters and return values.

### Instructor demo code — build up step by step

**Step 1:** A simple void function

```cpp
#include <iostream>
using namespace std;

void greet(string name) {
    cout << "Hello, " << name << "! Welcome to CPE." << endl;
}

int main() {
    greet("Maria");
    greet("Juan");
    greet("Ana");
    return 0;
}
```

**Step 2:** A function that returns a value

```cpp
double computeAverage(double s1, double s2, double s3) {
    return (s1 + s2 + s3) / 3.0;
}

int main() {
    double avg = computeAverage(85, 90, 78);
    cout << "Average: " << avg << endl;
    return 0;
}
```

**Step 3:** Function + conditional inside (putting it all together)

```cpp
string getRemarks(double average) {
    if (average >= 90) return "Excellent";
    if (average >= 75) return "Passed";
    return "Failed";
}

int main() {
    double avg = computeAverage(85, 90, 78);
    cout << "Average: " << avg << endl;
    cout << "Remarks: " << getRemarks(avg) << endl;
    return 0;
}
```

### Talking points while typing
- "Notice how `main()` stays clean. Each function does one job."
- "The return type before the function name tells C++ what the function gives back."
- "Functions must be declared before they're called — or use a prototype. Show the error."
- "This is a preview of OOP: in a class, `computeAverage` and `getRemarks` would be *methods* belonging to a `Student` object."

### Try it (student turn, ~10 min)
> Write two functions:
> 1. `double computeGWA(double g1, double g2, double g3, double g4, double g5)` — returns the average of 5 subject grades
> 2. `string getStanding(double gwa)` — returns "Passed", "Failed", or "For Removal" (GWA > 3.00 in PH system)
>
> In `main()`, ask the user to enter 5 grades, call both functions, and print the GWA and standing.

---

## Topic 6 — Wrap-up Coding Challenge
**Time:** ~15 minutes | **Individual or pair work**

### The challenge: Student Record Program

Write a complete C++ program that:

1. Asks the user to input a student's name, year level, and grades for 3 subjects
2. Uses a **function** to compute the average grade
3. Uses an **if-else** to determine pass/fail (passing = average >= 75)
4. Uses a **switch** to print the year level label (Freshman, Sophomore, etc.)
5. Prints a complete summary to the screen

**Expected output:**
```
--- Student Summary ---
Name:       Juan dela Cruz
Year Level: Sophomore
Average:    82.33
Remarks:    Passed
```

### Debrief questions (ask the class)
- "Where did you put your functions — before or after `main()`? What happened if you put them after?"
- "Could we do this whole program without functions? Yes — but why is the function version better?"
- "What would you need to add if this were a `Student` *object* in OOP?"

---

## Instructor Notes

- Keep your editor font large (20pt+) so the back row can follow.
- Deliberately make a typo or two and fix it live — normalizes debugging.
- After each "Try it", cold-call one student to share their screen or read their code.
- The wrap-up challenge doubles as an exit ticket — collect or screen-share 2–3 solutions.
- This entire flow mirrors how a `Student` class in OOP would be structured — mention this connection at the end to prime the next lecture.
