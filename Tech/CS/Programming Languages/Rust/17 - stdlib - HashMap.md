# Struct HashMap
This is the standard hash map. Rust `HashMap` implements protection against the HashDoS attacks.
```Rust
pub struct HashMap<K, V, S = RandomState> { /* private fields */ }
```
The default hash algorithm used is the `SipHash1-3` which provides reasonable performance with medium length keys. However performance for short keys e.g. Integers or extremely long strings may not be that good. The default hashing algorithm can be replaced per `HashMap` using the `with_hasher()` method.
*Note: The hash table implementation is a Rust port of Google’s [SwissTable](https://abseil.io/blog/20180927-swisstables).*

```Rust
use std::collections::HashMap;

// Type inference lets us omit an explicit type signature (which
// would be `HashMap<String, String>` in this example).
let mut book_reviews = HashMap::new();

// Review some books.
book_reviews.insert(
    "Adventures of Huckleberry Finn".to_string(),
    "My favorite book.".to_string(),
);
book_reviews.insert(
    "Grimms' Fairy Tales".to_string(),
    "Masterpiece.".to_string(),
);
book_reviews.insert(
    "Pride and Prejudice".to_string(),
    "Very enjoyable.".to_string(),
);
book_reviews.insert(
    "The Adventures of Sherlock Holmes".to_string(),
    "Eye lyked it alot.".to_string(),
);

if !book_reviews.contains_key("Les Misérables") {
    println!("We've got {} reviews, but Les Misérables ain't one.",
             book_reviews.len());
}

// Look up the value for a key (will panic if the key is not found).
println!("Review for Jane: {}", book_reviews["Pride and Prejudice"]);

book_reviews.remove("The Adventures of Sherlock Holmes");

let to_find = ["Pride and Prejudice", "Alice's Adventure in Wonderland"];
for &book in &to_find {
    match book_reviews.get(book) {
        Some(review) => println!("{book}: {review}"),
        None => println!("{book} is unreviewed.")
    }
}

// Iterate over everything.
for (book, review) in &book_reviews {
    println!("{book}: \"{review}\"");
}
```
## Custom Key for `HashMap`
The easiest way to use `HashMap` with a custom key type is to derive [`Eq`](https://doc.rust-lang.org/std/cmp/trait.Eq.html "trait std::cmp::Eq") and [`Hash`](https://doc.rust-lang.org/std/hash/trait.Hash.html "trait std::hash::Hash"). We must also derive `PartialEq`. Or we must define those `traits` for the custom keys.
```Rust
use std::collections::HashMap;

#[derive(Hash, Eq, PartialEq, Debug)]
struct Viking {
    name: String,
    country: String,
}

impl Viking {
    /// Creates a new Viking.
    fn new(name: &str, country: &str) -> Viking {
        Viking { name: name.to_string(), country: country.to_string() }
    }
}

// Use a HashMap to store the vikings' health points.
let vikings = HashMap::from([
    (Viking::new("Einar", "Norway"), 25),
    (Viking::new("Olaf", "Denmark"), 24),
    (Viking::new("Harald", "Iceland"), 12),
]);

// Use derived implementation to print the status of the vikings.
for (viking, health) in &vikings {
    println!("{viking:?} has {health} hp");
}
```
# Implementation and Methods
- `HashMap::new()` - Create an empty HashMap
```Rust
pub fn new() -> HashMap<K, V, RandomState>
// Example
use std::collections::HashMap;
let mut map: HashMap<&str, i32> = HashMap::new();
```
- `HashMap::with_hasher()` - Create an empty HashMap with the given hash function to hash keys.
```Rust
pub const fn with_hasher(hash_builder: S) -> HashMap<K, V, S>
```
- `HashMap::keys()` - Return an iterator for visiting all keys in arbitrary order. The iterator elements type is `&'a K`
```Rust
pub fn keys(&self) -> Keys<'_, K, V>
```
- `HashMap::values()` - Return an iterator for visiting all values in arbitrary order. The iterator element type is `&'a V`.
```Rust
pub fn values(&self) -> Values<'_, K, V>
```
- `HashMap::iter()` - Return an iterator for visiting key value pairs in arbitrary order. The iterator element type is `(&'a K, &'a V)`.
```Rust
pub fn iter(&self) -> Iter<'_, K, V>
```
- `HashMap::len()` - Return the number of elements in the HashMap
```Rust
pub fn len(&self) -> usize
```
- `HashMap::is_empty()` - Return `true` if map no elements.
```Rust
pub fn is_empty(&self) -> bool
```
- `HashMap::clear()` - Remove all key-value pairs
```Rust
pub fn clear(&mut self)
```
- `HashMap::get()` - Get the reference to value for the given key.
```Rust
pub fn get<Q>(&self, key: &Q) -> Option<&V>
// Example
use std::collections::HashMap;

let mut map = HashMap::new();
map.insert("Hello", 5);
map.insert("Mr Parker", 8);
println!("Hello value = {}", map["Hello"]);
assert_eq!(map.get("Hello"), Some(&5));
```
- `HashMap::contains_key()` - Returns `true` if the map contains a value for the specified key.
```Rust
pub fn contains_key<Q>(&self, k: &Q) -> bool
// Example
use std::collections::HashMap;

let mut map = HashMap::new();
map.insert(1, "a");
assert_eq!(map.contains_key(&1), true);
assert_eq!(map.contains_key(&2), false);
```
- `HashMap::insert()` - Insert a key-value pair into the map. Returns `None` if key is inserted. If the key was already present, update with new value and return the old value.
```Rust
pub fn insert(&mut self, k: K, v: V) -> Option<V>
// Example
use std::collections::HashMap;

let mut map = HashMap::new();
assert_eq!(map.insert(37, "a"), None);
map.insert(37, "b");
assert_eq!(map.insert(37, "c"), Some("b"));
assert_eq!(map[&37], "c");
```
- `HashMap::remove()` - Remove the key from the map and return the value if the key was found.
```Rust
pub fn remove<Q>(&mut self, k: &Q) -> Option<V>
// Example
use std::collections::HashMap;

let mut map = HashMap::new();
map.insert(1, "a");
assert_eq!(map.remove(&1), Some("a"));
assert_eq!(map.remove(&1), None);
```