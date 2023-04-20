# 🧵 Enums

1. Enums Syntax

	- Rust's enums let you define a type that represents a set of predefined values.
	- Declare an enumeration with the `enum` keyword.
	- Fields within an enum are separated by commas.
	- Like Structs, Enum definition doesn't end with a semicolon.

	```rust
	pub enum Hobbies {
	    Coding,                    <------------------ no associated data.
	    Reading { books: i32},     <------------------ contains data of an integer `books`.
	    Hiking,                    <------------------ no associated data.
	}
	```

	![var](https://pbs.twimg.com/media/FuAd7PKWAAE1xKn?format=jpg&name=small)

1. Initialize an Enum

	- Use the `::` syntax to initialize an enum.

	```rust
	pub enum Hobbies {
	    Coding,
	    Reading { books: i32},
	    Hiking,
	}

	let my_hobby = Hobbies::Reading { books: 3 };
	```

	![var](https://pbs.twimg.com/media/FuAeGR4WwAEwIAT?format=jpg&name=small)

1. Enums inside Structs

	- Add a new field to a struct that has a previously defined enum type.  

	```rust
	pub enum Hobbies {
	    Coding,
	    Reading { books: i32},
	    Hiking,
	}

	pub struct Person {
	  name: String,
	  hobbies: Hobbies,   // This field contains data of a `Hobbies` enum
	}
	```

	![var](https://pbs.twimg.com/media/FuA2fyGWIAEdvN1?format=jpg&name=small)

1. Attach Methods

	- Enums can have methods implemented on them as well.
	- Use the `impl` keyword followed by the name of the enum.
	- The `new` method is a constructor for the enum.

	```rust
	pub enum Hobbies {
	  Coding,   
	  Reading { books: i32 },
	  Hiking,
	}
	struct Person {
	  name: String,
	  hobbies: Hobbies,
	}
	impl Person {
	  fn new(name: &str, hobbies: Hobbies) -> Self {
	    Self {
	      name: name.to_lowercase(),
	      hobbies,
	    }
	  }
	}
	```

	![var](https://pbs.twimg.com/media/FuAh9oSWwAAFyvm?format=jpg&name=small)


1. Option Enum

	- `Option<T>` type is used where a value could be something or nothing.
	- It has two variants:
	  - `None`, which carries no value.
	  - `Some(v)`, which has the value v of type T.

	```rust
	enum Option<T> {
	  None,
	  Some(T)
	}
	let my_num: Option<i32> = Some(42);
	let no_num: Option<i32> = None;
	```

	![var](https://pbs.twimg.com/media/FuAiA5-XoAEo3Vp?format=jpg&name=small)

1. Pattern Matching

	- Pattern matching can be used to process enums and their associated data.
	- It can also be used to destructure an enum and extract its data.

	```rust
	pub enum Operation {
	    Add(i32, i32),
	    Subtract(i32, i32),
	}

	let my_op = Operation::Add(3, 4);
	let result = match my_op {
	    Operation::Add(a, b) => a + b,
	    Operation::Subtract(a, b) => a - b,
	};
	assert_eq!(result, 7);
	```

	![var](https://pbs.twimg.com/media/FuAiQHjWIAEYunu?format=jpg&name=small)


You can refer to [this twitter thread](https://twitter.com/wiseaidev/status/1647881627035377664) for more info.