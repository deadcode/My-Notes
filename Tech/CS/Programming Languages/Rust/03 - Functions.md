Functions are defined by the "fn" keyword. Function names are named using snake_case. Function parameters are declared similar to variables.
```rust
fn main() {
	my_fun("Let's print this");
}

fn my_fun(s: &str) {
	println!("String: {s}");
}
```
Function return value is specified using the arrow "->" syntax. The function return type needs to be specified. The last expression in the function is the return value. The last expression (not statement) does NOT need an ending semi-colon for the return value.
```rust
fn mul(x: i32, y: i32) -> i32 {
	println!("Multiplying {x} x {y}"); // Statement -> does not return any value
	x * y // Last expression does not have semi-colon, so its value is returned.
}
```
The "return" keyword can also be used to return from the middle of a function.
```rust
fn mul(x: i32, y: i32) -> i32 {
	return x*y; // Explicit return statement
}
```
A function can return more than 1 values. Common approach is to return multiple values as Tuples.
```rust
fn some_calc(x: i32, y: i32) -> (i32, i32, i32) {
	(x * y, x + y, x - y)
}
```
The Tuple return value can be assigned to a variable or deconstructed.
```rust
let result: (i32, i32, i32) = some_calc(x: 10, y: 5);
let (mul: i32, sum: i32, dif: i32) = some_calc(x:10, y: 5);
```
## Code Block
Any block of code enclosed using braces "{}". Like a function, a block of code can also return a value. The last expression in the block is the return value.
```rust
let full_name: String = { // Return value from block stored in a variable
	let first_name: &str = "Bruce";
	let last_name: &str = "Wayne";
	format!("{last_name}, {first_name}");
}; // The block is an assignment statement, so it ends with a semi-colon.
```
### Code Block vs Function
Both have similar use, however a code block cannot be re-used unlike a function and a unlike a function a code block cannot take any parameters.

## Labels
A block of code can be given a name label. A label is created using a tick "'" syntax followed by a colon ":". The label can be used to "break" from a block of code.
```rust
let score: u32 = 98;
let grade = 'block: { // Define a named block "block"
	if score >= 90 {
		break 'block 'A' // break from named block and return expr 'A'
	} else if score >= 80 {
		break 'block 'B'
	} else if score >= 70 {
		'C' // Can also return expr 'C' without "break" as there is no nested scope.
	} else {
		'F'
	}
}; // Semi-colon needed as this is an assignment statement
```
