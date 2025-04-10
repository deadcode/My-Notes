# Structures
Rust `structs` work similar to structs in C/C++. Data fields of different types can be made elements of struct to define a custom data type.
```rust
struct Car { // Type defined with struct keyword
	owner: String, // Unlike C, fields do NOT terminate with semi-colon
	year: u32,
	fuel_level: f32,
	price: u32,
} // No semi-colon at end

fn main() {
	let my_car1: Car = Car { // Similar to calling constructor in C++
		owner: String::from("Bruce Wayne"),
		year: 2010,
		fuel_level: 0.5,
		price: 10_000_000,
	}; // All the fields of the struct need to be intialized
	let my_car2: Car = Car {
		owner: String::from("Robin"),
		year: 2015,
		fuel_level: 0.2,
	}; // Error: missing intilizer for price
}
```
Note: Unlike C++ structs cannot be inherited in Rust.
As in example above, all initializers for structs need to be complete. Any values omitted in initialization will result in compilation error.
Fields of a `struct` are accessed similar to C/C++ using the dot field name notation.
```rust
let year_built: u32 = my_car1.year;
```
## Mutability
Just like normal variables, custom `struct` variables are by default immutable. This means the fields of the `struct` cannot be changed unless the struct variable is made mutable. Note: Individual fields of the struct inherit the mutability of the variable. The individual fields of the struct cannot be made mutable.
```rust
let my_car1: Car = Car {
	owner: String::from("Bruce Wayne"),
	year: 2010,
	fuel_level: 0.5,
	price: 10_000_000,	
};
let mut my_car2: Car = Car {
	owner: String::from("Robin"),
	year: 2015,
	fuel_level: 0.2,
	price: 10_000,
};
my_car1.fuel_level = 0.75; // Error: my_car1 is immutable so all its fields are too
my_car2.fuel_level = 0.6; // Works fine
```
## Ownership
The ownership rules for values work just the same with individual fields of the struct as well. With the basic scalar data types, the accessing the struct field works with copy semantics. For data types allocated on the heap accessing the value of the struct field transfers the ownership to the new variable and the struct field is no longer accessible.
```rust
let my_car1: Car = Car {
	owner: String::from("Bruce Wayne"),
	year: 2010,
	fuel_level: 0.5,
	price: 10_000_000,	
};
println!("my_car1: Year = {}, Owner = {}", my_car1.year, my_car1.owner); // Prints: my_car1: Year = 2010, Owner = Bruce Wayne
let my_car1_year = my_car1.year; // Value of my_car1.year copied to my_car1_year
let my_car1_owner = my_car1.owner; // Ownership of my_car1.owner transferred to my_car1_owner making reference to my_car1.owner invalid
println!("Extracted my_car1: Year = {my_car1_year}, Owner = {my_car1_owner}"); // Prints: Extracted my_car1: Year = 2010, Owner = Bruce Wayne
println!("my_car1: Year = {}, Owner = {}", my_car1.year, my_car1.owner); // Error: borrow of moved value: `my_car1.owner`
```
## Initializers
New `struct` variables can be initialized from existing variables values by coping using the double dot `..` notation. The source of values is put after the `..` in the initializer and has to be the last element of the initializer.
```rust
#[derive(Debug)]
struct Car { // Type defined with struct keyword
	owner: String, // Unlike C, fields do NOT terminate with semi-colon
	year: u32,
	fuel_level: f32,
	price: u32,
} // No semi-colon at end

fn main() {
	let my_car1: Car = Car {
	   owner: String::from("Bruce Wayne"),
	   year: 2010,
	   fuel_level: 0.5,
	   price: 10_000_000,
	};
	let my_car2: Car = Car {
	   owner: String::from("Robin"), // my_car2 has new owner
	   ..my_car1 // remaining fields of my_car2 are copied from my_car1
	};
	println!("my_car1: {:?}", my_car1); // Prints: my_car1: Car { owner: "Bruce Wayne", year: 2010, fuel_level: 0.5, price: 10000000 }
	println!("my_car2: {:?}", my_car2); // Prints: my_car2: Car { owner: "Robin", year: 2010, fuel_level: 0.5, price: 10000000 }
}
```
When using the `..` initializer, the ownership rules still apply. Ownership of heap data values would get moved to the new variable and no longer be accessible from original struct variable.
```rust
#[derive(Debug)]
struct Car { // Type defined with struct keyword
	owner: String, // Unlike C, fields do NOT terminate with semi-colon
	year: u32,
	fuel_level: f32,
	price: u32,
} // No semi-colon at end

fn main() {
	let my_car1: Car = Car {
	   owner: String::from("Bruce Wayne"),
	   year: 2010,
	   fuel_level: 0.5,
	   price: 10_000_000,
	};
	let my_car2: Car = Car {
	   year: 2020, // I got a new model car
	   ..my_car1 // remaining fields of my_car2 are copied from my_car1 except the `owner`
	};
	println!("my_car2: {:?}", my_car2); // Prints: my_car2: Car { owner: "Bruce Wayne", year: 2020, fuel_level: 0.5, price: 10000000 }
	println!("my_car1: {:?}", my_car1); // Error: , ownership of 'owner' field moves to my_car2 and no longer accessible
}
```
# Tuple Structs
Tuples as similar to structs as they allow to create custom data types with a mix of data types and any number of fields. However they are different from structs as the tuple fields are not named. Tuples are also called un-named structs. Tuples are also defined similar to structs though they use parentheses "()" instead of curly braces "{}".
```rust
struct Point_2D(i32, i32); // x,y co-ordiantes
struct Point_3D(i32, i32, i32); // x,y,z co-ordinates
```
The fields of the tuple structs are accessed similar to tuples using the "var_name.index" notation.
```rust
let point1: Point_3D = Point_3D(3, 4, 5);
let p1_x = point1.0;
```
## Tuples vs Tuple Structs
Both tuples and tuple structs look similar in function and syntax. Syntactically the use of `struct` keyword is different between the two.
* Tuples structs create a new type, so they could provide more context when used compared to tuples.
* Like `struct` tuple structs could implement "methods" to act on the data while tuples cannot.

# Unit struct
Also called zero size struct is used to "declare" a struct name without "defining" it. The idea is same as pre-declaring struct types in C/C++. This is usually used when defining a struct to create its methods (traits) without specifying the data fields.
```rust
struct `abc`; // Zero size struct
```
# Methods
Similar to `struct` or `class` in C++ or Python, structs in Rust can have functions associated with them. This is done by creating an `impl` (implementation) block for the struct.
```rust
#[derive(Debug)]
struct Car { 
	owner: String,
	year: u32,
	fuel_level: f32,
	price: u32,
}

impl Car { // Define methods on Car{} type within the 'impl' block.
	fn display_info(&self) { // The first argument to a method is a reference to the caller
		println!(
			"Owner: {}, Year: {}, Price: {}",
			self.owner, self.year, self.price
		);
	}
}

fn main() {
	let my_car1: Car = Car {
	   owner: String::from("Bruce Wayne"),
	   year: 2010,
	   fuel_level: 0.5,
	   price: 10_000_000,
	};

	my_car1.display_info(); // Invoke methods using the dot function notatoin.
	Car::display_info(&my_car1); // methods can also called as associated functions by passing the reciever my_car1 reference explicitly
}
```
Methods differ from regular functions by having a reference to the calling variable using the `self` keyword. This is similar to methods in Python. The self arguments specify the ”receiver” - the object the method acts on. Also similar to C++/Python methods are called using the dot method_name notation. Note: the `self` argument is implicit in the call and does not need to be explicitly passed.
There are different forms of `self` receivers:
1. `&self` immutable reference: borrows the object from the caller using a shared and immutable reference. The object can be used again afterwards.
2. `&mut self` mutable reference: borrows the object from the caller using a unique and mutable reference. The object can be used again afterwards.
3. `self` immutable ownership: takes ownership of the object and moves it away from the caller. The method becomes the owner of the object. The object will be dropped (deallocated) when the method returns, unless its ownership is explicitly transmitted.
4. `mut self` mutable ownership: same as above, but the method can mutate the object.
5. No receiver: this becomes a static method on the struct. Typically used to create constructors which are called "new()" by convention.
## `self` vs `Self`
`Self` (with the capital 'S') is also a keyword that is an alias for the type of the `impl` block. In the method `self` is a shortcut for `self: Self`.
```rust
impl Car { // All following functions are similar in function
	fn display_info(&self) { ... }
	fn display_more_info(self: &Self) { ... } // Self is an alias for Car
	fn display_more(self: &Car) { ... }

	fn refuel(&mut self, gallons: f32) { // self is mutable reference as its member field is changed
		self.fuel_level += gallons;
	}

	fn sell(self) -> Self { // takes ownership of self and returns a new Self (or Car)
	   self // The caller variable will no longer be accessible
	}
}

fn main() {
	...
	my_car1.refuel(10.0); // Rust automatically fills in the required self type
	let your_car1: Car = my_car1.sell(); // my_car1 is no longer accessbile
	my_car1.display_info(); // Error: borrow of moved value: `my_car1`
	your_car1.display_info(); // Works fine
}
```
## Associated Functions
Associated functions are similar to `static` Class functions in C++. They are associated with the struct but do NOT operate on the variable (receiver). So the associated function do not need to take `self` as an argument. The associated functions are called using the type::function_name syntax instead of the receiver.method() syntax.
```rust
impl Car {
	...
	fn oil_change_period() -> u32 { // self is not required
		6 // Oil change required every 6 months
	}
}

fn main() {
	println!("Car needs an Oil change every {} months",
		Car::oil_change_period());
}
```
It is customary in Rust to define a `constructor` pattern using a associated function called `new()`.
```rust
impl Car {
	fn new(name: String, year: u32) -> Self { // Constructor returns a new Self instance
	   Self {
		   onwer: name,
		   year: year,
		   fuel_level: 0.0,
		   price: 0,
	   }
	}
}

fn main () {
	let new_car: Car = Car::new("Orangie".to_string(), 2020); // Call the constructor
}
```