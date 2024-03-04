## Getting started with Zig (Quickly)

### Installation and hello world

Getting started on Linux (I am on Fedora)

`dnf install zig`

Once Zig is up, verify using
`zig version`

Create a new project (as a lib)
`zig init-lib` 

NOTE: All the source code will go in `src`

Hello world in Zig

```zig
const std = @import("std");

pub fn main() void {
std.debug.print("Hello from {s}!\n", .{"Zig's compiler"});
}
```

>Have to get back in the habit of adding a ";" at the end of every line :D

### Assignment 

Generic syntax: `(const|var) name[: type] = val`

Example: 
`const limit: i32 = 5;`
`var count: i32 = 1;`

### Arrays

Declare arrays as
`(const|var) name = [(length|_)]type{'elements'}`

`const a = [5]u8{'z','i','g';`
`const b = [_]u8{'z', 'i', 'g'};`

When we want to infer the length of the array we use `[_]`

To get the length of the array directly we doe. 
`const c = [_]u8{'a', 'b', 'c'};`
`const len = c.len;`

Another way to declare array is 
`var vector: [3]u8 = [3]u8{'z', 'i', 'g'}`

### If expressions

Standard way to use if-else
```zig
if (num % 2 == 0) {
	std.debug.print("This is even: {}\n", .{num})
} else {
	std.debug.print("This is odd: {}\n", .{num})
}
```

if-else can also be used as simply an expression (is a very "pythonic" way)

```zig
const a = true;
var x: u16 = 0;
x += if (a) 1 else 2;
```

NOTE: Unlike C if clause will not have implicit bool values coercing. 

### Loops

#### `while`

Zig supports both while and for loops (unlike Go)

While loops are pretty standard. There are three core components to a while expression: 
- condition
- block
- continue-with

```zig
while (condition) : (continue-with) {
	block;
}
```

Printing first 100 integers with even and odd labels

```zig
const limit: u8 = 100
var num_new: u8 = 1;
while (num_new <= limit) : (num_new += 1) {
	if (num_new % 2 == 0) {
		std.debug.print("This is even: {}\n", .{num_new})
	} else {
		std.debug.print("This is odd: {}\n", .{num_new})
	}
}
```

#### `for`

For loops in Zig are extremely powerful.

General structure is 

```zig
for (collection) |values, index| {
	block
}
```

So for example, finding max would look like this:
```zig
const items = [_]i32{ 23, 12, 3, 4, 3, 56, 98, 32, 12, 19 };
var max: i32 = 0;
for (items) |elements| {
	if (elements > max) {
		max = elements;
	}
}
std.debug.print("Max element is: {}\n", .{max});
```

Iterating over slice and getting the elements and index would look like this.
```zig
const items = [_]i32{ 23, 12, 3, 4, 3 }
for (items) |elements, index| {
	std.debug.print("Element: {} | Index: {}\n", .{elements, index});
}
```

Iterating over two slices, super easy!
```zig
const items_a = [_]i32{ 23, 12, 3, 4, 3 }
const items_b = [_]i32{ 12, 15, 1, 8, 19 }

for (items_a, items_b) |elements_a, elements_b| {
	std.debug.print("Elements of a: {} | Elements of b: {}\n", .{elements_a, elements_b})
}
```

#### Functions

>Functions in zig are in camelCase, unlike variables which are snake_case

Functions have the following structure
```zig
fn functionName(name: type) type {
	// Block
}
```

Example fibonacci
```zig
fn fibonacci(num: u16): u16 {
	if (n == 0 or n == 1) return n;
	return fibonacci(n - 1) + fibonacci(n - 2);
}
```

#### Errors in zig

Errors in zig are values, just like how they are in Go. There are no exceptions. 

Errors can be handled with the `try` keyword, for example

```zig
const parseu64 = @import("error_union_u64.zig").parse64;

fn doSomething(n: []u8) !void {
	const number = try parseu64(str, 10);
	// continue with logic 
}
```

In the above code in case an error occurs while parsing u64 integer, we would return the value as error in case error is returned by parseu64, otherwise we move ahead. 

A more verbose way of handling error is 
```zig
const parse64 = @import("error_union_u64.zig").parse64;

fn doSomething(n: []u8) !void {
	const number = parseu64(int, 10) catch |err| return err;
}
```

NOTE: There is more to errors in zig that can be discussed as part of union. 


