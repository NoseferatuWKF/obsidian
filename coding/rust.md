## Docs

[the-book](https://doc.rust-lang.org/book/)
[rust-by-example](https://doc.rust-lang.org/rust-by-example/index.html)
[rust-blog](https://github.com/pretzelhammer/rust-blog/tree/master)

## Installation

rustup
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
```

## Guidelines

- by default every variable is immutable
- try not to use unwraps and clones
- instead of void, rust uses an empty tuple (), which is known as a unit
- by default the items in a module are private

## When not to use rust?

- when you want to use unsafe operations (performance purposes)
- do not like the expressiveness of rust
- want to collect garbage 
- want to have production issues that is hard to debug

## Basics

>by default rust compiler will throw an error for unused variables, use underscore to bypass this
```rust
let mut mutable_var = 0; // mut here will make this variable mutable
let some_var = "this will throw an error if unused";
let _another_var = "this will not throw an error even when unused";
```

>`usize` is the largest unsigned int depends on the machine
```rust
let max: usize = usize::MAX; // can different results on different machines
```

>you can put underscores for large numbers to make it more readable
```rust
let big_num: u32 = 1_000_000;
```

using crates
```rust
// use::<crate>::<mod>::<Item>
use::std::collections::Hashmap;
// use only certain items
use::sync::{Arc, Mutex};
// use all items within mod
use::std::collections::*;
```

macro and attribute macro
```rust
#[allow(unused)] // this is a attribute macro
fn main() {
	println!("Hello"); // this is a macro
}
```

value and borrow
```rust
let mut x = 5; // this is value
let y = &x; // this is a borrow / read-only reference
let z = &mut x; // this is a mutable borrow / read and write reference
let new_x = x; // now new_x has the value of x and x no longer owns a value
```

**shadowing**
>Variable with the same name and different data types can be set
```rust
let age: &str = 29;
let mut age: u32 = age.trim().parse().expect("Whoops");
```

[let vs const](https://doc.rust-lang.org/std/keyword.const.html)
>Sometimes a certain value is used many times throughout a program, and it can become inconvenient to copy it over and over. What’s more, it’s not always possible or desirable to make it a variable that gets carried around to each function that needs it. In these cases, the `const` keyword provides a convenient alternative to code duplication:
```rust
const THING: u32 = 0xABAD1DEA; // global constant and must be explicit!
let foo = 123 + THING;
// the only lifetime allowed for const is 'static
const WORDS: &'static str = "hello rust!";
// due to static lifetime elision, 'static can be ignored
const WORDS: &str = "hello convenience!"; 
```

[let-else](https://doc.rust-lang.org/rust-by-example/flow_control/let_else.html)
>similar to a ternary operator
```rust
let condition: bool = true;
let ternary = if condition == true {
	true
} else {
	false
};
```

pattern matching
```rust
let speed_limit: u32 = 60;
match speed_limit { // must include all possible values
	1..=59 => println!("you are within speed limit"); // = range including value
	60 => println!("max speed limit allowed");
	_ => println!("you are over the speed limit"); // _ is for the rest
}
```

? operator
>can only be used with Result types
```rust
// without ? operator
fn main() -> Result<(), Box<dyn Error>> { 
    let greeting_file = match File::open("hello.txt") {
	    OK(),
	    Err(),
    }
}
// ? operator can be used to astract result and option types
fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;
    Ok(())
}
```

arrays
```rust
let arr: [i32; 3] = [1,2,3]; // default to i32
let mut i = 0; // if a type is not specified this will be usize
// this is one way to do a loop
loop {
	println!("value is {} for index {}", arr[i], i);
	i += 1;
	// to get out of loop
	break; 
}
// while loop
while i < arr.len() {
	println!("Value = {}", arr[i]);
	i += 1;
	break;
}
// for loop with iterator
for val: &32 in arr.iter() { // make sure the type is referencing correctly
	println!("Value = {}", val);
}
```

vectors
>Vectors are smart pointers
```rust
    let mut foo: Vec<&str> = Vec::new(); // instantiate an empty vector
    let bar: Vec<u32> = vec![1, 2, 3]; // another way of instantiating a vector
    let baz = Vec::from([1, 2, 3]); // yet another way to instantiate a vector
    foo.push("Pushed");
    foo.pop();
    let _bar2 = bar[0]; // what if index does not have any value?
    let bar3 = baz.get(0); // this is a safer way to access value
    match bar3 {
        Some(value) => println!("value exists: {}", value),
        None => println!("value doesn't exist"),
    };
    for b in baz.iter() { // looping vectors
	    println!("{}", b);
    }
    // infer from iterator
	let infer: Vec<_> = vec![1, 2, 3] // the _ is telling rust to infer from map
		.iter()
		.map(|x| x + 1) // the || is a closure
		.collect();
```

tuples
```rust
let tuple: (u32, String, f32) = (29, "John".to_string(), 10_000.00); 
// need to use to_string() or into() to let Rust infer or the type will be &str
println!("{} is {}, his salary is {}", tuple.1, tuple.0, tuple.2);
// pattern matching kinda like destructring
let (t1: u32, t2: String, t3: f32) = tuple;
// will print same thing as above
println!("{} is {}, his salary is {}", t2, t1, t3); 
```

String and &str
>a String is mutable and stored in the heap, &str is immutable and commonly known as a slice, also has a UTF-8 encoding

>String is a smart pointer

```rust
    let mut foo: String = String::new(); // instantiate a new mutable String
    let _bar: &str = "Hello Rust"; // create a new &str
    foo.push('A'); // you can push a char into a String
    foo.push_str(" string"); // you can push a String into another mutable String
    println!("{}", foo); 
    for f in foo.split_whitespace() {
        println!("{}", f);
    }
    let foo2 = foo.replace("A", "Some"); // assigned to a variable to avoid dangling
    println!("{}", foo2);
    
    let foo3 = String::from("Hello again"); // instantiate a String with a value
    let mut str_vec: Vec<char> = foo3.chars().collect();
    str_vec.sort(); // sorting
    str_vec.dedup(); // remove duplicates
    for c in str_vec {
        println!("{}", c);
    }
    let bar2 = "this is &str";
    let mut foo4 = bar2.to_string(); // convert to a String into the heap
    println!("string in bytes {:?}", foo4.as_bytes()); // convert to bytes
    println!("string cut short {:?}, actual length is {:?}", &foo4[0..3], foo4.len());
    let foo5 = String::from("the beginning ") + &foo4; // combining two Strings
    println!("{}", foo5);
    foo4.clear(); // turn into empty String"
```

casting
```rust
    let foo: u32 = 10;
    let bar: u32 = 29;
    let _baz: u64 = (foo as u64) + (bar as u64);
```

enum
```rust
enum Shapes { // a warning will show if any of the enum is not constructed
    Square,
    Pentagon,
    Triangle,
    Circle,
}

impl Shapes {
    fn is_polygon(&self) -> bool { // method referencing to self
        match self {
            Shapes::Circle => false,
            Shapes::Square | Shapes::Pentagon => true,
            _ => true,
        }
    }
}

fn main() {
    let shape = Shapes::Circle;
    match shape {
        Shapes::Square => println!("I am a Square, I have 4 sides"),
        Shapes::Pentagon => println!("I am a Pentagon, I have 5 sides"),
        Shapes::Triangle => println!("I am a Triangle, I have 3 sides"),
        Shapes::Circle => println!("I am a Circle, I don't have any sides"),
    }
    println!("is this shape a polygon? {}", shape.is_polygon());
}
```

closures
```rust
    let mut foo = 10;
    println!("{}", foo); // 10
    let mut closure = |x: i32| foo += x; // closure mutates foo
    closure(1);
    println!("{}", foo); // 11
    foo = 1; // normal mutation
    println!("{}", foo); // 1
```

function
```rust
fn main() {
    foo(1);
    let (_a, _b) = bar(2, 3);
    let v = vec![1, 2, 3];
    ref_fn(&v);
}

fn foo(x: u32) -> u32 { 
    x // will return value if no semicolon
}

fn bar(x: u32, y: u32) -> (i64, i64) {
    ((x as i64), (y as i64)) // returning two values 
}

fn ref_fn(x: &Vec<u32>) -> &u32 {
    match x.get(0) {
        Some(v) => v,
        None => &0,
    }
}
```

trait, struct, impl
>this is probably a bad example
```rust
trait Car { // methods
    fn new(x: u32, y: u32, z: String) -> Self; // Constructor
    fn print_manufacturer(&self) -> ();
    // https://github.com/rust-lang/rfcs/pull/1546
    // can't access struct fields here
    fn get_specs(&self) -> String; 
}

struct Supra { // fields
    hp: u32,
    torque: u32,
    _manufacturer: String,
}

struct Skyline {
    hp: u32,
    torque: u32,
    _manufacturer: String,
}

struct Lancer {
    hp: u32,
    torque: u32,
    manufacturer: String,
}

impl Car for Supra {
    fn new(x: u32, y: u32, z: String) -> Supra {
        return Supra { hp: x, torque: y, _manufacturer: z };
    }
    
    fn get_specs(&self) -> String {
        return print_specs(&self.hp, &self.torque);
    }

    fn print_manufacturer(&self) -> () {
        todo!() // to mark something as not yet implemented
    }
}

impl Car for Skyline {
    fn new(x: u32, y: u32, z: String) -> Skyline {
        return Skyline { hp: x, torque: y, _manufacturer: z };
    }
    
    fn get_specs(&self) -> String {
        return print_specs(&self.hp, &self.torque);
    }

    fn print_manufacturer(&self) -> () {
        todo!()
    }
}

impl Car for Lancer {
    fn new(x: u32, y: u32, z: String) -> Lancer {
        return Lancer { hp: x, torque: y, manufacturer: z };
    }
    
    fn get_specs(&self) -> String {
        return print_specs(&self.hp, &self.torque);
    }

    fn print_manufacturer(&self) -> () {
        println!("{}", &self.manufacturer);
    }
}

fn print_specs(hp: &u32, torque: &u32) -> String {
	// this fn own this variable
    let mut specs = String::from("Horsepower: ") + &hp.to_string();
    specs.push_str("\nTorque: ");
    specs.push_str(&torque.to_string());
    return specs;
}

fn main() {
    let lancer: Lancer = Lancer::new(105, 191, String::from("Mitsubishi"));
    println!("Lancer Evolution");
    println!("{}", lancer.get_specs()); // this one returns string
    lancer.print_manufacturer(); // this one already prints
}
```

[smart-pointers](https://doc.rust-lang.org/book/ch15-00-smart-pointers.html)
>smart pointers can own the data they point to and allows manipulation of said data. It also has extra metadata and capabilities

```rust
struct Node<T> {
    _val: T,
    next: Option<Box<Node<T>>>, // Box is a smart pointer to the heap
}

impl<T> Node<T> {
    fn new(val: T) -> Self {
        Node { _val: val, next: None } // a semicolon here breaks everything why?
    }

	// not really how you would create a linked list but just to show an example
    fn insert(mut self, node: Node<T>) -> Self {
        self.next = Some(Box::new(node));
        self
    }
}

fn main() {
    let linked_list = Node::new(1);
    linked_list.insert(Node::new(2))
}
```

lifetimes
[video](https://www.youtube.com/watch?v=juIINGuZyBc&ab_channel=Let%27sGetRusty)
```rust
// &'a str - this is a borrow with an explicit lifetime
// &'a mut str - this is a mutable borrow with an explicit lifetime
// &'static - this is a lifetime that will last for the whole program
// T: 'static - static bound lifetime not to be confused with &'static

fn main() {
    const A: i32 = -10;
    let b: i32 = 10;
    life_time(&A, &b);
}

// function with a generic lifetime annotation
// this creates a relationship of the variable lifetimes
fn life_time<'z>(a: &'z i32, b: &'z i32) -> &'z i32 {
    // a.to_string is ephemeral so need to bind it to a variable
    let binding = a.to_string();
    let life_time = LifeTime {
        life: binding.as_str(), // can also pass it as &binding
        time: a
    };

    // aviator operator?
    println!("{}:{}", life_time.life, life_time.time);

    return match a > b {
        true => a,
        false => b,
    };
}

// struct with a generic lifetime annotation
// need to specify a generic lifetime annotation if a field is a referenced type
struct LifeTime<'a, 'b> {
	life: &'a i32,
	time: &'b str,
	stand_alone: String,
}
```

## Modules

>`mod.rs` is the entrypoint for a mod directory, similar to `index.js` or `init.lua`

folder structure
```
├── car
│   ├── engine.rs
│   ├── mod.rs  
└── main.rs
```

engine.rs
```rust
pub struct Engine {
    pub manufacturer: String,
    pub displacement: u32,
    pub no_of_cylinders: u32,
    pub configuration: String,
    pub horsepower: u32,
    pub torque: u32,
}

impl Engine {
    pub fn start_engine(&self) { // associated fn
        unreachable!(); // kinda similar to todo! but signifies an error
    }

    pub fn _stop_engine(&self) {
        todo!();
    }
}

pub trait Specs {
    fn new(
        manufacturer: String,
        displacement: u32,
        no_of_cylinders: u32,
        configuration: String,
        horsepower: u32,
        torque: u32,
    ) -> Self;
    fn get_cylinder_volume(&self) -> f32;
}

impl Specs for Engine {
    fn new(
        manufacturer: String,
        displacement: u32,
        no_of_cylinders: u32,
        configuration: String,
        horsepower: u32,
        torque: u32,
    ) -> Self {
        // kinda like the thing you can do in typescript
        Engine { manufacturer, displacement, no_of_cylinders, configuration, horsepower, torque }
    }

    fn get_cylinder_volume(&self) -> f32 {
        todo!()
    }
}

pub mod engine_defaults { // nested mod
    use super::{Engine, Specs}; // inherit from parent mod
    pub fn _set_firing_order() {
        todo!();
    }

    pub fn _create_mock_engine() -> Engine {
        let engine = Engine::new("mock".to_string(), 1000, 4, "V".to_string(), 100, 100);
        self::_set_firing_order(); // call fn within same mod
        super::Engine::start_engine(&engine); // call parent fn directly
        return engine;
    }
}
```

mod.rs
```rust
pub mod engine;
```

main.rs
```rust
mod car;

use car::engine::{Engine, Specs};

fn main() {
    let engine: Engine = Engine::new(String::from("Nissan"), 2568, 6, String::from("inline"), 276, 260);
    engine.start_engine();
}
```

## Concurrency

thread
```rust
use std::thread;

fn main() {
    let t = thread::spawn(|| job("thread"));
    job("main");
    // to make sure all threads finish executing
    match t.join() {
        Ok(_) => {},
        Err(_) => panic!("Oh my thread"),
    }
}

fn job(who: &str) {
    for i in 1..50 {
        println!("Iterating {} times in {}", i, who);
    }
}
```

> If a single piece of data must be accessible from more than one task concurrently, then it must be shared using synchronization primitives such as Arc.

[arc](https://doc.rust-lang.org/std/sync/struct.Arc.html) (atomically reference counted) and [mutex](https://doc.rust-lang.org/std/sync/struct.Mutex.html) (mutual exclusion)
```rust
use std::{sync::{Arc, Mutex}, thread};

struct Tank {
    fuel: usize,
}

fn main() {
    let fuel_tank = Arc::new(Mutex::new(Tank {fuel: 5000}));
    let pistons = (1..5).map(|x| {
		// clone is adding an atomic reference count to this variabel
		// it is a fancy way of saying that it is creating a shallow copy
		// this is also different than how Rc(reference count) clone
        let fuel_tank_ref = fuel_tank.clone(); 
        thread::spawn(move || { // moving ownership of x
            consume_fuel(&fuel_tank_ref, 5, x); // let main fn keep ownership
        })
    });
    for p in pistons {
        p.join().unwrap();
    }
}

fn consume_fuel(tank: &Arc<Mutex<Tank>>, amt: usize, piston: i32) {
    let mut tank = tank.lock().unwrap();
    tank.fuel -= amt;
    println!("Consumed by Piston: {}, Fuel left: {}", piston, tank.fuel);
}
```

## Cases

comparing stdin to a string
```rust
#[test]
fn compare_two_strings() {
	let mut input = String::new();
	std::io::stdin()::read_line(&mut input).unwrap();
	assert!(input.trim().eq("string_to_be_compared")) // trim trailing newline
}
```

