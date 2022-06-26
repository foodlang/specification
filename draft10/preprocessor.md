# Food Specification Draft (v10) :: Preprocessor
Any Food implementation should support a preprocessor. However, as opposed to many C-like languages, it discourages the excessive use of the preprocessor. The preprocessor should be used when **needed**.

The preprocessor is a source-manipulating program that inserts and modifies bits of code before giving it to the compiler. It is important to know that macros are not namespace-safe, meaning that declaring a macro in a file exposes it to all code below.

Food also lacks any concept of *headers*. Declarations are done with the definitions in the source code. The lack of headers is done in an attempt to simplify the effort required and prevent the few pitfalls they allow.
## Sections
 - [Defining Macros](#defining-macros)
 - [Conditions](#conditions)
 - [#error and #warning](#error-and-warning)
 - [Specification Macros](#specification-macros)

## Defining Macros
Macros are the most powerful part of the preprocessor. A macro is a block of code that can be pasted in the source code. Note that macros that start with a leading underscore are specification-reserved, and double leading underscore is reserved for the implementation.

### 1. Definition
You can define a macro with this syntax:
```c
#define MACRO_NAME code
```
Every time `MACRO_NAME` will be encountered in the code as an indentifier (not part of a string, comment or other), the code `code` will be pasted. The code cannot be comments, as they are ignored by the preprocessor.

### 2. Parameterized Macros
Macros can have parameters, like many C-like languages. They are separated by commas. Example:
```c
#define PASTE(x) $$x
```
There are multiple ways a macro parameter can be pasted. The first and most simple way is the **literal pasting.** It is denoted by the absence of prefix before the parameter name. It consists of simply pasting the parameter code in. Example:
```cpp
#define LITERAL_PASTE(x) $$x

public int main()
{
	LITERAL_PASTE(return 0;);
}
```
The second way is called **string pasting.** This pastes the parameter as a string literal. It is denoted by the prefix `$`.
```cpp
#define SAY(x) say($x);
```
The third way is intended for arithmetic and logic operations. It is called **parenthesized pasting.** This pastes the parameter in brackets, so that the precedence of the parameter is kept whatever the context may be. The prefix is `$$`.
```cpp
#define ADD(x, y) ($$x + $$y) // same as (x) + (y).
```
The fourth way is called **ignoring.** Yes, it pastes no value. That's the point, its in case you need to access a variable or something else without interferring with the paremeter. Use the prefix `##` to prevent this.

### **3. An Important Warning**
Because macros are simply bits of code being pasted in, they do not take in account the context of where they are pasted. Imagine the following macro:
```c
#define ADD(x, y) x + y
```
If you read about **parenthesized pasting**, you might notice that the brackets are missing and it uses regular pasting. Lets take a look at it anyways.
```c
#define ADD(x, y) x + y

int my_var = ADD(2, 1) * 3;
```
`my_var` should be equal to 9, right?

Well, you might notice this yields 5 instead. This is because the pasted code was:
```c
int my_var = 2 + 1 * 3;
```
This is why putting arithmetic/logic macros inside brackets, and also doing the same for its parameters.

### 4. Undefinition
You can undefine a macro by simply doing the following:
```c
#undef MACRO_NAME
```


## Conditions
The preprocessor supports a way of doing basic compile-time checks. It is important to note that it can only check for macro definition and equality.

There are multiple ways to do checks using preprocessor directives:
 - `#ifeq` : Pasted if macro is equal to
 - `#ifdef` : Pasted if defined
 - `#ifneq` : Pasted if macro is not equal to
 - `#ifndef` : Pasted if not defined
 - `#elifdef` : Else if not defined
 - `#elifeq` : Else if equal to
 - `#elifndef` : Else if not defined
 - `#elifneq` : Else if not equal to
 - `#else` : Else

Every condition must end with `#endif` to tell the preprocessor that the conditional code ends here.

Here is an example:
```c
#ifdef __FOOD_V10
// the code here will be pasted if the source is compiled with the version 1.0 of Food.
#else
// the code here will be pasted if the source code is not compiled with  the version 1.0 Food.
#endif // terminating the condition

#ifeq _FOOD_VER 10
// if the Food version is 10
#endif
```

## Error and Warning
The `#error` and `#warning` directives are used to cause user-defined error and warning messages. `#error` prevents compilation. Usage:
```c
#error Enter error message here!
#warning Enter warning message here!
```

## Specification Macros
This section explains the different macros that are defined by default by the specification. The compiler should define them by default.
```c
// This macro is only for the version 10 (1.0) of Food.
#define _FOOD_V10

// This macro is set to the current version of Food.
#define _FOOD_VER 10

// Should be a C-string that is equal to the name of the implementation.
#define _IMPL /* implementation defined */

// A C-string that is equal to the current date when compiling. mmm/dd/yyyy
#define _DATE /* implementation defined */

// The current time when compiling. h:m:s, C-string
#define _TIME /* implementation defined */

// The name of the source file (C-string)
#define _SRCNAME /* implementation defined */

// The name of the currently compiling project. (C-string)
#define _PROJNAME /* implementation defined */

// The current target architecture as an integer.
#define _ARCH /* implementation defined */

// The current target operating system, as an integer.
#define _PLATFORM /* implementation defined */

// The alignment of padding of variables.
#define _ALIGN /* implementation defined */

#define _I8_MAX 127 // Maximum i8 value
#define _U8_MAX 255 // Maximum u8 value
#define _I16_MAX 32767 // Maximum i16 value
#define _U16_MAX 65536 // Maximum u16 value
#define _I32_MAX 2147483647 // Maximum i32 value
#define _U32_MAX 4294967295 // Maximum u32 value
#define _I64_MAX 9223372036854775807 // Maximum i64 value
#define _U64_MAX 18446744073709551615 // Maximum u64 value
#define _SIZE_MAX _U64_MAX // Maximum size
#define _PTR_MAX /* implementation defined */ // Maximum pointer value

#define _I8_MIN -128 // Minimum i8 value
#define _U8_MIN 0 // Minimum u8 value
#define _I16_MIN -32768 // Maximum i16 value
#define _U16_MIN 0 // Maximum u16 value
#define _I32_MIN -2147483648 // Maximum i32 value
#define _U32_MIN 0 // Maximum u32 value
#define _I64_MIN -9223372036854775808 // Maximum i64 value
#define _U64_MIN 0 // Maximum u64 value

// Size of a pointer
#define _PTR_SIZE /* implementation defined */
```
