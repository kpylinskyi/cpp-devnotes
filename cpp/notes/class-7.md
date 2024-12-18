# Kinds of Memory

## RAM

RAM (Random Access Memory) is a type of computer memory that can be accessed randomly. It is the main memory used by the computer to store data that is actively being used or processed by the CPU.

```
+-------------------------------RAM---+
|--|--Data--|--Text--|--Stack--|------|
+-------------------------------------+
```

### Data (Static Memory)

The Data segment is used to store global and static variables. This memory is allocated at compile time and remains allocated throughout the program's execution. It can be further divided into two parts:

- **BSS (Block Started by Symbol):** Contains uninitialized global and static variables. The memory is zero-initialized before program execution.
- **Data Segment:** Contains initialized global and static variables.

### Text

The Text segment, also known as the code segment, stores the actual compiled code (instructions) of the program. This memory is typically read-only to prevent accidental modification of the code.

### Stack (Automatic Memory)

The Stack segment is used for local variables and function call information. Memory on the stack is managed in a LIFO (Last In, First Out) manner using push and pop operations.

**Stack Pointer:**  
The stack pointer keeps track of the top of the stack.

**Push and Pop:**  
- **Push:** Add a new item to the top of the stack.
- **Pop:** Remove the top item from the stack.

**Stack Frames:**  
Each function call creates a new stack frame, which contains the function's local variables and return address.

```cpp
int main(){
    int x = 5; // stack push
    { // new scope, new stack frame
        int y = 6; // stack push
    } // scope closed, stack pop 'y'
}
```

**Stack Visualization:**

```
+-------------------+
|x|-----------------|
|x|y|---------------|
|x|-----------------|
+-------------------+
```

#### Stack and Functions

When a function is called, the return address and function parameters are pushed onto the stack.

```cpp
void f(int y){
    std::cout << y + 1;
}

int main(){
    int x = 0;
    f(5);
}
```

**Stack Visualization:**

```
+-------------------+
|x|-----------------|
|x|ptr|y|-----------|
|x|ptr|-------------|
|x|-----------------|
+-------------------+
```

**Stack Size:**

The default stack size is usually 8 megabytes. Exceeding this limit causes a stack overflow.

```cpp
void f(int x){
    std::cout << ++x << std::endl;
    f(x); // recursive call
}

int main(){
    f(1); // segfault on 250,000
}
```

```cpp
int x = 0;

void f(){
    std::cout << ++x << std::endl;
    f(); // recursive call
}

int main(){
    f(); // segfault on 500,000
}
```

### Dynamic Memory

Dynamic memory is allocated during runtime using operators like `new` and `malloc`. Unlike stack memory, which is automatically managed, dynamic memory must be manually allocated and deallocated by the programmer.

#### `new`

Requests memory from the OS that is not part of the stack.

```cpp
int* p = new int(5);
```

#### `delete`

Frees memory that was previously allocated using `new`.

```cpp
delete p;
```

#### Dynamic Arrays

Dynamic arrays are arrays allocated on the heap.

```cpp
int* pa = new int[10'000];
delete[] pa;
```

### Memory Leaks

Memory leaks occur when dynamically allocated memory is not deallocated, causing a gradual loss of available memory. C++ does not have a garbage collector, so it's essential to match every `new` with a corresponding `delete`.

```cpp
int i = 0;

void f() {
    int* p = new int(i);
    std::cout << p << ' ' << *p << '\n';
    ++i;
    // no delete p
}

int main(){
    while(true){
        f(); // memory leaking each iteration
    }
}
```

#### Errors with `delete`

Deleting a pointer incorrectly can cause segmentation faults or undefined behavior.

```cpp
int* p = new int(5);
int* pp = new int(10);

delete p; // correct
delete pp; // correct

delete p, pp; // deletes only p, not pp
delete (p, pp); // deletes only pp, not p
```

### Static Variables

Static variables are variables that retain their value between function calls. They are stored in the data segment.

```cpp
void f(){
    static int x = 0; // initialized only once
    std::cout << ++x << '\n';
    f();
}
```

Static variables are not deallocated when the stack frame is closed.

**Static Variables in Data Segment:**

Static variables are stored in the data segment, which ensures their lifetime throughout the program execution.