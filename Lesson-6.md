# Rust Programming Lesson 6: Ownership, Borrowing, and References

Welcome to Lesson 6! This is where Rust truly sets itself apart from other programming languages. Ownership is Rust's most unique feature, and it's what enables Rust to guarantee memory safety without needing a garbage collector. At first, these concepts might seem challenging, but they're the key to writing safe, efficient code.

## Understanding the Problem: Memory Management

Before we dive into Rust's solution, let's understand the problem. In programming, we need to manage memory:
- **Manual management** (like C): Fast but error-prone. You can forget to free memory (memory leaks) or free it twice (crashes).
- **Garbage collection** (like Java, Python): Safe but slower. A background process cleans up unused memory.
- **Rust's approach**: Ownership rules checked at compile time. Safe AND fast!

---

## Program 1: Ownership Basics - Move Semantics

Ownership is a set of rules that govern how Rust manages memory. Let's see these rules in action.

```rust
fn main() {
    // Rule 1: Each value has a single owner
    let s1 = String::from("hello");
    println!("s1: {}", s1);
    
    // Rule 2: When the owner goes out of scope, the value is dropped
    {
        let s2 = String::from("inner scope");
        println!("s2: {}", s2);
    } // s2 is dropped here
    
    // This would error - s2 is no longer valid
    // println!("{}", s2);
    
    // Rule 3: There can only be one owner at a time
    let s3 = String::from("world");
    let s4 = s3; // s3's value MOVES to s4
    
    println!("s4: {}", s4);
    
    // This would error - s3 is no longer valid!
    // println!("s3: {}", s3);
    
    // Integers and other simple types are copied, not moved
    let x = 5;
    let y = x; // x is copied to y
    
    println!("x: {}, y: {}", x, y); // Both are valid!
}
```

### Key Concepts

- **Ownership Rules**:
  1. Each value in Rust has a variable that's called its owner.
  2. There can only be one owner at a time.
  3. When the owner goes out of scope, the value will be dropped (freed).
- **Move Semantics**: When you assign a `String` (or other heap-allocated type) to another variable, ownership moves. The original variable becomes invalid.
- **Copy Types**: Simple types like integers, floats, and booleans implement the `Copy` trait, so they're copied instead of moved.
- **Scope**: Variables are valid from the point they're declared until the end of their scope (usually marked by `}`).

### Output

```
s1: hello
s2: inner scope
s4: world
x: 5, y: 5
```

---

## Program 2: Ownership and Functions

When you pass a value to a function, ownership is transferred (moved) to that function.

```rust
fn main() {
    let s = String::from("hello");
    
    takes_ownership(s); // s's value moves into the function
    
    // This would error - s is no longer valid
    // println!("{}", s);
    
    let x = 5;
    makes_copy(x); // x is copied into the function
    
    println!("x is still valid: {}", x); // This works!
    
    // Getting ownership back with return values
    let s1 = String::from("world");
    let s2 = takes_and_gives_back(s1); // s1 moves in, return value moves to s2
    
    println!("s2: {}", s2);
}

fn takes_ownership(some_string: String) {
    println!("Function received: {}", some_string);
} // some_string goes out of scope and is dropped

fn makes_copy(some_integer: i32) {
    println!("Function received: {}", some_integer);
} // some_integer goes out of scope, but nothing special happens (it's just a copy)

fn takes_and_gives_back(a_string: String) -> String {
    println!("Processing: {}", a_string);
    a_string // Return ownership to the caller
}
```

### Key Concepts

- **Function Parameters**: Passing a value to a function transfers ownership (for non-Copy types).
- **Return Values**: Functions can return ownership back to the caller.
- **Copy vs. Move**: Copy types (like `i32`) are copied when passed to functions, so the original remains valid.
- **Tedious Pattern**: Having to return ownership every time is tedious. This is where references come in!

### Output

```
Function received: hello
Function received: 5
x is still valid: 5
Processing: world
s2: world
```

---

## Program 3: References and Borrowing

References allow you to refer to a value without taking ownership of it. This is called "borrowing."

```rust
fn main() {
    let s1 = String::from("hello");
    
    let len = calculate_length(&s1); // Pass a reference
    
    println!("The length of '{}' is {}", s1, len); // s1 is still valid!
    
    // Multiple immutable references are allowed
    let r1 = &s1;
    let r2 = &s1;
    println!("r1: {}, r2: {}", r1, r2);
    
    // Demonstrating borrowing with different types
    let numbers = [1, 2, 3, 4, 5];
    let sum = sum_array(&numbers);
    println!("Sum of {:?} is {}", numbers, sum); // numbers still valid
}

fn calculate_length(s: &String) -> usize {
    s.len()
} // s goes out of scope, but it doesn't own the String, so nothing is dropped

fn sum_array(arr: &[i32; 5]) -> i32 {
    let mut total = 0;
    for &num in arr.iter() {
        total += num;
    }
    total
}
```

### Key Concepts

- **References**: Created with `&`. They allow you to refer to a value without taking ownership.
- **Borrowing**: When a function takes a reference as a parameter, we say it "borrows" the value.
- **Immutable References**: By default, references are immutable. You can have multiple immutable references to the same value.
- **Reference Syntax**: `&variable` creates a reference, `&Type` is the type of a reference.
- **No Ownership Transfer**: When a reference goes out of scope, the value it points to is not dropped.

### Output

```
The length of 'hello' is 5
r1: hello, r2: hello
Sum of [1, 2, 3, 4, 5] is 15
```

---

## Program 4: Mutable References

Sometimes you need to modify a borrowed value. Mutable references allow this, but with strict rules.

```rust
fn main() {
    let mut s = String::from("hello");
    
    println!("Before: {}", s);
    change(&mut s); // Pass a mutable reference
    println!("After: {}", s);
    
    // Rule: You can have only ONE mutable reference at a time
    let r1 = &mut s;
    append_exclamation(r1);
    // Can't use r1 here anymore because it was used above
    
    println!("Final: {}", s);
    
    // Demonstrating the restriction
    let mut x = 5;
    {
        let r1 = &mut x;
        *r1 += 10; // Dereference with * to modify the value
    } // r1 goes out of scope here
    
    // Now we can create another mutable reference
    let r2 = &mut x;
    *r2 += 5;
    
    println!("x: {}", x);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}

fn append_exclamation(some_string: &mut String) {
    some_string.push('!');
}
```

### Key Concepts

- **Mutable References**: Created with `&mut`. Allow you to modify the borrowed value.
- **One Mutable Reference Rule**: You can have only ONE mutable reference to a value in a particular scope.
- **No Mixing**: You cannot have a mutable reference while immutable references exist.
- **Dereferencing**: Use `*` to access/modify the value behind a reference.
- **Data Race Prevention**: These rules prevent data races at compile time!

### Output

```
Before: hello
After: hello, world
Final: hello, world!
x: 20
```

---

## Program 5: String Slices

A string slice is a reference to part of a String. It's one of the most common uses of references.

```rust
fn main() {
    let s = String::from("hello world");
    
    // String slices
    let hello = &s[0..5];  // "hello"
    let world = &s[6..11]; // "world"
    
    println!("First word: {}", hello);
    println!("Second word: {}", world);
    
    // Shorthand syntax
    let hello2 = &s[..5];  // Same as [0..5]
    let world2 = &s[6..];  // From 6 to the end
    let entire = &s[..];   // The entire string
    
    println!("Entire: {}", entire);
    
    // Practical use: finding the first word
    let sentence = String::from("The quick brown fox");
    let first = first_word(&sentence);
    println!("First word of '{}' is '{}'", sentence, first);
    
    // String literals are slices!
    let literal = "Hello, Rustaceans!"; // Type is &str
    println!("Literal: {}", literal);
    
    // Array slices work too
    let numbers = [1, 2, 3, 4, 5];
    let slice = &numbers[1..4]; // [2, 3, 4]
    println!("Array slice: {:?}", slice);
}

fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' { // b' ' is a byte literal for space
            return &s[0..i];
        }
    }
    
    &s[..] // Return the whole string if no space found
}
```

### Key Concepts

- **String Slices**: Written as `&str`, they're references to a portion of a String.
- **Slice Syntax**: `&s[start..end]` creates a slice from index `start` to `end-1` (exclusive end).
- **Slice Shortcuts**: `&s[..n]` starts from 0, `&s[n..]` goes to the end, `&s[..]` is the entire string.
- **String Literals**: String literals like `"hello"` are actually slices (`&str`) pointing to the binary.
- **Immutable References**: Slices are always immutable references.
- **Array Slices**: The same syntax works for arrays, creating `&[T]` slices.

### Output

```
First word: hello
Second word: world
Entire: hello world
First word of 'The quick brown fox' is 'The'
Literal: Hello, Rustaceans!
Array slice: [2, 3, 4]
```

---

## Program 6: Putting It All Together - A Text Analyzer

This program demonstrates ownership, borrowing, and slices working together in a practical example.

```rust
fn main() {
    let text = String::from("Rust is a systems programming language focused on safety");
    
    println!("Analyzing: '{}'", text);
    println!("─────────────────────────────────────────────────────");
    
    // Borrowing for analysis
    let word_count = count_words(&text);
    let char_count = count_characters(&text);
    let longest = find_longest_word(&text);
    
    println!("Word count: {}", word_count);
    println!("Character count: {}", char_count);
    println!("Longest word: '{}'", longest);
    
    // text is still valid because we only borrowed it
    println!("\nOriginal text still accessible: '{}'", text);
    
    // Modifying the text
    let mut mutable_text = text; // Ownership moves
    add_emphasis(&mut mutable_text);
    println!("Modified text: '{}'", mutable_text);
}

fn count_words(s: &String) -> usize {
    s.split_whitespace().count()
}

fn count_characters(s: &String) -> usize {
    s.chars().count()
}

fn find_longest_word(s: &String) -> &str {
    let mut longest = "";
    
    for word in s.split_whitespace() {
        if word.len() > longest.len() {
            longest = word;
        }
    }
    
    longest
}

fn add_emphasis(s: &mut String) {
    s.push_str(" 🦀");
}
```

### Key Concepts

- **Combining Concepts**: This program uses immutable borrowing for analysis and mutable borrowing for modification.
- **Lifetime Relationship**: The returned slice from `find_longest_word` is tied to the input string's lifetime.
- **Ownership Transfer**: `let mut mutable_text = text` moves ownership, making `text` invalid afterward.
- **Method Chaining**: Methods like `split_whitespace()` and `count()` work on borrowed values.
- **Real-World Pattern**: Analyze with immutable borrows, modify with mutable borrows.

### Output

```
Analyzing: 'Rust is a systems programming language focused on safety'
─────────────────────────────────────────────────────────────────────
Word count: 9
Character count: 57
Longest word: 'programming'

Original text still accessible: 'Rust is a systems programming language focused on safety'
Modified text: 'Rust is a systems programming language focused on safety 🦀'
```

---

## Summary & Key Takeaways

You've now learned Rust's most distinctive feature - the ownership system:

- **Ownership**: Each value has one owner; when the owner goes out of scope, the value is dropped.
- **Move Semantics**: Non-Copy types transfer ownership when assigned or passed to functions.
- **Borrowing**: References (`&T`) let you use a value without taking ownership.
- **Mutable References**: `&mut T` allows modification, but only one at a time.
- **Slices**: References to contiguous sequences, like `&str` for strings and `&[T]` for arrays.

### The Borrowing Rules (Memorize These!)

At any given time, you can have EITHER:
- One mutable reference (`&mut T`), OR
- Any number of immutable references (`&T`)

Additionally:
- References must always be valid (no dangling references)

### Why This Matters

These rules eliminate entire classes of bugs at compile time:
- **No null pointer errors**: References are always valid
- **No data races**: Mutable access is exclusive
- **No use-after-free**: Ownership tracking prevents it
- **No memory leaks**: Values are automatically cleaned up

### Try It Yourself! 🚀

1. **Modify Program 2**: Create a function that takes ownership of a String, converts it to uppercase, and returns it.
2. **Extend Program 3**: Write a function that takes a reference to an array and returns a reference to the smallest element.
3. **Challenge Program 4**: Create a function that takes a mutable reference to a vector of integers and removes all even numbers.
4. **Advanced Challenge**: Write a function `find_word(text: &str, position: usize) -> Option<&str>` that returns the nth word in a string, or `None` if it doesn't exist.

## Next Steps

In the next lesson, we'll explore:

- Structs - Creating custom data types
- Methods and associated functions
- Implementing functionality for your own types
- Tuple structs and unit-like structs

**Happy Coding! 🦀**
