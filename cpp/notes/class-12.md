# III. Basics of OOP

## 3.1 Classes and Structures, Encapsulation

### Declaration
- **Classes** and **structures** can be declared in C++ before they are fully defined. This allows for forward declarations, which are useful in cases where you want to refer to a type before providing its complete definition.

Example:
```cpp
class C;    // Forward declaration of class C
struct S;   // Forward declaration of struct S
```

### Definition
- The **definition** of a class or structure includes the declaration of its members (fields, methods, etc.). This is where memory is allocated for its members.

Example:
```cpp
class C {
    int x = 1;  // Private by default
};

int main() {
    C c;
    // std::cout << c.x; // Error: x is private
}
```

- Structures in C++ are similar to classes but have public access by default.

Example:
```cpp
struct S {
    int x = 1;        // Public by default
    double d = 3.14;  // Public by default
};

int main() {
    S s;
    std::cout << s.x << " " << s.d; // Outputs: 1 3.14
}
```

### Memory Layout
- The memory layout of a class or structure in C++ follows the order in which the members are defined. However, due to padding for alignment, the size of a structure might be larger than the sum of the sizes of its individual members.

Example:
```cpp
struct S {
    int x;       // sizeof(int) is 4 bytes
    double d;    // sizeof(double) is 8 bytes
}; // sizeof(S) may be 16 bytes due to alignment
```

- In the above example, the structure `S` occupies 16 bytes, not 12 bytes, due to alignment requirements.

### Aggregate Initialization
- Structures and classes can be initialized using aggregate initialization, where you directly list the values for the members in curly braces.

Example:
```cpp
struct S {
    int x;
    double d = 3.14;
};

int main() {
    S s{2, 6.28};  // x = 2, d = 6.28
}
```

### Methods
- Methods (member functions) can be defined within the structure or class, providing functionality related to the object's data.

Example:
```cpp
struct S {
    int x;
    void f(int y) {
        std::cout << x * y;
    }
};

int main() {
    S s{2};
    s.f(5); // Outputs: 10
}
```

- **No forward declaration needed**: Inner methods in structures or classes can be used before they are fully defined.

Example:
```cpp
struct S {
    int x;
    void f(int y) {
        int n = x * y;
        print(n);  // Calls the print function defined later
    }
    void print(int n) {
        std::cout << n;
    }
};

int main() {
    S s{2};
    s.f(5); // Outputs: 10
}
```

- **Method definitions outside the class/struct**: Methods can also be defined outside the class/struct body using the scope resolution operator (`::`).

Example:
```cpp
struct S {
    void f(int x);  // Method prototype
};

void S::f(int x) {  // Method definition outside the struct
    std::cout << x * x;
}
```

### `this` Keyword
- The `this` pointer is an implicit pointer available within non-static member functions, pointing to the instance of the class or structure that invoked the method.

Example:
```cpp
struct S {
    int x;
    void f(int x) {
        std::cout << x;           // Local x (parameter)
        std::cout << this->x;      // Member x
        std::cout << (*this).x;    // Member x
    }
};
```

### Arrow Operator (`->`)
- The arrow operator (`->`) is used to access members of a structure or class through a pointer.

Example:
```cpp
struct S {
    int x;
};

int main() {
    S* s = new S;
    s->x = 10;              // Using arrow operator to access x
    std::cout << s->x;      // Outputs: 10
    delete s;               // Don't forget to delete dynamically allocated memory
}
```

### Inner Structures
- Structures or classes can be nested within other structures or classes. The inner structure can be used to logically group related data.

Example:
```cpp
struct A {
    int x;
    double d = 3.14;
    
    struct AA {
        char c;
    } aa; // Variable of type AA
};

int main() {
    A a; // sizeof(a) is 16 bytes (due to alignment)
    A::AA aa; // sizeof(aa) is 1 byte (just char c)
}
```

- **Inner structure variables**: You can declare a variable of an inner structure type right after its definition within the outer structure.

### Difference Between Structures and Classes
- The primary difference between `struct` and `class` in C++ is the default access level:
  - **Structures (`struct`)**: Members are `public` by default.
  - **Classes (`class`)**: Members are `private` by default.

Example:
```cpp
struct S {
    int x; // Public by default
};

class C {
    int x; // Private by default
};
```

This basic understanding of classes, structures, and encapsulation forms the foundation of object-oriented programming in C++. These concepts are crucial for creating well-structured and maintainable code.