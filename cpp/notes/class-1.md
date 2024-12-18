# Advanced C++ Notes: Class 1

### Types and Their Operations

#### Static Typing
In C++, the type of a variable is defined at compile-time and cannot be changed at runtime. This ensures type safety and can catch errors during the compilation process rather than at runtime.

### Data Types

#### Integral Types

| Data Type   | Description        | Size (Bytes) | Example              |
| ----------- | ------------------ | ------------ | -------------------- |
| `int`       | Integer type       | 4            | `int i = 10;`        |
| `long long` | Long Long          | 8            | `long long ll = 10;` |
| `short`     | Short Integer type | 2            | `short s = 10;`      |
| `char`      | Character type     | 1            | `char c = 'a';`      |
| `bool`      | Boolean type       | 1            | `bool b = true;`     |

- `int` is at least 2 bytes in size, typically 4 bytes, capable of storing values in the range [-2,147,483,648; 2,147,483,647].

#### Floating-Point Types

| Data Type     | Description | Size (Bytes) | Example                    |
| ------------- | ----------- | ------------ | -------------------------- |
| `float`       | Single-precision floating-point | 4            | `float f = 3.14f;`         |
| `double`      | Double-precision floating-point | 8            | `double d = 6.24;`         |
| `long double` | Extended-precision floating-point | 16           | `long double ld = 123.654;`|

Floating-point types use a binary representation that consists of a sign bit, an exponent, and a mantissa (or significand). The size of the mantissa determines the precision of the floating-point type.

#### Modifiers

Modifiers can be applied to basic types to alter their characteristics:

| Modifier    | Description                                     | Example            |
| ----------- | ----------------------------------------------- | ------------------ |
| `signed`    | Can hold both negative and positive values      | `signed int a;`    |
| `unsigned`  | Can only hold positive values                   | `unsigned int a;`  |
| `short`     | Shorter integer type (usually 2 bytes)          | `short int a;`     |
| `long`      | Longer integer type (usually 8 bytes)           | `long int a;`      |
| `long long` | Extended length integer type (at least 8 bytes) | `long long int a;` |

#### Fixed-Width Data Types

- `int8_t`: 8-bit signed integer.
- `uint8_t`: 8-bit unsigned integer.
- `int16_t`: 16-bit signed integer.
- `uint16_t`: 16-bit unsigned integer.
- `int32_t`: 32-bit signed integer.
- `uint32_t`: 32-bit unsigned integer.
- `int64_t`: 64-bit signed integer.
- `uint64_t`: 64-bit unsigned integer.
- `std::byte`: 1-byte integral type.

These types are defined in the `<cstdint>` header and ensure that variables have a specific size, which is useful for low-level programming and data manipulation.

#### Boolean
Boolean type requires 1 byte and stores the values `true` or `false`.

#### `size_t`
Unsigned integral type used for representing sizes and indexing. It is guaranteed to be large enough to represent the size of any object in memory.

### `std::string`

#### Initialization
```cpp
std::string s = "abc";
```

#### Indexing
```cpp
s[0]; // 'a'
s[1]; // 'b'
s[2]; // 'c'
s[3]; // Undefined behavior (no null terminator in std::string)
s[4]; // Undefined behavior
```

#### Concatenation
```cpp
std::string sc = s + "def"; // sc = "abcdef"
sc += "ghi"; // sc = "abcdefghi"
```

#### Methods
```cpp
s.at(1);          // Indexing with range check
s.push_back('x'); // Append to the end of the string
s.pop_back();     // Remove from the end of the string
s.front();        // First character of the string
s.back();         // Last character of the string
s.size();         // Return the size of the string
```

### `std::vector`

#### Initialization
```cpp
std::vector<int> v;
```

#### Methods
```cpp
v.at(1);             // Indexing with range check
v.push_back(10);     // Append to the end of the vector
v.pop_back();        // Remove from the end of the vector
v.front();           // First element of the vector
v.back();            // Last element of the vector
v.size();            // Return the size of the vector
v.clear();           // Clear vector's values, not memory
v.shrink_to_fit();   // Reduce memory usage to fit the current size
v.reserve(size_t n); // Reserve memory for n elements
v.resize(size_t n, int e = 0); // Resize vector to n elements, initialize new elements with e
v.capacity();        // Return the number of reserved elements
```

### Other Containers

#### `std::list`
Doubly-linked list allowing constant time insertions and deletions from anywhere in the sequence. Slower traversal compared to `std::vector`.

#### `std::forward_list`
Singly-linked list allowing constant time insertions and deletions from the front of the sequence. Uses less memory than `std::list`.

#### `std::deque`
Double-ended queue allowing fast insertions and deletions from both ends. Elements are stored in multiple contiguous memory blocks.

#### `std::stack`
LIFO (Last In, First Out) data structure. Elements are inserted and removed from the same end.

#### `std::queue`
FIFO (First In, First Out) data structure. Elements are inserted at the back and removed from the front.

#### `std::priority_queue`
A type of container adaptor, specifically designed such that the first element of the queue is the greatest of all elements in the queue, and elements are in non-increasing order.

### `std::map<key_type, value_type>`
A balanced tree (typically a Red-Black Tree), providing logarithmic time complexity for insertion, deletion, and search operations.

#### Operations
```cpp
std::map<int, std::string> map;
map[1] = "one";                // Insert or update
map.at(1);                     // Access with range check
auto it = map.find(1);         // Find element, returns iterator
map.insert({2, "two"});        // Insert element
```

### `std::multimap`
Allows duplicate keys. Stores elements in a balanced tree structure, providing logarithmic time complexity for insertion and retrieval operations.

```cpp
std::multimap<int, std::string> mm;
mm.insert({1, "one"});
mm.insert({1, "uno"});
mm.lower_bound(1); // Iterator to the first element with key >= 1 : 1 -> one
mm.upper_bound(1); // Iterator to the first element with key > 1 : 1 -> uno
```

### `std::unordered_map`
A hash table, providing average constant time complexity for insertion, deletion, and search operations. Keys are not stored in any particular order.

### I/O Stream

#### Input
```cpp
std::cin >> x; // Read input into x
```

#### Output
```cpp
std::cout << x; // Print x to standard output
```

I/O streams in C++ provide a standard way to read and write data. `std::istream` is used for input streams, and `std::ostream` is used for output streams. They support various operations for handling input and output in a type-safe manner.