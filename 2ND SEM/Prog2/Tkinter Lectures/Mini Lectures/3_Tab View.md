# The Tab View (Notebook Widget)

### A Beginner-Friendly Mini-Lecture for Programming 2

*Organize your app into clean, separate tabs.*

---

## What You Will Learn

In this short lecture, you will learn how to:

- Create tabs at the top of your window
- Put different widgets inside each tab
- Switch between tabs cleanly

This is a follow-up to the main **Tkinter + SQLite** lecture. We'll reorganize the Notes app so different actions live in different tabs.

---

## Part 1: Why Use Tabs?

Look at any modern app — VS Code, Chrome, MS Word, even your phone's settings. They all use **tabs** to organize different sections of the same window.

Why? Because cramming everything into one window gets messy fast. Imagine if our Notes app needed to:

- Add new notes
- View all notes
- Show settings

That's three different jobs. Putting them all in one window means a wall of buttons and labels. Putting each on its own tab keeps things clean.

---

## Part 2: Meet the Notebook Widget

In Tkinter, the tab widget is called **Notebook**. It comes from the `ttk` add-on (just like Treeview).

We import it like this:

```python
from tkinter import ttk
```

Then we create tabs with `ttk.Notebook(...)`.

---

## Part 3: A Minimal Notebook

Create a new file called `notebook_practice.py`:

```python
import tkinter as tk
from tkinter import ttk

window = tk.Tk()
window.title("Tab Practice")
window.geometry("400x300")

# Create the Notebook (the tab container)
notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True, padx=10, pady=10)

# Create a Frame for each tab
tab1 = tk.Frame(notebook)
tab2 = tk.Frame(notebook)

# Add the frames to the notebook with tab labels
notebook.add(tab1, text="First Tab")
notebook.add(tab2, text="Second Tab")

# Put some widgets inside each tab
tk.Label(tab1, text="This is the first tab.", font=("Arial", 14)).pack(pady=50)
tk.Label(tab2, text="This is the second tab.", font=("Arial", 14)).pack(pady=50)

window.mainloop()
```

Run it. You'll see two tabs at the top. Click between them — each one shows different content.

### What happened?

Let's break it down line by line.

```python
notebook = ttk.Notebook(window)
```

Creates the **Notebook** — the empty container that holds tabs.

```python
tab1 = tk.Frame(notebook)
tab2 = tk.Frame(notebook)
```

Each tab is actually a **Frame** inside the notebook. A Frame is just an invisible container that holds widgets.

> **Important:** Tabs are Frames. Whenever you put a widget into a tab, the parent is the *frame*, not the notebook.

```python
notebook.add(tab1, text="First Tab")
notebook.add(tab2, text="Second Tab")
```

This connects each frame to the notebook and gives it a label. The `text` is what the user sees on the tab.

```python
tk.Label(tab1, text="...").pack(...)
```

Notice the first argument is `tab1`, not `window`. This widget lives **inside** the tab.

---

## Part 4: The Recipe

Every time you use Notebook, follow this recipe:

1. Create the **Notebook**
2. Create a **Frame** for each tab
3. Use `notebook.add(frame, text="Tab Label")` to attach the frame
4. Put widgets inside the frames (use the frame as the parent, not the window)

That's it. Once you understand this, you can build any tabbed interface.

---

## Part 5: Different Widgets in Different Tabs

Let's try a more useful example — a tab for "About" info and a tab for "Settings."

```python
import tkinter as tk
from tkinter import ttk

window = tk.Tk()
window.title("Tabs Demo")
window.geometry("400x300")

notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True, padx=10, pady=10)

# ----- About tab -----
about_tab = tk.Frame(notebook)
notebook.add(about_tab, text="About")

tk.Label(about_tab, text="My Notes App", font=("Arial", 18, "bold")).pack(pady=20)
tk.Label(about_tab, text="Version 1.0").pack()
tk.Label(about_tab, text="Made by Mr. Gazzingan").pack(pady=10)

# ----- Settings tab -----
settings_tab = tk.Frame(notebook)
notebook.add(settings_tab, text="Settings")

tk.Label(settings_tab, text="App Settings", font=("Arial", 14, "bold")).pack(pady=10)

tk.Label(settings_tab, text="Your Name:").pack()
name_entry = tk.Entry(settings_tab, width=30)
name_entry.pack(pady=5)

tk.Label(settings_tab, text="Favorite Color:").pack()
color_entry = tk.Entry(settings_tab, width=30)
color_entry.pack(pady=5)

tk.Button(settings_tab, text="Save Settings").pack(pady=10)

window.mainloop()
```

Run it. Click between the tabs — each one has a completely different layout. Yet they're all in the same window.

---

## Part 6: Reorganizing the Notes App with Tabs

Now let's improve the Notes app from the main lecture. Currently, everything is on one screen. Let's split it into two tabs:

- **Tab 1: Add Note** — Just the input box and Save button
- **Tab 2: View Notes** — The list of all notes and the Delete button

```python
import tkinter as tk
from tkinter import ttk, messagebox
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
        messagebox.showwarning("Empty Note", "Please type something.")
        return
    cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
    conn.commit()
    entry.delete(0, tk.END)
    refresh_list()
    messagebox.showinfo("Saved", "Note saved successfully!")

def delete_note():
    selection = listbox.curselection()
    if not selection:
        messagebox.showinfo("No Selection", "Please click a note to delete.")
        return
    if not messagebox.askyesno("Confirm", "Delete this note?"):
        return
    index = selection[0]
    note_id = note_ids[index]
    cur.execute("DELETE FROM notes WHERE id = ?", (note_id,))
    conn.commit()
    refresh_list()

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App (Tabbed)")
window.geometry("500x400")

# Create the Notebook
notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True, padx=10, pady=10)

# ----- Tab 1: Add Note -----
add_tab = tk.Frame(notebook)
notebook.add(add_tab, text="Add Note")

tk.Label(add_tab, text="Type your note:", font=("Arial", 12)).pack(pady=10)

entry = tk.Entry(add_tab, width=50)
entry.pack(pady=5)

tk.Button(add_tab, text="Save Note", command=save_note, width=20).pack(pady=20)

# ----- Tab 2: View Notes -----
view_tab = tk.Frame(notebook)
notebook.add(view_tab, text="View Notes")

tk.Label(view_tab, text="All saved notes:", font=("Arial", 12)).pack(pady=5)

listbox = tk.Listbox(view_tab, width=50, height=12)
listbox.pack(pady=5, padx=10, fill="both", expand=True)

tk.Button(view_tab, text="Delete Selected", command=delete_note).pack(pady=10)

refresh_list()

window.mainloop()
```

Run it. You'll see two tabs at the top. Click "Add Note" to type a new one. Click "View Notes" to see the list and delete things.

### What changed?

The **logic** is the same. The **layout** is different.

- We created two frames: `add_tab` and `view_tab`
- Every widget that used to have `window` as its parent now has `add_tab` or `view_tab`
- The notebook handles the tab switching for us

That's the whole trick. Tabs don't change *what* your app does — they just organize *where* it appears.

---

## Part 7: When to Use Tabs

Tabs are great for:

- **Different actions** — Add, View, Settings
- **Different categories** — Personal Notes, Work Notes, School Notes
- **Different views of the same data** — List View, Chart View, Calendar View

Tabs are NOT great for:

- **Workflows that need to be done in order** — Step 1 → Step 2 → Step 3 (use a different layout for that)
- **Apps with only 1 section** — Just use the window directly, no need for tabs

A good rule: if you find yourself with **2 or more distinct sections** in your app, tabs probably help.

---

## Part 8: One Tricky Thing — Variables Across Tabs

If you put an Entry in `add_tab` and a Button in `view_tab` that needs to read it, they can still talk to each other. **Tabs don't isolate widgets from your code.**

```python
# In Tab 1
entry = tk.Entry(add_tab)
entry.pack()

# In Tab 2
def use_entry():
    text = entry.get()    # This works! Even though entry is in a different tab.
    print(text)

button = tk.Button(view_tab, text="Use Entry Text", command=use_entry)
button.pack()
```

The widgets are organized visually by tab, but in your Python code they're all just variables. Any function can access any widget.

This is helpful but can also be confusing. **Best practice:** put widgets near the function that uses them, and use clear variable names like `add_entry` and `view_entry` so you don't mix them up.

---

## Quick Reference

### Basic Notebook

```python
from tkinter import ttk

notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True)

tab1 = tk.Frame(notebook)
notebook.add(tab1, text="Tab Label")

tk.Label(tab1, text="Inside the tab").pack()
```

### The Recipe

1. Create the Notebook
2. Create one Frame per tab
3. Use `notebook.add(frame, text="Label")` to attach each frame
4. Put widgets inside the frames, using the frame as the parent

---

## Practice Activities

1. **Two-Tab Calculator** — Tab 1: Basic Calculator (add, subtract). Tab 2: Advanced Calculator (multiply, divide).

2. **Phonebook with Tabs** — Tab 1: Add Contact. Tab 2: View Contacts.

3. **Student List** — Tab 1: Add Student. Tab 2: View All Students.

4. **Sari-Sari Inventory** — Tab 1: Add Product. Tab 2: View Inventory.

For each one, follow the recipe:

1. Plan your tabs (what does each one do?)
2. Create a frame for each tab
3. Build the widgets inside each frame
4. Write the functions to make everything work

---

## What's Next?

You now know three powerful add-ons to the basic Tkinter + SQLite app:

- **Exception Handling and Messagebox** — make your app robust and friendly
- **Treeview** — show data in proper tables
- **Tab View** — organize your app into clean sections

Combine all three, and you have the building blocks for **almost any data-driven desktop app**. Try building one of the practice activities using all three techniques together — exception handling for safety, Treeview for displaying data, and tabs for organization.

You're no longer building toy apps. You're building real ones.

---

*End of Mini-Lecture*
