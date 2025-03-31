# Statements
All statements end in semi-colon ";".
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