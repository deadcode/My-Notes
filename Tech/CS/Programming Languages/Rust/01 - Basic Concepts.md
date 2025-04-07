# Statements
All statements end in semi-colon ";".

# Comments
Single line comments begin with double slashes. C-style multi line comments are also supported.
```rust
// Single line Comment
/* 
Multi line
comments can span
multiple lines
*/
```

# Variables
Definitions using "let" keyword.
Type specified using ":< type>" syntax. If not specified, compiler infers the type based on assignement.
```rust
let x: u32 = 10;
```
# Constants
Declared with "const" keyword and the type is explicitly required (i.e cannot be inferred by the compiler). Naming convention is Upper Snake case for constant names. The constants cannot be mutated.
```rust
const MAX_VALUE: u32 = 100;
```
# Mutability
All variables are im-mutable by default i.e once a variable is assigned a value it cannot be changed. To make a variable mutable, keyword "mut" should be used.
```rust
let y: i32 = 5;
y = 10; // Error: compile time

let mut z: i32 = 5;
z = 10; // is ok
```

# Scope
Variable lifetime is within the closing spope.
```rust
{
	let z: u32 = 10; // Scope of 'z' is within the braces
}
let s = z; // Error: z is out of scope
```

# Shadowing
Variable can be re-defined to new type or value. The last definition takes precedence
```rust
let z: i32 = 10;
let z: i32 = z + 10; // Value changed
println!("t is {t}"); // Prints: t is 20
```

## Shadowing vs Mutability
Shadowing is not same as Mutability. With Mutability the same variable (or memory location) is given a new value and type stays the same. With Shadowing we create a new variable (new memory location) which can have a different type and value from the original variable.
```rust
let u: i32 = 10;
let u: f64 = 3.0; // Value and Type of "u" is changed
```

## Shadowing in different Scope
A variable can be shadowed in a different scope without changing the variable in the original scope.
```rust
let v: u32 = 10;
{
	let v: i32 = 20;
	println!("Innver v = {v}"); // Prints: Inner v = 20
}
println!("Outer v = {v}"); // Prints: Outer v = 10
```

# Memory Management
Traditionally, languages have fallen into two broad categories:
* Full control via manual memory management: C, C++, Pascal, ...
	* Programmer decides when to allocate or free heap memory.
	* Programmer must determine whether a pointer still points to valid memory.
	* Studies show, programmers make mistakes.
	* C must manage heap manually with malloc and free. Common errors include for- getting to call free, calling it multiple times for the same pointer, or dereferencing a pointer after the memory it points to has been freed.
	* C++ has tools like smart pointers (unique_ptr, shared_ptr) that take advantage of language guarantees about calling destructors to ensure memory is freed when a function returns. It is still quite easy to mis-use these tools and create similar bugs to C.
* Full safety via automatic memory management at runtime: Java, Python, Go, Haskell, ...
	* A runtime system ensures that memory is not freed until it can no longer be referenced.
	* Typically implemented with reference counting or garbage collection.
	* Java, Go, and Python rely on the garbage collector to identify memory that is no longer reachable and discard it. This guarantees that any pointer can be dereferenced, eliminating use-after-free and other classes of bugs. But, GC has a runtime cost and is difficult to tune properly.

Rust offers a new mix: Full control and safety via compile time enforcement of correct memory management. It does this with an explicit ownership concept. Rust's ownership and borrowing model can, in many cases, get the performance of C, with alloc and free operations precisely where they are required -- zero cost. It also provides tools similar to C++'s smart pointers.

# Call by value vs reference
The default calling convention is to pass arguments into function by value. Pass by reference is also allowed.