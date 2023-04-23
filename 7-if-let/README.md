# 🧵 If Let

1. Conditional Assignment

    - `if let` is a shorthand syntax for matching and binding on a single pattern.
    - It can be used to conditionally assign a value to a variable.
    - It is useful when you only care about one pattern and want to avoid writing a full match expression.

	```rust
	let my_option = Some(5);

	// check if `my_option` is `Some(x)`
	// if it is, assign the value of `my_option` tp `x`.
	if let Some(x) = my_option {
	    println!("The value of x is {}", x);
	} else {
	    println!("my_option is None");
	}

	// Output

	// The value of x is 5
	```

	![var](https://pbs.twimg.com/media/Fua4x0cWcAoQ64D?format=jpg&name=small)

1. Matching Multiple Conditions

    - `if let` can also be used in combination with `else if` statements to match multiple conditions.
    - This can help to simplify code and avoid nested match expressions.

	```rust
	let my_number = 7;

	// check if my_number falls within a certain range using `if let` and `else if` statements.
	if let 0..=5 = my_number {
	    println!("The number is between 0 and 5");
	} else if let 6..=10 = my_number {
	    println!("The number is between 6 and 10");
	} else {
	    println!("The number is greater than 10");
	}

	// Output

	// The number is between 6 and 10
	```

	![var](https://pbs.twimg.com/media/Fua46hDXgAA0WUI?format=jpg&name=small)

1. Struct Matching

    - `if let` can also be used to match on structs and extract their fields.
    - This can be useful when you only care about one or two fields in a struct

	```rust
	struct Person {
	    name: String,
	    experience: u32,
	}

	let person = Person { name: String::from("Mahmoud"), experience: 3 };

	// match on a Person struct and extract the name field. 
	if let Person { name, experience: 3 } = person {
	    println!("The person's name is {}", name);
	} else {
	    println!("The person's experience is not 3");
	}

	// Output

	// The person's name is Mahmoud
	```

	![var](https://pbs.twimg.com/media/Fua5aT1WwAIcnNf?format=jpg&name=small)

1. Enums Matching

    - if let can also be used to match on enums and extract their variants.
    - This can be useful when you only care about one or two variants of an enum.

	```rust
	enum Color {
	    Red(u8),
	    Green(u8),
	    Blue(u8),
	}

	let my_color = Color::Red(255);

	// match on an enum variant and extract the value of the Red variant.
	if let Color::Red(value) = my_color {
	    println!("The value of the red channel is {}", value);
	} else {
	    println!("The color is not red");
	}

	// Output

	// The value of the red channel is 255
	```

	![var](https://pbs.twimg.com/media/Fua5frXWYAgUWbg?format=jpg&name=small)

1. Using `if let` with `Result`

    - The Result type in Rust can be either an Ok variant or an Err variant.
    - We can use the if let expression to match the Ok variant and extract the value from it.
    - The if let expression can also handle the Err variant with an else block.

	```rust
	fn divide(x: f32, y: f32) -> Result<f32, String> {
	    if y == 0.0 {
	        Err("Cannot divide by zero".to_owned())
	    } else {
	        Ok(x / y)
	    }
	}

	let result = divide(10.0, 2.0);

	// using if let to match Ok variant
	if let Ok(value) = result {
	    println!("Result is: {}", value);
	} else {
	    println!("Error: {}", result.unwrap_err());
	}


	// Output:

	// Result is: 5
	```

	![var](https://pbs.twimg.com/media/Fua5k_uXwAAxNgy?format=jpg&name=small)

1. `if let` with `while let`

    - `if let` can be used in combination with `while let` to handle pattern matching in a loop.
    - This is useful when you want to keep looping while a certain pattern matches.

	```rust
	let mut numbers = vec![Some(1), Some(2), Some(3)];

	// checks if the popped element matches the pattern Some(Some(number)).
	// If it does, the code inside the block is executed and number is set
	// to the value inside the inner Some. If it doesn't, the loop ends.
	while let Some(Some(number)) = numbers.pop() {
	    println!("The number is {}", number);
	}

	// Output:

	// The number is 3
	// The number is 2
	// The number is 1
	```

	![var](https://pbs.twimg.com/media/Fua5oZLWIAMgQPM?format=jpg&name=small)

1. `if let` with a wildcard

    - If let can also be used with a wildcard (\_) to match any value in a particular variable.
    - This is particularly useful when we are only interested in knowing if the pattern matches or not.
    - Using a wildcard (\_) is the same as using a variable without assigning it to a value.
    - The underscore (\_) is used to discard values and it is a special variable in Rust.

	```rust
	let my_var1= Some(10);

	if let Some(_) = my_var1 {
	    println!("Pattern matched!");
	}

	let my_var2 = Option::<()>::None;

	if let Some(_) = my_var2 {
	    println!("Pattern matched!");
	} else {
	    println!("Pattern didn't match!");
	}

	// Output:

	// Pattern matched!
	// Pattern didn't match!
	```

	![var](https://pbs.twimg.com/media/Fua5wLrXsAQ3mVB?format=jpg&name=small)

1. `if let` with closures

    - You can use if let statements to simplify closures by removing unnecessary match statements.
    - If a closure only has a single pattern match, you can use if let to simplify the code and make it more readable.
    - The closure will only be executed if the pattern match is successful.

	```rust
	let my_result = Some(42);

	let my_closure = |value| {
	    println!("My value is {}", value);
	};

	match my_result {
	    Some(value) => my_closure(value),
	    None => (),
	}

	// Using if let:
	if let Some(value) = my_result {
	    my_closure(value);
	}

	// Output:

	// My value is 42
	// My value is 42
	```

	![var](https://pbs.twimg.com/media/Fua54IPWcAEr0RS?format=jpg&name=small)

1. `if let` with early `return`

    - You can use if let with an early return statement to simplify code and reduce nesting.
    - If the pattern match is successful, the return statement will be executed immediately, without any additional code in the block being executed.
    - This can be useful for cases where you want to exit a function early if a certain condition is met.

	```rust
	fn my_function(my_result: Option<i32>) -> i32 {
	    if let Some(value) = my_result {
	        return value * 2;
	    }

	    0
	}

	let result_1 = my_function(Some(42));
	let result_2 = my_function(None);

	println!("Result 1: {}", result_1);
	println!("Result 2: {}", result_2);

	// Output:

	// Result 1: 84
	// Result 2: 0
	```

	![var](https://pbs.twimg.com/media/Fua58BqXwAIgfej?format=jpg&name=small)

1. `if let` with `find`

    - Rust's standard library provides a `find` method for searching an iterator for an element that satisfies a predicate.
    - We can use `if let` with `find` to extract the found value `if it` exists.

	```rust
	let numbers = vec![1, 2, 3, 4, 5];

	// If 3 is found, the `if let` expression will extract the value
	if let Some(num) = numbers.iter().find(|&&x| x == 3) {
	    println!("Found number: {}", num);
	} else {
	    println!("Number not found.");
	}

	// Output:

	// Found number: 3
	```

	![var](https://pbs.twimg.com/media/Fua5_FaXsAEocol?format=jpg&name=small)

1. `if let` with `options` 

    - Rust's Option type is used to represent the presence or absence of a value.
    - We can use if let to extract the value from the Option if it contains a value, otherwise do nothing.
    - This can be useful when we only care about the case where the Option has a value and want to handle it specially.

	```rust
	fn get_username(id: i32) -> Option<String> {
	    if id == 13 {
	        Some("Mahmoud".to_string())
	    } else {
	        None
	    }
	}

	fn main() {
	    let id = 13;
	    // The `if let` statement is used to extract the value from the `Option<String>`
	    if let Some(username) = get_username(id) {
	        println!("Username: {}", username);
	    } else {
	        println!("User not found");
	    }
	}

	// Output:

	// Username: Mahmoud
	```

	![var](https://pbs.twimg.com/media/Fua6FasWwAMb80X?format=jpg&name=small)

You can refer to [this Twitter thread](https://twitter.com/wiseaidev/status/1650211186846183425) for reference