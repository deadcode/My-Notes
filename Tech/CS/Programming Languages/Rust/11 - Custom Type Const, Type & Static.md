# Type Aliases
A `type` alias creates a name for another type. This is similar to a `typedef` in C/C++.
```rust
enum DaysOfWeek { // enum keyword followed by name of enum
	Monday,    // enum variants named here seperated by commas
	Tuesday,   // Simple valriant as auto values
	Thursday,
	Wednesday,
	Friday,
	Sunday,
	Saturday,
}

type DayNames = DaysOfWeek;

fn main() {
	let my_day = DayNames::Sunday;
}
```
# Const
Like in C/C++, Rust allows to mark variables as constants using the `const` keyword. However Rust `const` function differently than C/C++. In C/C++ `const` makes the variable immutable, however all variables in Rust are already immutable by default. In Rust `const` marks the variable to be evaluated and inlined at compile time. In this way the `const` in Rust behaves similar to `constexpr` in C++.
Rust also allows functions to be marked `const` meaning they are inlined at compile time. However `const` functions can be called at runtime as well.
NOTE: Only `const` functions can be called at compile time to determine the value of `const` variables.
```rust
const DIGEST_SIZE: usize = 3;
const FILL_VALUE: u8 = calculate_fill_value(); // Fill value calculated at compile time.
const fn calculate_fill_value() -> u8 {
	if DIGEST_SIZE < 10 {
	   42
	} else {
	   13
	}
}

fn compute_digest(text: &str) -> [u8; DIGEST_SIZE] {
	let mut digest = [FILL_VALUE; DIGEST_SIZE]; // Create a digest array of size 3 and initialize it with value '42'
	for (idx, &b) in text.as_bytes().iter().enumerate() {
	   // Very rudimentary digest by doing wrapping add of overflow bits
	   digest[idx % DIGEST_SIZE] = digest[idx % DIGEST_SIZE].wrapping_add(b);
	}
	digest
}

fn main() {
	let digest = compute_digest("Bruce Wayne");
	println!("digest: {digest:?}"); // Prints: digest: [148, 199, 56]
}
```
# Static
Static variables have lifetime for the duration of the whole program. They are similar to global variables in C/C++ except they are immutable by default. Similar to global variables, access to `static` variables multiple threads needs to be synchronized (see Mutex and Sync). The Static variable is defined using the `static` keyword.
```rust
static BANNER: &str = "Welcome to RustOS 3.14"; // BANNER is accessible at runtime
fn main() {
	println!("{BANNER}");
}
```
Similar to C/C++ `static` variables can be defined inside of a function as well. The `static` keyword inside a function only limits the lexical scope of where the variable is accessible, how the variable is allocated does not change.
```rust
struct Token(u32);

impl Token {
	fn new() -> Self {
	   static mut COUNTER: u32 = 1; // Global COUNTER created inside the scope of new() function
	   let inner = unsafe {COUNTER+=1; COUNTER}; // unsafe block used because COUNTER is not locked by a MUTEX
	   Token(inner) // return ownership of new value
	}
}

fn main() {
	let t1 = Token::new();
	let t2 = Token::new();
	let t3 = Token::new();
	println!("{:?} {:?} {:?}", t1.0, t2.0, t3.0); // Prints: 2 3 4
}
```