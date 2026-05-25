# Making Web Pages Come Alive: JavaScript with HTML & CSS

## Lesson Overview

By the end of this lesson, you will be able to:

- Identify the three core web technologies and where each one lives in a web page
- Explain why static HTML alone cannot respond to user actions
- Declare variables and write simple functions in JavaScript
- Connect JavaScript to HTML elements to make a page interactive
- Build a working calculator from scratch

**Key Terms:** boilerplate, DOM, element, event, variable, function, `getElementById`, event listener

---

## Part 1: The Standard HTML, CSS, JS Setup

Every web page is built from three technologies, and each has a job:

- **HTML** is the *structure* — the skeleton. Headings, paragraphs, buttons.
- **CSS** is the *style* — the appearance. Colors, fonts, spacing.
- **JavaScript** is the *behavior* — the brain. It makes things happen when the user does something.

Here is the standard arrangement, or *boilerplate*, that you'll start almost every project with:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My First Page</title>

    <!-- CSS link goes in the HEAD, at the TOP -->
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <!-- The main HTML content goes in the MIDDLE -->
    <h1>Hello World</h1>
    <p>This is a static web page.</p>

    <!-- The JS script goes at the BOTTOM, before </body> -->
    <script src="script.js"></script>
</body>
</html>
```

### Why this order matters

> **CSS at the top:** The browser knows how things should look *before* it draws them, so the page doesn't flash unstyled content.

> **JS at the bottom:** The script runs *after* the HTML above it has loaded. If the script ran first, it would look for buttons and text that don't exist yet — and fail.

This is the single most common beginner mistake: putting the `<script>` in the `<head>` and then wondering why JavaScript "can't find" the elements. **The HTML has to exist before JavaScript can touch it.**

> **Did You Know?** You can write all three languages in a *single* `.html` file using `<style>` tags for CSS and `<script>` tags for JS. For all examples below we'll keep everything in one file to make experimenting easier.

### Self-Check
1. Which of the three technologies controls *behavior*?
2. Why is the `<script>` tag placed at the bottom of the `<body>`?

---

## Part 2: Why Static HTML "Breaks" — and Why That's a Good Thing

Let's look at a page that is completely *static*. Nothing about it can ever change once it loads:

```html
<h1 id="greeting">Good morning!</h1>
<p>The time is fixed. This text will say "Good morning" forever.</p>
```

No matter what the user does — click, type, wait until midnight — this page says "Good morning!" It is frozen.

**This is the limitation JavaScript solves.** HTML and CSS describe a page *as it is at one moment*. JavaScript lets the page *change in response to things* — a click, a typed character, a timer, data from a server.

When we say JavaScript "breaks the static page," we mean it breaks the page *free* from being frozen. It turns a printed poster into a touchscreen.

Here's the smallest possible taste. Don't worry about understanding every word yet — just notice that *clicking does something*:

```html
<h1 id="greeting">Good morning!</h1>
<button onclick="document.getElementById('greeting').innerText = 'Good evening!'">
    Switch greeting
</button>
```

Click the button and the heading changes. The HTML didn't reload. The page *responded*. That responsiveness is what the rest of this lesson is about — and we'll learn to write it properly, not crammed into one line.

### Self-Check
1. In your own words, what does it mean for a page to be "static"?
2. Name two things a user could do that JavaScript can respond to.

---

## Part 3: Variables — Storing Information

Before we make things happen, JavaScript needs a way to *remember* things. That's what a **variable** is: a labeled box that holds a value.

```javascript
let name = "Maria";       // text (a string) — can change later
const pi = 3.14159;       // a number that should never change
let count = 0;            // a number we expect to update
```

- Use `let` when the value **will change** later.
- Use `const` when the value **should stay fixed**.

You can read a variable just by using its name, and update a `let` variable by assigning a new value:

```javascript
let count = 0;
count = count + 1;   // count is now 1
count = count + 1;   // count is now 2
```

> **Did You Know?** `count = count + 1` can be shortened to `count++`. The `++` means "add one to this." You'll see it constantly.

### Self-Check
1. Which keyword would you use for a value that must never change?
2. What is the value of `count` after running `let count = 5; count = count + 3;`?

---

## Part 4: Functions — Reusable Instructions

A **function** is a named block of instructions you can run whenever you want. You *define* it once, then *call* it as many times as you like.

```javascript
function sayHello() {
    console.log("Hello!");   // prints to the browser console
}

sayHello();   // this runs the function — prints "Hello!"
sayHello();   // call it again — prints "Hello!" again
```

Functions can also accept **inputs** (called *parameters*) and hand back a **result** using `return`:

```javascript
function add(a, b) {
    return a + b;
}

let total = add(4, 7);   // total is now 11
```

Think of a function like a recipe: `add` is the recipe, `a` and `b` are the ingredients you pass in, and `return` is the finished dish handed back to you.

### Self-Check
1. What is the difference between *defining* a function and *calling* it?
2. What does the `return` keyword do?

---

## Part 5: Example 1 — A Button That Changes Text

Now we combine everything: HTML for structure, CSS for style, JS for behavior. This is the classic first interactive program.

**The goal:** A heading and a button. Click the button, and the heading's text changes.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Example 1</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; }
        button { padding: 10px 20px; font-size: 16px; cursor: pointer; }
    </style>
</head>
<body>

    <h1 id="message">Click the button below</h1>
    <button id="changeBtn">Change the text</button>

    <script>
        // 1. Grab the heading and the button from the HTML
        const heading = document.getElementById("message");
        const button  = document.getElementById("changeBtn");

        // 2. Define what should happen on click
        function changeText() {
            heading.innerText = "You changed me!";
        }

        // 3. Connect the function to the button's click event
        button.addEventListener("click", changeText);
    </script>
</body>
</html>
```

### How it works, line by line

- `document.getElementById("message")` — finds the `<h1>` whose `id` is `message` and stores it in a variable.
- `function changeText() { ... }` — defines the behavior: set the heading's `innerText` to new text.
- `addEventListener("click", changeText)` — says: *when this button is clicked, run `changeText`.*

`innerText` is simply the text *inside* an element. Reading it gives you the current text; assigning to it replaces the text. This is the bridge between JavaScript and what the user sees.

> **Common mistake:** Writing `changeText()` (with parentheses) inside `addEventListener` runs the function immediately instead of waiting for a click. Pass the *name* only: `changeText`.

### Self-Check
1. What does `getElementById` return?
2. Why do we pass `changeText` without parentheses to `addEventListener`?

---

## Part 6: Example 2 — A Click Counter

Let's add a variable that *remembers* across clicks. This shows why variables matter.

**The goal:** A button that counts how many times it has been clicked.

```html
<h1 id="display">Clicks: 0</h1>
<button id="countBtn">Click me</button>

<script>
    const display = document.getElementById("display");
    const button  = document.getElementById("countBtn");

    let count = 0;   // the memory — survives between clicks

    button.addEventListener("click", function() {
        count++;                              // add one
        display.innerText = "Clicks: " + count;   // show the new number
    });
</script>
```

### What's new here

- `let count = 0;` lives *outside* the click function, so it isn't reset every click — it keeps its value.
- We used an **anonymous function** (a function with no name) directly inside `addEventListener`. This is common for short, one-off behavior.
- `"Clicks: " + count` **joins** text and a number together. This joining is called *concatenation*.

> **Did You Know?** If you put `let count = 0` *inside* the click function, it would reset to 0 on every click and the counter would never go past 1. *Where* you declare a variable controls how long it remembers.

### Self-Check
1. Why is `count` declared outside the click function?
2. What does the `+` do in `"Clicks: " + count`?

---

## Part 7: Example 3 — Reading Input From the User

So far the user only clicks. Now let's read what they *type*. This is the final skill we need before the calculator.

**The goal:** The user types their name, clicks a button, and the page greets them.

```html
<input id="nameInput" type="text" placeholder="Type your name">
<button id="greetBtn">Greet me</button>
<h2 id="output"></h2>

<script>
    const input  = document.getElementById("nameInput");
    const button = document.getElementById("greetBtn");
    const output = document.getElementById("output");

    button.addEventListener("click", function() {
        let name = input.value;                  // read what was typed
        output.innerText = "Hello, " + name + "!";
    });
</script>
```

### Key new idea

- `input.value` reads the **current text inside an input box**. For inputs we use `.value`, not `.innerText`.

That one property — `.value` — is how every form, search bar, and calculator on the web gets its data from the user.

> **Common mistake:** Whatever comes out of `.value` is always *text*, even if the user typed numbers. `"5"` is text, not the number `5`. We'll fix this in the calculator using `Number()`.

### Self-Check
1. Which property reads text from an `<input>` box?
2. If a user types `42` into a text input, is `input.value` a number or text?

---

## Part 8: Example 4 — A Simple Calculator

Now we put it all together: two inputs, a set of buttons, a result. This uses everything from the lesson — variables, functions, reading input, events, and updating the page.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Simple Calculator</title>
    <style>
        body   { font-family: sans-serif; text-align: center; margin-top: 40px; }
        input  { width: 80px; padding: 8px; font-size: 16px; text-align: center; }
        button { padding: 8px 14px; font-size: 16px; margin: 4px; cursor: pointer; }
        #result { font-size: 24px; margin-top: 20px; font-weight: bold; }
    </style>
</head>
<body>

    <h1>Simple Calculator</h1>

    <input id="num1" type="text" placeholder="0">
    <input id="num2" type="text" placeholder="0">

    <div>
        <button id="addBtn">+</button>
        <button id="subBtn">−</button>
        <button id="mulBtn">×</button>
        <button id="divBtn">÷</button>
    </div>

    <p id="result">Result: </p>

    <script>
        // Grab both inputs and the result line
        const num1   = document.getElementById("num1");
        const num2   = document.getElementById("num2");
        const result = document.getElementById("result");

        // A function that takes an operation and shows the answer
        function calculate(operation) {
            // Read the inputs and convert text -> number
            let a = Number(num1.value);
            let b = Number(num2.value);
            let answer;

            if (operation === "add")      answer = a + b;
            else if (operation === "sub") answer = a - b;
            else if (operation === "mul") answer = a * b;
            else if (operation === "div") answer = a / b;

            result.innerText = "Result: " + answer;
        }

        // Wire each button to call calculate with the right operation
        document.getElementById("addBtn").addEventListener("click", function() { calculate("add"); });
        document.getElementById("subBtn").addEventListener("click", function() { calculate("sub"); });
        document.getElementById("mulBtn").addEventListener("click", function() { calculate("mul"); });
        document.getElementById("divBtn").addEventListener("click", function() { calculate("div"); });
    </script>
</body>
</html>
```

### How it ties everything together

- **Reading input:** `num1.value` and `num2.value` grab what the user typed — exactly like Example 3.
- **Converting text to numbers:** `Number(num1.value)` turns `"5"` (text) into `5` (a real number). Without this, `"5" + "3"` would give `"53"` (joined text) instead of `8`.
- **One reusable function:** `calculate(operation)` is written *once* and handles all four buttons by checking which operation was passed in — exactly the point of functions from Part 4.
- **Events:** Each button has its own listener that calls `calculate` with the matching operation.
- **Updating the page:** `result.innerText = ...` shows the answer — the same `innerText` from Example 1.

> **Did You Know?** Notice how every single skill from the lesson appears here. That's intentional. Real programs aren't new magic — they're small, familiar pieces combined.

### Self-Check
1. Why do we wrap `num1.value` in `Number()`?
2. The `calculate` function is written once but used four times. What programming idea does that demonstrate?

---

## Lesson Summary

| Concept | What it does | Example from lesson |
|---|---|---|
| Boilerplate | CSS at top, HTML middle, JS at bottom | Part 1 |
| Static vs dynamic | JS makes frozen pages respond | Part 2 |
| Variable (`let` / `const`) | Stores and remembers a value | `let count = 0` |
| Function | Reusable block of instructions | `calculate(operation)` |
| `getElementById` | Finds an HTML element from JS | every example |
| `innerText` | Reads/changes text in an element | Example 1 |
| `.value` | Reads text from an input box | Examples 3 & 4 |
| `addEventListener` | Runs code when an event happens | every example |
| `Number()` | Converts text into a real number | Calculator |

### Where to go next
- Add a "clear" button to the calculator that empties the inputs and result.
- Handle dividing by zero gracefully (what happens now if `b` is 0?).
- Try changing element *colors* from JavaScript using `element.style.color`.

---

## Answer Key (Self-Check)

**Part 1:** (1) JavaScript. (2) So the HTML elements exist before the script tries to use them.

**Part 2:** (1) A page that cannot change after it loads. (2) Any two: click, type, hover, wait/timer, etc.

**Part 3:** (1) `const`. (2) `8`.

**Part 4:** (1) Defining writes the instructions once; calling actually runs them. (2) It hands a result back to whoever called the function.

**Part 5:** (1) The HTML element with that id. (2) Without parentheses we pass the function itself to run *later* on click; with parentheses it would run immediately.

**Part 6:** (1) So it keeps its value between clicks instead of resetting. (2) It joins (concatenates) the text and the number into one string.

**Part 7:** (1) `.value`. (2) Text (a string).

**Part 8:** (1) To convert the text from the input into a real number so math works correctly. (2) Reusability — write a function once, use it many times.
