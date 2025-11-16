# What is borrowing
Setting a reference to data is called borrowing. It is similar to using Pointers in C/C++ but has some constraints. A reference is created using the "&" operator (similar to taking address-of a variable in C/C++). The reference itself can be mutable or immutable. Just like C/C++ the value of reference can be accessed or de-referenced using the "* " operator.
```rust
let a: i32 = 10; // Define a value
let y: &i32 = &a; // Define y as immutable reference to a(10). 'y' cannot be used to modify value of a.
let b: mut i32 = 20; // Define a mutable value
{
	let z: &mut i32 = &mut b; // Define z as mutable reference to b(20). 'z' can be used to modify value or b.
	*z = 30; // 
}
println!("a={a}, y={y}, b={b}") // Prints: a=10, y=10, b=30
```
Above, 'y' is an immutable reference to 'a' allowing the value '10' to be accessed using both 'a' and 'y' since ownership is not transferred. In contrast, 'z' is a mutable reference to a mutable value '20' through variable 'b'. The reference 'z' can be used modify the value of 'b'. NOTE: 'z' is in a different scope than 'b', see borrow checking rules for exclusive references below.
# Why use borrowing
Using borrowing (or references) prevents copying or cloning (for basic types or user types that implement copy trait). This allows for compiler to do memory optimizations. Second with borrowing the ownership of the value/data is NOT transferred, so the original value can continue to be used.
# Borrow Checking
Rust's borrow checker puts constraints on how values can be borrowed.
1. Reference must always be valid i.e. a reference cannot outlive the value it borrows
```rust
let vec2: &Vec<i32> = { // Error: `vec1` does not live long enough
	let vec1 Vec<i32> = vec![10, 20, 30];
	&vec1 // block returns a reference to vec1, but at end of block vec1 is dropped
}
```
2. Aliasing - At any given time there can be
	1. multiple immutable (shared) references to a value
	2. or single mutable (exclusive) reference to a value
```rust
{
	let mut vec1 = vec![10, 20, 30];
	let ref1 = &vec1;
	let ref2 = &vec1;
	println!("ref1: {:?}, ref2: {:?}", ref1, ref2); // This works as ref1 and ref2 are immutable references
}
{
	let mut vec1 = vec![10, 20, 30];
	let ref1 = &mut vec1;
	let ref2 = &mut vec1;
	println!("ref1: {:?}, ref2: {:?}", ref1, ref2); // Error: cannot borrow `vec1` as mutable more than once at a time i.e single mutable reference allowed
}
{
	let mut vec1 = vec![10, 20, 30];
	let ref1 = &vec1;
	let mut ref2 = &mut vec1;
	println!("ref1: {:?}, ref2: {:?}", ref1, ref2); // Error: cannot borrow `vec1` as mutable because it is also borrowed as immutable i.e cannot mix mutable and immutable references
}
```

## Non-lexical lifetimes
In above example, both 'ref1' and 'ref2' were in scope at the same time which resulted in compiler errors. In addition to explicit scope with blocks and function, Rust can also figure out the dynamic scope of variable based on lifetimes i.e. where they are accessed last. After the last use of a variable, its lifetime is over and the compiler can delete the variable.
```rust ln=true
let mut vec1 = vec![10, 20, 30];
let ref1 = &vec1;
let ref2 = &vec1;
println!("ref1: {:?}, ref2: {:?}", ref1, ref2); // Works as both ref1 & ref2 are immutable references
let mut ref3 = &mut vec1;
println!("ref3: {:?}", ref3); // After line #4, ref1 & ref2 are no longer used and so at line #5 new reference to vec1 can be created
```
# Borrowing in functions
Passing a value into a function as a reference allows the caller to keep the ownership of the value.
```rust
fn main() {
	let vec1: Vec<i32> = vec![10, 20, 30];
	borrow_vec(&vec1); // Passing vec1 reference so the ownership of vec1 is not transffered to borrow_vec()
	println!("vec1: {:?}", vec1); // Able to access vec1
}

fn borrow_vec(vec: &Vec<i32>) {
	println!("In borrow_vec: {:?}", vec);
}
```
When allocating/creating a value in function and returning it, its better to return the ownership of the value rather than returning a reference.
```rust
fn main() {
	let _vec1: &Vec<i32> = get_new_vec_ref();
	let _vec2: Vec<i32> = get_new_vec();
}
fn get_new_vec_ref() -> &Vec<i32> {
	let vec: Vec<i32> = vec![10, 20, 30]; // Create a new vector
	&vec // Return a reference to the vector. Error: vec goes out of scope at end of function and hence a refernce to it is invalid.
}
fn get_new_vec() -> Vec<i32> {
	let vec: Vec<i32> = vec![10, 20, 30];
	vec // Return the vector and its ownership
}
```