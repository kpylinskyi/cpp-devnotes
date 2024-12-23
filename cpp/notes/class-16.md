Here’s a more structured, detailed, and well-commented version of your notes, along with additional examples and explanations for better clarity.

---

# **3.6 Operator Overloading in C++**

Operator overloading allows custom behavior for standard operators when used with user-defined types. This can make your classes more intuitive and easier to use.

---

## **Basic Operator Overloading**

Overloading operators inside a class involves defining a member function that implements the desired behavior. Here’s an example with a `Complex` class:

### **Example: Basic Arithmetic Overloading**

```cpp
struct Complex {
    double re = 0.0;
    double im = 0.0;

    Complex(double re) : re(re) {}
    Complex(double re, double im) : re(re), im(im) {}

    // Overload + for Complex + Complex
    Complex operator+(const Complex& other) const {
        return Complex(re + other.re, im + other.im);
    }

    // Overload += for Complex += Complex
    Complex& operator+=(const Complex& other) {
        re += other.re;
        im += other.im;
        return *this; // Return reference to current object
    }
};

int main() {
    Complex c1(1.0);
    Complex c2 = c1 + Complex(2.0, 3.0); // Uses c1.operator+(...)
}
```

---

## **Global vs Member Operator Overloading**

When overloading an operator, consider whether it should be a member function or a global function.

- **Member Function**: The left-hand operand must be the class type.
- **Global Function**: Can handle cases where the left-hand operand is a different type.

### **Example: Fixing Non-Commute Behavior**

```cpp
struct Complex {
    double re = 0.0;
    double im = 0.0;
    Complex(double re) : re(re) {}
    Complex(double re, double im) : re(re), im(im) {}
};

// Global operator for Complex + any numeric type
Complex operator+(const Complex& a, double b) {
    return Complex(a.re + b, a.im);
}

// Global operator for numeric type + Complex
Complex operator+(double a, const Complex& b) {
    return Complex(a + b.re, b.im);
}

int main() {
    Complex c1(1.0);
    Complex c2 = c1 + 3.14; // Works
    Complex c3 = 3.14 + c1; // Also works now
}
```

---

## **Optimizing `+=` Operator**

For compound assignment operators (`+=`, `-=`, etc.), returning `*this` by reference avoids unnecessary copies.

### **Example**

```cpp
struct Complex {
    double re = 0.0;
    double im = 0.0;

    Complex& operator+=(const Complex& other) {
        re += other.re;
        im += other.im;
        return *this; // Return reference to allow chaining
    }
};

int main() {
    Complex c1(1.0);
    c1 += Complex(2.0, 3.0); // Efficient in-place modification
}
```

---

## **Overloading for Lvalue and Rvalue Contexts**

Overloading operators differently for lvalues and rvalues can prevent unintended usage.

### **Example**

```cpp
struct Complex {
    double re = 0.0;
    double im = 0.0;

    // Assignment for lvalue
    Complex& operator=(const Complex& other) & {
        re = other.re;
        im = other.im;
        return *this;
    }

    // Assignment for rvalue
    Complex& operator=(const Complex& other) && {
        re = other.re;
        im = other.im;
        return *this;
    }
};
```

---

## **Stream Operators Overloading**

Stream operators (`<<` and `>>`) are usually overloaded as global functions.

### **Example**

```cpp
#include <iostream>
#include <string>

struct Complex {
    double re = 0.0;
    double im = 0.0;

    Complex(double re, double im) : re(re), im(im) {}
};

// Output Complex numbers
std::ostream& operator<<(std::ostream& out, const Complex& c) {
    return out << "(" << c.re << " + " << c.im << "i)";
}

// Input Complex numbers
std::istream& operator>>(std::istream& in, Complex& c) {
    return in >> c.re >> c.im;
}

int main() {
    Complex c1(0.0, 0.0);
    std::cin >> c1; // Enter values as: 3.5 2.1
    std::cout << "Complex number: " << c1; // Outputs: (3.5 + 2.1i)
}
```

---

## **Comparison Operators**

Comparison operators (`<`, `>`, `==`, etc.) can be overloaded for custom types to allow sorting, equality checks, etc.

### **Example**

```cpp
struct Complex {
    double re = 0.0;
    double im = 0.0;

    bool operator<(const Complex& other) const {
        return re < other.re || (re == other.re && im < other.im);
    }
};

int main() {
    Complex c1(1.0, 2.0);
    Complex c2(2.0, 1.0);
    std::cout << (c1 < c2); // Outputs: 1 (true)
}
```

---

## **Spaceship Operator (`<=>`)**

The three-way comparison operator simplifies comparison logic and automatically generates `>`, `>=`, `<`, and `<=`.

### **Example**

```cpp
#include <compare>

struct Complex {
    double re = 0.0;
    double im = 0.0;

    // Spaceship operator
    std::partial_ordering operator<=>(const Complex& other) const {
        if (auto cmp = re <=> other.re; cmp != 0) return cmp;
        return im <=> other.im;
    }
};

int main() {
    Complex c1(1.0, 2.0), c2(1.0, 3.0);
    auto result = c1 <=> c2; // Compare c1 and c2
}
```

### **Orderings**

- **Weak Ordering**: Some elements are incomparable.
- **Partial Ordering**: May have undefined relationships.
- **Strong Ordering**: Fully defined and consistent comparison.

---

## **Increment and Decrement Operators**

Increment (`++`) and decrement (`--`) operators can be overloaded for both prefix and postfix forms.

### **Example**

```cpp
struct UserId {
    int value = 0;

    // Prefix (++x)
    UserId& operator++() {
        ++value;
        return *this;
    }

    // Postfix (x++)
    UserId operator++(int) {
        UserId temp = *this;
        ++value;
        return temp; // Return copy for the previous state
    }
};

int main() {
    UserId id{5};
    ++id;    // Prefix: increments and returns reference
    id++;    // Postfix: increments and returns previous value
}
```

---

## **Parenthesis Operator**

Overloading the parenthesis operator makes a class callable like a function (functor).

### **Example: Functor**

```cpp
#include <vector>
#include <algorithm>

struct Greater {
    bool operator()(int x, int y) const {
        return x > y;
    }
};

int main() {
    std::vector<int> v{1, 3, 2, 4};
    std::sort(v.begin(), v.end(), Greater()); // Sort in descending order
}
```
---
