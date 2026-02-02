# VAL_EDU - Educational Platform

A comprehensive web-based educational management system designed for managing tutoring relationships, courses, payments, and student progress. Built with PHP, MySQL, and vanilla JavaScript for a robust and scalable learning platform.

## 📋 Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Configuration](#configuration)
- [Project Structure](#project-structure)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [User Roles](#user-roles)
- [Testing](#testing)
- [Documentation](#documentation)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## ✨ Features

### Core Functionality

- **Multi-Role User System** - Support for Admin, Tutor, Student, and Parent roles
- **Course Management** - Create, update, and manage courses with detailed scheduling
- **Payment System** - Track payments, generate invoices, and manage financial records
- **Calendar Integration** - Visual calendar for scheduling and event management
- **Student Dashboard** - Personalized student interface with course information
- **Parent Portal** - Parent access to student progress and course information
- **Tutor Management** - Tutor profiles, scheduling, and course assignments
- **Authentication** - Secure login and session management
- **Time Tracking** - Precise time tracking for sessions and courses
- **Data Validation** - Comprehensive input validation and error handling

### Advanced Features

- **Role-Based Access Control** - Fine-grained permissions for each user role
- **API Endpoints** - RESTful API for programmatic access
- **Responsive Design** - Works on desktop and mobile devices
- **UTF-8 Support** - Full support for international characters (including Vietnamese)
- **Error Logging** - Comprehensive error tracking and debugging

## 🏗️ Architecture

### Design Pattern: MVC (Model-View-Controller)

```
App Layer:
  ├── Controllers (handle requests and business logic)
  ├── Models (handle data access and queries)
  └── Views (handle presentation and UI)

Supporting Layer:
  ├── Base Classes (BaseController, BaseModel, Database)
  ├── Routing (URL to Controller mapping)
  └── API Routes (REST endpoints)
```

### Request Flow

```
1. User Request
   ↓
2. index.php (routing)
   ↓
3. Controller (authentication & validation)
   ↓
4. Model (database queries)
   ↓
5. View (HTML rendering) OR JSON Response
```

## 🛠️ Tech Stack

### Backend
- **Language:** PHP 7.4+
- **Database:** MySQL 5.7+
- **Architecture:** Custom MVC Framework
- **Session Management:** PHP Sessions
- **Encryption:** bcrypt for passwords

### Frontend
- **Markup:** HTML5
- **Styling:** CSS3
- **Scripts:** Vanilla JavaScript (no dependencies)
- **Calendar:** Custom JavaScript calendar implementation
- **Forms:** Dynamic form handling with AJAX

### Development Tools
- **Version Control:** Git
- **Server:** Apache (XAMPP recommended)
- **Database Management:** MySQL

## 📦 Installation

### Prerequisites

- PHP 7.4 or higher
- MySQL 5.7 or higher
- Apache with mod_rewrite enabled
- XAMPP (recommended for local development)
- Git (for cloning the repository)

### Step 1: Clone the Repository

```bash
git clone https://github.com/anomalyco/VAL_EDU.git
cd VAL_EDU
```

### Step 2: Set Up Database

1. **Using XAMPP:**
   - Start Apache and MySQL from XAMPP Control Panel
   - Open phpMyAdmin: http://localhost/phpmyadmin

2. **Import Database Schema:**
   ```bash
   # Option 1: Via phpMyAdmin
   # 1. Create new database: ValEduDatabase
   # 2. Import webapp/ValEduDatabase.sql
   
   # Option 2: Via Command Line
   mysql -u root < webapp/ValEduDatabase.sql
   ```

### Step 3: Configure Application

1. **Database Configuration:**
   - Open `webapp/Base/Database.php`
   - Default XAMPP configuration is already set:
     ```php
     private $host = '127.0.0.1';
     private $database = 'ValEduDatabase';
     private $username = 'root';
     private $password = ''; // Empty for XAMPP
     ```

2. **Web Server Configuration:**
   - Place project in XAMPP htdocs: `C:\xampp\htdocs\VAL_EDU`
   - OR configure your Apache DocumentRoot to point to project root

### Step 4: Verify Installation

Access the application:
```
http://localhost/VAL_EDU/webapp/
```

### Step 5: Create Admin User (if needed)

```bash
# Generate password hash
php webapp/generate_password.php

# Then manually insert into users table in phpMyAdmin
# Or use the admin registration interface
```

## ⚙️ Configuration

### Database Configuration

Edit `webapp/Base/Database.php`:

```php
private $host = '127.0.0.1';      // Database host
private $port = 3306;              // MySQL port
private $database = 'ValEduDatabase'; // Database name
private $username = 'root';        // MySQL username
private $password = '';            // MySQL password
```

### Session Configuration

Default session configuration in `webapp/index.php`:
```php
session_start();
```

To modify session settings, update PHP configuration or add to index.php:
```php
ini_set('session.gc_maxlifetime', 3600); // 1 hour
```

### Charset Configuration

UTF-8 is configured in `Base/Database.php`:
```php
$this->connection->set_charset("utf8mb4");
```

## 🎓 Configuration Management - Learning Notes

### Current Approach: Hard-coded Configuration

This project uses hard-coded configuration values in `Base/Database.php` for **learning purposes only**. This demonstrates how basic configuration works but is **not recommended for production use**.

**Current Method:**
```php
private $host = '127.0.0.1';
private $database = 'ValEduDatabase';
private $username = 'root';
private $password = ''; // Empty for XAMPP default
```

### Why This Approach Works for Learning

✅ **Simple to understand** - Easy to see how configuration values are used  
✅ **No dependencies** - No external libraries needed  
✅ **Clear flow** - Configuration → Connection → Usage  
✅ **Low barrier to entry** - Beginners can quickly modify settings

### Production Best Practice: Environment Variables (.env Files)

For production applications, you would use a `.env` file approach instead:

**Why `.env` files are better:**
- **Security** - Sensitive data never committed to Git
- **Environment separation** - Different settings for dev/test/production
- **Deployment flexibility** - Same code runs anywhere
- **Team collaboration** - Easy to share `.env.example` template

**How it would look:**
```bash
# .env file (never committed to Git)
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=ValEduDatabase
DB_USER=root
DB_PASSWORD=
DB_CHARSET=utf8mb4
```

**Code would use:**
```php
private $host = getenv('DB_HOST');
private $port = getenv('DB_PORT');
private $database = getenv('DB_NAME');
private $username = getenv('DB_USER');
private $password = getenv('DB_PASSWORD');
```

**Or with a library like `phpdotenv`:**
```php
require_once 'vendor/autoload.php';
$dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
$dotenv->load();

private $host = $_ENV['DB_HOST'];
private $database = $_ENV['DB_NAME'];
// ... etc
```

### Key Differences: Learning vs. Production

| Aspect | Learning Project (Current) | Production Project |
|--------|---------------------------|-------------------|
| Configuration | Hard-coded in PHP files | Environment variables (.env) |
| Credentials | Visible in code | Hidden from version control |
| Flexibility | Requires code changes | Change .env to switch environments |
| Security | Not secure | Secure with proper setup |
| Dependencies | None needed | May use library like `phpdotenv` |
| Team Setup | Copy code, modify Database.php | Copy code, create .env file |
| Deployment | Requires code modification | No code changes needed |
| Containerization | Difficult | Easy with Docker |

### Educational Path Forward

As you progress with this project:

1. **Now (Learning)**: Understand configuration in `Base/Database.php`
2. **Intermediate**: Learn about environment variables and `.env` files
3. **Advanced**: Implement `.env` file support using `phpdotenv` or similar
4. **Professional**: Implement full configuration management with multiple environments

### Example: Converting to `.env` (Future Enhancement)

If you want to implement `.env` support later, here's a basic example:

**1. Install dependency:**
```bash
composer require vlucas/phpdotenv
```

**2. Create `.env` file:**
```
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=ValEduDatabase
DB_USER=root
DB_PASSWORD=
DB_CHARSET=utf8mb4
```

**3. Create `.env.example` for Git:**
```
DB_HOST=localhost
DB_PORT=3306
DB_NAME=ValEduDatabase
DB_USER=root
DB_PASSWORD=
DB_CHARSET=utf8mb4
```

**4. Add `.env` to `.gitignore`:**
```
.env
.env.local
.env.*.php
```

**5. Update `Base/Database.php`:**
```php
<?php
require_once dirname(__DIR__) . '/../vendor/autoload.php';
$dotenv = Dotenv\Dotenv::createImmutable(dirname(__DIR__) . '/..');
$dotenv->load();

class Database {
    private static $instance = null;
    private $connection;
    
    private $host = '';
    private $port = 3306;
    private $database = '';
    private $username = '';
    private $password = '';
    
    private function __construct() {
        // Load from environment
        $this->host = $_ENV['DB_HOST'] ?? '127.0.0.1';
        $this->port = $_ENV['DB_PORT'] ?? 3306;
        $this->database = $_ENV['DB_NAME'] ?? 'ValEduDatabase';
        $this->username = $_ENV['DB_USER'] ?? 'root';
        $this->password = $_ENV['DB_PASSWORD'] ?? '';
        
        // ... rest of configuration
    }
}
```

### Summary

This project uses **hard-coded configuration for learning purposes**. Understanding how this works is valuable, but remember that **production applications should always use environment variables** to keep sensitive data secure and enable flexible deployments.

## 📁 Project Structure

```
VAL_EDU/
├── README.md                          # This file
├── TESTING_REFACTOR_SUMMARY.md        # Test organization guide
│
├── webapp/                            # Application root
│   ├── index.php                      # Main routing file
│   ├── .htaccess                      # Apache rewrite rules
│   │
│   ├── Base/                          # Base framework classes
│   │   ├── Database.php               # Database connection (Singleton)
│   │   ├── BaseController.php         # Base controller class
│   │   └── BaseModel.php              # Base model class
│   │
│   ├── Controller/                    # Application controllers
│   │   ├── HomeController.php
│   │   ├── AuthController.php         # Authentication
│   │   ├── AdminController.php        # Admin functions
│   │   ├── StudentController.php      # Student functions
│   │   ├── TutorController.php        # Tutor functions
│   │   └── ParentController.php       # Parent functions
│   │
│   ├── Model/                         # Data models
│   │   ├── UserModel.php              # User operations
│   │   ├── AdminModel.php             # Admin data
│   │   ├── StudentModel.php           # Student data
│   │   ├── TutorModel.php             # Tutor data
│   │   ├── ParentModel.php            # Parent data
│   │   └── HomeModel.php              # Home page data
│   │
│   ├── View/                          # HTML templates
│   │   ├── Auth/                      # Login/Register views
│   │   ├── Admin/                     # Admin panel views
│   │   ├── Student/                   # Student views
│   │   ├── Tutor/                     # Tutor views
│   │   ├── Parent/                    # Parent views
│   │   ├── Home/                      # Home page views
│   │   ├── Partial/                   # Reusable components
│   │   └── images/                    # Static images
│   │
│   ├── api/                           # API endpoints
│   │   └── student/                   # Student API routes
│   │
│   ├── routes/                        # URL routing
│   │   └── student.php                # Student routes
│   │
│   ├── tests/                         # Test suite (organized)
│   │   ├── unit/                      # Unit tests
│   │   ├── integration/               # Integration tests
│   │   ├── e2e/                       # End-to-end tests
│   │   ├── fixtures/                  # Test data
│   │   ├── scripts/                   # Utility scripts
│   │   └── archived/                  # Temporary debug files
│   │
│   ├── ValEduDatabase.sql             # Database schema
│   ├── PAYMENT_SYSTEM_DOCUMENTATION.md # Payment details
│   ├── USERNAME_ROUTING.md            # Routing reference
│   │
│   └── Utility Scripts                # Helper scripts
│       ├── generate_password.php      # Password generation
│       ├── create_
