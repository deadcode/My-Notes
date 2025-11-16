Rust has an extensive Standard Library. The Standard Library itself has different layers:
1. core: most of the basic types and functions of Rust go here. This library can work even without the `libc` or even the OS.
2. alloc: types that need a global heap allocator go here e.g. the Vec, Box and Arc.
3. std: collections types and algorithms go here.
## Option
Rust does not have the `NULL` type. To facilitate indicating the optional presence of some value or no value the generic `Option` enum from the library can be used. The `Option` enum is extensively used in all the Rust libraries to return results. The `Option` types is defined using the `Option<type>` generics syntax, where 'type' indicates the type of value being returned.
Note: See [[xx- Generics]] for generics syntax.
```Rust
// How Option is defined in the library
enum Option<T> {
	None,
	Some(T),
}
```
`None` and `Some` are the variants defined to denote no value and any-non-null values.
```Rust
struct Student {
	name: String,
	grade: u32,
}

let student_list: Vec<Student> = vec![
	Student {
		name: String::from("Curly"),
		grade: 35,
	},
	Student {
		name: String::from("Larry"),
		grade: ,  // Error: Cannot leave un-initialized
	},
];
```
As in the example above when initializing the grades, they all need to be given values and cannot be left unassigned. We could designate special values e.g. '-1' to denote unknown or unevaluated grades but that makes is implementation dependent. To get around this `grade` can be defined as `Option<u32>` instead which will allow us to assign special value `None` to unknown grades.
```Rust
struct Student {
	name: String,
	grade: Option<u32>,
}

let student_list: Vec<Student> = vec![
	Student {
		name: String::from("Curly"),
		grade: Some(35),
	},
	Student {
		name: String::from("Larry"),
		grade: None,
	},
	Student {
		name: String::from("Moe"),
		grade: Some(10),
	},
];
```
Let us use this student list to implement a search function that finds a name in the list and returns the corresponding grade.
```Rust
fn get_student_grade(name: &String, student_list: &Vec<Student>) -> Option<u32> {
	// Find the student name in the list and return their grade if found else return None
	for student in student_list {
		if student.name == *name { // String comparision in Rust
			return student.grade;
		}
	}
	None
}

let student_name: String = String::from("Moe");
let student_grade: Option<u32> = get_student_grade(&student_name, &student_list);

// Handle the return from the search using a match
match student_grade {
	Some(grade: u32) => println!("{student_name}'s grade is: {grade}"), //Bind value in Option<u32> to grade variable
	None => println!("{student_name}'s grades have not been evaluated yet!"),
}
```
Note: If handling for None is not desired in the `match` above, it can be replaced with the `if let` control statement.
```Rust
if let Some(grade) = student_grade {
    println!("{student_name}'s grade is: {grade}");
}
```
## Result
`Result` is similar to `Option`. It is an `enum` provided by the Rust library to have a standard handling of return values from library functions. Similar to `Option` is also a generic enum with two generic types `Ok` and `Err`, for Success and the Error case.
```Rust
enum Result<T, E> {
	Ok(T),
	Err(E),
}
```
It is idiomatic to handle the `Result` of a function return in a `match`.

The previous implementation of student grade has a problem. When finding a student's grade we can only return a grade or None. The `None` return value cannot differentiate between different kinds of errors e.g. the student had invalid grades or student name itself was not in the list or maybe the student list itself was invalid. To differentiate these errors we can implement a student find function that returns an error if no name was found and we try to lookup grades only if the student name exists in the list.
```Rust
struct Student {
	name: String,
	grade: Option<u32>,
}

fn get_student_grade(name: &String, student_list: &Vec<Student>) -> Option<u32> {
// Find the student name in the list and return their grade if found else return None
	for student in student_list {
	   if student.name == *name { // String comparision in Rust
	       return student.grade;
	   }
	}
	None
}

fn is_a_student(name: &String, student_list: &Vec<Student>) -> Result<(), String> { // Returning the Result enum
	for student in student_list {
	   if student.name == *name {
	       return Ok(()); // Return Ok with empty/unit value since we only care about the return status not the value
	   }
	}
	Err (String::from("Student not found")) // Error string for what went wrong
}

fn main() {
	let student_list: Vec<Student> = vec![
		Student {
		   name: String::from("Curly"),
		   grade: Some(35),
		},
		Student {
		   name: String::from("Larry"),
		   grade: None,
		},
		Student {
		   name: String::from("Moe"),
		   grade: Some(10),
		},
	];

	let student_name: String = String::from("Mr. Bean");
	let is_enrolled = is_a_student(&student_name, &student_list);

	match is_enrolled {
	   Ok(_) => {
	       // Only get student grades if student exists
	       let student_grade: Option<u32> = get_student_grade(&student_name, &student_list);
	       if let Some(grade) = student_grade {
	           println!("{student_name}'s grade is: {grade}");
	       }
	   },
	   Err(err_msg) => println!("Error: {student_name} - {err_msg}"),
	}
}
```
Here we implement the `is_a_student()` function that returns the `Result`. The result could be `Ok` with empty (unit) value or `Err` with a string value.
This can further be refined. We can combine the `is_a_student()` and the `get_student_grade()` function into single function. The single function can lookup and return the `Option` grade for success `Ok` case and `Err` for failure case.
```Rust
struct Student {
	name: String,
	grade: Option<u32>,
}

fn find_and_get_student_grade(
    name: &String,
    student_list: &Vec<Student>
) -> Result<Option<u32>, String> { // Return Option<u32> instead of unit() for Ok
    for student in student_list {
        if student.name == *name {
            return Ok(student.grade); // Ok(Option<u32>)
        }
    }
    Err(format!("Error: Student {name} not found")) // Err(string)
}

fn main() {
	let student_list: Vec<Student> = vec![
		Student {
		   name: String::from("Curly"),
		   grade: Some(35),
		},
		Student {
		   name: String::from("Larry"),
		   grade: None,
		},
		Student {
		   name: String::from("Moe"),
		   grade: Some(10),
		},
	];

	let student_name: String = String::from("Mr. Bean");
	// Check student enrollment and get their grades
    let student_status = find_and_get_student_grade(&student_name, &student_list);

    match student_status {
        Ok(grade_option) => { // grade_option is bound to Option<u32> value
            if let Some(grade) = grade_option { // bind grade to u32 from Option
                println!("{student_name}'s grade is: {grade}");
            }
        },
        Err(err_msg) => println!("{err_msg}"),
    }
}
```