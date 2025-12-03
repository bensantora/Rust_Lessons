# Rust Programming Lesson 7: Structs and Methods

Welcome to Lesson 7! Now that you understand ownership and borrowing, you're ready to create your own custom data types. Structs allow you to package related data together, and methods let you define behavior for those types. This is where you start building real, organized programs!

## Program 1: Defining and Using Structs

A struct (short for "structure") lets you create a custom type by grouping related values together. Think of it as a blueprint for your data.

```rust
// Define a struct
struct User {
    username: String,
    email: String,
    age: u32,
    active: bool,
}

fn main() {
    // Create an instance of the struct
    let user1 = User {
        username: String::from("rustacean42"),
        email: String::from("rustacean@example.com"),
        age: 25,
        active: true,
    };
    
    // Access struct fields with dot notation
    println!("Username: {}", user1.username);
    println!("Email: {}", user1.email);
    println!("Age: {}", user1.age);
    println!("Active: {}", user1.active);
    
    // Mutable struct
    let mut user2 = User {
        username: String::from("coder_jane"),
        email: String::from("jane@example.com"),
        age: 30,
        active: true,
    };
    
    // Modify a field
    user2.email = String::from("jane.new@example.com");
    println!("\nUpdated email: {}", user2.email);
    
    // Note: The entire instance must be mutable, not individual fields
}
```

### Key Concepts

- **Struct Definition**: Use the `struct` keyword followed by a name and field definitions in curly braces.
- **Field Syntax**: Each field has a name and a type, separated by a colon.
- **Creating Instances**: Use the struct name followed by curly braces with field values.
- **Field Access**: Use dot notation (`.`) to access struct fields.
- **Mutability**: To modify fields, the entire struct instance must be declared as `mut`.
- **Naming Convention**: Struct names use `PascalCase` (capitalize each word).

### Output

```
Username: rustacean42
Email: rustacean@example.com
Age: 25
Active: true

Updated email: jane.new@example.com
```

---

## Program 2: Struct Update Syntax and Functions

Rust provides convenient syntax for creating struct instances and working with them in functions.

```rust
struct Point {
    x: f64,
    y: f64,
}

fn main() {
    let point1 = Point { x: 3.0, y: 4.0 };
    
    // Struct update syntax - create a new instance using values from another
    let point2 = Point {
        x: 5.0,
        ..point1  // Use remaining fields from point1
    };
    
    println!("Point 1: ({}, {})", point1.x, point1.y);
    println!("Point 2: ({}, {})", point2.x, point2.y);
    
    // Using a function to create structs
    let point3 = create_point(10.0, 20.0);
    println!("Point 3: ({}, {})", point3.x, point3.y);
    
    // Field init shorthand
    let x = 7.0;
    let y = 8.0;
    let point4 = Point { x, y }; // Shorthand when variable names match field names
    println!("Point 4: ({}, {})", point4.x, point4.y);
    
    // Passing structs to functions
    let distance = calculate_distance(point1, point3);
    println!("Distance: {:.2}", distance);
    
    // Note: point1 and point3 are no longer valid (moved into function)
}

fn create_point(x: f64, y: f64) -> Point {
    Point { x, y } // Field init shorthand
}

fn calculate_distance(p1: Point, p2: Point) -> f64 {
    let dx = p2.x - p1.x;
    let dy = p2.y - p1.y;
    (dx * dx + dy * dy).sqrt()
}
```

### Key Concepts

- **Struct Update Syntax**: `..other_struct` copies remaining fields from another instance.
- **Field Init Shorthand**: When variable names match field names, you can write `Point { x, y }` instead of `Point { x: x, y: y }`.
- **Constructor Functions**: Functions that return struct instances are common (though not special in Rust).
- **Ownership with Structs**: Passing a struct to a function moves ownership (unless it implements `Copy`).
- **Copy Trait**: Structs containing only `Copy` types can derive `Copy`, but those with `String` cannot.

### Output

```
Point 1: (3, 4)
Point 2: (5, 4)
Point 3: (10, 20)
Point 4: (7, 8)
Distance: 16.76
```

---

## Program 3: Methods - Adding Behavior to Structs

Methods are functions defined within the context of a struct. They always take `self` as their first parameter.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // Method that borrows self immutably
    fn area(&self) -> u32 {
        self.width * self.height
    }
    
    // Method that borrows self immutably
    fn perimeter(&self) -> u32 {
        2 * (self.width + self.height)
    }
    
    // Method that takes another Rectangle as a parameter
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
    
    // Method that borrows self mutably
    fn scale(&mut self, factor: u32) {
        self.width *= factor;
        self.height *= factor;
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    
    println!("Rectangle: {} x {}", rect1.width, rect1.height);
    println!("Area: {}", rect1.area());
    println!("Perimeter: {}", rect1.perimeter());
    
    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };
    
    let rect3 = Rectangle {
        width: 60,
        height: 45,
    };
    
    println!("\nCan rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
    
    // Using a mutable method
    let mut rect4 = Rectangle {
        width: 5,
        height: 10,
    };
    
    println!("\nBefore scaling: {} x {}", rect4.width, rect4.height);
    rect4.scale(3);
    println!("After scaling by 3: {} x {}", rect4.width, rect4.height);
}
```

### Key Concepts

- **impl Block**: Use `impl StructName` to define methods for a struct.
- **&self Parameter**: Methods take `&self` (immutable borrow), `&mut self` (mutable borrow), or `self` (takes ownership).
- **Method Syntax**: Call methods with dot notation: `rect.area()`.
- **Automatic Referencing**: Rust automatically adds `&`, `&mut`, or `*` to match the method signature.
- **Multiple Parameters**: Methods can take additional parameters after `self`.
- **Multiple impl Blocks**: You can have multiple `impl` blocks for the same struct (though usually one is enough).

### Output

```
Rectangle: 30 x 50
Area: 1500
Perimeter: 160

Can rect1 hold rect2? true
Can rect1 hold rect3? false

Before scaling: 5 x 10
After scaling by 3: 15 x 30
```

---

## Program 4: Associated Functions

Associated functions are defined in an `impl` block but don't take `self` as a parameter. They're often used as constructors.

```rust
struct Circle {
    radius: f64,
}

impl Circle {
    // Associated function (constructor)
    fn new(radius: f64) -> Circle {
        Circle { radius }
    }
    
    // Another associated function
    fn unit_circle() -> Circle {
        Circle { radius: 1.0 }
    }
    
    // Method
    fn area(&self) -> f64 {
        std::f64::consts::PI * self.radius * self.radius
    }
    
    // Method
    fn circumference(&self) -> f64 {
        2.0 * std::f64::consts::PI * self.radius
    }
    
    // Method that consumes self and returns a new Circle
    fn resize(self, new_radius: f64) -> Circle {
        Circle { radius: new_radius }
    }
}

fn main() {
    // Using associated functions (called with ::)
    let circle1 = Circle::new(5.0);
    let circle2 = Circle::unit_circle();
    
    println!("Circle 1 - Radius: {}", circle1.radius);
    println!("  Area: {:.2}", circle1.area());
    println!("  Circumference: {:.2}", circle1.circumference());
    
    println!("\nCircle 2 - Radius: {}", circle2.radius);
    println!("  Area: {:.2}", circle2.area());
    
    // Using a consuming method
    let circle3 = Circle::new(3.0);
    let circle4 = circle3.resize(7.0);
    
    // circle3 is no longer valid (moved into resize)
    println!("\nResized circle - Radius: {}", circle4.radius);
    println!("  Area: {:.2}", circle4.area());
}
```

### Key Concepts

- **Associated Functions**: Functions in an `impl` block that don't take `self`.
- **Calling Syntax**: Use `::` to call associated functions: `Circle::new(5.0)`.
- **Constructor Pattern**: `new` is a common name for constructor functions (but not enforced by Rust).
- **Consuming Methods**: Methods that take `self` (not `&self`) consume the instance.
- **Namespacing**: Associated functions are namespaced under the struct name.

### Output

```
Circle 1 - Radius: 5
  Area: 78.54
  Circumference: 31.42

Circle 2 - Radius: 1
  Area: 3.14

Resized circle - Radius: 7
  Area: 153.94
```

---

## Program 5: Tuple Structs and Unit-Like Structs

Rust has two special kinds of structs for specific use cases.

```rust
// Tuple struct - fields have no names
struct Color(u8, u8, u8);
struct Point3D(f64, f64, f64);

// Unit-like struct - no fields at all
struct AlwaysEqual;

fn main() {
    // Creating tuple structs
    let black = Color(0, 0, 0);
    let white = Color(255, 255, 255);
    let red = Color(255, 0, 0);
    
    // Accessing tuple struct fields by index
    println!("Red color: RGB({}, {}, {})", red.0, red.1, red.2);
    
    // Destructuring tuple structs
    let Color(r, g, b) = white;
    println!("White color: RGB({}, {}, {})", r, g, b);
    
    // Tuple structs create distinct types
    let origin = Point3D(0.0, 0.0, 0.0);
    let point = Point3D(3.5, 4.2, 1.8);
    
    println!("Point: ({}, {}, {})", point.0, point.1, point.2);
    
    // Unit-like struct
    let subject = AlwaysEqual;
    
    // Useful for implementing traits without data
    println!("Unit-like struct created");
}

// You can implement methods for tuple structs too
impl Color {
    fn new(r: u8, g: u8, b: u8) -> Color {
        Color(r, g, b)
    }
    
    fn is_grayscale(&self) -> bool {
        self.0 == self.1 && self.1 == self.2
    }
}

impl Point3D {
    fn distance_from_origin(&self) -> f64 {
        (self.0 * self.0 + self.1 * self.1 + self.2 * self.2).sqrt()
    }
}
```

### Key Concepts

- **Tuple Structs**: Structs with unnamed fields, accessed by index like tuples.
- **Type Distinction**: `Color(0, 0, 0)` and `Point3D(0.0, 0.0, 0.0)` are different types, even with similar data.
- **Destructuring**: You can destructure tuple structs just like regular tuples.
- **Unit-Like Structs**: Structs with no fields, useful for implementing traits.
- **Use Cases**: Tuple structs are good for simple wrappers; unit-like structs for marker types.

### Output

```
Red color: RGB(255, 0, 0)
White color: RGB(255, 255, 255)
Point: (3.5, 4.2, 1.8)
Unit-like struct created
```

---

## Program 6: Practical Example - A Simple Bank Account

Let's put everything together with a realistic example that demonstrates structs, methods, and associated functions.

```rust
struct BankAccount {
    account_number: String,
    holder_name: String,
    balance: f64,
}

impl BankAccount {
    // Associated function - constructor
    fn new(account_number: String, holder_name: String) -> BankAccount {
        BankAccount {
            account_number,
            holder_name,
            balance: 0.0,
        }
    }
    
    // Associated function - constructor with initial deposit
    fn with_balance(account_number: String, holder_name: String, initial_balance: f64) -> BankAccount {
        BankAccount {
            account_number,
            holder_name,
            balance: initial_balance,
        }
    }
    
    // Method - deposit money
    fn deposit(&mut self, amount: f64) {
        if amount > 0.0 {
            self.balance += amount;
            println!("Deposited ${:.2}. New balance: ${:.2}", amount, self.balance);
        } else {
            println!("Invalid deposit amount");
        }
    }
    
    // Method - withdraw money
    fn withdraw(&mut self, amount: f64) -> bool {
        if amount > 0.0 && amount <= self.balance {
            self.balance -= amount;
            println!("Withdrew ${:.2}. New balance: ${:.2}", amount, self.balance);
            true
        } else {
            println!("Insufficient funds or invalid amount");
            false
        }
    }
    
    // Method - check balance
    fn check_balance(&self) {
        println!("Account {}: ${:.2}", self.account_number, self.balance);
    }
    
    // Method - display account info
    fn display_info(&self) {
        println!("─────────────────────────────");
        println!("Account Number: {}", self.account_number);
        println!("Holder: {}", self.holder_name);
        println!("Balance: ${:.2}", self.balance);
        println!("─────────────────────────────");
    }
}

fn main() {
    // Create accounts using associated functions
    let mut account1 = BankAccount::new(
        String::from("ACC001"),
        String::from("Alice Johnson")
    );
    
    let mut account2 = BankAccount::with_balance(
        String::from("ACC002"),
        String::from("Bob Smith"),
        1000.0
    );
    
    // Display initial state
    account1.display_info();
    account2.display_info();
    
    // Perform transactions on account1
    println!("\n--- Account 1 Transactions ---");
    account1.deposit(500.0);
    account1.deposit(250.0);
    account1.withdraw(100.0);
    account1.check_balance();
    
    // Perform transactions on account2
    println!("\n--- Account 2 Transactions ---");
    account2.withdraw(300.0);
    account2.deposit(150.0);
    account2.withdraw(2000.0); // Should fail
    account2.check_balance();
    
    // Final state
    println!("\n--- Final Account States ---");
    account1.display_info();
    account2.display_info();
}
```

### Key Concepts

- **Real-World Modeling**: Structs model real-world entities with data and behavior.
- **Encapsulation**: Methods provide a clean interface for interacting with struct data.
- **Multiple Constructors**: Associated functions can provide different ways to create instances.
- **Validation Logic**: Methods can include validation (like checking for sufficient funds).
- **State Management**: Mutable methods (`&mut self`) modify the struct's state.
- **Information Hiding**: Users interact through methods, not direct field access (though Rust doesn't enforce privacy here yet).

### Output

```
─────────────────────────────
Account Number: ACC001
Holder: Alice Johnson
Balance: $0.00
─────────────────────────────
─────────────────────────────
Account Number: ACC002
Holder: Bob Smith
Balance: $1000.00
─────────────────────────────

--- Account 1 Transactions ---
Deposited $500.00. New balance: $500.00
Deposited $250.00. New balance: $750.00
Withdrew $100.00. New balance: $650.00
Account ACC001: $650.00

--- Account 2 Transactions ---
Withdrew $300.00. New balance: $700.00
Deposited $150.00. New balance: $850.00
Insufficient funds or invalid amount
Account ACC002: $850.00

--- Final Account States ---
─────────────────────────────
Account Number: ACC001
Holder: Alice Johnson
Balance: $650.00
─────────────────────────────
─────────────────────────────
Account Number: ACC002
Holder: Bob Smith
Balance: $850.00
─────────────────────────────
```

---

## Summary & Key Takeaways

You've now learned how to create custom types and add behavior to them:

- **Structs**: Group related data into custom types with named fields.
- **Methods**: Functions that operate on struct instances, taking `self` as the first parameter.
- **Associated Functions**: Functions in an `impl` block that don't take `self`, often used as constructors.
- **Tuple Structs**: Structs with unnamed fields, accessed by index.
- **Unit-Like Structs**: Structs with no fields, useful for trait implementations.

### The Three Forms of self

- **`&self`**: Borrows the instance immutably (read-only access)
- **`&mut self`**: Borrows the instance mutably (can modify fields)
- **`self`**: Takes ownership of the instance (consumes it)

### Best Practices

- Use `new` as a convention for constructor associated functions
- Use `&self` by default; only use `&mut self` when you need to modify
- Rarely use `self` (consuming methods) - only when transforming the instance
- Group related functionality in `impl` blocks
- Use tuple structs for simple wrappers around single values

### Try It Yourself! 🚀

1. **Modify Program 3**: Add a method to `Rectangle` that returns `true` if it's a square.
2. **Extend Program 4**: Add a `scale` method to `Circle` that takes a factor and returns a new `Circle` with the scaled radius.
3. **Enhance Program 6**: Add a `transfer` function that takes two mutable references to `BankAccount` and transfers money between them.
4. **Challenge**: Create a `Book` struct with fields for title, author, pages, and available. Implement methods for `checkout()`, `return_book()`, and `display_info()`. Create a small library system with multiple books.

## Next Steps

In the next lesson, we'll explore:

- Enums - Types that can be one of several variants
- The `Option` enum for handling absence of values
- The `Result` enum for error handling
- Pattern matching with enums

**Happy Coding! 🦀**
