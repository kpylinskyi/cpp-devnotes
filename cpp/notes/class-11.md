Continuing from where you left off, here are the expanded notes with additional details and completion:

# Type Casting
Type casting in C++ allows conversion of one data type to another. There are several types of casts, each with specific use cases and rules.

## Static Cast
`static_cast<to>(from)`
- Used for well-defined and standard type conversions.
- Converts any pointer to a void pointer and back.
- Can be used to cast up or down a class hierarchy.
- Should be used when the conversion is known to be safe at compile time.

Example:
```cpp
int main() {
    int x = 0;
    double b = static_cast<double>(x); // Converts int to double
    std::cout << b << std::endl;
    return 0;
}
```

## Reinterpret Cast
`reinterpret_cast<to>(from)`
- Reads the bytes of one type as bytes of a different type without considering sizes.
- Often leads to undefined behavior if used incorrectly.
- Should only be used for low-level casting when you know what you're doing.
- Used to cast to pointer or reference types, not recommended for casting to a different type directly.

Example:
```cpp
int main() {
    long long y = 1729;
    double& d = reinterpret_cast<double&>(y); // Interprets the bytes of y as a double
    std::cout << d << std::endl;
    return 0;
}
```

Casting to a pointer:
```cpp
int main() {
    int x = 1729;
    int* p = &x;
    std::string* str = reinterpret_cast<std::string*>(p);
    std::cout << *str; // This will likely cause a runtime error
    return 0;
}
```

## Const Cast
`const_cast<to>(from)`
- Used to add or remove the `const` qualifier from a variable.
- Can only cast pointers or references, not values.
- Useful when you need to modify a variable that was originally declared as `const`.

Example:
```cpp
int main() {
    const int c = 5;
    int& cc = const_cast<int&>(c);
    cc = 57;
    std::cout << c << ' ' << cc; // Outputs implementation defined
    return 0;
}
```

Function taking a `const` reference:
```cpp
void f(const int& c) {
    // Function implementation
}

int main() {
    int x = 0;
    f(x); // Valid call
    return 0;
}
```

## Implicit Conversion
- Implicit conversions are automatically applied by the compiler when types are compatible.
- Can lead to unexpected results if not properly understood.

### C-style Cast
- A C-style cast tries to convert using all possible methods (static_cast, reinterpret_cast, const_cast).
- Considered less safe and less readable compared to C++-style casts.

Example:
```cpp
int main() {
    int x = 10;
    double y = (double)x; // C-style cast from int to double
    std::cout << y << std::endl;
    return 0;
}
```

### C-style Cast Logic
- First, attempts `const_cast`.
- If that fails, tries `static_cast`.
- If that fails, attempts `reinterpret_cast`.
- If all fail, it raises an error.

## Three Forbidden Casts
- `reinterpret_cast`, `const_cast`, and C-style casts are not recommended due to potential safety and readability issues.
- Prefer using `static_cast` and other explicit casts for better code maintenance and safety.

# Building Stages
The build process of a C++ program involves several stages, each transforming the source code in different ways.

## Preprocessing
- Handles preprocessor directives such as `#include`, `#define`, `#if`, etc.
- The output is a pure C++ source file with all macros expanded and included files merged.

Example:
```cpp
# include <iostream>
# include "class.h"
# if DEBUG
# pragma message("Debug mode")
# endif
```

## Compilation
- Translates the preprocessed C++ code into assembly code.
- The output is an assembly file.

## Assembling
- Converts assembly code into machine code, creating object files with the `.o` extension.

## Linking
- Combines object files and resolves symbols to produce the final executable.
- Links with necessary libraries and other object files to produce the final output.

Example of a simple build process:
```plaintext
file.cpp
    || preprocessor
    \/
file.i
    || compiler
    \/
file.s
    || assembler
    \/
file.o
    || linker
    \/
a.out
```

## Reading the Build Process
- Use `-v` for verbose output to understand each stage of the build process:
```sh
g++ -v main.cpp
```

- To see the preprocessed output, use `-E`:
```sh
g++ -E main.cpp > output.cpp
```

- To get the assembly code, use `-S`:
```sh
g++ -S main.cpp
```

- To get the object file, use `-c`:
```sh
g++ -c main.cpp
```

# Sanitizers
Sanitizers are tools used to detect various runtime errors in programs.

## Address Sanitizer
- Detects memory corruption, buffer overflows, use-after-free, etc.
- Use with `-fsanitize=address`:
```sh
g++ -fsanitize=address -g main.cpp
```

## Leak Sanitizer
- Detects memory leaks.
- Use with `-fsanitize=leak`:
```sh
g++ -fsanitize=leak -g main.cpp
```

## Undefined Behavior Sanitizer
- Detects undefined behavior.
- Use with `-fsanitize=undefined`:
```sh
g++ -fsanitize=undefined -g main.cpp
```