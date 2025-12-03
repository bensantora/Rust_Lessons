# Rust Programming Lesson 5: Loops, Arrays, Tuples, and Pattern Matching

Welcome to Lesson 5! You've mastered basic control flow with `if` statements and the `loop` keyword. Now it's time to level up with more sophisticated looping constructs, learn how to store multiple values in collections, and unlock the power of Rust's `match` expression for pattern matching.

## Program 1: Conditional Loops with while

The `while` loop repeats code as long as a condition remains true. Unlike `loop`, which requires an explicit `break`, `while` checks its condition before each iteration.

```rust
fn main() {
    let mut countdown = 5;
    
    println!("Starting countdown...");
    while countdown > 0 {
        println!("{}", countdown);
        countdown -= 1;
    }
    println!("Liftoff! 🚀");
    
    // Using while for input validation
    let mut attempts = 0;
    let password = "rust123";
    let mut user_input = "wrong";
    
    while user_input != password && attempts < 3 {
        println!("Attempt {}: Access denied", attempts + 1);
        attempts += 1;
        
        // Simulating different inputs
        if attempts == 2 {
            user_input = "rust123"; // Correct on third try
        }
    }
    
    if user_input == password {
        println!("Access granted!");
    } else {
        println!("Too many failed attempts.");
    }
}
```

### Key Concepts

- **while Loop**: Continues executing as long as the condition is `true`. The condition is checked *before* each iteration.
- **Compound Conditions**: Using `&&` (AND) to combine multiple conditions. Both must be true for the loop to continue.
- **Decrement Operator**: `countdown -= 1` is shorthand for `countdown = countdown - 1`.
- **Loop Guards**: The condition acts as a "guard" that prevents the loop from running when it's no longer needed.

### Output

```
Starting countdown...
5
4
3
2
1
Liftoff! 🚀
Attempt 1: Access denied
Attempt 2: Access denied
Attempt 3: Access denied
Access granted!
```

---

## Program 2: Iterating with for Loops

The `for` loop is the most common way to iterate over a collection or a range of numbers. It's safer and more concise than `while` for most iteration tasks.

```rust
fn main() {
    // Iterating over a range
    println!("Counting from 1 to 5:");
    for number in 1..6 {
        println!("{}", number);
    }
    
    // Reverse iteration
    println!("\nCountdown:");
    for number in (1..6).rev() {
        println!("{}", number);
    }
    println!("Liftoff! 🚀");
    
    // Inclusive range
    println!("\nInclusive range (1 to 5):");
    for number in 1..=5 {
        println!("{}", number);
    }
    
    // Using for with step
    println!("\nEven numbers from 0 to 10:");
    for number in (0..=10).step_by(2) {
        println!("{}", number);
    }
}
```

### Key Concepts

- **Range Syntax**: `1..6` creates a range from 1 to 5 (exclusive of the end). `1..=5` is inclusive of both ends.
- **rev() Method**: Reverses the order of iteration.
- **step_by() Method**: Allows you to skip values in a range (e.g., count by 2s).
- **Automatic Iteration**: The `for` loop automatically handles the iteration logic, making it less error-prone than manual counter management.
- **Immutable Loop Variable**: The loop variable (`number`) is immutable within each iteration.

### Output

```
Counting from 1 to 5:
1
2
3
4
5

Countdown:
5
4
3
2
1
Liftoff! 🚀

Inclusive range (1 to 5):
1
2
3
4
5

Even numbers from 0 to 10:
0
2
4
6
8
10
```

---

## Program 3: Storing Multiple Values with Arrays

Arrays allow you to store multiple values of the same type in a single variable. Arrays in Rust have a fixed size that must be known at compile time.

```rust
fn main() {
    // Declaring an array
    let numbers = [1, 2, 3, 4, 5];
    
    // Accessing array elements (zero-indexed)
    println!("First element: {}", numbers[0]);
    println!("Third element: {}", numbers[2]);
    
    // Array with type annotation
    let floats: [f64; 3] = [1.5, 2.7, 3.9];
    println!("Float array: {:?}", floats);
    
    // Array with repeated values
    let zeros = [0; 5]; // Creates [0, 0, 0, 0, 0]
    println!("Zeros: {:?}", zeros);
    
    // Iterating over an array
    println!("\nIterating over numbers:");
    for num in numbers.iter() {
        println!("{}", num);
    }
    
    // Getting array length
    println!("\nArray length: {}", numbers.len());
    
    // Calculating sum
    let mut sum = 0;
    for num in numbers.iter() {
        sum += num;
    }
    println!("Sum of numbers: {}", sum);
}
```

### Key Concepts

- **Array Declaration**: `[value1, value2, ...]` creates an array. All elements must be the same type.
- **Type Annotation**: `[type; length]` specifies the element type and array size.
- **Zero-Indexed**: The first element is at index 0, the second at index 1, etc.
- **Fixed Size**: Array size cannot change after creation. The size is part of the type.
- **Debug Formatting**: `{:?}` prints the entire array for debugging purposes.
- **Initialization Syntax**: `[value; count]` creates an array with `count` copies of `value`.
- **iter() Method**: Creates an iterator over the array elements.

### Output

```
First element: 1
Third element: 3
Float array: [1.5, 2.7, 3.9]
Zeros: [0, 0, 0, 0, 0]

Iterating over numbers:
1
2
3
4
5

Array length: 5
Sum of numbers: 15
```

---

## Program 4: Grouping Different Types with Tuples

Tuples allow you to group together values of different types. Unlike arrays, tuples can have mixed types and are accessed by position rather than index.

```rust
fn main() {
    // Creating a tuple
    let person = ("Alice", 25, 5.6);
    
    // Accessing tuple elements with dot notation
    println!("Name: {}", person.0);
    println!("Age: {}", person.1);
    println!("Height: {} feet", person.2);
    
    // Destructuring a tuple
    let (name, age, height) = person;
    println!("\nDestructured: {} is {} years old and {} feet tall", name, age, height);
    
    // Tuple with type annotation
    let coordinates: (f64, f64) = (40.7128, -74.0060);
    println!("\nNew York coordinates: {:?}", coordinates);
    
    // Nested tuples
    let nested = ((1, 2), (3, 4));
    println!("Nested tuple: {:?}", nested);
    println!("Accessing nested: {}", (nested.0).1);
    
    // Function returning a tuple
    fn calculate_stats(numbers: [i32; 5]) -> (i32, i32, f64) {
        let mut sum = 0;
        let mut min = numbers[0];
        let mut max = numbers[0];
        
        for &num in numbers.iter() {
            sum += num;
            if num < min { min = num; }
            if num > max { max = num; }
        }
        
        let average = sum as f64 / numbers.len() as f64;
        (min, max, average)
    }
    
    let data = [10, 25, 5, 30, 15];
    let (min, max, avg) = calculate_stats(data);
    println!("\nStats for {:?}:", data);
    println!("Min: {}, Max: {}, Average: {:.2}", min, max, avg);
}
```

### Key Concepts

- **Tuple Declaration**: `(value1, value2, ...)` creates a tuple. Values can be different types.
- **Accessing Elements**: Use `.0`, `.1`, `.2`, etc. to access tuple elements by position.
- **Destructuring**: Assign tuple elements to individual variables in one statement.
- **Multiple Return Values**: Tuples are commonly used to return multiple values from a function.
- **Type Annotations**: `(type1, type2, ...)` specifies the types of tuple elements.
- **Reference Operator**: `&num` in the for loop creates a reference to avoid moving the value.

### Output

```
Name: Alice
Age: 25
Height: 5.6 feet

Destructured: Alice is 25 years old and 5.6 feet tall

New York coordinates: (40.7128, -74.006)

Nested tuple: ((1, 2), (3, 4))
Accessing nested: 2

Stats for [10, 25, 5, 30, 15]:
Min: 5, Max: 30, Average: 17.00
```

---

## Program 5: Pattern Matching with match

The `match` expression is one of Rust's most powerful features. It allows you to compare a value against a series of patterns and execute code based on which pattern matches.

```rust
fn main() {
    // Basic match with integers
    let number = 7;
    
    match number {
        1 => println!("One!"),
        2 | 3 | 5 | 7 | 11 => println!("{} is a prime number", number),
        13..=19 => println!("{} is a teen", number),
        _ => println!("{} is something else", number),
    }
    
    // Match with multiple statements
    let day = 3;
    
    match day {
        1 => {
            println!("Monday - Start of the work week");
            println!("Time to be productive!");
        },
        2..=5 => println!("Weekday"),
        6 | 7 => println!("Weekend!"),
        _ => println!("Invalid day"),
    }
    
    // Match as an expression
    let grade_number = 85;
    let letter_grade = match grade_number {
        90..=100 => 'A',
        80..=89 => 'B',
        70..=79 => 'C',
        60..=69 => 'D',
        _ => 'F',
    };
    println!("Grade: {}", letter_grade);
    
    // Match with tuples
    let point = (0, 5);
    
    match point {
        (0, 0) => println!("Origin"),
        (0, y) => println!("On the y-axis at y = {}", y),
        (x, 0) => println!("On the x-axis at x = {}", x),
        (x, y) => println!("Point at ({}, {})", x, y),
    }
    
    // Match with enums (simple example)
    enum Coin {
        Penny,
        Nickel,
        Dime,
        Quarter,
    }
    
    fn value_in_cents(coin: Coin) -> u8 {
        match coin {
            Coin::Penny => 1,
            Coin::Nickel => 5,
            Coin::Dime => 10,
            Coin::Quarter => 25,
        }
    }
    
    let my_coin = Coin::Quarter;
    println!("Coin value: {} cents", value_in_cents(my_coin));
}
```

### Key Concepts

- **match Expression**: Compares a value against patterns and executes the code for the first matching pattern.
- **Arms**: Each pattern and its code form a "match arm", written as `pattern => code`.
- **Multiple Patterns**: Use `|` to match multiple patterns in one arm (like an OR operator).
- **Range Patterns**: `13..=19` matches any value from 13 to 19 inclusive.
- **Catch-All Pattern**: `_` matches anything and is typically used as the last arm.
- **Exhaustiveness**: `match` must cover all possible values. The compiler will error if you miss a case.
- **Binding Variables**: Patterns can bind parts of the matched value to variables (like `y` in `(0, y)`).
- **Enums**: Custom types that can be one of several variants. Perfect for use with `match`.

### Output

```
7 is a prime number
Weekday
Grade: B
On the y-axis at y = 5
Coin value: 25 cents
```

---

## Summary & Key Takeaways

You've now mastered essential Rust concepts for working with collections and control flow:

- **while Loops**: Repeat code while a condition is true.
- **for Loops**: Iterate over ranges and collections safely and concisely.
- **Arrays**: Store multiple values of the same type with a fixed size.
- **Tuples**: Group values of different types together.
- **match Expressions**: Powerful pattern matching for handling multiple cases elegantly.

### Important Points to Remember

- **Choosing Loops**: Use `for` when you know how many times to iterate, `while` when you have a condition, and `loop` when you need an infinite loop with manual breaks.
- **Array Bounds**: Accessing an array out of bounds will cause a panic. Always ensure your index is valid.
- **Tuple Size**: Tuples can have up to 12 elements, but it's best to keep them small for readability.
- **match Exhaustiveness**: The compiler ensures you handle all cases, preventing bugs.
- **Pattern Matching Power**: `match` is more powerful than `if/else` chains for complex conditional logic.

### Try It Yourself! 🚀

1. **Modify Program 2**: Create a multiplication table for numbers 1-10 using nested `for` loops.
2. **Extend Program 3**: Write a function that finds the maximum value in an array without using built-in methods.
3. **Enhance Program 4**: Create a function that takes a tuple of three integers and returns them sorted in a new tuple.
4. **Challenge**: Write a program that uses `match` to implement a simple calculator. It should take a tuple of `(number1, operator, number2)` where operator is a `char` ('+', '-', '*', '/') and return the result.

## Next Steps

In the next lesson, we'll explore:

- Ownership and borrowing - Rust's unique memory management system
- References and slices
- The String type vs string slices
- Understanding the stack and heap

**Happy Coding! 🦀**
