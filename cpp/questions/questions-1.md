Your answers cover the main points from the class notes well. Below, I've provided a detailed and slightly more polished version of your answers to ensure clarity and completeness.

### Types and Their Operations

1. **What is static typing in C++ and how does it ensure type safety?**
    - Static typing requires the programmer to declare the type of a variable at compile-time. This ensures type safety by catching type-related errors during compilation, rather than at runtime.

2. **Explain the difference between `int` and `long long` data types. Provide examples of their usage.**
    - `int` is a 4-byte integral type that can store values from approximately -2 billion to 2 billion and is used for general-purpose integer operations. `long long` is an 8-byte integral type that can store much larger values and is used when larger ranges are needed.
    - Examples:
      ```cpp
      int i = 10;
      long long ll = 10000000000;
      ```

3. **What are the sizes of the following data types: `int`, `long long`, `short`, `char`, and `bool`?**
    - `int`: 4 bytes
    - `long long`: 8 bytes
    - `short`: 2 bytes
    - `char`: 1 byte
    - `bool`: 1 byte

4. **Describe the range of values that a typical `int` can hold in C++.**
    - A typical `int` can hold values from -2,147,483,648 to 2,147,483,647.

5. **What is the binary representation of floating-point types and how does it affect precision?**
    - Floating-point types are represented using a sign bit, an exponent, and a mantissa (or significand). This representation allows for a wide range of values but can introduce precision errors, especially for very large or very small numbers.

6. **List the three floating-point types in C++ and their sizes. Provide examples of their initialization.**
    - `float`: 4 bytes
      ```cpp
      float f = 3.14f;
      ```
    - `double`: 8 bytes
      ```cpp
      double d = 6.24;
      ```
    - `long double`: 16 bytes
      ```cpp
      long double ld = 123.654;
      ```

7. **Explain the purpose of the following modifiers: `signed`, `unsigned`, `short`, `long`, and `long long`.**
    - `signed`: Allows a variable to hold both negative and positive values.
    - `unsigned`: Allows a variable to hold only positive values.
    - `short`: Represents a shorter version of the `int` type, typically 2 bytes.
    - `long`: Represents a longer version of the `int` type, typically 4 or 8 bytes.
    - `long long`: Represents an extended length integer type, typically 8 bytes.

8. **What are fixed-width data types and why are they useful? Provide examples.**
    - Fixed-width data types are types defined in the `<cstdint>` header that guarantee variables have a specific size, useful for low-level programming and data manipulation.
    - Examples:
      ```cpp
      int8_t a = -128;
      uint8_t b = 255;
      int16_t c = 32767;
      ```

9. **Define the `size_t` type and its typical use in C++ programming.**
    - `size_t` is an unsigned integral type used for representing sizes and indexing. It is guaranteed to be large enough to represent the size of any object in memory.
    - Example:
      ```cpp
      size_t size = 10;
      ```

### `std::string`

10. **How do you initialize a `std::string` and access its individual characters? Provide code examples.**
    - Example:
      ```cpp
      std::string s = "abc";
      char a = s[0]; // 'a'
      ```

11. **What is the result of the following code snippet:**
    ```cpp
    std::string s = "abc";
    std::string sc = s + "def";
    sc += "ghi";
    std::cout << sc;
    ```
    - Result: `abcdefghi`

12. **List and explain at least four methods available for `std::string` and their usage.**
    - Example:
      ```cpp
      std::string s = "abc";
      s.size();           // Returns the size of the string
      s.capacity();       // Returns the capacity of the string
      s.at(1);            // Returns the character at index 1 with range check
      s.find('b');        // Finds the character 'b' in the string and returns its position
      ```

### `std::vector`

13. **Describe how to initialize a `std::vector<int>` and add elements to it. Provide code examples.**
    - Example:
      ```cpp
      std::vector<int> v = {1, 2, 3, 4, 5};
      v.push_back(6); // Adds 6 to the end of the vector
      v.pop_back();   // Removes the last element of the vector
      ```

15. **What does the `push_back` method do in a `std::vector`? Provide an example.**
    - `push_back` adds an element to the end of the vector.
    - Example:
      ```cpp
      std::vector<int> v;
      v.push_back(10); // v now contains {10}
      ```

16. **Explain the difference between `size` and `capacity` methods in a `std::vector`.**
    - `size()`: Returns the number of elements currently in the vector.
    - `capacity()`: Returns the number of elements that can be held in the currently allocated storage.
    - Example:
      ```cpp
      std::vector<int> v = {1, 2, 3};
      std::cout << v.size();     // Output: 3
      std::cout << v.capacity(); // Output might vary, typically more than 3
      ```

18. **How do you resize a `std::vector` and initialize new elements with a specific value? Provide code examples.**
    - Example:
      ```cpp
      std::vector<int> v = {1, 2, 3};
      v.resize(6);        // v is now {1, 2, 3, 0, 0, 0}
      v.resize(12, 5);    // v is now {1, 2, 3, 0, 0, 0, 5, 5, 5, 5, 5, 5}
      ```

### Other Containers

17. **Compare and contrast `std::list` and `std::forward_list`. When would you use each?**
    - `std::list` is a doubly-linked list where each element has pointers to both the previous and next elements. It allows bidirectional traversal.
    - `std::forward_list` is a singly-linked list where each element has a pointer only to the next element. It uses less memory and is faster for certain operations.
    - Use `std::list` when you need frequent insertions and deletions from both ends or in the middle of the sequence.
    - Use `std::forward_list` when memory efficiency is critical and you only need to traverse in one direction.

19. **What is a `std::deque` and what are its advantages over `std::vector` and `std::list`?**
    - `std::deque` (double-ended queue) allows fast insertions and deletions from both ends. Unlike `std::vector`, it does not require reallocation of the entire container when growing. Unlike `std::list`, it allows direct access to elements by index.
    - Advantages:
      - Fast insertions and deletions at both ends.
      - Efficient random access like `std::vector`.
      - Example:
        ```cpp
        std::deque<int> dq = {1, 2, 3};
        dq.push_front(0); // dq is now {0, 1, 2, 3}
        dq.push_back(4);  // dq is now {0, 1, 2, 3, 4}
        ```

21. **Describe the use and typical operations of `std::map`. Provide code examples.**
    - `std::map` stores key-value pairs in a sorted order based on the keys. It provides logarithmic time complexity for insertion, deletion, and search operations.
    - Example:
      ```cpp
      std::map<int, std::string> map;
      map[1] = "one";                // Insert or update
      std::cout << map[1];           // Access with range check
      auto it = map.find(1);         // Find element, returns iterator
      map.insert({2, "two"});        // Insert element
      ```

23. **Explain the differences between `std::map`, `std::multimap`, and `std::unordered_map`. Provide examples where each might be appropriately used.**
    - `std::map`: Stores unique key-value pairs in a sorted order. Suitable when order and uniqueness are important.
    - `std::multimap`: Allows duplicate keys and stores elements in sorted order. Suitable when multiple values for a single key are needed.
    - `std::unordered_map`: Uses a hash table for storage, providing average constant time complexity for insertion, deletion, and search operations. Suitable when order is not important but fast access is needed.
- Examples:
    
```cpp
std::map<int, std::string> m;
m[1] = "one"; // Unique keys

std::multimap<int, std::string> mm;
mm.insert({1, "one"});
mm.insert({1, "uno"}); // Duplicate keys allowed

std::unordered_map<int, std::string> um;
um[1] = "one"; // Fast access

```