## II. Compound Types

### 2.1 Pointers

Pointers are variables that store memory addresses. They allow for the manipulation of data stored in different memory locations.

#### `&` Address of Operator

The `&` operator is used to obtain the address of a variable.

```cpp
int main(){
    int x = 0;
    std::cout << &x; // Outputs the address of 'x'
}
```

#### `T*` Pointer Type

A pointer is declared using the `*` symbol after the type.

```cpp
int main(){
    int x = 0;
    int* px = &x; // 'px' is a pointer to 'x'
    std::cout << px; // Outputs the address stored in 'px'
}
```

#### `*px` Dereferencing

Dereferencing a pointer means accessing the value stored at the address the pointer is pointing to.

```cpp
int main(){
    int x = 0;
    int* px = &x; // 'px' points to 'x'
    int xval = *px; // 'xval' is assigned the value of 'x'
    std::cout << xval; // Outputs the value of 'x'
}
```

#### Pointer Arithmetic

Pointers can be incremented or decremented to point to other elements in an array or a sequence of contiguous memory locations.

```cpp
std::vector<int> v {1, 2, 3};
int* p = &v[0];
```

Adding

```cpp
int* p1 = p + 1; // Points to the second element in 'v'
std::cout << *p1; // Outputs: 2
```

Subtracting

```cpp
int* p2 = p + 2; // Points to the third element in 'v'
std::cout << *p2; // Outputs: 3
p2 = p2 - 1; // Points back to the second element
std::cout << *p2; // Outputs: 2
```

Incrementing

```cpp
std::cout << *p; // Outputs: 1
++p; // Points to the second element
std::cout << *p; // Outputs: 2
++p; // Points to the third element
std::cout << *p; // Outputs: 3
```

Decrementing

```cpp
--p; // Points back to the second element
std::cout << *p; // Outputs: 2
--p; // Points back to the first element
std::cout << *p; // Outputs: 1
```

Difference

```cpp
std::cout << &v[0] - &v[2]; // Outputs: -2
```

#### Pointer to Pointer

A pointer to a pointer is a pointer that stores the address of another pointer.

```cpp
int main(){
    int a = 0;
    int* p = &a;
    std::cout << p << '\n'; // Outputs the address of 'a'
    int** pp = &p; // 'pp' is a pointer to 'p'
    std::cout << pp << '\n'; // Outputs the address of 'p'
    
    std::cout << *pp << " equals " << p << '\n'; // Both are the address of 'a'
    std::cout << **pp << " equals " << *p << " equals " << a << '\n'; // All are the value of 'a'
}
```

#### Size of Pointer

The size of a pointer is typically 8 bytes on a 64-bit system.

```cpp
int main(){
    int a = 0;
    int* p = &a;
    std::cout << sizeof(p) << '\n'; // Outputs: 8
}
```

#### Dereferencing Address-of Operator

Using `*&` or `&*` to get back the original pointer or variable.

```cpp
int main(){
    int a = 0;
    int* p = &a;

    std::cout << *&p << ' ' << &*p << '\n'; // Outputs: address of 'a' twice
}
```

#### Assigning to Dereferenced Pointer

You can assign a value to the location a pointer is pointing to.

```cpp
int main() {
    int a = 0;
    int* p = &a;
    *p = 1; // Value of 'a' is set to 1
    std::cout << a; // Outputs: 1
}
```

However, you can't assign to the address of a variable directly.

```cpp
int main() {
    int a = 0;
    &a = 1; // Error: lvalue required as left operand of assignment
}
```

#### Undefined Behavior Example

Accessing a pointer after its target goes out of scope leads to undefined behavior.

```cpp
int main(){
    int a = 1;
    int* p = &a;
    {
        int b = 2;
        p = &b;
    }

    std::cout << p << '\n'; // Outputs: address of 'b'
    std::cout << *p << '\n'; // UB: possibly outputs 2 or anything else
}
```

Memory reuse leads to undefined behavior.

```cpp
int main(){
    int a = 1;
    int* p = &a;
    {
        int b = 2;
        p = &b;
    }

    std::cout << p << ' : ' << *p << '\n';

    int c = 3, d = 4, e = 5, f = 6;
    std::cout << &c << ' ' << &d << ' ' << &e << ' ' << &f << '\n';

    // c, d, e, f can use the address of b, since it is out of scope

    ++*p; // UB: It's possible for one of c, d, e, f to be changed
    std::cout << c << d << e << f << '\n'; // Possible different values from 3456

    std::cout << *p << '\n'; // Possible not 2 anymore
}
```

#### Working with Pointers of Different Types

Comparing pointers of different types is not allowed.

```cpp
int main(){
    int x = 0;
    double y = 1.5;

    // std::cout << (&x < &y); // Error: comparison of distinct pointer types
}
```

Accessing memory outside the bounds of an array leads to undefined behavior.

```cpp
int main(){
    int x = 0;
    int y = 1;
    int* p = &x;
    *++p; // UB: no guarantee that we find y after x
}
```

#### `void*` Type

A `void*` pointer can point to any data type, but it cannot be dereferenced without casting.

```cpp
int main(){
    int x = 0;
    void* vp = &x;
    // Cannot increment or decrement
    // Cannot dereference void*
}
```

#### `nullptr` Keyword

In C++, `nullptr` is used to represent a null pointer.

```cpp
int main(){
    int* p = nullptr;
    if (p == nullptr) {
        std::cout << "p is null\n";
    }
    // shorter nullptr check
    if (!p) {
	    std::cout << "p is null\n";
    }
}
```

#### Pointers in Use: Swapping Values

Swapping values using pointers and references.

```cpp
void naive_swap(int x, int y){
    int t = x;
    x = y;
    y = t;
}

void c_style_swap(int* x, int* y){
    int t = *x;
    *x = *y;
    *y = t;
}

void swap(int& x, int& y){
    int t = x;
    x = y;
    y = t;
}

int main(){
    int x = 0;
    int y = 1;

    naive_swap(x, y); // No effect
    c_style_swap(&x, &y); // Swaps x and y
    swap(x, y); // Swaps back x and y

    std::cout << x << ' ' << y << '\n'; // Outputs: 0 1
}
```

#### C-Style Use Cases

C-style functions often use pointers to modify variables or access arrays.

```cpp
int main() {
    int x = 0;
    printf("%d", x); // By value
    scanf("%d", &x); // By address
}
```