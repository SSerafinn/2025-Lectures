# Integrative and Programming Technologies
## Course Introduction & Native PHP Fundamentals

---

## Course Overview

### What are Integrative and Programming Technologies?

Integrative and Programming Technologies encompass the tools, languages, and methodologies used to build interconnected software systems. This course focuses on web development technologies that allow different components to work together seamlessly - from databases to server-side logic to client interfaces.

### Course Learning Path

1. **Weeks 1-7: Native PHP Foundation**
   - Core PHP syntax and programming concepts
   - Working with databases using MySQLi/PDO
   - Building dynamic web applications from scratch
   - Understanding HTTP, sessions, and security basics

2. **Midterms: Introduction to Modern PHP & Composer**
   - Package management
   - Autoloading and namespaces
   - MVC pattern implementation

3. **Finals: Laravel Framework**
   - Framework architecture
   - Eloquent ORM
   - Routing and middleware
   - Building production-ready applications

---

## Module 1: Introduction to PHP

### What is PHP?

PHP (PHP: Hypertext Preprocessor) is a server-side scripting language designed for web development. Created by Rasmus Lerdorf in 1994, PHP powers approximately 77% of all websites whose server-side language is known, including platforms like WordPress, Facebook, and Wikipedia.

### Why Learn Native PHP First?

Understanding native PHP before jumping into frameworks is crucial because:

- **Foundation Knowledge**: Frameworks abstract many concepts that you need to understand fundamentally
- **Debugging Skills**: When framework magic fails, you need to understand what's happening under the hood
- **Flexibility**: Not every project needs a framework; sometimes native PHP is the better choice
- **Career Advantage**: Employers value developers who understand core concepts, not just framework syntax

### PHP in the Modern Web Stack

PHP typically works within this architecture:

```
Client (Browser) → HTTP Request → Web Server (Apache/Nginx) → PHP Interpreter → Database
                 ← HTTP Response ←                          ←                ←
```

---

## Setting Up Your Development Environment

### Required Software

1. **Local Development Server**
   - Option A: XAMPP (Windows/Mac/Linux) - Includes Apache, MySQL, PHP
   - Option B: WAMP (Windows) / MAMP (Mac)
   - Option C: Docker with PHP image (Advanced)

2. **Code Editor**
   - Visual Studio Code (Recommended)
   - PHPStorm (Professional IDE)
   - Sublime Text

3. **Browser Developer Tools**
   - Chrome DevTools or Firefox Developer Tools

### Installation Steps (XAMPP)

1. Download XAMPP from apachefriends.org
2. Install with default settings
3. Start Apache and MySQL from XAMPP Control Panel
4. Test installation by navigating to `http://localhost`
5. Your PHP files go in `htdocs` folder (usually `C:\xampp\htdocs` on Windows)

### First PHP Script

Create a file named `info.php` in your htdocs folder:

```php
<?php
phpinfo();
?>
```

Navigate to `http://localhost/info.php` - you should see PHP configuration information.

---

## PHP Fundamentals

### Basic Syntax

PHP code is embedded in HTML using PHP tags:

```php
<!DOCTYPE html>
<html>
<body>
    <h1>My First PHP Page</h1>
    <?php
        echo "Hello, World!";
        // This is a single-line comment
        /* This is a 
           multi-line comment */
    ?>
</body>
</html>
```

### Variables and Data Types

PHP is dynamically typed - you don't declare variable types:

```php
<?php
// Variables start with $
$name = "John Doe";        // String
$age = 25;                 // Integer
$height = 5.9;            // Float
$isStudent = true;        // Boolean
$grades = [85, 90, 78];   // Array
$person = null;           // NULL

// PHP is loosely typed - automatic type conversion
$sum = "10" + 5;  // Results in 15 (integer)

// Variable variables (dynamic variable names)
$varName = "message";
$$varName = "Hello";  // Creates $message = "Hello"

// Constants (cannot be changed)
define("SITE_NAME", "My Website");
const DB_HOST = "localhost";
?>
```

### Operators

```php
<?php
// Arithmetic Operators
$a = 10; $b = 3;
echo $a + $b;  // 13
echo $a - $b;  // 7
echo $a * $b;  // 30
echo $a / $b;  // 3.333...
echo $a % $b;  // 1 (modulus)
echo $a ** $b; // 1000 (exponentiation)

// Assignment Operators
$x = 5;
$x += 3;  // $x = $x + 3
$x -= 2;  // $x = $x - 2
$x *= 4;  // $x = $x * 4
$x /= 2;  // $x = $x / 2
$x .= " text"; // String concatenation assignment

// Comparison Operators
$a == $b;   // Equal (value only)
$a === $b;  // Identical (value and type)
$a != $b;   // Not equal
$a !== $b;  // Not identical
$a < $b;    // Less than
$a > $b;    // Greater than
$a <= $b;   // Less than or equal
$a >= $b;   // Greater than or equal
$a <=> $b;  // Spaceship operator (PHP 7+)

// Logical Operators
$a && $b;   // AND
$a || $b;   // OR
!$a;        // NOT
$a and $b;  // AND (lower precedence)
$a or $b;   // OR (lower precedence)
$a xor $b;  // XOR
?>
```

### Control Structures

```php
<?php
// If-Else Statement
$score = 85;

if ($score >= 90) {
    echo "Grade: A";
} elseif ($score >= 80) {
    echo "Grade: B";
} elseif ($score >= 70) {
    echo "Grade: C";
} else {
    echo "Grade: F";
}

// Ternary Operator
$status = ($age >= 18) ? "Adult" : "Minor";

// Null Coalescing Operator (PHP 7+)
$username = $_GET['user'] ?? 'guest';

// Switch Statement
$day = "Monday";

switch ($day) {
    case "Monday":
        echo "Start of the work week";
        break;
    case "Friday":
        echo "TGIF!";
        break;
    case "Saturday":
    case "Sunday":
        echo "Weekend!";
        break;
    default:
        echo "Midweek";
}

// Match Expression (PHP 8+)
$result = match($day) {
    'Monday' => 'Start of work week',
    'Friday' => 'TGIF',
    'Saturday', 'Sunday' => 'Weekend',
    default => 'Midweek'
};
?>
```

### Loops

```php
<?php
// For Loop
for ($i = 0; $i < 5; $i++) {
    echo "Number: $i <br>";
}

// While Loop
$count = 0;
while ($count < 5) {
    echo "Count: $count <br>";
    $count++;
}

// Do-While Loop
$num = 0;
do {
    echo "Number: $num <br>";
    $num++;
} while ($num < 5);

// Foreach Loop (for arrays)
$colors = ["red", "green", "blue"];

foreach ($colors as $color) {
    echo "Color: $color <br>";
}

// Foreach with key-value pairs
$person = [
    "name" => "John",
    "age" => 25,
    "city" => "New York"
];

foreach ($person as $key => $value) {
    echo "$key: $value <br>";
}

// Break and Continue
for ($i = 0; $i < 10; $i++) {
    if ($i == 3) continue;  // Skip iteration
    if ($i == 7) break;     // Exit loop
    echo $i;
}
?>
```

---

## Working with Forms and User Input

### HTML Form Basics

```html
<!-- form.html -->
<!DOCTYPE html>
<html>
<body>
    <h2>Registration Form</h2>
    <form method="POST" action="process.php">
        <label>Name:</label>
        <input type="text" name="fullname" required><br><br>
        
        <label>Email:</label>
        <input type="email" name="email" required><br><br>
        
        <label>Age:</label>
        <input type="number" name="age" min="1" max="100"><br><br>
        
        <label>Gender:</label>
        <input type="radio" name="gender" value="male"> Male
        <input type="radio" name="gender" value="female"> Female<br><br>
        
        <label>Interests:</label>
        <input type="checkbox" name="interests[]" value="sports"> Sports
        <input type="checkbox" name="interests[]" value="music"> Music
        <input type="checkbox" name="interests[]" value="reading"> Reading<br><br>
        
        <label>Country:</label>
        <select name="country">
            <option value="usa">USA</option>
            <option value="uk">UK</option>
            <option value="canada">Canada</option>
        </select><br><br>
        
        <label>Comments:</label>
        <textarea name="comments" rows="4" cols="50"></textarea><br><br>
        
        <input type="submit" value="Submit">
    </form>
</body>
</html>
```

### Processing Form Data

```php
<?php
// process.php

// Check if form was submitted
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    
    // Retrieve and sanitize input
    $fullname = sanitizeInput($_POST["fullname"] ?? "");
    $email = filter_var($_POST["email"] ?? "", FILTER_SANITIZE_EMAIL);
    $age = filter_var($_POST["age"] ?? 0, FILTER_SANITIZE_NUMBER_INT);
    $gender = sanitizeInput($_POST["gender"] ?? "");
    $interests = $_POST["interests"] ?? [];
    $country = sanitizeInput($_POST["country"] ?? "");
    $comments = sanitizeInput($_POST["comments"] ?? "");
    
    // Validate input
    $errors = [];
    
    if (empty($fullname)) {
        $errors[] = "Name is required";
    }
    
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        $errors[] = "Invalid email format";
    }
    
    if ($age < 1 || $age > 100) {
        $errors[] = "Age must be between 1 and 100";
    }
    
    // Display results or errors
    if (empty($errors)) {
        echo "<h2>Registration Successful!</h2>";
        echo "Name: " . htmlspecialchars($fullname) . "<br>";
        echo "Email: " . htmlspecialchars($email) . "<br>";
        echo "Age: " . $age . "<br>";
        echo "Gender: " . htmlspecialchars($gender) . "<br>";
        echo "Interests: " . implode(", ", array_map('htmlspecialchars', $interests)) . "<br>";
        echo "Country: " . htmlspecialchars($country) . "<br>";
        echo "Comments: " . nl2br(htmlspecialchars($comments)) . "<br>";
    } else {
        echo "<h2>Errors:</h2>";
        foreach ($errors as $error) {
            echo "• " . $error . "<br>";
        }
        echo '<a href="form.html">Go back</a>';
    }
}

// Sanitization function
function sanitizeInput($data) {
    $data = trim($data);
    $data = stripslashes($data);
    $data = htmlspecialchars($data);
    return $data;
}
?>
```

---

## Functions in PHP

### Defining and Calling Functions

```php
<?php
// Basic function
function greet() {
    echo "Hello, World!";
}

greet(); // Call the function

// Function with parameters
function greetUser($name, $age) {
    echo "Hello, $name! You are $age years old.";
}

greetUser("John", 25);

// Function with default parameters
function createUser($name, $role = "member") {
    echo "Created user: $name with role: $role";
}

createUser("Alice");           // Uses default role
createUser("Bob", "admin");    // Overrides default

// Function with return value
function calculateArea($length, $width) {
    return $length * $width;
}

$area = calculateArea(10, 5);
echo "Area: $area";

// Variable-length argument lists
function sum(...$numbers) {
    $total = 0;
    foreach ($numbers as $num) {
        $total += $num;
    }
    return $total;
}

echo sum(1, 2, 3, 4, 5); // 15

// Type declarations (PHP 7+)
function addNumbers(int $a, int $b): int {
    return $a + $b;
}

// Anonymous functions (closures)
$multiply = function($a, $b) {
    return $a * $b;
};

echo $multiply(3, 4); // 12

// Arrow functions (PHP 7.4+)
$double = fn($n) => $n * 2;
echo $double(5); // 10
?>
```

### Variable Scope

```php
<?php
$globalVar = "I'm global";

function testScope() {
    $localVar = "I'm local";
    
    // Accessing global variable
    global $globalVar;
    echo $globalVar;
    
    // Or use $GLOBALS array
    echo $GLOBALS['globalVar'];
}

testScope();
// echo $localVar; // Error: undefined variable

// Static variables
function counter() {
    static $count = 0;
    $count++;
    echo "Count: $count<br>";
}

counter(); // Count: 1
counter(); // Count: 2
counter(); // Count: 3
?>
```

---

## Arrays in PHP

### Array Types and Creation

```php
<?php
// Indexed Arrays
$fruits = array("Apple", "Banana", "Orange");
$vegetables = ["Carrot", "Broccoli", "Spinach"]; // Short syntax

// Associative Arrays
$person = [
    "name" => "John Doe",
    "age" => 30,
    "email" => "john@example.com"
];

// Multidimensional Arrays
$students = [
    ["name" => "Alice", "grade" => 85],
    ["name" => "Bob", "grade" => 92],
    ["name" => "Charlie", "grade" => 78]
];

// Accessing array elements
echo $fruits[0];           // Apple
echo $person["name"];      // John Doe
echo $students[1]["grade"]; // 92

// Adding elements
$fruits[] = "Mango";              // Adds to end
$fruits[10] = "Grape";            // Specific index
$person["phone"] = "123-456-7890"; // New key

// Modifying elements
$fruits[0] = "Green Apple";
$person["age"] = 31;
?>
```

### Array Functions

```php
<?php
$numbers = [3, 1, 4, 1, 5, 9, 2, 6];
$assoc = ["b" => 2, "a" => 1, "c" => 3];

// Count elements
echo count($numbers); // 8

// Sort arrays
sort($numbers);        // Ascending order
rsort($numbers);       // Descending order
asort($assoc);        // Sort by value, maintain keys
ksort($assoc);        // Sort by keys

// Search and filter
if (in_array(5, $numbers)) {
    echo "Found 5!";
}

$key = array_search(9, $numbers);  // Returns key of value
$filtered = array_filter($numbers, fn($n) => $n > 3);

// Transform arrays
$doubled = array_map(fn($n) => $n * 2, $numbers);
$sum = array_reduce($numbers, fn($carry, $item) => $carry + $item, 0);

// Array operations
$arr1 = [1, 2, 3];
$arr2 = [4, 5, 6];

$merged = array_merge($arr1, $arr2);     // [1,2,3,4,5,6]
$combined = array_combine($arr1, $arr2); // [1=>4, 2=>5, 3=>6]
$diff = array_diff($arr1, $arr2);        // Elements in arr1 not in arr2
$intersect = array_intersect($arr1, $arr2); // Common elements

// Extract and compact
$info = ["name" => "John", "age" => 30];
extract($info); // Creates $name and $age variables

$name = "Jane";
$age = 25;
$newArray = compact("name", "age"); // Creates array from variables

// Array destructuring (PHP 7.1+)
[$first, $second, $third] = $arr1;
["name" => $userName, "age" => $userAge] = $info;
?>
```

---

## String Manipulation

### String Functions

```php
<?php
$text = "  Hello, World!  ";
$email = "john.doe@example.com";

// Length and trimming
echo strlen($text);           // 17
echo trim($text);             // "Hello, World!"
echo ltrim($text);            // Remove left whitespace
echo rtrim($text);            // Remove right whitespace

// Case conversion
echo strtoupper($text);       // "  HELLO, WORLD!  "
echo strtolower($text);       // "  hello, world!  "
echo ucfirst("hello");        // "Hello"
echo ucwords("hello world");  // "Hello World"

// Substring operations
echo substr($email, 0, 8);    // "john.doe"
echo substr($email, -11);     // "example.com"
echo strpos($email, "@");     // 8
echo strrpos($email, ".");    // 15

// String replacement
echo str_replace("World", "PHP", $text);  // "  Hello, PHP!  "
echo str_ireplace("world", "PHP", $text); // Case-insensitive replace

// String splitting and joining
$parts = explode("@", $email);     // ["john.doe", "example.com"]
$joined = implode("-", $parts);    // "john.doe-example.com"

// String formatting
$name = "John";
$age = 30;
echo sprintf("Name: %s, Age: %d", $name, $age);
printf("Price: $%.2f", 19.95);    // Price: $19.95

// String comparison
if (strcmp("abc", "abc") == 0) {
    echo "Strings are equal";
}

if (strcasecmp("ABC", "abc") == 0) {
    echo "Equal (case-insensitive)";
}

// Regular expressions
if (preg_match("/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/", $email)) {
    echo "Valid email";
}

$phone = "123-456-7890";
$cleaned = preg_replace("/[^0-9]/", "", $phone); // "1234567890"

// Heredoc and Nowdoc (for multiline strings)
$heredoc = <<<EOT
This is a heredoc string.
Variables like $name are parsed.
EOT;

$nowdoc = <<<'EOT'
This is a nowdoc string.
Variables like $name are NOT parsed.
EOT;
?>
```

---

## Lab Exercise 1: Building a Simple Student Management System

### Objective
Create a basic student management system using native PHP that demonstrates form handling, data validation, and array manipulation.

### Requirements

1. Create a registration form for students with fields:
   - Student ID
   - Full Name
   - Email
   - Course
   - Year Level (1-4)

2. Implement validation:
   - All fields are required
   - Email must be valid format
   - Student ID must be numeric
   - Year level must be between 1-4

3. Store submitted data in a session array

4. Display all registered students in a table

5. Add search functionality to find students by name or ID

### Starter Code Structure

```php
<?php
// index.php
session_start();

// Initialize students array in session
if (!isset($_SESSION['students'])) {
    $_SESSION['students'] = [];
}

// Handle form submission
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Your validation and processing code here
}

// Handle search
$searchResults = [];
if (isset($_GET['search'])) {
    // Your search code here
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Student Management System</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .form-group { margin-bottom: 10px; }
        label { display: inline-block; width: 120px; }
        input, select { width: 200px; padding: 5px; }
        table { border-collapse: collapse; width: 100%; margin-top: 20px; }
        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
        .error { color: red; }
        .success { color: green; }
    </style>
</head>
<body>
    <h1>Student Management System</h1>
    
    <!-- Registration Form -->
    <h2>Register New Student</h2>
    <form method="POST">
        <!-- Add your form fields here -->
    </form>
    
    <!-- Search Form -->
    <h2>Search Students</h2>
    <form method="GET">
        <!-- Add search field here -->
    </form>
    
    <!-- Students Table -->
    <h2>Registered Students</h2>
    <table>
        <!-- Display students here -->
    </table>
</body>
</html>
```

---

## Best Practices for Native PHP Development

### 1. Security Best Practices

```php
<?php
// Always validate and sanitize user input
$email = filter_input(INPUT_POST, 'email', FILTER_SANITIZE_EMAIL);

// Use prepared statements for database queries (we'll cover this in detail later)
// NEVER do this:
// $query = "SELECT * FROM users WHERE email = '$email'";

// Prevent XSS attacks
echo htmlspecialchars($userInput, ENT_QUOTES, 'UTF-8');

// Prevent CSRF attacks
session_start();
if (empty($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}

// Validate file uploads
$allowedTypes = ['image/jpeg', 'image/png', 'image/gif'];
if (in_array($_FILES['upload']['type'], $allowedTypes)) {
    // Process file
}

// Use password hashing
$hashedPassword = password_hash($password, PASSWORD_DEFAULT);
if (password_verify($inputPassword, $hashedPassword)) {
    // Password is correct
}
?>
```

### 2. Code Organization

```php
<?php
// Separate concerns using includes

// config.php - Configuration settings
define('DB_HOST', 'localhost');
define('DB_NAME', 'myapp');

// functions.php - Reusable functions
function validateEmail($email) {
    return filter_var($email, FILTER_VALIDATE_EMAIL);
}

// header.php - Common HTML header
?>
<!DOCTYPE html>
<html>
<head>
    <title><?= $pageTitle ?? 'My App' ?></title>
</head>
<body>

<?php
// footer.php - Common HTML footer
?>
</body>
</html>

<?php
// main.php - Main application file
require_once 'config.php';
require_once 'functions.php';

$pageTitle = 'Home';
include 'header.php';

// Main content here

include 'footer.php';
?>
```

### 3. Error Handling

```php
<?php
// Development environment
error_reporting(E_ALL);
ini_set('display_errors', 1);

// Production environment
error_reporting(E_ALL);
ini_set('display_errors', 0);
ini_set('log_errors', 1);
ini_set('error_log', '/path/to/error.log');

// Custom error handling
set_error_handler(function($errno, $errstr, $errfile, $errline) {
    error_log("Error [$errno]: $errstr in $errfile on line $errline");
    
    if ($errno === E_USER_ERROR) {
        echo "A critical error occurred. Please try again later.";
        exit;
    }
});

// Try-catch for exceptions
try {
    // Code that might throw an exception
    if (!$result) {
        throw new Exception("Operation failed");
    }
} catch (Exception $e) {
    error_log($e->getMessage());
    echo "An error occurred. Please try again.";
}
?>
```

---

## Homework Assignment

### Task: Create a Simple Calculator Web Application

Build a calculator that:

1. Has a form with two number inputs and operation selection (+, -, *, /, %)
2. Validates that inputs are numeric
3. Handles division by zero
4. Stores calculation history in session
5. Displays last 10 calculations
6. Includes a "Clear History" button
7. Uses functions for each mathematical operation
8. Implements proper error handling

### Bonus Features
- Add scientific operations (power, square root)
- Allow chaining operations
- Export history as CSV
- Add keyboard support using JavaScript

---

## Resources for Further Learning

### Official Documentation
- PHP Manual: https://www.php.net/manual/
- PHP: The Right Way: https://phptherightway.com/

### Recommended Reading
- "Modern PHP" by Josh Lockhart
- "PHP Objects, Patterns, and Practice" by Matt Zandstra

### Online Platforms
- Laracasts (Excellent PHP and Laravel tutorials)
- PHP Tutorial by W3Schools (for quick references)
- Stack Overflow PHP Tag (for problem-solving)

### Next Week Preview
In our next session, we'll dive into:
- Working with databases using MySQLi and PDO
- CRUD operations (Create, Read, Update, Delete)
- Building a complete web application with database integration
- Introduction to sessions and cookies
- File handling and uploads

---

## Key Takeaways

1. **PHP is everywhere**: Understanding PHP opens doors to working with countless existing systems and frameworks

2. **Master the fundamentals**: Strong foundation in native PHP makes learning frameworks much easier

3. **Security is paramount**: Always validate, sanitize, and escape data appropriately

4. **Practice makes perfect**: Build small projects to reinforce concepts

5. **Code organization matters**: Even in native PHP, structure your code for maintainability

Remember: Laravel and other frameworks are just PHP under the hood. The time you invest in understanding native PHP will pay dividends throughout your career as a web developer.

---

## Questions for Discussion

1. Why do you think PHP remains so popular despite competition from Node.js, Python, and other languages?

2. What are the advantages and disadvantages of using native PHP vs. a framework?

3. How does PHP's approach to web development differ from JavaScript's approach?

4. What security vulnerabilities should PHP developers be most concerned about?

5. How might PHP evolve in the coming years to remain competitive?

Good luck with your PHP journey! Remember, every expert was once a beginner. Keep coding, keep learning, and don't be afraid to make mistakes - they're your best teachers.