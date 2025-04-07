# Loop
Simplest loop is with the "loop" keyword. By default this is a for ever loop unless it is broken explicitly.
```rust
loop { // loop with no condition
	println!("Print forever"); // Keeps printing indefinitely
}
```
To terminate the loop, use the "break" statement. The "break" statement only terminates the loop in inner-most scope. If there is a nested loop, multiple "break" statements would be needed.
```rust
loop {
	println!("Print once");
	break; // terminate the loop after one iteration
}
```
For nested loops, they can be labeled and single break can be used to get out of nested loops using "break" label.
```rust
'outer: loop { // label outer loop as 'outer
	println!("Loop Outer");
	'inner: loop { // label inner loop as 'inner
		println!(" Loop Inner");
		break 'outer; // terminate both loops. Without a label, it is same as "break 'inner"
	}
}
```
Just like conditionals a loop block can be used to assign a value.
```rust
let a: i32 = loop {
	break 10;
};
```

# For loop
Use to loop over all the values of a collection.
```rust
let vec: Vec<u32> = vec![15, 25, 35, 45, 55, 65, 75];
for i in vec { // Do NOT specify type for i as it is implied by iterator
	println!("{i}");
}
```

# While loop
While loop executes while a given condition is true.
```rust
let mut num: i32 = 0;
while num < 10 { // Iterate while 'num' is lower than 10
	println!("{num}");
	num = num + 1;
}
```