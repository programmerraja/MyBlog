---
title : Rust
date : 2024-07-31T06:33:42.4242+05:30
draft : false
tags : 
---


Rust is static types language
Rustc  compiler convert rust to executable
cargo rust build system and pkg manager

cargo new projectname (like npm  in js)
 - cargo.tomo -> (similar to pkg.json file  in js)

```rust
fn main(){
 println!("hello world")
 println!("{}",1) // we cannot directly print number we need to format
}
```

cargo run filename  ->  compile to bin and run
rustc filename ->convert to bin and we need to run


Varaibles 
- are imutable by default 
- const my need to define the vairable types

```rust

let x =1
x=5  // will error
let x= 5; //no error we can redeclare to bypasss

//we also cannot chnage the type 
let a =1
a="hai"  ///error
let a ="hai" //allowed

//const must need to define the type

const A : u32 = 50;
```

## Types

Data type of every expression must be known at compile time.we don't need to explicitly specify the type of every variable, but the type system will infer the types based on how the variables are used. This is known as type inference.

**Scalar Types**
1. **Integers**:
    - `i32`: 32-bit signed integer (default)
    - `i64`: 64-bit signed integer
    - `u32`: 32-bit unsigned integer
    - `u64`: 64-bit unsigned integer
    - `isize`: Pointer-sized signed integer
    - `usize`: Pointer-sized unsigned integer
2. **Floats**:
    - `f32`: 32-bit floating-point number
    - `f64`: 64-bit floating-point number (default) 
3. **Characters**:
    - `char`: Unicode scalar value (e.g., 'a', '😊')

Example: `let x: char = 'a';`

4. **Boolean**:
    - `bool`: true or false

Example: `let x: bool = true;`

**Compound Types**

```rust
fn main() {

	//Array

    // 1. Creating an array
    let numbers = [1, 2, 3, 4, 5];
    println!("Array: {:?}", numbers);
    // 2. Accessing an array element
    println!("First element: {}", numbers[0]);
    // 3. Array length
    println!("Array length: {}", numbers.len());
    // 4. Array iteration
    for num in &numbers {
        println!("Number: {}", num);
    }
    // 5. Array mutation
    let mut numbers_mut = [1, 2, 3, 4, 5];
    numbers_mut[0] = 10;
    println!("Mutated array: {:?}", numbers_mut);
    // 6. Creating an array with a default value
    let mut bools = [false; 5];
    println!("Array of booleans: {:?}", bools);
    // 7. Array slicing
    let slice = &numbers[1..4];
    println!("Slice: {:?}", slice);

    // 8. Multidimensional arrays
    let matrix = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
    ];
    println!("Matrix: {:?}", matrix);


    // 1. Creating a tuple
    let person = ("John", 30, "Developer");
    println!("Name: {}, Age: {}, Occupation: {}", person.0, person.1, person.2);

    // 2. Pattern matching with tuples
    let (name, age, occupation) = person;
    println!("Name: {}, Age: {}, Occupation: {}", name, age, occupation);

    // 3. Tuple indexing
    println!("Name: {}", person.0);
    println!("Age: {}", person.1);
    println!("Occupation: {}", person.2);

    // 4. Tuple destructuring
    let (_, age, _) = person;
    println!("Age: {}", age);

    // 5. Creating a tuple with different types
    let mixed_tuple = (1, "hello", true, 3.14);
    println!("{:?}", mixed_tuple);

    // 6. Tuple as function return value
    let result = calculate_area(10, 20);
    println!("Area: {}", result.0);
    println!("Perimeter: {}", result.1);
}

fn calculate_area(length: i32, width: i32) -> (i32, i32) {
    let area = length * width;
    let perimeter = 2 * (length + width);
    (area, perimeter)
}
```

4. **Structs**:
    - A custom data type with named fields
 Example 
```rust
struct Person {
    name: String,
    age: u32,
}

let x: Person = Person {
    name: "Alice".to_string(),
    age: 30,
};
```

5. String : have two imutable fixed length and growable heap allocated data structure
```rust

fn main() {

	let s = "hello"
	//growable string
    let mut s = "   Hello, World!   ".to_string();

    // Trim whitespace from the beginning and end of the string
    s = s.trim().to_string();

    // Convert the string to uppercase
    s = s.to_uppercase();

    // Replace commas with dashes
    s = s.replace(",", "-");

    // Insert a substring at a specific position
    s.insert_str(7, " Beautiful ");

    // Remove a range of characters from the string
    s.remove(15..20);

    println!("{}", s); // Output: "HELLO- Beautiful WORLD!"
}
```

## Loops

```rust
fn main() {
    // 1. For loop
    let numbers = [1, 2, 3, 4, 5];
    for num in numbers {
        println!("For loop: {}", num);
    }

    // 2. While loop
    let mut i = 0;
    while i < 5 {
        println!("While loop: {}", i);
        i += 1;
    }

    // 3. Loop (infinite loop)
    let mut j = 0;
    loop {
        println!("Loop: {}", j);
        j += 1;
        if j == 5 {
            break;
        }
    }

    // 4. For loop with iterator
    let fruits = vec!["apple", "banana", "cherry"];
    for fruit in fruits {
        println!("For loop with iterator: {}", fruit);
    }

    // 5. For loop with range
    for k in 1..6 {
        println!("For loop with range: {}", k);
    }
}
```

## Macro
In Rust, a macro is a way to extend the language itself. Macros are essentially functions that generate code at compile-time, allowing you to create new syntax, abstractions, and domain-specific languages (DSLs) within Rust.

Macros are defined using the `macro` keyword, followed by the name of the macro and a set of rules that define how the macro should be expanded. When the macro is invoked, the Rust compiler will replace the macro invocation with the expanded code.

**Macros need to be call with !**

```rust
macro_rules! greet {
    ($name:expr) => {
        println!("Hello, {}!", $name);
    };
}

fn main() {
    greet!("Alice");
    greet!("Bob");
}
```

Types of Macros in Rust:

1. **Declarative Macros**: These macros are defined using the `macro_rules!` syntax and are used to generate code at compile-time.
2. **Procedural Macros**: These macros are defined using the `proc_macro` attribute and are used to generate code at compile-time using a procedural interface.
3. **Attribute Macros**: These macros are used to attach attributes to items, such as functions or structs.

### Builtin Macro

**1. `println!`**: Prints its arguments to the standard output, followed by a newline character.

**2. `vec!`**: Creates a new vector with the specified elements.

**3. `format!`**: Formats its arguments into a string.

**4. `assert!`**: Asserts that a condition is true, and panics if it's false.

**5. `assert_eq!`**: Asserts that two values are equal, and panics if they're not.

**6. `assert_ne!`**: Asserts that two values are not equal, and panics if they are.

**7. `panic!`**: Panics with a custom message.

**8. `unimplemented!`**: Panics with a message indicating that a function or method is not implemented.

**9. `todo!`**: Panics with a message indicating that a function or method is not implemented, and suggests that it should be implemented.

**10. `include!`**: Includes the contents of a file into the current module.

**11. `include_str!`**: Includes the contents of a file as a string.

**12. `module!`**: Defines a new module.

**13. `concat!`**: Concatenates its arguments into a single string.

**14. `stringify!`**: Converts its argument into a string.

**15. `debug_assert!`**: Asserts that a condition is true, and panics if it's false, but only in debug builds.


## Function

```rust
fn main() {
    // 1. Defining a function
    fn greet(name: &str) {
        println!("Hello, {}!", name);
    }
    greet("Alice");

    // 2. Defining a function with return value
    fn add(a: i32, b: i32) -> i32 {
        a + b
    }
    let result = add(2, 3);
    println!("Result: {}", result);

    // 3. Defining a closure
    let names = vec!["Alice", "Bob", "Charlie"];
    let greet = |name: &str| println!("Hello, {}!", name);
    for name in names {
        greet(name);
    }

    // 4. Defining a closure with capture
    let message = "Hello, ".to_string();
    let greet = move |name: &str| println!("{}", format!("{}{}", message, name));
    greet("Alice");

    // 5. Defining a closure as a function parameter
    fn process<F>(data: &str, func: F)
    where
        F: Fn(&str),
    {
        func(data);
    }
    process("Hello, World!", |s| println!("{}", s));

    // 6. Defining a closure as a function return value
    fn create_greeter(name: &str) -> impl Fn() {
        move || println!("Hello, {}!", name)
    }
    let greeter = create_greeter("Alice");
    greeter();
}
```