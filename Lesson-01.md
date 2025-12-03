# Rust Programming: A First-Year Introduction

Welcome to your first deep dive into Rust! In this lesson, we will explore four fundamental programs. Each one is designed to teach you specific concepts of the language, from basic variables to recursion and control flow.

Rust is a modern systems programming language focused on safety, speed, and concurrency. It might feel strict at first, but it's designed to help you write bug-free code.

---

## Program 1: Celsius to Fahrenheit Conversion 🌡️

This simple program introduces you to the structure of a Rust application, variables, and basic arithmetic.

```rust
fn main() {
    let celsius = 25.0; // Input temperature in Celsius
    let fahrenheit = celsius * 1.8 + 32.0;
    println!("{} Celsius is {} Fahrenheit", celsius, fahrenheit);
}
```

### Key Concepts

1.  **`fn main()`**: This is the entry point of every Rust executable. Code execution starts here.
2.  **`let`**: We use the `let` keyword to declare variables. By default, variables in Rust are **immutable**, meaning their values cannot be changed after assignment.
    *   *Note*: If we wanted to change `celsius` later, we would need to write `let mut celsius = ...`.
3.  **Type Inference**: You'll notice we didn't specify that `celsius` is a floating-point number. Rust is smart enough to infer the type. Because we used `25.0` (with a decimal), Rust assigns it the type `f64` (a 64-bit float).
4.  **`println!`**: This is a **macro** (indicated by the `!`), not a function. It prints text to the console. The `{}` are placeholders (or "holes") that get filled with the values of the variables you provide (`celsius` and `fahrenheit`).

---

## Program 2: Check for Palindrome 🔄

This program checks if a word reads the same forwards and backwards. It introduces functions, string manipulation, and iterators.

```rust
fn is_palindrome(text: &str) -> bool {
    let reversed: String = text.chars().rev().collect();
    text == reversed
}

fn main() {
    let word1 = "level";
    let word2 = "hello";
    
    println!("'{}' is a palindrome: {}", word1, is_palindrome(word1));
    println!("'{}' is a palindrome: {}", word2, is_palindrome(word2));
}
```

### Key Concepts

1.  **Functions**:
    *   `fn is_palindrome(text: &str) -> bool`: This defines a function named `is_palindrome`.
    *   `text: &str`: It takes one argument named `text` of type `&str` (a "string slice"). This is a view into a string, which is efficient because it doesn't copy the data.
    *   `-> bool`: It returns a boolean value (`true` or `false`).
2.  **Method Chaining**:
    *   `text.chars()`: Turns the string slice into an iterator of characters.
    *   `.rev()`: Reverses the order of the iterator.
    *   `.collect()`: Consumes the iterator and builds a new collection (in this case, a `String`).
3.  **Implicit Return**: Notice the last line `text == reversed` does **not** have a semicolon `;`. In Rust, the last expression in a block is automatically returned. If you added a semicolon, it would become a statement and return `()` (unit type), which would cause an error.
4.  **`String` vs `&str`**:
    *   `String`: An owned, growable text buffer (like `reversed`).
    *   `&str`: A borrowed view into string data (like `text`).

---

## Program 3: Factorial Calculation ❗

This program calculates the factorial of a number (e.g., 5! = 5 * 4 * 3 * 2 * 1). It demonstrates recursion and conditional logic.

```rust
fn factorial(n: u64) -> u64 {
    if n == 0 {
        1
    } else {
        n * factorial(n - 1)
    }
}

fn main() {
    let number = 5;
    println!("The factorial of {} is {}", number, factorial(number));
}
```

### Key Concepts

1.  **Recursion**: The function `factorial` calls itself. This is a common way to solve problems that can be broken down into smaller, similar sub-problems.
2.  **`u64`**: This stands for "unsigned 64-bit integer". It can only hold positive numbers, which makes sense for a factorial input.
3.  **Expression-Based Control Flow**:
    *   In many languages, `if` is a statement. In Rust, `if` is an **expression**, meaning it returns a value.
    *   The entire `if/else` block evaluates to either `1` or the result of `n * factorial(n - 1)`. This result is then returned by the function.

---

## Program 4: Find the Largest of Three Numbers 🤔

This program finds the maximum of three variables. It shows how to handle complex logic and control flow.

```rust
fn main() {
    let a = 10;
    let b = 25;
    let c = 15;
    
    let largest = if a >= b && a >= c {
        a
    } else if b >= a && b >= c {
        b
    } else {
        c
    };
    
    println!("The largest of {}, {}, and {} is {}", a, b, c, largest);
}
```

### Key Concepts

1.  **`if` as an Expression (Again)**:
    *   Here, we assign the result of the `if/else` chain directly to the variable `largest`.
    *   `let largest = if ... { ... } else { ... };`
    *   This is much cleaner than declaring `let mut largest;` and assigning it inside each block.
2.  **Boolean Logic**:
    *   `&&`: The logical AND operator. Both conditions must be true.
    *   `>=`: Greater than or equal to.
3.  **Scope**: The variables `a`, `b`, and `c` are defined in the `main` function scope and are available throughout the `if` blocks.

---

## Summary & Exercises

You've now seen four distinct Rust programs covering:
*   **Variables & Math**: How to store and manipulate numbers.
*   **Strings & Iterators**: How to process text efficiently.
*   **Recursion**: How to write functions that call themselves.
*   **Control Flow**: How to make decisions in code.

### Try It Yourself!
1.  **Modify Program 1**: Change the formula to convert Fahrenheit to Celsius.
2.  **Modify Program 2**: Make the palindrome checker case-insensitive (so "Level" is also a palindrome). *Hint: Look up `to_lowercase()`.*
3.  **Modify Program 4**: Add a fourth number `d` and find the largest of four.

Happy Coding! 🦀
