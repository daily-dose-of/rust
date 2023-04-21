# 🧵 Lifetimes

1. Lifetimes

    - Lifetimes are like a referee for Rust's borrowing system.
    - They ensure that all borrows are valid and prevent dangling references.
    - A lifetime is used to track the beginning and end of a variable's existence.
    - The borrow checker ensures that all borrows are valid during a variable's lifetime.

	```rust
	/// a function that takes a reference to a string and returns the first word in
	/// that string, without including the space character that follows i
	fn first_word(s: &str) -> &str {
		// The lifetime of borrow starts here.
	    let bytes = s.as_bytes();

	    for (i, &item) in bytes.iter().enumerate() {
	    	// The reference is valid in this scope.
	        if item == b' ' {
	            return &s[0..i]; 
	        }
	    }

	    &s[..]
	} // The lifetime of borrow ends here when the scope is exited.

	let s = String::from("hello world");
	// pass the reference of x to the function.
	let word = first_word(&s);
	println!("The first word is: {}", word);

	// Output

	// The first word is: hello
	```

	![var](https://pbs.twimg.com/media/FuQ2Er1X0BMUr7X?format=jpg&name=small)

1. Lifetimes != Scopes

    - Although lifetimes and scopes are often referred to together, they are not the same thing.
    - A scope is used to define where a variable is valid.
    - For example, consider this code that borrows a variable x twice in different scopes:


	```rust
	fn main() {
	    let i = 3; // The variable is created here.

	    {
	        let borrow1 = &i; // borrow1 borrows i here.
	        println!("borrow1: {}", borrow1);
	    } // i is destroyed here, but borrow1 still borrows it.

	    {
	        let borrow2 = &i; // borrow2 borrows i here.
	        println!("borrow2: {}", borrow2);
	    } // i is destroyed here, but borrow2 still borrows it.
	}
	// Although i has the same lifetime in both scopes, its scope is different,
	// since it's only valid within each scope.

	// Output:

	// borrow1: 3
	// borrow2: 3
	```

	![var](https://pbs.twimg.com/media/FuQ2bPwX0AY3wHr?format=jpg&name=small)

1. Lifetime Annotations

    - Lifetime annotations tell Rust's borrow checker how long a reference is valid.
    - Lifetime annotations are written using the `'a` syntax, where `'a` is a name for a lifetime.
    - They are used to specify the relationship between multiple references in a function or struct.

	```rust
	struct Foo<'a> {
	    x: &'a i32,
	}

	fn main() {
	    let y = 5;
	    let x = &y;
	    let f = Foo { x: x }; // The lifetime of borrow is annotated here.
	    println!("{}", f.x);
	    // y is still accessible here
	}

	// Output:

	// 5
	```

	![var](https://pbs.twimg.com/media/FuQ2PSlX0A8naqK?format=jpg&name=small)

1. Lifetime Elision Rules

    - The lifetime elision rules are a set of patterns that Rust's borrow checker can use to infer lifetimes.
    - The rules are based on common usage patterns of references in Rust programs.

	```rust
	/// a function with a single input lifetime parameter can have the 
	/// same lifetime for its output references.
	fn first_word(s: &str) -> &str {
	    let bytes = s.as_bytes();
	    for (i, &item) in bytes.iter().enumerate() {
	        if item == b' ' {
	            return &s[..i];
	        }
	    }
	    &s[..]
	}
	```

	![var](https://pbs.twimg.com/media/FuQ2SxyX0AoflwK?format=jpg&name=small)

1. Valid In a Scope

    - Lifetimes can also be used to specify that a reference is valid only for a certain scope, which helps prevent the reference from being used after the data it points to has been deallocated.

	```rust
	fn main() {
	    let y;
	    {
	        let x = 5;
	        y = &x;
	    }
	    // error: `x` does not live long enough
	    println!("{}", y);
	}
	```

	![var](https://pbs.twimg.com/media/FuQ2VeNX0A8-c7M?format=jpg&name=small)

1. Lifetimes & Structs

    - One of the most common uses of lifetimes is when working with structs that contain references. In this case, the lifetime of the struct must be tied to the lifetime of the references it contains.


	```rust
	struct Foo<'a> {
	    x: &'a i32,
	}

	fn main() {
	    let y = 5;
	    let foo = Foo { x: &y };
	    println!("{}", foo.x);
	}

	// Output:

	// 5
	```

	![var](https://pbs.twimg.com/media/FuQ2fHVX0AYYqPR?format=jpg&name=small)

1. Program Lifetime

    - `'static` is used for static variables, string literals, and other data that has a fixed lifetime that extends beyond the scope of any function or variable.
    - Lifetime annotations with `'static` can be used to create self-referential structs, where a struct contains a reference to itself.

	```rust
	struct Foo<'a> {
	    x: &'a i32,
	    y: &'a str,
	}

	fn main() {
	    let x = 5;
	    let y = "hello";
	    let f = Foo { x: &x, y: y };
	    println!("{}, {}", f.x, f.y);
	    // x and y are still accessible here
	}

	// Output

	// 5, hello
	```

	![var](https://pbs.twimg.com/media/FuQ2r4_XwAAofrv?format=jpg&name=small)

You can refer to [this Twitter thread](https://twitter.com/wiseaidev/status/1649503633636052992) for reference.