# Print Macro
Print macro can be used to output to the terminal. The "print" macro takes arguments inline with string, or as positional arguments or as named arguments. The "print" macro supports the same kind of escape sequences as "printf()" in "C". The "println" macro terminates the output with a newline.
```rust
print!("Output the string with a tab: \t!"); // Tab escape. No newline
print!("Terminate with newline.\n"); // Continue on last line and end with newline escape
println!("Terminate with newline."); // Exactly the same as above

let a: u32 = 420;
println!("Time to {a}"); // Inline argument in format string

println!("Learning {2} for {1} week does not make you a {0}", "master", 1, "Rust"); // positional arguments for format string. Arguments start at index 0.

println!("{language} is {type} system programming language",
type = "cool",
language = "Rust"); // named arguments to format string
```

# Read line to get Input
"read_line()" function from the "std::io" library can be used to read user input from the terminal.
``` rust
let mut inline: String = String::new(); // Mutable variable to read input
println!("Enter a string: ");  // Prompt the user what to type
std::io::stdin() // Library to read from terminal STDIN
	.read_line(&mut inline) // read_line reads from term into the provided variable
	.expect("Failed to read input"); // print an error in case input fails
println!("You typed: {inline}");
```
