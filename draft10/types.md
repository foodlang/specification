# Food Specification Draft (v10) :: Types
Types are one of the most important concepts of Food. They are used *everywhere*, from expressions to functions.
## Sections
 - [Type Qualifiers](#type-qualifiers)

Built-in types:
 - [Primitive Types](#primitive-types)
 - [Pointers](#pointers)
 - [References](#references)
 - [Function Callbacks](#function-callbacks)
 - [Arrays](#arrays)

User-defined types:

 - [Enumerated Types (Enums)](#enumerated-types)
 - [Structures and Records](#structure-and-records)
 - [Unions](#unions)
 - [Type Aliases](#type-aliases)

## Type Qualifiers
Type Qualifiers change a type's meaning. Here is a list of all type qualifiers with their purpose:
| Keyword | Purpose |
|:-------:|:--------|
| `const` | Denotes a value that does not change after its original assignment.
| `volatile` | Denotes a value that may change between its different accesses, even if not changed by the user. |
| `restrict` | Denotes a pointer that for its lifetime, its pointed value will never be pointed by any other pointer. |
| `atomic` | Denotes a value that it must be locked before writing to it, as it can be accessed by multiple threads. |

## Primitive Types
Primitive Types are the most basic types avaiable in the Food programming language. Here is a list of them:
| Size Name | Keyword    | Size (bytes) | Unsigned? | Nickname(s) | Purpose  |
|:---------:|:----------:|-------------:|:---------:|:-----------:|:---------|
| (none)    | `void`     | 0            | N/A       | (none)      | See below. |
| (none)    | `bool`     | 1            | Yes       | (none)      | Boolean. |
| `u8`/`U8` | `byte`     | 1            | Yes       | `uchar`     | Integer. |
| `i8`/`I8` | `sbyte`    | 1            | No        | `char`      | Integer. |
|`i16`/`I16`| `short`    | 2            | No        | (none)      | Integer. |
|`u16`/`U16`| `ushort`   | 2            | Yes       | (none)      | Integer. |
|`f16`/`F16`| `half`     | 2            | N/A       | (none)      | Float.   |
|`i32`/`I32`| `int`      | 4            | No        | (none)      | Integer. |
|`u32`/`U32`| `uint`     | 4            | Yes       | (none)      | Integer. |
|`f32`/`F32`| `float`    | 4            | N/A       | (none)      | Float.   |
|`i64`/`I64`| `long`     | 8            | No        | (none)      | Integer. |
|`u64`/`U64`| `ulong`    | 8            | Yes       | (none)      | Integer. |
|`f64`/`F64`| `double`   | 8            | N/A       | (none)      | Float.   |
| (none)    | `size`     | Largest Integer| Yes     | (none)      | Size.    |
| (none)    | `string`   | Minimum 4    | N/A       | (none)      | String.  |

Booleans can represent either `true` or `false`, respectively 1 and 0. On an important note, a boolean is considered `true` if it is not zero. Comparing to
`true` will actually check if the boolean is not `false` (0).

Integers can be signed or unsigned, meaning that they can either accept or not a negative value. Signed integers must use *Two's Complement.*

Floating-point values must conform to the standard [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) to be valid.

The `size` type refers to the biggest unsigned integer that the implementation can store natively, typically refers to the biggest value that can be stored in a generic register.

The `string` type is a Pascal String that can hold up to $2^{32}-1$ 8-bit characters. It is heavily recommended to pass large strings as pointers/references, as copying a string (or passing it around) will copy all of its data.

The `void` type is an incomplete type, and cannot store a value. It is used to denote functions that return nothing.

## Pointer Types
Pointers are unsigned integers that will have the same size as the type `size`. They **point to data**, meaning the value stored in a pointer is an address to data. Some important concepts must be understood about pointers:
 - A pointer that its base type is `void` is an **any pointer**, and can point to any kind of data and be casted to any other type of pointers. It cannot be dereferenced without cast.
 - if a pointer's value is zero, or `null` (`null` is a pointer constant that points to nothing), it is considered a **null pointer**, or a pointer that points to no data. Derefercing is undefined behaviour.
 - Adding a value to a pointer, or incrementing a pointer will increment the address stored by **element size times the value.** **Element size** refers to the size of a value pointed by this pointer. *Note: For any pointers, increments of one will be used, as `void` has no size, because it is an incomplete type.* Same goes for decrementing.
 - Pointers are usually considered *unsafe* because of their diverse powerful features, which means that **when possible, use a reference instead.**

The syntax of a pointer goes as follows:
```c
type*
```
*Note: The star can be anywhere. As long as there's a star, it is a pointer.*

### Computed Goto
Because *any pointers* can point to any kind of data, they can point to code. This means that computed goto is possible. Refer to the [goto statement](statements.md#goto-statement) for more information.

## Reference Types
References are a variant of pointers, but have substantial differences. As opposed to a pointer, references **cannot**:
 - A reference is always constant, once it is assigned it can never change.
 - Adding or incrementing a pointer is also forbidden.
 - You cannot cast a reference to **any** other type. Exception for casting references to a pointer of the same base type, for example `int&` to `int*`.
 - **Any references**, or **void&** does not make any sense, and so, is forbidden.
 - References cannot be assigned `null`.
 - References cannot be the base of another type.

The syntax of a reference goes as follows:
```cpp
type&
```
*Note: As with pointers, the ampersand (And symbol) does not need to be a precise location.*

As opposed to C++'s references, Food references use the same syntax as pointers (`*` for dereference, `&` to make a reference), as they are a variant of pointers.

## Function Callbacks
Function Callbacks, or function pointers, are a very special type of pointer that points to a function. They can:
 - Be reassigned at any moment, but only if the assigned function has the same signature.
 - Be casted to **any pointers** and from **any pointers**.
 - Be assigned `null`, in the case of an unset function callback. Please take note that then calling the function callback will result in undefined behaviour.

However, they cannot:
 - Be incremented, decremented, added to or subtracted from.
 - Be casted to another function callback of another signature.

The syntax is:
```c
function return_type(params)
```

You can call function callbacks with the same syntax as you would call any other function.

## Arrays
Arrays represent a set of linear objects of the base type. They can have a constant or variable length.

Typically, arrays are stored on stack but can also be stored in another memory spaces. If an array is not stored on the stack, then you should use a [Pointer](#pointer-types).

The syntax for an array goes as follows:
```c
type[array_element_count] array;
```
If an array has a variable length, `sizeof()` will return the size of a pointer. If it doesn't, then it will be $cs$, where $c$ means the number of elements it can hold (capacity), and $s$ the size of an element.

## Enumerated Types
Enumerated Types, or Enums are a kind of user-defined that consists of a single value that represents can be set to a set of named constants. Enumerated types use an underlying type, which is most often `int`. Enums can also be directly assigned an integer value. A duplicate enum constant is not allowed.

The syntax to declare an enum goes as follows:
```cs
enum fruits
{
	apple, // By default, enums start at zero. However, this can be changed.
	banana, // This will be equal to 1.
	orange = 10, // You can assign a constant value to an enum constant.
	grape, // This will be 11.
	peach, // etc.
	grapefruit, 
	lemon // Here, you can end the enum with a comma or not, it does not matter.
}
```

Then, you can use the enum has if it was a type:
```cs
fruits my_fruit_kind = fruits.apple; // my_fruit_kind = 0
my_fruit_kind = fruits.lemon; // my_fruit_kind = 14
```

## Structure and Records
A structure is a linear group of member values. Two members cannot have the same name. Structures are user-defined types.

A record is a special variant of a structure, where its members cannot be modified. If you want to change one of its members, you have to change it fully.

### Declaring a Structure
Structures are declared like so:
```cs
// date is a "structure template." It defines all of the members
// a structure of type date should have.
struct date
{
	short year; // Year is a stored value in the structure.
	byte month; // Same thing here
	byte day;   // Again
}
```

### Initialization & Member Access
To initialize structures or records, you use the `new` expression:
```cs
date my_date = new date(1970, 1, 1);

// You can read from members using the member access operator.
short my_year = my_date.year;
my_year += 3;

// You can assign members to values. Note: This is forbidden in the case
// of records.
my_date.year = my_year;
```

## Unions
An union is a group of members of different types that share a common address. A union's member cannot have the same type as another member in the union, nor the same name.

### Declaring an Union
Unions are declared like so:
```cpp
// All of its members share a common address, meaning that &as_i8 == &as_u64.
union integer
{
	i8 as_i8;
	u8 as_u8;
	i16 as_i16;
	u16 as_u16;
	i32 as_i32;
	u32 as_u32;
	i64 as_i64;
	u64 as_u64;
}
```

### Initialization & Member Acces
As opposed to structure-like objects, unions require a direct assignment:
```cs
integer my_integer = 2000; // assigning an int (i32) to the union
i8 my_i8 = my_integer.as_i8; // will be equal to 208
u64 my_u64 = my_integer.as_u64; // will be equal to 2000
```

## Type Aliases
In Food, you can make type aliases, which are other names for types. Type qualifiers, pointers, references and arrays are all allowed. The syntax for a type alias is as follows:
```cpp
class alias_name = const int *; // now alias_name is equal to const int *

const int a = 3;
const int *b = &a;
alias_name c = &a; // same type as b
```
