# # Linux Bash Scripting — Complete Fundamentals Reviewer**

*A Midterm-Level Book Chapter for Undergraduate Students*

---

# ## **Chapter 1 — The Linux Shell and Bash**

### **1.1 What is a Shell?**

A **shell** is a command-line interpreter that allows users to interact with the operating system. It receives commands, interprets them, and sends them to the OS to be executed.

There are many types of shells:

* `bash` – Bourne Again Shell (default on most Linux systems)
* `zsh` – Modern, feature-rich shell
* `sh` – Original Bourne shell
* `fish` – User-friendly, interactive shell

> **Why Bash?**
> It is widely available, stable, easy to learn, and fully capable of writing automation scripts.

---

# ## **Chapter 2 — Introduction to Bash Scripts**

### **2.1 What is a Bash Script?**

A Bash script is a **text file** that contains a sequence of commands executed by the Bash interpreter.

### **2.2 The Shebang**

Every script begins with:

```bash
#!/bin/bash
```

This tells Linux *which interpreter* should process the file.

### **2.3 Basic Script Structure**

```bash
#!/bin/bash
# This is a comment explaining what the script does
echo "This is my first script."
```

### **2.4 Running a Script**

Two steps:

1. Make executable

   ```bash
   chmod +x script.sh
   ```
2. Execute

   ```bash
   ./script.sh
   ```

> **Note:** Running `bash script.sh` works, but does NOT use the permissions. Midterm tasks usually require `chmod +x`.

---

# ## **Chapter 3 — Comments and Documentation**

Comments help others (and yourself) understand your script.

```bash
# This script prints today's date
date
```

There are no multi-line comments in Bash, but you may simulate them using `: <<EOF … EOF`.

---

# ## **Chapter 4 — Variables**

### **4.1 Creating Variables**

Variables do not need type declarations.

```bash
username="Serafin"
```

### **4.2 Using Variables**

```bash
echo "Welcome, $username"
```

### **4.3 Rules**

* No spaces around `=`
* Variable names are case-sensitive
* Use quotes when storing strings with spaces

---

# ## **Chapter 5 — Special Variables**

| Variable   | Meaning                     |
| ---------- | --------------------------- |
| `$0`       | Script name                 |
| `$1`, `$2` | Positional arguments        |
| `$@`       | All arguments               |
| `$#`       | Argument count              |
| `$$`       | Process ID                  |
| `$?`       | Exit status of last command |

### **Example**

```bash
echo "First argument: $1"
echo "Total arguments: $#"
```

---

# ## **Chapter 6 — User Input**

### **6.1 Reading User Input**

```bash
read -p "Enter your name: " name
echo "Hello, $name"
```

### **6.2 Silent Input**

Useful for passwords:

```bash
read -sp "Enter password: " pass
```

---

# ## **Chapter 7 — Arithmetic in Bash**

Bash arithmetic uses double parentheses:

```bash
x=$((5 + 7))
```

### **Operators**

| Operator  | Meaning               |
| --------- | --------------------- |
| `+ - * /` | Basic math            |
| `%`       | Remainder             |
| `**`      | Power                 |
| `++ --`   | Increment / Decrement |

### **Comparison Operators**

| Operator | Meaning   |
| -------- | --------- |
| `-eq`    | equal     |
| `-ne`    | not equal |
| `-lt`    | less than |
| `-le`    | ≤         |
| `-gt`    | >         |
| `-ge`    | ≥         |

---

# ## **Chapter 8 — Conditional Statements**

### **8.1 Basic If**

```bash
if [ CONDITION ]; then
    commands
fi
```

### **8.2 If-Else**

```bash
if [ -f "$file" ]; then
    echo "File exists."
else
    echo "File not found."
fi
```

### **8.3 File Test Flags**

| Test | Description      |
| ---- | ---------------- |
| `-f` | File exists      |
| `-d` | Directory exists |
| `-e` | Exists           |
| `-r` | Readable         |
| `-w` | Writable         |
| `-x` | Executable       |

---

# ## **Chapter 9 — Loops**

### **9.1 For Loop (List)**

```bash
for i in 1 2 3; do
  echo $i
done
```

### **9.2 For Loop (Range)**

```bash
for i in {1..5}; do
  echo $i
done
```

### **9.3 While Loop**

```bash
count=1
while [ $count -le 5 ]; do
  echo "Count $count"
  ((count++))
done
```

---

# ## **Chapter 10 — Functions**

A function groups commands into reusable blocks.

```bash
myfunc() {
  echo "Hello $1"
}

myfunc "students"
```

Functions can take arguments just like scripts.

---

# ## **Chapter 11 — Arrays**

### **11.1 Declare**

```bash
fruits=("apple" "banana" "orange")
```

### **11.2 Access**

```bash
echo ${fruits[1]}
```

### **11.3 Length**

```bash
echo ${#fruits[@]}
```

### **11.4 All Elements**

```bash
echo ${fruits[@]}
```

---

# ## **Chapter 12 — Globbing (Wildcards)**

| Pattern   | Meaning           |
| --------- | ----------------- |
| `*`       | Any string        |
| `?`       | Any one character |
| `[abc]`   | a or b or c       |
| `{a,b,c}` | expand to list    |

Example:

```bash
cp *.txt backup/
```

---

# ## **Chapter 13 — Redirection and Pipes**

### **13.1 Redirect Output**

| Operator | Meaning                |
| -------- | ---------------------- |
| `>`      | Overwrite output       |
| `>>`     | Append output          |
| `2>`     | Redirect errors        |
| `2>&1`   | Combine error + output |

### **13.2 Pipes**

```bash
ls -l /etc | grep conf
```

Pipes send output of one command to the next.

---

# ## **Chapter 14 — Operators in Bash**

### **Logical Operators**

| Operator | Meaning |   |    |
| -------- | ------- | - | -- |
| `&&`     | AND     |   |    |
| `        |         | ` | OR |
| `!`      | NOT     |   |    |

### **Example**

```bash
mkdir test && echo "Directory created."
```

---

# ## **Chapter 15 — Exit Codes**

Exit codes tell whether a command succeeded.

* `0` = success
* `1–255` = error

Check last exit:

```bash
echo $?
```

Exit your own:

```bash
exit 1
```

---

# ## **Chapter 16 — File Permissions & Execution**

### **16.1 Octal Permissions**

| Octal | Meaning |
| ----- | ------- |
| 4     | Read    |
| 2     | Write   |
| 1     | Execute |

### **16.2 Common Modes**

| Mode  | Description                     |
| ----- | ------------------------------- |
| `644` | Owner read/write; others read   |
| `755` | Owner full; others read/execute |
| `700` | Only owner has access           |

---

# ## **Chapter 17 — Logging in Scripts**

```bash
echo "Task run at $(date)" >> script.log
```

Logging is essential for automation.

---

# ## **Chapter 18 — Cron: Linux Task Scheduler**

### **18.1 What Is Cron?**

A service that runs commands on a **time-based schedule**.

### **18.2 Cron Format**

```
* * * * * command
│ │ │ │ │
│ │ │ │ └─ weekday (0–7)
│ │ │ └─── month
│ │ └───── day
│ └─────── hour
└───────── minute
```

### **18.3 Examples**

| Task          | Cron Entry              |
| ------------- | ----------------------- |
| Every 5 mins  | `*/5 * * * * script.sh` |
| Every day 8am | `0 8 * * * script.sh`   |
| Every Sunday  | `0 9 * * 0 script.sh`   |

### **18.4 Cron Commands**

| Command      | Use               |
| ------------ | ----------------- |
| `crontab -e` | Edit cron jobs    |
| `crontab -l` | View current cron |
| `crontab -r` | Remove cron jobs  |

### **18.5 Cron Logging**

```bash
command >> output.log 2>&1
```

---

# ## **Chapter 19 — Debugging Bash Scripts**

### **19.1 Using Trace Mode**

```bash
bash -x script.sh
```

### **19.2 Inside Script**

```bash
set -x   # Start debug
set +x   # Stop debug
```

### **19.3 Static Analysis**

If installed:

```bash
shellcheck script.sh
```

---

# ## **Chapter 20 — Common Errors & Fixes**

| Problem             | Cause                    | Fix                  |
| ------------------- | ------------------------ | -------------------- |
| “Permission denied” | Script not executable    | `chmod +x`           |
| “command not found” | Typo or missing path     | Recheck spelling     |
| Cron not running    | Wrong absolute path      | Use `/home/user/...` |
| “bad substitution”  | Using sh instead of bash | Use `#!/bin/bash`    |


