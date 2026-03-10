# Bash Cheet Sheet


## Introduction to Bash {#introduction}

### What is Bash?

**Bash** (Bourne Again Shell) is a command-line interface and scripting language for Unix-like operating systems. It is the default shell for most Linux distributions.

### What is a Shell Script?

A shell script is a text file containing a series of commands that are executed sequentially by the shell interpreter.

### Creating Your First Script

```bash
#!/bin/bash
# This is a comment
echo "Hello, World!"
```

**Key Points:**
- The first line `#!/bin/bash` is called a **shebang** - it tells the system which interpreter to use
- Comments start with `#`
- Make scripts executable: `chmod +x script.sh`
- Run scripts: `./script.sh` or `bash script.sh`

---

## Shell Basics {#shell-basics}

### Common Commands

```bash
# File operations
ls          # List files
cd          # Change directory
pwd         # Print working directory
mkdir       # Make directory
rm          # Remove files
cp          # Copy files
mv          # Move/rename files
touch       # Create empty file

# Text processing
cat         # Display file contents
grep        # Search text
sed         # Stream editor
awk         # Pattern scanning
sort        # Sort lines
uniq        # Remove duplicates
wc          # Word count

# System information
whoami      # Current user
hostname    # System name
date        # Current date/time
uptime      # System uptime
df          # Disk usage
free        # Memory usage
ps          # Process status
top         # System monitor
```

### Command Structure

```bash
command [options] [arguments]

# Examples:
ls -la /home/user
grep -r "pattern" /var/log
find . -name "*.txt"
```

---

## Variables {#variables}

### Declaring Variables

```bash
# Variable assignment (no spaces around =)
name="John"
age=25
PI=3.14159

# Using variables
echo "My name is $name"
echo "I am ${age} years old"
```

### Special Variables

```bash
$0      # Script name
$1-$9   # Command line arguments (positional parameters)
$#      # Number of arguments
$@      # All arguments as separate words
$*      # All arguments as single word
$$      # Process ID of current script
$?      # Exit status of last command
$!      # Process ID of last background command
$_      # Last argument of previous command
```

### Variable Types

```bash
# String variables
name="Alice"
greeting="Hello, $name"

# Integer variables
count=10
sum=$((count + 5))

# Read-only variables
readonly PI=3.14159

# Environment variables
export PATH="/usr/local/bin:$PATH"

# Command substitution
current_date=$(date)
files=$(ls -l)
```

### Variable Expansion

```bash
# Basic expansion
echo $var
echo ${var}

# Default values
${var:-default}      # Use default if var is unset
${var:=default}      # Assign default if var is unset
${var:?error}        # Show error if var is unset
${var:+value}        # Use value if var is set

# String length
${#var}

# Substring
${var:offset:length}
```

---

## Input/Output {#input-output}

### Reading User Input

```bash
# Simple read
read name
echo "Hello, $name"

# Read with prompt
read -p "Enter your name: " name
echo "Hello, $name"

# Read password (hidden input)
read -sp "Enter password: " password

# Read multiple variables
read -p "Enter first and last name: " first last

# Read with timeout
read -t 5 -p "Enter within 5 seconds: " input

# Read from file
while read line; do
    echo "$line"
done < file.txt
```

### Output Commands

```bash
# echo - simple output
echo "Hello, World!"
echo -n "No newline"
echo -e "Line1\nLine2"  # Enable escape sequences

# printf - formatted output
printf "Name: %s, Age: %d\n" "John" 25
printf "%.2f\n" 3.14159

# Output to file
echo "text" > file.txt      # Overwrite
echo "text" >> file.txt     # Append
```

---

## Conditional Statements {#conditionals}

### if Statement

```bash
# Basic if
if [ condition ]; then
    # commands
fi

# if-else
if [ condition ]; then
    # commands
else
    # commands
fi

# if-elif-else
if [ condition1 ]; then
    # commands
elif [ condition2 ]; then
    # commands
else
    # commands
fi
```

### Test Conditions

#### Numeric Comparisons

```bash
-eq     # Equal to
-ne     # Not equal to
-gt     # Greater than
-ge     # Greater than or equal to
-lt     # Less than
-le     # Less than or equal to

# Example:
if [ $age -ge 18 ]; then
    echo "Adult"
fi
```

#### String Comparisons

```bash
=       # Equal to
!=      # Not equal to
-z      # String is empty
-n      # String is not empty

# Example:
if [ "$name" = "John" ]; then
    echo "Hello John"
fi

if [ -z "$var" ]; then
    echo "Variable is empty"
fi
```

#### File Tests

```bash
-e file     # File exists
-f file     # Regular file exists
-d file     # Directory exists
-r file     # File is readable
-w file     # File is writable
-x file     # File is executable
-s file     # File size > 0
-L file     # File is symbolic link
file1 -nt file2  # file1 is newer than file2
file1 -ot file2  # file1 is older than file2

# Example:
if [ -f "/etc/passwd" ]; then
    echo "File exists"
fi

if [ -d "$HOME/documents" ]; then
    echo "Directory exists"
fi
```

#### Logical Operators

```bash
!       # NOT
-a      # AND (inside [ ])
-o      # OR (inside [ ])
&&      # AND (between commands)
||      # OR (between commands)

# Examples:
if [ $age -ge 18 ] && [ $age -le 65 ]; then
    echo "Working age"
fi

if [ ! -f file.txt ]; then
    echo "File does not exist"
fi

if [ $score -ge 90 ] || [ $bonus -eq 1 ]; then
    echo "Passed"
fi
```

### case Statement

```bash
case $variable in
    pattern1)
        # commands
        ;;
    pattern2)
        # commands
        ;;
    pattern3|pattern4)
        # commands for multiple patterns
        ;;
    *)
        # default case
        ;;
esac

# Example:
read -p "Enter fruit name: " fruit
case $fruit in
    apple|Apple)
        echo "Red fruit"
        ;;
    banana|Banana)
        echo "Yellow fruit"
        ;;
    orange|Orange)
        echo "Orange fruit"
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```

---

## Loops {#loops}

### for Loop

```bash
# Traditional for loop
for i in 1 2 3 4 5; do
    echo "Number: $i"
done

# Range
for i in {1..10}; do
    echo $i
done

# Step range
for i in {0..100..10}; do
    echo $i
done

# C-style for loop
for ((i=0; i<10; i++)); do
    echo $i
done

# Iterate over files
for file in *.txt; do
    echo "Processing $file"
done

# Iterate over command output
for user in $(cat /etc/passwd | cut -d: -f1); do
    echo "User: $user"
done

# Iterate over array
fruits=("apple" "banana" "orange")
for fruit in "${fruits[@]}"; do
    echo $fruit
done
```

### while Loop

```bash
# Basic while loop
counter=0
while [ $counter -lt 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done

# Read file line by line
while read line; do
    echo "Line: $line"
done < file.txt

# Infinite loop
while true; do
    echo "Running..."
    sleep 1
done

# While with condition
while [ -f /tmp/running ]; do
    echo "Process running..."
    sleep 5
done
```

### until Loop

```bash
# Runs until condition becomes true
counter=0
until [ $counter -eq 5 ]; do
    echo "Counter: $counter"
    ((counter++))
done
```

### Loop Control

```bash
# break - exit loop
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break
    fi
    echo $i
done

# continue - skip to next iteration
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue
    fi
    echo $i
done

# Loop with break on specific condition
while true; do
    read -p "Enter 'quit' to exit: " input
    if [ "$input" = "quit" ]; then
        break
    fi
    echo "You entered: $input"
done
```

---

## Functions {#functions}

### Defining Functions

```bash
# Method 1
function_name() {
    # commands
}

# Method 2
function function_name {
    # commands
}

# Method 3 (with function keyword)
function function_name() {
    # commands
}
```

### Function Examples

```bash
# Simple function
greet() {
    echo "Hello, World!"
}

# Call function
greet

# Function with parameters
greet_user() {
    echo "Hello, $1!"
}

greet_user "John"

# Function with return value
add() {
    local sum=$(($1 + $2))
    echo $sum
}

result=$(add 5 3)
echo "Sum: $result"

# Function with return status
check_file() {
    if [ -f "$1" ]; then
        return 0  # Success
    else
        return 1  # Failure
    fi
}

if check_file "/etc/passwd"; then
    echo "File exists"
fi
```

### Local vs Global Variables

```bash
# Global variable
global_var="I am global"

my_function() {
    # Local variable
    local local_var="I am local"
    
    # Can access global
    echo $global_var
    
    # Modify global
    global_var="Modified"
}

my_function
echo $global_var      # Modified
echo $local_var       # Empty (not accessible)
```

### Function Parameters

```bash
show_params() {
    echo "Function name: $0"
    echo "First parameter: $1"
    echo "Second parameter: $2"
    echo "All parameters: $@"
    echo "Number of parameters: $#"
}

show_params arg1 arg2 arg3
```

---

---

## File Operations {#file-operations}

### Reading Files

```bash
# Read entire file
cat file.txt

# Read with line numbers
cat -n file.txt

# Read line by line
while IFS= read -r line; do
    echo "Line: $line"
done < file.txt

# Read into array
mapfile -t lines < file.txt
# or
readarray -t lines < file.txt
```

### Writing to Files

```bash
# Overwrite file
echo "Hello" > file.txt

# Append to file
echo "World" >> file.txt

# Write multiple lines
cat > file.txt << EOF
Line 1
Line 2
Line 3
EOF

# Write to file from loop
for i in {1..5}; do
    echo "Line $i" >> file.txt
done
```

### File Testing

```bash
# Check if file exists
if [ -e file.txt ]; then
    echo "File exists"
fi

# Check if regular file
if [ -f file.txt ]; then
    echo "Regular file"
fi

# Check if directory
if [ -d /home/user ]; then
    echo "Directory exists"
fi

# Check if readable
if [ -r file.txt ]; then
    echo "File is readable"
fi

# Check if writable
if [ -w file.txt ]; then
    echo "File is writable"
fi

# Check if executable
if [ -x script.sh ]; then
    echo "File is executable"
fi

# Check if empty
if [ -s file.txt ]; then
    echo "File is not empty"
fi
```

### File Manipulation

```bash
# Create file
touch newfile.txt

# Copy file
cp source.txt destination.txt

# Move/rename file
mv oldname.txt newname.txt

# Delete file
rm file.txt

# Create directory
mkdir newdir

# Create nested directories
mkdir -p path/to/nested/dir

# Remove directory
rmdir emptydir
rm -r directory      # Remove with contents

# Find files
find /path -name "*.txt"
find /path -type f -mtime -7  # Modified in last 7 days
find /path -size +10M         # Larger than 10MB

# Change permissions
chmod 755 script.sh
chmod u+x script.sh

# Change ownership
chown user:group file.txt
```

---

## Process Management {#processes}

### Running Processes

```bash
# Run in foreground
./script.sh

# Run in background
./script.sh &

# Get background process ID
echo $!

# List running jobs
jobs

# Bring job to foreground
fg %1

# Send job to background
bg %1

# Kill process
kill PID
kill -9 PID    # Force kill
killall script.sh
```

### Process Information

```bash
# Show processes
ps
ps aux
ps -ef

# Process tree
pstree

# Monitor processes
top
htop

# Find process by name
pgrep process_name
pidof process_name

# Check if process is running
if pgrep -x "process_name" > /dev/null; then
    echo "Process is running"
fi
```

### Process Control in Scripts

```bash
# Wait for background process
./script.sh &
wait $!
echo "Process completed"

# Timeout for command
timeout 10s ./long_running_script.sh

# Run command with nice priority
nice -n 10 ./script.sh

# Trap signals
trap "echo 'Script interrupted'; exit" INT TERM

# Cleanup on exit
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/tempfile
}
trap cleanup EXIT
```

---

## Command Line Arguments {#arguments}

### Accessing Arguments

```bash
#!/bin/bash
# Script name: $0
# Arguments: $1, $2, $3, etc.

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

### Parsing Arguments

```bash
# Simple argument parsing
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

filename=$1
echo "Processing file: $filename"

# Multiple arguments
#!/bin/bash

name=$1
age=$2
city=$3

echo "Name: $name"
echo "Age: $age"
echo "City: $city"
```



---

## Exit Codes and Error Handling {#error-handling}

### Exit Codes

```bash
# Exit with status
exit 0    # Success
exit 1    # General error
exit 2    # Misuse of shell command

# Check exit status of last command
command
if [ $? -eq 0 ]; then
    echo "Success"
else
    echo "Failed"
fi

# Use exit status directly
if command; then
    echo "Success"
else
    echo "Failed"
fi
```

### Error Handling

```bash
# Exit on error
set -e

# Exit on undefined variable
set -u

# Exit on pipe failure
set -o pipefail

# Combine all
set -euo pipefail

# Or in shebang
#!/bin/bash -euo pipefail

# Custom error function
error_exit() {
    echo "Error: $1" >&2
    exit 1
}

# Usage
[ -f file.txt ] || error_exit "File not found"

# Error handling with trap
error_handler() {
    echo "Error occurred in script at line $1"
    exit 1
}

trap 'error_handler $LINENO' ERR
```

### Try-Catch Pattern

```bash
# Simulate try-catch
{
    # Try block
    risky_command
    another_command
} || {
    # Catch block
    echo "An error occurred"
    exit 1
}

# With specific error handling
if ! critical_command; then
    echo "Critical command failed"
    # Cleanup
    rm -f temp_files
    exit 1
fi
```

---

## Redirection and Pipes {#redirection}

### Standard Streams

```bash
# STDIN (0), STDOUT (1), STDERR (2)

# Redirect STDOUT
command > file.txt          # Overwrite
command >> file.txt         # Append

# Redirect STDERR
command 2> error.txt        # Overwrite
command 2>> error.txt       # Append

# Redirect both STDOUT and STDERR
command > output.txt 2>&1
command &> output.txt       # Shorthand

# Discard output
command > /dev/null
command 2> /dev/null
command &> /dev/null

# Redirect STDIN
command < input.txt
```

### Here Documents

```bash
# Basic here document
cat << EOF
Line 1
Line 2
Line 3
EOF

# Write to file
cat > file.txt << EOF
Content line 1
Content line 2
EOF

# With variable expansion
name="John"
cat << EOF
Hello, $name
EOF

# Without variable expansion
cat << 'EOF'
Hello, $name
EOF

# Here string
grep "pattern" <<< "text to search"
```

### Pipes

```bash
# Basic pipe
command1 | command2

# Multiple pipes
cat file.txt | grep "pattern" | sort | uniq

# Pipe to multiple commands (tee)
command | tee file.txt | grep "pattern"

# Pipe STDERR
command 2>&1 | grep "error"

# Named pipes (FIFO)
mkfifo mypipe
command1 > mypipe &
command2 < mypipe
```

### Process Substitution

```bash
# Compare output of two commands
diff <(command1) <(command2)

# Use command output as file
while read line; do
    echo $line
done < <(ls -l)
```

---

## Regular Expressions {#regex}

### Basic Patterns

```bash
# Using grep
grep "pattern" file.txt
grep -i "pattern" file.txt     # Case insensitive
grep -v "pattern" file.txt     # Invert match
grep -r "pattern" /path        # Recursive
grep -n "pattern" file.txt     # Show line numbers
grep -c "pattern" file.txt     # Count matches

# Extended regex with grep
grep -E "pattern" file.txt
egrep "pattern" file.txt
```

### Pattern Matching

```bash
# Match with conditional
[[ $string =~ pattern ]]

if [[ $email =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    echo "Valid email"
fi

# Extract matched groups
if [[ $string =~ ([0-9]+)-([0-9]+) ]]; then
    echo "First number: ${BASH_REMATCH[1]}"
    echo "Second number: ${BASH_REMATCH[2]}"
fi
```

### sed (Stream Editor)

```bash
# Substitute
sed 's/old/new/' file.txt              # First occurrence
sed 's/old/new/g' file.txt             # All occurrences
sed 's/old/new/gi' file.txt            # Case insensitive

# Delete lines
sed '/pattern/d' file.txt              # Delete matching lines
sed '1d' file.txt                      # Delete first line
sed '1,5d' file.txt                    # Delete lines 1-5

# In-place editing
sed -i 's/old/new/g' file.txt

# Multiple commands
sed -e 's/old/new/' -e 's/foo/bar/' file.txt
```

---

## Cron Jobs {#cron}

### Cron Syntax

```
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sunday=0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

### Special Characters

- `*` : Any value
- `,` : List separator (e.g., 1,15,30)
- `-` : Range (e.g., 1-5)
- `/` : Step values (e.g., */5 means every 5)

### Common Cron Patterns

```bash
# Every minute
* * * * * /path/to/script.sh

# Every 5 minutes
*/5 * * * * /path/to/script.sh

# Every hour at minute 0
0 * * * * /path/to/script.sh

# Every day at 2:30 AM
30 2 * * * /path/to/script.sh

# Every Monday at 9:00 AM
0 9 * * 1 /path/to/script.sh

# Every weekday at 6:00 PM
0 18 * * 1-5 /path/to/script.sh

# First day of every month
0 0 1 * * /path/to/script.sh
```

### Special Strings

```bash
@reboot         # Run once at startup
@yearly         # 0 0 1 1 *
@monthly        # 0 0 1 * *
@weekly         # 0 0 * * 0
@daily          # 0 0 * * *
@hourly         # 0 * * * *
```

### Managing Crontab

```bash
# View crontab
crontab -l

# Edit crontab
crontab -e

# Remove crontab
crontab -r

# Edit another user's crontab (requires root)
sudo crontab -u username -e
```

### Cron Job Best Practices

```bash
# Use absolute paths
0 2 * * * /home/user/scripts/backup.sh

# Redirect output to log
0 2 * * * /path/to/script.sh >> /var/log/script.log 2>&1

# Set environment variables
SHELL=/bin/bash
PATH=/usr/local/bin:/usr/bin:/bin
MAILTO=admin@example.com

0 2 * * * /path/to/script.sh

# Prevent overlapping jobs
*/5 * * * * flock -n /tmp/script.lock /path/to/script.sh
```

### Example Cron Script

```bash
#!/bin/bash
set -euo pipefail

# Configuration
BACKUP_DIR="/home/user/backups"
SOURCE_DIR="/home/user/data"
LOG_FILE="/var/log/backup.log"
DATE=$(date +%Y%m%d_%H%M%S)

# Logging function
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"
}

# Create backup
log "Starting backup..."
tar -czf "$BACKUP_DIR/backup_$DATE.tar.gz" "$SOURCE_DIR" 2>&1 | tee -a "$LOG_FILE"
log "Backup completed"

# Cleanup old backups (keep last 7 days)
find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +7 -delete
log "Cleanup completed"
```

---

---

## Quick Reference Cheat Sheet

### Variables
```bash
var="value"              # Assignment
$var or ${var}          # Access
readonly var="value"    # Read-only
unset var               # Delete variable
export var="value"      # Environment variable
```

### Conditionals
```bash
if [ condition ]; then ... fi
if [ cond1 ] && [ cond2 ]; then ... fi
case $var in pattern) ... ;; esac
[[ $str =~ regex ]]     # Regex match
```

### Loops
```bash
for i in {1..10}; do ... done
for ((i=0; i<10; i++)); do ... done
while [ condition ]; do ... done
until [ condition ]; do ... done
```

### Functions
```bash
function_name() { ... }
return 0                # Return status
local var="value"       # Local variable
```

### File Tests
```bash
-e file    # Exists
-f file    # Regular file
-d file    # Directory
-r file    # Readable
-w file    # Writable
-x file    # Executable
-s file    # Not empty
```

### String Operations
```bash
${#str}              # Length
${str:pos:len}       # Substring
${str/old/new}       # Replace first
${str//old/new}      # Replace all
${str^^}             # Uppercase
${str,,}             # Lowercase
```

### Arrays
```bash
arr=(1 2 3)          # Declare
${arr[0]}            # Access
${arr[@]}            # All elements
${#arr[@]}           # Length
arr+=(4)             # Append
```

### Redirection
```bash
cmd > file           # Redirect stdout
cmd >> file          # Append stdout
cmd 2> file          # Redirect stderr
cmd &> file          # Redirect both
cmd < file           # Redirect stdin
cmd1 | cmd2          # Pipe
```

### Common Commands
```bash
grep, sed, awk       # Text processing
find                 # Find files
tar, gzip            # Compression
chmod, chown         # Permissions
ps, top, kill        # Processes
df, du, free         # System info
```

---

