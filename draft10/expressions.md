# Food Specification Draft (v10) :: Expressions
Expressions are sequences of operations that are used to compute values, assign values or *side effects*.
## Sections
 - [Rules of Implicit Casting](#rules-of-implicit-casting)
 - [lvalues and rvalues](#lvalues-and-rvalues)
 - [Primary Expressions](#primary-expressions)
 - [Scope Resolution Operator (::, AKA the cube operator)](#scope-resolution-operator)
 - [Postfix Expressions](#postfix-expressions)
 - [Prefix Unary Expressions](#prefix-unary-expressions)
 - [Binary Expressions](#binary-expressions)
 - [Conditional Expression](#conditional-expression)
 - [Assignment Expressions](#assignment-expressions)
 - [Compound Expression](#compound-expression)

## Rules of Implicit Casting
Implicit casting is when a value is casted from one type to another implicitely. For example:
```c
float my_float = 3;
```
Here, 3, an integer stored in a regular `int`, is implicitely casted into a float.

Typically, the data will be casted to an expected type if a type is expected, like with the float example. However, it isn't always the case:
```c
1.5 + (2 * 3)
```
In this case, an important rule come into play:
 - Cast into the type that can store the most data.

Here, it is more than just the size of the type. For example, floats are defined to be capable of holding more data, as they have an exponent form and can hold a decimal fraction. **Non-scalar non-arithmetic** types **cannot** be subject to implicit casting.

Here is in order, from least capacity to greatest capacity, the applicable types:
 - bool
 - sbyte
 - byte
 - short
 - ushort
 - half
 - int
 - uint
 - float
 - long
 - ulong
 - double

*Note: Size is equal to the biggest unsigned integer.*

## lvalues and rvalues
An **lvalue** is a kind of value that can be modified and referenced.

An **rvalue** is a kind of value that cannot be modified nor referenced.

## Primary Expressions
The most basic and fundamental type of expressions are primary expressions. They are:
 - [Literal Expressions](lex.md#literals)
 - [Identifiers (used to refer to symbols)](lex.md#identifiers)
 - `sizeof(type)` => `size`
 - `alignof(type)` => `size`
 - `nameof(type)` => `string&`
 - `default(T)` => `T`
 - Parenthesized Expressions `(expr)`
 - `new T(params)` => `T`
 - `[T] { elems }` => `T[]`

### Sizeof
The `sizeof()` operator takes in a type, and returns the size of the given type.

### Alignof
The `alignof()` operator returns the alignment of a type.

### Nameof
The `nameof()` operator returns a reference to a string that represents the name of the symbol.

### Default
The `default()` operator yields the default value of a type. For enums, it is be 0, for pointers `null`, and for structures and records, all members will be set to the default value of their type.

### Parenthesized Expressions
Expressions can be put in brackets to increase their precedence. Here is an example:
```cpp
int a123 = 1 + 2 * 3; // a123 = 7
int b123 = (1 + 2) * 3; // b123 = 9
```

### New
The `new` operator is very different from the C++ operator, which allocates an object on the heap. In Food, the `new` operator calls a *constructor* of a user-defined type. *Constructors* cannot be overloaded, thus only set the values inside the structure. `new` can be used for:
 - Primitive integer and floating types, with the syntax `new T(value)`
 - Strings, `new string(string_length)`. `string_length` is of type `uint`.
 - User-defined structures and records.

For structures and records, you must specify the members in order. By default, all of the members are equal to the default value of their type. Example:
```cs
record ivec3
{
  int x;
  int y;
  int z;
}

ivec3 vec1 = new ivec3(1);       // vec1 = { 1, 0, 0 }
ivec3 vec2 = new ivec3(1, 2);    // vec2 = { 1, 2, 0 }
ivec3 vec3 = new ivec3(1, 2, 3); // vec3 = { 1, 2, 3 }
```

### Array Literals `[T] { elemes }`
An array literal represents an array. Example:
```cs
int[] my_array = [int] { 1, 2, 3, 4, 5, 6 };
// my_array is now an array of 6 elements: 1, 2, 3, 4, 5 and 6
```

## Sope Resolution Operator
The Scope resolution operator `::` is a special operator that is used to resolve symbols inside projects and modules. Its syntax is:
```cpp
namespace::symbol
```
It can also be nested, in the case of accessing symbols inside a module, for example.
```cpp
project::module::symbol
```

## Postfix Expressions
 - Postfix Increment: `v++`
 - Postfix Decrement: `v--`
 - Function Call: `v(args)`
 - Array Subscript: `v[index]`
 - Member Access: `v.member`
 - Pointer/Reference Member Access: `v->member`

### Postfix Increment & Decrement
The postfix increment `v++` and postfix decrement `v--` operators are used to increment or decrement respectively. In:
```c
T y = x++; // alternatively x--
```
 - `x` is guaranteed to be an *lvalue*.
 - `y` will be equal to `x`.
 - `x` will be incremented/decremented by 1, although the expression will yield its previous value.
 - `T` is the type of `x`.
- `T` must be arithmetic-capable.
 - `x++`/`x--` is an *rvalue*.

### Function Call
The function call expression `v(args)` calls a function with given arguments. In:
```c
T y = x(z);
```
 - `x` must be a declared function, or a function pointer.
 - `T` is the return type of `x`.
 - `y` will be equal to the `yield` of function `x`.
 - `z` is the arguments and can be either:
   - Zero arguments
   - One argument
   - Many arguments
 - Arguments will be evaluated as they come.
 - `x(z)` is an *rvalue*.

### Array Subscript
You can read an array's member using array subscripting. In:
```c
T y = x[n];
```
 - `x` is either an array, or a pointer.
 - `T` is the base type of `x`.
 - `n` is the array index, and must be between 0 and the index of the last value in the array.
 - `n` must be an unsigned integer.
 - `y` will be equal to $x_y$, or the $y$ th element in the array.
 - By definition, `x[n]` is the same as `*(x + n)`.
 - `x[n]` is an *lvalue*.

### Member Accesss & Pointer/Reference Member Access
Member access allows you to access a member inside a user-defined structure, record, union or enum. In:
```c
T y = x.z;
```
 - `x` must have a type that is user-defined.
 - `z` is the name given to the member to access.
 - `T` is the type of `x`'s member `z`.
 - `y` will be equal to `z`.
 - `x.z` is an *lvalue*.

Pointer Access:
```c
T y = x->z;
```
 - `x` must be a pointer or reference to a user-defined type.
 - `z` is the name given to the member to access.
 - `T` is the type of `*x`'s member `z`.
 - `y` will be equal to `z`.
 - By definition, `x->z` is the same as `(*x).z`.
 - `x->z` is an *lvalue*.

## Prefix Unary Operators
These operators are *right-associative*.
 - Prefix Increment: `++v`
 - Prefix Decrement: `--v`
 - Unary Plus: `+v`
 - Unary Minus: `-v`
 - Logical Not: `!v`
 - Bitwise Not: `~v`
 - C-style Cast: `(T)v`
 - Dereference: `*v`
 - Address-of/Referencing: `&v`
 - Opaque Referencing: `&&v`

### Prefix Increment and Decrement
The prefix increment and decrement (`++x`, `--x`) are used to respectively increment and decrement an *lvalue*:
```c
T y = ++x; // or --x
```
 - `x` is an *lvalue*.
 - `T` is equal to the type of `x`.
 - `T` must be arithmetic-capable.
 - `y` will be equal to `x` incremented/decremented.
 - `x` gets incremented/decremented.
 - `++x`/`--x` is an *rvalue*.

### Unary Plus and Minus
The unary plus and minus operators (`+x` and `-x` respectively) are important operators.

Unary minus is:
```c
T y = -x;
```
 - `T` is equal to the type of `x`.
 - `T` must be arithmetic-capable.
 - `y` will be equal to the negative version of `x`. If `T` is an unsigned integer, then store the bit pattern representing the *Two's Complement* of `x`.
 - `-x` is an *rvalue*.

Unary plus is:
```c
T y = +x;
```
 - `T` is equal to the type of `x`.
 - `T` must be arithmetic-capable.
 - `y` will be equal to `x`.
 - `+x` is an *rvalue*.

### Logical Not
The logical not (`!x`) is used to flip a boolean. In:
```c
bool y = !x;
```
 - `x` must be either a boolean, an integer or a pointer.
 - `y` will be equal to the opposite of `x`. Namely, if `x == true`, then `y == false`, and the other way around.
- `!x` is an *rvalue*.

### Bitwise Not
The bitwise not (`~x`) flips all of the bits of an integer. In:
```c
T y = ~x;
```
 - `x` must be an integer.
 - `y` will be equal to the binary inverse of `x`, where all 1s become 0s and 0s become 1s.
 - `~x` is an *rvalue*.

### Dereference
Dereferencing (`*x`) allows to read a pointer's pointed value. It can also be used to read a reference's referred object. In:
```c
T y = *x;
```
 - `x` is a pointer or reference to the type `T`.
 - `y` will be equal to the value stored at the pointer/reference. If `x` is a pointer, it must point to a valid object or it will cause undefined behaviour.
 - `*x` is an *lvalue*.

### Address-Of/Referencing
This operator (`&x`) is used to get the address of an object to create a pointer, or to create a reference to an object. In:
```c
T* y = &x;
// OR
T& y = &x;
```
 - `x` is an object of type `T`.
 - `y` is either a pointer or a reference to type `T`.
 - `y` will point or refer `x`.
 - `x` must be an *lvalue*.
 - `&x` is an *rvalue*.

### Opaque Referencing
The opaque referencing operator (`&&x`) is almost identical to the address-of operator. In:
```c
void* y = &&x;
```
 - `x` must be either an *lvalue* expression, or a *label*.
 - `y` will point `x`.
 - `&&x` is an *rvalue*.

### C-style cast
A C-style cast (`(T)x`) is used to cast a value to another type. In:
```c
T y = (T)x;
```
 - `x` must be castable to `T`.
 - `x`'s type and `T` must be both scalars, both arrays or both pointers. They cannot be references or function pointers. Reference to pointer cast is possible, and function pointer to *any pointer* cast is possible. No explicit cast is needed if `x`'s type is an *any pointer*.
 - `y` will be the value translated from `x`, and of type `T`.
 - `(T)x` is an *rvalue*.

## Binary Operators
 - First Priority Level:
   - Multiplication: `a * b`
   - Division: `a / b`
   - Modulo: `a % b`
 - Second Priority Level:
   - Addition: `a + b`
   - Subtraction: `a - b`
 - Third Priority Level:
   - Bitwise Left Shift: `a << b`
   - Bitwise Right Shift: `a >> b`
 - Fourth Priority Level:
   - Lower than: `a < b`
   - Lower than or equal to: `a <= b`
   - Greater than: `a > b`
   - Greater than or equal to: `a >= b`
 - Fifth Priority Level:
   - Equal to: `a == b`
   - Not equal to: `a != b`
 - Sixth Priority Level:
   - Bitwise and: `a & b`
 - Seventh Priority Level:
   - Bitwise xor: `a ^ b`
 - Eighth Priority Level:
   - Bitwise or: `a | b`
 - Ninth Priority Level:
   - Logical and: `a && b`
 - Tenth Priority Level:
   - Logical or: `a || b`

### Multiplication, division and modulo
In:
```c
T y = x * z; // or x / z, or x % z.
```
 - `x` and `z` must be arithmetic capable.
 - `x` and `z` must have compatible types.
 - `T` must be compatible with both `x`'s and `z`'s type.
 - If a division or modulo is performed, `z` must not be zero.
 - `y` will be equal to the product (if multiplying), quotient (if dividing) or remainder of the division (if modulo).
 - `x * z`, `x / z` and `x % z` are *rvalues*.
 - Note for modulo: `x` and `z` must both be integers.

### Addition and subtraction
In:
```c
T y = x + z; // or x - z.
```
 - `x` and `z` must be arithmetic capable.
 - `x` and `z` must have compatible types.
 - `T` must be compatible with both `x`'s and `z`'s type.
 - `y` will be equal to the sum of `x` and `z` (if adding) or their difference (if subtracting).
 - `x + z` and `x - z` are *rvalues*.

### Shifting
In:
```c
T y = x << z; // or x >> z.
```
 - `x` and `z` must be integers.
 - `z` must be greater than zero.
 - Only the last byte of `z` will be used for the shifting.
 - `T` must be compatible with `x`'s type.
 - `y` will equal to `x` shifted (left or right, depends on operator used) `z` bits.
 - `x << z` and `x >> z` are *rvalues*.

### Lower/Greater Comparisons
In:
```c
bool y = x < z; // or x <= z, x > z, x >= z
```
 - `x` and `z` must be arithmetic capable.
 - `x` and `z` must have compatible types.
 - If the operator is the lower operator `<`, y will be `true` if `x` is lower than `z`.
 - If the operator is the lower or equal operator `<=`, y will be `true` if `x` is lower than or equal to `z`.
 - If the operator is the greater operator `>`, y will be `true` if `x` is greater than `z`.
 - If the operator is the greater or equal operator `>=`, y will be `true` if `x` is greater than or equal to `z`.
 - `x < z`, `x <= z`, `x > z`, `x >= z` are *rvalues*.
 
### Equality
In:
```c
bool y = x == z; // or x != z
```
 - `x` and `z` must have compatible types.
 - If `x` and `z` are equal and using double equal operator `==`, y will be `true`.
 - If `x` and `z` are not equal and using not equal operator `!=`, y will be `true`.
 - `x == z` and `x != z` are *rvalues*.

### Bitwise And
In:
```c
T y = x & z;
```
 - `x` and `z` must be integers.
 - `T` must be an integer compatible with `x` and `z`.
 - `y` will be equal to `x` AND `z`. *AND refers to the bitwise AND gate.*
 - `x & z` is an *rvalue*.

### Bitwise XOR
In:
```c
T y = x ^ z;
```
 - `x` and `z` must be integers.
 - `T` must be an integer compatible with `x` and `z`.
 - `y` will be equal to `x` XOR `z`. *XOR refers to the bitwise XOR gate.*
 - `x ^ z` is an *rvalue*.

### Bitwise Or
In:
```c
T y = x | z;
```
 - `x` and `z` must be integers.
 - `T` must be an integer compatible with `x` and `z`.
 - `y` will be equal to `x` OR `z`. *OR refers to the bitwise OR gate.*
 - `x | z` is an *rvalue*.

### Logical And
In:
```c
bool y = x && z;
```
 - `x` and `z` must be integers or booleans.
 - `y` will be equal to `true` only if `x` and `z` are `true`.
 - `x` is **guaranteed** to be evaluated first.
 - `x && z` is an *rvalue*.

### Logical Or
In:
```c
bool y = x || z;
```
 - `x` and `z` must be integers or booleans.
 - `x` is **guaranteed** to be evaluated first.
 - `y` will be equal to `true` if either `x` or `z` is `true`.
 - `x || z` is an *rvalue*.

## Conditional Expression
This operator is *right-associative*.

The conditional expression `a?b:c` is a particular expression.  
It yields `b` if `a` yields `true`, otherwise it yields `c`.

In:
```c
T y = w ? x : z;
```
 - `x` and `z` must be compatible types.
 - `w` must be of type integer or boolean.
 - `T` must be compatible with `x` and `z`.
 - `y` will be equal to `x` if `w` yields `true`.  
   Otherwise, it is equal to `z`.
 - `w` is **guaranteed** to be evaluated first.
 - `w ? x : z` is an *rvalue*.

## Assignment Expressions
The assignment expressions are a special type of expression that assigns its right operand to its left operand.

The left operand must be an *lvalue*, and the right operand has to be a *rvalue*.

As opposed to many C-like languages, you cannot have more than one assignment, unless they are separated by a compound expression. This means that this:
```c
int a = b = c;
(a += b) = c; // also forbidden
a = b, c += 3; // allowed
(a *= b) + 2; // allowed
```
Which would traditionally set `b` to `c`, then `a` to `b`, is forbidden. The reasoning behind this decision is that this makes the program obscure and hard to understand. It is also true that it makes the language easier to parse, hence making implementations of the language simpler.

In:
```c
y = x; // or
y += x; // or
y -= x; // or
y *= x; // or
y /= x; // or
y %= x; // or
y &= x; // or
y ^= x; // or
y |= x; // or
y <<= x; // or
y >>= x;
```
 - `y` must be an *lvalue*.
 - `y` and `x` must be of compatible types.
   - For `+=`, `-=`, `*=` and `/=`, their types must be arithmetic-capable.
   - For `%=`, `&=`, `^=`, `|=`, `<<=` and `>>=`, their types must be integers.
 - `y` will be:
   - If `+=`, incremented by `x`
   - If `-=`, decremented by `x`
   - If `*=`, multiplied by `x`
   - If `/=`, divided by `x`. `x` must not be zero.
   - If `%=`, will be set to `y % x`. `x` must not be zero.
   - If `&=`, will be set to `y & x`.
   - If `^=`, will be set to `y ^ x`.
   - If `|=`, will be set to `y | x`.
   - If `<<=`, will be set to `y <<= x`.
   - If `>>=`, will be set to `y >>= x`.
 - Assignments are *lvalues*.

## Compound Expressions
Compound expressions are expressions grouped together separated by commas (`,`). The yielded value of a compound expression is its last value. Each element is evaluated one after another.

In:
```c
a, b
```
 - `b` will be the yielded value.
 - Compound Expressions are *rvalues*.