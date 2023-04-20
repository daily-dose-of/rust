# 🧵 Struct

1. Struct Syntax.

	- Rust's `struct` keyword allows us to define custom data types with named fields and associated data types.

	- Use it to organize your data and write more efficient code.

	```rust
	struct Employee {
	  name: String,
	  experience: u8,
	}
	```

	![var](https://pbs.twimg.com/media/Ft5xMTDXgAA0huj?format=jpg&name=small)


1. Structs Types.

	- Unit structs - field-less, helpful for generics.

	- Tuple structs - useful for grouping values of different types.

	- Regular struct.

	```rust
	struct Unit;

	struct Color( u8, u8, u8);

	struct Employee {
	  name: String,
	  experience: u8,
	}
	```

	![var](https://pbs.twimg.com/media/Ft5xcIhWAAAIM1m?format=jpg&name=small)


1. Structs Instantiation.

	- Verbose approach.
	- Fields init shorthand - if function parameter names match the struct field names.

	```rust
	struct Employee {
	  name: String,
	  experience: u8,
	}

	fn init_employee(name: String) -> Employee {
	  Employee {
	    name: name,
	    experience: 2,
	  }
	}

	let employee = init_employee("Mahmoud".to_string());

	fn init_employee(name: String) —> Employee {
	  Employee {
	    nane,
	    experience: 3,
	  }
	}

	let employee = init_employee("Mahmoud".to_string());
	```

	![var](https://pbs.twimg.com/media/Ft5yIaIWYAIpyl9?format=jpg&name=small)


1. Structs Shadow Copy.

    - Re-instantiate with a new one.
	- Update specific fields with new values and append `..<instance_name>`.

	```rust
	// 1. Define a new struct.
	struct Employee {
	  name: String,
	  experience: u8,
	}

	// 2. Instantiate a predefined struct.
	let employee = Employee {
	  name: "Mahmoud".to_string(), // or: name: String::from("Mahmoud")
	  experience: 3,
	}

	// 3. Update an instantiated struct.
	//  a. Re-instantiate with a new one.

	let employee Employee {
	  name: "Mahmoud".to_string(), // or: name: String::from("Mahmoud")
	  experience: 4,
	};

	// b. add the fields whose values are different, and append `..< instance_name>` at the end.
	let employee = Employee {
	  experience: 4,
	  employee
	};
	```

	![var](https://pbs.twimg.com/media/Ft5yTquXwAQJt2U?format=jpg&name=small)


1. Structs Methods Definitions.

	- Rust structs come to life with methods!
	
	- Define them under an `impl` block to access struct fields with the dot operator using `self`.

	```rust
	struct Employee {
	  name: String,
	  experience: u8,
	}
	// Use the `impl` keyword to implement different methods for the previous struct.

	impl Employee{
	// Employee fields can be accessed through self using dot operator
	  fn get_name(&self) -> &str {
	    &self.name
	  }
	  fn print_employee(&self) {
	    println("Employee: name = {}, experience = {}", self.name, self.experience);
	  }
	}
	fn main() {
	  let employee = Employee {
	    name: "Mahmoud".to_string(),
	    experience: 3,
	  };
	  
	  employee.print_employee(); // Employee: name = Mahmoud, experience: 3
	  println("{}", employee.get_name()); // Mahmoud
	}
	```

	![var](https://pbs.twimg.com/media/Ft5yiOhWYAA9IXI?format=jpg&name=small)


1. Associated functions.

	- They don't require `self` as a parameter and can be accessed using the `::` syntax.
	
	- Use them to perform operations on the struct type itself rather than an instance of the struct.

	```rust
	struct Employee {
	  name: String,
	  experience: u8,
	}

	impl Employee{
	  fn new(name: String) -> Employee {
	  	Employee {
	      name: name,
	      experience: 4,
	    }
	  }

	  fn print_employee(&self) {
	    println("Employee: name = {}, experience = {}", self.name, self.experience);
	  }
	}
	fn main() {
	  let employee = Employee::new("Mahmoud".to_string());
	  
	  employee.print_employee(); // Employee: name = Mahmoud, experience: 4
	}
	```
	
	![var](https://pbs.twimg.com/media/Ft5zAgQX0AMApWH?format=jpg&name=small)


You can refer to [this twitter thread](https://twitter.com/wiseaidev/status/1647881627035377664) for more info.