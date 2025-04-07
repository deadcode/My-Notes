# If-Else statement
The conditional in the "if" statement has to evaluate to a "bool" type. Integers in the conditionals will result in an error.
```rust
let num: i32 = 10;
if num < 50 { // This is bool check
	println!("Number is less than 50");
} else {
	println!("Number is greater than or equal to 50");
}
if num { // Error: i32 is not bool
	println!("Number is not zero");
}
```
'IF' conditions can be cascaded.
```rust
let score: u32 = 98;
let mut grade: char = 'N';
if score >= 90 {
	grade = 'A';
} else if score >= 80 {
	grade = 'B';
} else if score >= 70 {
	grade = 'C';
} else { // Final 'else' is default case, without a conditional check
	grade = 'F';
}
```
'IF' conditional can be used as a return block. All the return expressions in the conditional need to have the same return type, else it will result in error.
```rust
let score: u32 = 98;
let grade: char = if score >= 90 {
	'A' // Expression is returning a 'char' so no semi-colon required
} else if score >= 80 {
	'B'
} else if score >= 70 {
	'C'
} else {
	"F" // Error: return type does not match the rest of the values
}; // Semi-colon needed as this is an assignment statement
```

# Match statement
Used to execute different branches of code based on a matching pattern. This is more readable compared to a cascaded "IF-Else" statements.
```rust
let score: u32 = 98;
let mut grade: char = 'N';
match score {
	90..=100 => grade = 'A',
	80..=89  => grade = 'B',
	70..=79  => grade = 'C',
	_        => grade = 'F',
}
```
Each pattern along with the code block in the match statement is called the "arm" of the statement. Each "arm" contains a "pattern" to match followed by "=>" keyword and the code block to execute if the pattern is true. Each arm is terminated by the comma ",". The final "arm"  with the under-score "_ " pattern is the default case which is executed if none of the other patterns match. All the patterns in the "match" arms need to exhaustive for the data type. If patterns are not exhaustive it will result in a compilation error.
```rust
let score: u32 = 98;
let mut grade: char = 'N';
match score {
	90..=100 => {println!("Score = {score}"); grade = 'A'}, // Block of code can contain multiple statements
	80..=89  => grade = 'B',
	70..=79  => grade = 'C',
	//_        => grade = 'F', // Error: Pattern are NOT-Exhaustive
}
```
Just like "IF-Else" block, the "Match" statement can also be used as a return block.
```rust
let score: u32 = 98;
let grade: char = match scrore {
	90..=100 => 'A', // Expression is returning the value, so no semi-colon
	80..=89  => 'B',
	70..=79  => 'C',
	_        => 'F',
}; // Semi-colon at end
```