# Rust Programming Lesson 9: Collections - Vectors, Strings, and HashMaps

Welcome to Lesson 9! Now it's time to explore Rust's dynamic collections that can grow and shrink at runtime. These are essential tools for building real-world applications.

## Program 1: Vectors - Growable Arrays

Vectors (`Vec<T>`) are resizable arrays stored on the heap.

```rust
fn main() {
    // Creating vectors
    let mut numbers: Vec<i32> = Vec::new();
    numbers.push(1);
    numbers.push(2);
    numbers.push(3);
    
    // Using the vec! macro
    let fruits = vec!["apple", "banana", "cherry"];
    println!("Fruits: {:?}", fruits);
    
    // Accessing elements
    let first = &fruits[0];
    println!("First: {}", first);
    
    // Safe access with get()
    match fruits.get(10) {
        Some(fruit) => println!("Fruit: {}", fruit),
        None => println!("Index out of bounds"),
    }
    
    // Iterating
    for (i, fruit) in fruits.iter().enumerate() {
        println!("{}: {}", i, fruit);
    }
}
```

### Key Concepts

- **Vec<T>**: Growable array on the heap
- **push/pop**: Add/remove from end
- **get()**: Safe access returning `Option`
- **Indexing**: `vec[i]` panics if out of bounds

### Output
```
Fruits: ["apple", "banana", "cherry"]
First: apple
Index out of bounds
0: apple
1: banana
2: cherry
```

---

## Program 2: Vector Operations

```rust
fn main() {
    let mut scores = vec![95, 87, 92, 78, 88];
    
    // Statistics
    let total: i32 = scores.iter().sum();
    let avg = total as f64 / scores.len() as f64;
    println!("Average: {:.2}", avg);
    
    // Filter and map
    let passing: Vec<i32> = scores.iter()
        .filter(|&&s| s >= 80)
        .map(|&s| s)
        .collect();
    println!("Passing: {:?}", passing);
    
    // Sorting
    scores.sort();
    println!("Sorted: {:?}", scores);
}
```

### Key Concepts

- **Iterator methods**: `sum()`, `filter()`, `map()`, `collect()`
- **Closures**: `|&&s| s >= 80`
- **sort()**: In-place sorting

---

## Program 3: Strings

```rust
fn main() {
    let mut s = String::from("Hello");
    s.push_str(", world!");
    println!("{}", s);
    
    // Concatenation
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");
    let s3 = s1 + &s2; // s1 moved
    
    // format! doesn't move
    let game = format!("{}-{}-{}", "tic", "tac", "toe");
    println!("{}", game);
    
    // Iteration
    for c in "नमस्ते".chars() {
        println!("{}", c);
    }
}
```

### Key Concepts

- **String vs &str**: Owned vs borrowed
- **UTF-8**: Characters can be multiple bytes
- **No indexing**: Use `chars()` or `bytes()`

---

## Program 4: HashMaps

```rust
use std::collections::HashMap;

fn main() {
    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Red"), 50);
    
    // Access
    if let Some(score) = scores.get("Blue") {
        println!("Blue: {}", score);
    }
    
    // Update or insert
    scores.entry(String::from("Yellow")).or_insert(50);
    
    // Word count example
    let text = "hello world wonderful world";
    let mut map = HashMap::new();
    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0);
        *count += 1;
    }
    println!("{:?}", map);
}
```

### Key Concepts

- **HashMap<K, V>**: Key-value pairs
- **entry().or_insert()**: Insert if missing
- **get()**: Returns `Option<&V>`

---

## Program 5: Student Database

```rust
use std::collections::HashMap;

struct Student {
    name: String,
    grades: Vec<u32>,
}

impl Student {
    fn new(name: String) -> Student {
        Student { name, grades: Vec::new() }
    }
    
    fn add_grade(&mut self, grade: u32) {
        self.grades.push(grade);
    }
    
    fn average(&self) -> f64 {
        if self.grades.is_empty() { return 0.0; }
        let sum: u32 = self.grades.iter().sum();
        sum as f64 / self.grades.len() as f64
    }
}

fn main() {
    let mut db: HashMap<String, Student> = HashMap::new();
    
    let mut alice = Student::new(String::from("Alice"));
    alice.add_grade(95);
    alice.add_grade(87);
    db.insert(String::from("alice"), alice);
    
    for student in db.values() {
        println!("{}: {:.2}", student.name, student.average());
    }
}
```

### Key Concepts

- **Nested collections**: HashMap with Vec
- **Real-world modeling**: Combining types

---

## Summary

- **Vec<T>**: Growable arrays
- **String**: UTF-8 text (Vec<u8>)
- **HashMap<K, V>**: Key-value storage

### Try It Yourself! 🚀

1. Remove duplicates from a vector
2. Count vowels in a string
3. Build a phone book with HashMap
4. Create an inventory system

## Next Steps

- Error handling with `?`
- Custom error types
- Panic vs Result

**Happy Coding! 🦀**
