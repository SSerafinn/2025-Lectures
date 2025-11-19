# LARAVEL FUNDAMENTALS - MIDTERM EXAM REVIEWER
## Comprehensive Study Guide

---

## TABLE OF CONTENTS

1. [Introduction to Laravel](#1-introduction-to-laravel)
2. [Routes and Views](#2-routes-and-views)
3. [Blade Templating](#3-blade-templating)
4. [Components and Layouts](#4-components-and-layouts)
5. [Controllers](#5-controllers)
6. [Database and Models](#6-database-and-models)
7. [Migrations](#7-migrations)
8. [Eloquent Relationships](#8-eloquent-relationships)
9. [CRUD Operations](#9-crud-operations)
10. [Authentication](#10-authentication)
11. [Middleware](#11-middleware)
12. [Common Commands](#12-common-commands)

---

## 1. INTRODUCTION TO LARAVEL

### What is Laravel?
- A PHP web application framework
- Uses **MVC (Model-View-Controller)** architecture
- Created by Taylor Otwell
- Makes web development faster and easier

### MVC Architecture
- **Model**: Handles data and business logic (talks to database)
- **View**: What users see (HTML/Blade templates)
- **Controller**: Handles user requests and connects Model and View

### Key Features
- Elegant syntax
- Built-in authentication
- Database migration system
- ORM (Eloquent) for database queries
- Blade templating engine
- Artisan command-line tool

### Installation Requirements
- PHP 8.2 or higher
- Composer (PHP package manager)
- Web server (WAMP/XAMPP for local development)
- Database (MySQL, SQLite, PostgreSQL)

### Project Structure
```
app/              - Core application code
  Http/Controllers/  - Controllers
  Models/           - Eloquent models
bootstrap/        - Framework bootstrapping
config/          - Configuration files
database/        - Migrations, factories, seeders
public/          - Entry point, assets
resources/       - Views, raw assets
  views/         - Blade templates
routes/          - Route definitions
  web.php       - Web routes
storage/         - Logs, cache, uploads
vendor/          - Composer dependencies
.env             - Environment configuration
```

### Important Commands
```bash
# Start development server
php artisan serve

# View all available commands
php artisan list

# Clear cache
php artisan cache:clear
```

---

## 2. ROUTES AND VIEWS

### What are Routes?
Routes define the URLs your application responds to and what should happen when users visit those URLs.

### Route File Location
`routes/web.php` - All web routes are defined here

### Basic Route Syntax
```php
// GET route
Route::get('/url', function() {
    return 'Response';
});

// POST route
Route::post('/url', function() {
    return 'Response';
});

// PUT route (for updates)
Route::put('/url', function() {
    return 'Response';
});

// DELETE route
Route::delete('/url', function() {
    return 'Response';
});
```

### Returning Views
```php
Route::get('/about', function() {
    return view('about');  // looks for resources/views/about.blade.php
});
```

### Passing Data to Views
```php
Route::get('/user', function() {
    return view('user', ['name' => 'John']);
});
```

### Route Parameters
```php
// Required parameter
Route::get('/user/{id}', function($id) {
    return "User ID: " . $id;
});

// Optional parameter
Route::get('/user/{name?}', function($name = 'Guest') {
    return "Hello, " . $name;
});
```

### Named Routes
```php
Route::get('/dashboard', function() {
    return view('dashboard');
})->name('dashboard');

// Generate URL: route('dashboard')
```

### Route Groups
```php
// Group routes with middleware
Route::middleware(['auth'])->group(function() {
    Route::get('/dashboard', [DashboardController::class, 'index']);
    Route::get('/profile', [ProfileController::class, 'show']);
});
```

### Resource Routes
```php
// Creates 7 RESTful routes automatically
Route::resource('posts', PostController::class);

// Generated routes:
// GET    /posts           - index()
// GET    /posts/create    - create()
// POST   /posts           - store()
// GET    /posts/{post}    - show()
// GET    /posts/{post}/edit - edit()
// PUT    /posts/{post}    - update()
// DELETE /posts/{post}    - destroy()
```

---

## 3. BLADE TEMPLATING

### What is Blade?
Blade is Laravel's templating engine that makes it easy to create dynamic HTML.

### Blade File Extension
`.blade.php` - All Blade files must end with this extension

### Displaying Data
```blade
<!-- Escaped output (safe from XSS) -->
{{ $variable }}

<!-- Unescaped output (dangerous - use carefully) -->
{!! $htmlContent !!}

<!-- Output with default value -->
{{ $name ?? 'Guest' }}
```

### Blade Directives

#### Conditionals
```blade
@if($condition)
    <p>Condition is true</p>
@elseif($another)
    <p>Another condition</p>
@else
    <p>All conditions false</p>
@endif

@unless($condition)
    <p>Shown when condition is false</p>
@endunless
```

#### Loops
```blade
@foreach($items as $item)
    <p>{{ $item }}</p>
@endforeach

@for($i = 0; $i < 10; $i++)
    <p>Number: {{ $i }}</p>
@endfor

@while($condition)
    <p>Loop content</p>
@endwhile

@forelse($items as $item)
    <p>{{ $item }}</p>
@empty
    <p>No items found</p>
@endforelse
```

#### Authentication
```blade
@auth
    <p>User is logged in</p>
@endauth

@guest
    <p>User is not logged in</p>
@endguest
```

#### CSRF Protection
```blade
<form method="POST">
    @csrf
    <!-- Form fields -->
</form>
```

#### Method Spoofing
```blade
<form method="POST" action="/posts/1">
    @csrf
    @method('PUT')
    <!-- For PUT, PATCH, or DELETE requests -->
</form>
```

#### Error Display
```blade
@error('field_name')
    <span class="error">{{ $message }}</span>
@enderror
```

---

## 4. COMPONENTS AND LAYOUTS

### What are Components?
Reusable pieces of UI that can be used across different pages.

### Creating a Component
Components are stored in `resources/views/components/`

### Layout Component Example
```blade
<!-- resources/views/components/layout.blade.php -->
<!DOCTYPE html>
<html>
<head>
    <title>{{ $title ?? 'My App' }}</title>
</head>
<body>
    <nav>Navigation here</nav>
    
    <main>
        {{ $slot }}
    </main>
</body>
</html>
```

### Using Components
```blade
<x-layout>
    <x-slot:title>Page Title</x-slot:title>
    
    <h1>Page content here</h1>
</x-layout>
```

### Component with Props
```blade
<!-- resources/views/components/alert.blade.php -->
<div class="alert alert-{{ $type }}">
    {{ $message }}
</div>

<!-- Usage -->
<x-alert type="success" message="Operation completed!" />
```

### Named Slots
```blade
<x-layout>
    <x-slot:title>Custom Title</x-slot:title>
    <x-slot:header>Header Content</x-slot:header>
    
    Main content goes here
</x-layout>
```

---

## 5. CONTROLLERS

### What is a Controller?
A controller handles the logic for your routes and keeps your code organized.

### Creating a Controller
```bash
php artisan make:controller NameController
```

### Controller Location
`app/Http/Controllers/`

### Basic Controller Structure
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class PostController extends Controller
{
    public function index()
    {
        // Display all posts
        return view('posts.index');
    }
    
    public function show($id)
    {
        // Display single post
        return view('posts.show', ['id' => $id]);
    }
}
```

### Using Controllers in Routes
```php
use App\Http\Controllers\PostController;

Route::get('/posts', [PostController::class, 'index']);
Route::get('/posts/{id}', [PostController::class, 'show']);
```

### Resource Controller
```bash
php artisan make:controller PostController --resource
```

Creates a controller with all RESTful methods:
- `index()` - Display list
- `create()` - Show create form
- `store()` - Store new record
- `show()` - Display single record
- `edit()` - Show edit form
- `update()` - Update record
- `destroy()` - Delete record

---

## 6. DATABASE AND MODELS

### Database Configuration
Located in `.env` file:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database
DB_USERNAME=root
DB_PASSWORD=
```

### SQLite Configuration
```env
DB_CONNECTION=sqlite
# Comment out other DB_ lines
```

Create SQLite database:
```bash
# Windows
type nul > database/database.sqlite

# Mac/Linux
touch database/database.sqlite
```

### What is a Model?
A model represents a table in your database and allows you to interact with it using PHP.

### Creating a Model
```bash
php artisan make:model Post
```

### Model Location
`app/Models/`

### Basic Model Structure
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    // Table name (optional - Laravel auto-detects)
    protected $table = 'posts';
    
    // Mass assignable fields
    protected $fillable = ['title', 'content', 'user_id'];
    
    // Hidden fields (not shown in JSON)
    protected $hidden = ['password'];
}
```

### Eloquent Basics
```php
// Get all records
$posts = Post::all();

// Find by ID
$post = Post::find(1);

// Find or fail (throws 404 if not found)
$post = Post::findOrFail(1);

// Create new record
$post = Post::create([
    'title' => 'Title',
    'content' => 'Content'
]);

// Update record
$post->update(['title' => 'New Title']);

// Delete record
$post->delete();

// Query with conditions
$posts = Post::where('user_id', 1)->get();

// Order results
$posts = Post::latest()->get();  // newest first
$posts = Post::oldest()->get();  // oldest first
```

---

## 7. MIGRATIONS

### What are Migrations?
Migrations are version control for your database. They define table structures.

### Creating a Migration
```bash
# Create new table
php artisan make:migration create_posts_table

# Add column to existing table
php artisan make:migration add_user_id_to_posts_table
```

### Migration Location
`database/migrations/`

### Migration Structure
```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('content');
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('posts');
    }
};
```

### Common Column Types
```php
$table->id();                    // Auto-increment ID
$table->string('name');          // VARCHAR
$table->string('name', 100);     // VARCHAR with length
$table->text('content');         // TEXT
$table->integer('count');        // INTEGER
$table->boolean('active');       // BOOLEAN
$table->date('published_at');    // DATE
$table->timestamps();            // created_at & updated_at
$table->softDeletes();          // deleted_at
```

### Foreign Keys
```php
$table->foreignId('user_id')
      ->constrained()
      ->cascadeOnDelete();

// Equivalent to:
$table->unsignedBigInteger('user_id');
$table->foreign('user_id')
      ->references('id')
      ->on('users')
      ->onDelete('cascade');
```

### Running Migrations
```bash
# Run all pending migrations
php artisan migrate

# Rollback last migration
php artisan migrate:rollback

# Reset all migrations
php artisan migrate:reset

# Fresh migration (drop all tables and re-run)
php artisan migrate:fresh

# Check migration status
php artisan migrate:status
```

---

## 8. ELOQUENT RELATIONSHIPS

### One-to-Many (hasMany / belongsTo)

**Example:** User has many Posts

```php
// User Model
class User extends Model
{
    public function posts()
    {
        return $this->hasMany(Post::class);
    }
}

// Post Model
class Post extends Model
{
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}

// Usage
$user = User::find(1);
$posts = $user->posts;  // Get all posts by user

$post = Post::find(1);
$author = $post->user;  // Get post author
```

### Many-to-Many (belongsToMany)

**Example:** Users can like many Posts

```php
// User Model
class User extends Model
{
    public function likedPosts()
    {
        return $this->belongsToMany(Post::class, 'likes');
    }
}

// Post Model
class Post extends Model
{
    public function likedBy()
    {
        return $this->belongsToMany(User::class, 'likes');
    }
}

// Usage
$user->likedPosts()->attach($postId);    // Add like
$user->likedPosts()->detach($postId);    // Remove like
$user->likedPosts()->toggle($postId);    // Toggle like
```

### Relationship Methods
```php
// Check if relationship exists
if ($user->posts()->exists()) { }

// Count related records
$count = $user->posts()->count();

// Create related record
$user->posts()->create([
    'title' => 'New Post',
    'content' => 'Content'
]);
```

---

## 9. CRUD OPERATIONS

### Create

**Route:**
```php
Route::get('/posts/create', [PostController::class, 'create']);
Route::post('/posts', [PostController::class, 'store']);
```

**Controller:**
```php
public function create()
{
    return view('posts.create');
}

public function store(Request $request)
{
    $validated = $request->validate([
        'title' => 'required|max:255',
        'content' => 'required',
    ]);
    
    auth()->user()->posts()->create($validated);
    
    return redirect()->route('posts.index')
        ->with('success', 'Post created!');
}
```

**View:**
```blade
<form method="POST" action="{{ route('posts.store') }}">
    @csrf
    <input type="text" name="title" value="{{ old('title') }}">
    @error('title')
        <span>{{ $message }}</span>
    @enderror
    
    <textarea name="content">{{ old('content') }}</textarea>
    
    <button type="submit">Create</button>
</form>
```

### Read

**Controller:**
```php
public function index()
{
    $posts = Post::latest()->get();
    return view('posts.index', ['posts' => $posts]);
}

public function show(Post $post)  // Route Model Binding
{
    return view('posts.show', ['post' => $post]);
}
```

### Update

**Routes:**
```php
Route::get('/posts/{post}/edit', [PostController::class, 'edit']);
Route::put('/posts/{post}', [PostController::class, 'update']);
```

**Controller:**
```php
public function edit(Post $post)
{
    return view('posts.edit', ['post' => $post]);
}

public function update(Request $request, Post $post)
{
    $validated = $request->validate([
        'title' => 'required|max:255',
        'content' => 'required',
    ]);
    
    $post->update($validated);
    
    return redirect()->route('posts.show', $post)
        ->with('success', 'Post updated!');
}
```

**View:**
```blade
<form method="POST" action="{{ route('posts.update', $post) }}">
    @csrf
    @method('PUT')
    
    <input type="text" name="title" value="{{ old('title', $post->title) }}">
    <textarea name="content">{{ old('content', $post->content) }}</textarea>
    
    <button type="submit">Update</button>
</form>
```

### Delete

**Route:**
```php
Route::delete('/posts/{post}', [PostController::class, 'destroy']);
```

**Controller:**
```php
public function destroy(Post $post)
{
    $post->delete();
    
    return redirect()->route('posts.index')
        ->with('success', 'Post deleted!');
}
```

**View:**
```blade
<form method="POST" action="{{ route('posts.destroy', $post) }}">
    @csrf
    @method('DELETE')
    <button type="submit">Delete</button>
</form>
```

### Validation Rules
```php
'field' => 'required',              // Must have value
'field' => 'required|max:255',      // Multiple rules
'field' => 'required|min:10',       // Minimum length
'field' => 'required|email',        // Must be email
'field' => 'required|unique:table', // Must be unique
'field' => 'required|exists:table,column',  // Must exist
'field' => 'nullable',              // Can be null
'field' => 'required|in:value1,value2',  // Must be one of
```

---

## 10. AUTHENTICATION

### Laravel Breeze Installation
```bash
composer require laravel/breeze --dev
php artisan breeze:install blade
npm install
npm run dev
php artisan migrate
```

### What Breeze Provides
- Login page
- Registration page
- Password reset
- Email verification
- Profile management
- Authentication middleware

### Authentication Helpers
```php
// Check if user is authenticated
auth()->check();

// Get current user
auth()->user();

// Get current user ID
auth()->id();

// Login user
auth()->login($user);

// Logout user
auth()->logout();
```

### Using in Blade
```blade
@auth
    <p>Welcome, {{ auth()->user()->name }}</p>
@endauth

@guest
    <a href="{{ route('login') }}">Login</a>
@endguest
```

### Protecting Routes
```php
Route::middleware(['auth'])->group(function() {
    Route::get('/dashboard', [DashboardController::class, 'index']);
});
```

### Authorization
```php
// In controller
if ($post->user_id !== auth()->id()) {
    abort(403, 'Unauthorized');
}
```

---

## 11. MIDDLEWARE

### What is Middleware?
Middleware filters HTTP requests entering your application.

### Built-in Middleware
- `auth` - Requires authentication
- `guest` - Only for non-authenticated users
- `verified` - Requires email verification

### Using Middleware
```php
// On single route
Route::get('/dashboard', function() {
    //
})->middleware('auth');

// On route group
Route::middleware(['auth'])->group(function() {
    // Routes here
});

// In controller constructor
public function __construct()
{
    $this->middleware('auth');
}
```

---

## 12. COMMON COMMANDS

### Artisan Commands
```bash
# Server
php artisan serve

# Database
php artisan migrate
php artisan migrate:fresh
php artisan migrate:rollback

# Generation
php artisan make:model ModelName
php artisan make:controller ControllerName
php artisan make:migration create_table_name
php artisan make:middleware MiddlewareName

# Cache
php artisan cache:clear
php artisan config:clear
php artisan route:clear

# Tinker (Interactive shell)
php artisan tinker

# List all routes
php artisan route:list
```

### Composer Commands
```bash
# Install dependencies
composer install

# Update dependencies
composer update

# Require package
composer require package-name
```

### NPM Commands
```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build
```

---


## QUICK REFERENCE CARD

| Concept | Syntax |
|---------|--------|
| Route | `Route::get('/url', [Controller::class, 'method'])` |
| Display data | `{{ $variable }}` |
| Loop | `@foreach($items as $item) ... @endforeach` |
| Auth check | `@auth ... @endauth` |
| CSRF | `@csrf` |
| Method spoofing | `@method('PUT')` |
| Get all | `Model::all()` |
| Find by ID | `Model::find($id)` |
| Create | `Model::create([...])` |
| Update | `$model->update([...])` |
| Delete | `$model->delete()` |
| Relationship | `$user->posts` |
| Migrate | `php artisan migrate` |
| Make model | `php artisan make:model Name` |
| Make controller | `php artisan make:controller Name` |

---

**Good luck with your exam! 🍀**

Remember: Understanding the concepts is more important than memorizing syntax. Focus on WHY things work the way they do, not just HOW to write them.
