# Rust Programming Lesson 10: Error Handling and Best Practices

Welcome to Lesson 10, the final lesson in this course! You've learned the fundamentals of Rust. Now let's master error handling and explore best practices that will help you write robust, production-ready code.

## Program 1: Understanding panic!

Sometimes errors are unrecoverable. The `panic!` macro stops execution immediately.

```rust
fn main() {
    println!("Starting program...");
    
    // Explicit panic
    // panic!("This is a critical error!");
    
    // Panics from invalid operations
    let v = vec![1, 2, 3];
    // v[99]; // This would panic: index out of bounds
    
    // Division by zero panics
    // let x = 5 / 0; // This would panic
    
    // Using unwrap (panics if None or Err)
    let some_value = Some(42);
    let value = some_value.unwrap();
    println!("Value: {}", value);
    
    // This would panic:
    // let none_value: Option<i32> = None;
    // none_value.unwrap();
    
    // Using expect for better error messages
    let result = Some(100);
    let num = result.expect("Expected a number but got None");
    println!("Number: {}", num);
    
    println!("Program completed successfully");
}
```

### Key Concepts

- **panic!**: Unrecoverable errors that stop execution
- **When to panic**: Programming errors, invalid state, or impossible situations
- **unwrap()**: Gets value or panics
- **expect()**: Like unwrap but with custom message
- **Backtrace**: Set `RUST_BACKTRACE=1` to see where panic occurred

### Output
```
Starting program...
Value: 42
Number: 100
Program completed successfully
```

---

## Program 2: The ? Operator - Error Propagation

The `?` operator makes error handling concise by propagating errors up the call stack.

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut file = File::open("username.txt")?; // Returns error if open fails
    let mut username = String::new();
    file.read_to_string(&mut username)?; // Returns error if read fails
    Ok(username)
}

fn read_number_from_string(s: &str) -> Result<i32, std::num::ParseIntError> {
    let num = s.trim().parse::<i32>()?;
    Ok(num)
}

fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Division by zero"))
    } else {
        Ok(a / b)
    }
}

fn calculate() -> Result<f64, String> {
    let x = divide(10.0, 2.0)?;
    let y = divide(x, 0.0)?; // This will return the error
    Ok(y)
}

fn main() {
    // Using ? in a function that returns Result
    match read_number_from_string("42") {
        Ok(num) => println!("Parsed number: {}", num),
        Err(e) => println!("Parse error: {}", e),
    }
    
    match read_number_from_string("not a number") {
        Ok(num) => println!("Parsed: {}", num),
        Err(e) => println!("Parse error: {}", e),
    }
    
    // Error propagation example
    match calculate() {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Calculation error: {}", e),
    }
}
```

### Key Concepts

- **? Operator**: Returns error early if Result is Err
- **Error Propagation**: Passes errors up to caller
- **Only in Result-returning functions**: Can't use ? in main (unless main returns Result)
- **Cleaner Code**: Replaces verbose match statements
- **Early Return**: Automatically returns on error

### Output
```
Parsed number: 42
Parse error: invalid digit found in string
Calculation error: Division by zero
```

---

## Program 3: Custom Error Types

Create your own error types for domain-specific errors.

```rust
use std::fmt;

#[derive(Debug)]
enum MathError {
    DivisionByZero,
    NegativeSquareRoot,
    Overflow,
}

impl fmt::Display for MathError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match self {
            MathError::DivisionByZero => write!(f, "Cannot divide by zero"),
            MathError::NegativeSquareRoot => write!(f, "Cannot take square root of negative number"),
            MathError::Overflow => write!(f, "Calculation resulted in overflow"),
        }
    }
}

fn safe_divide(a: f64, b: f64) -> Result<f64, MathError> {
    if b == 0.0 {
        Err(MathError::DivisionByZero)
    } else {
        Ok(a / b)
    }
}

fn safe_sqrt(x: f64) -> Result<f64, MathError> {
    if x < 0.0 {
        Err(MathError::NegativeSquareRoot)
    } else {
        Ok(x.sqrt())
    }
}

fn complex_calculation(a: f64, b: f64) -> Result<f64, MathError> {
    let quotient = safe_divide(a, b)?;
    let result = safe_sqrt(quotient)?;
    Ok(result)
}

fn main() {
    println!("=== Safe Math Operations ===\n");
    
    match safe_divide(10.0, 2.0) {
        Ok(result) => println!("10 / 2 = {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    match safe_divide(10.0, 0.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    match safe_sqrt(16.0) {
        Ok(result) => println!("√16 = {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    match safe_sqrt(-4.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    println!("\n=== Complex Calculation ===");
    match complex_calculation(16.0, 4.0) {
        Ok(result) => println!("√(16/4) = {}", result),
        Err(e) => println!("Error: {}", e),
    }
    
    match complex_calculation(16.0, 0.0) {
        Ok(result) => println!("Result: {}", result),
        Err(e) => println!("Error: {}", e),
    }
}
```

### Key Concepts

- **Custom Enums**: Define domain-specific errors
- **Display Trait**: Implement for user-friendly messages
- **Type Safety**: Different error types for different contexts
- **Composability**: Use ? with custom errors

### Output
```
=== Safe Math Operations ===

10 / 2 = 5
Error: Cannot divide by zero
√16 = 4
Error: Cannot take square root of negative number

=== Complex Calculation ===
√(16/4) = 2
Error: Cannot divide by zero
```

---

## Program 4: Combining Option and Result

Real programs often use both Option and Result together.

```rust
fn find_user(id: u32) -> Option<String> {
    match id {
        1 => Some(String::from("Alice")),
        2 => Some(String::from("Bob")),
        _ => None,
    }
}

fn get_user_age(name: &str) -> Result<u32, String> {
    match name {
        "Alice" => Ok(25),
        "Bob" => Ok(30),
        _ => Err(String::from("User not found in age database")),
    }
}

fn get_user_info(id: u32) -> Result<(String, u32), String> {
    let name = find_user(id)
        .ok_or(String::from("User ID not found"))?;
    
    let age = get_user_age(&name)?;
    
    Ok((name, age))
}

fn main() {
    println!("=== User Lookup System ===\n");
    
    match get_user_info(1) {
        Ok((name, age)) => println!("User 1: {} (age {})", name, age),
        Err(e) => println!("Error: {}", e),
    }
    
    match get_user_info(2) {
        Ok((name, age)) => println!("User 2: {} (age {})", name, age),
        Err(e) => println!("Error: {}", e),
    }
    
    match get_user_info(99) {
        Ok((name, age)) => println!("User: {} (age {})", name, age),
        Err(e) => println!("Error: {}", e),
    }
    
    // Using unwrap_or for defaults
    let user = find_user(5).unwrap_or(String::from("Guest"));
    println!("\nUser 5 or default: {}", user);
    
    // Using and_then for chaining Options
    let result = find_user(1)
        .and_then(|name| Some(name.to_uppercase()));
    println!("Uppercase name: {:?}", result);
}
```

### Key Concepts

- **ok_or()**: Converts Option to Result
- **Chaining**: Combine multiple fallible operations
- **unwrap_or()**: Provide default values
- **and_then()**: Chain operations on Option/Result

### Output
```
=== User Lookup System ===

User 1: Alice (age 25)
User 2: Bob (age 30)
Error: User ID not found

User 5 or default: Guest
Uppercase name: Some("ALICE")
```

---

## Program 5: Best Practices - A Complete Example

Let's build a configuration file parser demonstrating best practices.

```rust
use std::collections::HashMap;

#[derive(Debug)]
enum ConfigError {
    MissingField(String),
    InvalidValue(String),
    ParseError(String),
}

impl std::fmt::Display for ConfigError {
    fn fmt(&self, f: &mut std::fmt::Formatter) -> std::fmt::Result {
        match self {
            ConfigError::MissingField(field) => write!(f, "Missing required field: {}", field),
            ConfigError::InvalidValue(msg) => write!(f, "Invalid value: {}", msg),
            ConfigError::ParseError(msg) => write!(f, "Parse error: {}", msg),
        }
    }
}

struct Config {
    host: String,
    port: u16,
    debug: bool,
}

impl Config {
    fn new(host: String, port: u16, debug: bool) -> Config {
        Config { host, port, debug }
    }
    
    fn from_map(map: &HashMap<String, String>) -> Result<Config, ConfigError> {
        let host = map.get("host")
            .ok_or(ConfigError::MissingField(String::from("host")))?
            .clone();
        
        let port_str = map.get("port")
            .ok_or(ConfigError::MissingField(String::from("port")))?;
        
        let port = port_str.parse::<u16>()
            .map_err(|_| ConfigError::ParseError(format!("Invalid port: {}", port_str)))?;
        
        if port < 1024 {
            return Err(ConfigError::InvalidValue(
                String::from("Port must be >= 1024")
            ));
        }
        
        let debug = map.get("debug")
            .map(|s| s == "true")
            .unwrap_or(false);
        
        Ok(Config::new(host, port, debug))
    }
    
    fn display(&self) {
        println!("Configuration:");
        println!("  Host: {}", self.host);
        println!("  Port: {}", self.port);
        println!("  Debug: {}", self.debug);
    }
}

fn main() {
    println!("=== Configuration Parser ===\n");
    
    // Valid configuration
    let mut config1 = HashMap::new();
    config1.insert(String::from("host"), String::from("localhost"));
    config1.insert(String::from("port"), String::from("8080"));
    config1.insert(String::from("debug"), String::from("true"));
    
    match Config::from_map(&config1) {
        Ok(config) => config.display(),
        Err(e) => println!("Error: {}", e),
    }
    
    // Missing field
    println!("\n--- Missing field test ---");
    let mut config2 = HashMap::new();
    config2.insert(String::from("host"), String::from("localhost"));
    
    match Config::from_map(&config2) {
        Ok(config) => config.display(),
        Err(e) => println!("Error: {}", e),
    }
    
    // Invalid port
    println!("\n--- Invalid port test ---");
    let mut config3 = HashMap::new();
    config3.insert(String::from("host"), String::from("localhost"));
    config3.insert(String::from("port"), String::from("500"));
    
    match Config::from_map(&config3) {
        Ok(config) => config.display(),
        Err(e) => println!("Error: {}", e),
    }
}
```

### Key Concepts

- **Validation**: Check values before creating structs
- **map_err()**: Convert one error type to another
- **Builder Pattern**: Construct complex objects safely
- **Descriptive Errors**: Help users fix problems
- **Defensive Programming**: Validate all inputs

### Output
```
=== Configuration Parser ===

Configuration:
  Host: localhost
  Port: 8080
  Debug: true

--- Missing field test ---
Error: Missing required field: port

--- Invalid port test ---
Error: Invalid value: Port must be >= 1024
```

---

## Summary & Course Completion 🎓

Congratulations! You've completed all 10 lessons of Rust programming. Let's review what you've learned:

### Course Overview

1. **Lesson 1**: Basics - variables, functions, control flow
2. **Lesson 2**: Integer arithmetic operations
3. **Lesson 3**: Floating-point numbers and type conversion
4. **Lesson 4**: Control flow with if, else, and loops
5. **Lesson 5**: Advanced loops, arrays, tuples, and match
6. **Lesson 6**: Ownership, borrowing, and references
7. **Lesson 7**: Structs and methods
8. **Lesson 8**: Enums and pattern matching
9. **Lesson 9**: Collections (Vec, String, HashMap)
10. **Lesson 10**: Error handling and best practices

### Error Handling Guidelines

- **Use Result<T, E>** for recoverable errors
- **Use Option<T>** for optional values
- **Use panic!** only for unrecoverable errors
- **Use ?** operator for clean error propagation
- **Create custom error types** for domain-specific errors
- **Validate inputs** and provide helpful error messages

### Rust Best Practices

1. **Prefer immutability**: Use `let` by default, `mut` only when needed
2. **Use the type system**: Let the compiler catch bugs
3. **Handle all cases**: Use `match` exhaustively
4. **Avoid unwrap()**: Use `?`, `unwrap_or()`, or proper error handling
5. **Write tests**: Rust has built-in testing support
6. **Use clippy**: Rust's linter for best practices
7. **Read compiler messages**: They're incredibly helpful!

### What's Next?

You now have a solid foundation in Rust! Here are suggested next steps:

1. **Build Projects**: Create CLI tools, web servers, or games
2. **Learn Advanced Topics**: Lifetimes, traits, generics, async/await
3. **Read The Book**: "The Rust Programming Language" (official docs)
4. **Explore Crates**: Use crates.io to find libraries
5. **Join the Community**: Rust forums, Discord, Reddit
6. **Contribute**: Open source Rust projects welcome beginners

### Final Challenge 🚀

Build a complete application combining everything you've learned:

**Project: Task Manager CLI**
- Use structs for Task (id, description, completed)
- Use Vec to store tasks
- Use HashMap for categories
- Use Result for error handling
- Implement add, remove, list, and complete commands
- Save/load tasks from a file

### Thank You!

You've worked through 10 comprehensive lessons covering Rust fundamentals. The journey from "Hello, World!" to error handling and best practices is significant. Keep practicing, keep building, and most importantly, keep having fun with Rust!

**Happy Coding, Rustacean! 🦀**

---

*"Rust empowers everyone to build reliable and efficient software."*
