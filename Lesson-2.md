# Rust Programming Lesson 2: Arithmetic Operations with Integers

Welcome to Lesson 2! In this lesson, we'll explore the fundamental arithmetic operations in Rust: addition, subtraction, multiplication, and division. These are the building blocks of mathematical computation in programming.

Rust provides powerful type safety for numeric operations, helping you avoid common bugs. Let's dive into five programs that demonstrate these concepts clearly.

---

## Program 1: Adding Two Integers ➕

This program demonstrates the simplest arithmetic operation: addition. We'll add two integers and display the result.

```rust
fn main() {
    let num1 = 15;
    let num2 = 27;
    let sum = num1 + num2;
    
    println!("{} + {} = {}", num1, num2, sum);
}
```

### Key Concepts

1.  **Integer Type Inference**: When you write `let num1 = 15;` without a decimal point, Rust infers the type as `i32` (a 32-bit signed integer). This is Rust's default integer type.
2.  **The `+` Operator**: The addition operator works exactly as you'd expect. It takes two values of the same numeric type and produces their sum.
3.  **Variable Immutability**: All three variables (`num1`, `num2`, and `sum`) are immutable by default. Once assigned, their values cannot change.
4.  **String Formatting**: The `println!` macro uses `{}` placeholders to insert values into the output string in order.

### Output
```
15 + 27 = 42
```

---

## Program 2: Subtracting Two Integers ➖

This program shows subtraction and introduces the concept of negative results.

```rust
fn main() {
    let num1 = 50;
    let num2 = 23;
    let difference = num1 - num2;
    
    println!("{} - {} = {}", num1, num2, difference);
    
    // Demonstrating negative results
    let num3 = 10;
    let num4 = 25;
    let negative_result = num3 - num4;
    
    println!("{} - {} = {}", num3, num4, negative_result);
}
```

### Key Concepts

1.  **The `-` Operator**: Subtracts the second operand from the first.
2.  **Signed Integers**: The `i32` type can hold both positive and negative numbers. The range is approximately -2.1 billion to +2.1 billion.
3.  **Comments**: Lines starting with `//` are comments. They're ignored by the compiler and help explain your code to humans.
4.  **Multiple Operations**: You can perform multiple calculations in the same program. Each operation is independent.

### Output
```
50 - 23 = 27
10 - 25 = -15
```

---

## Program 3: Multiplying Two Integers ✖️

This program demonstrates multiplication and shows how quickly numbers can grow.

```rust
fn main() {
    let num1 = 12;
    let num2 = 8;
    let product = num1 * num2;
    
    println!("{} × {} = {}", num1, num2, product);
    
    // Multiplying by zero
    let num3 = 42;
    let num4 = 0;
    let zero_product = num3 * num4;
    
    println!("{} × {} = {}", num3, num4, zero_product);
}
```

### Key Concepts

1.  **The `*` Operator**: The asterisk performs multiplication. Note that we use `*` in code, but can display `×` in output using Unicode characters.
2.  **Multiplication by Zero**: Any number multiplied by zero equals zero. This is a fundamental mathematical property.
3.  **Integer Overflow**: Be careful! If the result exceeds the maximum value for `i32` (about 2.1 billion), Rust will panic in debug mode or wrap around in release mode. For larger numbers, use `i64` or `i128`.

### Output
```
12 × 8 = 96
42 × 0 = 0
```

---

## Program 4: Dividing Two Integers ➗

This program shows integer division and introduces an important concept: integer division truncates (drops) the decimal part.

```rust
fn main() {
    let num1 = 20;
    let num2 = 4;
    let quotient = num1 / num2;
    
    println!("{} ÷ {} = {}", num1, num2, quotient);
    
    // Integer division truncates
    let num3 = 17;
    let num4 = 5;
    let truncated_quotient = num3 / num4;
    
    println!("{} ÷ {} = {} (truncated)", num3, num4, truncated_quotient);
    
    // Getting the remainder with modulo
    let remainder = num3 % num4;
    println!("{} ÷ {} has remainder {}", num3, num4, remainder);
}
```

### Key Concepts

1.  **The `/` Operator**: Performs division. When both operands are integers, this is **integer division**.
2.  **Integer Division Truncates**: `17 / 5` equals `3`, not `3.4`. The decimal part is discarded (not rounded).
3.  **The `%` Operator (Modulo)**: Returns the remainder after division. `17 % 5` equals `2` because 17 = (5 × 3) + 2.
4.  **Division by Zero**: Attempting to divide by zero will cause your program to panic (crash). Always validate your divisor is not zero in real applications.

### Output
```
20 ÷ 4 = 5
17 ÷ 5 = 3 (truncated)
17 ÷ 5 has remainder 2
```

---

## Program 5: Calculator - All Operations Combined 🧮

This program creates a simple calculator function that performs all four basic operations on two integers.

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}

fn subtract(a: i32, b: i32) -> i32 {
    a - b
}

fn multiply(a: i32, b: i32) -> i32 {
    a * b
}

fn divide(a: i32, b: i32) -> i32 {
    if b == 0 {
        println!("Error: Cannot divide by zero!");
        return 0;
    }
    a / b
}

fn main() {
    let x = 24;
    let y = 6;
    
    println!("Calculator for {} and {}", x, y);
    println!("─────────────────────────");
    println!("Addition:       {} + {} = {}", x, y, add(x, y));
    println!("Subtraction:    {} - {} = {}", x, y, subtract(x, y));
    println!("Multiplication: {} × {} = {}", x, y, multiply(x, y));
    println!("Division:       {} ÷ {} = {}", x, y, divide(x, y));
    
    // Test division by zero protection
    println!("\nTesting division by zero:");
    let result = divide(10, 0);
    println!("Result: {}", result);
}
```

### Key Concepts

1.  **Function Parameters**: Each function takes two parameters of type `i32` and returns an `i32`.
    *   `fn add(a: i32, b: i32) -> i32`: This signature tells us the function takes two 32-bit integers and returns one.
2.  **Implicit Returns**: Notice that functions like `add` don't use the `return` keyword. The last expression without a semicolon is automatically returned.
3.  **Explicit Returns**: The `divide` function uses `return 0;` to exit early if division by zero is attempted.
4.  **Error Handling**: The `if b == 0` check prevents division by zero crashes. In real applications, you'd use Rust's `Result` type for proper error handling.
5.  **Code Organization**: Breaking operations into separate functions makes code more readable and reusable.
6.  **Unicode in Strings**: Rust fully supports Unicode, so we can use mathematical symbols like `×` and `÷` in our output.

### Output
```
Calculator for 24 and 6
─────────────────────────
Addition:       24 + 6 = 30
Subtraction:    24 - 6 = 18
Multiplication: 24 × 6 = 144
Division:       24 ÷ 6 = 4

Testing division by zero:
Error: Cannot divide by zero!
Result: 0
```

---

## Summary & Key Takeaways

You've now mastered the four basic arithmetic operations in Rust:
*   **Addition (`+`)**: Combines two numbers into their sum.
*   **Subtraction (`-`)**: Finds the difference between two numbers.
*   **Multiplication (`*`)**: Calculates the product of two numbers.
*   **Division (`/`)**: Divides one number by another (integer division truncates).
*   **Modulo (`%`)**: Finds the remainder after division.

### Important Points to Remember

1.  **Integer vs. Float**: When both operands are integers, you get integer division. For decimal results, use floating-point types (`f32` or `f64`).
2.  **Type Safety**: Rust won't let you mix types without explicit conversion. You can't add an `i32` to an `f64` without converting one of them.
3.  **Overflow**: Be aware of the limits of your integer types. Use larger types (`i64`, `i128`) for bigger numbers.
4.  **Division by Zero**: Always check for zero before dividing to prevent panics.

### Try It Yourself! 🚀

1.  **Modify Program 1**: Change the numbers to see different sums. Try very large numbers and observe what happens.
2.  **Modify Program 4**: Convert the integer division to floating-point division by using `17.0 / 5.0` instead.
3.  **Extend Program 5**: Add a `modulo` function that returns the remainder, and add it to the calculator output.
4.  **Challenge**: Create a program that calculates the average of three integers. Remember that integer division truncates!

### Next Steps

In the next lesson, we'll explore:
*   Working with floating-point numbers for precise decimal calculations
*   Type conversion between different numeric types
*   More complex mathematical operations

Happy Coding! 🦀
