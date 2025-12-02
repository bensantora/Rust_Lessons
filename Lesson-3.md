# Rust Programming Lesson 3: Floating-Point Numbers and Type Conversion

Welcome to Lesson 3! Now that you've mastered basic arithmetic with integers, let's explore floating-point numbers and type conversion in Rust. These concepts will expand your mathematical capabilities and help you work with more precise calculations.

## Program 1: Integer vs. Floating-Point Division

This program demonstrates the difference between integer division (which truncates) and floating-point division (which preserves decimal values).

```rust
fn main() {
    // Integer division (truncates)
    let int_a = 17;
    let int_b = 5;
    let int_result = int_a / int_b;
    
    // Floating-point division (preserves decimals)
    let float_a = 17.0;
    let float_b = 5.0;
    let float_result = float_a / float_b;
    
    println!("Integer division: {} ÷ {} = {}", int_a, int_b, int_result);
    println!("Floating-point division: {} ÷ {} = {}", float_a, float_b, float_result);
    
    // Using f64 for more precision
    let precise_a: f64 = 17.0;
    let precise_b: f64 = 5.0;
    let precise_result = precise_a / precise_b;
    
    println!("Precise floating-point division: {} ÷ {} = {}", precise_a, precise_b, precise_result);
}
```

### Key Concepts

- **Floating-Point Types**: Rust has two floating-point types: `f32` (32-bit) and `f64` (64-bit). `f64` is the default and provides more precision.
- **Type Annotations**: We can explicitly specify types using `: f64` syntax.
- **Integer vs. Floating-Point Division**: Integer division truncates the decimal part, while floating-point division preserves it.
- **Default Types**: When you write a number with a decimal point (like `17.0`), Rust infers it as `f64` by default.

### Output

```
Integer division: 17 ÷ 5 = 3
Floating-point division: 17 ÷ 5 = 3.4
Precise floating-point division: 17 ÷ 5 = 3.4
```

---

## Program 2: Type Conversion

This program demonstrates how to convert between different numeric types in Rust.

```rust
fn main() {
    let integer = 5;
    let float = 2.5;
    
    // This would cause an error:
    // let result = integer + float;
    
    // Correct way: convert integer to float
    let result = integer as f64 + float;
    println!("{} + {} = {}", integer, float, result);
    
    // Converting float to integer (truncates)
    let float_value = 3.7;
    let int_value = float_value as i32;
    println!("{} converted to integer is {}", float_value, int_value);
    
    // Converting between integer types
    let small_int: i16 = 100;
    let large_int: i64 = small_int as i64;
    println!("{} converted from i16 to i64 is {}", small_int, large_int);
    
    // Converting with potential data loss
    let big_int: i32 = 300;
    let small_int: i8 = big_int as i8; // i8 can only hold -128 to 127
    println!("{} converted to i8 is {} (data loss occurred)", big_int, small_int);
}
```

### Key Concepts

- **Type Casting**: Use the `as` keyword to convert between types.
- **Data Loss**: Be careful when converting to smaller types, as data may be lost.
- **Implicit vs. Explicit Conversion**: Rust requires explicit conversion between numeric types.
- **Truncation in Float to Int Conversion**: When converting a float to an integer, the decimal part is truncated.

### Output

```
5 + 2.5 = 7.5
3.7 converted to integer is 3
100 converted from i16 to i64 is 100
300 converted to i8 is 44 (data loss occurred)
```

---

## Program 3: Mathematical Functions

This program introduces some useful mathematical functions from Rust's standard library.

```rust
use std::f64::consts::PI;

fn main() {
    let radius = 5.0;
    
    // Calculating circle area
    let area = PI * radius * radius;
    println!("Circle with radius {} has area {:.2}", radius, area);
    
    // Square root
    let number = 25.0;
    let sqrt_result = number.sqrt();
    println!("Square root of {} is {}", number, sqrt_result);
    
    // Rounding
    let float_value = 3.7;
    println!("Rounding up {}: {}", float_value, float_value.ceil());
    println!("Rounding down {}: {}", float_value, float_value.floor());
    println!("Rounding {}: {}", float_value, float_value.round());
    
    // Power and absolute value
    let base = 2.0;
    let exponent = 3.0;
    let power_result = base.powf(exponent);
    println!("{} to the power of {} is {}", base, exponent, power_result);
    
    let negative_value = -4.5;
    println!("Absolute value of {} is {}", negative_value, negative_value.abs());
}
```

### Key Concepts

- **Importing Constants**: Use `use std::f64::consts::PI` to import mathematical constants.
- **String Formatting**: `{:.2}` formats a float to 2 decimal places.
- **Mathematical Methods**: Rust's floating-point types have methods like `sqrt()`, `ceil()`, `floor()`, `round()`, `powf()`, and `abs()`.
- **Floating-Point Methods**: These methods are available on both `f32` and `f64` types.

### Output

```
Circle with radius 5 has area 78.54
Square root of 25 is 5
Rounding up 3.7: 4
Rounding down 3.7: 3
Rounding 3.7: 4
2 to the power of 3 is 8
Absolute value of -4.5 is 4.5
```

---

## Program 4: Constants and Shadowing

This program demonstrates constants and variable shadowing in Rust.

```rust
const SECONDS_IN_MINUTE: u32 = 60;
const MINUTES_IN_HOUR: u32 = 60;

fn main() {
    // Using constants
    let hours = 2;
    let seconds = hours * MINUTES_IN_HOUR * SECONDS_IN_MINUTE;
    println!("{} hours = {} seconds", hours, seconds);
    
    // Variable shadowing
    let x = 5;
    println!("Initial value of x: {}", x);
    
    let x = x + 1;
    println!("After shadowing: {}", x);
    
    let x = x * 2;
    println!("After second shadowing: {}", x);
    
    // Shadowing with different types
    let spaces = "   ";
    println!("spaces as string: '{}'", spaces);
    
    let spaces = spaces.len();
    println!("spaces as number: {}", spaces);
    
    // Constants vs. immutable variables
    const MAX_POINTS: u32 = 100_000;
    let mut current_points = 50_000;
    
    println!("Current points: {}/{}", current_points, MAX_POINTS);
    
    // This would cause an error:
    // MAX_POINTS = 200_000;
    
    // But this is fine:
    current_points = 75_000;
    println!("Updated points: {}/{}", current_points, MAX_POINTS);
}
```

### Key Concepts

- **Constants**: Defined with `const` and must have an explicit type. Constants can't be changed.
- **Shadowing**: Declaring a new variable with the same name as a previous one. The new variable "shadows" the old one.
- **Shadowing with Different Types**: You can shadow a variable with a different type, which isn't possible with `mut`.
- **Naming Conventions**: Constants are typically named in `SCREAMING_SNAKE_CASE`.
- **Numeric Literals**: Underscores can be used in numeric literals for readability (e.g., `100_000`).

### Output

```
2 hours = 7200 seconds
Initial value of x: 5
After shadowing: 6
After second shadowing: 12
spaces as string: '   '
spaces as number: 3
Current points: 50000/100000
Updated points: 75000/100000
```

---

## Program 5: Interactive Calculator

This program creates an interactive calculator that takes user input and performs calculations.

```rust
use std::io;

fn main() {
    println!("Simple Calculator");
    println!("-----------------");
    
    // Get first number
    println!("Enter first number:");
    let mut num1_str = String::new();
    io::stdin().read_line(&mut num1_str)
        .expect("Failed to read line");
    
    let num1: f64 = match num1_str.trim().parse() {
        Ok(num) => num,
        Err(_) => {
            println!("Please enter a valid number!");
            return;
        }
    };
    
    // Get operation
    println!("Enter operation (+, -, *, /):");
    let mut operation = String::new();
    io::stdin().read_line(&mut operation)
        .expect("Failed to read line");
    
    // Get second number
    println!("Enter second number:");
    let mut num2_str = String::new();
    io::stdin().read_line(&mut num2_str)
        .expect("Failed to read line");
    
    let num2: f64 = match num2_str.trim().parse() {
        Ok(num) => num,
        Err(_) => {
            println!("Please enter a valid number!");
            return;
        }
    };
    
    // Perform calculation
    let result = match operation.trim() {
        "+" => num1 + num2,
        "-" => num1 - num2,
        "*" => num1 * num2,
        "/" => {
            if num2 == 0.0 {
                println!("Error: Division by zero!");
                return;
            }
            num1 / num2
        },
        _ => {
            println!("Invalid operation!");
            return;
        }
    };
    
    println!("Result: {:.2}", result);
}
```

### Key Concepts

- **Standard Library Input**: Using `std::io` to read user input.
- **String Parsing**: Converting strings to numbers using `parse()`.
- **Error Handling with match**: Handling potential parsing errors gracefully.
- **String Methods**: Using `trim()` to remove whitespace.
- **Pattern Matching**: Using `match` to determine which operation to perform.
- **Early Returns**: Using `return` to exit the function early when errors occur.

### Output (Example Interaction)

```
Simple Calculator
-----------------
Enter first number:
10.5
Enter operation (+, -, *, /):
*
Enter second number:
2
Result: 21.00
```

---

## Summary & Key Takeaways

You've now learned about floating-point numbers and type conversion in Rust:

- **Floating-Point Types**: `f32` and `f64` for decimal calculations, with `f64` being the default.
- **Type Conversion**: Using the `as` keyword to explicitly convert between types.
- **Mathematical Functions**: Using methods like `sqrt()`, `ceil()`, `floor()`, etc.
- **Constants**: Defining unchangeable values with `const`.
- **Shadowing**: Reusing variable names with different values or types.
- **User Input**: Reading and parsing user input for interactive programs.

### Important Points to Remember

- **Precision**: `f64` provides more precision than `f32` but uses more memory.
- **Type Safety**: Rust requires explicit conversion between types to prevent bugs.
- **Data Loss**: Be careful when converting to smaller types, as data may be lost.
- **Constants vs. Variables**: Constants are always immutable and must have a type annotation.
- **Error Handling**: Always handle potential errors when parsing user input.

### Try It Yourself! 🚀

1. **Modify Program 1**: Create a program that calculates the area and circumference of a circle with a user-provided radius.
2. **Extend Program 3**: Add more mathematical operations like trigonometric functions (sin, cos, tan).
3. **Enhance Program 5**: Add support for more operations like exponentiation (^) and modulo (%).
4. **Challenge**: Create a temperature converter that converts between Celsius, Fahrenheit, and Kelvin.

## Next Steps

In the next lesson, we'll explore:

- Control flow with `if`, `else if`, and `else`
- Loops with `loop`, `while`, and `for`
- Pattern matching with `match`
- Handling complex conditional logic

**Happy Coding! 🦀**
