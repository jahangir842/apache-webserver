The PHP MySQLi Database Class is a convenient class for simplifying database operations in PHP using MySQLi (MySQL Improved Extension). It helps with basic database operations like connecting, querying, and managing results. Here's a simple example of how you might use such a class:

### PHP MySQLi Database Class Example

#### 1. Database Class Definition

```php
<?php
class Database {
    private $host = "localhost";
    private $username = "root";
    private $password = "";
    private $database = "test";
    private $conn;

    // Constructor to initialize the connection
    public function __construct() {
        $this->connect();
    }

    // Method to connect to the database
    private function connect() {
        $this->conn = new mysqli($this->host, $this->username, $this->password, $this->database);

        // Check connection
        if ($this->conn->connect_error) {
            die("Connection failed: " . $this->conn->connect_error);
        }
    }

    // Method to execute a query
    public function query($sql) {
        $result = $this->conn->query($sql);
        if ($this->conn->error) {
            die("Query failed: " . $this->conn->error);
        }
        return $result;
    }

    // Method to fetch all rows from a query
    public function fetchAll($sql) {
        $result = $this->query($sql);
        return $result->fetch_all(MYSQLI_ASSOC);
    }

    // Method to fetch a single row from a query
    public function fetchOne($sql) {
        $result = $this->query($sql);
        return $result->fetch_assoc();
    }

    // Method to escape strings for safe queries
    public function escape($string) {
        return $this->conn->real_escape_string($string);
    }

    // Destructor to close the connection
    public function __destruct() {
        $this->conn->close();
    }
}
?>
```

#### 2. Using the Database Class

```php
<?php
// Include the database class file
require_once 'Database.php';

// Create an instance of the Database class
$db = new Database();

// Fetch all rows from a table
$rows = $db->fetchAll("SELECT * FROM users");
print_r($rows);

// Fetch a single row from a table
$row = $db->fetchOne("SELECT * FROM users WHERE id = 1");
print_r($row);

// Insert a new record
$name = $db->escape("John Doe");
$email = $db->escape("john.doe@example.com");
$db->query("INSERT INTO users (name, email) VALUES ('$name', '$email')");

// Update a record
$db->query("UPDATE users SET email = 'new.email@example.com' WHERE id = 1");

// Delete a record
$db->query("DELETE FROM users WHERE id = 2");
?>
```

### Key Features
- **Connection Management**: Handles database connection and disconnection.
- **Query Execution**: Executes SQL queries and handles errors.
- **Data Fetching**: Provides methods to fetch all rows or a single row from query results.
- **String Escaping**: Escapes strings to prevent SQL injection.
  
### Notes
- **Error Handling**: Ensure to handle errors properly to avoid exposing sensitive information.
- **Prepared Statements**: For more secure queries, especially with user input, consider using prepared statements to prevent SQL injection.

This basic class can be extended with additional methods as needed for more complex database operations.
