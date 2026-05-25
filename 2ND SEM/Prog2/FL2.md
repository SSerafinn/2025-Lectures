# Tkinter + SQLite

### A Beginner-Friendly Lecture for Programming 2

*Build your first desktop app that remembers your data.*

---

## What You Will Learn

By the end of this lecture, you will have built a complete **Notes App** — a desktop program where users can write notes, save them, and see them again the next time they open the app. Along the way, you will learn:

- How to create windows and buttons with Tkinter
- How to save and load data with SQLite
- How to put the two together into a real, working application

We will build the same app step by step. Each chapter adds one new feature.

---

## Before We Start

You need:

- **Python 3** installed on your computer
- A code editor like **VS Code**, **IDLE**, or **Thonny**

That's it. Both **Tkinter** and **SQLite** come built into Python. No `pip install` needed.

To check that Python is installed, open a terminal and type:

```
python --version
```

You should see something like `Python 3.11.0` or higher.

---

# Chapter 1: Your First Window

Let's start by making a window appear on the screen. Open your editor, create a new file called `notes.py`, and type this:

```python
import tkinter as tk

window = tk.Tk()
window.title("My Notes App")
window.geometry("400x300")

window.mainloop()
```

Save and run it. A small empty window should appear with "My Notes App" in the title bar.

### What did each line do?

- `import tkinter as tk` — Brings in Tkinter so we can use it. The `as tk` part is just a nickname so we can type `tk` instead of `tkinter`.
- `window = tk.Tk()` — Creates the window. Think of it as an empty box waiting for things to go inside.
- `window.title("My Notes App")` — Sets the text at the top of the window.
- `window.geometry("400x300")` — Makes the window 400 pixels wide and 300 tall. The `x` is just the letter x.
- `window.mainloop()` — Keeps the window open. Without this line, the window would flash and disappear.

### Try It Yourself

- Change the title to your own name.
- Change the size to `"500x400"` and see what happens.

---

# Chapter 2: Adding Text and a Button

An empty window is boring. Let's add a label (text) and a button.

Update your `notes.py`:

```python
import tkinter as tk

window = tk.Tk()
window.title("My Notes App")
window.geometry("400x300")

label = tk.Label(window, text="Welcome to My Notes App!")
label.pack()

button = tk.Button(window, text="Click Me")
button.pack()

window.mainloop()
```

Run it. You should see the text and a button.

### What's new?

- `tk.Label(window, text="...")` — Creates a label. The first argument, `window`, tells Tkinter *where* to put it.
- `tk.Button(window, text="...")` — Creates a button.
- `.pack()` — Places the widget inside the window. Without this, the widget exists but does not appear.

> **Rule of thumb:** Every widget needs to be `.pack()`-ed (or placed some other way), or it won't show up.

### Making the Button Do Something

Right now, clicking the button does nothing. Let's fix that.

```python
import tkinter as tk

def say_hello():
    print("Hello! You clicked the button.")

window = tk.Tk()
window.title("My Notes App")
window.geometry("400x300")

label = tk.Label(window, text="Welcome to My Notes App!")
label.pack()

button = tk.Button(window, text="Click Me", command=say_hello)
button.pack()

window.mainloop()
```

Run it and click the button. Look at your terminal — "Hello!" should appear each time you click.

### What's new?

- We defined a function `say_hello()`.
- We added `command=say_hello` to the button. This tells Tkinter: *"When clicked, call this function."*

> **Important:** Write `command=say_hello`, NOT `command=say_hello()`. The parentheses would call the function immediately when the program starts, instead of waiting for the click.

---

# Chapter 3: Getting Input from the User

We need a place for the user to type their note. That's what the `Entry` widget is for.

```python
import tkinter as tk

def show_note():
    note = entry.get()
    print("You wrote:", note)

window = tk.Tk()
window.title("My Notes App")
window.geometry("400x300")

label = tk.Label(window, text="Type your note:")
label.pack(pady=10)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

button = tk.Button(window, text="Show Note", command=show_note)
button.pack(pady=10)

window.mainloop()
```

Run it. Type something in the box, click the button, and check your terminal.

### What's new?

- `tk.Entry(window, width=40)` — A text input box, 40 characters wide.
- `entry.get()` — Reads whatever the user typed.
- `pady=10` — Adds 10 pixels of vertical space around the widget. It looks nicer.

> **Tip:** Use `padx` for horizontal spacing and `pady` for vertical spacing. Don't be shy with spacing — cramped interfaces look ugly.

---

# Chapter 4: Showing Output on the Screen

Printing to the terminal is fine for testing, but a real app shows results on the screen. Let's display the note in a label instead.

```python
import tkinter as tk

def show_note():
    note = entry.get()
    result_label.config(text="You wrote: " + note)

window = tk.Tk()
window.title("My Notes App")
window.geometry("400x300")

label = tk.Label(window, text="Type your note:")
label.pack(pady=10)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

button = tk.Button(window, text="Show Note", command=show_note)
button.pack(pady=10)

result_label = tk.Label(window, text="")
result_label.pack(pady=10)

window.mainloop()
```

Run it, type a note, click the button. The note now appears below the button.

### What's new?

- We added a second label called `result_label`. It starts empty (`text=""`).
- `result_label.config(text="...")` — Changes the label's text after it has been created.

> **Key idea:** `.config(...)` is how you change a widget's properties *after* it exists. Almost any property you set when creating a widget can be changed later with `.config()`.

---

# Chapter 5: A Problem to Solve

Right now, our app has a big problem: **when you close it, your note is gone forever.**

That's because everything is stored in memory (RAM). When the program ends, the memory is wiped. To keep data around, we need to save it somewhere — like a file or a database.

This is where **SQLite** comes in.

### What is SQLite?

SQLite is a database that lives in a single file on your computer. You can save data into it, and the data stays there even after your program closes. The next time you open your app, you can read the data back.

### Why Not Just Use a Text File?

You *could* save notes to a `.txt` file. But databases are much better at:

- Searching for specific data
- Updating one item without rewriting everything
- Keeping data organized (like a spreadsheet, but smarter)

For now, just trust that databases are the right tool. You'll see why soon.

---

# Chapter 6: Your First Database

Let's pause Tkinter and just play with SQLite in a separate file. Create a new file called `db_practice.py`:

```python
import sqlite3

# Open (or create) a database file
conn = sqlite3.connect("notes.db")

# Create a "cursor" to send commands to the database
cur = conn.cursor()

# Create a table to store notes
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL
    )
""")

# Save the changes
conn.commit()

# Close the connection
conn.close()

print("Database ready!")
```

Run it. You should see "Database ready!" in the terminal. Look at your folder — a new file called `notes.db` has appeared. That's your database.

### What did each part do?

- `sqlite3.connect("notes.db")` — Opens the database. If `notes.db` doesn't exist, SQLite creates it.
- `conn.cursor()` — A cursor is like your "hand" inside the database. You use it to give commands.
- `cur.execute(...)` — Runs a database command. The command we ran is **SQL** (a language for talking to databases).
- `conn.commit()` — Saves your changes. **Without this, your changes are lost.**
- `conn.close()` — Closes the connection.

### Reading the SQL

```sql
CREATE TABLE IF NOT EXISTS notes (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    content TEXT NOT NULL
)
```

In plain English: *"Create a table called `notes` if it doesn't exist yet. It has two columns: `id` (a number that goes up automatically) and `content` (text that cannot be empty)."*

Think of a table like a spreadsheet:

```
notes table
┌────┬────────────────────────┐
│ id │ content                │
├────┼────────────────────────┤
│  1 │ Buy rice               │
│  2 │ Submit ITE 124 lab     │
│  3 │ Meeting at 3pm         │
└────┴────────────────────────┘
```

Each row is one note. The `id` is just a unique number SQLite gives each row automatically.

---

# Chapter 7: Adding Data

A table with no data is useless. Let's add some notes. Update `db_practice.py`:

```python
import sqlite3

conn = sqlite3.connect("notes.db")
cur = conn.cursor()

# Insert some notes
cur.execute("INSERT INTO notes (content) VALUES (?)", ("Buy rice",))
cur.execute("INSERT INTO notes (content) VALUES (?)", ("Submit ITE 124 lab",))
cur.execute("INSERT INTO notes (content) VALUES (?)", ("Meeting at 3pm",))

conn.commit()
conn.close()

print("Notes added!")
```

Run it.

### Reading the SQL

```sql
INSERT INTO notes (content) VALUES (?)
```

This says: *"Add a new row to the `notes` table. Set the `content` column to whatever is in the `?`."*

Notice we did NOT give an `id`. SQLite assigns one automatically.

### What's the `?` for?

The `?` is a **placeholder**. The actual value comes from the tuple after the SQL string:

```python
cur.execute("INSERT INTO notes (content) VALUES (?)", ("Buy rice",))
```

The `("Buy rice",)` is a tuple with one item. The comma is **required** — without it, Python thinks `("Buy rice")` is just a string in parentheses, not a tuple.

> **Why use `?` placeholders instead of just putting the text in the string?** Because it keeps your app safe from a hacking technique called **SQL injection**. We will explain this later. For now: **always use `?` for values.**

### If You Run It Again...

You'll notice that running the script twice adds the notes twice. The database keeps growing. That's normal — each `INSERT` adds a new row.

---

# Chapter 8: Reading Data

Adding data is only useful if we can read it back. Update `db_practice.py`:

```python
import sqlite3

conn = sqlite3.connect("notes.db")
cur = conn.cursor()

# Read all notes
cur.execute("SELECT * FROM notes")
rows = cur.fetchall()

for row in rows:
    print(row)

conn.close()
```

Run it. You'll see something like:

```
(1, 'Buy rice')
(2, 'Submit ITE 124 lab')
(3, 'Meeting at 3pm')
```

Each row is a tuple. The first item is the `id`, the second is the `content`.

### Reading the SQL

```sql
SELECT * FROM notes
```

In plain English: *"Get everything from the `notes` table."* The `*` means "all columns."

### Unpacking the Rows

You can grab specific values from each row:

```python
for row in rows:
    note_id = row[0]
    content = row[1]
    print(f"Note #{note_id}: {content}")
```

Or, even cleaner, unpack directly:

```python
for note_id, content in rows:
    print(f"Note #{note_id}: {content}")
```

Both produce the same result. The second style is what most Python programmers use.

---

# Chapter 9: Deleting Data

What if you finish a task and want to remove the note? Use `DELETE`:

```python
import sqlite3

conn = sqlite3.connect("notes.db")
cur = conn.cursor()

# Delete the note with id = 1
cur.execute("DELETE FROM notes WHERE id = ?", (1,))

conn.commit()
conn.close()

print("Note deleted!")
```

### Reading the SQL

```sql
DELETE FROM notes WHERE id = ?
```

In plain English: *"Remove rows from `notes` where the `id` matches the `?`."*

> **WARNING:** If you forget the `WHERE` part, like just `DELETE FROM notes`, it will delete **every row** in the table. Always include `WHERE` when deleting.

---

# Chapter 10: Putting Tkinter and SQLite Together

Now for the exciting part. Let's combine everything.

Go back to `notes.py` (the Tkinter file) and rewrite it:

```python
import tkinter as tk
import sqlite3

# ----- Set up the database -----
conn = sqlite3.connect("notes.db")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL
    )
""")
conn.commit()

# ----- Functions -----
def save_note():
    note = entry.get()
    if note == "":
        return
    cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
    conn.commit()
    entry.delete(0, tk.END)
    status_label.config(text="Note saved!")

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App")
window.geometry("400x300")

label = tk.Label(window, text="Type your note:")
label.pack(pady=10)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

button = tk.Button(window, text="Save Note", command=save_note)
button.pack(pady=10)

status_label = tk.Label(window, text="")
status_label.pack(pady=10)

window.mainloop()
```

Run it. Type a note, click "Save Note". You should see "Note saved!" appear.

### What's happening?

1. When the program starts, we open the database and make sure the `notes` table exists.
2. The `save_note()` function reads what's in the Entry, saves it to the database, and clears the Entry.
3. `entry.delete(0, tk.END)` — Erases everything from position 0 to the end of the Entry.

### Proving It Works

Close the app. Run it again. Save a few more notes. Now switch back to `db_practice.py` (the one that reads notes) and run *it*. You should see all the notes you saved — they survived between runs!

This is the magic of using a database. Your app finally has memory.

---

# Chapter 11: Showing Saved Notes in the App

Right now, we can only see saved notes by running a separate script. Let's display them inside the app instead.

We'll use a **Listbox** — a widget that shows a scrollable list.

```python
import tkinter as tk
import sqlite3

# ----- Set up the database -----
conn = sqlite3.connect("notes.db")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL
    )
""")
conn.commit()

# ----- Functions -----
def refresh_list():
    listbox.delete(0, tk.END)
    cur.execute("SELECT content FROM notes")
    for row in cur.fetchall():
        listbox.insert(tk.END, row[0])

def save_note():
    note = entry.get()
    if note == "":
        return
    cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
    conn.commit()
    entry.delete(0, tk.END)
    refresh_list()

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App")
window.geometry("400x400")

label = tk.Label(window, text="Type your note:")
label.pack(pady=5)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

button = tk.Button(window, text="Save Note", command=save_note)
button.pack(pady=5)

listbox = tk.Listbox(window, width=50, height=10)
listbox.pack(pady=10)

# Load existing notes when the app starts
refresh_list()

window.mainloop()
```

Run it. The listbox shows all your previously saved notes. Add a new one — it appears in the list immediately.

### What's new?

- **`Listbox`** — A widget that shows a list of items, one per row.
- **`listbox.delete(0, tk.END)`** — Clears the listbox (from row 0 to the end).
- **`listbox.insert(tk.END, row[0])`** — Adds one item to the end of the listbox.
- **`refresh_list()`** — A helper function that reloads notes from the database into the listbox. We call it when the app starts and after saving a new note.

> **Pattern to remember:** Whenever your data changes, refresh the display. Don't try to manually keep them in sync — just reload from the database.

---

# Chapter 12: Deleting a Note from the App

Let's let the user click a note and delete it.

We have a small problem: the listbox only shows the note *content*, not the *id*. To delete a note, we need its id. The fix is to keep a separate list of ids that matches the listbox order.

```python
import tkinter as tk
import sqlite3

# ----- Set up the database -----
conn = sqlite3.connect("notes.db")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL
    )
""")
conn.commit()

# A list that mirrors the listbox, storing each row's id
note_ids = []

# ----- Functions -----
def refresh_list():
    listbox.delete(0, tk.END)
    note_ids.clear()
    cur.execute("SELECT id, content FROM notes")
    for note_id, content in cur.fetchall():
        listbox.insert(tk.END, content)
        note_ids.append(note_id)

def save_note():
    note = entry.get()
    if note == "":
        return
    cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
    conn.commit()
    entry.delete(0, tk.END)
    refresh_list()

def delete_note():
    selection = listbox.curselection()
    if not selection:
        return
    index = selection[0]
    note_id = note_ids[index]
    cur.execute("DELETE FROM notes WHERE id = ?", (note_id,))
    conn.commit()
    refresh_list()

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App")
window.geometry("400x450")

label = tk.Label(window, text="Type your note:")
label.pack(pady=5)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

save_button = tk.Button(window, text="Save Note", command=save_note)
save_button.pack(pady=5)

listbox = tk.Listbox(window, width=50, height=10)
listbox.pack(pady=10)

delete_button = tk.Button(window, text="Delete Selected", command=delete_note)
delete_button.pack(pady=5)

refresh_list()

window.mainloop()
```

Run it. Click a note in the listbox, then click "Delete Selected". The note disappears — and stays gone the next time you open the app.

### What's new?

- **`note_ids = []`** — A list of database ids, in the same order as the listbox.
- **`listbox.curselection()`** — Returns the index of the currently selected item, like `(0,)` for the first item. If nothing is selected, it returns an empty tuple.
- **`note_ids[index]`** — Looks up the database id of the selected note.

### Walking Through the Logic

1. User clicks "Buy rice" (let's say it's at index 0).
2. `listbox.curselection()` returns `(0,)`.
3. We pick out `0`, then look up `note_ids[0]` to find the actual id, like `5`.
4. We tell the database: `DELETE FROM notes WHERE id = 5`.
5. We refresh the list, and the note is gone.

This is the standard pattern for working with listboxes and databases: **keep a parallel list of ids.**

---

# Chapter 13: One Last Polish — Confirmation and Error Messages

Right now, if the user clicks "Delete Selected" with nothing selected, nothing happens. Better to show a friendly message. Let's also confirm before deleting.

```python
import tkinter as tk
from tkinter import messagebox
import sqlite3

# ----- Set up the database -----
conn = sqlite3.connect("notes.db")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL
    )
""")
conn.commit()

note_ids = []

# ----- Functions -----
def refresh_list():
    listbox.delete(0, tk.END)
    note_ids.clear()
    cur.execute("SELECT id, content FROM notes")
    for note_id, content in cur.fetchall():
        listbox.insert(tk.END, content)
        note_ids.append(note_id)

def save_note():
    note = entry.get().strip()
    if note == "":
        messagebox.showwarning("Empty Note", "Please type something before saving.")
        return
    cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
    conn.commit()
    entry.delete(0, tk.END)
    refresh_list()

def delete_note():
    selection = listbox.curselection()
    if not selection:
        messagebox.showinfo("No Selection", "Please click a note to delete first.")
        return
    if not messagebox.askyesno("Confirm Delete", "Are you sure you want to delete this note?"):
        return
    index = selection[0]
    note_id = note_ids[index]
    cur.execute("DELETE FROM notes WHERE id = ?", (note_id,))
    conn.commit()
    refresh_list()

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App")
window.geometry("400x450")

label = tk.Label(window, text="Type your note:")
label.pack(pady=5)

entry = tk.Entry(window, width=40)
entry.pack(pady=5)

save_button = tk.Button(window, text="Save Note", command=save_note)
save_button.pack(pady=5)

listbox = tk.Listbox(window, width=50, height=10)
listbox.pack(pady=10)

delete_button = tk.Button(window, text="Delete Selected", command=delete_note)
delete_button.pack(pady=5)

refresh_list()

window.mainloop()
```

Run it. Try:

- Clicking "Save Note" with an empty box → warning popup
- Clicking "Delete Selected" with nothing selected → info popup
- Deleting a note → confirmation popup, must click "Yes" to actually delete

### What's new?

- **`from tkinter import messagebox`** — Tkinter's built-in popup library.
- **`messagebox.showinfo`** — Shows an info popup with an OK button.
- **`messagebox.showwarning`** — Shows a warning popup.
- **`messagebox.askyesno`** — Shows a Yes/No popup. Returns `True` if the user clicks Yes.

### Tiny Extra Touch

Notice we changed `entry.get()` to `entry.get().strip()`. This removes spaces from both ends, so a user who accidentally types just spaces won't save a useless empty-looking note.

---

# Chapter 14: Congratulations!

You just built a complete desktop application. It has:

- A graphical interface
- The ability to save data
- The ability to read data back
- The ability to delete data
- User-friendly popups

This is the foundation of nearly every "data app" in the world — from contact lists to inventory systems to grade trackers. The pattern is always the same:

1. Set up a database table.
2. Build widgets for the user to input and view data.
3. Connect button clicks to functions that read/write the database.
4. Refresh the display whenever data changes.

---

## Quick Reference

### Tkinter Cheat Sheet

```python
import tkinter as tk

window = tk.Tk()                        # Create the window
window.title("App Name")                # Set title
window.geometry("400x300")              # Set size

label = tk.Label(window, text="Hi")     # Text on screen
entry = tk.Entry(window, width=40)      # Text input box
button = tk.Button(window, text="Go",   # Clickable button
                   command=some_function)
listbox = tk.Listbox(window)            # Scrollable list

label.pack()                            # Show the widget

entry.get()                             # Read text from entry
entry.delete(0, tk.END)                 # Clear entry
label.config(text="New Text")           # Change label text

window.mainloop()                       # Keep window open
```

### SQLite Cheat Sheet

```python
import sqlite3

conn = sqlite3.connect("mydata.db")     # Open/create database
cur = conn.cursor()                     # Get cursor

# Create a table
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL
    )
""")

# Add data
cur.execute("INSERT INTO notes (content) VALUES (?)", ("Hello",))

# Read data
cur.execute("SELECT * FROM notes")
rows = cur.fetchall()

# Delete data
cur.execute("DELETE FROM notes WHERE id = ?", (1,))

conn.commit()                           # Save changes!
conn.close()                            # Close database
```

*End of Lecture*
