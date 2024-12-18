# Compile Time Errors, Runtime Errors, Undefined Behavior

Understanding the types of errors and undefined behaviors in C++ is essential for effective debugging and writing robust code.

## Compile Time Errors (CE)

Compile time errors occur during the compilation phase and prevent the code from being successfully compiled. These errors are typically related to syntax, semantics, and lexical issues.

### Token Interpretation of a Program

The compiler processes the code in multiple stages, one of which involves breaking down the code into tokens through lexical analysis.

```cpp
int main() {
    int x;             // tokens: int, x, ;
    std::cin >> x;     // tokens: std::cin, >>, x, ;
    std::cout << x + 5;// tokens: std::cout, <<, x, +, 5, ;
}
```

### Syntax Interpretation Tree

After tokenizing the code, the compiler builds a syntax tree to understand the structure and relationships between tokens.

### Errors

#### Lexical Error

Lexical errors occur when the compiler encounters invalid tokens that do not conform to the language's rules.

```cpp
int main() {
    \\; // CE: stray '\' in program
    std::cout << "Hello!";
}
```

Another example:
```cpp
int main() {
    int 1x = 0; // CE: invalid token '1x'
}
```

#### Syntax Error

Syntax errors occur when the code violates the grammatical rules of the programming language.

```cpp
int main() {
    int x = 0;
    x = x + ; // CE: expected primary-expression before ';' token
}
```

Another example:
```cpp
int main() {
    if (x // CE: expected ')' before '{' token
    {
        std::cout << "Hello!";
    }
}
```

#### Semantic Error

Semantic errors occur when the code is syntactically correct but makes no sense in terms of the language's semantics, often due to type mismatches or invalid operations.

```cpp
int main() {
    std::cout << "abc" + 5.0f; // CE: invalid operands of types 'const char[4]' and 'float' to binary 'operator+'
}
```

Another example:
```cpp
int main() {
    int x = "string"; // CE: invalid conversion from 'const char*' to 'int'
}
```

## Runtime Errors (RE)

Runtime errors occur during the execution of the program and can cause the program to terminate abnormally or produce incorrect results. These errors are often due to invalid operations, accessing illegal memory, or other unforeseen issues that the compiler cannot catch during compilation.

### Segmentation Fault (Segfault)

A segmentation fault occurs when a program tries to read or write to a memory location that it's not supposed to access. This often happens due to dereferencing invalid pointers or accessing out-of-bounds elements in arrays or vectors.
#### Invalid Array Access

Accessing an array out of bounds can lead to undefined behavior or a segmentation fault.

```cpp
#include <iostream>

int main() {
    int arr[3] = {1, 2, 3};
    std::cout << arr[4]; // Runtime Error: Undefined behavior, possibly segmentation fault
}
```


```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v(10);
    v[50'000] = 1; // Runtime Error: Segmentation fault (segfault)
}
```

#### Null Pointer Dereference
Accessing memory through a null pointer results in a segmentation fault.

```cpp
#include <iostream>

int main() {
    int* p = nullptr;
    *p = 5; // Runtime Error: Segmentation fault (segfault)
}
```

### Floating Point Exception (FPE)

A floating point exception occurs during illegal arithmetic operations such as division by zero.

```cpp
#include <iostream>

int main() {
    float y;
    std::cin >> y; // Enter 0.0f
    std::cout << 5 / y; // Runtime Error: Floating Point Exception (FPE)
}
```

Another example:

```cpp
#include <iostream>
#include <cmath>

int main() {
    double x = -1.0;
    std::cout << sqrt(x); // Runtime Error: Floating Point Exception (FPE), sqrt of negative number
}
```

### Aborted

An "Aborted" runtime error often occurs due to failed assertions or violations of runtime checks, such as accessing out-of-bounds elements using methods that perform bounds checking.

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v(10);
    v.at(50'000); // Runtime Error: Aborted (core dumped)
}
```

Another example:

```cpp
#include <iostream>
#include <cstdlib>

int main() {
    std::vector<int> v(10);
    if (v.size() > 50'000) {
        abort(); // Runtime Error: Aborted (core dumped)
    }
}
```

### Stack Overflow
#### Infinite Recursion

Infinite recursion can lead to a stack overflow.

```cpp
#include <iostream>

void recursiveFunction() {
    recursiveFunction(); // Runtime Error: Stack overflow
}

int main() {
    recursiveFunction();
}
```


## Undefined Behavior (UB)

Undefined behavior in C++ refers to code that can produce unpredictable results, including crashes, incorrect results, or even no effect at all. The compiler is free to handle undefined behavior in any manner, including generating unexpected machine code. Understanding and avoiding undefined behavior is crucial for writing reliable and safe programs.

### Examples of Undefined Behavior

1. **Out of Bounds Access**

    Description: Accessing elements outside the valid range of a container.

    ```cpp
    int main() {
        std::vector<int> v(10);
        v[10] = 1; // UB: Accessing element out of bounds.
    }
    ```

2. **Using Uninitialized Variables**

    Description: Using variables that have not been initialized.

    ```cpp
    int main() {
        int x; // Uninitialized
        std::cout << x; // UB: Reading an uninitialized variable.
    }
    ```

3. **Multiple Modifications Between Sequence Points**

    Description: Modifying a variable multiple times between sequence points without a clear order of operations.

    ```cpp
    int main() {
        int y = 1;
        y++ + ++y; // UB: Multiple modifications to y between sequence points.
    }
    ```

4. **Integer Overflow**

    Description: Overflowing an integer value, which is undefined for signed integers.

    ```cpp
    int main() {
        for (int i = 0; i < 300; ++i) {
            std::cout << i << ' ' << i * 12345678 << std::endl;
        }
    }
    ```

    Explanation: Running this program with the `-O2` optimization flag can lead to an endless loop because the multiplication `i * 12345678` can cause an overflow, making the compiler assume the loop condition `i < 300` is always `false`, which leads to a tautology and an infinite loop.

## Ill-formed Programs

An ill-formed program is one that does not conform to the syntax or semantic rules of the C++ standard. These programs should not compile, and the compiler should produce an error message.

### Example of Ill-formed Program

```cpp
int main() {
    int x = "Hello"; // CE: invalid conversion from 'const char*' to 'int'
}
```

## Ill-formed No Diagnostic Required

Certain programs are ill-formed, but the C++ standard does not require the compiler to diagnose them. This can lead to subtle bugs if the compiler does not produce a warning or error message.

### Example of Ill-formed No Diagnostic Required

```cpp
int main() {
    int arr[10];
    // Using an array index that is out of bounds is ill-formed but may not be diagnosed.
    arr[20] = 5; // UB: This access is out of bounds, but may not produce a compilation error.
}
```

## Implementation-defined Behavior

Behavior where the C++ standard allows different implementations to choose different behaviors. It is well-defined but can vary between different compilers or platforms.

### Example of Implementation-defined Behavior

```cpp
#include <iostream>

int main() {
    int x = -10;
    unsigned int y = x; // Implementation-defined behavior: conversion of negative int to unsigned int.
    std::cout << y << std::endl; // May produce different results on different systems.
}
```

## Unspecified Behavior

Behavior where the standard allows multiple possible outcomes without mandating which one should be chosen. The exact behavior is not predictable but is within a defined range of possibilities.

### Example of Unspecified Behavior

```cpp
#include <iostream>

void f(int a, int b) {
    std::cout << a << ' ' << b << std::endl;
}

int main() {
    int x = 1;
    int y = 2;
    // The order of evaluation of x and y is unspecified.
    f(x++, y++); // Output could be "1 2" or "2 1" or other variations.
}
```

### Aggressive Optimization

Modern compilers perform aggressive optimizations based on the assumption that the code is free of undefined behavior. This can lead to surprising results if the code actually contains UB.

### The As-if Rule

The as-if rule allows the compiler to optimize code under the assumption that the program's observable behavior remains the same. It can rearrange, combine, or eliminate operations as long as the final observable output is consistent with the original code.

## Warnings

Warnings are messages generated by the compiler to alert the programmer to potential issues in the code that might lead to undefined behavior, logic errors, or other unintended consequences. They do not stop the compilation but serve as a helpful tool to prevent bugs and improve code quality.

### Examples of Common Warnings

1. **Unused Expression Result**
   
   ```cpp
   int main() {
       5; // warning: expression result unused
       return 0;
   }
   ```

2. **Potential Mistaken Assignment**

   ```cpp
   int main() {
       int x = 0;
       if (x = 5) { // warning: use '==' to turn this assignment into an equality comparison
           // do something
       }
       return 0;
   }
   ```

3. **Multiple Unsequenced Modifications**

   ```cpp
   int main() {
       int x = 0;
       x = x++ + ++x; // warning: multiple unsequenced modifications to 'x'
       return 0;
   }
   ```

### Warning Flags

Compilers provide various flags to control the generation of warnings. These flags help in detecting potential issues and enforcing stricter coding standards.

- `-Wall` : Enables a common set of warnings, not all but a good start.
- `-Wextra` : Enables additional warnings not covered by `-Wall`.
- `-Wpedantic` : Enforces strict compliance with the language standard.
- `-Werror` : Treats all warnings as errors, stopping the compilation.
- `-Wno-error=<warning-name>` : Disables treating a specific warning as an error.

### Pragma to Push and Pop Warnings

Pragma directives can be used to modify compiler behavior for specific sections of code, such as suppressing specific warnings.

Corrected example:

```cpp
int main() {
    int x;
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wunused-value"
    5; // Suppress the "unused value" warning here
#pragma GCC diagnostic pop
    return 0;
}
```