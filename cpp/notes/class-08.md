# Arrays

Arrays in C++ are collections of elements of the same type stored in contiguous memory locations. They are useful for handling multiple data items of the same type using a single name.

## Initialization

```cpp
int main() {
    int a[5] = {1, 2, 3, 4, 5}; // Set size and initializer list
    int a[] = {1, 2, 3, 4, 5};  // Size is set automatically
    int b[5] = {1, 2};          // Remaining values are set to 0
    int b[5]{1, 2};             // Initialization is not necessary for all elements
    int c[10]{};                // Size 10, all values are 0
}
```

## Indexing

```cpp
int main() {
    int a[] {1, 2, 3, 4, 5};
    a[0] = 10;
    a[4] = 500;
}
```

## Size of Array

The `sizeof` operator returns the total size of the array in bytes.

```cpp
int main() {
    int a[5] = {1, 2, 3, 4, 5};
    std::cout << sizeof(a) << std::endl; // 5 * sizeof(int) == 20 (assuming int is 4 bytes)
}
```

To get the number of elements in an array:

```cpp
int main() {
    int a[5] = {1, 2, 3, 4, 5};
    for (int i = 0; i < sizeof(a) / sizeof(int); ++i) {
        std::cout << a[i] << " ";
    }
}
```

## Stack Overflow

Large arrays can cause stack overflow errors.

```cpp
int main() {
    int a[10'000'000]; // May cause segmentation fault
}
```

## Static Arrays

Static arrays avoid stack overflow by allocating memory in the static memory segment.

```cpp
static int a[10'000'000]; // No segmentation fault
```

## Array to Pointer Conversion

Arrays can decay to pointers when used in expressions.

```cpp
int main() {
    int a[5] = {1, 2, 3, 4, 5};
    std::cout << *a << std::endl;         // Prints 1, the first element
    std::cout << *(a + 2) << std::endl;   // Prints 3
    std::cout << a[2] << " " << *(a + 2) << " " << 2[a] << " " << *(2 + a); // All print 3
}
```

`a[2]` is equivalent to `*(a + 2)` and `2[a]` is equivalent to `*(2 + a)`.

## Assignment and Incrementing

Assigning one array to another directly is illegal and results in a compilation error.

```cpp
int main() {
    int a[5]{1, 2, 3, 4, 5};
    int b[5]{};
    // a = b; // Compilation error
}
```

Incrementing an array is also illegal.

## Function Arguments

When an array is passed to a function, it decays to a pointer.

```cpp
void f(int* a) {
    std::cout << "Hi! " << a[2];
}

void f(int a[3]) { // Redefinition of f(int*), illegal
    std::cout << "Boom!";
    std::cout << sizeof(a); // Returns sizeof(int*)
}
```

## Dynamic Arrays

Dynamic arrays are allocated on the heap using `new` and must be deleted with `delete[]`.

```cpp
int main() {
    int* a = new int[100]; // a is a pointer to the array start
    delete[] a;
}
```

## Vector as Dynamic Array Wrapper

`std::vector` is a standard library container that provides dynamic array capabilities.

```cpp
int main() {
    std::vector<int> v(10);
    v[-1] = 500000; // Undefined behavior, accessing out of bounds
}
```

## Variadic Length Arrays (VLA)

VLAs allow for arrays with sizes determined at runtime but are not part of the C++ standard and are generally discouraged.

```cpp
int main() {
    int n;
    std::cin >> n;
    // VLA (not standard in C++)
    int a[n]; // Works in some compilers, not portable
    for(int i = 0; i < n; ++i) {
        a[i] = i;
    }
}
```

## Multidimensional Arrays

Multidimensional arrays are arrays of arrays.

```cpp
int main() {
    int a[5][5];   // 2D array
    int* b[5];     // Array of pointers to int
    int (*c)[5];   // Pointer to array of 5 ints
}
```

Passing multidimensional arrays to functions:

```cpp
void f(int**); 
void f(int* [5]); // Same as first, redefinition
void f(int(*)[5]); // Pointer to array of 5 ints
```

## String as Char Array

Char arrays can be used to store strings. The output stream for char arrays prints until a null terminator `\0` is encountered.

```cpp
int main() {
    char* s1 = "abcdefghi\0";
    // s1[0] = 'A'; // Segfault, string literal is read-only

    char a[5] = {'a', 'b', 'c', 'd', 'e'};
    // a[5] = 'a'; // Undefined behavior, out of bounds
    char* p = a;

    const char* s2 = "abcdefghi\0"; // Correct C-style string declaration
}
```

In summary, arrays in C++ provide a way to store multiple items of the same type, with considerations for static and dynamic allocation, array decay to pointers, and multidimensional arrays. They are fundamental to understanding more complex data structures and memory management in C++.

# Pointer to Function

Pointers to functions are used to pass functions as arguments to other functions or to store functions in arrays.

## Passing Function to Function

```cpp
bool cmp(int a, int b) {
    return a > b;
}

int main() {
    int a[5] = {1, 2, 3, 4, 5};
    std::sort(a, a + 5, &cmp);

    for(int i = 0; i < 5; ++i) {
        std::cout << a[i] << " ";
    }
}
```

The type of `&cmp` function is:

```cpp
bool (*p)(int, int) = &cmp;
bool (*p)(int, int) = cmp; // Function to pointer conversion
```

## Address Resolution of Overloaded Functions

Resolving the address of an overloaded function requires specifying the exact function signature.

```cpp
void f(int) {}
void f(double) {}

int main() {
    // &f; // Statement cannot resolve address of overloaded function
    void (*p)(int) = &f; 
    void (*p2)(double) = &f;

    std::cout << (void*)p << ' ' << (void*)p2 << '\n';
}
```

## Complex Declarations

### Arrays and Pointers

```cpp
int main() {
    int* a[10];  // Array of pointers to int of size 10
    int (*b)[10]; // Pointer to array of 10 int
}
```

### Function Pointers

```cpp
int main() {
    void (*pf)(int); // Pointer to function which takes int and returns void
}
```

### Array of Function Pointers

```cpp
void (*pfa[10])(int); // Array of 10 pointers to functions that take an int and return void
```

### Pointers to Function Pointers

```cpp
void (*(*pff[10])(int))(int); // Array of 10 pointers to functions which take an int and return a pointer to a function that takes an int and returns void
```

### Abstract Type Declaration

```cpp
int (*(*)())(); // Pointer to function returning pointer to function returning int
```

**Rule of Thumb**: "Go right when you can, go left when you must."

# Default Arguments

Functions can have default arguments that are used if no corresponding arguments are passed.

```cpp
void point(int x = 3, int y = 4);

int main() {
    point(1, 2); // x = 1, y = 2
    point(1);    // x = 1, y = 4
    point();     // x = 3, y = 4
}
```

# Variadic Functions

Variadic functions can accept a variable number of arguments.

## Examples

### Standard Library Functions

```cpp
#include <cstdio>
#include <cstdarg>

void simple_printf(const char* fmt, ...) {
    va_list args;
    va_start(args, fmt);
    vprintf(fmt, args);
    va_end(args);
}

int main() {
    simple_printf("Hello %s\n", "world");
}
```

### Macros for Variadic Functions

- `va_start`: Initializes the argument list.
- `va_arg`: Retrieves the next argument.
- `va_copy`: Copies the state of

 the argument list.
- `va_end`: Cleans up the argument list.
- `va_list`: Type for iterating arguments.