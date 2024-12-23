# **3.7 Pointers to Members**

Pointers to members in C++ allow you to point to a specific member (field or method) of a class, rather than an instance of the class. This can be particularly useful when working with dynamic or generic programming techniques.

---

## **Pointer to Class Field**

A pointer to a class field is declared using the syntax:

```cpp
<type> <class>::* <pointer_name>;
```

### **Key Characteristics:**

1. The pointer stores the offset of a member in the class layout, not the actual value.
2. To access the field, you need an object of the class and dereference the pointer with `.*` or `->*`.

### **Example: Pointer to Field**

```cpp
#include <iostream>
struct S {
    int x;
    double y;
};

int main() {
    // Declare a pointer to an integer member of S
    int S::* p = &S::x;

    // Create an instance of S
    S s{10, 3.14};

    // Access member using the pointer
    std::cout << "Value of s.x: " << s.*p << std::endl;

    // Modify the value of s.x using the pointer
    s.*p = 20;
    std::cout << "Modified value of s.x: " << s.*p << std::endl;

    // Access via a pointer to S
    S* ps = &s;
    std::cout << "Value via ps->*p: " << ps->*p << std::endl;

    return 0;
}
```

**Output:**

```
Value of s.x: 10
Modified value of s.x: 20
Value via ps->*p: 20
```

---

## **Pointer to Method**

A pointer to a method stores the address of a member function of a class. It is declared using the syntax:

```cpp
<return_type> (<class>::* <pointer_name>)(<parameter_types>);
```

### **Key Characteristics:**

1. It requires an instance of the class to invoke the method.
2. Use `.*` for accessing with an object and `->*` for accessing with a pointer to the object.

### **Example: Pointer to Method**

```cpp
#include <iostream>
struct S {
    int x;

    // Member function
    void f(int z) {
        std::cout << "x + z = " << x + z << std::endl;
    }
};

int main() {
    S s{10};

    // Declare a pointer to method
    void (S::* pf)(int) = &S::f;

    // Call the function using an object
    (s.*pf)(5); // Output: x + z = 15

    // Call the function using a pointer to the object
    S* ps = &s;
    (ps->*pf)(8); // Output: x + z = 18

    return 0;
}
```

---

## **Using Pointers to Members with Arrays**

Pointers to members are useful when working with arrays of objects or in scenarios requiring dynamic member access.

### **Example: Pointer to Field in an Array of Objects**

```cpp
#include <iostream>
#include <vector>
struct S {
    int x;
    double y;
};

int main() {
    // Create a vector of S objects
    std::vector<S> vec{{1, 1.1}, {2, 2.2}, {3, 3.3}};

    // Pointer to an integer member
    int S::* px = &S::x;

    for (const S& obj : vec) {
        std::cout << "x: " << obj.*px << std::endl;
    }

    return 0;
}
```

**Output:**

```
x: 1
x: 2
x: 3
```

---

## **Combining Pointer to Field and Pointer to Method**

Pointers to fields and methods can be combined to perform more dynamic operations.

### **Example: Dynamically Setting and Calling Members**

```cpp
#include <iostream>
struct S {
    int x;
    void print() {
        std::cout << "Value of x: " << x << std::endl;
    }
};

int main() {
    S s{42};

    // Pointer to field
    int S::* px = &S::x;

    // Modify x dynamically
    s.*px = 100;
    std::cout << "Modified value: " << s.*px << std::endl;

    // Pointer to method
    void (S::* pf)() = &S::print;

    // Call method dynamically
    (s.*pf)();

    return 0;
}
```

**Output:**

```
Modified value: 100
Value of x: 100
```

---

## **Pointers to Const Members**

Pointers can also be used to point to `const` fields or methods. This is especially useful when working with constant objects.

### **Example**

```cpp
#include <iostream>
struct S {
    int x;
    void f() const {
        std::cout << "Const method: x = " << x << std::endl;
    }
};

int main() {
    const S s{10};

    // Pointer to const method
    void (S::* pf)() const = &S::f;

    // Call const method
    (s.*pf)();

    return 0;
}
```

**Output:**

```
Const method: x = 10
```

---

## **Practical Use Cases**

1. **Dynamic Dispatch Without Virtual Functions**:
    
    - Useful when virtual functions aren't an option, but you still need dynamic member access.
2. **Generic Frameworks**:
    
    - Libraries or frameworks can store pointers to members for later use.
3. **Event Systems**:
    
    - Function pointers or member pointers can be used in callback mechanisms.

---

## **Advanced: Storing Pointers to Members in Containers**

Pointers to members can be stored in containers for flexible access.

### **Example**

```cpp
#include <iostream>
#include <vector>
struct S {
    int x;
    double y;

    void printX() const {
        std::cout << "x = " << x << std::endl;
    }

    void printY() const {
        std::cout << "y = " << y << std::endl;
    }
};

int main() {
    S s{10, 3.14};

    // Vector of pointers to methods
    std::vector<void (S::*)() const> methods = {&S::printX, &S::printY};

    for (auto method : methods) {
        (s.*method)();
    }

    return 0;
}
```

**Output:**

```
x = 10
y = 3.14
```

---

Here’s a detailed explanation of enums and enum classes in C++, complete with additional examples, guidelines, and practical tips:

---

# **3.8 Enums and Enum Classes**

## **Basic Enums**

A basic enum defines a list of constants that are implicitly assigned integral values, starting from `0` by default.

### **Example: Basic Enum**

```cpp
#include <iostream>

enum Color {
    White, // 0
    Gray,  // 1
    Black  // 2
};

int main() {
    Color c = White;

    // Output enum value
    std::cout << "Enum value: " << c << std::endl;

    // Implicit conversion to int
    int colorValue = c; 
    std::cout << "As integer: " << colorValue << std::endl;

    return 0;
}
```

**Output:**

```
Enum value: 0
As integer: 0
```

---

## **Limitations of Basic Enums**

1. **Implicit Conversion to Integers:**
    - Basic enums can be implicitly converted to integers, leading to unintended behavior.
    - Example:
        
        ```cpp
        Color c = White;
        c = static_cast<Color>(3); // Invalid value, but allowed
        ```
        
2. **Global Scope Pollution:**
    - Enum constants are introduced into the enclosing scope.
    - Example:
        
        ```cpp
        enum Color { White, Gray, Black };
        enum Status { Gray, Error }; // Name conflict with Color::Gray
        ```
        

---

## **Enum Classes**

**Enum classes** (also called **scoped enums**) solve the issues associated with basic enums:

- They do not allow implicit conversions to integers.
- Their constants are scoped to the enum type.

### **Syntax**

```cpp
enum class EnumName {
    Constant1,
    Constant2,
    ...
};
```

### **Example: Enum Class**

```cpp
#include <iostream>

enum class Color {
    White,
    Gray,
    Black
};

int main() {
    Color c = Color::White;

    // Enum class requires qualified names
    std::cout << "Enum value: " << static_cast<int>(c) << std::endl;

    // c = 1; // Error: Implicit conversion not allowed

    return 0;
}
```

**Output:**

```
Enum value: 0
```

---

## **Casting Enum Class to Integer**

Enum classes restrict implicit casting, but you can explicitly cast them to integers using `static_cast`.

### **Example**

```cpp
#include <iostream>

enum class Color {
    White = 10,
    Gray = 20,
    Black = 30
};

int main() {
    Color c = Color::Gray;

    // Explicit cast to int
    std::cout << "Enum value: " << static_cast<int>(c) << std::endl;

    return 0;
}
```

**Output:**

```
Enum value: 20
```

---

## **Typed Enums**

C++ allows you to specify the underlying type of an enum or enum class. This is particularly useful for controlling memory usage or ensuring compatibility with specific APIs.

### **Syntax**

```cpp
enum class EnumName : UnderlyingType {
    Constant1,
    Constant2,
    ...
};
```

### **Example: Typed Enum Class**

```cpp
#include <iostream>
#include <cstdint>

enum class Color : uint8_t {
    White = 1,
    Gray = 2,
    Black = 3
};

int main() {
    Color c = Color::Black;

    // Explicit cast to underlying type
    std::cout << "Enum value: " << static_cast<uint8_t>(c) << std::endl;

    return 0;
}
```

**Output:**

```
Enum value: 3
```

---

## **Practical Usage of Enum Classes**

### **Enum as Flags**

Enums can represent bit flags for options or states.

```cpp
#include <iostream>
enum class Permission : uint8_t {
    Read = 1 << 0,  // 0b0001
    Write = 1 << 1, // 0b0010
    Execute = 1 << 2 // 0b0100
};

inline Permission operator|(Permission a, Permission b) {
    return static_cast<Permission>(static_cast<uint8_t>(a) | static_cast<uint8_t>(b));
}

inline Permission operator&(Permission a, Permission b) {
    return static_cast<Permission>(static_cast<uint8_t>(a) & static_cast<uint8_t>(b));
}

int main() {
    Permission p = Permission::Read | Permission::Write;

    if ((p & Permission::Read) != static_cast<Permission>(0)) {
        std::cout << "Read permission granted." << std::endl;
    }

    if ((p & Permission::Execute) == static_cast<Permission>(0)) {
        std::cout << "Execute permission denied." << std::endl;
    }

    return 0;
}
```

**Output:**

```
Read permission granted.
Execute permission denied.
```

---

## **Comparing Enums**

Enum constants can be compared using relational operators.

### **Example: Comparing Enum Class Values**

```cpp
#include <iostream>
enum class Priority {
    Low,
    Medium,
    High
};

int main() {
    Priority p1 = Priority::Low;
    Priority p2 = Priority::High;

    if (p1 < p2) {
        std::cout << "Priority p1 is lower than p2." << std::endl;
    }

    return 0;
}
```

**Output:**

```
Priority p1 is lower than p2.
```

---

## **Best Practices for Using Enums**

1. **Prefer `enum class` over basic enums** to avoid implicit conversions and scope pollution.
2. **Specify the underlying type** of enums when memory usage or API compatibility matters.
3. Use **explicit casting** when working with enum classes and integers.
4. Leverage **enums as bit flags** for representing combinations of states or options.

---

## **Advanced: Iterating Over Enum Constants**

Since enums don’t inherently support iteration, you need custom techniques to iterate over their values.

### **Example: Iteration Using a Helper Function**

```cpp
#include <iostream>
#include <array>

enum class Color {
    White,
    Gray,
    Black
};

constexpr std::array<Color, 3> allColors = {Color::White, Color::Gray, Color::Black};

int main() {
    for (Color c : allColors) {
        std::cout << static_cast<int>(c) << std::endl;
    }
    return 0;
}
```

**Output:**

```
0
1
2
```

---


# **IV. Inheritance**

Inheritance is a key feature of object-oriented programming in C++ that allows a class to acquire the properties and behavior of another class. It helps in code reuse and establishing a hierarchy between classes.

---

## **4.1 Public, Private, and Protected Inheritance**

### **Basic Syntax**

```cpp
struct Base {
    int x;
    void f() {}
};

struct Derived : Base {
    int y;
    void g() {}
};

int main() {
    Derived d;
    d.f(); // Accessible if inheritance is public
}
```

### **Types of Inheritance**

1. **Public Inheritance**:
    
    - Public members of the base class remain public in the derived class.
    - Protected members remain protected.
    - Private members are inaccessible directly from the derived class but can be accessed via public/protected member functions.
2. **Protected Inheritance**:
    
    - Public and protected members of the base class become protected in the derived class.
    - Private members remain inaccessible.
3. **Private Inheritance**:
    
    - Public and protected members of the base class become private in the derived class.
    - Private members remain inaccessible.

---

### **Access Qualifiers and Visibility**

The visibility of members in the derived class depends on the inheritance type.

#### **Base Class Access Specifiers**

- **`private:`** Only accessible within the base class itself.
- **`protected:`** Accessible within the base class and its derived classes.
- **`public:`** Accessible from outside the class, as well as by derived classes.

#### **Inheritance Table**

|Base Class Member|Public Inheritance|Protected Inheritance|Private Inheritance|
|---|---|---|---|
|**Private**|Inaccessible|Inaccessible|Inaccessible|
|**Protected**|Protected|Protected|Private|
|**Public**|Public|Protected|Private|

---

### **Example with Access Specifiers**

```cpp
class Base {
private:
    int privateVar = 1;  // Not accessible by derived classes
protected:
    int protectedVar = 2; // Accessible by derived classes
public:
    int publicVar = 3;    // Accessible by everyone
    void baseFunc() { std::cout << "Base Function\n"; }
};

class Derived : public Base {
public:
    void derivedFunc() {
        // privateVar; // Error: Inaccessible
        std::cout << "Protected Var: " << protectedVar << std::endl; // Accessible
        std::cout << "Public Var: " << publicVar << std::endl;       // Accessible
    }
};

int main() {
    Derived d;
    // d.privateVar;  // Error: Inaccessible
    // d.protectedVar; // Error: Inaccessible
    std::cout << "Public Var: " << d.publicVar << std::endl; // Accessible
    d.baseFunc(); // Accessible
    d.derivedFunc();
}
```

**Output:**

```
Protected Var: 2
Public Var: 3
Public Var: 3
Base Function
```

---

## **Friendship in Inheritance**

Friendship relationships do not transfer through inheritance.

### **Example: Friendship in a Multi-level Inheritance**

```cpp
class Granny {
public:
    friend int main(); // Allows main to access Granny's private members
    int x = 10;

protected:
    void f() { std::cout << "Granny f(): " << x << std::endl; }
};

class Mom : protected Granny {
public:
    int y = 20;

    void g() {
        // Access Granny's protected member
        f();
        std::cout << "Mom g(): " << x << std::endl; // x is accessible as protected
    }
};

class Son : public Mom {
public:
    int z = 30;

    void h() {
        // Access Granny's protected member
        f();
        std::cout << "Son h(): " << x << std::endl; // x is accessible as protected
    }
};

int main() {
    Son s;
    // s.f(); // Error: f() is protected
    // std::cout << s.x; // Error: x is protected
    s.h(); // Works, calls h() in Son
}
```

**Output:**

```
Granny f(): 10
Son h(): 10
```

---

### **Protected Access Across Generations**

Protected members of the base class can be accessed in any derived class, but they remain inaccessible to external code.

### **Example**

```cpp
class A {
protected:
    int x = 10;
};

class B : public A {
public:
    void show() {
        std::cout << "x in B: " << x << std::endl;
    }
};

class C : public B {
public:
    void show() {
        std::cout << "x in C: " << x << std::endl;
    }
};

int main() {
    C obj;
    obj.show();
}
```

**Output:**

```
x in C: 10
```

---

## **Key Points to Remember**

1. **Private Members:** Never inherited directly but can be accessed through public or protected functions of the base class.
2. **Protected Members:** Inherited as protected or private based on the inheritance type.
3. **Friendship:** Friend access does not extend to derived classes unless explicitly redefined.

---

# **4.2 Visibility in Inheritance**

Visibility in inheritance refers to how member functions or variables in a base class are accessed or hidden in derived classes. A derived class can override or hide base class members, which affects how they are called.

---

## **Hiding Base Class Members**

When a derived class defines a member (function or variable) with the same name as one in the base class, the base class member is "hidden" in the derived class.

### **Example: Function Hiding**

```cpp
struct Base {
    int x;
    void f() { std::cout << 1; }
};

struct Derived : Base {
    int y;
    void f() { std::cout << 2; }
};

int main() {
    Derived d;
    d.f(); // 2
}
```

- **Output:** `2`
- **Explanation:**
    - The `f()` in `Derived` hides the `f()` in `Base`.
    - Even though `Base::f()` exists, it is not called directly when invoking `d.f()`.

---

### **Explicitly Calling Base Class Methods**

To call the hidden method from the base class, use the **scope resolution operator (`::`)**:

```cpp
int main() {
    Derived d;
    d.f();           // Calls Derived::f() -> 2
    d.Base::f();     // Calls Base::f() -> 1
}
```

---

## **Hiding Functions with Different Signatures**

If a derived class defines a function with the same name but a different signature, the base class function is still hidden. This occurs even if the parameter types are different.

### **Example: Overloading vs. Hiding**

```cpp
struct Base {
    void f(int) { std::cout << 1; }
};

struct Derived : Base {
    void f(double) { std::cout << 2; }
};

int main() {
    Derived d;
    d.f(1);  // Conversion to double -> 2
    d.f(1.0); // Calls Derived::f(double) -> 2
}
```

- **Output:**
    
    ```
    2
    2
    ```
    
- **Explanation:**
    - `Derived::f(double)` hides `Base::f(int)`.
    - When `d.f(1)` is called, the integer is converted to a `double` to match `Derived::f(double)`.
    - To call the base class method explicitly:
        
        ```cpp
        d.Base::f(1); // Calls Base::f(int) -> 1
        ```
        

---

### **Best Practice: Using `using` to Unhide Base Methods**

If you want to retain access to base class functions while adding new overloads in the derived class, use the **`using` directive**.

#### **Example**

```cpp
struct Base {
    void f(int) { std::cout << 1; }
};

struct Derived : Base {
    using Base::f; // Unhide Base::f(int)
    void f(double) { std::cout << 2; }
};

int main() {
    Derived d;
    d.f(1);    // Calls Base::f(int) -> 1
    d.f(1.0);  // Calls Derived::f(double) -> 2
}
```

- **Output:**
    
    ```
    1
    2
    ```
    
- **Explanation:**
    - `using Base::f;` makes `Base::f(int)` accessible in the derived class.
    - `Derived` now has two overloads: `f(int)` from `Base` and `f(double)` defined in `Derived`.

---

## **Key Points**

1. **Hiding vs. Overriding**:
    
    - Overriding occurs when a derived class method has the same signature as a base class method.
    - Hiding occurs when a derived class method has the same name but a different signature.
2. **Accessing Hidden Members**:
    
    - Use the scope resolution operator (`Base::`) to explicitly call base class members that are hidden.
3. **`using` to Retain Access**:
    
    - The `using` directive can make hidden base class functions visible in the derived class.
4. **Avoid Ambiguity**:
    
    - Function hiding and implicit conversions can lead to unexpected behavior. Use `using` or explicit calls to ensure clarity.

By understanding these nuances, you can avoid common pitfalls when working with inheritance in C++.