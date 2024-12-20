# **3.5 Const, Static, and Explicit Methods**

---

## **Const Qualifier**

The `const` qualifier is a key feature in C++ that ensures immutability in various contexts. It can be used with objects, methods, and pointers to prevent unintended modifications.

### **Calling Non-Const Methods on Const Objects**

A `const` object cannot call non-`const` member functions because doing so might modify the object's state. This ensures that `const`-qualified objects remain immutable.

#### **Example: Compilation Error**

```cpp
struct S {
    void f() {
        std::cout << "Hi!";
    }
};

int main() {
    const S s; 
    s.f(); // Compilation Error: discards qualifiers
}
```

#### **Solution: Mark Method as `const`**

By marking a member function with `const`, it promises not to modify the object's state.

```cpp
struct S {
    void f() const {
        std::cout << "Hi!";
    }
};
```

---

### **Overloading Const and Non-Const Methods**

C++ allows overloading methods based on `const` qualification. This is useful when the same method should behave differently for `const` and non-`const` objects.

#### **Example**

```cpp
struct S {
    void f() {
        std::cout << "non-const!";
    }

    void f() const {
        std::cout << "const!";
    }
};

int main() {
    const S s1; 
    s1.f(); // Outputs: const!
    S s2;
    s2.f(); // Outputs: non-const!
}
```

---

### **Modifying Members with Const**

To modify class members in a `const`-qualified context, you must use `mutable`.

#### **Example: Overloaded Subscript Operator**

```cpp
struct S {
    char arr[10];

    char& operator[](size_t index) { return arr[index]; }               // Non-const version
    const char& operator[](size_t index) const { return arr[index]; }   // Const version
};

int main() {
    S s1;
    s1.arr[5] = 10; // OK: Non-const object can modify the array

    const S s2;
    s2.arr[5] = 10; // Compilation Error: Modifies const object
}
```

---

## **Mutable Qualifier**

The `mutable` keyword allows modification of a class member even if the containing object is `const`. This is useful for caching or reference counting.

#### **Example**

```cpp
struct S {
    mutable int x = 1;

    void increment() const {
        x++;
    }
};

int main() {
    const S s;
    s.increment(); // OK: Modifies mutable member
    std::cout << s.x; // Outputs: 2
}
```

---

## **Static Methods**

Static methods belong to the class rather than any specific instance. They do not have access to non-static members of the class.

### **Key Points**

- Declared with the `static` keyword.
- Can be called using the class name without creating an instance.
- Cannot access non-static members.

#### **Example**

```cpp
struct S {
    static int x;

    static void f() {
        std::cout << "Static Hi!";
    }
};

int S::x = 1;

int main() {
    std::cout << S::x; // Outputs: 1
    S::f();           // Outputs: Static Hi!
}
```

---

### **Static Members with Singleton Pattern**

The Singleton pattern ensures a class has only one instance. Static methods and members are key to implementing this pattern.

#### **Example**

```cpp
class Singleton {
private:
    Singleton() {}
    static Singleton* ptr;

    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    static Singleton& getInstance() {
        if (!ptr) ptr = new Singleton();
        return *ptr;
    }
};

Singleton* Singleton::ptr = nullptr;
```

---

## **Explicit Qualifier**

The `explicit` qualifier prevents implicit conversions or constructor calls. It is crucial for ensuring that constructors and conversion operators are only used intentionally.

### **Implicit Conversion Issue**

Without `explicit`, a constructor that takes one argument can be used for implicit conversions, which can lead to unintended behavior.

#### **Example**

```cpp
struct Latitude {
    double value;
    Latitude(double value) : value(value) {}
};

struct Longitude {
    double value;
    Longitude(double value) : value(value) {}
};

struct Coordinate {
    Latitude latitude;
    Longitude longitude;
    Coordinate(Latitude latitude, Longitude longitude)
        : latitude(latitude), longitude(longitude) {}
};

int main() {
    Coordinate(5.0, 3.0); // Implicit conversion, works but breaks logic
}
```

#### **Solution: Use `explicit`**

```cpp
struct Latitude {
    double value;
    explicit Latitude(double value) : value(value) {}
};

struct Longitude {
    double value;
    explicit Longitude(double value) : value(value) {}
};

struct Coordinate {
    Latitude latitude;
    Longitude longitude;
    Coordinate(Latitude latitude, Longitude longitude)
        : latitude(latitude), longitude(longitude) {}
};

int main() {
    Coordinate(Latitude(5.0), Longitude(3.0)); // Must explicitly construct Latitude and Longitude
}
```

---

### **Conversions with Explicit**

`explicit` can also be applied to conversion operators to prevent implicit conversions.

#### **Example**

```cpp
struct Latitude {
    double value;
    explicit Latitude(double value) : value(value) {}

    explicit operator double() const {
        return value;
    }
};

int main() {
    Latitude lat(45.0);
    double d = static_cast<double>(lat); // Explicit cast required
}
```

---

## **Custom Literal Suffixes**

Custom literal suffixes allow defining user-defined literals for custom types.

#### **Example**

```cpp
class BigInteger {};

BigInteger operator""_bi(unsigned long long x) {
    return BigInteger();
}

int main() {
    BigInteger bi = 1_bi; // User-defined literal
}
```

---

## **Enhanced Coordinate Example with Literals**

Combining `explicit` constructors and custom literal suffixes creates a robust and intuitive interface.

#### **Example**

```cpp
struct Latitude {
    double value;
    explicit Latitude(double value) : value(value) {}
};

Latitude operator""_lt(long double val) { return Latitude(val); }

struct Longitude {
    double value;
    explicit Longitude(double value) : value(value) {}
};

Longitude operator""_lg(long double val) { return Longitude(val); }

struct Coordinate {
    Latitude latitude;
    Longitude longitude;
    Coordinate(Latitude latitude, Longitude longitude)
        : latitude(latitude), longitude(longitude) {}
};

int main() {
    Coordinate coord(5.0_lt, 3.0_lg); // Use custom literals
}
```

---
