## Declarations, Definitions, and Scopes

A program is a sequence of declarations and definitions that determine how variables, functions, classes, and other entities are used and manipulated within various scopes.

### Basic Example

```cpp
#include <iostream>

int a = 5; // Global scope: 'a' is declared and defined globally.

void f(int x); // Function declaration: the definition may be elsewhere.

class C; // Class declaration: the definition may be elsewhere.
struct S; // Struct declaration: the definition may be elsewhere.
enum E; // Enum declaration: the definition may be elsewhere.
union U; // Union declaration: the definition may be elsewhere.

namespace N {
    int x; // Namespace scope: 'x' is declared and defined within namespace N.
}

int main() {
    // Local scope: inside main function
    a = 10; // Access and modify the global variable 'a'.

    // Uncommenting the following line will cause a compilation error because 'x' is not declared in this scope.
    // x = 55;

    N::x = 110; // Access and modify the variable 'x' within namespace N.

    using N::x; // Introduce 'N::x' into the current scope.
    x = 55; // Now, 'x' refers to 'N::x', not an undeclared variable.

    std::cout << "a: " << a << ", N::x: " << N::x << std::endl;

    return 0;
}
```

**Explanation**:
- `x` is an unqualified-id.
- `N::x` is a qualified-id.

### `using`
The `using` keyword introduces names from a namespace into the current declarative region.

```cpp
#include <iostream>

namespace N {
    int a = 1;
    int b = 2;
}

int main() {
    // Access members of namespace N using qualified names.
    std::cout << "N::a: " << N::a << ", N::b: " << N::b << std::endl;

    // Introduce 'N::a' into the current scope.
    using N::a;
    std::cout << "a: " << a << std::endl; // Now, 'a' refers to 'N::a'.

    // Introduce 'N::b' into the current scope.
    using N::b;
    std::cout << "b: " << b << std::endl; // Now, 'b' refers to 'N::b'.

    // Introduce all names from namespace N into the current scope.
    using namespace N;
    std::cout << "a: " << a << ", b: " << b << std::endl;

    // Using 'std::cout' from the standard namespace.
    using std::cout;
    cout << "Hi!" << std::endl;

    return 0;
}
```

### Typename Alias
Typename aliases make code more readable and easier to maintain.

```cpp
#include <vector>

using vi = std::vector<int>; // Create an alias for 'std::vector<int>'.

template <typename T>
using vt = std::vector<T>; // Create a template alias for 'std::vector<T>'.

typedef long long ll; // Legacy way to create an alias for 'long long'.

int main() {
    vi myVector = {1, 2, 3}; // Using the alias 'vi' for 'std::vector<int>'.
    vt<float> myFloatVector = {1.1f, 2.2f, 3.3f}; // Using the template alias 'vt' for 'std::vector<float>'.

    ll myLongLong = 123456789LL; // Using the alias 'll' for 'long long'.

    return 0;
}
```

### Name Conflicts
Name conflicts can occur when different entities have the same name in overlapping scopes.

```cpp
#include <iostream>

int x = 0; // Global 'x'

int main() {
    int x = 1; // 'x' in main scope, hides global 'x'
    std::cout << "main scope x: " << x << std::endl; // Prints 1

    for (int i = 0; i < 3; ++i) {
        int x = i; // 'x' in for-loop scope, hides main scope 'x'
        std::cout << "for-loop scope x: " << x << std::endl; // Prints 0, 1, 2
        std::cout << "global x: " << ::x << std::endl; // Prints 0
    }

    std::cout << "main scope x after loop: " << x << std::endl; // Prints 1

    return 0;
}
```

```cpp
#include <vector>
#include <iostream>

namespace N {
    int x = 10;
}

int main() {
    using N::x;
    {
        using x = std::vector<int>; // 'x' is redeclared as a different kind of entity (std::vector<int>).
        x vec = {1, 2, 3}; // Use the local 'x' (std::vector<int>).
        std::cout << "vec: ";
        for (const auto& elem : vec) {
            std::cout << elem << " ";
        }
        std::cout << std::endl;
    }
    // Outside the block, 'x' refers to 'N::x' again.
    std::cout << "N::x: " << x << std::endl; // Prints 10

    return 0;
}
```

```cpp
#include <iostream>

int x = 0; // Global 'x'

int main() {
    int x = x; // 'x' is initialized by itself, not by global 'x'.
    // This results in undefined behavior as local 'x' is used uninitialized.

    std::cout << "Uninitialized local x: " << x << std::endl; // Undefined behavior

    return 0;
}
```

### ODR (One Definition Rule)
The One Definition Rule (ODR) states that every program entity should be defined exactly once. However, declarations can appear multiple times as long as they are identical.

```cpp
void f(int x);
void f(int x); // OK: multiple declarations

void f(int x) {} // Definition
// void f(int x) {} // Error: redefinition

int main() {
    f(5);
    return 0;
}
```

### Cross Recursion
Cross recursion occurs when two or more functions call each other.

```cpp
#include <iostream>

void f(); // Forward declaration of 'f'

void g() {
    std::cout << "In g()" << std::endl;
    f(); // Call 'f' from 'g'
}

void f() {
    std::cout << "In f()" << std::endl;
    g(); // Call 'g' from 'f'
}

int main() {
    // Uncommenting the following line will cause infinite recursion and stack overflow.
    // f();
    return 0;
}
```

### Function Overloading
Function signatures are determined by the function name and argument types. Functions with the same name but different argument types are considered different functions.

```cpp
#include <iostream>

int f(int x) { return x + 1; }
int f(float x) { return x + 2; }

int main() {
    std::cout << f(1) << std::endl;     // Calls f(int)
    std::cout << f(1.0f) << std::endl;  // Calls f(float)
    // std::cout << f(1.0) << std::endl; // Error: ambiguous call (double to int or float)

    return 0;
}
```

### Common Errors

- **Variable not declared in scope**:
  - Occurs when a variable is used without being declared in the current or accessible scope.
  ```cpp
  int main() {
      x = 10; // Error: 'x' was not declared in this scope
      return 0;
  }
  ```

- **Redeclaration of a different kind**:
  - Happens when an entity is declared again in the same scope but as a different kind (e.g., a variable vs. a type).
  ```cpp
  #include <vector>

  namespace N {
      int x;
  }

  int main() {
      using N::x;
      {
          using x = std::vector<int>; // Error: 'x' redeclared as different kind of entity
      }
      return 0;
  }
  ```

- **Function redefinition**:
  - Happens when a function is defined more than once.
  ```cpp
  void f(int x) {}
  void f(int x) {} // Error: redefinition of 'void f(int)'

  int main() {
      f(5);
      return 0;
  }
  ```

- **Call of overloaded function is ambiguous**:
  - Occurs when a call could match more than one overloaded function equally well.
  ```cpp
  #include <iostream>

  int f(int x) { return x + 1; }
  int f(float x) { return x + 2; }

  int main() {
      // std::cout << f(1.0) << std::endl; // Error: ambiguous call (double to int or float)

      return 0;
  }
  ```

