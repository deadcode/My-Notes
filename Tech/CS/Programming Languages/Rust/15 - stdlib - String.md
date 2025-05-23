# String
`String` is a growable UTF-8 encoded string. It's prototype is as below:
```Rust
pub struct String { /* private elements */ }
```
`String` is the most common string type in Rust. It has ownership over it's contents that are allocated on heap. The `str` primitive is its borrowed counterpart. Since `String` is UTF-8 encoded while a `char` in Rust is 4 bytes, it is typically smaller than an array of same chars. The `OsString` is the non-UTF-8 string.
`String` is internally represented by three components: a pointer to storage on the heap, a length (current) and a capacity (max length). The length is always lesser than the capacity. If the storage grows beyond the capacity the pointer might move to a new location.
```Rust
let mut s1 = String::new();
s1.push_str("Hello");
println!("s1: len = {}, capacity = {}", s1.len(), s1.capacity()); // Prints: s1: len = 5, capacity = 8

s1.push_str(" World");
println!("s1: len = {}, capacity = {}", s1.len(), s1.capacity()); // Prints: s1: len = 11, capacity = 16

let mut s2 = String::with_capacity(s1.len() + 1);
s2.push_str(&s1);
s2.push('!');
println!("s2: len = {}, capacity = {}", s2.len(), s2.capacity()); // Prints: s2: len = 12, capacity = 12
```
# Implementation & Methods
Some of the important methods are listed below.
- **`String::new()`** - Create a new empty String. Since the new String is empty, for optimization there maybe no allocation at creation time.
```Rust
pub const fn new() -> String
```
- **`String::len()`** - Returns the size of the String in bytes. Because of UTF-8 encoding this could be different from the size in characters.
```Rust
pub fn len(&self) -> usize
```
- `String::into_raw_parts()` - Decompose string into `(pointer, len, capacity)`.
```Rust
pub fn into_raw_parts(self) -> (*mut u8, usize, usize)
```
* `String::into_bytes()` - Convert `String` into a byte vector.
```Rust
pub fn into_bytes(self) -> Vec<u8>
```
- `String::as_str()` & `String::as_mut_str()` - Convert into a string slice or mutable string slice.
```Rust
pub fn as_str(&self) -> &str
pub fn as_mut_str(&self) -> &mut str
```
- `String::push_str()` - Append a string slice to the end of the String.
```Rust
pub fn push_str(&mut self, string: &str)
```
- `String::push()` - Append a `char` to the end of the String.
```Rust
pub fn push(&mut self, ch: char)
```
- `String::as_bytes()` - Return a byte slice
```Rust
pub fn as_bytes(&self) -> &[u8]
```
- `String::truncate()` - Shortens the string to specified length
```Rust
pub fn truncate(&mut self, new_len: usize)
```
- `String::pop()` - Removes and returns the last character (or None) from the String.
```Rust
pub fn pop(&mut self) -> Option<char>
```
- `String::remove_matches()` - Remove all matches of pattern from String
```Rust
pub fun remove_matches<P>(&mut self, pat: P)
let mut s = String::from("Trees are not green, the sky is not blue");
s.remove_matches("not ");
```
- `String::insert_str()` - Insert string slice into String at byte position. Will Panic if position is not on char boundary
```Rust
pub fn insert_str(&mut self, idx: usize, string: &str)
let mut s = String::from("bar");
s.insert_str(0, "foo");
```
- `String::is_empty()` - Returns `true` is String has len zero, `false` otherwise.
```Rust
pub fn is_empty(&self) -> bool
```
- `is_char_boundary()` - Checks if byte at index is the first byte of the UTF-8 code sequence.
```Rust
pub fn is_char_boundary(&self, index: usize) -> bool
```
- `split_whitespace()` - Split a string by (any) whitespace and return iterator over the string slices
```Rust
pub fn split_whitespace(&self) -> SplitWhitespace<'_>
let mut iter = "A few good men".split_whitespace();
assert_eq!(Some("A"), iter.next());
assert_eq!(Some("few"), iter.next());
assert_eq!(Some("good"), iter.next());
assert_eq!(Some("men"), iter.next());
assert_eq!(None, iter.next());
```
- `contains()` - Returns `true` if pattern matches a sub-slice of string.
```Rust
pub fn contains<P>(&self, pat: P) -> bool
let bananas = "bananas";
assert!(bananas.contains("nana"));
```
- `trim()` - Return a string slice with leading and trailing whitespace removed
```Rust
pub fn trim(&self) -> &str
let s = String::from("\n Hello World\t\n");
assert_eq!("Hello World", s.trim());
```

## Trait Implementations
Following common operations are supported
- `Add` or `+` operator. 
```Rust
let a = String::from("hello");
let b = String::from(" world");
let c = a + &b;
// `a` is moved and can no longer be used here.
let a = String::from("hello");
let b = String::from(" world");
let c = a.clone() + &b;
// `a` is still valid here.
```
Similar to the `+` operation, the `+=` operation is also supported.
- `From` - Convert from various representations (e.g. string slice, char, array) to a `String`
```Rust
let s1: String = String::from("hello world");
```
- `PartialEq` or == operator - Used to compare strings
```Rust
let s1 = String::from("Romeo");
let s2 = String::from("Juliet");
if s1 == s2 {
	println!("Found soulmates!!");
}
```
- `PartialOrd` - Used to sorting strings

# Reference
-[[ https://doc.rust-lang.org/std/string/struct.String.html#]]