Like in C/C++, `enum` in Rust allows to define a type that enumerates all the types of a variable type. Enum is defined with the `enum` keyword followed by the name of the type. The different values in the Enum are called the "variants" of the Enum. The variant of the enum are accessed using the "::" syntax similar to the associated methods in a struct.
```rust
enum DaysOfWeek { // enum keyword followed by name of enum
	Monday,    // enum variants named here seperated by commas
	Tuesday,   // Simple valriant as auto values
	Thursday,
	Wednesday,
	Friday,
	Sunday,
	Saturday,
} // no semi colon at the end

fn main() {
	let first_day_of_work: DaysOfWeek = DaysOfWeek::Monday;
}
```
Just like C/C++ the values given to variants (called "discriminant") can be explicitly provided.
```rust
enum DaysOfWeek {
	Monday,    // = 0
	Tuesday,   // = 1
	Thursday = 100,  // = 100
	Wednesday, // = 101
	Friday,
	Sunday,
	Saturday,
} // no semi colon at the end

fn main() {
	println!("Monday: {}", DaysOfWeek::Monday as u32); // Prints: Monday: 0
	println!("Tuesday: {}", DaysOfWeek::Tuesday as u32); // Prints: Tuesday: 1
	println!("Thursday: {}", DaysOfWeek::Thursday as u32); // Prints: Thursday: 100
	println!("Wednesday: {}", DaysOfWeek::Wednesday as u32); // Prints: Wednesday: 101
	println!("Friday: {}", DaysOfWeek::Friday as u32); // Print: Friday: 102
}
```
An `enum` in C/C++ default to `integer` types, but Rust allows to mix different data types to be mixed in an Enum. So the user could create an `enum` with variants as `integers`, Strings or even `struct`. So in a way `enum` in Rust behaves a lot like a `struct`.
```rust
enum Message {
    Quit,                       // No data
    Move { x: i32, y: i32 },    // Struct-like data
    Write(String),              // Tuple-like variant with a String
    ChangeColor(u8, u8, u8),    // Tuple-like variant with three u8 values
}
fn main() {
    let msg1 = Message::Quit;
    let msg2 = Message::Move { x: 10, y: 20 }; // The value of the variant is part of the variable definition and needs to provided
    let msg3 = Message::Write(String::from("Hello, Rust!"));
    let msg4 = Message::ChangeColor(255, 0, 0);
    let msg5_white = Message::ChangeColor(255, 255); // Error: missing value for last field of color tuple

    process_message(msg2);
}

fn process_message(msg: Message) {
    match msg { // match arms have to exhastive
        Message::Quit => println!("The Quit variant has no data."),
        Message::Move { x, y } => println!("Move to coordinates: ({}, {})", x, y),
        Message::Write(text) => println!("Text message: {}", text),
        Message::ChangeColor(r, g, b) => println!("Change color to RGB({}, {}, {})", r, g, b),
    }
}
```
A common pattern in Rust is to use `match` on `enum`. The benefit of this is that Rust enforces the `match` patterns to be exhaustive, every variant of the `enum` will get matched.
Just like Structs, enums can also use the `impl` implement block to write "methods" that operate on them.
```rust
enum Message {
    Quit,                       // No data
    Move { x: i32, y: i32 },    // Struct-like data
    Write(String),              // Tuple-like variant with a String
    ChangeColor(u8, u8, u8),    // Tuple-like variant with three u8 values
}

impl Message {
	fn print(&self) { // Borrows immutable self reference
		match self {
			Message::Quit => println!("The Quit variant has no data."),
            Message::Move { x, y } => println!("Move to coordinates: ({}, {})", x, y),
            Message::Write(text) => println!("Text message: {}", text),
            Message::ChangeColor(r, g, b) => println!("Change color to RGB({}, {}, {})", r, g, b),
        }
    }
}

fn main() {
    let msg1 = Message::Quit;
    let msg2 = Message::Move { x: 10, y: 20 };
    let msg3 = Message::Write(String::from("Hello, Rust!"));
    let msg4 = Message::ChangeColor(255, 0, 0);

	msg1.print(); // The Quit variant has no data.
	msg2.print(); // Move to coordinates: (10, 20)
	msg3.print(); // Text message: Hello, Rust!
	msg4.print(); // Change color to RGB(255, 0, 0)
}
```
## Enum as Union
As seen, a enum can be used to store variants of different types. But a given Enum variable can have only one type of variant. In this sense Enum can also be thought of similar to `union` in C/C++. This property of Enum can be used to store different value types in a "Vector" (A vector enforces that all the types in the store have to be the same type). Just like the C/C++ compiler, Rust compiler will take care to allocate the minimum amount of storage required for the combined data value.
```Rust
enum Value {
	Integer(u32),
	Float(f32),
} // enum or union of integer and floating point values
impl Value {
	fn print(&self) {
		match self {
			Value::Integer(val) => println!("Integer: {val}"),
			Value::Float(val) => println!("Float: {val}"),
		}
	}
}
fn main() {
	let val_vec = vec![Value::Integer(420),
	   Value::Float(42.0),
	   Value::Integer(69)];

	for i in &val_vec { // Notice the referece to val_vec is used for iteration
		i.print();
	}
	val_vec[1].print(); // This would not work if a reference wasn't used above
}
```