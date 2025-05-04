The `let` control flow statements don't have an equivalent in C/C++. It could be considered similar to the pattern match conditionals in Perl/Python.
Unlike the regular `if` statement a pattern is provided instead of a conditional.
# `if let` Expression
`if let` executes your code if a value matches a pattern. Unlike the `match` statement `if let` does not have to match all the branches (of the value). This makes the code a bit more concise. Just like the regular `if` statement, an `else` clause can also be specified in the `if let` block.
```Rust
let dish = ("Ham", "Eggs");
// this body will be skipped because the pattern is refuted
if let ("Bacon", b) = dish {
	println!("Bacon is served with {}", b);
} else {
	// This block is evaluated instead.
	println!("No bacon will be served");
}
// this body will execute
if let ("Ham", b) = dish {
	println!("Ham is served with {}", b);
}
if let _ = 5 {
	println!("Irrefutable patterns are always true");
}
```
The `if` and `if let` statements can be intermixed to make one statement.

The below `if let` statement
```Rust
if let PATTERN = EXPRESSION {
	/* body */
} else {
	/* body */
}
```
is equivalent to
```Rust
match EXPRESSION {
	PATTERN => { /* body */ },
	_ => { /* body */ } // body can be () if there is no else clause
}
```
# `while let` Expression
The `while let` statement tests the value against a pattern in a loop.
```Rust
fn main() {
   let mut x = vec![1, 2, 3];
   while let Some(y) = x.pop() { // Iterate over the Vector in reverse.
       println!("y = {}", y);
   }
}
```
Note: See [[Stdlib - Options]] for usage of `Some` above.
The `while let` loop below
```Rust
while let PATTERN = EXPRESSION {
	// loop body
}
```
is equivalent to
```Rust
loop {
	match EXPRESSION {
		PATTERN => { /* loop body */ },
		_ => break,
	}
}
```
# `let else` Expression
The `let else` expressions allows to declare a variable with a pattern and handle an error if the pattern is not matched. This syntax is typically used in Rust to handle errors from function return values.
```Rust
let mut v = vec![1, 2, 3];
let Some(t) = v.pop(); // Declare a variable equal to a value in the vector
println!("Got a value: {t}");
```
Here we want to initialize `t` to value `3`. But this will fail because let expects the pattern to be irrefutable for declaration. Above the error from `v.pop()` is not handled (the vector could be empty and return None) so compilation fails.
```Rust
fn main() {
    let mut v = vec![1, 2, 3];
    // v.pop(); v.pop(); v.pop();
    let Some(t) = v.pop() else {
        // Refutable patterns require an else block
        panic!(); // The else block must diverge
    };
    
	println!("Got a value: {t}"); // Prints: Got a value: 3
}
```
Here previous error is fixed. The error from `v.pop()` is handled in the `else` clause so compilation works. If the `v.pop()` is uncommented in line 3, compilation would still work but runtime would result in a panic.  The condition for the `let else` clause is that the code in the `else` clause must diverge i.e. you can `return`, `break` or `panic!` but code after the block end should be unreachable from within the block.
Note: This pattern is similar to the `|| die()` pattern in Perl.