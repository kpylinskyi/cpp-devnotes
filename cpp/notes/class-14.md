# Destructor, Copy Constructor, Assignment Operator, and the Rule of Three

---

## **Destructor**

A destructor is a special member function automatically invoked when an object goes out of scope or is explicitly deleted. It is used to clean up resources, such as dynamic memory, acquired by the object during its lifetime.

### **Key Points**

- A destructor is declared with a tilde `~` followed by the class name.
- If no destructor is defined, the compiler generates a default one.
- A destructor cannot take arguments or be overloaded.
- Trivial destructors may be skipped by the compiler if they are unnecessary for resource cleanup.

### **Example: Resource Cleanup**

```cpp
class String {
private:
    char* arr;     // Dynamically allocated array
    size_t sz;     // Size of the array
public:
    // Constructor
    String(size_t n, char c) : sz(n) {
        arr = new char[n + 1];
        std::fill(arr, arr + n, c);  // Fill array with character `c`
        arr[n] = '\0';              // Null-terminate the string
    }

    // Destructor
    ~String() {
        delete[] arr; // Free dynamically allocated memory
        std::cout << "Destructor called\n";
    }
};
```

---

## **Copy Constructor**

The copy constructor is used to create a new object as a copy of an existing object. It performs a **deep copy** to ensure each object has its own copy of dynamically allocated resources.

### **Key Points**

- If not explicitly defined, the compiler generates a default **shallow copy constructor**.
- A deep copy is necessary when managing dynamic memory to avoid double-deletion issues.

### **Example: Custom Copy Constructor**

```cpp
class String {
private:
    char* arr;
    size_t sz;

public:
    // Constructor
    String(size_t n, char c) : sz(n) {
        arr = new char[n + 1];
        std::fill(arr, arr + n, c);
        arr[n] = '\0';
    }

    // Copy Constructor
    String(const String& other) : sz(other.sz) {
        arr = new char[sz + 1];  // Allocate new memory
        std::copy(other.arr, other.arr + sz + 1, arr);  // Deep copy
        std::cout << "Copy Constructor called\n";
    }

    // Destructor
    ~String() {
        delete[] arr;
    }

    void display() const {
        std::cout << "String: " << arr << "\n";
    }
};

int main() {
    String s1(5, 'a');  // Create object
    String s2 = s1;     // Copy constructor is called
    s2.display();       // Outputs: String: aaaaa
}
```

---

## **Copy-On-Write (COW) Idiom**

COW delays copying until the copied object is modified. This saves memory and improves performance when multiple objects share the same data.

### **COW Example**

```cpp
#include <cstring>
#include <iostream>

class String {
private:
    char* arr;
    size_t sz;
    size_t* ref_count;  // Reference count

public:
    // Constructor
    String(const char* str) : sz(strlen(str)), ref_count(new size_t(1)) {
        arr = new char[sz + 1];
        strcpy(arr, str);
    }

    // Copy Constructor (COW)
    String(const String& other) : arr(other.arr), sz(other.sz), ref_count(other.ref_count) {
        (*ref_count)++;
    }

    // Destructor
    ~String() {
        if (--(*ref_count) == 0) {
            delete[] arr;
            delete ref_count;
        }
    }

    // Modify (break sharing)
    void modify(size_t index, char c) {
        if (*ref_count > 1) {
            char* new_arr = new char[sz + 1];
            strcpy(new_arr, arr);
            --(*ref_count);
            ref_count = new size_t(1);
            arr = new_arr;
        }
        arr[index] = c;
    }

    void display() const {
        std::cout << "String: " << arr << ", Ref Count: " << *ref_count << "\n";
    }
};

int main() {
    String s1("hello");
    String s2 = s1;  // Share the same memory
    s1.display();    // Outputs: String: hello, Ref Count: 2
    s2.display();    // Outputs: String: hello, Ref Count: 2

    s1.modify(0, 'H');  // Break sharing
    s1.display();       // Outputs: String: Hello, Ref Count: 1
    s2.display();       // Outputs: String: hello, Ref Count: 1
}
```

---

## **Delegating Constructor**

A delegating constructor allows one constructor to call another within the same class, reducing code duplication.

### **Example**

```cpp
class String {
private:
    char* arr;
    size_t sz;

    // Base Constructor
    String(size_t n) : arr(new char[n + 1]), sz(n) {
        arr[sz] = '\0';
    }

public:
    // Constructor with fill character
    String(size_t n, char c) : String(n) {
        std::fill(arr, arr + n, c);
    }

    // Constructor with initializer list
    String(std::initializer_list<char> list) : String(list.size()) {
        std::copy(list.begin(), list.end(), arr);
    }

    ~String() {
        delete[] arr;
    }

    void display() const {
        std::cout << "String: " << arr << "\n";
    }
};

int main() {
    String s1(5, 'a');  // Delegating constructor
    s1.display();       // Outputs: String: aaaaa

    String s2{'h', 'e', 'l', 'l', 'o'};  // Delegating initializer list
    s2.display();       // Outputs: String: hello
}
```

---

## **Rule of Three**

The **Rule of Three** states that if a class defines one of the following:

- A **Destructor**,
- A **Copy Constructor**, or
- An **Assignment Operator**,

It likely needs to define all three to properly manage resources.

---

## **Assignment Operator**

The assignment operator allows one object to be assigned the values of another. Like the copy constructor, it must perform a **deep copy** if dynamic memory is involved.

### **Example: Copy-and-Swap Idiom**

The copy-and-swap idiom is a safe and efficient way to implement the assignment operator.

```cpp
class String {
private:
    char* arr;
    size_t sz;

    void swap(String& other) {
        std::swap(arr, other.arr);
        std::swap(sz, other.sz);
    }

public:
    String(size_t n, char c) : arr(new char[n + 1]), sz(n) {
        std::fill(arr, arr + n, c);
        arr[n] = '\0';
    }

    String(const String& other) : arr(new char[other.sz + 1]), sz(other.sz) {
        std::copy(other.arr, other.arr + sz + 1, arr);
    }

    String& operator=(String other) {
        swap(other);  // Reuse copy constructor and destructor
        return *this;
    }

    ~String() {
        delete[] arr;
    }

    void display() const {
        std::cout << "String: " << arr << "\n";
    }
};

int main() {
    String s1(5, 'x');
    String s2(3, 'y');

    s1 = s2;  // Assignment operator
    s1.display();  // Outputs: String: yyy
}
```

This implementation is exception-safe and reduces code duplication by reusing the copy constructor and destructor.