### Questions on Kinds of Memory

#### General Concepts
1. **What is RAM and how is it used by the computer?**
2. **List the main segments of memory in RAM.**

#### Data Segment
3. **What is stored in the Data segment?**
4. **Differentiate between the BSS and the Data Segment.**
5. **What happens to uninitialized global and static variables before program execution?**

#### Text Segment
6. **What is stored in the Text segment?**
7. **Why is the Text segment typically read-only?**

#### Stack Segment
8. **What is the Stack segment used for?**
9. **Explain the LIFO (Last In, First Out) principle in the context of the stack.**
10. **What is the role of the stack pointer?**

#### Stack Operations
11. **What are the push and pop operations in the context of the stack?**
12. **Explain stack frames with an example.**
13. **What happens to the stack frame when a function call is completed?**

#### Stack and Functions
14. **What is pushed onto the stack when a function is called?**
15. **Visualize the stack before and after a function call with local variables.**

#### Stack Size
16. **What is the default stack size in most systems?**
17. **What causes a stack overflow?**

#### Dynamic Memory
18. **What is dynamic memory and how is it different from stack memory?**
19. **How is memory allocated dynamically in C++?**
20. **How is dynamically allocated memory deallocated in C++?**

#### Dynamic Arrays
21. **How do you create and delete a dynamic array in C++?**

#### Memory Leaks
22. **What is a memory leak?**
23. **Why is it important to deallocate dynamically allocated memory in C++?**

#### Errors with `delete`
24. **What errors can occur when deleting pointers incorrectly?**
25. **Explain the following code snippet and the potential error:**
    ```cpp
    int* p = new int(5);
    int* pp = new int(10);
    delete p, pp;
    ```

#### Static Variables
26. **What is a static variable?**
27. **Where are static variables stored?**
28. **What is the lifetime of a static variable in a function?**

#### Static Variables in Data Segment
29. **Why are static variables stored in the data segment?**
30. **Explain the following code snippet and the behavior of the static variable:**
    ```cpp
    void f(){
        static int x = 0; // initialized only once
        std::cout << ++x << '\n';
        f();
    }
    ```

### Answers on Kinds of Memory

#### General Concepts
1. **What is RAM and how is it used by the computer?**
   - RAM (Random Access Memory) is a type of computer memory that can be accessed randomly. It is the main memory used by the computer to store data that is actively being used or processed by the CPU.

2. **List the main segments of memory in RAM.**
   - Data segment (Static Memory)
   - Text segment (Code)
   - Stack segment (Automatic Memory)

#### Data Segment
3. **What is stored in the Data segment?**
   - The Data segment stores global and static variables.

4. **Differentiate between the BSS and the Data Segment.**
   - The BSS contains uninitialized global and static variables, while the Data Segment contains initialized global and static variables.

5. **What happens to uninitialized global and static variables before program execution?**
   - Uninitialized global and static variables in the BSS are zero-initialized before program execution.

#### Text Segment
6. **What is stored in the Text segment?**
   - The Text segment stores the actual compiled code (instructions) of the program.

7. **Why is the Text segment typically read-only?**
   - The Text segment is typically read-only to prevent accidental modification of the code.

#### Stack Segment
8. **What is the Stack segment used for?**
   - The Stack segment is used for local variables and function call information.

9. **Explain the LIFO (Last In, First Out) principle in the context of the stack.**
   - In the stack, the most recently added item (last in) is the first one to be removed (first out).

10. **What is the role of the stack pointer?**
    - The stack pointer keeps track of the top of the stack.

#### Stack Operations
11. **What are the push and pop operations in the context of the stack?**
    - Push: Add a new item to the top of the stack.
    - Pop: Remove the top item from the stack.

12. **Explain stack frames with an example.**
    - A stack frame is created for each function call and contains the function's local variables and return address. For example:
    ```cpp
    int main(){
        int x = 5; // stack push
        { // new scope, new stack frame
            int y = 6; // stack push
        } // scope closed, stack pop 'y'
    }
    ```

13. **What happens to the stack frame when a function call is completed?**
    - When a function call is completed, its stack frame is popped off the stack, freeing the memory used for the function's local variables.

#### Stack and Functions
14. **What is pushed onto the stack when a function is called?**
    - The return address and function parameters are pushed onto the stack.

15. **Visualize the stack before and after a function call with local variables.**
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

#### Stack Size
16. **What is the default stack size in most systems?**
    - The default stack size is usually 8 megabytes.

17. **What causes a stack overflow?**
    - A stack overflow is caused by exceeding the stack size limit, often due to deep or infinite recursion.

#### Dynamic Memory
18. **What is dynamic memory and how is it different from stack memory?**
    - Dynamic memory is allocated during runtime using operators like `new` and `malloc`. Unlike stack memory, which is automatically managed, dynamic memory must be manually allocated and deallocated by the programmer.

19. **How is memory allocated dynamically in C++?**
    - Memory is allocated dynamically in C++ using the `new` operator.

20. **How is dynamically allocated memory deallocated in C++?**
    - Dynamically allocated memory is deallocated using the `delete` operator.

#### Dynamic Arrays
21. **How do you create and delete a dynamic array in C++?**
    ```cpp
    int* pa = new int[10'000];
    delete[] pa;
    ```

#### Memory Leaks
22. **What is a memory leak?**
    - A memory leak occurs when dynamically allocated memory is not deallocated, causing a gradual loss of available memory.

23. **Why is it important to deallocate dynamically allocated memory in C++?**
    - It is important to deallocate dynamically allocated memory to avoid memory leaks, which can lead to a gradual reduction in available memory and eventual program failure.

#### Errors with `delete`
24. **What errors can occur when deleting pointers incorrectly?**
    - Incorrectly deleting pointers can cause segmentation faults or undefined behavior, such as double deletion or deleting the wrong pointer.

25. **Explain the following code snippet and the potential error:**
    ```cpp
    int* p = new int(5);
    int* pp = new int(10);
    delete p, pp;
    ```
    - The code attempts to delete both `p` and `pp` using a comma operator, but it only deletes `p`, not `pp`. The correct way to delete both pointers is to use two separate `delete` statements:
    ```cpp
    delete p;
    delete pp;
    ```

#### Static Variables
26. **What is a static variable?**
    - A static variable is a variable that retains its value between function calls and is stored in the data segment.

27. **Where are static variables stored?**
    - Static variables are stored in the data segment.

28. **What is the lifetime of a static variable in a function?**
    - The lifetime of a static variable in a function is the entire duration of the program execution.

#### Static Variables in Data Segment
29. **Why are static variables stored in the data segment?**
    - Static variables are stored in the data segment to ensure they retain their value throughout the program's execution.

30. **Explain the following code snippet and the behavior of the static variable:**
    ```cpp
    void f(){
        static int x = 0; // initialized only once
        std::cout << ++x << '\n';
        f();
    }
    ```
    - In this code, the static variable `x` is initialized only once and retains its value between function calls. Each recursive call to `f()` increments `x` and prints its value, leading to increasing output until a stack overflow occurs due to infinite recursion.