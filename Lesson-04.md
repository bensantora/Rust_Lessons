# Rust Programming Lesson 4: Control Flow with if, else, and Loops

Welcome to Lesson 4! So far, our programs have executed sequentially, line by line, from top to bottom. Now, it's time to make our programs smarter by introducing control flow. Control flow allows you to execute code conditionally and repeat blocks of code, making your programs dynamic and responsive.

## Program 1: Making Decisions with if

This program introduces the most fundamental form of control flow: the if statement. It runs a block of code only if a specified condition is true.

```rust
fn main() {
    let number = 10;

    if number > 5 {
        println!("The condition was true!");
        println!("{} is greater than 5.", number);
    }

    let another_number = 3;
    if another_number > 5 {
        // This block will not run
        println!("You won't see this.");
    }

    println!("Program finished.");
}
```

### Key Concepts

- **if Statement**: The `if` keyword is followed by a condition. If the condition evaluates to `true`, the code inside the subsequent curly braces `{}` is executed.
- **Conditions**: A condition is an expression that evaluates to a boolean value (`true` or `false`). Common comparison operators include:
    - `>` greater than
    - `<` less than
    - `==` equal to
    - `!=` not equal to
- **Code Blocks**: The code inside the curly braces `{...}` is the "body" of the `if` statement. It only runs when the condition is met.
- **Immutability**: The `if` statement does not change the value of the variable; it only checks it.

### Output

```
The condition was true!
10 is greater than 5.
Program finished.
```

---

## Program 2: Providing Alternatives with else

What if you want to do something when the condition is false? The `else` keyword provides an alternative block of code to execute.

```rust
fn main() {
    let age = 16;

    if age >= 18 {
        println!("You are old enough to vote!");
    } else {
        println!("Sorry, you are not old enough to vote yet.");
    }
    
    let temperature = 30;
    
    if temperature > 25 {
        println!("It's a hot day!");
    } else {
        println!("It's not a hot day.");
    }
}
```

### Key Concepts

- **else Statement**: The `else` block is executed if the preceding `if` condition is `false`.
- **Mutually Exclusive**: The `if` block and the `else` block are mutually exclusive. Exactly one of them will run, never both.
- **Two-Way Branch**: An if-else statement creates a two-way branch in your program's logic.

### Output

```
Sorry, you are not old enough to vote yet.
It's a hot day!
```

---

## Program 3: Handling Multiple Conditions with else if

When you have more than two possible outcomes, you can chain `else if` statements to check multiple conditions in sequence.

```rust
fn main() {
    let score = 85;
    let mut grade = 'F';

    if score >= 90 {
        grade = 'A';
    } else if score >= 80 {
        grade = 'B';
    } else if score >= 70 {
        grade = 'C';
    } else if score >= 60 {
        grade = 'D';
    } else {
        // This is the final catch-all
        grade = 'F';
    }

    println!("With a score of {}, your grade is {}.", score, grade);
}
```

### Key Concepts

- **else if Chain**: You can have as many `else if` conditions as you need after an initial `if`. Rust checks them in order from top to bottom.
- **First True Condition Wins**: As soon as Rust finds a condition that is `true`, it executes that block and skips the rest of the `else if` and `else` chain.
- **Final else**: The final `else` block is optional but good practice. It acts as a "catch-all" if none of the preceding conditions are met.

### Output

```
With a score of 85, your grade is B.
```

---

## Program 4: Using if in a let Statement

In Rust, `if` is an expression, meaning it evaluates to a value. This allows you to use `if` statements directly on the right side of a `let` statement to assign a variable.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {}", number);

    let is_logged_in = false;
    let status_message = if is_logged_in {
        "Welcome back!"
    } else {
        "Please log in to continue."
    };

    println!("Status: {}", status_message);
}
```

### Key Concepts

- **if as an Expression**: Because `if` evaluates to a value, you can assign its result to a variable.
- **Type Consistency**: The values returned in the `if` and `else` arms must be of the same type. In the first example, both are `i32`. In the second, both are `&str` (string slices). Rust will not compile your code if the types don't match.
- **No Semicolons**: Notice there are no semicolons after the values (`5` and `6`). This is because they are the expressions that the `if` block evaluates to.

### Output

```
The value of number is: 6
Status: Please log in to continue.
```

---

## Program 5: Repeating Code with loop

Sometimes, you need to repeat a block of code until a certain condition is met. The `loop` keyword creates an infinite loop, which you can then exit explicitly using the `break` keyword.

```rust
fn main() {
    let mut counter = 0;

    println!("Starting countdown...");
    loop {
        println!("{}", counter);
        counter += 1;

        if counter > 5 {
            // Exit the loop
            break;
        }
    }

    println!("Countdown finished!");
}
```

### Key Concepts

- **loop Keyword**: Creates an infinite loop. The code inside its block will repeat forever unless you explicitly stop it.
- **break Keyword**: Immediately exits the innermost loop it's in.
- **Mutable Counter**: We use a `mut` variable (`counter`) to keep track of how many times the loop has run.
- **Shorthand Assignment**: `counter += 1` is shorthand for `counter = counter + 1`.

### Output

```
Starting countdown...
0
1
2
3
4
5
Countdown finished!
```

---

## Program 6: Combining Concepts - A Simple Number Guessing Game

This program combines `loop`, `if`, `else if`, and `else` to create a simple interactive game.

```rust
use std::io;

fn main() {
    println!("Guess the number!");
    println!("I'm thinking of a number between 1 and 10.");

    let secret_number = 7;
    let mut guess_str = String::new();

    loop {
        println!("Please input your guess.");

        // Read user input
        io::stdin()
            .read_line(&mut guess_str)
            .expect("Failed to read line");

        // Parse the string into a number
        let guess: u32 = match guess_str.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("Please type a number!");
                guess_str.clear(); // Clear the string for the next iteration
                continue; // Skip the rest of this loop iteration
            }
        };

        // Compare the guess to the secret number
        if guess < secret_number {
            println!("Too small!");
        } else if guess > secret_number {
            println!("Too big!");
        } else {
            println!("You got it!");
            break; // Exit the loop on a correct guess
        }
        
        guess_str.clear(); // Clear the string for the next guess
    }
}
```

### Key Concepts

- **Combining Logic**: This program shows how control flow structures can be nested and combined to create complex logic.
- **continue Keyword**: The `continue` keyword skips the rest of the current loop iteration and starts the next one. We use it here if the user enters non-numeric input.
- **Game Loop**: The `loop` creates a "game loop" that continues until the player wins.
- **Conditional break**: The `break` statement is inside an `if` block, so the loop only terminates when the player's guess is correct.

### Output (Example Interaction)

```
Guess the number!
I'm thinking of a number between 1 and 10.
Please input your guess.
5
Too small!
Please input your guess.
9
Too big!
Please input your guess.
7
You got it!
```

---

## Summary & Key Takeaways

You've now learned how to control the flow of your Rust programs:

- **if**: Executes code if a condition is true.
- **else**: Provides an alternative block of code for when the `if` condition is false.
- **else if**: Chains multiple conditions together.
- **if as an Expression**: Uses `if` to assign a value to a variable, ensuring all arms return the same type.
- **loop**: Creates an infinite loop.
- **break**: Exits a loop.
- **continue**: Skips the rest of the current loop iteration.

### Important Points to Remember

- **Boolean Conditions**: `if` conditions must always evaluate to a boolean (`true` or `false`). Rust will not automatically convert non-boolean types (like numbers) to booleans.
- **Order Matters**: In an `else if` chain, the order of your conditions is important.
- **Infinite Loops**: Be careful with `loop`! Always ensure there's a reachable `break` condition, or your program will run forever.

### Try It Yourself! 🚀

1. **Modify Program 3**: Add grades for A+ (score >= 97) and A- (score >= 90).
2. **Even or Odd**: Write a program that takes an integer and uses the modulo operator (`%`) and an if-else statement to print whether the number is even or odd.
3. **Extend Program 5**: Modify the countdown to print "Blast off!" instead of "Countdown finished!".
4. **Challenge**: Write a program that uses a loop and an if statement to print the numbers from 1 to 100, but for multiples of 3 print "Fizz" instead of the number and for multiples of 5 print "Buzz". For numbers which are multiples of both 3 and 5, print "FizzBuzz".

## Next Steps

In the next lesson, we'll explore:

- More structured loops: `while` and `for`
- The powerful `match` control flow operator
- Tuples and Arrays for storing collections of data

**Happy Coding! 🦀**
