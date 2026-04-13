<img src="https://drive.google.com/uc?id=1uPUwnTqN6FmhirxTGO-nPf-G4lSF3dn5" alt="Banner" width="1500">

# Chapter 6 — Lambda Functions & List Comprehensions
### Lecture Notes with Problem Sets
**Programming 2 · Python · St. Paul University Philippines**

> These are two of Python's most expressive features. You have already seen both in Chapter 5 — `min(roster, key=lambda s: s["gwa"])` and `[s for s in roster if s["gwa"] <= 1.75]`. This chapter explains how they work, why they exist, and how to use them confidently.

---

## Table of Contents

- [6.1 The problem they solve](#61-the-problem-they-solve)
- [6.2 List comprehensions — basic form](#62-list-comprehensions--basic-form)
- [6.3 List comprehensions — with a condition](#63-list-comprehensions--with-a-condition)
- [6.4 Nested list comprehensions](#64-nested-list-comprehensions)
- [6.5 Lambda functions — syntax and usage](#65-lambda-functions--syntax-and-usage)
- [6.6 Lambda with sorted(), min(), max()](#66-lambda-with-sorted-min-max)
- [6.7 Lambda with filter() and map()](#67-lambda-with-filter-and-map)
- [6.8 Combining lambda and list comprehensions](#68-combining-lambda-and-list-comprehensions)
- [6.9 When NOT to use them](#69-when-not-to-use-them)
- [Problem Set A — List Comprehensions](#problem-set-a--list-comprehensions)
- [Problem Set B — Lambda Functions](#problem-set-b--lambda-functions)
- [Problem Set C — Combined Challenges](#problem-set-c--combined-challenges)

---

## 6.1 The problem they solve

Consider this classic pattern you have written many times already:

```python
# Build a list of passing grades
grades = [88, 55, 72, 91, 60, 85, 43, 78]

passing = []
for g in grades:
    if g >= 75:
        passing.append(g)

print(passing)  # [88, 91, 85, 78]
```

This works perfectly. But it takes four lines to express one simple idea: *"give me every grade that is at least 75."* Python provides a shorter, more readable way to write this same logic — and that is what this chapter teaches.

Similarly, consider this pattern for sorting:

```python
roster = [
    {"name": "Juan",  "gwa": 2.25},
    {"name": "Maria", "gwa": 1.50},
    {"name": "Ana",   "gwa": 1.75},
]

# Sort by GWA — but sorted() doesn't know which field to use
roster.sort(???)
```

`sorted()` works fine for simple lists of numbers or strings. But for a list of dictionaries, you need to tell it *which field* to compare. That is exactly what a lambda function does.

---

## 6.2 List comprehensions — basic form

A **list comprehension** builds a new list from an existing iterable in a single expression. The basic syntax is:

```
[expression  for  item  in  iterable]
```

Read it left to right: *"give me `expression`, for each `item` in `iterable`."*

### Transformation — changing every element

```python
# Regular loop version
grades = [88, 75, 92, 60, 81]
scaled = []
for g in grades:
    scaled.append(round(g * 1.05, 2))  # 5% bonus

# List comprehension version — same result, one line
scaled = [round(g * 1.05, 2) for g in grades]

print(scaled)  # [92.4, 78.75, 96.6, 63.0, 85.05]
```

The expression `round(g * 1.05, 2)` is applied to every element. The comprehension collects the results into a new list automatically.

### Extraction — pulling a field from each item

```python
roster = [
    {"name": "Maria", "gwa": 1.50},
    {"name": "Juan",  "gwa": 2.25},
    {"name": "Ana",   "gwa": 1.75},
]

# Extract just the names
names = [s["name"] for s in roster]
print(names)  # ['Maria', 'Juan', 'Ana']

# Extract just the GWAs
gwas = [s["gwa"] for s in roster]
print(gwas)   # [1.5, 2.25, 1.75]
```

This pattern — extracting one field from a list of dictionaries — is one of the most common uses of list comprehensions in data work.

### Type conversion — applying a function to every element

```python
raw = ["88", "75", "92", "60"]   # strings from user input

# Convert all to integers
grades = [int(x) for x in raw]
print(grades)   # [88, 75, 92, 60]
print(type(grades[0]))  # <class 'int'>
```

> 💡 **Mental model:** A list comprehension is a loop and an `.append()` compressed into one readable line. If you can write the loop version, you can always write the comprehension version by moving the append expression to the front.

---

## 6.3 List comprehensions — with a condition

Adding an `if` clause at the end **filters** which elements are included. Elements that do not satisfy the condition are simply skipped.

```
[expression  for  item  in  iterable  if  condition]
```

### Filtering — keeping only matching elements

```python
grades = [88, 55, 72, 91, 60, 85, 43, 78]

# Keep only passing grades (>= 75)
passing = [g for g in grades if g >= 75]
print(passing)  # [88, 91, 85, 78]

# Keep only failing grades
failing = [g for g in grades if g < 75]
print(failing)  # [55, 72, 60, 43]
```

### Filtering + transforming at the same time

The `if` clause filters which items are included; the expression transforms each included item. Both happen in one line.

```python
roster = [
    {"name": "Maria", "gwa": 1.50, "year": 2},
    {"name": "Juan",  "gwa": 2.25, "year": 3},
    {"name": "Ana",   "gwa": 1.75, "year": 2},
    {"name": "Carlo", "gwa": 3.10, "year": 3},
]

# Names of year-2 students only
year2_names = [s["name"] for s in roster if s["year"] == 2]
print(year2_names)  # ['Maria', 'Ana']

# GWAs of honor students (GWA <= 1.75), multiplied for display
honor_gwas = [s["gwa"] for s in roster if s["gwa"] <= 1.75]
print(honor_gwas)   # [1.5, 1.75]
```

### if-else inside the expression

You can also place an `if-else` **inside the expression** (not at the end) to transform every element differently based on a condition. Note the position — this is not filtering, it is branching:

```python
grades = [88, 55, 72, 91, 60, 85]

# Label each grade — transform, not filter
remarks = ["Passed" if g >= 75 else "Failed" for g in grades]
print(remarks)  # ['Passed', 'Failed', 'Failed', 'Passed', 'Failed', 'Passed']
```

> ⚠️ **Position matters:**
> - `[expr for x in lst if cond]` → **filters** (some elements excluded)
> - `[expr_if_true if cond else expr_if_false for x in lst]` → **transforms all** (no elements excluded)

---

## 6.4 Nested list comprehensions

A nested list comprehension contains a loop inside a loop — useful for working with 2D data or generating grids.

### Flattening a 2D list

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

# Flatten into a single list — loop over rows, then over each item in the row
flat = [num for row in matrix for num in row]
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Read the nested comprehension left to right, matching how you would write the equivalent nested loop:

```python
# Equivalent nested loop — same result
flat = []
for row in matrix:
    for num in row:
        flat.append(num)
```

### Generating a multiplication table

```python
# 3x3 multiplication table as a 2D list
table = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print(table)
# [[1, 2, 3],
#  [2, 4, 6],
#  [3, 6, 9]]
```

> ⚠️ **Readability limit:** Nested comprehensions become hard to read quickly. If a nested comprehension takes more than a moment to parse, rewrite it as a regular nested loop. Clarity is always worth more than brevity.

---

## 6.5 Lambda functions — syntax and usage

A **lambda function** is a small, anonymous function defined in a single expression. The word *anonymous* means it has no name — unlike functions defined with `def`.

### Syntax

```
lambda  parameters  :  expression
```

A lambda takes any number of parameters and returns the result of exactly one expression. It cannot contain statements, loops, or multiple lines.

### Comparing def and lambda

```python
# Regular named function
def square(x):
    return x ** 2

# Equivalent lambda
square = lambda x: x ** 2

print(square(5))  # 25
```

Both are equivalent. In practice, you would rarely assign a lambda to a variable like this — if you need a named function, use `def`. Lambdas are most useful when passed directly as an argument to another function.

### Multiple parameters

```python
# Add two numbers
add = lambda a, b: a + b
print(add(3, 7))  # 10

# Compute weighted average of two grades
weighted = lambda g1, g2, w1, w2: (g1 * w1 + g2 * w2) / (w1 + w2)
print(weighted(90, 75, 0.6, 0.4))  # 84.0
```

### Lambda as an inline argument

This is the most common real-world usage — passing a lambda directly where a function is expected:

```python
grades = [88, 55, 72, 91, 60]

# Pass a lambda directly to max() as the key
highest = max(grades, key=lambda g: g)
print(highest)  # 91

# More useful example — find the grade closest to 80
closest = min(grades, key=lambda g: abs(g - 80))
print(closest)  # 78  (closest to 80 in absolute difference)
```

> 💡 **Mental model:** A lambda is a function you use once and throw away. Think of it as a sticky note with a formula written on it — you hand it to another function, it uses the formula, and the sticky note is discarded.

---

## 6.6 Lambda with sorted(), min(), max()

The `key` parameter in `sorted()`, `min()`, and `max()` accepts a function that tells Python *what to compare*. A lambda is the perfect tool here because the comparison logic is usually short enough to fit on one line.

### Sorting a list of dictionaries

```python
roster = [
    {"name": "Maria", "gwa": 1.50},
    {"name": "Juan",  "gwa": 2.25},
    {"name": "Ana",   "gwa": 1.75},
    {"name": "Carlo", "gwa": 3.00},
]

# Sort by GWA ascending (best first in PH system = lowest GWA)
by_gwa = sorted(roster, key=lambda s: s["gwa"])
for s in by_gwa:
    print(s["name"], s["gwa"])
# Maria 1.5
# Ana   1.75
# Juan  2.25
# Carlo 3.0

# Sort by name alphabetically
by_name = sorted(roster, key=lambda s: s["name"])
```

### Sorting by multiple fields

```python
students = [
    {"name": "Maria", "year": 2, "gwa": 1.50},
    {"name": "Juan",  "year": 3, "gwa": 2.25},
    {"name": "Ana",   "year": 2, "gwa": 1.75},
    {"name": "Rosa",  "year": 3, "gwa": 1.90},
]

# Sort by year first, then by GWA within the same year
# Return a tuple from the lambda — Python sorts tuples element by element
by_year_then_gwa = sorted(students, key=lambda s: (s["year"], s["gwa"]))
for s in by_year_then_gwa:
    print(s["name"], s["year"], s["gwa"])
# Maria  2  1.5
# Ana    2  1.75
# Rosa   3  1.9
# Juan   3  2.25
```

### min() and max() with a key

```python
roster = [
    {"name": "Maria", "gwa": 1.50},
    {"name": "Juan",  "gwa": 2.25},
    {"name": "Ana",   "gwa": 1.75},
]

top    = min(roster, key=lambda s: s["gwa"])   # lowest GWA = best student
bottom = max(roster, key=lambda s: s["gwa"])   # highest GWA = needs help

print(f"Top student:    {top['name']} ({top['gwa']})")
print(f"Needs support:  {bottom['name']} ({bottom['gwa']})")
```

---

## 6.7 Lambda with filter() and map()

Python has two built-in functions that accept a function and an iterable: `filter()` and `map()`. Lambdas pair naturally with both.

### filter()

`filter(function, iterable)` returns only the elements for which the function returns `True`. It returns a **filter object** (a lazy iterator) — wrap it with `list()` to get a regular list.

```python
grades = [88, 55, 72, 91, 60, 85, 43, 78]

# Keep only passing grades
passing = list(filter(lambda g: g >= 75, grades))
print(passing)  # [88, 91, 85, 78]

# Same result as a list comprehension
passing = [g for g in grades if g >= 75]
```

With a list of dictionaries:

```python
roster = [
    {"name": "Maria", "gwa": 1.50},
    {"name": "Juan",  "gwa": 2.25},
    {"name": "Ana",   "gwa": 1.75},
    {"name": "Carlo", "gwa": 3.10},
]

# Honor students only
honor = list(filter(lambda s: s["gwa"] <= 1.75, roster))
print([s["name"] for s in honor])  # ['Maria', 'Ana']
```

### map()

`map(function, iterable)` applies the function to **every element** and returns a map object. Wrap with `list()` to materialise it.

```python
grades = [88, 75, 92, 60, 81]

# Apply 5% bonus to all grades
scaled = list(map(lambda g: round(g * 1.05, 2), grades))
print(scaled)  # [92.4, 78.75, 96.6, 63.0, 85.05]

# Convert all strings to integers
raw = ["88", "75", "92"]
nums = list(map(int, raw))   # int is itself a function — no lambda needed
print(nums)  # [88, 75, 92]
```

### filter() vs map() vs list comprehension — when to use which

| Goal | filter() | map() | List comprehension |
|---|---|---|---|
| Keep elements matching a condition | ✓ | | ✓ |
| Transform every element | | ✓ | ✓ |
| Filter AND transform in one step | | | ✓ best |
| Pass as argument to another function | ✓ | ✓ | |

> 💡 **In modern Python, list comprehensions are generally preferred** over `filter()` and `map()` for readability. `filter()` and `map()` are still useful when passing a function to a higher-order function, or when working with very large datasets where lazy evaluation matters.

---

## 6.8 Combining lambda and list comprehensions

Lambda and list comprehensions solve overlapping problems — both process lists — but they are not interchangeable. Knowing when to combine them is a sign of Python fluency.

### Pattern 1 — Comprehension to extract, lambda to sort

```python
roster = [
    {"name": "Maria", "gwa": 1.50, "year": 2},
    {"name": "Juan",  "gwa": 2.25, "year": 3},
    {"name": "Ana",   "gwa": 1.75, "year": 2},
    {"name": "Carlo", "gwa": 3.00, "year": 3},
]

# Step 1: comprehension to filter year-2 students
year2 = [s for s in roster if s["year"] == 2]

# Step 2: lambda to sort the filtered list by GWA
year2_sorted = sorted(year2, key=lambda s: s["gwa"])

for s in year2_sorted:
    print(s["name"], s["gwa"])
# Maria  1.5
# Ana    1.75
```

### Pattern 2 — Comprehension to build summary data

```python
inventory = [
    {"item": "Noodles",  "price": 8.50,  "stock": 24},
    {"item": "Sardines", "price": 15.00, "stock": 12},
    {"item": "Rice",     "price": 52.00, "stock": 5},
    {"item": "Soda",     "price": 17.00, "stock": 30},
]

# Total value of each item (price × stock)
values = [i["price"] * i["stock"] for i in inventory]
print(values)   # [204.0, 180.0, 260.0, 510.0]

# Total inventory value
total = sum(values)
print(f"Total inventory value: ₱{total:.2f}")  # ₱1154.00

# Most valuable item
richest = max(inventory, key=lambda i: i["price"] * i["stock"])
print(f"Most valuable: {richest['item']}")  # Soda
```

### Pattern 3 — map() with lambda to build a new list of dicts

```python
students = [
    {"name": "Maria", "grade": 88},
    {"name": "Juan",  "grade": 55},
    {"name": "Ana",   "grade": 72},
]

# Add a "remarks" field to each student — map produces a new list
updated = list(map(
    lambda s: {**s, "remarks": "Passed" if s["grade"] >= 75 else "Failed"},
    students
))

for s in updated:
    print(s)
# {'name': 'Maria', 'grade': 88, 'remarks': 'Passed'}
# {'name': 'Juan',  'grade': 55, 'remarks': 'Failed'}
# {'name': 'Ana',   'grade': 72, 'remarks': 'Failed'}
```

> 💡 `{**s, "remarks": ...}` is called **dictionary unpacking** — it creates a new dictionary with all the keys from `s` plus the new `"remarks"` key. This leaves the original `s` unchanged, which is the safer approach when you want to build a new dataset rather than mutate the old one.

---

## 6.9 When NOT to use them

Both features can be overused. Here are the cases where a regular `def` function or a regular `for` loop is the better choice.

### When the logic is complex

```python
# Bad — too much logic crammed into one lambda
process = lambda s: "Summa" if s["gwa"] <= 1.20 else "Magna" if s["gwa"] <= 1.45 else "Cum Laude" if s["gwa"] <= 1.75 else "Good Standing"

# Good — readable named function
def get_honors(s):
    if s["gwa"] <= 1.20: return "Summa Cum Laude"
    if s["gwa"] <= 1.45: return "Magna Cum Laude"
    if s["gwa"] <= 1.75: return "Cum Laude"
    return "Good Standing"
```

### When you need to reuse the function

```python
# Bad — duplicating the same lambda in multiple places
honor1 = list(filter(lambda s: s["gwa"] <= 1.75, section_a))
honor2 = list(filter(lambda s: s["gwa"] <= 1.75, section_b))

# Good — define it once with def
def is_honor(s):
    return s["gwa"] <= 1.75

honor1 = list(filter(is_honor, section_a))
honor2 = list(filter(is_honor, section_b))
```

### When the comprehension has more than two conditions

```python
# Hard to read — stop and use a loop
result = [s["name"] for s in roster if s["year"] == 2 if s["gwa"] <= 1.75 if s["scholarship"] == True]

# Easier to read
result = []
for s in roster:
    if s["year"] == 2 and s["gwa"] <= 1.75 and s["scholarship"]:
        result.append(s["name"])
```

> **Rule of thumb:** If a list comprehension or lambda needs a comment to be understood, rewrite it as a regular loop or `def` function. Code is read far more often than it is written.

---

# Problem Sets

> **Instructions:** For each problem, write your answer in the code cell below the problem. Do not use regular `for` loops unless the problem explicitly allows it — use list comprehensions, `lambda`, `filter()`, `map()`, or `sorted()` as directed.

---

## Problem Set A · List Comprehensions

---

### A1 — Basic transformation

Given the list of jeepney fares in pesos, create a new list with all fares increased by the LTFRB surcharge of ₱1.50.

```python
fares = [13.0, 15.0, 17.5, 20.0, 13.0, 22.5]

# Your answer — use a list comprehension
surcharge_fares = ___

print(surcharge_fares)
# Expected: [14.5, 16.5, 19.0, 21.5, 14.5, 24.0]
```

---

### A2 — Filtering

Given the list of student names below, use a list comprehension to keep only names that have more than 5 characters.

```python
names = ["Ana", "Maria", "Juan", "Rosario", "Ed", "Carmela", "Ali"]

# Your answer
long_names = ___

print(long_names)
# Expected: ['Maria', 'Rosario', 'Carmela']
```

---

### A3 — Extraction from a list of dicts

Given the sari-sari store inventory below, use a list comprehension to extract only the item names.

```python
inventory = [
    {"item": "Noodles",  "price": 8.50,  "stock": 24},
    {"item": "Sardines", "price": 15.00, "stock": 12},
    {"item": "Rice",     "price": 52.00, "stock": 5},
    {"item": "Soda",     "price": 17.00, "stock": 30},
]

# Your answer
item_names = ___

print(item_names)
# Expected: ['Noodles', 'Sardines', 'Rice', 'Soda']
```

---

### A4 — Filter + transform together

From the same inventory, use a **single** list comprehension to get the **names** of items whose stock is below 10.

```python
# Your answer — filter on stock, extract name, in one comprehension
low_stock_names = ___

print(low_stock_names)
# Expected: ['Rice']
```

---

### A5 — if-else expression (label all elements)

Given the list of exam scores, use a list comprehension with an `if-else` **inside the expression** (not at the end) to label each score as `"Passed"` if ≥ 75, otherwise `"Failed"`. The result should be a list of strings.

```python
scores = [88, 55, 72, 91, 60, 85, 43, 78]

# Your answer
labels = ___

print(labels)
# Expected: ['Passed', 'Failed', 'Failed', 'Passed', 'Failed', 'Passed', 'Failed', 'Passed']
```

---

### A6 — Nested list comprehension

Given the 2D grade matrix below where each inner list is one student's grades across three subjects, use a **nested list comprehension** to flatten it into a single list.

```python
grade_matrix = [
    [88, 90, 85],
    [75, 80, 72],
    [92, 88, 95],
]

# Your answer
flat_grades = ___

print(flat_grades)
# Expected: [88, 90, 85, 75, 80, 72, 92, 88, 95]
```

---

### A7 — Comprehension with string methods

Given the list of raw subject names entered by a student (with inconsistent casing and extra spaces), use a list comprehension to clean them — strip whitespace and convert to title case.

```python
raw_subjects = ["  math ", "PHYSICS", " english ", "pe", "  Programming  "]

# Your answer
clean = ___

print(clean)
# Expected: ['Math', 'Physics', 'English', 'Pe', 'Programming']
```

---

## Problem Set B · Lambda Functions

---

### B1 — Basic lambda

Write a lambda function called `to_peso` that takes a number and returns it formatted as a peso string — e.g., `88` → `"₱88.00"`. Test it on the number `1250.5`.

```python
# Your answer
to_peso = ___

print(to_peso(1250.5))
# Expected: ₱1250.50
```

---

### B2 — Lambda with sorted()

Sort the roster below by GWA in ascending order (best first) using `sorted()` with a lambda key.

```python
roster = [
    {"name": "Carlo",  "gwa": 3.00},
    {"name": "Maria",  "gwa": 1.50},
    {"name": "Juan",   "gwa": 2.25},
    {"name": "Ana",    "gwa": 1.75},
    {"name": "Rosa",   "gwa": 1.25},
]

# Your answer
sorted_roster = ___

for s in sorted_roster:
    print(s["name"], s["gwa"])
# Expected:
# Rosa  1.25
# Maria 1.50
# Ana   1.75
# Juan  2.25
# Carlo 3.00
```

---

### B3 — Lambda with sorted(), multiple fields

Sort the students below **first by year level ascending**, then **by name alphabetically** within the same year. Use a lambda that returns a tuple.

```python
students = [
    {"name": "Maria", "year": 2, "gwa": 1.50},
    {"name": "Carlo", "year": 3, "gwa": 3.00},
    {"name": "Ana",   "year": 2, "gwa": 1.75},
    {"name": "Rosa",  "year": 1, "gwa": 1.25},
    {"name": "Juan",  "year": 3, "gwa": 2.25},
]

# Your answer
sorted_students = ___

for s in sorted_students:
    print(s["year"], s["name"])
# Expected:
# 1 Rosa
# 2 Ana
# 2 Maria
# 3 Carlo
# 3 Juan
```

---

### B4 — Lambda with min() and max()

Using the inventory below, find the cheapest item and the most expensive item using `min()` and `max()` with a lambda key.

```python
inventory = [
    {"item": "Noodles",  "price": 8.50},
    {"item": "Sardines", "price": 15.00},
    {"item": "Rice",     "price": 52.00},
    {"item": "Soda",     "price": 17.00},
]

# Your answer
cheapest  = ___
expensive = ___

print(f"Cheapest:  {cheapest['item']} at ₱{cheapest['price']:.2f}")
print(f"Priciest:  {expensive['item']} at ₱{expensive['price']:.2f}")
# Expected:
# Cheapest:  Noodles at ₱8.50
# Priciest:  Rice at ₱52.00
```

---

### B5 — Lambda with filter()

Use `filter()` with a lambda to keep only the voters aged 60 and above from the registry below. Return a list.

```python
voters = [
    {"name": "Lola Rosa",  "age": 72},
    {"name": "Juan",       "age": 34},
    {"name": "Maria",      "age": 61},
    {"name": "Carlo",      "age": 28},
    {"name": "Lolo Bert",  "age": 80},
]

# Your answer
senior_voters = ___

for v in senior_voters:
    print(v["name"], v["age"])
# Expected:
# Lola Rosa 72
# Maria 61
# Lolo Bert 80
```

---

### B6 — Lambda with map()

Use `map()` with a lambda to add a `"discount_price"` field to every item in the store list below. The discount price is 90% of the original price (10% off). Return a list.

```python
items = [
    {"item": "Shampoo",  "price": 12.00},
    {"item": "Soap",     "price": 8.00},
    {"item": "Toothpaste","price": 35.00},
]

# Your answer — use {**i, "discount_price": ...} to add the new field
discounted = ___

for d in discounted:
    print(d["item"], d["price"], "→", d["discount_price"])
# Expected:
# Shampoo    12.0  → 10.8
# Soap        8.0  → 7.2
# Toothpaste 35.0  → 31.5
```

---

## Problem Set C · Combined Challenges

---

### C1 — Filter then sort

From the class roster below, use a **list comprehension** to get only the students in year 2, then use `sorted()` with a **lambda** to sort them by GWA ascending.

```python
roster = [
    {"name": "Maria",  "year": 2, "gwa": 1.50},
    {"name": "Juan",   "year": 3, "gwa": 2.25},
    {"name": "Ana",    "year": 2, "gwa": 1.75},
    {"name": "Carlo",  "year": 3, "gwa": 3.00},
    {"name": "Rosa",   "year": 2, "gwa": 2.00},
]

# Step 1: filter year-2 students using a list comprehension
year2 = ___

# Step 2: sort by GWA using sorted() + lambda
year2_sorted = ___

for s in year2_sorted:
    print(s["name"], s["gwa"])
# Expected:
# Maria 1.5
# Ana   1.75
# Rosa  2.0
```

---

### C2 — Compute total value, find maximum

Given the barangay budget allocations below:
1. Use a **list comprehension** to build a list of total values (`allocated * utilization_rate`)
2. Use `sum()` on that list to get the total spent
3. Use `max()` with a **lambda** to find the program with the highest spending

```python
budget = [
    {"program": "Livelihood",   "allocated": 50000, "utilization_rate": 0.85},
    {"program": "Health",       "allocated": 80000, "utilization_rate": 0.92},
    {"program": "Education",    "allocated": 60000, "utilization_rate": 0.78},
    {"program": "Infra",        "allocated": 120000,"utilization_rate": 0.65},
]

# Step 1: list comprehension for total spending per program
spent = ___

# Step 2: total spent across all programs
total_spent = ___
print(f"Total spent: ₱{total_spent:,.2f}")

# Step 3: program with highest spending using max() + lambda
top_program = ___
print(f"Highest spender: {top_program['program']}")

# Expected:
# Total spent: ₱257,600.00
# Highest spender: Infra
```

---

### C3 — Map to add a computed field, then filter

Given the student list below:
1. Use `map()` with a **lambda** to add an `"average"` key to each student (average of their three grades)
2. Use a **list comprehension** to get only students whose average is ≥ 75
3. Print their names and averages

```python
students = [
    {"name": "Maria", "grades": [88, 90, 85]},
    {"name": "Juan",  "grades": [55, 60, 58]},
    {"name": "Ana",   "grades": [75, 80, 72]},
    {"name": "Carlo", "grades": [92, 88, 95]},
    {"name": "Rosa",  "grades": [65, 70, 68]},
]

# Step 1: map() to add average field
with_avg = ___

# Step 2: list comprehension to keep only passing students (avg >= 75)
passing = ___

# Step 3: print results
for s in passing:
    print(f"{s['name']}: {s['average']:.2f}")

# Expected:
# Maria: 87.67
# Ana:   75.67
# Carlo: 91.67
```

---

### C4 — Sort by computed value

Sort the inventory below by **total value** (price × stock) in descending order using `sorted()` with a lambda that computes the value on the fly. Then print a ranked list.

```python
inventory = [
    {"item": "Noodles",  "price": 8.50,  "stock": 24},
    {"item": "Sardines", "price": 15.00, "stock": 12},
    {"item": "Rice",     "price": 52.00, "stock": 5},
    {"item": "Soda",     "price": 17.00, "stock": 30},
    {"item": "Shampoo",  "price": 12.00, "stock": 8},
]

# Your answer — sort descending by price * stock
ranked = ___

print("Rank | Item      | Total Value")
print("-" * 35)
for i, item in enumerate(ranked, start=1):
    total = item["price"] * item["stock"]
    print(f"  {i}  | {item['item']:<10}| ₱{total:.2f}")

# Expected:
# Rank | Item      | Total Value
# -----------------------------------
#   1  | Soda      | ₱510.00
#   2  | Rice      | ₱260.00
#   3  | Noodles   | ₱204.00
#   4  | Sardines  | ₱180.00
#   5  | Shampoo   | ₱96.00
```

---

### C5 — Capstone: full pipeline

The barangay health center has a list of patients. Using only list comprehensions, lambdas, `filter()`, `map()`, `sorted()`, `min()`, and `max()` — build a complete report without using any `for` loop in your solution code.

```python
patients = [
    {"name": "Lola Rosa",   "age": 72, "bp": 140, "is_diabetic": True},
    {"name": "Juan Reyes",  "age": 34, "bp": 120, "is_diabetic": False},
    {"name": "Maria Cruz",  "age": 61, "bp": 160, "is_diabetic": True},
    {"name": "Carlo Bato",  "age": 28, "bp": 115, "is_diabetic": False},
    {"name": "Ana Santos",  "age": 55, "bp": 135, "is_diabetic": False},
    {"name": "Lolo Bert",   "age": 80, "bp": 150, "is_diabetic": True},
]

# Task 1: Names of patients aged 60 and above
# (use list comprehension or filter + lambda)
seniors = ___
print("Seniors:", seniors)

# Task 2: Names of diabetic patients, sorted alphabetically
diabetics_sorted = ___
print("Diabetics:", diabetics_sorted)

# Task 3: Patient with highest blood pressure
most_at_risk = ___
print(f"Highest BP: {most_at_risk['name']} ({most_at_risk['bp']} mmHg)")

# Task 4: Average blood pressure across all patients
avg_bp = ___
print(f"Average BP: {avg_bp:.1f} mmHg")

# Task 5: Patients with normal BP (below 130), sorted by age
normal_bp_sorted = ___
print("Normal BP patients (by age):", [p["name"] for p in normal_bp_sorted])

# Expected:
# Seniors: ['Lola Rosa', 'Maria Cruz', 'Lolo Bert']
# Diabetics: ['Lola Rosa', 'Lolo Bert', 'Maria Cruz']
# Highest BP: Maria Cruz (160 mmHg)
# Average BP: 136.7 mmHg
# Normal BP patients (by age): ['Carlo Bato', 'Juan Reyes', 'Ana Santos']
```

---

*Chapter 6 · Lambda Functions & List Comprehensions · Programming 2 — Python · St. Paul University Philippines*
