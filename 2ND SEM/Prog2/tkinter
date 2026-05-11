# Python Tkinter
## Lecture: Introduction to GUI Programming

**Course:** Programming | **Topic:** Graphical User Interface (GUI) Basics  
**Prerequisites:** Basic Python (variables, functions, loops)

---

## 1. What is Tkinter?

**Tkinter** (Tk Interface) is Python's **standard GUI library** — it comes built-in with Python, so no installation is needed. It allows you to create **windows, buttons, text fields, labels**, and other visual components for desktop applications.

> Think of Tkinter as the toolbox you use to build the "face" of your program — the part users actually see and click on.

### Why Tkinter?
- Built into Python (no pip install needed)
- Simple and beginner-friendly
- Cross-platform (Windows, macOS, Linux)
- Great for learning GUI fundamentals before frameworks like PyQt or Kivy

---

## 2. Core Concept: The Window (Root)

Every Tkinter application starts with a **root window** — the main container for everything else.

```python
import tkinter as tk

# Step 1: Create the main window
root = tk.Tk()

# Step 2: Set window properties
root.title("My First App")
root.geometry("400x300")   # Width x Height in pixels

# Step 3: Start the event loop
root.mainloop()
```

### Breakdown:
| Line | What it does |
|------|-------------|
| `tk.Tk()` | Creates the main application window |
| `.title()` | Sets the text in the title bar |
| `.geometry()` | Sets the window size (width x height) |
| `.mainloop()` | Keeps the window open and listens for events |

> ⚠️ **Important:** `mainloop()` must always be the **last line**. It starts the event loop — without it, the window opens and immediately closes.

---

## 3. Widgets — The Building Blocks

A **widget** is any visual element you place inside a window. Here are the most common ones:

| Widget | Class | Purpose |
|--------|-------|---------|
| Label | `tk.Label` | Display text or images |
| Button | `tk.Button` | Clickable button |
| Entry | `tk.Entry` | Single-line text input |
| Text | `tk.Text` | Multi-line text input |
| Frame | `tk.Frame` | Container to group widgets |

---

## 4. Your First Widgets

### 4.1 Label

```python
import tkinter as tk

root = tk.Tk()
root.title("Label Demo")
root.geometry("300x200")

# Create a label
label = tk.Label(root, text="Hello, SPUP!", font=("Arial", 16))
label.pack(pady=20)  # Add vertical spacing

root.mainloop()
```

### 4.2 Button

```python
import tkinter as tk

def on_click():
    print("Button was clicked!")

root = tk.Tk()
root.title("Button Demo")
root.geometry("300x200")

btn = tk.Button(root, text="Click Me", command=on_click)
btn.pack(pady=20)

root.mainloop()
```

> The `command` parameter links a **function** to the button click. This is called an **event handler** or **callback**.

### 4.3 Entry (Text Input)

```python
import tkinter as tk

def greet():
    name = entry.get()          # Get the text typed by user
    label.config(text=f"Hello, {name}!")

root = tk.Tk()
root.title("Entry Demo")
root.geometry("300x200")

entry = tk.Entry(root, width=25)
entry.pack(pady=10)

btn = tk.Button(root, text="Greet", command=greet)
btn.pack(pady=5)

label = tk.Label(root, text="")
label.pack(pady=10)

root.mainloop()
```

---

## 5. Layout Management — `pack()`

Tkinter has **3 layout managers**. For now, we use `pack()` — the simplest one.

`pack()` places widgets **one after another** (top to bottom by default).

### Common `pack()` options:
```python
widget.pack(side="top")       # Default — stacks top to bottom
widget.pack(side="left")      # Places widget to the left
widget.pack(pady=10)          # Adds vertical (y-axis) padding
widget.pack(padx=10)          # Adds horizontal (x-axis) padding
widget.pack(fill="x")         # Stretches widget horizontally
```

---

## 6. Styling Widgets

You can customize the appearance of any widget using options:

```python
label = tk.Label(
    root,
    text="Styled Label",
    font=("Courier", 14, "bold"),
    fg="white",         # Text (foreground) color
    bg="#2c3e50",       # Background color
    padx=10,
    pady=10
)
label.pack()
```

Common style options:
| Option | Description |
|--------|-------------|
| `font` | Tuple: `("font name", size, "style")` |
| `fg` | Foreground (text) color |
| `bg` | Background color |
| `width` | Widget width (in characters for text widgets) |
| `height` | Widget height |
| `relief` | Border style: `flat`, `raised`, `sunken`, `groove` |

---

## 7. Putting It All Together — Mini App

**Simple Name Greeter App:**

```python
import tkinter as tk

def greet():
    name = name_entry.get()
    if name.strip() == "":
        result_label.config(text="Please enter your name.", fg="red")
    else:
        result_label.config(text=f"Welcome, {name}! 🎉", fg="green")

# Main window
root = tk.Tk()
root.title("Greeter App")
root.geometry("350x200")
root.configure(bg="#f0f0f0")

# Widgets
tk.Label(root, text="Enter your name:", bg="#f0f0f0", font=("Arial", 12)).pack(pady=10)

name_entry = tk.Entry(root, font=("Arial", 12), width=25)
name_entry.pack()

tk.Button(root, text="Greet Me", command=greet, bg="#3498db", fg="white",
          font=("Arial", 11), padx=10, pady=5).pack(pady=10)

result_label = tk.Label(root, text="", bg="#f0f0f0", font=("Arial", 13, "bold"))
result_label.pack()

root.mainloop()
```

---

## 8. Key Takeaways

1. Every Tkinter app needs `tk.Tk()` to create the window and `mainloop()` to run it.
2. Widgets are created by passing the parent window as the first argument.
3. Use `.pack()` to place widgets on screen.
4. Use `command=function_name` to connect buttons to actions.
5. Use `.get()` to read user input from an `Entry` widget.
6. Use `.config()` to update a widget's properties after it's created.

---

## Practice Exercise

**Task:** Build a simple **"Student Info Card"** GUI app with the following:
- A title label: *"Student Information"*
- Two `Entry` fields: one for **Name**, one for **Student ID**
- A **Submit** button that displays the entered info in a label below
- If either field is empty, show an error message in red

> **Hint:** Use `entry.get()` to retrieve values, and `label.config(text=...)` to update the display.

---

*Next Lecture: Layout Managers (`grid()`) and More Widgets (Checkbutton, Radiobutton, Listbox)*
