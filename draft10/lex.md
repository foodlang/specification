# Food Specification Draft (v10) :: Lexical Elements
This file will describe the different lexical tokens used by Food.

## Sections
 - [Comments](#comments)
 - [Literals or Primary Values](#literals)
 - [Identifiers](#identifiers)
 - [Keywords](#keywords)
 - [Operators](#operators)

## Comments
A comment is a section, or line of source code that is ignored by the compiler.
It is intended for purposes such as documenting symbols, disabling code temporarly or describing code. Any characters may be inserted until the comment terminator. If a comment is found inside a string or character literal, it is ignored.   
There are two types of comments in the Food programming language, namely:
 - C-style comments
 - C++-style comments

### C-style comments
C-style comments are surrounded by `/*` and `*/`. Syntax:  
```
ccomment := /* text */
```

### C++-style comments
C++-style comments are one-line comments that start with `//`, and end when a line break (\n) is encountered.  Syntax:
```
cppcomment := // text
```

## Literals
A literal value, or primary value, is a value that is *hardcoded* into source code. There are multiple types of literals:
 - Integer Literals
 - Floating-point Literals
 - Boolean Literals
 - Character Literals
 - String Literals

### Integer Literals
An Integer literal represents an integer. They can be written in 4 different bases, Decimal, [Binary](https://en.wikipedia.org/wiki/Binary_number), [Octal](https://en.wikipedia.org/wiki/Octal) and [Hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal). Syntax Examples:
```c
int a = 15;      // Decimal (AKA base 10). Digits range from 0 through 10
int b = 0b1111;  // Binary (AKA base 2). Either 0s or 1s, has a 0b prefix.
int c = 017;     // Octal (AKA base 8). Digits range from 0 through 7, has a 0 prefix.
int d = 0xF;     // Hexadecimal (AKA base 16). Digits range from 0 to 9, (A through F for 10-15, case does not matter.)
```
As opposed to other popular languages, food lacks any size suffixes. Instead, it is recommended to cast the integers to the appropriate size. By default, an integer literal is of type `int`/`i32`, but if the value cannot fit into a 32-bit integer, a 64-bit integer will be used (`long`/`i64`.) If a value goes beyond $2^{63}-1$, an unsigned long (`ulong`/`u64`) will be used.

### Floating-point Literals
A Floating-point literal represents a floating-point value. These conform to the standard [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754). They consist of an integer, followed by a decimal separator (`.`), then a decimal part. An exponent can be specified. Examples:
```c
float one_half = 0.5; // classic way
float exponents = 1.5e3; // exponents
float exponents_negative = 1.5e-3; // exponents can be negative.
```
The exponents use a base 10 ($n * 10^e$, $e$ is the given exponent and $n$ the value.)

### Character Literals
Character literals are used to represent characters (can represent up to 8 characters). The syntax is as follows:
```c
char c = 'A'; // Character literal consisting of the letter A
```
One important note, when using multiple characters, they are stored using this pattern: `'Abcd'` will be stored as `'d' << 24 | 'c' << 16 | 'b' << 8 | 'A'`. In other terms, the characters are stored in reverse order. They accept [Escape Sequences](#escape-sequences).

### String Literals
String literals are used to represent a string of characters. They are similar to character literals, except they don't have limits. String literals go as follow:
```c
string hello = "Hello, World!";
```
Here, the text stored will be **Hello, World!**. String literals accept [Escape Sequences](#escape-sequences).  
**Warning**: As opposed to many C-like languages, the `string` type is a *Pascal String.* C strings can be created by simply casting the string literal to a char pointer (e.g. `char *` or `const char *`). Type Qualifiers are ignored. The cast must be done on the literal value directly, and not on a variable for example (so `(char *)"Hello, World!` or `"Hello, World!" => char *` only.)

### Escape Sequences
Escape Sequences are a specific linear set of characters in a character or string literal. Here's a complete list of all of the escape sequences Food supports:  
| Code | Hex. Value | Purpose |
|:----:|-----------:|:------------------------|
| \\a  | 0x07       | Alert/Bell Character    |
| \\b  | 0x08       | Backspace Character     |
| \\e  | 0x1B       | Escape Character (usually followed by other characters) |
| \\f  | 0x0C       | Form Feed |
| \\n  | 0x0A       | Line Feed/Newline/Line Terminator |
| \\r  | 0x0D       | Carriage Return |
| \\t  | 0x09       | Horizontal Tabulation |
| \\v  | 0x0B       | Vertical Tabulation |
| \\\\ | 0x5C       | Backslash |
| \\'  | 0x27       | Apostrophe |
| \\"  | 0x22       | Quotation Mark |
| \\000 | Variable  | Inserts a specific character. Input uses the octal base. |
| \\xFF | Variable  | Inserts a specific character. Input uses the hexadecimal base. |

## Identifiers
Identifiers are used to give names to things in the program. They can be used for:
 - Variable Names
 - Structure/Record/Union Names
 - Enum Names and Enum Members
 - Structure/Record/Union Members
 - Function Names
 - Macro Names

These characters can be used to construct an identifier:
```
a b c d e f g h i j k l m n o p q r s t u v w x y z
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
0 1 2 3 4 5 6 7 8 9 _
```
Note for digits: You cannot start the identifier's name with digits, as it would be confused with an integer literal.

**Warning**: A user-declared identifier cannot start with two underscores or an underscore followed by an uppercase. These are reserved for the implementation.

Food is case sensitive, meaning that `my_variable` and `my_Variable` are totally different.

## Keywords
Keywords are very similar to identifiers, except they cannot contain digits. They are words, or sequences of words all united by an uppercase reserved by the implementation. Some additional keywords may be reserved by the implementation, but they must follow the rule stated in the [Identifiers](#identifiers) section, which is either:
 - Starts with two underscores (e.g. `__asm`)
 - Starts with an underscore followed by an uppercase (e.g. `_Packed`)

*Note that the examples given up are fictional, but may be used by an implementation for different purposes.*

Here is a list of keywords reserved by the specification:
| | | |
|:-:|:-:|:-:|
| alignof | atomic | break |
| bool | byte | case |
| char | class | const |
| continue | default | do |
| double | else | end $^±$ |
| enum | extern | false |
| extern | false | float |
| for | function | goto |
| half | if | int |
| lengthof | long | nameof |
| new | null | public |
| record | restrict | return |
| sbyte | short | size |
| sizeof | start $^±$ | static |
| string | struct | switch |
| true | uchar | union |
| uint | ulong | ushort |
| using | void | volatile |
| while | i8/I8 | u8/U8 |
| i16/I16 | u16/U16 | f16/F16 |
| i32/I32 | u32/U32 | f32/F32 |
| i64/I64 | u64/U64 | f64/F64 |
| module | interface | implements |
| assert | sponge | typeof $^±$ |
| namespace $^±$ | var $^±$ | try $^±$ |
|catch $^±$ | throw $^±$ |

$^±$: The keyword is not in use in the current version of the standard, but it reserved for later use.

## Operators
Operators, or punctuators are special symbols or series of symbols that are used to give meaning to a statement, expression, type, etc. They can separate identifiers and numbers without the need of a space.

Below is a list of all of the currently supported operators and punctuators:
| Representation | Alt. Name        |
|:--------------:|:-----------------|
| (              | Open Parenthesis |
| )              | Closed Parenthesis |
| [              | Open Square Bracket |
| ]              | Closed Square Bracket |
| {              | Open Bracket |
| }              | Closed Bracket |
| ~              | Tilde, or Wave |
| ?              | Question Mark |
| ;              | Semicolon |
| ,              | Comma |
| +              | Plus |
| ++             | Plus Plus |
| +=             | Plus Equal |
| &              | And |
| &&             | Double And |
| &=             | And Equal |
| \|             | Pipe |
| \|\|           | Double Pipe |
| \|=            | Pipe Equal |
| -              | Minus |
| --             | Minus Minus |
| -=             | Minus Equal |
| ->             | Arrow, or more precisely Thin Arrow |
| =              | Equal |
| ==             | Double Equal, or Equal Equal |
| =>             | Thick Arrow |
| !              | Exclamation Mark |
| !=             | Exclamation Equal |
| *              | Star |
| *=             | Star Equal |
| /              | Slash |
| /=             | Slash Equal |
| %              | Percentage |
| %=             | Percentage Equal |
| ^              | Caret, or Circumflex |
| ^=             | Caret Equal, or Circumflex Equal |
| :              | Colon |
| ::             | Cube, or Double Colon |
| .              | Dot |
| ..             | Dot Dot |
| ...            | Dot Dot Dot |
| <              | Lower |
| <=             | Lower Equal |
| <<             | Double Lower |
| <<=            | Double Lower Equal |
| >              | Greater |
| >=             | Greater Equal |
| >>             | Double Greater |
| >>=            | Double Greater Equal |

*Note: Their purpose is context sensitive and not defined here.*