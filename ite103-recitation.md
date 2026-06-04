# ITE 103 – PROGRAMMING 2
## Final Graded Recitation — Question Bank

**St. Paul University Philippines | School of Information Technology and Engineering**

*Coverage: Modules 2–6 (whole-course review). Each module has 11 questions tiered as 3 Easy / 3 Moderate / 3 Hard / 2 Synthesis (cross-module). A final **General Theory (Course-Wide)** section adds 22 broad conceptual items. Questions are tagged **[Theory]** or **[Practical]**. Model answers follow each item for the instructor's reference. — 77 items total.*

---

## MODULE 2 — Review of Python Basics
*(Variables, data types, operators, conditionals, loops, lists, dictionaries, list comprehension, lambda)*

### Easy

**Q1. [Theory]** What is the difference between a **list** and a **dictionary** in Python?
> **Answer:** A list is an ordered collection accessed by integer index (e.g., `items[0]`), allowing duplicate values. A dictionary stores data as **key–value pairs** accessed by key (e.g., `student["name"]`); keys must be unique. Lists are best for sequences; dictionaries are best for labeled/lookup data.

**Q2. [Theory]** Name at least four basic Python data types and give one example of each.
> **Answer:** `int` (5), `float` (3.14), `str` ("hello"), `bool` (True), `list` ([1, 2, 3]), `dict` ({"a": 1}). Any four are acceptable.

**Q3. [Practical]** What is the output?
> ```python
> x = 7
> y = 2
> print(x // y, x % y)
> ```
> **Answer:** `3 1` — `//` is integer (floor) division (7 ÷ 2 = 3), and `%` is the remainder (7 mod 2 = 1).

### Moderate

**Q4. [Theory]** What is the difference between `=` and `==`?
> **Answer:** `=` is the **assignment** operator (stores a value in a variable). `==` is the **comparison** operator (checks if two values are equal and returns `True`/`False`).

**Q5. [Practical]** What does this loop print?
> ```python
> for i in range(1, 6):
>     print(i, end=" ")
> ```
> **Answer:** `1 2 3 4 5` — `range(1, 6)` generates 1 up to but **not including** 6, and `end=" "` keeps output on one line.

**Q6. [Practical]** What is the value of `nums`?
> ```python
> nums = [x * x for x in range(1, 6)]
> print(nums)
> ```
> **Answer:** `[1, 4, 9, 16, 25]` — a list comprehension that squares each number from 1 to 5.

### Hard

**Q7. [Practical]** Trace the output.
> ```python
> student = {"name": "Ana", "grade": 90}
> student["grade"] = 95
> print(student["grade"])
> ```
> **Answer:** `95` — the value for key `"grade"` is reassigned from 90 to 95, then printed.

**Q8. [Practical]** What is the output, and what does this lambda do?
> ```python
> f = lambda a, b: a if a > b else b
> print(f(3, 9))
> ```
> **Answer:** `9` — the lambda returns the larger of two values (a one-line "max" function). Since 3 is not greater than 9, it returns `b`.

**Q9. [Theory]** Explain the difference between `break` and `continue`, with a one-line example each.
> **Answer:** `break` exits the loop entirely (e.g., `if x == 5: break` stops the loop). `continue` skips the rest of the current iteration and jumps to the next (e.g., `if x % 2 == 0: continue` skips even numbers but keeps looping).

### Synthesis (Cross-Module)

**Q10. [Practical]** *(M2 + M4)* You need to read a list of student grades from a text file and find the **highest** grade. Which basics combine with file handling, and how?
> **Answer:** Read the file (M4) and loop through each line (loop, M2); convert each line to a number; track the maximum using a variable and a comparison (`if grade > highest:`). It combines **file reading, type conversion, loops, conditionals, and variables** — the core basics applied to real data.

**Q11. [Practical]** *(M2 + M4)* How would you use a **dictionary** to count how many times each word appears in a text file?
> **Answer:** Read the file and split it into words (M4 + string handling). Loop through the words; for each word, if it's already a key in the dictionary, increment its value, otherwise add it with a value of 1. The dictionary's **key–value structure** makes it ideal for counting — keys are words, values are counts.

---

## MODULE 3 — Functions, Exceptions, and Events
*(Functions, parameters/return values, recursion, exception handling, event-driven programming)*

### Easy

**Q1. [Theory]** What is a function, and why do we use functions in a program?
> **Answer:** A function is a named, reusable block of code that performs a specific task. We use functions for **modularity, reusability, and readability** — they avoid code duplication and make programs easier to test and maintain.

**Q2. [Theory]** What is the difference between a **parameter** and an **argument**?
> **Answer:** A **parameter** is the variable named in the function definition (e.g., `def greet(name):`). An **argument** is the actual value passed in when the function is called (e.g., `greet("Ana")`).

**Q3. [Theory]** What does the `return` statement do, and what happens if a function has no `return`?
> **Answer:** `return` sends a value back to the caller and ends the function. If there is no `return`, the function returns `None` by default.

### Moderate

**Q4. [Theory]** What is a recursive function, and what two essential parts must it always have?
> **Answer:** A recursive function is one that **calls itself**. It must have a **base case** (the stopping condition) and a **recursive case** (the call to itself with a smaller/simpler input). Without a base case it would loop infinitely and crash.

**Q5. [Practical]** What is the output?
> ```python
> def fact(n):
>     if n == 1:
>         return 1
>     return n * fact(n - 1)
>
> print(fact(4))
> ```
> **Answer:** `24` — it computes 4 × 3 × 2 × 1. The base case is `n == 1`.

**Q6. [Theory]** What is exception handling, and why is it important? Name the keywords used.
> **Answer:** Exception handling lets a program **catch and manage runtime errors gracefully** instead of crashing. Keywords: `try`, `except`, `else`, `finally`, and `raise`.

### Hard

**Q7. [Practical]** What is the output?
> ```python
> try:
>     print(10 / 0)
> except ZeroDivisionError:
>     print("Cannot divide by zero")
> ```
> **Answer:** `Cannot divide by zero` — dividing by zero raises a `ZeroDivisionError`, which the `except` block catches.

**Q8. [Practical]** Trace the output.
> ```python
> def total(n):
>     if n == 0:
>         return 0
>     return n + total(n - 1)
>
> print(total(5))
> ```
> **Answer:** `15` — it sums 5 + 4 + 3 + 2 + 1 + 0. The base case is `n == 0`.

**Q9. [Theory]** Explain event-driven programming. Give an example of an event and how the program responds.
> **Answer:** In event-driven programming, the flow is controlled by **events** (user actions or system signals) rather than a fixed top-to-bottom sequence. Example: a user **clicks a button** (the event), which triggers a **callback/handler function** that runs the response. Common in GUIs.

### Synthesis (Cross-Module)

**Q10. [Practical]** *(M3 + M4)* How would you write a **function** that safely reads a number from a CSV cell, using exception handling for bad data?
> **Answer:** Define a function that takes the cell value, then inside a `try` block attempt `float(value)` and return it; in the `except ValueError` block, return a default (e.g., 0) or skip the row. This combines **modular functions, return values, and exception handling** so one bad cell doesn't crash the whole CSV-processing program.

**Q11. [Theory/Practical]** *(M3 + M5)* Why is it good practice to wrap a database connection in `try/except/finally`?
> **Answer:** Database operations can fail at runtime (connection lost, bad query, duplicate key). `try/except` **catches the error gracefully** and shows a useful message instead of crashing; `finally` ensures the connection/cursor is **always closed**, even when an error occurs — preventing leaked connections. It directly applies M3 exception handling to M5 database work.

---

## MODULE 4 — Python File Handling
*(File operations, CRUD on files, CSV manipulation)*

### Easy

**Q1. [Theory]** What does **CRUD** stand for in the context of file/data handling?
> **Answer:** **C**reate, **R**ead, **U**pdate, **D**elete — the four basic operations on stored data.

**Q2. [Theory]** Name the common file modes `'r'`, `'w'`, and `'a'` and what each does.
> **Answer:** `'r'` = read (default; file must exist). `'w'` = write (creates the file, or **overwrites** existing content). `'a'` = append (adds to the end without erasing existing content).

**Q3. [Theory]** Why is it good practice to use the `with` statement when opening a file?
> **Answer:** The `with` statement **automatically closes the file** when the block ends, even if an error occurs. This prevents resource leaks and data loss from unclosed files.

### Moderate

**Q4. [Practical]** A file contains "Hello". What is the difference in the file's contents after opening it with `'w'` versus `'a'` and writing "World"?
> **Answer:** With `'w'`, the file is overwritten and contains only `World`. With `'a'`, the text is appended, giving `HelloWorld`.

**Q5. [Theory]** What is the difference between `read()`, `readline()`, and `readlines()`?
> **Answer:** `read()` returns the **entire file** as one string. `readline()` returns **one line** at a time. `readlines()` returns a **list of all lines**.

**Q6. [Theory]** Which built-in module does Python provide for working with CSV files, and what is a common way to read its rows?
> **Answer:** The **`csv`** module. You typically create a `csv.reader(file)` and loop through it (`for row in reader:`), where each `row` is a list of values; `csv.DictReader` reads rows as dictionaries.

### Hard

**Q7. [Practical]** A CSV file has columns `name, price`. Walk through how you would compute the **total of all prices**.
> **Answer:** Open the file → create a `csv.reader` (skip the header row) → initialize `total = 0` → loop through each row, convert the price column to a number (`float(row[1])`), and add it to `total` → after the loop, print `total`. (Conversion is needed because CSV values are read as strings.)

**Q8. [Practical]** What bug will cause data loss here, and how do you fix it?
> ```python
> file = open("data.txt", "w")
> contents = file.read()
> print(contents)
> ```
> **Answer:** Opening in `'w'` mode **erases the file**, and you cannot read from a write-mode file — so this both destroys data and fails to read. Fix: open in `'r'` mode to read (`open("data.txt", "r")`).

**Q9. [Theory]** Text files can't easily be edited "in place." How would you implement an **update** or **delete** of a specific line?
> **Answer:** **Read** the whole file into memory (e.g., a list of lines) → **modify** the target line in memory (change it for update, remove it for delete) → **write** the updated content back to the file in `'w'` mode. This read–modify–write pattern rebuilds the file with the changes.

### Synthesis (Cross-Module)

**Q10. [Theory]** *(M4 + M5)* Compare storing data in a **CSV file** versus a **MySQL database**. When would you choose each?
> **Answer:** A CSV is simple, portable, and good for **small, flat datasets** or quick exports/imports, but it has no built-in querying, relationships, or concurrent access. A database handles **large, structured, related data**, supports queries (SELECT/JOIN), enforces integrity (keys, types), and allows multi-user access. Choose CSV for lightweight/portable needs; choose a database for serious, growing, multi-user applications.

**Q11. [Practical]** *(M4 + M5)* Walk through how you would **import data from a CSV into a MySQL table** using Python.
> **Answer:** Open the CSV and create a `csv.reader` (M4) → connect to MySQL and create a cursor (M5) → loop through each CSV row → for each row run a **parameterized** `INSERT` with the row's values → `commit()` after the loop → close the connection. Wrap it in `try/except` so a bad row is reported rather than crashing the import.

---

## MODULE 5 — Database Programming in Python
*(MySQL fundamentals, SQL DDL/DQL/DML, Python–MySQL integration)*

### Easy

**Q1. [Theory]** In a relational database, define **table**, **row**, and **column**.
> **Answer:** A **table** stores data about one entity (e.g., Students). A **row** (record) is one entry in that table (one student). A **column** (field) is one attribute shared by all rows (e.g., name, grade).

**Q2. [Theory]** What do **DDL**, **DML**, and **DQL** stand for? Give one command for each.
> **Answer:** **DDL** (Data Definition Language) — `CREATE`, `ALTER`, `DROP`. **DML** (Data Manipulation Language) — `INSERT`, `UPDATE`, `DELETE`. **DQL** (Data Query Language) — `SELECT`.

**Q3. [Theory]** What is a **primary key**?
> **Answer:** A column (or set of columns) that **uniquely identifies each row** in a table. It cannot contain duplicate or NULL values (e.g., `student_id`).

### Moderate

**Q4. [Practical]** What does this statement do?
> ```sql
> CREATE TABLE students (
>     id INT PRIMARY KEY,
>     name VARCHAR(50),
>     grade INT
> );
> ```
> **Answer:** It creates a table named `students` with three columns: `id` (integer, primary key), `name` (text up to 50 characters), and `grade` (integer).

**Q5. [Practical]** What does this query return?
> ```sql
> SELECT name FROM students WHERE grade >= 90;
> ```
> **Answer:** The **names** of all students whose grade is 90 or higher.

**Q6. [Theory]** What are the steps to connect a Python script to a MySQL database and run a query?
> **Answer:** (1) Import the connector → (2) establish a **connection** (host, user, password, database) → (3) create a **cursor** → (4) `execute()` the SQL → (5) `fetchall()`/`fetchone()` for SELECT, or `commit()` for changes → (6) **close** the cursor and connection.

### Hard

**Q7. [Practical]** Write a SQL statement that increases the grade of the student with `id = 3` to 95.
> **Answer:**
> ```sql
> UPDATE students SET grade = 95 WHERE id = 3;
> ```
> The `WHERE` clause is essential — without it, **every** row would be updated.

**Q8. [Theory]** Why must you call `commit()` after an `INSERT`, `UPDATE`, or `DELETE` in Python, but not after a `SELECT`?
> **Answer:** `INSERT`/`UPDATE`/`DELETE` **change the data**, and `commit()` permanently saves those changes to the database; without it, the changes are lost. `SELECT` only **reads** data and makes no changes, so there is nothing to commit.

**Q9. [Practical]** Why is the first line risky, and what is the safer approach?
> ```python
> cursor.execute("SELECT * FROM users WHERE name = '" + user_input + "'")
> ```
> **Answer:** Building queries by **string concatenation** allows **SQL injection** (malicious input can alter the query). The safer approach is a **parameterized query**, letting the connector handle the value:
> ```python
> cursor.execute("SELECT * FROM users WHERE name = %s", (user_input,))
> ```

### Synthesis (Cross-Module)

**Q10. [Practical]** *(M5 + M3)* Design a function `add_student(name, grade)` that inserts a record into MySQL with validation and error handling.
> **Answer:** The function (1) **validates** inputs (name not empty, grade is a number in range) and returns/raises if invalid; (2) opens the connection and cursor; (3) runs a **parameterized** `INSERT INTO students (name, grade) VALUES (%s, %s)`; (4) `commit()`s; (5) closes the connection — all inside `try/except/finally` so failures are caught and the connection always closes. This combines functions, validation, exception handling (M3) with DB programming (M5).

**Q11. [Practical]** *(M5 + M6)* How would you populate a GUI table/list with rows fetched from a database?
> **Answer:** On load (or on a button click), the callback connects to MySQL, runs `SELECT * FROM students`, and `fetchall()`s the rows (M5). Then it loops through the results and inserts each row into a GUI widget such as a `Listbox` or `Treeview` (M6). Wrap the DB calls in `try/except` to show an error message if the fetch fails.

---

## MODULE 6 — Python GUI Development
*(Tkinter, basic widgets, file handling in GUI, event-driven programming, database integration)*

### Easy

**Q1. [Theory]** What library is commonly used to build GUIs in Python? What is a **widget**? Name three.
> **Answer:** **Tkinter**. A widget is a GUI building block / element the user sees and interacts with. Examples: `Label`, `Button`, `Entry`, `Text`, `Frame` (any three).

**Q2. [Theory]** What does `mainloop()` do in a Tkinter program?
> **Answer:** It starts the **event loop** — the program waits and listens for events (clicks, key presses) and keeps the window open and responsive until it is closed.

**Q3. [Theory]** What is the purpose of a **Label**, an **Entry**, and a **Button**?
> **Answer:** A **Label** displays text or an image (output). An **Entry** is a single-line text box for user input. A **Button** triggers an action when clicked.

### Moderate

**Q4. [Theory]** What are **geometry managers** in Tkinter? Name them.
> **Answer:** They control how widgets are **positioned/arranged** in a window. The three are **`pack()`**, **`grid()`**, and **`place()`**.

**Q5. [Practical]** How do you make a button run a function when clicked?
> **Answer:** Pass the function name to the button's `command` parameter (no parentheses):
> ```python
> btn = Button(window, text="Click", command=say_hello)
> ```
> When clicked, `say_hello()` runs. This function is called a **callback/event handler**.

**Q6. [Theory]** How do you retrieve the text a user typed into an `Entry` widget?
> **Answer:** Use the `.get()` method, e.g., `name = entry.get()`. It returns the current contents as a string.

### Hard

**Q7. [Practical]** Walk through how you would build a simple form that **saves a name to a file** when a button is clicked.
> **Answer:** Create an `Entry` for the name and a `Button`. Write a callback that (1) reads the entry with `.get()`, (2) opens a file in append mode `'a'`, (3) writes the value, (4) closes the file (or uses `with`). Set the button's `command` to that callback so clicking it saves the input.

**Q8. [Theory]** How does **event-driven programming** apply in a GUI? Connect it to callbacks.
> **Answer:** A GUI doesn't run top-to-bottom; it **waits for events** (clicks, typing). Each event is bound to a **callback function** that runs only when that event occurs. The `mainloop()` continuously listens and dispatches events to their handlers.

**Q9. [Practical]** Walk through how you would integrate a **database insert** into a GUI — e.g., a button that saves an Entry's value into MySQL. Mention error handling.
> **Answer:** On button click, the callback (1) reads input with `.get()`, (2) **validates** it isn't empty, (3) opens a MySQL connection and cursor, (4) runs a **parameterized** `INSERT` (`cursor.execute("INSERT INTO ... VALUES (%s)", (value,))`), (5) `commit()`s and closes the connection, (6) shows a confirmation. Wrap the database calls in **`try/except`** to catch connection or input errors gracefully and show a message instead of crashing.

### Synthesis (Cross-Module)

**Q10. [Practical]** *(M2 + M3 + M5 + M6)* Walk through a complete GUI feature: a button that reads input from Entry fields, validates it, saves it to MySQL, and shows a success or error message.
> **Answer:** The callback (1) reads each `Entry` with `.get()` (M6); (2) **validates** the data — not empty, grade is numeric and in range (M2 conditionals); (3) inside `try/except` (M3) connects to MySQL, runs a **parameterized** `INSERT`, and `commit()`s (M5); (4) on success shows a confirmation label/messagebox, on failure shows the error message. This is essentially the capstone flow — it ties input handling, validation, exception handling, and database work into one event-driven feature.

**Q11. [Practical]** *(M4 + M6)* Describe how you'd build a GUI that **loads records from a CSV file into a table** and lets the user save edits back.
> **Answer:** A "Load" button's callback opens the CSV with `csv.reader` (M4) and inserts each row into a `Treeview`/`Listbox` (M6). The user edits entries through the GUI. A "Save" button's callback reads the current data from the widget, opens the CSV in `'w'` mode, and writes all rows back (read–modify–write, M4). Use `try/except` for file errors and confirm the save to the user.

---

## GENERAL THEORY (Course-Wide)
*(Broad principles spanning the whole course and the ITE 103 course outcomes — coding standards, documentation, debugging, secure coding, testing, problem-solving. Use these for any student, regardless of module.)*

**G1. [Theory]** Why is **modular design** (breaking a program into functions) considered good programming practice?
> **Answer:** It improves **readability, reusability, and maintainability** — each function does one clear task, can be tested independently, reused in multiple places, and updated without affecting unrelated code. It also makes debugging easier by isolating where problems occur.

**G2. [Theory]** What is a **coding standard**, and why does it matter?
> **Answer:** A coding standard is a set of agreed conventions for naming, indentation, formatting, and structure. It produces **consistent, readable, maintainable** code that others (and your future self) can understand, and it reduces errors in team projects.

**G3. [Theory]** Why is **documentation / commenting** important in a program?
> **Answer:** Comments and documentation explain *why* the code does something, making it easier to understand, maintain, debug, and hand over to others. Well-documented code is a key part of writing professional, maintainable software.

**G4. [Theory]** Describe the general **debugging process**. What techniques have you learned?
> **Answer:** Reproduce the bug consistently → locate it by tracing variable values → fix it → verify the fix. Techniques include **print statements/tracing**, reading error messages, using **breakpoints** and IDE debuggers, and testing small parts in isolation.

**G5. [Theory]** What does **secure coding** mean, and give one example you practiced in this course.
> **Answer:** Writing code that protects against misuse and errors. Examples from this course: using **parameterized queries** to prevent SQL injection, **validating user input** before processing, and handling files/data without compromising integrity or privacy.

**G6. [Theory]** What is the difference between a **syntax error**, a **runtime error**, and a **logic error**?
> **Answer:** A **syntax error** breaks the language rules so the code won't run (e.g., missing colon). A **runtime error** happens during execution (e.g., divide by zero, file not found). A **logic error** runs without crashing but produces the **wrong result** because the logic is flawed.

**G7. [Theory]** Why is **input validation** important? Give an example.
> **Answer:** Users can enter wrong, empty, or malicious data that crashes the program or corrupts data. Validation checks input before use — e.g., confirming a grade is a number between 0 and 100, or that a required field isn't empty — making the program robust and secure.

**G8. [Theory]** What is the difference between an **iterative** and a **recursive** solution, and when might you prefer each?
> **Answer:** Iteration uses loops; recursion has a function call itself with a base case. Recursion is elegant for naturally self-similar problems (factorials, tree structures) but uses more memory (call stack). Iteration is usually more memory-efficient for simple repetition. Choose based on clarity and the problem's structure.

**G9. [Theory]** Explain the **input–process–output (IPO)** model and how it appeared in the programs you built.
> **Answer:** Most programs **take input** (user entry, file, or database), **process** it (calculations, logic, queries), and **produce output** (display, file, or stored record). Every project — from a calculator to a GUI database form — follows this flow.

**G10. [Theory]** Why is it important to **test your code against the given specifications**?
> **Answer:** Testing confirms the program actually does what was required, not just what you assumed. It catches logic errors and edge cases, and ensures the solution meets the specification — a core part of producing correct, reliable software.

**G11. [Theory]** How does **exception handling** contribute to making software "robust"?
> **Answer:** It lets the program anticipate and recover from errors (bad input, missing files, failed connections) instead of crashing. The user gets a clear message and the program continues or exits cleanly — the hallmark of robust, professional software.

**G12. [Theory]** Across this course you handled data with **files, databases, and GUIs**. In your own words, how do these three work together in a real application?
> **Answer:** The **GUI** is the front end the user interacts with (input/output). Behind it, **files or a database** store the data persistently. **Functions and exception handling** connect them — reading user input from the GUI, validating it, and saving it to a file or database, then retrieving and displaying it back. Together they form a complete, layered application.

**G13. [Theory]** What is an **algorithm**, and why is it important to plan the logic before writing code?
> **Answer:** An algorithm is a clear, step-by-step procedure for solving a problem. Planning the logic first (through pseudocode or a flowchart) helps you think through the solution, catch flaws early, and write cleaner code — coding without a plan often leads to confusion and bugs.

**G14. [Theory]** What is an **IDE**, and what development tools did you use in this course?
> **Answer:** An IDE (Integrated Development Environment) is software that combines an editor, compiler/interpreter, and debugging tools in one place. In this course we used **Visual Studio Code** for writing and running Python, along with **MySQL** for databases and **Git/GitHub** for version control.

**G15. [Theory]** Why is **version control** (e.g., Git/GitHub) useful when developing software?
> **Answer:** Version control tracks changes to code over time, lets you revert to earlier working versions, and allows multiple people to collaborate on the same project without overwriting each other's work. It's a standard professional practice for managing and backing up code.

**G16. [Theory]** What is **variable scope**? Explain the difference between a **local** and a **global** variable.
> **Answer:** Scope is the region of a program where a variable can be accessed. A **local** variable is defined inside a function and exists only there; a **global** variable is defined outside functions and is accessible throughout the program. Local variables help keep functions self-contained and avoid unintended changes.

**G17. [Theory]** What is the difference between **mutable** and **immutable** data types? Give an example of each.
> **Answer:** A **mutable** object can be changed after it is created (e.g., a **list** or **dictionary**). An **immutable** object cannot be changed once created (e.g., a **string**, **int**, or **tuple**) — operations on it produce a new object instead.

**G18. [Theory]** Why do **meaningful variable and function names** matter in a program?
> **Answer:** Clear names like `total_price` or `compute_average()` make code self-explanatory and easier to read, debug, and maintain. Vague names like `x` or `temp` force readers to guess the purpose, increasing the chance of errors. Good naming is part of writing clean, professional code.

**G19. [Practical]** What is the difference between a function that **returns** a value and one that only **prints** a value?
> **Answer:** A function that **returns** a value sends it back to the caller, so it can be stored, reused, or passed into other code (e.g., `result = add(2, 3)`). A function that only **prints** just displays the value on screen — the value can't be reused afterward. Returning is more flexible; printing is only for output.

**G20. [Theory]** Why is **data privacy and integrity** important when handling files and databases?
> **Answer:** Programs often store personal or important data, so we must protect it from loss, unauthorized access, or accidental corruption. Practices like careful file handling, input validation, accurate record-keeping, and not exposing sensitive data uphold both ethical responsibility and user trust — a value emphasized throughout the course.

**G21. [Theory]** What does it mean for code to be **maintainable**, and why does it matter?
> **Answer:** Maintainable code is easy to read, understand, fix, and extend later — through modular functions, clear names, consistent style, and good documentation. It matters because software is updated and reused over time; poorly written code becomes costly and error-prone to change.

**G22. [Theory]** What is the role of **pseudocode or a flowchart** before writing actual code?
> **Answer:** Pseudocode and flowcharts let you plan a program's logic in plain, language-independent terms before committing to syntax. They make it easier to organize steps, spot logical gaps, and communicate the design to others — so the actual coding becomes a translation of an already-sound plan.

---

*End of recitation question bank — 77 items (55 module questions across Modules 2–6 + 22 general theory).*
