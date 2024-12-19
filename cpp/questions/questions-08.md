### Questions on Arrays and Pointers in C++

#### Initialization
1. **How do you initialize an array in C++ with specified values?**
2. **What happens if you initialize an array with fewer values than its size?**
3. **What does the following code do?**
    ```cpp
    int b[5] = {1, 2};
    ```

#### Indexing
4. **How do you access and modify elements in an array?**
5. **What does the following code snippet do?**
    ```cpp
    int a[] {1, 2, 3, 4, 5};
    a[0] = 10;
    a[4] = 500;
    ```

#### Size of Array
6. **What does the `sizeof` operator return when used on an array?**
7. **How do you calculate the number of elements in an array?**
8. **What will the following code print?**
    ```cpp
    int a[5] = {1, 2, 3, 4, 5};
    std::cout << sizeof(a) << std::endl;
    ```

#### Stack Overflow
9. **Why can large arrays cause stack overflow errors?**
10. **What happens when you declare an array with a very large size like this?**
    ```cpp
    int a[10'000'000];
    ```

#### Static Arrays
11. **How do static arrays help avoid stack overflow?**
12. **What is the difference between static arrays and regular arrays in terms of memory allocation?**

#### Array to Pointer Conversion
13. **What does it mean for an array to decay to a pointer?**
14. **What will the following code print?**
    ```cpp
    int a[5] = {1, 2, 3, 4, 5};
    std::cout << *a << std::endl;
    std::cout << *(a + 2) << std::endl;
    ```

#### Assignment and Incrementing
15. **Is it possible to assign one array to another directly in C++?**
16. **Why is incrementing an array illegal in C++?**

#### Function Arguments
17. **What happens to an array when it is passed to a function?**
18. **Explain the following code snippet:**
    ```cpp
    void f(int* a) {
        std::cout << "Hi! " << a[2];
    }
    ```

#### Dynamic Arrays
19. **How do you allocate and deallocate a dynamic array in C++?**
20. **What is the difference between static and dynamic arrays in terms of memory management?**

#### Vector as Dynamic Array Wrapper
21. **What is `std::vector` and how does it differ from a regular array?**
22. **Why is accessing out of bounds in a `std::vector` undefined behavior?**

#### Variadic Length Arrays (VLA)
23. **What is a VLA and why is it not part of the C++ standard?**
24. **What does the following code snippet do, and why is it not portable?**
    ```cpp
    int main() {
        int n;
        std::cin >> n;
        int a[n];
        for(int i = 0; i < n; ++i) {
            a[i] = i;
        }
    }
    ```

#### Multidimensional Arrays
25. **How do you declare a 2D array in C++?**
26. **What is the difference between `int a[5][5]`, `int* b[5]`, and `int (*c)[5]`?**

#### String as Char Array
27. **How do you declare a C-style string in C++?**
28. **What is the significance of the null terminator `\0` in a char array?**

#### Pointer to Function
29. **What is a pointer to a function and how is it declared?**
30. **Explain the following code snippet:**
    ```cpp
    bool cmp(int a, int b) {
        return a > b;
    }
    int main() {
        int a[5] = {1, 2, 3, 4, 5};
        std::sort(a, a + 5, &cmp);
        for(int i = 0; i < 5; ++i) {
            std::cout << a[i] << " ";
        }
    }
    ```

#### Address Resolution of Overloaded Functions
31. **How do you resolve the address of an overloaded function?**
32. **Explain the following code snippet:**
    ```cpp
    void f(int) {}
    void f(double) {}
    int main() {
        void (*p)(int) = &f;
        void (*p2)(double) = &f;
        std::cout << (void*)p << ' ' << (void*)p2 << '\n';
    }
    ```

#### Complex Declarations
33. **What is the meaning of the following declaration?**
    ```cpp
    void (*(*pff[10])(int))(int);
    ```

#### Default Arguments
34. **How do default arguments work in C++ functions?**
35. **What will the following code output?**
    ```cpp
    void point(int x = 3, int y = 4);
    int main() {
        point(1, 2);
        point(1);
        point();
    }
    ```

#### Variadic Functions
36. **What is a variadic function and how is it defined in C++?**
37. **Explain the purpose of the following macros used with variadic functions:**
    - `va_start`
    - `va_arg`
    - `va_copy`
    - `va_end`