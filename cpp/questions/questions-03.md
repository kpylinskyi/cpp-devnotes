Sure, I'll review and correct your answers:

1. **What is the difference between a global variable and a local variable in terms of scope?**
   - **Correction**: 
     - **Local Variable**: A variable declared within a function or a block (enclosed in curly braces `{}`) and can only be accessed within that function or block. It has a scope limited to the block in which it is declared.
     - **Global Variable**: A variable declared outside of all functions, usually at the top of a file. It can be accessed from any function within the same translation unit (source file) or even across different files if declared as `extern`.

2. **How do you declare a function without defining it immediately?**
   - **Correction**: You declare a function by writing its signature (prototype) without providing the body. For example:
     ```cpp
     void myFunction(int, double); // Function declaration
     ```

3. **What is the purpose of the `using` keyword in C++?**
   - **Correction**: The `using` keyword serves two main purposes:
     - It allows you to create an alias for a type or namespace, making code easier to read and write. For example:
       ```cpp
       using IntPtr = int*; // Creates an alias for int*
       ```
     - It introduces names from a namespace into the current scope, so you don’t need to prefix them with the namespace name. For example:
       ```cpp
       using namespace std;
       ```

4. **What does the `typedef` keyword do, and how does it differ from `using`?**
   - **Correction**: The `typedef` keyword creates an alias for a type. For example:
     ```cpp
     typedef unsigned long ulong;
     ```
     - The `using` keyword, introduced in C++11, also creates type aliases but is often preferred for its clearer syntax and additional capabilities, such as template aliasing. For example:
       ```cpp
       using ulong = unsigned long;
       ```

5. **How does the One Definition Rule (ODR) affect function declarations and definitions?**
   - **Correction**: The One Definition Rule (ODR) states that in a program, each function or object must be defined exactly once. However, multiple declarations of the same function or object are allowed as long as they are consistent. For example, functions can be declared in header files but must be defined only once in a source file.

6. **What is cross recursion, and how does it impact program execution?**
   - **Correction**: Cross recursion (or mutual recursion) occurs when two or more functions call each other. It can be useful but may impact performance and readability. For example:

```cpp
	void funcB(); // need to be declared before call
     void funcA() {
         funcB(); // Calls funcB
     }

     void funcB() {
         funcA(); // Calls funcA
     }
     ```

7. **Explain the concept of function overloading and provide an example.**
   - **Correction**: Function overloading allows multiple functions with the same name but different parameter lists (different number or types of parameters) to coexist. The correct function is chosen based on the argument list. For example:
     ```cpp
     void print(int i);
     void print(double d);
     void print(const std::string& s);
     ```

8. **What is the role of namespaces in C++ and how do they prevent name conflicts?**
   - **Correction**: Namespaces group related names (e.g., functions, variables) to avoid naming conflicts and improve code organization. You can access names in a namespace using the `namespace::name` syntax. For example:
     ```cpp
     namespace MyNamespace {
         void myFunction();
     }

     MyNamespace::myFunction(); // Calls the function from MyNamespace
     ```

9. **How can you resolve a name conflict in C++?**
   - **Correction**: You can resolve name conflicts by:
     - Using namespaces to encapsulate and disambiguate names.
     - Using the scope resolution operator (`::`) to specify the desired namespace or class.
     - Renaming one of the conflicting entities.

11. **How can you use `using namespace` to simplify code?**
    - **Correction**: `using namespace` allows you to bring all names from a namespace into the current scope, so you can refer to them without needing to prefix them with the namespace name. For example:
      ```cpp
      using namespace std;
      cout << "Hello, World!" << endl; // No need to use std::cout or std::endl
      ```

12. **What is the effect of redeclaring a variable with a different type within the same scope?**
    - **Correction**: Redeclaring a variable with a different type within the same scope is not allowed and results in a compilation error. Each variable in a scope must have a unique name and type.

13. **Describe a situation where a function call might be ambiguous due to function overloading.**
    - **Correction**: Ambiguity arises when there are multiple overloaded functions and the compiler cannot determine which function to call based on the provided arguments. For example:
      ```cpp
      void func(int);
      void func(float);
      func(1.5); // Ambiguous because '1.5' is double and can match both int and float
      ```

14. **How can you use `using` to introduce names from a namespace into the current scope?**
    - **Correction**: You can use `using` to bring specific names or all names from a namespace into the current scope. For example:
	```cpp
      namespace N {
          int x = 0;
      }

      int main() {
          using N::x; // Introduce 'x' from namespace N into the current scope
          x = 10;     // Use 'x' directly
      }
```

15. **What are the differences between `std::vector<int>` and `using vi = std::vector<int>`?**
    - **Correction**: `std::vector<int>` is the actual type, while `using vi = std::vector<int>;` creates an alias `vi` for `std::vector<int>`. Using the alias can make the code more concise:
      ```cpp
      std::vector<int> v1;
      vi v2; // Equivalent to std::vector<int> v2;
      ```

16. **Explain why a function might be incorrectly defined more than once and the consequences of this.**
    - **Correction**: A function might be incorrectly defined more than once if its definition appears in multiple translation units or if a header file containing the definition is included in multiple source files. This results in a linker error due to multiple definitions. To avoid this, use function declarations in headers and definitions in source files.

17. **How does the compiler resolve which overloaded function to call when provided with different argument types?**
    - **Correction**: The compiler resolves which overloaded function to call by performing name lookup and then using a process called "argument matching" or "best match" to select the function whose parameter list best matches the provided arguments.