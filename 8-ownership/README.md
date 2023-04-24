# 🧵 Ownership

1. One Owner at a time

	- When an owner goes out of scope, Rust automatically drops the value associated with that owner.
	- This means that the memory used by that value is freed up for other parts of the program to use.
	- This behavior is known as "automatic memory management" and is a key feature of Rust's ownership system.
	- It eliminates the need for developers to manually manage memory, reducing the risk of errors and improving program stability.

	```rust
	let s1 = String::from("hello");
	let s2 = s1;

	// This following print statement result in a compile-time error since the ownership has moved to s2
	println!("{}", s1);
	```

	![var](https://pbs.twimg.com/media/Fugm0uAWYAY6srC?format=jpg&name=small)

1. An owner exists in a scope

    - When an owner goes out of scope, its associated value will be dropped.
    - This means that the memory used by that value is freed up for other parts of the program to use.
    - This behavior is known as "automatic memory management".

	```rust
	let s1 = String::from("hello");
	{
	    let s2 = s1;
	} // s2 goes out of scope, and `s1` doesn't become invalid because ownership has been transferred to `s2`
	println!("{}", s1); // This will result in a compile-time error because ownership has been transferred to `s2`
	```

	![var](https://pbs.twimg.com/media/Fugm76IWwAAJKbN?format=jpg&name=small)

1. Compiler check

    - Ownership is enforced by the compiler, which checks that all ownership rules are followed.
    - The compiler helps to ensure that all memory is correctly managed and that there are no memory-related errors in the code.

	```rust
	let mut s = String::from("hello");
	let r1 = &mut s;
	let r2 = &mut s;
	// This will result in a compile-time error since multiple mutable references are not allowed
	println!("{:?}", r1);
	```

	![var](https://pbs.twimg.com/media/FugnB7tWIAEBTvG?format=jpg&name=small)

1. Prevents dangling pointers

    - the ownership model eliminates the risk of common memory errors like null pointer exceptions & dangling pointers.
    - These errors can be difficult to debug and can cause serious problems, but the ownership model ensures that they cannot occur.

	```rust
	let s1: Option<String> = Some(String::from("hello"));
	let s2 = s1.unwrap(); // This will work as expected because `s1` is guaranteed to be not null, and ownership is moved to `s2`
	let s3 = s1.unwrap(); // This will panic because `s1` is now null and unwrapping a null value is not allowed
	```

	![var](https://pbs.twimg.com/media/FugnOFgWYAAMbpN?format=jpg&name=small)

1. Stack and Heap

    - Rust uses two different parts of memory for storing values: the stack and the heap.
    - The stack is used for storing values in a LIFO order, while the heap is used for storing values that can change in size during the program's execution.

	```rust
	let x = 5; // `x` is stored on the stack
	let mut s = String::from("hello"); // `s` is stored on the heap
	let s2 = s1.clone(); // This creates a deep copy of `s1`
	s.push_str(", world!");
	println!("{}", s); // This will output "hello, world!"
	```

	![var](https://pbs.twimg.com/media/FugnUEfWAAE-xFW?format=jpg&name=small)

1. Lifetimes

    - In the following example, the `longest` function takes 2 references to strings. The lifetime `'a` is used to ensure that the 2 references have the same lifetime, and that the returned reference is valid for as long as the inputs are valid.

	```rust
	fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
	    if x.len() > y.len() {
	        x
	    } else {
	        y
	    }
	}

	fn main() {
	    let s1 = String::from("hello");
	    let s2 = String::from("world");

	    let result = longest(&s1, &s2);

	    println!("The longest string is {}", result);
	}


	// Output:

	// The longest string is world
	```

	![var](https://pbs.twimg.com/media/Fugna7CWYAI2_Fc?format=jpg&name=small)

1. Zero-cost Abstractions

    - Move semantics allow Rust to transfer ownership of data from one owner to another without making any copies.
    - Copy-on-write (COW) allow Rust to avoid making copies of data until it is actually necessary.

	```rust
	fn main() {
	    let v1 = vec![1, 2, 3]; // v1 is the owner of a vector containing three integers
	    let v2 = v1; // v1 is moved to v2, so v1 is no longer valid

	    println!("v2 = {:?}", v1);
	}

	// Output:

	// v2 = [1, 2, 3]
	```

	![var](https://pbs.twimg.com/media/FugnhhYXgAAyFkP?format=jpg&name=small)

1. Easier Multithreading

    - Only one thread has mutable access to a piece of data at a time.
    - Ownership allows for easy transfer of data between threads using channels.
    - Borrowing rules ensure multiple threads can read from data simultsly, but only one thread can write.

	```rust
	use std::sync::mpsc::{Sender, Receiver};
	use std::sync::mpsc;
	use std::thread;

	fn main() {
	    let (tx, rx): (Sender<i32>, Receiver<i32>) = mpsc::channel();

	    // Spawn a new thread to send data to the receiver
	    let handle = thread::spawn(move || {
	        tx.send(42).unwrap();
	    });

	    // Wait for the thread to finish and receive the data
	    let result = handle.join().unwrap();
	    let received = rx.recv().unwrap();
	    println!("Received: {}", received);
	}

	// Output:

	// Received: 42
	```

	![var](https://pbs.twimg.com/media/FugnpuXXgAoIwQk?format=jpg&name=small)

1. FP Patterns

    - Ownership rules make it easy to write functional-style code by ensuring that values are not mutated after they are created.
    - This promotes safer and more predictable code, as well as making it easier to reason about.

	```rust
	fn sum(numbers: &Vec<i32>) -> i32 {
	    let mut total = 0;
	    for number in numbers.iter() {
	        total += number;
	    }
	    total
	}

	fn main() {
	    let numbers = vec![1, 2, 3, 4, 5];
	    let result = sum(&numbers);
	    println!("{}", result);
	}

	// Output:

	// 15
	```

	![var](https://pbs.twimg.com/media/FugnvOMXsAE1oIz?format=jpg&name=small)

1. Closures & Iterators

    - Ownership rules enable Rust to have powerful language features that are both flexible and safe.
    - Closures and iterators are two examples of expressive features made possible by Rust's ownership system.

	```rust
	let numbers = vec![1, 2, 3, 4, 5, 6];
	let evens = numbers.into_iter().filter(|&n| n % 2 == 0).collect::<Vec<_>>();
	println!("{:?}", evens);

	// Output

	// [2, 4, 6]
	```

	![var](https://pbs.twimg.com/media/Fugn32PXoAwsllm?format=jpg&name=small)

You can refer to [this Twitter thread](https://twitter.com/wiseaidev/status/1650613330313113604) for reference.
