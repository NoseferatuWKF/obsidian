every function with a ! is a macro
```rust
println!("Hello");
```

by default every variable is immutable
by default rust compiler will throw an error for unused variables, use underscore to bypass this
```rust
let some_var = "this will throw an error if unused";
let _another_var = "this will not throw an error even when unused";
```

`usize` is the largest unsigned int depends on the machine
```rust
let max: usize = usize::MAX; // can produce different results on different machines
```

you can put underscores for large numbers
```rust
let big_num: u32 = 1_000_000;
```

**shadowing** - variable with the same name and different data types can be set
```rust
let age: &str = 29;
let mut age: u32 = age.trim().parse().expect("Whoops");
```

rust ternary operator can we written like this
```rust
let condition: bool = true;
let ternary = if condition == true {
	true
} else {
	false
};
```

match conditional
```rust
let speed_limit: u32 = 60;
match speed_limit { // must include all possible values
	1..=59 => println!("you are within speed limit"); // = range including value
	60 => println!("max speed limit allowed");
	_ => println!("you are over the speed limit"); // _ is for the rest
}
```