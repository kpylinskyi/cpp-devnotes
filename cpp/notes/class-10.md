# Constants in C++

Constants in C++ are type modifiers that restrict variable modification operations, making them read-only.

## Basic Usage of `const`

A `const` variable is initialized when it is declared and cannot be modified afterward.

```cpp
int x = 5; // We can change x

const int cx = 5; // Initialization
cx = 10; // CE: Cannot assign to constant, assignment of read-only variable
++cx; // CE: Cannot increment constant variable
```

For `const` types, modification operations are deleted.

## `const` with Pointers

### Pointer to Constant Data

A pointer to `const` data means that the data being pointed to cannot be changed through that pointer.

```cpp
int x = 5;
int y = 10;
int* p = &x;
const int* pc = p; // Value is constant, pointer is not
*pc = 1; // CE: Cannot modify value through const pointer
pc = &y; // Ok, changing the pointer itself is allowed
```

### Constant Pointer

A constant pointer means that the pointer itself cannot be changed, but the data it points to can be.

```cpp
int x = 5;
int y = 10;
int* const cp = &x; // Pointer is constant, value is not
*cp = 1; // Ok, modifying value through pointer is allowed
cp = &y; // CE: Cannot change constant pointer
```

### Constant Pointer to Constant Data

Both the pointer and the data it points to are constant.

```cpp
int x = 5;
const int* const cpc = &x; // Const pointer with const value
```

## Pointer Conversions

### Converting `int*` to `const int*`

This is allowed and safe:

```cpp
int x = 5;
int* p = &x;
const int* pc = p; // int* -> const int*
```

### Converting `const int*` to `int*`

This is not allowed because it would break `const` safety:

```cpp
const int* pc = &x;
int* p2 = pc; // CE: Cannot convert const int* to int*
```

## Example: `const` Pointers and References

```cpp
int main() {
    int x = 5;
    const int* p = &x;
    ++x; // Incrementing x is allowed 
    std::cout << *p; // Pointer value changed, prints 6
}
```

### Constant References

A constant reference to a variable ensures that the referenced value cannot be modified through that reference.

```cpp
int main() {
    int x = 5;
    const int& rx = x; // Constant reference
    ++x; // Ok
    ++rx; // CE: Cannot modify through constant reference
    std::cout << rx; // Prints 6
}
```

### Invalid Reference Assignment

A non-constant reference cannot be bound to a constant reference:

```cpp
const int& rx = x;
int& rx2 = rx; // CE: Cannot bind non-const reference to const reference
```

### Dynamic Allocation with `const`

```cpp
int main() {
    const int* p1 = new const int; // CE: Uninitialized const 
    const int* p2 = new const int(1); // Ok
    const int* p3 = new const int[10]{}; // Ok, array of 10 const ints initialized to 0
}
```

## Function Arguments and `const`

### Passing Arguments to Functions

- `f(std::string str)` - Pass by value
- `f(const std::string str)` - Pass by value and make it `const` (not efficient)
- `f(std::string& str)` - Pass by non-constant reference
- `f(const std::string& str)` - Pass by constant reference (recommended for large objects)

```cpp
std::string substr(const std::string& str, size_t from, size_t to) {
    // Function implementation
}
```

### References and Rvalues

Non-constant references cannot be initialized with rvalues, but constant references can.

```cpp
std::string& str_ref = "abcd"; // CE: Cannot bind rvalue to non-const reference
const std::string& const_str_ref = "abcd"; // Ok
```

## Lifetime Extension

When a `const` reference is initialized with a temporary, the lifetime of the temporary is extended to match the lifetime of the reference.

```cpp
const std::string& s_ref = "aaa"; // Lifetime of "aaa" is extended
```

For small types such as `int` or `double`, it is better to pass by value:

```cpp
void f(int); // Better than
void f(int&); // Because dereferencing is not needed
```

## Dangers of Lifetime Extension for Non-Constant References

Temporary objects cannot be bound to non-constant references:

```cpp
void g(size_t& y) {
    ++y;
}

int main() {
    int x = 0;
    g(x); // CE: No matching function for call to 'g'
}
```

## Function Overloading with `const`

```cpp
void f(const std::string&); // Implicit conversion
void f(std::string&); // Exact type matching

int main() {
    std::string s = "abc";
    f(s); // Calls void f(std::string&)
    
    const std::string& rs = s;
    f(rs); // Calls void f(const std::string&)
}
```

## Dangling References

Lifetime extension applies only to declared constant references, not to those returned from functions:

```cpp
const int& g(int x) {
    return x++; // Returns reference to local variable, which is destroyed
}

int main() {
    g(5); // Dangling reference
}
```

## Multiple-Level Pointers and `const`

Adding `const` for multiple-level pointers is not straightforward:

```cpp
int* p = new int(0);
int** pp = &p;
const int** cpp = pp; // CE: Cannot convert int** to const int**
```

### Example of Potential Issue

```cpp
const char x = 'a';
char* p = nullptr;
const char* /*nonconst pointer*/ * /*nonconst char*/ q = &p; // q now points to p
*q = &x; // p now points to x
*p = 'b'; // Modifies a const char!
std::cout << x; // Prints an invalid character
```

Correct usage:

```cpp
const char* const* q = &p; // Both pointer and pointed data are const
```