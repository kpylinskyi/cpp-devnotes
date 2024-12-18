## Statements

In C++, a statement is a complete unit of execution. There are three main types of statements:

- **Declaration Statements**: Introduce new variables or constants.
- **Expression Statements**: Perform calculations or call functions.
- **Control Statements**: Direct the flow of execution (e.g., loops, conditionals).

## Declarations

A declaration introduces a name and its type in a program, allowing it to be used later.

```cpp
int a = 10; // Declares and initializes an integer 'a' with the value 10.
double b;   // Declares a double variable 'b' without initializing it.
```

## Expressions

Expressions produce a value and can be composed of variables, literals, operators, and function calls.

```cpp
int x = 5;
int y = x + 5; // Expression that adds 5 to 'x' and assigns the result to 'y'.

std::cout << x; // Expression that outputs the value of 'x'.
```

## Operators

Operators are special symbols that perform operations on operands.

### Binary Operators

#### Arithmetic Operators
- `+` : Addition
- `-` : Subtraction
- `*` : Multiplication
- `/` : Division
- `%` : Modulus (remainder)

#### Bitwise Operators
- `<<` : Left shift
- `>>` : Right shift
- `|`  : Bitwise OR
- `&`  : Bitwise AND
- `^`  : Bitwise XOR

#### Logical Operators
- `&&` : Logical AND
- `||` : Logical OR
- `<`  : Less than
- `>`  : Greater than
- `==` : Equal to
- `!=` : Not equal to
- `<=>`: Three-way comparison (C++20)

### Lazy Evaluation

For `&&` operator:
```cpp
std::vector<int> v = {1, 2, 3};
if (v.size() >= 5 && v[4] == 1) {
    // If the first expression is false, the second expression is not evaluated.
}
```

For `||` operator:
```cpp
int x = 8;
if (x > 10 || x < 5) {
    // If the first expression is true, the second expression is not evaluated.
}
```

### Assignment Operators

- `=`   : Assign
- `+=`  : Add and assign
- `-=`  : Subtract and assign
- `*=`  : Multiply and assign
- `/=`  : Divide and assign
- `%=`  : Modulus and assign
- `<<=` : Left shift and assign
- `>>=` : Right shift and assign
- `|=`  : Bitwise OR and assign
- `^=`  : Bitwise XOR and assign
- `&=`  : Bitwise AND and assign

### lvalue and rvalue

An lvalue refers to an object that occupies identifiable memory locations, whereas an rvalue is a temporary value that does not persist beyond the expression that uses it.

```cpp
std::cout << (x = y);  // Outputs the result of 'x = y', which is an lvalue.

(x = y) = z;           // Valid: 'x = y' is an lvalue, so it can be assigned to 'z'.

(x += y) += 5;         // Valid: 'x += y' is an lvalue, so it can be further modified.
```

### Right and Left Associative Operators

- `=` is right-associative:
  ```cpp
  x = y = z; // Interpreted as x = (y = z)
  ```
- `+` is left-associative:
  ```cpp
  a + b + c; // Interpreted as (a + b) + c
  ```
- `*` is left-associative:
  ```cpp
  a * a * a * a; // Interpreted as ((a * a) * a) * a, not as (a * a) * (a * a)
  ```

### Unary Operators
- `a++` : Post-increment (rvalue)
- `++a` : Pre-increment (lvalue)
- `a--` : Post-decrement (rvalue)
- `--a` : Pre-decrement (lvalue)

```cpp
int a = 5;
++a++;  // Error: ++a is an lvalue, but a++ is an rvalue.

int b = 3;
a++ + ++b; // Valid: Increments 'a' after the operation and 'b' before the operation.

++++a; // Valid: Increment 'a' twice.
```

### Lexical Parser Example

```cpp
a++ + ++b;
| First lexeme: a++
    | Second lexeme: +
        | Third lexeme: ++b
```

### Ternary Operator

The ternary operator `?:` returns one of two values based on a condition.

```cpp
int a = 10;
int b = (x > 5) ? 10 : 2; // If 'x' is greater than 5, 'b' is 10; otherwise, 'b' is 2.

(b ? a++ : ++a) = 1; // Error: different value categories (rvalue and lvalue).

int a = 5;
std::string s = "abc";

// Compile-time error: cannot deduce common type between 'int' and 'std::string'.
auto result = (x > 5) ? a : s;

// Compile-time error: ambiguous call to overloaded function 'f'.
f(x > 5 ? a : s);
```

### Comma Operator (,)

The comma operator `,` evaluates both of its operands and returns the value of the second operand.

```cpp
int x = 0;
f(x++, x++); // Undefined behavior: order of evaluation of function arguments is unspecified.

f((x++, x++)); // Guaranteed: evaluates 'x++' twice, left to right.
```

### `sizeof` Operator

The `sizeof` operator returns the size, in bytes, of a type or object.

```cpp
int v = 5;
std::cout << sizeof(v) << std::endl; // Returns 4 (size of int).

std::cout << sizeof(char) << std::endl; // Returns 1 (size of char).

std::vector<int> vec {1, 2, 3};
std::cout << sizeof(vec) << std::endl; // Size of vector object (implementation-defined, often 24 or more bytes).

std::cout << vec.size() << std::endl; // Number of elements in the vector: 3.
```

### Operators Precedence

Operator precedence determines the order in which different operators in an expression are evaluated. Operators with higher precedence are evaluated before those with lower precedence. When operators have the same precedence, their associativity determines the order of evaluation.

Here's a table of C++ operators in order of precedence from highest to lowest:

| Precedence | Operators                                                                                          | Description       | Associativity |
| ---------- | -------------------------------------------------------------------------------------------------- | ----------------- | ------------- |
| 1          | `::`                                                                                               | Scope resolution  | Left-to-right |
| 2          | `++` `--` `()` `[]` `.` `->` `typeid` `dynamic_cast` `static_cast` `const_cast` `reinterpret_cast` | Postfix           | Left-to-right |
| 3          | `++` `--` `+` `-` `!` `~` `*` `&` `sizeof` `new` `delete` `typeid` `cast`                          | Prefix and unary  | Right-to-left |
| 4          | `.*` `->*`                                                                                         | Pointer-to-member | Left-to-right |
| 5          | `*` `/` `%`                                                                                        | Multiplicative    | Left-to-right |
| 6          | `+` `-`                                                                                            | Additive          | Left-to-right |
| 7          | `<<` `>>`                                                                                          | Shift             | Left-to-right |
| 8          | `<` `<=` `>` `>=`                                                                                  | Relational        | Left-to-right |
| 9          | `==` `!=`                                                                                          | Equality          | Left-to-right |
| 10         | `&`                                                                                                | Bitwise AND       | Left-to-right |
| 11         | `^`                                                                                                | Bitwise XOR       | Left-to-right |
| 12         | \|\|                                                                                               | Bitwise OR        | Left-to-right |
| 13         | `&&`                                                                                               | Logical AND       | Left-to-right |
| 14         | \|\|                                                                                               | Logical OR        | Left-to-right |
| 15         | `?:`                                                                                               | Conditional       | Right-to-left |
| 16         | `=` `+=` `-=` `*=` `/=` `%=` `<<=` `>>=` `&=` `^=` \|=                                             | Assignment        | Right-to-left |
| 17         | `throw`                                                                                            | Throw exception   | Right-to-left |
| 18         | `,`                                                                                                | Comma             | Left-to-right |

### Examples and Comments on Precedence

1. **Multiplicative and Additive Operators**
   ```cpp
   int a = 5 + 2 * 3; // Multiplication has higher precedence than addition.
   // Equivalent to: int a = 5 + (2 * 3); // a = 11
   ```

2. **Unary and Assignment Operators**
   ```cpp
   int x = 5;
   int y = ++x * 2; // Prefix increment has higher precedence than multiplication.
   // Equivalent to: int y = (x + 1) * 2; // y = 12, x = 6
   ```

3. **Conditional (Ternary) Operator**
   ```cpp
   int a = 10;
   int b = (a > 5) ? 100 : 200; // The conditional operator has right-to-left associativity.
   // b = 100 because the condition (a > 5) is true.
   ```

4. **Relational and Logical Operators**
   ```cpp
   bool result = (5 < 10) && (10 > 5); // Relational operators have higher precedence than logical AND.
   // Equivalent to: bool result = ((5 < 10) && (10 > 5)); // result = true
   ```

5. **Bitwise and Logical Operators**
   ```cpp
   int a = 5;   // 0101 in binary
   int b = 3;   // 0011 in binary
   int c = a & b; // Bitwise AND has higher precedence than logical OR.
   // Equivalent to: int c = (a & b); // c = 1 (0001 in binary)
   ```

6. **Comma Operator**
   ```cpp
   int x = (1, 2, 3); // The comma operator evaluates each operand and returns the last one.
   // x = 3
   ```

### Example of Combining Multiple Operators

```cpp
int a = 5;
int b = 10;
int c = 15;
int result = a + b * c > 50 ? a + b : c - b;
// Here, the precedence and associativity rules are applied as follows:
// 1. Multiplication (*) has higher precedence than addition (+), so b * c is evaluated first.
// 2. Then, addition (+) and the relational operator (>) are evaluated.
// 3. The conditional operator (?:) is applied last due to its lowest precedence (among the operators in this expression).
// Equivalent to: int result = ((a + (b * c)) > 50) ? (a + b) : (c - b);
```
### Order of Evaluation

The order of evaluation determines the sequence in which parts of an expression are evaluated. This is different from operator precedence, which determines the grouping of parts of an expression, and associativity, which determines the direction of evaluation when operators of the same precedence appear in an expression.

In C++, the order of evaluation of expressions involving multiple operators is often unspecified, meaning the compiler is free to choose any order as long as the final result adheres to the precedence and associativity rules. This can lead to undefined behavior if not handled carefully.

### Examples and Comments on Order of Evaluation

1. **Function Calls**
   ```cpp
   int f() {
       std::cout << "f() called" << std::endl;
       return 1;
   }

   int g() {
       std::cout << "g() called" << std::endl;
       return 2;
   }

   int main() {
       int x = f() + g();
       // The order of evaluation of f() and g() is unspecified.
       // Output could be:
       // f() called
       // g() called
       // or:
       // g() called
       // f() called
   }
   ```

2. **Sequence Points**
   ```cpp
   int main() {
       int a = 1;
       int b = (a++) + (a++);
       // The order of evaluation of (a++) and (a++) is unspecified, leading to undefined behavior.
       // It can result in different values for b.
   }
   ```

3. **Logical Operators (&&, ||)**
   ```cpp
   bool f() {
       std::cout << "f() called" << std::endl;
       return false;
   }

   bool g() {
       std::cout << "g() called" << std::endl;
       return true;
   }

   int main() {
       if (f() && g()) {
           std::cout << "Both are true" << std::endl;
       }
       // Since f() returns false, g() is not called due to short-circuit evaluation.
       // Output:
       // f() called
   }
   ```

4. **Comma Operator**
   ```cpp
   int main() {
       int a = (1, 2, 3);
       // The comma operator evaluates each operand from left to right, returning the rightmost operand.
       // a = 3
   }
   ```

5. **Function Argument Evaluation**
   ```cpp
   void print(int x, int y) {
       std::cout << x << ", " << y << std::endl;
   }

   int f() {
       std::cout << "f() called" << std::endl;
       return 1;
   }

   int g() {
       std::cout << "g() called" << std::endl;
       return 2;
   }

   int main() {
       print(f(), g());
       // The order of evaluation of function arguments is unspecified.
       // Output could be:
       // f() called
       // g() called
       // 1, 2
       // or:
       // g() called
       // f() called
       // 1, 2
   }
   ```

6. **Member Access and Function Calls**
   ```cpp
   struct S {
       int f() {
           std::cout << "f() called" << std::endl;
           return 1;
       }
       int g() {
           std::cout << "g() called" << std::endl;
           return 2;
       }
   };

   int main() {
       S s;
       int x = s.f() + s.g();
       // The order of evaluation of s.f() and s.g() is unspecified.
       // Output could be:
       // f() called
       // g() called
       // or:
       // g() called
       // f() called
   }
   ```

### Order of Evaluation

Order of evaluation refers to the sequence in which the operands of an expression are evaluated. In C++, the order of evaluation is often unspecified, meaning the compiler can evaluate operands in any order. This can lead to different behaviors if side effects (like function calls) are involved.

For example:

```cpp
#include <iostream>

int a() { return std::puts("a"); }
int b() { return std::puts("b"); }
int c() { return std::puts("c"); }

void z(int, int, int) {}

int main() {
    z(a(), b(), c());      // All 6 permutations of output are allowed
    return a() + b() + c(); // All 6 permutations of output are allowed
}
```

In the above code, the order of evaluation for the arguments to `z` and the addition operation is unspecified. This means the output could be any permutation of "a", "b", and "c".

#### Ensuring Defined Order of Evaluation

To ensure a defined order of evaluation, you can use explicit sequence points or separate statements.

1. **Using Temporary Variables**

    By using temporary variables, you can control the order in which expressions are evaluated.

    ```cpp
    int main() {
        int a = 1;
        int temp1 = a++;
        int temp2 = a++;
        int b = temp1 + temp2;
        std::cout << b << std::endl; // Output: 3 (b = 1 + 2)
        return 0;
    }
    ```

2. **Separate Statements**

    By splitting expressions into separate statements, you can ensure a defined order of evaluation.

    ```cpp
    int f() {
        std::cout << "f() called" << std::endl;
        return 1;
    }

    int g() {
        std::cout << "g() called" << std::endl;
        return 2;
    }

    int main() {
        int x = f();
        int y = g();
        int result = x + y;
        std::cout << result << std::endl; // Output: 3
        // Output order:
        // f() called
        // g() called
        // 3
        return 0;
    }
    ```

### "Sequenced Before" Rules

The "sequenced before" rules define a partial order on evaluations, establishing when one expression must be evaluated before another. These rules help in determining the order in which expressions are executed, ensuring certain side effects happen in a defined order.

1. **Operands of Operators**

    For most operators, there is no sequencing relationship between the evaluations of their operands. For example, in the expression `f() + g()`, the calls to `f` and `g` are unsequenced relative to each other.

2. **Function Arguments**

    The order of evaluation of function arguments is unspecified, meaning `f(a(), b())` can call `a` and `b` in any order.

3. **Comma Operator**

    The comma operator introduces a sequence point, ensuring that the expression on the left is evaluated before the expression on the right.

    ```cpp
    int main() {
        int x = 0;
        int y = (x = 1, x + 2); // x is set to 1, then y is calculated as x + 2
        std::cout << y << std::endl; // Output: 3
        return 0;
    }
    ```

4. **Short-Circuit Operators**

    The logical AND (`&&`) and OR (`||`) operators introduce sequence points. For `&&`, the right operand is evaluated only if the left operand is true. For `||`, the right operand is evaluated only if the left operand is false.

    ```cpp
    int main() {
        int x = 0;
        if (x == 0 || ++x == 2) {
            std::cout << "x is still 0" << std::endl; // Output: x is still 0
        }
        return 0;
    }
    ```

5. **Conditional Operator**

    The conditional (ternary) operator also introduces a sequence point between the condition and the selected subexpression.

    ```cpp
    int main() {
        int x = 5;
        int y = (x > 0) ? ++x : --x;
        std::cout << y << std::endl; // Output: 6
        return 0;
    }
    ```

Understanding these rules helps in writing correct and predictable C++ programs, especially when dealing with complex expressions involving side effects.
### Control Statements

Control statements are used to direct the flow of execution in a program based on certain conditions or to repeat a set of statements.

## `if`

`if` is a control statement that evaluates a boolean expression and executes the following block of code if the expression is true. Additional `else if` and `else` blocks can be used to handle multiple conditions.

```cpp
int x = 0;
std::cin >> x;

if (x > 0) {
    // Code executed if x > 0
} else if (x < 0) {
    // Code executed if x < 0
} else if (x > -10) {
    // Code executed if x > -10
} else if (x > -100) {
    // Code executed if x > -100
} else {
    // Code executed if none of the above conditions are met
}
```

You can also use a declaration statement inside an `if` condition:

```cpp
std::vector<int> v = {1, 2, 3};

if (int size = v.size(); size > 0) {
    // Use size in this block
}
```

## `switch`

`switch` is a control statement that selects a case to execute based on the value of a variable. It allows branching into different sections of code.

```cpp
int variable = 0;
std::cin >> variable;

switch (variable) {
    case 1:
        std::cout << "AAAA";
        break; // Exit switch after case 1
    case 2:
        std::cout << "BBBB";
        // No break, so execution falls through to default
    default:
        std::cout << "CCCC";
        break; // Exit switch after default
}
```

## `while`

`while` repeatedly executes a block of code as long as a specified boolean expression is true.

```cpp
int x = 0;
while (x < 10) {
    std::cout << x << std::endl;
    ++x;
}
```

`do-while` guarantees that the block of code is executed at least once before the boolean expression is tested.

```cpp
int x = 0;
do {
    std::cout << x << std::endl;
    ++x;
} while (x < 10);
```

## `for`

`for` loops are used to iterate over a range of values or until a condition is met.

```cpp
for (int i = 0; i < 10; ++i) {
    std::cout << i << std::endl;
}
```

### Unconventional Use of `for`

A `for` loop can be used in unconventional ways, such as initializing multiple variables or creating infinite loops.

```cpp
for (int i = 0, j = 10; i < j; ++i, --j) {
    std::cout << i << " " << j << std::endl;
}

for (;;) {
    // Infinite loop
}
```

### Range-Based `for`

Range-based `for` loops simplify iteration over containers.

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
for (int n : v) {
    std::cout << n << std::endl;
}
```

## `goto` and `label`

`goto` jumps to a labeled statement, which can lead to unstructured and hard-to-maintain code, so it is generally discouraged.

```cpp
int main() {
    int x = 0;
    std::cin >> x;

    if (x < 0) {
        goto negative;
    }

    std::cout << "Positive or zero" << std::endl;
    return 0;

negative:
    std::cout << "Negative" << std::endl;
    return 0;
}
```

### Errors with `goto` and `label`

Using `goto` to jump to a label within an inner scope can lead to errors, such as accessing uninitialized variables.

```cpp
int main() {
    int x = 0;
    std::cin >> x;

    {
        int y = 5;
label:
        ++y; // Label within inner scope
    }

    ++x;
    std::cout << x << std::endl;
    goto label; // Error: label not within scope
}

// error: use of uninitialized variable 'y'
```

### `try` and `catch`
`try` and `catch` is control statements used for error handling.