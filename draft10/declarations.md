# Food Specification Draft (v10) :: Declarations
A declaration declares a symbol in the current scope. Two symbols in the same *scope* cannot have the same name.
## Sections
 - [Scopes](#scopes)
 - [Variable Declarations](#variable-declarations)
 - [Function Declarations](#function-declarations)
 - [User Type Declarations](#user-type-declarations)

## Scopes
A scope refers to a context where declarations can be made. An example of a scope is a module, or a function body.

Declarations inside a scope can only be used inside the scope. Once a scope is left, all declarations are no longer reacheable. This means that **heap-allocated variables, for example, must be freed before exiting a scope to prevent memory leaks.** As for stack-allocated objects, including variable-length arrays, are already taken care of at the end of a scope. Some statements declare scopes and other declarations too.

## Variable Declarations
Variables can be declared in any scope.

You can declare variables with the following syntax:
```c
T variable_name;
```
Where:
 - `T` is the type of the variable.

You can also assign a default value to a variable.
```c
T variable_name = default_value;
```
Where:
 - `T` is the type of the variable.
 - `default_value` is an expression of type `T`.

As opposed to many C-like languages, it is forbidden to declare multiple variables in one line, separated by commas (e.g. `int a, b, c;`). The reason is that this can also make code obscure, especially when default values are involved.

## Function Declarations
Functions can be declared in any scope.

You can declare functions like so:
```c
T function_name(parameters) body
```
Where:
 - `T` is the returned type. If `void` is specified, this function returns no value.
 - `parameters` is a list of parameters separated by a comma. Parameters can have default values, but they must be located at the end of the function.
 - `body` can be two things:
   - Expression Body/Functional Declaration: `body` is an expression followed by a semicolon.
   - Imperative Declaration: `body` is a *block statement.*
Examples:
```c
int add_three_numbers(int a, int b, int c = 4): a + b + c;
add_three_numbers(1, 2); // this yields 7
add_three_numbers(1, 2, 3); // overriding default argument; yields instead 6

int main()
{
	return 0; // return statement makes the function return and yield a value.
}
```

### The `yield` symbol
For imperative-declared functions, a symbol is implicitely declared. It is named `yield`, and is the same type as the return type of the function. Here is an example using it:
```c
int triangular_numbers(int n)
{
	// please use the actual formula, this is just an example.
	for (int i = 1; i <= n; i++) {
		yield += i;
	}
	return; // An empty return statement returns the contents of yield.
}
```

## User Type Declarations
See [Types](types.md).