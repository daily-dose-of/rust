# 🧵 Traits.

1. Traits

    - Traits in Rust are like interfaces in other languages. 
    - They define a common interface for a group of types to implement.
    - Traits define a set of requirements that types must fulfill to implement the trait.
    - Types can implement multiple traits, allowing them to be treated abstractly as any of those traits.


	```rust
	trait ExampleTrait {
	  fn do_something(&self);
	}

	struct ExampleStruct {}

	impl ExampleTrait for ExampleStruct {
	  fn do_something(&self) {
	    println!("I did something!");
	  }
	}

	fn main() {
	  let ex = ExampleStruct{};
	  ex.do_something();
	}

	// Output:

	// I did something!
	```

	![var](https://pbs.twimg.com/media/FuKnbPtXgAEqxAt?format=jpg&name=small)

1. Trait & Generics

    - When a type implements a trait, it can be treated abstractly as that trait using generics or trait objects.
    - Generics allow functions to accept any type that implements a given trait, without knowing the concrete type.
    - Trait objects allow types to be treated abstractly as a trait, even when their concrete type is not known at compile time.

	```rust
	fn print<T: std::fmt::Display>(value: T) {
	    println!("{}", value);
	}

	fn main() {
	    print("Hello, world!");
	}

	// Output:

	// Hello, world!
	```

	![var](https://pbs.twimg.com/media/FuKnfXGWIAEfkhu?format=jpg&name=small)

1. Traits Association

    - Traits can have associated functions, types, and constants, which are implemented by the type that implements the trait.
    - Associated functions are functions that belong to the trait itself, rather than to a specific implementation of the trait.
    - Associated types are types that are defined within the trait, and can be used as part of the trait's definition.

	and so on.


	```rust
	trait MyTrait {
	    const MY_CONSTANT: u32;
	    type MyType;

	    fn my_function(&self) -> Self::MyType;
	}

	struct MyStruct {}

	impl MyTrait for MyStruct {
	    const MY_CONSTANT: u32 = 42;
	    type MyType = String;

	    fn my_function(&self) -> Self::MyType {
	        "Hello, world!".to_string()
	    }
	}

	fn main() {
	    let my_struct = MyStruct{};
	    println!("My constant: {}", MyStruct::MY_CONSTANT);
	    println!("My function: {}", my_struct.my_function());
	}


	// Output

	// My constant: 42
	// My function: Hello, world!
	```

	![var](https://pbs.twimg.com/media/FuKnlDSXwAEtD2N?format=jpg&name=small)

1. Traits Inside Traits

    - A trait can extend the functionality of another trait by declaring it as a supertrait.
    - A type that implements the subtrait must also implement all of its supertraits.
    - In the following example, ThreeIterator is a subtrait of `std::iter::Iterator` trait.

	```rust
	trait ThreeIterator: std::iter::Iterator {
	    fn next_three(&mut self) -> Option<[Self::Item; 3]>;
	}

	impl ThreeIterator for std::vec::IntoIter<i32> {
	    fn next_three(&mut self) -> Option<[Self::Item; 3]> {
	        let mut res = [0; 3];
	        for i in 0..3 {
	            if let Some(val) = self.next() {
	                res[i] = val;
	            } else {
	                return None;
	            }
	        }
	        Some(res)
	    }
	}
	fn main() {
	    let nums = vec![1, 2, 3, 4, 5];
	    let mut it = nums.into_iter();
	    assert_eq!(it.next_three(), Some([1, 2, 3]));
	}
	```

	![var](https://pbs.twimg.com/media/FuKntlsXgAIyAxY?format=jpg&name=small)

1. Traits as Parameters

    - Traits can be used in function parameters to specify that the argument must implement a particular trait.
    - In the following example, the `debug_iter` function accepts any `Iterator` where the item type implements `std::fmt::Debug`.

	```rust
	fn debug_iter<I: Iterator>(it: I) where I::Item: std::fmt::Debug {
	    for elem in it {
	        println!("{:#?}", elem);
	    }
	}

	fn main() {
	    let nums = vec![1, 2, 3];
	    debug_iter(nums.iter());
	}


	// Output

	// 1
	// 2
	// 3
	```

	![var](https://pbs.twimg.com/media/FuKnxY2XwAIQY-Q?format=jpg&name=small)

1. Traits As Return Types

    - Traits can also be used as the return type of a function, allowing the implementation details to be hidden from the user.
    - In the following example, the `from_zero_to` function returns an `Iterator` with items of type `u8`.

	```rust
	fn from_zero_to(v: u8) -> impl Iterator<Item = u8> {
	    (0..v).into_iter()
	}

	let nums = from_zero_to(5);
	assert_eq!(nums.collect::<Vec<u8>>(), vec![0, 1, 2, 3, 4]);
	```

	![var](https://pbs.twimg.com/media/FuKn1PlWAAMM4Zj?format=jpg&name=small)

1. Traits Objects

    - A trait object is an opaque value of another type that implements a set of traits.
    - Trait objects can be used when the concrete type implementing the trait is unknown at compile time.
    - Syntax: `dyn BaseTrait + AutoTrait1 + ... AutoTraitN`.
    - In the following example, a `Box<dyn Print>` is a trait object that can hold any value that implements the `Print` trait.

	```rust
	trait Animal {
	    fn speak(&self);
	}

	struct Dog;
	impl Animal for Dog {
	    fn speak(&self) {
	        println!("Woof!");
	    }
	}

	struct Cat;
	impl Animal for Cat {
	    fn speak(&self) {
	        println!("Meow!");
	    }
	}

	fn main() {
	    let dog = Dog;
	    let cat = Cat;

	    // create trait objects
	    let animal1: &dyn Animal = &dog;
	    let animal2: &dyn Animal = &cat;

	    animal1.speak();
	    animal2.speak();
	}

	// Output

	// Woof!
	// Meow!
	```

	![var](https://pbs.twimg.com/media/FuKn40FWIAIbwYq?format=jpg&name=small)

1. Unsafe Traits

    - Some traits may be unsafe to implement.
    - The `unsafe` keyword is used to mark a trait as unsafe.

	```rust
	unsafe trait UnsafeTrait {}

	unsafe impl UnsafeTrait for i32 {}

	fn main() {
	    let n: i32 = 42;
	    let ptr = &n as *const i32;
	    println!("{:p}", ptr);
	}

	// Output

	// 0x7ffc79cba93c
	```

	![var](https://pbs.twimg.com/media/FuKn-31WIAErmC9?format=jpg&name=small)
 
1. Traits 2015 vs 2018

    - In the 2015 edition, the parameters pattern was not needed for traits. This behavior is no longer valid in edition 2018.

	```rust
	// 2015 edition
	trait Tr {
	    fn f(i32);
	}

	// 2018 edition
	trait Tr {
	    fn f(&self, i32);
	}
	```

	![var](https://pbs.twimg.com/media/FuKoCZWXgAQnNXZ?format=jpg&name=small)

1. Inheritance with traits

    - Traits can build upon the requirements of other traits.
    - This is called "inheritance" with traits.

	```rust
	trait Animal {
	    fn speak(&self);
	}

	trait Pet: Animal {
	    fn sit(&self);
	}

	struct Dog;
	impl Animal for Dog {
	    fn speak(&self) {
	        println!("Woof!");
	    }
	}
	impl Pet for Dog {
	    fn sit(&self) {
	        println!("Sitting like a good boy!");
	    }
	}

	fn main() {
	    let dog = Dog;
	    let pet: &dyn Pet = &dog;

	    pet.speak();
	    pet.sit();
	}

	// Output:

	// Woof!
	// Sitting like a good boy!
	```

	![var](https://pbs.twimg.com/media/FuKoFdDWYAMfSAc?format=jpg&name=small)

1. Marker traits

    - Traits can serve as markers or carry other logical semantics that aren’t expressed through their items.
    - When a type implements that trait, it is promising to uphold its contract.
    - Examples of marker traits in the standard library are `Send` and `Sync`.

	```rust
	struct MyType;
	unsafe impl Send for MyType {}

	fn main() {
	    let my_type = MyType;
	    // do something with my_type
	}
	```

	![var](https://pbs.twimg.com/media/FuKoKJlXgAIjOT2?format=jpg&name=small)

You can refer to [this twitter](https://twitter.com/wiseaidev/status/1649065476721442817) thread for more info.