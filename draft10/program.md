# Food Specification Draft (v10) :: Program Structrure
Food does things differently than most C-like programming languages. Instead of focusing on translation units and headers, it focuses on something known as **modules**. Modules are the most important part of any Food program.

## Sections
 - [Project (Collection of modules forming a library or program.)](#project)
 - [Module & Module Interfaces](#module)
 - [Labels](#labels)
 - [External Linking](#external-linking)


## Project
Projects are a group of modules, or module interfaces that forms a library or a program. They are built by linking together many modules. It also shares a common namespace. A good example is the library `food`, which is the standard Food library. It is a project composed of many modules that all share the `food` base namespace.

For linking projects and other binaries (such as DLLs, or static libraries), see [External Linking](#external-linking). To see how to build projects, refer to your compiler.

### Using a project
In code, for every project you're using, you must add a `using` directive. Example:
```cpp
using food;
```
This directive tells the compiler you're using the project `food`.

## Module
Modules are a group of functions and variables that share a common namespace. Modules can also implemeent interfaces, which declare what the module should contain. Module interfaces are most often used to describe modules that can be switched.

Modules have to be written in one file. Syntax:
```cpp
module module_name
{
	// Insert declarations like functions and variables here.
}
```

Module interfaces have also to be written in one file. Syntax:
```cs
interface module_name
{
	// Common declarations are already pre-defined and don't require
	// modules to implement them.
	int my_integer = 0;
	void common_func() {}

	/*
		Here, modules that implement this interface will have to
		define their own version of void a(). More on that later.
	*/
	interface void a();
}
```

### Implementing an interface
Interfaces are powerful tools. Here's how you can use them with your modules (syntax example):
```cpp
// Here is the interface we want to implement.
interface arithmetic_functions
{
	// These functions below are interface functions
	// and will require the module to define them.
	interface public int add(int a, int b);
	interface public int sub(int a, int b);
	interface public int mul(int a, int b);
	interface public int div(int a, int b);
}

module my_arithm : arithmetic_functions
{
	// To implement a function, start the definition with
	// the keyword implements, followed by the signature and
	// then the function body. Here, functional or simple
	// function bodies are used.
	implements public int add(int a, int b): a + b;
	implements public int sub(int a, int b): a - b;
	implements public int mul(int a, int b): a * b;
	implements public int div(int a, int b): a / b;
}
```

## Labels
Labels are pointers to statements that are used by the `goto` statements to move the control of execution. The syntax for labels is:
```c
label_name:
```
 - Labels must always be declared in a context where there can be statements (functions, block statements, etc.)
 - After a label, there must always be a statement.
 - Labels, much like variables, are only exposed to the current scope and its subscopes.
 - Labels can be declared at any point in code, but may be referrenced by code before and after their declaration.

## External Linking
Sometimes, you will need to use an external library written in another language than Food. In this case, you will have to use externa linking. This can be done using the `extern` function. Here is the syntax:
```cpp
extern decl;
```
`decl` refers to any kind of declaration. Note that you cannot assign any value to an external variable, nor can you assign any body to an external function. The name of the function will be the name searched. Note that specifying the `public` member will expose it.
