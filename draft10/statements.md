# Food Specification Draft (v10) :: Statements
Statements describe a function's behaviour line by line.
## Sections
Here is a list of all of the specification-defined statements:
 - [Empty Statements](#empty-statements)
 - [Block Statements](#block-statements)
 - [Expression Statements](#expression-statements)
 - [Declaration Statements](#declaration-statements)
 - [If Statements](#if-statements)
 - [While Statements](#while-statements)
 - [Do While Statements](#do-while-statements)
 - [For Statements](#for-statements)
 - [Switch Statements](#switch-statements)
 - [Break and Continue Statements](#break-and-continue-statements)
 - [Goto Statements](#goto-statements)
 - [Assert Statements](#assert-statements)
 - [Sponge Statements](#sponge-statements)
 - [Implementation-defined Statements](#implementation-defined-statements)

## Empty Statements
*Syntax*:
```c
;
```
An empty statement is a statement consisting of only a semicolon. It does not generate any code.

## Block Statements
*Syntax*:
```c
{
	statements
}
```
A block statement is a type of statement used to group statements together. It also creates a new declaration scope. It is generally used as a body for another statement, or an imperative function declaration.

## Expression Statements
*Syntax*:
```c
expression;
```
An expression statement is a statement that takes in an expression. It is mostly used to assign or evaluate variables, but can also be used for function calls.

## Declaration Statements
*Syntax*:
```bnf
  <variable-declaration> ';'
| <local-function-declaration>
```
A declaration statement can only happen at the start of a function. Only variables and local functions can be declared at the beginning of a block statement (declaration statements cannot happen elsewhere.) Once another kind of statement is encountered, no more declarations are parsed until in another block statement.

## If Statements
*Syntax*:
```c
if (expression) then_statement                     // classic if
if (expression) then_statement else else_statement // if/else
```
An if statement is a kind of conditional statement that executes code only of an expression is met. If `expression` yields `true`, `then_statement` will be executed.

In case of an if/else statement, if `expression` yields `false`, `else_statement` will be executed.

## While Statements
*Syntax*:
```c
while (expression) then_statement
```
While `expression` yields `true`, `then_statement` will be executed over and over.

## Do While Statements
*Syntax*:
```c
do do_statement while (expression);
```
The statement `do_statement` will be executed, then `expression` will be evaluated. If `expression` yields `true`, `do_statement` will be executed again, and the `expression` will be re-evaluated. This goes on and on.

## For Statements
*Syntax*:
```c
for (init; condition; ending) then_statement
```
 - `init` can be either a declaration or a statement.
 - `condition` and `ending` must be an expression.

For loops are a special kind of loop. A new scope is created for the for loop if `init` is a declaration. Execution order:

1. `init` will be executed if not declaration.
2. `then_statement` will be executed.
3. `ending` will be executed.
4. `condition` will be evaluated, if it yields `true`, then the loop will repeat (step 2 in the list). If it yields `false`, break out.

## Switch Statements
*Syntax*:
```c
switch (expression) statement
```
A switch allows "cases" to be declared in its body. Cases are special labels with constant values that point to statements. If `expression` evaluates to a specific case value, the control is moved to that statement. Many cases may be defined in a switch, however two cases cannot have the same constant value. If `expression` does not evaluate to any case's constant value, two things can happen. If a `default` label is declared, then the control will be moved to that default label. If not declared, then all of the switch's statements will not be executed.

Syntax for classic case:
```c
case constant_expression:
```
Syntax for default case:
```c
default:
```

## Break and Continue Statements
### 1. Break Statement
*Syntax*:
```c
break;
```
The break statement is used to break out of a loop (`while`, `do...while`, `for`) or a switch (prevents falling into other cases.)
### 2. Continue Statement
*Syntax*:
```c
continue;
```
The continue statement is used to cause a loop (`while`, `do...while`, `for`) to loop over, meaning that it will directly start evaluated its condition and execute again if the expression yields `true`.

Note for `for` loops: The `ending` expression will also be executed.

## Goto Statement
*Syntax*:
```c
goto label;
```
Goto will move the control to the label specified.

### Computed Goto
*Syntax*:
```c
goto *address;
```
 - `address` must be an *any pointer*, (`void *`).
 - `address` must point to valid code.

Computed goto works the same as the classic goto, except instead of giving it a label, you give it a pointer to code. It will move the control to the code at `address`.

## Assert Statements
### Runtime Assertion, or classic assertion
*Syntax*:
```java
assert(expression);
```
Assertions are used to make sure that a given `expression` evaluates to true. If the given expression evaluates to `false`, then the program must terminate and indicate an error message. The way it does so is implementation-defined.

### Static Assertion
*Syntax*:
```java
assert static(constant_expression);
```
 - `constant_expression` must be a constant expression.

If `constant_expression` evaluates to `false`, the implementation must fail and prevent code execution. Static assertion is used to ensure macros are defined correctly and such. It can be only done while parsing declarations.

## Sponge Statement
*Syntax*:
```c
sponge statement
```
The sponge statement notifies the user of its presence with a notice, indicating the line number. Sponge statements are intended to be used for debugging, where you may place temporary statements to check things. These statements could have a performance impact, which is why it is heavily recommended to remove them once you're done debugging.

## Implementation-defined Statements
Some statements might be defined by the implementation. These typically will contain keywords that start with a double underscore (e.g. `__asm`) or an underscore followed by an uppercase (e.g. `_Asm`).