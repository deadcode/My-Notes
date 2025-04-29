# Patterns and De-structuring
Rust allows extracting individual elements of a compound types (e.g. tuples and structs) by de-structuring them.
```Rust
let place_in_space = (10, 20, 30, 12345.23); // Create a tuple with 4 elements
let (x_ord, y_ord, z_ord, time) = place_in_space; // Extract elements - destructuring

// Above pattern de-structuring is the same as below
x_ord2 = place_in_space.0;
y_ord2 = place_in_space.1;
z_ord2 = place_in_space.2;
time2 = place_in_space.3;

println!("Our place in space-time: {x_ord},{y_ord},{z_ord} @ {time}");
```
Here Rust uses pattern matching (let statement on line 2) to de-structure the tuple into its parts. In this type of pattern matching the compiler can verify that pattern on LHS of "=" matches the value on the RHS. So this type of pattern matching is called **`Irrefutable Pattern`**. 
If the pattern on the LHS does not exactly match the value on the RHS that would result in a compile error. If you want to extract only some of the values from the compound type you can explicitly extract and ignore them.
```Rust
let place_in_space = (10, 20, 30, 12345.23); // Create a tuple with 4 elements

// Ignore the x_ord from the tuple, only bind the 2nd, 3rd, 4th values
let (_, y_ord, z_ord, time) = place_in_space;

// Ignore the time, only bind the 1st, 2nd, 3rd values
let (x_ord, y_ord, z_ord, _) = place_in_space;

// Ignore all z, y, z values only bind the time
let (.., time) = place_in_space;

// Ignore y, z values, only binding x and time
let (x_ord, .., time) = place_in_space;
```
 In example above, `"_"` pattern is used to match any single value and discard it. The `".."` (double dots) pattern is used to match and ignore multiple values at once. As long as the ignored values and matched pattern on the LHS match everything on the RHS the pattern is valid.
# Match Patterns
As seen before patterns can be specified in the arms of the `match` block as well.
```Rust
fn main() {
    let input = 'w';
    match input {
        'q'                             => println!("Quitting"),
        dir @ ('a' | 's' | 'w' | 'd')   => println!("Moving around: {dir}"),
        steps @ '0'..='9'               => println!("Number input {steps}"),
        key if key.is_lowercase()       => println!("Lowercase: {key}"),
        _                               => println!("Something else"),
    }
}
```
Here the:
* `|` is used combine multiple values in the pattern using `OR` logic
* `..` is used to specify a range
* `..=` is used to specify an inclusive range
* `_` is used as in de-structuring to match as *wild-card* (any value)
# De-Structuring Struct
Like tuples, `structs` can also be de-constructed using patterns and even used in `match` arms.
```Rust
struct Car {
	fuel: f32, // gas gallons
	fluids: (u32, u32, u32), // brake, wiper, gear
	battery: u32, // battery health
}

fn main () {
	let car1 = Car{ fuel: 7.5, fluids: (2, 3, 4), battery: 10};
	match car1 {
		Car {fuel: 7.2, ..}                     => println!("Fuel is 7.5"), // Only fuel level is matched, everything else is ignored
        Car {fluids: (_, 4, _), ..}             => println!("Wiper fluid is 3"), // Only fluilds.1 field is matched
        Car {fuel: f, fluids: (2, 3, 5), ..}    => println!("Car with fluids(2, 3, 4), has fuel={f}"), // match fluids tuple and bind fuel level to "f"
        Car {battery: 10, fluids:(.., g), ..}   => println!("Car has {g} Glallon gear oil!"),
        _                                       => println!("{:?}", car1),
	}
}
```
# De-structuring Enum
Just like tuples and structs, `enum` can also be de-structured using pattern and used in `match` arms. A common idiom is to use Enum as a return value from functions to check for success or failure in the calling function.
```Rust
enum Res { // Return a success code or an error string from the function
	Ok(i32), // success value
	Err(String), // error description
}

fn divide_in_two(n: i32) -> Res {
	if n % 2 == 0 {
	   Res::Ok(n / 2)
	} else {
	   Res::Err(format!("cannot divide {n} into two equal parts"))
	}
}

fn main() {
	let n = 101;

	match divide_in_two(n) { // match arms are used to de-structure the Res value
	   Res::Ok(half) => println!("{n} divided in two is {half}"),
	   Res::Err(msg) => println!("sorry, an error happened: {msg}"),
	}
}
```
Note: Since Rust guarantees that all variants / values in `match` get handled, the common mistake of unhandled return values in C/C++ is avoided here.
