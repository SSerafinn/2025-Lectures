# Python File Manipulation
### Programming 2 | St. Paul University Philippines — SITE
---

## Learning Objectives
By the end of this lesson, students should be able to:
1. **Explain** how Python interacts with the file system (Remember/Understand)
2. **Use** `open()`, `read()`, `write()`, and `close()` correctly (Apply)
3. **Distinguish** between file modes and choose the appropriate one (Analyze)
4. **Handle** file errors gracefully using `try-except` (Apply/Analyze)
5. **Parse** and write CSV and JSON files for structured data tasks (Apply/Evaluate)

---

## Part 1 — How File I/O Works

When a Python program reads or writes a file, it goes through three steps:

1. **Open** — Python asks the OS for access to the file
2. **Read / Write** — Data is transferred between memory and disk
3. **Close** — The OS releases the file, flushing any buffered data

Forgetting to close a file can cause data loss or locked files. This is why the `with` statement (covered in Part 3) is strongly preferred.

---

## Part 2 — The `open()` Function

```python
file = open(filename, mode, encoding="utf-8")
```

### File Modes

| Mode | Meaning | File must exist? | Overwrites? |
|------|---------|-----------------|-------------|
| `"r"` | Read (default) | Yes | No |
| `"w"` | Write | No (creates it) | **Yes** |
| `"a"` | Append | No (creates it) | No |
| `"x"` | Exclusive create | No (fails if exists) | — |
| `"r+"` | Read + Write | Yes | No |

Add `"b"` for binary mode (e.g., `"rb"`, `"wb"`) when working with images, PDFs, etc.

> ⚠️ **Always specify `encoding="utf-8"`** when working with text files to avoid encoding errors, especially on Windows.

### Basic Read Example

```python
file = open("students.txt", "r", encoding="utf-8")
content = file.read()
print(content)
file.close()
```

### Basic Write Example

```python
file = open("output.txt", "w", encoding="utf-8")
file.write("Hello, SPUP!\n")
file.write("Programming 2 rocks.\n")
file.close()
```

### Reading Line by Line

```python
file = open("students.txt", "r", encoding="utf-8")
for line in file:
    print(line.strip())   # .strip() removes the trailing newline
file.close()
```

Other read methods:

```python
file.read()        # entire file as one string
file.readline()    # one line at a time
file.readlines()   # list of all lines
```

---

## Part 3 — The `with` Statement (Context Manager)

The `with` statement automatically closes the file even if an error occurs. **This is the recommended way to work with files.**

```python
with open("students.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(content)
# File is automatically closed here
```

### Writing with `with`

```python
names = ["Maria Santos", "Juan dela Cruz", "Ana Reyes"]

with open("classlist.txt", "w", encoding="utf-8") as file:
    for name in names:
        file.write(name + "\n")
```

### Appending with `with`

```python
with open("classlist.txt", "a", encoding="utf-8") as file:
    file.write("Pedro Penduko\n")
```

---

## Part 4 — Exception Handling for Files

File operations can fail: the file might not exist, the disk might be full, or permissions might be denied. Always guard critical file operations with `try-except`.

### Common File Exceptions

| Exception | Cause |
|-----------|-------|
| `FileNotFoundError` | File doesn't exist (read mode) |
| `PermissionError` | No read/write access |
| `IsADirectoryError` | Path points to a folder |
| `IOError` | General I/O failure |

### Pattern: Safe File Read

```python
try:
    with open("grades.txt", "r", encoding="utf-8") as file:
        data = file.read()
        print(data)
except FileNotFoundError:
    print("Error: The file was not found.")
except PermissionError:
    print("Error: You do not have permission to read this file.")
except IOError as e:
    print(f"An I/O error occurred: {e}")
finally:
    print("File operation attempted.")
```

> 💡 The `finally` block always runs — useful for cleanup messages or logging.

---

## Part 5 — Working with CSV Files

CSV (Comma-Separated Values) is the most common format for tabular data — think Excel sheets saved as plain text.

### Sample CSV: `students.csv`
```
student_id,name,grade
2024001,Maria Santos,88
2024002,Juan dela Cruz,91
2024003,Ana Reyes,75
```

### Reading CSV with the `csv` Module

```python
import csv

with open("students.csv", "r", encoding="utf-8") as file:
    reader = csv.DictReader(file)   # reads each row as a dictionary
    for row in reader:
        print(f"{row['name']} — Grade: {row['grade']}")
```

**Output:**
```
Maria Santos — Grade: 88
Juan dela Cruz — Grade: 91
Ana Reyes — Grade: 75
```

### Writing CSV

```python
import csv

students = [
    {"student_id": "2024004", "name": "Pedro Penduko", "grade": 82},
    {"student_id": "2024005", "name": "Nena Cruz", "grade": 95},
]

with open("new_students.csv", "w", newline="", encoding="utf-8") as file:
    fieldnames = ["student_id", "name", "grade"]
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(students)
```

> ⚠️ Use `newline=""` when opening CSV files for writing to prevent extra blank lines on Windows.

---

## Part 6 — Working with JSON Files

JSON (JavaScript Object Notation) is the standard format for structured/nested data — used heavily in APIs and config files.

### Sample JSON: `student.json`
```json
{
    "student_id": "2024001",
    "name": "Maria Santos",
    "grades": {
        "Math": 88,
        "English": 92,
        "Programming": 95
    }
}
```

### Reading JSON

```python
import json

with open("student.json", "r", encoding="utf-8") as file:
    data = json.load(file)    # parses JSON into a Python dict

print(data["name"])
print(data["grades"]["Programming"])
```

### Writing JSON

```python
import json

student = {
    "student_id": "2024002",
    "name": "Juan dela Cruz",
    "grades": {"Math": 80, "English": 85, "Programming": 91}
}

with open("student.json", "w", encoding="utf-8") as file:
    json.dump(student, file, indent=4)   # indent=4 makes it human-readable
```

### JSON vs CSV — When to Use Which

| Situation | Use |
|-----------|-----|
| Simple table / spreadsheet data | CSV |
| Nested or hierarchical data | JSON |
| Data for a web API | JSON |
| Data for Excel / reports | CSV |

---

## Part 7 — Practical Exercises

### Exercise 1 — Class List Manager (Beginner)
Write a program that:
1. Asks the user to input student names one at a time (stop when they type `"done"`)
2. Saves all names to `classlist.txt`, one name per line
3. Reads the file back and prints the total number of students

---

### Exercise 2 — Grade File Reader (Intermediate)
Given a `grades.csv` file with columns `student_id`, `name`, `Math`, `English`, `Programming`:
1. Read the file using `csv.DictReader`
2. Compute each student's average grade
3. Print a summary showing name and average
4. Write a new file `summary.csv` with columns `name` and `average`

**Bonus:** Flag students with an average below 75 with a `"NEEDS IMPROVEMENT"` tag.

---

### Exercise 3 — Student Profile JSON (Intermediate–Advanced)
1. Create a Python dictionary for a student with fields: `name`, `id`, `section`, and a nested `grades` dictionary (at least 3 subjects)
2. Write the data to `profile.json`
3. Read it back, compute the average grade, and print a formatted report
4. Add error handling for the case where the file doesn't exist yet

---

### Exercise 4 — Combined: CSV to JSON Converter (Advanced)
Write a program that:
1. Reads `students.csv` (with columns: `student_id`, `name`, `grade`)
2. Converts the data into a list of dictionaries
3. Saves the result as `students.json`
4. Wraps all file operations in proper `try-except` blocks

---

## Quick Reference Cheat Sheet

```python
# Open & read
with open("file.txt", "r", encoding="utf-8") as f:
    content = f.read()

# Write (overwrites)
with open("file.txt", "w", encoding="utf-8") as f:
    f.write("text\n")

# Append
with open("file.txt", "a", encoding="utf-8") as f:
    f.write("more text\n")

# CSV read
import csv
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)

# CSV write
with open("data.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["col1", "col2"])
    writer.writeheader()
    writer.writerows(list_of_dicts)

# JSON read
import json
with open("data.json", "r", encoding="utf-8") as f:
    data = json.load(f)

# JSON write
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=4)

# Safe file operation
try:
    with open("file.txt", "r", encoding="utf-8") as f:
        content = f.read()
except FileNotFoundError:
    print("File not found!")
```

---

*St. Paul University Philippines — College of Information Technology Education*
*Programming 2 | Python File Manipulation*
