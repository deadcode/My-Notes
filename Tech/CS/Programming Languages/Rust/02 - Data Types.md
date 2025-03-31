# Primitive Data Types

1. Unsigned Integers
```rust
let unsigned_num: u8 = 5;
// u16, u32, u64 & u128
```
2. Signed Integers
```rust
let signed_num: i8 = -5;
// i16, i32, i64 & i128
```
3. Floating Point Numbers
```rust
let float_num: f32 = 5.0;
// f64
```
4. Platform Specific Integers
```rust
let arch_1: usize = 5; // Pointer sized unsigned integer
let arch_2: isize = 5; // Pointer sized signed integer
```
5. Character
```rust
let character: char = `a`;
```
6. Boolean
```rust
let b: bool = true; // false
```

# Type Aliasing
Define a new type with "type" keyword.
```rust
type Age = u8;
let my_age: Age = 420;
```

# Type Conversion
Convert a type to another type for the sake of computation. Declared with the "as" keyword.
```rust
let a: i32 = 10;
let b: f64 = a as f64; // Temp convert a to f64 type
```

# Compound Data Types
## Strings
1. String Slice: Declared with "&str" keyword. The string slice is immutable.
```rust
let fixed_str: &str = "Fixed length string";
```
1. String Type: Comes from Rust Standard Lib. Declared with "String" keyword.
```rust
let mut flex_str: String = String::from("This string can grow");
flex_str.push(ch: 's'); // Push a character onto the String
```
## Arrays
Arrays hold multiple values of the same type. Declared with the "[]" syntax specifying the type of elements and number of elements in the array. If unspecified, type and length can be determined by the compiler. Once declared the size of the array cannot be changed.
```rust
let mut array_1: [i32, 5] = [4, 5, 6, 7, 8];
println!("array_1 = {?}", array_1); // Prints: 
```

Arrays are index starting at '0'. Array elements are accessed using the "[]" index notation.
```rust
let arr_1: [10, 11, 12, 13, 14];
let num = arr_1[3]; // num = 13
```

### Default Initializer
All elements can be initiazed to same default value using "value; elements" notation.
```rust
let arr_2: [i32, 10] = [0; 10]; // All 10 elements have zero '0' value
```

## Vectors
Vectors are similar to Arrays, but can unlike arrays they can grow. They are created using "vec!" macro.
```rust
let vec_1: Vec<i32> = vec![4, 5, 6, 7, 8];
let num = vec_1[3]; // num = 7
```

## Tuples
Like Arrays and Vectors, Tuples contain multiple values. But unlike Arrays and Vectors, Tuples can hold values of different data types.
```rust
let my_info: (&str, i32, &str, u32) = ("Salary", 400000, "Age", 420);
```
Elements can be accessed using "var_name.index" notation. Tuple index start at "0".
```rust
let my_salary: i32 = my_info.1; //
```
Tuples can be deconstructed by assignment.
```rust
let (salary: &str, my_salary: i32, age: &str, my_age: u32) = my_info;
// salary="Salary", my_salary=400000, age="Age", my_age=420
```
Empty tuples are called "unit" type. They are sized zero and do not consume memory.
```rust
let unit: () = ();
```