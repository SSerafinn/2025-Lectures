# Upgrading the Sari-Sari Inventory App

### Programming 2 — Tkinter + SQLite (Follow-up Lesson)

*Take your working Inventory app and make it more robust, more visual, and more organized — one step at a time.*

---

## What You Will Learn

You already have a working Sari-Sari Inventory App. It does its job — adds products, displays them, deletes them. But there are still things we can improve:

1. **It fails silently.** If the user types bad input or makes a mistake, the app does nothing and gives no feedback.
2. **The display is cramped.** Everything is squeezed into one Listbox line per product.
3. **The window is getting crowded.** All controls are stacked on top of each other.
4. **There's no way to edit.** If a price is wrong, the only fix is delete + re-add.

In this lesson, we will fix each of these in order:

- **Step 1** — Understand the exception handling already in your code
- **Step 2** — Add message boxes for clearer feedback
- **Step 3** — Replace the Listbox with a Treeview (a real table)
- **Step 4** — Reorganize features into tabs using a Notebook
- **Step 5** — Add an Update feature in its own tab

By the end, your Inventory app will look and behave like real software.

---

## Starting Point

Here is the code we will build on. Make sure yours runs:

```python
import tkinter as tk
import sqlite3

# ----- Database setup -----
conn = sqlite3.connect("inventory.db")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        price REAL NOT NULL,
        stock INTEGER NOT NULL
    )
""")
conn.commit()

# ----- Functions -----
def refresh_list():
    listbox.delete(0, tk.END)
    cur.execute("SELECT id, name, price, stock FROM products")
    for product_id, name, price, stock in cur.fetchall():
        listbox.insert(tk.END, f"[{product_id}] {name} — PHP {price:.2f} (Stock: {stock})")

def add_product():
    name = name_entry.get().strip()
    price_text = price_entry.get().strip()
    stock_text = stock_entry.get().strip()
    if name == "" or price_text == "" or stock_text == "":
        return
    try:
        price = float(price_text)
        stock = int(stock_text)
    except ValueError:
        return
    cur.execute(
        "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)",
        (name, price, stock)
    )
    conn.commit()
    name_entry.delete(0, tk.END)
    price_entry.delete(0, tk.END)
    stock_entry.delete(0, tk.END)
    refresh_list()

def delete_product():
    delete_id = id_entry.get().strip()
    if delete_id == "":
        return
    cur.execute("DELETE FROM products WHERE id = ?", (delete_id,))
    conn.commit()
    id_entry.delete(0, tk.END)
    refresh_list()

# (window and widget code...)
```

We will improve this step by step.

---

# Step 1: Understanding Exception Handling

Before we add anything new, let's understand a feature that's already in your code — but maybe you didn't notice.

## 1.1 The Hidden Safety Net

Look closely at your `add_product()` function:

```python
try:
    price = float(price_text)
    stock = int(stock_text)
except ValueError:
    return
```

This block is **exception handling**. Without it, your program would crash if the user typed something silly.

## 1.2 What Could Go Wrong?

Imagine the user does this:

- Types `"abc"` in the Price field
- Types `"five"` in the Stock field
- Types nothing but spaces

Then they click "Add Product." Your code reaches this line:

```python
price = float(price_text)
```

Python tries to convert `"abc"` into a number. It can't, so it raises a **ValueError** — a kind of error specifically about "wrong value type." If nothing catches it, your program crashes and the window may freeze or close.

## 1.3 The try/except Pattern

The `try` block lets us *attempt* something that might fail. If it does fail, the `except` block runs instead of crashing.

```python
try:
    # Code that might fail
    price = float(price_text)
    stock = int(stock_text)
except ValueError:
    # Code that runs if the conversion fails
    return
```

Walk through what happens when the user types `"abc"` for price:

1. Python enters the `try` block.
2. It tries `float("abc")`.
3. That raises a `ValueError`.
4. Python jumps to `except ValueError:`.
5. The `return` statement exits the function — no crash, no save.

The app stays running. The user's bad input is rejected safely.

## 1.4 Why "ValueError" Specifically?

Python has many kinds of errors: `TypeError`, `ZeroDivisionError`, `FileNotFoundError`, and so on. The `except ValueError:` line tells Python: *"I only want to catch ValueError specifically. If a different kind of error happens, let it through."*

This is good practice. Catching every possible error with `except:` (no type) is dangerous — it can hide bugs you actually need to know about.

> **Rule of thumb:** Catch the specific error you expect. Let unexpected errors crash loudly so you can find and fix them.

## 1.5 The Silent Problem

Here's the catch: your current code catches the error but **does nothing to tell the user**.

```python
except ValueError:
    return            # <-- just exits silently
```

The user clicks "Add Product," nothing happens, and they have no idea why. Did the program freeze? Did the click not register? Was something wrong with their input?

This is exactly the problem we'll fix in Step 2.

---

# Step 2: Adding Message Boxes for Feedback

Now that we understand *why* the silent failures happen, let's give the user real feedback.

## 2.1 The messagebox Module

Tkinter has a built-in tool for pop-ups called `messagebox`. Add this line to the top of your file:

```python
from tkinter import messagebox
```

## 2.2 Three Pop-ups You'll Use

```python
# An information message — just an "OK" button
messagebox.showinfo("Title", "Your message here.")

# A warning message — an "OK" button with a warning icon
messagebox.showwarning("Title", "Something needs attention.")

# A yes/no question — returns True if the user clicks Yes
answer = messagebox.askyesno("Title", "Are you sure?")
```

Each takes a **title** (top of the pop-up) and a **message** (the text inside).

The first two just *display* something. The third one, `askyesno`, **returns a value** — `True` if Yes is clicked, `False` if No. You can use that in an `if` statement.

## 2.3 Replacing Silent Returns with Warnings

Let's go through your `add_product()` function and replace every silent `return` with a real warning.

**Empty fields:**

```python
if name == "" or price_text == "" or stock_text == "":
    messagebox.showwarning("Missing Information", "Please fill in all fields.")
    return
```

**Invalid number conversion:**

```python
try:
    price = float(price_text)
    stock = int(stock_text)
except ValueError:
    messagebox.showwarning("Invalid Input", "Price must be a number and stock must be a whole number.")
    return
```

Now when the user enters bad input, they see a clear message instead of nothing.

## 2.4 Confirming Successful Saves

It's also nice to confirm that something *did* work. At the end of `add_product()`, after the database is updated:

```python
refresh_list()
messagebox.showinfo("Saved", "Product added successfully.")
```

## 2.5 Improving Delete

Deleting is permanent. It deserves a confirmation. Update `delete_product()` like this:

```python
def delete_product():
    delete_id = id_entry.get().strip()

    if delete_id == "":
        messagebox.showwarning("Missing ID", "Please enter an ID to delete.")
        return

    confirm = messagebox.askyesno("Confirm Delete", "Are you sure you want to delete this product?")
    if not confirm:
        return

    cur.execute("DELETE FROM products WHERE id = ?", (delete_id,))
    conn.commit()
    id_entry.delete(0, tk.END)
    refresh_list()
    messagebox.showinfo("Deleted", "Product deleted.")
```

Two new things:

- A warning if the ID field is empty.
- An `askyesno` confirmation before deleting. The `if not confirm:` check stops the delete if the user clicks No.

Run the app and try:
- Adding with empty fields → warning
- Adding with letters in the Price → warning
- Adding valid data → success message
- Deleting with no ID → warning
- Deleting valid → confirmation pops up first

The app finally *talks back* to the user.

---

# Step 3: Replacing the Listbox with a Treeview

The Listbox shows products like this:

```
[1] Kopiko — PHP 7.00 (Stock: 25)
[2] Lucky Me Pancit Canton — PHP 18.50 (Stock: 40)
[3] Coke 1.5L — PHP 75.00 (Stock: 12)
```

It works, but the columns are crammed into one string. A **Treeview** shows real columns:

```
ID  | Name                  | Price | Stock
----+-----------------------+-------+------
1   | Kopiko                | 7.00  | 25
2   | Lucky Me Pancit Canton| 18.50 | 40
3   | Coke 1.5L             | 75.00 | 12
```

Much cleaner.

## 3.1 What is `ttk`?

The Treeview lives in a Tkinter submodule called **`ttk`** — short for "themed Tk." These are newer, more modern widgets. Add this import:

```python
from tkinter import ttk
```

You can mix `ttk` widgets with regular `tk` widgets. We'll keep using `tk.Label`, `tk.Entry`, and `tk.Button`, and just add `ttk.Treeview` where it's useful.

## 3.2 Building the Treeview

Setting up a Treeview is a four-step process:

```python
# 1. Create the Treeview, listing the column names
tree = ttk.Treeview(window, columns=("id", "name", "price", "stock"), show="headings")

# 2. Set the text shown at the top of each column
tree.heading("id", text="ID")
tree.heading("name", text="Product")
tree.heading("price", text="Price (PHP)")
tree.heading("stock", text="Stock")

# 3. Set the width of each column in pixels
tree.column("id", width=50)
tree.column("name", width=250)
tree.column("price", width=100)
tree.column("stock", width=80)

# 4. Show it in the window
tree.pack(pady=10, fill="both", expand=True)
```

What each part does:

- **`columns=(...)`** — Lists the internal names of your columns. Use these names when referring to columns.
- **`show="headings"`** — Tells the Treeview to show *only* the column headers. Without this, you'd get an unwanted extra column on the left for tree icons.
- **`tree.heading(name, text=...)`** — Sets the label shown at the top of a column.
- **`tree.column(name, width=...)`** — Sets the column's width.
- **`fill="both", expand=True`** — Makes the Treeview grow to fill available space.

## 3.3 Updating refresh_list

The Listbox-based `refresh_list()` looked like this:

```python
def refresh_list():
    listbox.delete(0, tk.END)
    cur.execute("SELECT id, name, price, stock FROM products")
    for product_id, name, price, stock in cur.fetchall():
        listbox.insert(tk.END, f"[{product_id}] {name} — PHP {price:.2f} (Stock: {stock})")
```

The Treeview version is similar but cleaner — no manual string formatting:

```python
def refresh_list():
    # Clear existing rows
    for row in tree.get_children():
        tree.delete(row)

    # Load all products
    cur.execute("SELECT id, name, price, stock FROM products")
    for product_id, name, price, stock in cur.fetchall():
        tree.insert("", tk.END, values=(product_id, name, f"{price:.2f}", stock))
```

Two new methods:

- **`tree.get_children()`** — Returns the IDs of all rows in the tree. We loop through them and delete each one.
- **`tree.insert("", tk.END, values=(...))`** — Adds a new row. The empty string `""` means "no parent" (we don't use nesting). `tk.END` means "at the bottom." `values=` takes a tuple in the same order as the columns.

The display now uses real columns instead of a single squished string.

## 3.4 Removing the Listbox

Since we're replacing the Listbox, remove the line:

```python
listbox = tk.Listbox(window, width=55, height=10)
listbox.pack(pady=10)
```

Run the app. Add a few products and check the new table. Much better.

---

# Step 4: Organizing Features with a Notebook (Tabs)

The window is still a tall vertical stack: header, form, button, table, delete inputs, delete button. As you add more features, this gets worse. The fix is **tabs**.

## 4.1 What is a Notebook?

A **Notebook** is a container that shows multiple "pages," each with its own tab label at the top. Click a tab → that page shows. It's like a folder of pages.

## 4.2 Creating the Notebook

```python
notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True, padx=10, pady=10)
```

Now we need to add pages.

## 4.3 Tabs are Frames

Each tab is a **`Frame`** — a container widget that holds other widgets. To add a tab:

```python
# Create the frame for this tab
add_tab = tk.Frame(notebook)

# Add it to the notebook with a label
notebook.add(add_tab, text="Add Product")
```

After this, every widget on the Add tab should have `add_tab` as its parent, not `window`:

```python
tk.Label(add_tab, text="Product:").pack(pady=2)
name_entry = tk.Entry(add_tab, width=30)
name_entry.pack(pady=2)
```

> **Important:** This is the #1 thing to watch for when adding tabs. If you write `tk.Label(window, ...)` when you meant `tk.Label(add_tab, ...)`, the label will appear floating outside the tabs, not inside them.

## 4.4 Reorganizing the Inventory App

Let's split the existing app into three tabs:

| Tab | What's on it |
|-----|--------------|
| Add Product | Name/Price/Stock fields + Add button |
| Delete Product | ID field + Delete button |
| All Products | The Treeview showing everything |

Here is the structure:

```python
# Create the Notebook
notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True, padx=10, pady=10)

# ----- Add Product tab -----
add_tab = tk.Frame(notebook)
notebook.add(add_tab, text="Add Product")

tk.Label(add_tab, text="Add a Product", font=("Arial", 14, "bold")).pack(pady=10)

tk.Label(add_tab, text="Product:").pack(pady=2)
name_entry = tk.Entry(add_tab, width=30)
name_entry.pack(pady=2)

tk.Label(add_tab, text="Price (PHP):").pack(pady=2)
price_entry = tk.Entry(add_tab, width=30)
price_entry.pack(pady=2)

tk.Label(add_tab, text="Stock:").pack(pady=2)
stock_entry = tk.Entry(add_tab, width=30)
stock_entry.pack(pady=2)

tk.Button(add_tab, text="Add Product", command=add_product).pack(pady=15)

# ----- Delete Product tab -----
delete_tab = tk.Frame(notebook)
notebook.add(delete_tab, text="Delete Product")

tk.Label(delete_tab, text="Delete a Product", font=("Arial", 14, "bold")).pack(pady=10)
tk.Label(delete_tab, text="ID to Delete:").pack(pady=2)
id_entry = tk.Entry(delete_tab, width=10)
id_entry.pack(pady=2)
tk.Button(delete_tab, text="Delete Product", command=delete_product).pack(pady=15)

# ----- All Products tab -----
all_tab = tk.Frame(notebook)
notebook.add(all_tab, text="All Products")

tk.Label(all_tab, text="Inventory Catalog", font=("Arial", 14, "bold")).pack(pady=10)

tree = ttk.Treeview(all_tab, columns=("id", "name", "price", "stock"), show="headings")
tree.heading("id", text="ID")
tree.heading("name", text="Product")
tree.heading("price", text="Price (PHP)")
tree.heading("stock", text="Stock")
tree.column("id", width=50)
tree.column("name", width=250)
tree.column("price", width=100)
tree.column("stock", width=80)
tree.pack(pady=10, fill="both", expand=True, padx=10)
```

Notice how each widget's parent is the tab's frame, not `window`.

Run it. You'll see three tabs at the top — Add Product, Delete Product, All Products. Each one has its own clean page.

---

# Step 5: Adding an Update Feature (Its Own Tab)

The last missing feature is **Update** — fixing a product without deleting and re-adding it. This is the perfect chance to put it in its own tab.

## 5.1 The Plan

The Update tab works like this:

1. The user types a Product ID and clicks **Load**.
2. The app fetches that product and fills the form fields with its current values.
3. The user changes whatever they want.
4. They click **Update Product**, and the database is updated.

## 5.2 The SQL UPDATE Statement

The SQL keyword for editing existing data is `UPDATE`:

```sql
UPDATE products SET name = ?, price = ?, stock = ? WHERE id = ?
```

In plain English: *"In the products table, set name, price, and stock to these new values — but only for the row whose id matches."*

> **WARNING:** Always include `WHERE` in an UPDATE. Without it, every row in the table gets changed. This is the same danger as `DELETE` without `WHERE`.

In Python:

```python
cur.execute(
    "UPDATE products SET name = ?, price = ?, stock = ? WHERE id = ?",
    (name, price, stock, product_id)
)
conn.commit()
```

The values tuple has *four* items — the three new values plus the id to find the row.

## 5.3 Tracking What We're Editing

We need to remember *which* product is being edited. When the user clicks Load, we'll save its id. When they click Update, we'll use that id.

Add this near the top of your file (with the database setup):

```python
editing_id = None
```

`None` means "we're not editing anything yet."

## 5.4 The Load Function

This function reads the typed ID, looks up the product, and fills the Update form fields:

```python
def load_product():
    global editing_id

    product_id = update_id_entry.get().strip()
    if product_id == "":
        messagebox.showwarning("Missing ID", "Please enter a product ID.")
        return

    cur.execute("SELECT name, price, stock FROM products WHERE id = ?", (product_id,))
    result = cur.fetchone()

    if result is None:
        messagebox.showwarning("Not Found", "No product with that ID.")
        return

    name, price, stock = result

    update_name_entry.delete(0, tk.END)
    update_name_entry.insert(0, name)
    update_price_entry.delete(0, tk.END)
    update_price_entry.insert(0, price)
    update_stock_entry.delete(0, tk.END)
    update_stock_entry.insert(0, stock)

    editing_id = product_id
    messagebox.showinfo("Loaded", f"Editing product ID {product_id}.")
```

New things here:

- **`cur.fetchone()`** — Like `fetchall()`, but returns *one* row instead of a list. Perfect when you expect a single result.
- **`if result is None:`** — If no product matches, `fetchone()` returns `None`. We warn and stop.
- **`name, price, stock = result`** — Unpacks the tuple into three variables in one line.
- **`entry.insert(0, text)`** — Puts text into an Entry. The `0` means "start at position 0" (the beginning).
- **`global editing_id`** — Lets the function modify the `editing_id` variable defined outside it.

## 5.5 What Does `global` Do?

Normally in Python, when a function uses a variable, it can *read* outside variables but not *change* them. Writing `editing_id = product_id` inside the function would create a new local variable that disappears when the function ends — our outside `editing_id` would stay `None`.

The `global` keyword fixes that. It tells Python: *"I want to modify the outside variable, not create a new local one."*

> **Rule:** If a function changes a variable defined outside it, write `global variable_name` at the top of the function.

## 5.6 The Update Function

This function saves the changes back to the database:

```python
def update_product():
    global editing_id

    if editing_id is None:
        messagebox.showwarning("Nothing to Update", "Click 'Load' first to choose a product.")
        return

    name = update_name_entry.get().strip()
    price_text = update_price_entry.get().strip()
    stock_text = update_stock_entry.get().strip()

    if name == "" or price_text == "" or stock_text == "":
        messagebox.showwarning("Missing Information", "Please fill in all fields.")
        return

    try:
        price = float(price_text)
        stock = int(stock_text)
    except ValueError:
        messagebox.showwarning("Invalid Input", "Price must be a number and stock must be a whole number.")
        return

    confirm = messagebox.askyesno("Confirm Update", "Save changes to this product?")
    if not confirm:
        return

    cur.execute(
        "UPDATE products SET name = ?, price = ?, stock = ? WHERE id = ?",
        (name, price, stock, editing_id)
    )
    conn.commit()

    update_id_entry.delete(0, tk.END)
    update_name_entry.delete(0, tk.END)
    update_price_entry.delete(0, tk.END)
    update_stock_entry.delete(0, tk.END)
    editing_id = None

    refresh_list()
    messagebox.showinfo("Updated", "Product updated successfully.")
```

Notice how it reuses the same checks from `add_product` — empty fields, ValueError handling — because the same things can go wrong. And it resets `editing_id = None` at the end, so the user has to load again before another update.

## 5.7 Building the Update Tab

Now let's build the Update tab itself. Add it between the Delete tab and the All Products tab:

```python
# ----- Update Product tab -----
update_tab = tk.Frame(notebook)
notebook.add(update_tab, text="Update Product")

tk.Label(update_tab, text="Update a Product", font=("Arial", 14, "bold")).pack(pady=10)

tk.Label(update_tab, text="Product ID:").pack(pady=2)
update_id_entry = tk.Entry(update_tab, width=10)
update_id_entry.pack(pady=2)
tk.Button(update_tab, text="Load", command=load_product).pack(pady=5)

tk.Label(update_tab, text="Product:").pack(pady=2)
update_name_entry = tk.Entry(update_tab, width=30)
update_name_entry.pack(pady=2)

tk.Label(update_tab, text="Price (PHP):").pack(pady=2)
update_price_entry = tk.Entry(update_tab, width=30)
update_price_entry.pack(pady=2)

tk.Label(update_tab, text="Stock:").pack(pady=2)
update_stock_entry = tk.Entry(update_tab, width=30)
update_stock_entry.pack(pady=2)

tk.Button(update_tab, text="Update Product", command=update_product).pack(pady=15)
```

The Update tab has *its own* entry widgets — `update_name_entry`, `update_price_entry`, etc. — separate from the ones in the Add tab. Each tab keeps its own form fields so they don't interfere with each other.

---

# The Complete Upgraded App

Here's the final code with all the upgrades in place:

```python
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox
import sqlite3

# ----- Database setup -----
conn = sqlite3.connect("inventory.db")
cur = conn.cursor()
cur.execute("""
    CREATE TABLE IF NOT EXISTS products (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        price REAL NOT NULL,
        stock INTEGER NOT NULL
    )
""")
conn.commit()

editing_id = None

# ----- Functions -----
def refresh_list():
    for row in tree.get_children():
        tree.delete(row)
    cur.execute("SELECT id, name, price, stock FROM products")
    for product_id, name, price, stock in cur.fetchall():
        tree.insert("", tk.END, values=(product_id, name, f"{price:.2f}", stock))

def add_product():
    name = name_entry.get().strip()
    price_text = price_entry.get().strip()
    stock_text = stock_entry.get().strip()

    if name == "" or price_text == "" or stock_text == "":
        messagebox.showwarning("Missing Information", "Please fill in all fields.")
        return

    try:
        price = float(price_text)
        stock = int(stock_text)
    except ValueError:
        messagebox.showwarning("Invalid Input", "Price must be a number and stock must be a whole number.")
        return

    cur.execute(
        "INSERT INTO products (name, price, stock) VALUES (?, ?, ?)",
        (name, price, stock)
    )
    conn.commit()

    name_entry.delete(0, tk.END)
    price_entry.delete(0, tk.END)
    stock_entry.delete(0, tk.END)
    refresh_list()
    messagebox.showinfo("Saved", "Product added successfully.")

def delete_product():
    delete_id = id_entry.get().strip()

    if delete_id == "":
        messagebox.showwarning("Missing ID", "Please enter an ID to delete.")
        return

    confirm = messagebox.askyesno("Confirm Delete", "Are you sure you want to delete this product?")
    if not confirm:
        return

    cur.execute("DELETE FROM products WHERE id = ?", (delete_id,))
    conn.commit()
    id_entry.delete(0, tk.END)
    refresh_list()
    messagebox.showinfo("Deleted", "Product deleted.")

def load_product():
    global editing_id
    product_id = update_id_entry.get().strip()
    if product_id == "":
        messagebox.showwarning("Missing ID", "Please enter a product ID.")
        return

    cur.execute("SELECT name, price, stock FROM products WHERE id = ?", (product_id,))
    result = cur.fetchone()
    if result is None:
        messagebox.showwarning("Not Found", "No product with that ID.")
        return

    name, price, stock = result
    update_name_entry.delete(0, tk.END)
    update_name_entry.insert(0, name)
    update_price_entry.delete(0, tk.END)
    update_price_entry.insert(0, price)
    update_stock_entry.delete(0, tk.END)
    update_stock_entry.insert(0, stock)

    editing_id = product_id
    messagebox.showinfo("Loaded", f"Editing product ID {product_id}.")

def update_product():
    global editing_id

    if editing_id is None:
        messagebox.showwarning("Nothing to Update", "Click 'Load' first to choose a product.")
        return

    name = update_name_entry.get().strip()
    price_text = update_price_entry.get().strip()
    stock_text = update_stock_entry.get().strip()

    if name == "" or price_text == "" or stock_text == "":
        messagebox.showwarning("Missing Information", "Please fill in all fields.")
        return

    try:
        price = float(price_text)
        stock = int(stock_text)
    except ValueError:
        messagebox.showwarning("Invalid Input", "Price must be a number and stock must be a whole number.")
        return

    confirm = messagebox.askyesno("Confirm Update", "Save changes to this product?")
    if not confirm:
        return

    cur.execute(
        "UPDATE products SET name = ?, price = ?, stock = ? WHERE id = ?",
        (name, price, stock, editing_id)
    )
    conn.commit()

    update_id_entry.delete(0, tk.END)
    update_name_entry.delete(0, tk.END)
    update_price_entry.delete(0, tk.END)
    update_stock_entry.delete(0, tk.END)
    editing_id = None

    refresh_list()
    messagebox.showinfo("Updated", "Product updated successfully.")

# ----- Window -----
window = tk.Tk()
window.title("Sari-Sari Inventory")
window.geometry("600x500")

notebook = ttk.Notebook(window)
notebook.pack(fill="both", expand=True, padx=10, pady=10)

# ----- Add Product tab -----
add_tab = tk.Frame(notebook)
notebook.add(add_tab, text="Add Product")

tk.Label(add_tab, text="Add a Product", font=("Arial", 14, "bold")).pack(pady=10)
tk.Label(add_tab, text="Product:").pack(pady=2)
name_entry = tk.Entry(add_tab, width=30)
name_entry.pack(pady=2)
tk.Label(add_tab, text="Price (PHP):").pack(pady=2)
price_entry = tk.Entry(add_tab, width=30)
price_entry.pack(pady=2)
tk.Label(add_tab, text="Stock:").pack(pady=2)
stock_entry = tk.Entry(add_tab, width=30)
stock_entry.pack(pady=2)
tk.Button(add_tab, text="Add Product", command=add_product).pack(pady=15)

# ----- Delete Product tab -----
delete_tab = tk.Frame(notebook)
notebook.add(delete_tab, text="Delete Product")

tk.Label(delete_tab, text="Delete a Product", font=("Arial", 14, "bold")).pack(pady=10)
tk.Label(delete_tab, text="ID to Delete:").pack(pady=2)
id_entry = tk.Entry(delete_tab, width=10)
id_entry.pack(pady=2)
tk.Button(delete_tab, text="Delete Product", command=delete_product).pack(pady=15)

# ----- Update Product tab -----
update_tab = tk.Frame(notebook)
notebook.add(update_tab, text="Update Product")

tk.Label(update_tab, text="Update a Product", font=("Arial", 14, "bold")).pack(pady=10)
tk.Label(update_tab, text="Product ID:").pack(pady=2)
update_id_entry = tk.Entry(update_tab, width=10)
update_id_entry.pack(pady=2)
tk.Button(update_tab, text="Load", command=load_product).pack(pady=5)
tk.Label(update_tab, text="Product:").pack(pady=2)
update_name_entry = tk.Entry(update_tab, width=30)
update_name_entry.pack(pady=2)
tk.Label(update_tab, text="Price (PHP):").pack(pady=2)
update_price_entry = tk.Entry(update_tab, width=30)
update_price_entry.pack(pady=2)
tk.Label(update_tab, text="Stock:").pack(pady=2)
update_stock_entry = tk.Entry(update_tab, width=30)
update_stock_entry.pack(pady=2)
tk.Button(update_tab, text="Update Product", command=update_product).pack(pady=15)

# ----- All Products tab -----
all_tab = tk.Frame(notebook)
notebook.add(all_tab, text="All Products")

tk.Label(all_tab, text="Inventory Catalog", font=("Arial", 14, "bold")).pack(pady=10)
tree = ttk.Treeview(all_tab, columns=("id", "name", "price", "stock"), show="headings")
tree.heading("id", text="ID")
tree.heading("name", text="Product")
tree.heading("price", text="Price (PHP)")
tree.heading("stock", text="Stock")
tree.column("id", width=50)
tree.column("name", width=250)
tree.column("price", width=100)
tree.column("stock", width=80)
tree.pack(pady=10, fill="both", expand=True, padx=10)

# Load existing products on startup
refresh_list()

window.mainloop()
```

---

# What Changed and Why

| Before | After | Why it matters |
|--------|-------|----------------|
| Silent return on bad input | Warning pop-ups | User knows what went wrong |
| Listbox with squished text | Treeview with real columns | Easier to read, looks professional |
| All controls in one window | Tabs for each action | Clean, organized interface |
| No way to edit | Update tab with Load + UPDATE SQL | Full CRUD — complete app |

---

# Common Mistakes to Avoid

**Catching too broadly with `except:`**
Using `except:` (no error type) hides bugs you'd want to find. Always catch the specific error: `except ValueError:`.

**Forgetting `show="headings"` on the Treeview**
Without it, you'll see an unwanted empty column on the left where the tree icons normally go.

**Using `window` as a widget's parent when it should be a tab**
If you write `tk.Label(window, ...)` inside what should be a tab, the label appears floating outside the tabs. The parent must be the tab's frame.

**Forgetting `global editing_id`**
Without it, your function creates a new local variable instead of modifying the outside one. The Update function won't know which product to edit.

**Forgetting `WHERE` in UPDATE**
`UPDATE products SET price = 99` (no WHERE) sets *every* product's price to 99. Always include `WHERE id = ?`.

**Mismatching column order in the values tuple**
If `columns=("id", "name", "price", "stock")`, your `values=(...)` tuple must be in the same order. Otherwise data silently lands in the wrong columns.

---

# Practice Activities

1. **Add a Search tab.** New tab with a search Entry and a Search button. Display matching products in a second Treeview using `SELECT ... WHERE name LIKE ?`.
2. **Show low-stock warning.** When a product's stock drops below 5, color the row red or display a warning in a label.
3. **Add totals.** In the All Products tab, show the total stock value (sum of `price * stock`) at the bottom.
4. **Apply the same upgrades** to your Phonebook and To-Do List apps.

---

*End of Lesson*
