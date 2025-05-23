# Vec
Vec is a collection of data elements that are stored on the heap. It is resizable (i.e. it can grow and shrink at runtime).
```Rust
pub struct Vec<T, A = Global> // A is the optional allocator
{ /* private fields */ }
```
`Vec` provides is like a growable Array and guarantees contiguous memory allocation. It is short for 'Vector'. The `Vec::new()` method is used to create an empty `Vec`. Alternatively the `vec!()` macro can also be used to allocate and also initialize the contents of the Vector.
```Rust
let mut v1 = Vec::new();
v1.push(42);
println!("v1: len = {}, capacity = {}", v1.len(), v1.capacity());

v1.extend([1, 2, 3]);
assert_eq!(v1, [42, 1, 2, 3]);

let mut v2 = vec![1, 2, 3];
v2.push(4);
let v3 = Vec::from([1, 2, 3, 4]);
assert_eq!(v2, v3);

let v4 = vec![0; 5]; // Initilize len 5 Vec all with values 0
assert_eq!(v4, [0, 0, 0, 0, 0]);
```
`Vec` can be indexed using the array syntax `[]`. Like arrays indexes start at `0`. However there is no compile time check to make sure index is within range. Trying to index beyond the length of `Vec` will result in panic. The `get()` and `get_mut()` methods can be used to check if index is valid.
```Rust
let v = vec![0, 2, 4, 6];
println!("{}", v[1]); // it will display '2'
println!("{}", v[6]); // it will panic!
```
Just like a `String` a `Vec` is represented by the triple `(pointer, capacity, length`. Below is a representation of a `Vec` with two elements.
```Rust
let vec = vec![42, 69];
/* 
vec->		ptr      len  capacity
       +--------+--------+--------+
       | 0x0123 |      2 |      4 |
       +--------+--------+--------+
            |
            v
Heap   +--------+--------+--------+--------+
       |    42  |    69  | uninit | uninit |
       +--------+--------+--------+--------+
*/
```
# Implementation and Methods
- `Vec::new()` - Construct a new empty `Vec<T>`.
```Rust
pub const fn new() -> Vec<T>
```
- `Vec::with_capacity()` - Construct a new empty `Vec<T>` with atleast the specified capacity.
```Rust
pub fn with_capacity(capacity: usize) -> Vec<T>
```
- `Vec::into_parts()` - Decompose into parts `(NonNull pointer, length, capacity)`.
```Rust
pub fn into_parts(self) -> (NonNull<T>, usize, usize)

let v: Vec<i32> = vec![-1, 0, 1];
let (ptr, len, cap) = v.into_parts();
```
- `Vec::shrink_to()` - Shrink vector to min of current length and specified size.
```Rust
pub fn shrink_to(&mut self, min_capacity: usize)
```
- `Vec::truncate()` - Shortens the Vec to the specified len and drops the extra elements
```Rust
pub fn truncate(&mut self, len: usize)
```
- `Vec::insert()` - Insert an element at specified index shifting rest of the elements to the right. Panics if index > len.
```Rust
pub fn insert(&mut self, index: usize, element: T)
```
- `Vec::remove()` - Remove and return an element at specified index shifting remaining elements to the left. Panics if index > len.
```Rust
pub fn remove(&mut self, index: usize) -> T
```
- `Vec::push()` - Append an element to the back of the vector
```Rust
pub fn push(&mut self, value: T)
```
- `Vec::pop()` - Remove and return the last element of the Vector or None if empty.
```Rust
pub fn pop(&mut self) -> Option<T>
```
- `Vec::append()` - Moves the elements of other slice into self and empties the other.
```Rust
pub fn append(&mut self, other: &mut Vec<T, A>)
```
- `Vec::len()` - Returns the number of elements in the Vector.
```Rust
pub fn len(&self) -> usize
```
- `Vec::is_empty()` - Returns `true` if vector has no elements.
```Rust
pub const fn is_empty(&self) -> bool
```
- `Vec::get() - Returns a refernece to element at index or a subslice if range is specified
```Rust
pub fn get<I>(&self, index: I) -> Option<&<I as SliceIndex<[T]>>::Output>

let v = [10, 40, 30];
assert_eq!(Some(&40), v.get(1));
assert_eq!(Some(&[10, 40][..]), v.get(0..2));
assert_eq!(None, v.get(3));
assert_eq!(None, v.get(0..4));
```
- `Vec::reverse()` - Reverses the order of elements in place
```Rust
pub fn reverse(&mut self)
```
- `Vec::iter()` - Returns an iterator
```Rust
pub fn iter(&self) -> Iter<'_, T>

let x = &[1, 2, 4];
let mut iterator = x.iter();
assert_eq!(iterator.next(), Some(&1));
assert_eq!(iterator.next(), Some(&2));
assert_eq!(iterator.next(), Some(&4));
assert_eq!(iterator.next(), None);
```
- `Vec::split_at()` - Divides into two at the index
```Rust
pub fn split_at(&self, mid: usize) -> (&[T], &[T])
// first slice from [0, mid) (excluding mid)
// second slice from [mid, len) (excluding at len)

let v = ['a', 'b', 'c'];
let (left, right) = v.split_at(2);
assert_eq!(left, ['a', 'b']);
assert_eq!(right, ['c']);
```
- `Vec:contains()` - Returns `true` if Vec contains element with given value
```Rust
pub fn contains(&self, x: &T) -> bool
```
- `Vec::binary_search()` - Binary search for element with value assuming the vector is sorted. Result is unspecified if vector is un-sorted.
```Rust
pub fn binary_search(&self, x: &T) -> Result<usize, usize>

let s = [0, 1, 1, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55];
assert_eq!(s.binary_search(&13),  Ok(9)); // OK(index) where element is found
assert_eq!(s.binary_search(&4),   Err(7)); // Err(index) where element could be inserted while maintaining sorted order
```
- `Vec::is_sorted()` - Checks if the vector is sorted.
```Rust
pub fn is_sorted(&self) -> bool
```
- `Vec::sort()` - Sorts the vector and preserve original order for duplicate elements. Worst case order of `O(n * log(n))`

# Reference
- [Struct Vec ](https://doc.rust-lang.org/std/vec/struct.Vec.html)