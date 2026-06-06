# The Treeview Widget

### A Beginner-Friendly Mini-Lecture for Programming 2

*Show your data in a real table, not a plain list.*

---

## What You Will Learn

In this short lecture, you will learn how to:

- Display data in a table with multiple columns
- Read which row the user clicked
- Update a Treeview when your data changes

This is a follow-up to the main **Tkinter + SQLite** lecture. We'll upgrade the Notes app to use a real table view.

---

## Part 1: Why Use a Treeview?

In the main lecture, we used a `Listbox` to show our notes. It works, but it has limits:

- It only shows **one column** of text
- You can't see the id, date, or any extra info
- It looks plain

A **Treeview** is a much better widget for showing data. It can:

- Show multiple columns (like a spreadsheet)
- Add column headers
- Look more like a real app

Look at any modern app — Excel, your phone's contacts, file managers. They all use table-like views. That's what Treeview gives us.

---

## Part 2: Where Does Treeview Come From?

Treeview is part of a special Tkinter add-on called **ttk** (Themed Tkinter). It contains widgets that look nicer and more modern than the basic ones.

To use it, we import it like this:

```python
from tkinter import ttk
```

After that, we write `ttk.Treeview(...)` instead of `tk.Treeview(...)`.

---

## Part 3: A Minimal Treeview

Create a new file called `treeview_practice.py`:

```python
import tkinter as tk
from tkinter import ttk

window = tk.Tk()
window.title("Treeview Practice")
window.geometry("400x250")

tree = ttk.Treeview(window, columns=("name", "course"), show="headings")
tree.heading("name", text="Name")
tree.heading("course", text="Course")
tree.pack(fill="both", expand=True, padx=10, pady=10)

# Add some sample rows
tree.insert("", tk.END, values=("Andrea Reyes", "BSIT"))
tree.insert("", tk.END, values=("Juan dela Cruz", "BSCpE"))
tree.insert("", tk.END, values=("Maria Santos", "BSCS"))

window.mainloop()
```

Run it. You'll see a nice table with two columns and three rows.

### What did each line do?

Let's go through this carefully because there's a lot happening.

```python
tree = ttk.Treeview(window, columns=("name", "course"), show="headings")
```

This creates the Treeview with two columns. Two important pieces:

- **`columns=("name", "course")`** — Tells Treeview the column names. These are internal IDs, not what the user sees.
- **`show="headings"`** — Hides Treeview's first hidden column. Without this, you'd see an extra blank column on the left. *Always include this.*

```python
tree.heading("name", text="Name")
tree.heading("course", text="Course")
```

This sets the **header text** — what the user sees at the top of each column.

```python
tree.insert("", tk.END, values=("Andrea Reyes", "BSIT"))
```

This adds a row. Let's break it down:

- **`""`** — The empty string means "this is a top-level row." (Treeview can show tree-like nested rows, but we're not using that.)
- **`tk.END`** — Add the row at the bottom.
- **`values=(...)`** — The values for each column, in order.

---

## Part 4: Customizing Column Widths

By default, Treeview makes all columns the same width. Usually that's not what you want — a "Name" column should be wider than an "Age" column.

Use `tree.column(...)` to set widths:

```python
tree = ttk.Treeview(window, columns=("name", "course"), show="headings")

tree.heading("name", text="Name")
tree.heading("course", text="Course")

tree.column("name", width=200)
tree.column("course", width=100)
```

You can also set alignment:

```python
tree.column("course", width=100, anchor="center")
```

The `anchor` controls where the text sits inside the cell. Common values: `"w"` (left), `"center"`, `"e"` (right).

---

## Part 5: A Bigger Example

Let's build a slightly more realistic table:

```python
import tkinter as tk
from tkinter import ttk

window = tk.Tk()
window.title("Students")
window.geometry("500x300")

tree = ttk.Treeview(window, columns=("id", "name", "course", "year"), show="headings")

tree.heading("id", text="ID")
tree.heading("name", text="Name")
tree.heading("course", text="Course")
tree.heading("year", text="Year")

tree.column("id", width=50, anchor="center")
tree.column("name", width=200)
tree.column("course", width=100, anchor="center")
tree.column("year", width=80, anchor="center")

tree.pack(fill="both", expand=True, padx=10, pady=10)

students = [
    (1, "Andrea Reyes", "BSIT", 2),
    (2, "Juan dela Cruz", "BSCpE", 3),
    (3, "Maria Santos", "BSCS", 1),
    (4, "Joshua Bautista", "BSIT", 4),
]

for student in students:
    tree.insert("", tk.END, values=student)

window.mainloop()
```

Run it. You'll see a proper-looking table.

> **Tip:** `fill="both", expand=True` tells the Treeview to fill all available space and grow when the window resizes. Without this, it looks tiny.

---

## Part 6: Knowing Which Row Was Selected

The whole point of a Treeview is to let users *interact* with rows. To know which row the user clicked, use this pattern:

```python
def on_select(event):
    selected = tree.selection()
    if not selected:
        return
    values = tree.item(selected[0])["values"]
    print("You selected:", values)

tree.bind("<<TreeviewSelect>>", on_select)
```

Three things to understand:

- **`tree.selection()`** — Returns a list of selected row IDs (usually just one).
- **`tree.item(row_id)["values"]`** — Gets the values of that row as a list.
- **`tree.bind("<<TreeviewSelect>>", ...)`** — Runs the function whenever the user picks a row.

### Full Working Example

```python
import tkinter as tk
from tkinter import ttk

def on_select(event):
    selected = tree.selection()
    if not selected:
        return
    values = tree.item(selected[0])["values"]
    label.config(text=f"Selected: {values[1]} from {values[2]}")

window = tk.Tk()
window.title("Click a Row")
window.geometry("500x350")

tree = ttk.Treeview(window, columns=("id", "name", "course"), show="headings")
tree.heading("id", text="ID")
tree.heading("name", text="Name")
tree.heading("course", text="Course")
tree.column("id", width=50, anchor="center")
tree.column("name", width=200)
tree.column("course", width=100, anchor="center")
tree.pack(fill="both", expand=True, padx=10, pady=10)

tree.bind("<<TreeviewSelect>>", on_select)

students = [
    (1, "Andrea Reyes", "BSIT"),
    (2, "Juan dela Cruz", "BSCpE"),
    (3, "Maria Santos", "BSCS"),
]
for s in students:
    tree.insert("", tk.END, values=s)

label = tk.Label(window, text="Click a row above", font=("Arial", 12))
label.pack(pady=10)

window.mainloop()
```

Run it. Click any row. The label updates instantly.

---

## Part 7: Clearing and Refreshing

You'll often want to clear the Treeview and reload data — for example, after adding or deleting a row in a database.

```python
def refresh_tree():
    # Clear everything first
    for item in tree.get_children():
        tree.delete(item)
    # Then reload
    for student in students:
        tree.insert("", tk.END, values=student)
```

The key part: `tree.get_children()` returns the IDs of all rows. We loop through them and delete each one.

> **Pattern:** Whenever your data changes, call `refresh_tree()`. Don't try to add/remove single items — clearing and reloading is simpler and more reliable.

---

## Part 8: Adding a Scrollbar

If you have many rows, you want a scrollbar. Here's the standard pattern:

```python
import tkinter as tk
from tkinter import ttk

window = tk.Tk()
window.title("Scrollable Treeview")
window.geometry("500x250")

# Container frame for tree + scrollbar
frame = tk.Frame(window)
frame.pack(fill="both", expand=True, padx=10, pady=10)

# Create the Treeview
tree = ttk.Treeview(frame, columns=("name", "course"), show="headings")
tree.heading("name", text="Name")
tree.heading("course", text="Course")

# Create the scrollbar
scrollbar = ttk.Scrollbar(frame, orient="vertical", command=tree.yview)
tree.configure(yscrollcommand=scrollbar.set)

# Pack them side by side
tree.pack(side="left", fill="both", expand=True)
scrollbar.pack(side="right", fill="y")

# Add a bunch of rows to test scrolling
for i in range(30):
    tree.insert("", tk.END, values=(f"Student {i+1}", "BSIT"))

window.mainloop()
```

The recipe:

1. Put both the Treeview and Scrollbar inside a Frame
2. Connect them with `command=tree.yview` and `yscrollcommand=scrollbar.set`
3. Pack the Treeview on the left, the scrollbar on the right

Copy this recipe whenever you need a scrollable Treeview.

---

## Part 9: Upgrading the Notes App

Let's update the Notes app from the main lecture to use a Treeview. We'll show two columns: the ID and the note content.

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

# ----- Functions -----
def refresh_tree():
    for item in tree.get_children():
        tree.delete(item)
    cur.execute("SELECT id, content FROM notes")
    for row in cur.fetchall():
        tree.insert("", tk.END, values=row)

def save_note():
    note = entry.get().strip()
    if note == "":
        messagebox.showwarning("Empty Note", "Please type something.")
        return
    cur.execute("INSERT INTO notes (content) VALUES (?)", (note,))
    conn.commit()
    entry.delete(0, tk.END)
    refresh_tree()

def delete_note():
    selected = tree.selection()
    if not selected:
        messagebox.showinfo("No Selection", "Please click a note to delete first.")
        return
    if not messagebox.askyesno("Confirm", "Delete this note?"):
        return
    note_id = tree.item(selected[0])["values"][0]
    cur.execute("DELETE FROM notes WHERE id = ?", (note_id,))
    conn.commit()
    refresh_tree()

# ----- Build the window -----
window = tk.Tk()
window.title("My Notes App (with Treeview)")
window.geometry("500x450")

tk.Label(window, text="Type your note:").pack(pady=5)

entry = tk.Entry(window, width=50)
entry.pack(pady=5)

tk.Button(window, text="Save Note", command=save_note).pack(pady=5)

# Treeview
tree = ttk.Treeview(window, columns=("id", "content"), show="headings", height=10)
tree.heading("id", text="ID")
tree.heading("content", text="Note")
tree.column("id", width=50, anchor="center")
tree.column("content", width=400)
tree.pack(fill="both", expand=True, padx=10, pady=10)

tk.Button(window, text="Delete Selected", command=delete_note).pack(pady=5)

refresh_tree()

window.mainloop()
```

### What changed from the original?

1. **Listbox → Treeview** — Now we see both ID and content side by side.
2. **No more `note_ids` list** — In the Listbox version, we kept a parallel list to track database IDs. With Treeview, we just read the ID directly from the selected row: `tree.item(selected[0])["values"][0]`. Much cleaner!
3. **Two-column display** — The user can see exactly which note has which ID.

This is the real strength of Treeview: **the data structure matches what the user sees.** You don't need to keep separate parallel lists.

---

## Part 10: Showing More Columns

Now that we have a Treeview, let's give our notes more info. Update the table to also store a category:

```python
cur.execute("""
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        content TEXT NOT NULL,
        category TEXT NOT NULL DEFAULT 'General'
    )
""")
```

Then update the Treeview to show all three columns:

```python
tree = ttk.Treeview(window, columns=("id", "content", "category"), show="headings")
tree.heading("id", text="ID")
tree.heading("content", text="Note")
tree.heading("category", text="Category")

tree.column("id", width=50, anchor="center")
tree.column("content", width=300)
tree.column("category", width=100, anchor="center")
```

And update `save_note`:

```python
def save_note():
    note = entry.get().strip()
    cat = category_entry.get().strip()
    if note == "":
        messagebox.showwarning("Empty Note", "Please type a note.")
        return
    if cat == "":
        cat = "General"
    cur.execute("INSERT INTO notes (content, category) VALUES (?, ?)", (note, cat))
    conn.commit()
    entry.delete(0, tk.END)
    category_entry.delete(0, tk.END)
    refresh_tree()
```

And `refresh_tree`:

```python
def refresh_tree():
    for item in tree.get_children():
        tree.delete(item)
    cur.execute("SELECT id, content, category FROM notes")
    for row in cur.fetchall():
        tree.insert("", tk.END, values=row)
```

That's it. Three columns of data, side by side, with proper headers. Try adding a few notes with different categories like "Work", "Personal", or "School".

---

## Quick Reference

### Creating a Treeview

```python
from tkinter import ttk

tree = ttk.Treeview(window, columns=("col1", "col2"), show="headings")
tree.heading("col1", text="Header 1")
tree.heading("col2", text="Header 2")
tree.column("col1", width=100, anchor="center")
tree.column("col2", width=200)
tree.pack(fill="both", expand=True)
```

### Inserting a Row

```python
tree.insert("", tk.END, values=("value1", "value2"))
```

### Reading the Selected Row

```python
selected = tree.selection()
if selected:
    values = tree.item(selected[0])["values"]
```

### Clearing All Rows

```python
for item in tree.get_children():
    tree.delete(item)
```

### Detecting Selection

```python
def on_select(event):
    # ... handle the click

tree.bind("<<TreeviewSelect>>", on_select)
```

---

## Practice Activities

1. **Student List** — Build a Treeview that shows ID, Name, Course, and Year. Add a form to insert new students.

2. **Phonebook with Treeview** — Show contact names and phone numbers in a Treeview instead of a Listbox.

3. **Inventory Display** — Show Product, Price, and Stock in a Treeview. Bonus: show the total inventory value at the bottom.

4. **Grade Logger** — Show Subject, Grade, and Term in a Treeview. Bonus: show the average grade at the bottom.

For each one, follow the same recipe:

1. Define your columns
2. Set headers and widths
3. Write `refresh_tree()` to load from the database
4. Write `save` and `delete` functions that update the database and call refresh

---

## What's Next?

Now that your app shows data in a proper table, you can:

- Handle errors gracefully → see the **Exception Handling and Messagebox** mini-lecture
- Organize your app into tabs → see the **Tab View** mini-lecture

Your apps are starting to look professional. Keep building.

---

*End of Mini-Lecture*
