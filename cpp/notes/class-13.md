# 3.2 Access Modifiers, Friend Functions and Classes, Constructors, and Initialization Lists

## Access Modifiers

Access modifiers control the visibility and accessibility of members (variables and methods) within a class or struct.

### Overview

|Modifier|Description|
|---|---|
|`private`|Accessible only within the defining class.|
|`protected`|Accessible within the defining class and its derived classes.|
|`public`|Accessible from anywhere the object is visible.|

### Example

```cpp
class C {
private:
    int x{5}; // Not accessible outside the class
public:
    void f(int y) { 
        std::cout << x + y; // Allowed because `x` is accessed within the class
    }
};
```

---

## `friend` Keyword

The `friend` keyword allows external functions or classes to access the private and protected members of a class. While it breaks encapsulation to some extent, it can be useful for special operations like operator overloading.

### Friend Functions

A `friend` function is a non-member function with access to private and protected members of a class.

```cpp
class C {
private:
    int x{5}; // Private member
public:
    friend void g(C, int); // Declaration of a friend function
};

void g(C c, int y) {
    std::cout << c.x + y; // Access to private member `x`
}
```

#### Defining Friend Functions Inside the Class

Although allowed, defining a friend function inside the class body is discouraged as it goes against standard style conventions.

```cpp
class C {
private:
    int x{5};
public:
    friend void g(C c, int y) {
        std::cout << c.x + y; // Discouraged style
    }
};
```

### Friend Classes

A friend class can access all private and protected members of another class.

```cpp
class C {
private:
    int x{5};
public:
    friend class CC; // Declares `CC` as a friend
};

class CC {
public:
    void printX(const C& c) {
        std::cout << c.x; // Access to private member `x`
    }
};
```

**Note:** Friendship is not reciprocal; `C` cannot access private members of `CC`.

---

## Inner Private Class

An inner class declared as `private` is only accessible within its containing class. To allow external access, we can provide a getter method.

### Example

```cpp
#include <iostream>

class C {
private:
    class Inner {
    public:
        int x = 1; // Public within the inner class
    private:
        int y = 2; // Private within the inner class
    };
public:
    Inner getInner() { 
        return Inner(); 
    }
};

int main() {
    C c;
    std::cout << c.getInner().x; // Outputs: 1
}
```

---

## Access Modifier Overloading

When overloading methods, access modifiers can affect visibility.

```cpp
#include <iostream>

class C {
private:
    void f(int) { 
        std::cout << 1; 
    }
public:
    void f(float) { 
        std::cout << 2; 
    }
};

int main() {
    C c;
    // c.f(0);   // Error: `0` is not a float; private `f(int)` is inaccessible
    // c.f(3.14); // Error: Ambiguous call (`3.14` is a double)
    c.f(3.14f); // Works, calls `f(float)`, Outputs: 2
}
```

---

## Constructors and Destructors

### Constructors

Constructors initialize an object when it is created. They can accept parameters, provide default values, or be explicitly defined.

#### Syntax

```cpp
class Complex {
private:
    double re = 0.0;
    double im = 0.0;
public:
    Complex() {}                 // Default constructor
    Complex(double r) : re(r) {} // Constructor with initialization list
    Complex(double r, double i) : re(r), im(i) {}
};
```

#### Usage

```cpp
int main() {
    Complex c1;          // Default constructor
    Complex c2(5.0);     // Constructor with one parameter
    Complex c3{7.0, 2.0}; // Constructor with two parameters
}
```

---

### Member Initializer Lists

An initializer list is used to initialize class members directly before entering the body of the constructor. It is particularly useful for:

- `const` members.
- Reference members.
- Avoiding redundant initialization.

```cpp
class Complex {
private:
    const double re; // Must be initialized
    double im;
public:
    Complex(double r, double i) : re(r), im(i) {}
};
```

---

### Aggregate Initialization

If a class has no user-defined constructors, it can be initialized using aggregate initialization.

```cpp
class Complex {
public:
    double re;
    double im;
};

int main() {
    Complex c{1, 2}; // Aggregate initialization
}
```

However, defining any constructor disables aggregate initialization.

---

### Default Constructor

If no constructor is defined, the compiler provides a default constructor. You can explicitly declare one as `= default`.

```cpp
class C {
    int a;
public:
    C() = default; // Explicitly declared
};
```

---

### Destructors

A destructor cleans up resources when an object goes out of scope.

```cpp
class Complex {
public:
    ~Complex() {
        std::cout << "Object destroyed!";
    }
};
```

### Example with Dynamic Memory

```cpp
class String {
private:
    char* arr;
    size_t sz;
public:
    String(size_t n, char c) : sz(n) {
        arr = new char[n + 1];
        std::fill(arr, arr + n, c);
        arr[n] = '\0';
    }
    ~String() {
        delete[] arr;
    }
};
```

---

### Constructor with `std::initializer_list`

A constructor accepting `std::initializer_list` allows initialization with braces.

```cpp
#include <vector>
class String {
public:
    String(std::initializer_list<char> list) {
        for (auto c : list) std::cout << c;
    }
};

int main() {
    String s{'H', 'e', 'l', 'l', 'o'};
}
```
