# Rust Programming Lesson 8: Enums and Pattern Matching

Welcome to Lesson 8! Enums (enumerations) are one of Rust's most powerful features. They allow you to define a type by enumerating its possible variants. Combined with pattern matching, enums enable you to write expressive, safe code that handles all possible cases.

## Program 1: Basic Enums

An enum defines a type that can be one of several variants. This is perfect for representing data that can be one of a fixed set of options.

```rust
// Define an enum
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

enum Direction {
    North,
    South,
    East,
    West,
}

fn main() {
    // Create enum values
    let light = TrafficLight::Red;
    let heading = Direction::North;
    
    // Using match with enums
    match light {
        TrafficLight::Red => println!("Stop!"),
        TrafficLight::Yellow => println!("Slow down!"),
        TrafficLight::Green => println!("Go!"),
    }
    
    // Enums in functions
    let action = get_action(heading);
    println!("Action: {}", action);
    
    // Another example
    let directions = [
        Direction::North,
        Direction::East,
        Direction::South,
        Direction::West,
    ];
    
    println!("\nNavigating:");
    for dir in directions.iter() {
        print_direction(dir);
    }
}

fn get_action(direction: Direction) -> &'static str {
    match direction {
        Direction::North => "Move forward",
        Direction::South => "Move backward",
        Direction::East => "Turn right",
        Direction::West => "Turn left",
    }
}

fn print_direction(direction: &Direction) {
    let name = match direction {
        Direction::North => "North",
        Direction::South => "South",
        Direction::East => "East",
        Direction::West => "West",
    };
    println!("Heading: {}", name);
}
```

### Key Concepts

- **Enum Definition**: Use the `enum` keyword followed by variant names.
- **Enum Values**: Create values using `EnumName::Variant` syntax.
- **Exhaustive Matching**: `match` expressions must cover all variants (compiler enforced).
- **Type Safety**: You can't accidentally use the wrong variant.
- **Namespacing**: Variants are namespaced under the enum name.

### Output

```
Stop!
Action: Move forward

Navigating:
Heading: North
Heading: East
Heading: South
Heading: West
```

---

## Program 2: Enums with Data

Enum variants can hold data! This makes them incredibly powerful for representing complex states.

```rust
enum Message {
    Quit,                       // No data
    Move { x: i32, y: i32 },   // Named fields (like a struct)
    Write(String),              // Single value
    ChangeColor(u8, u8, u8),   // Multiple values (like a tuple)
}

impl Message {
    fn process(&self) {
        match self {
            Message::Quit => {
                println!("Quit message received");
            },
            Message::Move { x, y } => {
                println!("Move to coordinates: ({}, {})", x, y);
            },
            Message::Write(text) => {
                println!("Text message: {}", text);
            },
            Message::ChangeColor(r, g, b) => {
                println!("Change color to RGB({}, {}, {})", r, g, b);
            },
        }
    }
}

fn main() {
    let messages = vec![
        Message::Quit,
        Message::Move { x: 10, y: 20 },
        Message::Write(String::from("Hello, Rust!")),
        Message::ChangeColor(255, 0, 0),
    ];
    
    println!("Processing messages:");
    for msg in messages.iter() {
        msg.process();
    }
}
```

### Key Concepts

- **Variant Data**: Variants can contain different types and amounts of data.
- **Struct-Like Variants**: Use `{ }` for named fields.
- **Tuple-Like Variants**: Use `( )` for unnamed fields.
- **Unit Variants**: No data at all (like `Quit`).
- **Pattern Matching with Data**: Extract data from variants using pattern matching.
- **Methods on Enums**: You can implement methods for enums just like structs.

### Output

```
Processing messages:
Quit message received
Move to coordinates: (10, 20)
Text message: Hello, Rust!
Change color to RGB(255, 0, 0)
```

---

## Program 3: The Option Enum - Handling Absence

`Option<T>` is Rust's way of representing a value that might not exist. It eliminates null pointer errors!

```rust
fn main() {
    // Option can be Some(value) or None
    let some_number = Some(5);
    let some_string = Some("a string");
    let absent_number: Option<i32> = None;
    
    println!("some_number: {:?}", some_number);
    println!("absent_number: {:?}", absent_number);
    
    // Using match with Option
    let x = Some(10);
    match x {
        Some(value) => println!("Got a value: {}", value),
        None => println!("Got nothing"),
    }
    
    // Practical example: finding an element
    let numbers = [1, 2, 3, 4, 5];
    let search_result = find_number(&numbers, 3);
    
    match search_result {
        Some(index) => println!("Found at index: {}", index),
        None => println!("Not found"),
    }
    
    let not_found = find_number(&numbers, 10);
    match not_found {
        Some(index) => println!("Found at index: {}", index),
        None => println!("Number 10 not found"),
    }
    
    // Using if let for cleaner code when you only care about one case
    let favorite_color: Option<&str> = Some("blue");
    
    if let Some(color) = favorite_color {
        println!("Your favorite color is: {}", color);
    }
    
    // Option methods
    let value = Some(42);
    println!("Is some: {}", value.is_some());
    println!("Is none: {}", value.is_none());
    println!("Unwrap or default: {}", absent_number.unwrap_or(0));
}

fn find_number(arr: &[i32], target: i32) -> Option<usize> {
    for (index, &value) in arr.iter().enumerate() {
        if value == target {
            return Some(index);
        }
    }
    None
}
```

### Key Concepts

- **Option<T>**: An enum with two variants: `Some(T)` and `None`.
- **No Null**: Rust doesn't have null. Use `Option` instead.
- **Type Safety**: You must handle both cases, preventing null pointer errors.
- **if let**: Syntactic sugar for matching when you only care about one variant.
- **Useful Methods**: `is_some()`, `is_none()`, `unwrap_or()`, and many more.
- **Generic Type**: The `<T>` means Option works with any type.

### Output

```
some_number: Some(5)
absent_number: None
Got a value: 10
Found at index: 2
Number 10 not found
Your favorite color is: blue
Is some: true
Is none: false
Unwrap or default: 0
```

---

## Program 4: The Result Enum - Error Handling

`Result<T, E>` is used for operations that might fail. It's Rust's primary error handling mechanism.

```rust
use std::fs::File;
use std::io::ErrorKind;

fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Cannot divide by zero"))
    } else {
        Ok(a / b)
    }
}

fn parse_number(s: &str) -> Result<i32, String> {
    match s.parse::<i32>() {
        Ok(num) => Ok(num),
        Err(_) => Err(String::from("Failed to parse number")),
    }
}

fn main() {
    // Using Result with match
    let result1 = divide(10.0, 2.0);
    match result1 {
        Ok(value) => println!("10 / 2 = {}", value),
        Err(error) => println!("Error: {}", error),
    }
    
    let result2 = divide(10.0, 0.0);
    match result2 {
        Ok(value) => println!("Result: {}", value),
        Err(error) => println!("Error: {}", error),
    }
    
    // Using if let with Result
    if let Ok(value) = divide(20.0, 4.0) {
        println!("20 / 4 = {}", value);
    }
    
    // Parsing with Result
    let valid_input = "42";
    let invalid_input = "abc";
    
    match parse_number(valid_input) {
        Ok(num) => println!("Parsed: {}", num),
        Err(e) => println!("Error: {}", e),
    }
    
    match parse_number(invalid_input) {
        Ok(num) => println!("Parsed: {}", num),
        Err(e) => println!("Error: {}", e),
    }
    
    // Using unwrap_or for default values
    let good = divide(15.0, 3.0).unwrap_or(0.0);
    let bad = divide(15.0, 0.0).unwrap_or(0.0);
    println!("Good result: {}, Bad result: {}", good, bad);
}
```

### Key Concepts

- **Result<T, E>**: An enum with `Ok(T)` for success and `Err(E)` for errors.
- **Explicit Error Handling**: You must handle both success and failure cases.
- **Type Parameters**: `T` is the success type, `E` is the error type.
- **No Exceptions**: Rust doesn't have exceptions; use `Result` instead.
- **Propagation**: Errors can be propagated up the call stack (we'll see `?` operator later).
- **Methods**: `unwrap_or()`, `is_ok()`, `is_err()`, and more.

### Output

```
10 / 2 = 5
Error: Cannot divide by zero
20 / 4 = 5
Parsed: 42
Error: Failed to parse number
Good result: 5, Bad result: 0
```

---

## Program 5: Advanced Pattern Matching

Pattern matching is incredibly powerful. Let's explore advanced techniques.

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}

#[derive(Debug)]
enum UsState {
    Alabama,
    Alaska,
    California,
    Texas,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        },
    }
}

fn main() {
    let coin1 = Coin::Penny;
    let coin2 = Coin::Quarter(UsState::Alaska);
    
    println!("Coin 1 value: {} cents", value_in_cents(coin1));
    println!("Coin 2 value: {} cents", value_in_cents(coin2));
    
    // Matching with Option
    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
    
    println!("five: {:?}, six: {:?}, none: {:?}", five, six, none);
    
    // Using _ placeholder
    let some_value = 7;
    match some_value {
        1 => println!("one"),
        3 => println!("three"),
        5 => println!("five"),
        7 => println!("seven"),
        _ => println!("something else"),
    }
    
    // Matching ranges
    let age = 25;
    match age {
        0..=12 => println!("Child"),
        13..=19 => println!("Teenager"),
        20..=64 => println!("Adult"),
        _ => println!("Senior"),
    }
    
    // Multiple patterns
    let number = 4;
    match number {
        1 | 2 => println!("One or two"),
        3 | 4 | 5 => println!("Three, four, or five"),
        _ => println!("Something else"),
    }
}

fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        Some(i) => Some(i + 1),
        None => None,
    }
}
```

### Key Concepts

- **Nested Enums**: Enums can contain other enums as data.
- **Extracting Values**: Pattern matching extracts data from enum variants.
- **Multiple Statements**: Match arms can contain multiple statements in `{ }`.
- **Placeholder `_`**: Matches anything, used as a catch-all.
- **Range Patterns**: Match ranges of values with `..=`.
- **Multiple Patterns**: Use `|` to match multiple patterns in one arm.
- **Derive Debug**: `#[derive(Debug)]` allows printing with `{:?}`.

### Output

```
Lucky penny!
Coin 1 value: 1 cents
State quarter from Alaska!
Coin 2 value: 25 cents
five: Some(5), six: Some(6), none: None
seven
Adult
Three, four, or five
```

---

## Program 6: Building a Simple Game State Machine

Let's create a practical example that uses enums to model a game's state.

```rust
#[derive(Debug)]
enum GameState {
    Menu,
    Playing { level: u32, score: u32 },
    Paused { level: u32, score: u32 },
    GameOver { final_score: u32 },
}

impl GameState {
    fn new() -> GameState {
        GameState::Menu
    }
    
    fn start_game(&self) -> GameState {
        match self {
            GameState::Menu => GameState::Playing { level: 1, score: 0 },
            _ => {
                println!("Can only start from menu");
                GameState::Menu
            }
        }
    }
    
    fn pause(&self) -> GameState {
        match self {
            GameState::Playing { level, score } => {
                GameState::Paused { level: *level, score: *score }
            },
            _ => {
                println!("Can only pause while playing");
                self.clone()
            }
        }
    }
    
    fn resume(&self) -> GameState {
        match self {
            GameState::Paused { level, score } => {
                GameState::Playing { level: *level, score: *score }
            },
            _ => {
                println!("Can only resume from pause");
                self.clone()
            }
        }
    }
    
    fn add_score(&self, points: u32) -> GameState {
        match self {
            GameState::Playing { level, score } => {
                let new_score = score + points;
                let new_level = if new_score >= 100 { level + 1 } else { *level };
                GameState::Playing { level: new_level, score: new_score }
            },
            _ => self.clone()
        }
    }
    
    fn game_over(&self) -> GameState {
        match self {
            GameState::Playing { score, .. } => GameState::GameOver { final_score: *score },
            _ => self.clone()
        }
    }
    
    fn display(&self) {
        match self {
            GameState::Menu => println!("📋 MENU - Press start to play"),
            GameState::Playing { level, score } => {
                println!("🎮 PLAYING - Level: {}, Score: {}", level, score)
            },
            GameState::Paused { level, score } => {
                println!("⏸️  PAUSED - Level: {}, Score: {}", level, score)
            },
            GameState::GameOver { final_score } => {
                println!("💀 GAME OVER - Final Score: {}", final_score)
            },
        }
    }
    
    fn clone(&self) -> GameState {
        match self {
            GameState::Menu => GameState::Menu,
            GameState::Playing { level, score } => {
                GameState::Playing { level: *level, score: *score }
            },
            GameState::Paused { level, score } => {
                GameState::Paused { level: *level, score: *score }
            },
            GameState::GameOver { final_score } => {
                GameState::GameOver { final_score: *final_score }
            },
        }
    }
}

fn main() {
    println!("=== Game State Machine Demo ===\n");
    
    let mut state = GameState::new();
    state.display();
    
    println!("\n--- Starting game ---");
    state = state.start_game();
    state.display();
    
    println!("\n--- Scoring points ---");
    state = state.add_score(30);
    state.display();
    
    state = state.add_score(50);
    state.display();
    
    println!("\n--- Pausing game ---");
    state = state.pause();
    state.display();
    
    println!("\n--- Resuming game ---");
    state = state.resume();
    state.display();
    
    println!("\n--- More points ---");
    state = state.add_score(40);
    state.display();
    
    println!("\n--- Game over ---");
    state = state.game_over();
    state.display();
}
```

### Key Concepts

- **State Machines**: Enums are perfect for modeling finite state machines.
- **Immutable State Transitions**: Methods return new states rather than modifying in place.
- **Data in States**: Different states carry different data.
- **Pattern Matching for Logic**: Each method uses `match` to handle state-specific behavior.
- **Real-World Modeling**: This pattern is common in games, UI, and protocols.

### Output

```
=== Game State Machine Demo ===

📋 MENU - Press start to play

--- Starting game ---
🎮 PLAYING - Level: 1, Score: 0

--- Scoring points ---
🎮 PLAYING - Level: 1, Score: 30
🎮 PLAYING - Level: 2, Score: 80

--- Pausing game ---
⏸️  PAUSED - Level: 2, Score: 80

--- Resuming game ---
🎮 PLAYING - Level: 2, Score: 80

--- More points ---
🎮 PLAYING - Level: 3, Score: 120

--- Game over ---
💀 GAME OVER - Final Score: 120
```

---

## Summary & Key Takeaways

You've now mastered enums and pattern matching in Rust:

- **Enums**: Define types with multiple possible variants.
- **Data in Variants**: Variants can hold different types and amounts of data.
- **Option<T>**: Rust's way of handling optional values (no null!).
- **Result<T, E>**: Rust's primary error handling mechanism.
- **Pattern Matching**: Exhaustively handle all cases with `match`.
- **if let**: Cleaner syntax when you only care about one case.

### The Power of Enums

Enums + pattern matching eliminate entire classes of bugs:
- **No null pointer errors**: Use `Option` instead
- **Explicit error handling**: Use `Result` instead of exceptions
- **Exhaustive checking**: Compiler ensures you handle all cases
- **Type safety**: Can't use the wrong variant

### Common Patterns

- Use `Option<T>` when a value might not exist
- Use `Result<T, E>` for operations that might fail
- Use custom enums to model state machines
- Use `match` for exhaustive handling
- Use `if let` when you only care about one variant

### Try It Yourself! 🚀

1. **Modify Program 2**: Add a new `Message` variant called `Resize(u32, u32)` and handle it in the `process` method.
2. **Extend Program 3**: Write a function that finds the maximum value in an array and returns `Option<i32>`.
3. **Enhance Program 4**: Create a function that reads a file and returns `Result<String, String>` with appropriate error messages.
4. **Challenge**: Create a traffic light system using enums. Model states (Red, Yellow, Green) and implement a `next()` method that transitions to the next state. Add a timer field to each state.

## Next Steps

In the next lesson, we'll explore:

- Vectors - Growable arrays
- HashMaps - Key-value storage
- Iterators and closures
- Working with collections efficiently

**Happy Coding! 🦀**
