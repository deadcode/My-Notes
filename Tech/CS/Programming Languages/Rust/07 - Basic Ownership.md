# Program Memory
Programs allocate memory in two ways:
* Stack: Continuous area of memory for local variables.
	* Values have fixed sizes known at compile time.
	* Fast: just move a stack pointer.
	* Easy to manage: follows function calls.
	* Great memory locality.
* Heap: Storage of values outside of function calls.
	* Values have dynamic sizes determined at runtime.
	* Slightly slower than the stack: some book-keeping needed.
	* No guarantee of memory locality.

For Scalar data types eg. i32, u64, bool etc., the variable and its value are directly stored on the stack. For other data types, the variable is stored on the stack while its value is stored in the heap. Such types include, the tuples, arrays, structs and Strings etc.

```rust
let x: i32 = 10;
Stack
.- - - - - - - - - - - - - -.
: 							:
:	x                       :
: +-----------+-------+     :
: | type      |   i32 |     :
: | value     |    10 |     :
: +-----------+-------+     :
:                           :
`- - - - - - - - - - - - - -'

let s1 = String::from("Hello");
Stack
.- - - - - - - - - - - - - -.       Heap
: 							:     .- - - - - - - - - - - - - - - -.
:	s1                      :     :                               :
: +-----------+-------+     :     :                               :
: | capacity  |     5 |     :     :   +----+----+----+----+----+  :
: | ptr 	  |     o-+-----+-----+-->|  H |  e | l  | l  | o  |  :
: | len 	  |     5 |     :     :   +----+----+----+----+----+  :
: +-----------+-------+     :     :                               :
:                           :     :                               :
`- - - - - - - - - - - - - -'     `- - - - - - - - - - - - - - - -'
```

# Ownership
When defining a variable, the variable is said to "own" the value of the variable. Rust allows only single (one) owner for any value.

## Copy vs Move semantics
In Rust assignment for scalar data types works just like any other languages. The value of the variable is copies and modifying one does not affect the other (this is called the copy semantics for scalar data types).
For all other types, Rust implements the move semantics. When assigning one variable to another, the ownership of the value get transferred to the new variable. After the assignment the original variable is no longer in scope and accessing the value through it will result in an error. The same happens when a variable is passed as a function argument (the ownership of the value is transferred to the function parameter).
```rust
let x: i32 = 10;
let y: i32 = x; // Value of x copied to y
println!("x = {x}, y = {y}");
Stack
.- - - - - - - - - - - - - -. 
: 							:
:	x                       :
: +-----------+-------+     :
: | type      |   i32 |     :
: | value     |    10 |     :
: +-----------+-------+     :
:                           :
`- - - - - - - - - - - - - -'
.- - - - - - - - - - - - - -.
: 							:
:	y                       :
: +-----------+-------+     :
: | type      |   i32 |     :
: | value     |    10 |     :
: +-----------+-------+     :
:                           :
`- - - - - - - - - - - - - -'

let s1 = String::from("Hello");
let s2 = s1; // Ownership of "Hello" string moved to s2
println!("s1 = {s1}, s2 = {s2}"); // Error: Compilation, s1 is no longer valid
Stack
.- - - - - - - - - - - - - -.       Heap
: 							:     .- - - - - - - - - - - - - - - -.
:	s1 (inaccessible)       :     :                               :
: +-----------+-------+     :     :                               :
: | capacity  |     5 |     :     :   +----+----+----+----+----+  :
: | ptr 	  |     o-+xxxxx+xxx+-+-->|  H |  e | l  | l  | o  |  :
: | len 	  |     5 |     :   | :   +----+----+----+----+----+  :
: +-----------+-------+     :   | :                               :
:                           :   | :                               :
`- - - - - - - - - - - - - -'   | `- - - - - - - - - - - - - - - -'
	                            |
.- - - - - - - - - - - - - -.   |
: 							:   |
:	s2                      :   |
: +-----------+-------+     :   |
: | capacity  |     5 |     :   |
: | ptr 	  |     o-+-----+---'
: | len 	  |     5 |     :
: +-----------+-------+     :
:                           :
`- - - - - - - - - - - - - -'
```

Example of a function call.
```rust
fn say_hello(name: String) {
	println!("Hello {name}") // 'name' goes out of scope at end of function
	                         // and Rust compiler will re-claim the memory
	                         // it points to
}

fn main() {  
	let name = String::from("Bruce");
	say_hello(name); // Ownership of 'name' transferred to 'say_hello::name'
	say_hello(name); // Error: 'name' is no longer accessible
}
```

## Cloning
To access a variable value after passing it into a function you can either:
1. Pass the variable by reference rather than by assigning.
```rust
fn say_hello(name: &String) {
	println!("Hello {name}") // 'name' refers to the original value from the scope of main()
}

fn main() {  
	let name = String::from("Bruce");
	say_hello(&name); // Ownership of 'name' is not moved
	say_hello(&name);
```
2. Make a clone of the value and pass the ownership of the clone to the function.
```rust
fn take_ownership(vec: Vec<i32>) {
	println!("vec is: {:?}", vec); // vec goes out of scope and vec_1 clone from main is deleted
}

fn main() {
	let vec_1: Vec<i32> = vec![1, 2, 3, 4, 5];
	take_ownership(vec_1.clone()); // Ownership of vec_1 clone is passed to function
	println!("vec_1 is: {:?}", vec_1); // vec_1 is still accessible
}
```

# Value vs Reference
Default function calling convention is Rust is to call by value (values get copied on stack when calling functions)
```rust
fn main () {
	let x: i32 = 10;
	work_on_integer(x);
	println!("In main x = {x}"); // Prints x = 10
}

fn work_on_integer(mut var: i32) {
	var = 50;
	println!("Inside func var = {var}"); // Prints x = 50
}
```