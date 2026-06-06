# Exception Handling and Messagebox

### A Beginner-Friendly Mini-Lecture for Programming 2

*Make your app friendly when things go wrong.*

---

## What You Will Learn

In this short lecture, you will learn how to:

- Show pop-up messages to the user (info, warnings, errors, confirmations)
- Catch errors before they crash your app
- Combine the two to make your app feel professional

This is a follow-up to the main **Tkinter + SQLite** lecture. We'll keep using the Notes app as our example.

---

## Part 1: Why This Matters

Imagine your friend tries your Notes app. They:

1. Click "Save Note" with an empty box → **the app crashes**
2. Click "Delete" with nothing selected → **the app crashes**
3. Lose the database file by accident → **the app crashes**

In every case, your friend sees a scary Python error in the terminal. Not great.

A good app should:

- **Stop the user gently** when they do something invalid
- **Recover quietly** when something unexpected happens
- **Explain clearly** what went wrong

Tkinter and Python give us two tools for this: **messagebox** and **try/except**.

---

## Part 2: The Messagebox

The `messagebox` lets you show pop-up windows. Let's start with the simplest example.

Create a new file called `messagebox_practice.py`:

```python
import tkinter as tk
from tkinter import messagebox

window = tk.Tk()
window.title("Messagebox Practice")
window.geometry("300x200")

def show_message():
    messagebox.showinfo("Hello", "This is an info pop-up!")

button = tk.Button(window, text="Show Message", command=show_message)
button.pack(pady=50)

window.mainloop()
```

Run it. Click the button. A clean pop-up appears with an OK button.

### What's happening?

- `from tkinter import messagebox` — Brings in the messagebox tools.
- `messagebox.showinfo("Title", "Message text")` — Shows a pop-up.

That's it. Two arguments: the **title** at the top, and the **message** in the body.

---

## Part 3: The Four Types of Pop-Ups

There are four main pop-ups you will use, each for a different situation.

### 3.1 `showinfo` — General Information

Use this for friendly notifications.

```python
messagebox.showinfo("Saved", "Your note was saved successfully.")
```

Shows a blue/white info icon.

### 3.2 `showwarning` — Warnings

Use this when the user did something questionable but not catastrophic.

```python
messagebox.showwarning("Empty Note", "Please type something before saving.")
```

Shows a yellow/triangle warning icon.

### 3.3 `showerror` — Errors

Use this when something *went wrong* — usually inside a `try/except` block (more on this later).

```python
messagebox.showerror("Database Error", "Could not connect to the database.")
```

Shows a red error icon.

### 3.4 `askyesno` — Yes/No Questions

This one is special — it **returns a value**. `True` if the user clicked Yes, `False` if they clicked No.

```python
answer = messagebox.askyesno("Confirm", "Delete this note?")
if answer:
    print("User chose Yes")
else:
    print("User chose No")
```

You **must save the result** in a variable. Otherwise, the user's choice is lost.

---

## Part 4: Trying Each Pop-Up

Let's build a small test app to see all four in action.

```python
import tkinter as tk
from tkinter import messagebox

def show_info():
    messagebox.showinfo("Information", "Everything is fine!")

def show_warning():
    messagebox.showwarning("Warning", "Be careful with that.")

def show_error():
    messagebox.showerror("Error", "Something went wrong!")

def ask_question():
    answer = messagebox.askyesno("Question", "Do you like programming?")
    if answer:
        messagebox.showinfo("Cool", "Glad to hear it!")
    else:
        messagebox.showinfo("Okay", "Maybe it'll grow on you.")

window = tk.Tk()
window.title("Messagebox Demo")
window.geometry("300x250")

tk.Button(window, text="Info",    command=show_info,    width=20).pack(pady=5)
tk.Button(window, text="Warning", command=show_warning, width=20).pack(pady=5)
tk.Button(window, text="Error",   command=show_error,   width=20).pack(pady=5)
tk.Button(window, text="Yes/No",  command=ask_question, width=20).pack(pady=5)

window.mainloop()
```

Run it. Click each button. Notice the different icons and how `askyesno` waits for your answer before doing anything.

---

## Part 5: A Quick Rule for Choosing the Right Pop-Up

| Situation                                        | Use            |
|--------------------------------------------------|----------------|
| Telling the user something good happened         | `showinfo`     |
| Telling the user they need to fix their input    | `showwarning`  |
| Telling the user something failed                | `showerror`    |
| Asking the user to confirm before doing something| `askyesno`     |

---

## Part 6: What Are Exceptions?

Now let's talk about errors.

When something goes wrong in Python, the program **crashes** by default. Try this:

```python
number = int("hello")    # Can't turn "hello" into a number
```

Run it. You'll see:

```
ValueError: invalid literal for int() with base 10: 'hello'
```

That `ValueError` is called an **exception**. It's Python's way of saying "I gave up — fix this."

In a regular script, a crash is annoying but okay. In a GUI app, **a crash is unacceptable** — the user just sees a frozen window. We need to *catch* exceptions and handle them gracefully.

---

## Part 7: The try/except Pattern

The basic shape is:

```python
try:
    # Risky code here
except:
    # What to do if it failed
```

Python *tries* the risky code. If it fails, instead of crashing, it runs the `except` block.

### A Simple Example

```python
try:
    number = int("hello")
    print("Number is:", number)
except:
    print("That wasn't a number.")
```

Run it. Instead of crashing, you see: `That wasn't a number.`

The program kept going.

---

## Part 8: Catching Specific Errors

The bare `except:` catches *every* error, even ones we didn't mean to. That's lazy and dangerous. Better practice is to catch the *specific* kind of error you expect.

```python
try:
    number = int("hello")
except ValueError:
    print("That wasn't a valid number.")
```

This catches only `ValueError`. If a different kind of error happens, it still crashes — which is what you want, so you can find and fix the real problem.

### Common Errors You'll See

| Error name        | When it happens                                    |
|-------------------|----------------------------------------------------|
| `ValueError`      | Bad value (like `int("hello")`)                    |
| `TypeError`       | Wrong type (like `"hello" + 5`)                    |
| `ZeroDivisionError` | Dividing by zero                                 |
| `FileNotFoundError` | Trying to open a file that doesn't exist        |
| `sqlite3.Error`   | Any database problem                               |

---

## Part 9: Getting the Error Message

Sometimes you want to see *what* the error said. Use `as`:

```python
try:
    number = int("hello")
except ValueError as e:
    print("Error:", e)
```

The `as e` part captures the error in a variable called `e`. Print it to see the message:

```
Error: invalid literal for int() with base 10: 'hello'
```

This is incredibly useful when combined with messagebox — you can show the user *exactly* what went wrong.

---

## Part 10: Combining try/except with Messagebox

Now we put it together. This is the real-world pattern you'll use over and over.

```python
import tkinter as tk
from tkinter import messagebox

def calculate_age():
    try:
        year = int(year_entry.get())
        age = 2026 - year
        messagebox.showinfo("Your Age", f"You are {age} years old.")
    except ValueError:
        messagebox.showerror("Invalid Input", "Please enter a valid year (numbers only).")

window = tk.Tk()
window.title("Age Calculator")
window.geometry("300x200")

tk.Label(window, text="Enter your birth year:").pack(pady=10)

year_entry = tk.Entry(window, width=20)
year_entry.pack(pady=5)

tk.Button(window, text="Calculate", command=calculate_age).pack(pady=10)

window.mainloop()
```

Run it. Try:

- Type `2005` → tells you your age
- Type `hello` → polite error pop-up, no crash
- Leave it empty → polite error pop-up, no crash

**This is the magic.** Your app stays alive no matter what the user does.

---

## Part 11: Database Errors

Database operations can fail too. Maybe the database file got deleted, or the user typed something weird. Wrap database code in try/except like this:

```python
import sqlite3
from tkinter import messagebox

try:
    conn = sqlite3.connect("notes.db")
    cur = conn.cursor()
    cur.execute("INSERT INTO notes (content) VALUES (?)", ("My note",))
    conn.commit()
    conn.close()
    messagebox.showinfo("Success", "Note saved!")
except sqlite3.Error as e:
    messagebox.showerror("Database Error", f"Could not save note:\n{e}")
```

`sqlite3.Error` catches any error from the database. The user gets a clear message instead of a crash.

---

## Part 12: Putting It All Together — A Safer Notes App

Let's improve the Notes app from the main lecture with proper exception handling.

```python
import tkinter as tk
from tkinter import messagebox
import sqlite3

# ----- Set up the database -----
try:
    conn = sqlite3.connect("notes.db")
    cur = conn.cursor()
    cur.execute("""
        CREATE TABLE IF NOT EXISTS notes (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            content TEXT NOT NULL
        )
    """)
    conn.commit()
except sqlite3.Error as e:
    messagebox.showerror("Startup Error", f"Could not open database:\n{e}")
    exit()

note_ids = []

# ----- Functions -----
def refresh_list():
    try:
        listbox.delete(0, tk.END)
        note_ids.clear()
        cur.execute("SELECT id, content FROM notes")
        for note_id, content in cur.fetchall():
            listbox.insert(tk.END, content)
            note_ids.append(note_id)
    except sqlite3.Error as e:
        messagebox.showerror("Database Error", f"Could not load notes:\n{e}")

def save_note():
    note = entry.get().strip()
    if note == "":
        messagebox.showwarning("Empty Note", "Please type something before saving.")
        return
    try:
        cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
        conn.commit()
        entry.delete(0, tk.END)
        refresh_list()
        messagebox.showinfo("Saved", "Your note has been saved!")
    except sqlite3.Error as e:
        messagebox.showerror("Save Failed", f"Could not save note:\n{e}")

def delete_note():
    selection = listbox.curselection()
    if not selection:
        messagebox.showinfo("No Selection", "Please click a note to delete first.")
        return
    if not messagebox.askyesno("Confirm Delete", "Are you sure you want to delete this note?"):
        return
    try:
        index = selection[0]
        note_id = note_ids[index]
        cur.execute("DELETE FROM notes WHERE id = ?", (note_id,))
        conn.commit()
        refresh_list()
    except sqlite3.Error as e:
        messagebox.showerror("Delete Failed", f"Could not delete note:\n{e}")

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App (Safer Version)")
window.geometry("400x450")

tk.Label(window, text="Type your note:").pack(pady=5)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

tk.Button(window, text="Save Note", command=save_note).pack(pady=5)

listbox = tk.Listbox(window, width=50, height=10)
listbox.pack(pady=10)

tk.Button(window, text="Delete Selected", command=delete_note).pack(pady=5)

refresh_list()

window.mainloop()
```

Every database operation is wrapped in try/except. Every error becomes a friendly pop-up.

---

## Quick Reference

### Messagebox

```python
from tkinter import messagebox

messagebox.showinfo("Title", "Message")        # Info pop-up
messagebox.showwarning("Title", "Message")     # Warning pop-up
messagebox.showerror("Title", "Message")       # Error pop-up

answer = messagebox.askyesno("Title", "Question?")
# Returns True if Yes, False if No
```

### try/except

```python
try:
    # Risky code
except ValueError as e:
    # Handle that specific error
except sqlite3.Error as e:
    # Handle database errors
```

### The Golden Pattern

```python
try:
    # Do something that might fail
except SomeError as e:
    messagebox.showerror("Error Title", f"Something went wrong:\n{e}")
```

---

## Practice Activities

1. **Calculator with Safety** — Build a calculator where the user types two numbers. Use try/except to catch invalid input. Show errors as pop-ups.

2. **BMI Checker** — Ask for height and weight. Show a warning if either is empty. Show an error if either isn't a number. Show the BMI as an info pop-up.

3. **Improved Phonebook** — From the main lecture's practice list. Add validation: empty fields trigger warnings, database errors become pop-ups.

For each one, follow the pattern:

1. Wrap risky code in `try`
2. Catch the specific error in `except`
3. Show a friendly `messagebox` instead of crashing

---

## What's Next?

Now that your app handles errors gracefully, you can:

- Display data in nicer tables → see the **Treeview** mini-lecture
- Organize multiple sections of your app into tabs → see the **Tab View** mini-lecture

You've leveled up. Your apps now feel like real software.

---

*End of Mini-Lecture*
