# Advanced C++ Notes: Class 2

## Literals

- **Integer Literals**: These represent integral numbers. Examples include: `5`, `3`, `32`, `-10`, `0`. Integer literals can be specified in different bases:
  - Decimal (base 10): `10`
  - Octal (base 8): `012` (starts with a `0`)
  - Hexadecimal (base 16): `0xA` or `0X1F`
  - Binary (base 2) (C++14 and later): `0b1010`

- **Floating Point Literals**: These represent floating-point numbers. The `f` suffix indicates a float type:
  - Examples: `5.43f`, `54.68f`, `3.14F`, `2.0e10f` (scientific notation)
  - Without the `f` suffix, the default type is `double`.

- **Double Literals**: These are floating-point numbers that default to `double` type:
  - Examples: `4.53`, `9.32`, `2.0e10` (scientific notation)

- **String Literals**: These are sequences of characters enclosed in double quotes. In C++, they can be represented as:
  - Examples: `"abc"`, `"Hello, World!"`
  - Raw string literals can be created using `R"(string content)"`, allowing for multi-line strings and the inclusion of special characters without escape sequences.

- **Character Literals**: These are single characters enclosed in single quotes:
  - Examples: `'a'`, `'1'`, `'\n'` (newline character), `'\t'` (tab character)

- **Unsigned Literals**: These represent non-negative integral numbers. The `u` or `U` suffix indicates that the literal is of type `unsigned int` or `unsigned long`:
  - Examples: `5u`, `10U`
  - Note that unsigned floating-point literals like `5.43u` are not standard; they are typically just treated as `float` or `double`.

- **Long Literals**: These represent integral numbers that can hold a larger range. The `l` or `L` suffix indicates a `long` type:
  - Examples: `5L`, `12345678901234l`

- **Long Long Literals**: These represent even larger integral numbers. The `ll` or `LL` suffix indicates a `long long` type:
  - Examples: `5LL`, `12345678901234567890ll`

- **Boolean Literals**: These represent the two possible truth values:
  - Examples: `true`, `false`

- **Null Pointer Literals**: This represents a null pointer in C++:
  - Example: `nullptr` (C++11 and later)

## Promotions

### Integer Promotions
When operating on two integral types with different capacities, the result type will be the one with the larger capacity to prevent data loss. This process is known as integer promotion or type promotion.

- **Example**:
  - If you operate on an `int` and a `long long`, the result will be `long long`.
  - If you operate on a `char` and an `int`, the result will be `int`.
  - If you operate on an `unsigned int` and a `long`, the result will be `long`.

### Float Promotions
Similar to integer promotions, floating-point promotions ensure that the result type has enough precision to accommodate the operands.

- **Example**:
  - If you operate on a `float` and a `double`, the result will be `double`.
  - If you operate on a `float` and a `long double`, the result will be `long double`.

### Detailed Rules for Type Promotions

1. **Integer Promotions**:
   - Small integer types (`char`, `short`, `bool`) are promoted to `int` if `int` can represent all the values of the original type; otherwise, they are promoted to `unsigned int`.
   - When two types are combined in an expression:
     - If either type is `unsigned long long`, the other type is converted to `unsigned long long`.
     - If one type is `long long` and the other is `unsigned long`, the result depends on whether `unsigned long` can represent all values of `long long`.
     - If either type is `long long`, the other type is converted to `long long`.
     - If either type is `unsigned long`, the other type is converted to `unsigned long`.
     - If either type is `long` and the other is `unsigned int`, the result depends on whether `unsigned int` can represent all values of `long`.
     - If either type is `long`, the other type is converted to `long`.
     - If either type is `unsigned int`, the other type is converted to `unsigned int`.

2. **Floating-Point Promotions**:
   - If either operand is `long double`, the other is converted to `long double`.
   - If either operand is `double`, the other is converted to `double`.
   - If either operand is `float`, the other is converted to `float`.

### Mixed-Type Promotions
When combining integral and floating-point types, the integral type is usually converted to a floating-point type.

- **Example**:
  - If you operate on an `int` and a `float`, the `int` is promoted to `float`, and the result is `float`.
  - If you operate on a `long` and a `double`, the `long` is promoted to `double`, and the result is `double`.

## Linux Commands

### Navigation Commands

- `cd (dirname)`: Change the current directory to `dirname`.
- `cd ..`: Move up one directory level.
- `pwd`: Print the current working directory.
- `cd /`: Change to the root directory.
- `cd ../../..`: Move up three directory levels.
- `cd -`: Change to the previous directory.

### Listing Commands

- `ls`: List the contents of the current directory.
- `ls -l`: List the contents of the current directory in long format, displaying detailed information about each file and directory.
- **Colors of Names in `ls` File List**:
  - Blue: Directory
  - Green: Executable file
  - Red: Compressed file
  - Purple: Symbolic link
  - Yellow: Device file

### File Permissions

File permissions are displayed in the long format listing of `ls -l`. The permissions string is divided into four sections:

1. **Type**:
   - `-`: Regular file
   - `d`: Directory
   - `l`: Symbolic link

2. **Owner Permissions**: The next three characters represent the owner's permissions:
   - `r`: Read
   - `w`: Write
   - `x`: Execute

3. **Group Permissions**: The next three characters represent the group's permissions:
   - `r`: Read
   - `w`: Write
   - `x`: Execute

4. **Others' Permissions**: The last three characters represent the permissions for others:
   - `r`: Read
   - `w`: Write
   - `x`: Execute

**Examples**:
- `lrwxrwxrwx`: A symbolic link with read, write, and execute permissions for everyone.
- `drwxrwxrwx`: A directory with read, write, and execute permissions for everyone.
- `-rwxrwxrwx`: A regular file with read, write, and execute permissions for everyone.

**Changing Permissions**
- `chmod`: Change the mode (permissions) of a file or directory.
    - `chmod -x filename`: Remove the execution permission from `filename`.
    - `chmod +x filename`: Add the execution permission to `filename`.

**Echo Command**
- `echo`: Display a line of text or the value of a variable.
    - Example: `echo "Hello, World!"` will print `Hello, World!` to the terminal.
    - Example: `echo $HOME` will print the value of the `HOME` environment variable.

### Basic File and Directory Commands

- `cat filename`: Display the contents of `filename`.
- `mkdir (dirname)`: Create a new directory named `dirname`.

### Editing with `vim`

- `vim filename`: Open `filename` in the `vim` text editor.

**Basic `vim` Commands**:
- `i`: Enter insert mode to start editing.
- `:q!`: Quit without saving.
- `:wq`: Save changes and quit.
- `Ctrl + w`: Delete the previous word in insert mode.
- `Ctrl + u`: Delete the entire line in insert mode.

Here's the expanded version with additional information:

## Compilers

### Overview of Common Compilers

1. **GNU g++**:
   - Part of the GNU Compiler Collection (GCC).
   - Widely used for compiling C++ programs.
   - Open-source and available on many platforms including Linux, macOS, and Windows (via MinGW or Cygwin).

2. **LLVM clang**:
   - A compiler front end for the C, C++, and Objective-C languages.
   - Part of the LLVM project.
   - Known for its modularity and extensibility.
   - Provides useful diagnostic messages and error reports.
   - Used as the default compiler on macOS.

3. **MSVC (Microsoft Visual C++)**:
   - The C, C++, and C++/CLI compiler provided by Microsoft for use in its development environment.
   - Part of the Microsoft Visual Studio suite.
   - Widely used for developing Windows applications.
   - Offers a rich set of debugging tools and integration with the Windows API.

### Using GNU g++

- **Compiling a C++ Program**:
  - Command: `g++-11 main.cpp`
  - This command invokes the g++ compiler to compile `main.cpp`.
  - By default, the resulting binary is named `a.out`.

- **Execution of Resulting Binary**:
  - Command: `./a.out`
  - This command runs the compiled program `a.out`.

### Using LLVM clang

- **Compiling a C++ Program**:
  - Command: `clang++ main.cpp -o main`
  - This command invokes the clang++ compiler to compile `main.cpp` and outputs the binary as `main`.

- **Execution of Resulting Binary**:
  - Command: `./main`
  - This command runs the compiled program `main`.

### Using MSVC

- **Compiling a C++ Program**:
  - Command: `cl /EHsc main.cpp`
  - This command invokes the MSVC compiler to compile `main.cpp`.
  - By default, the resulting binary is named `main.exe`.

- **Execution of Resulting Binary**:
  - Command: `main.exe`
  - This command runs the compiled program `main.exe`.

### Additional Information

- **Compilation Flags**:
  - **Optimization**: `-O1`, `-O2`, `-O3` for various levels of optimization.
  - **Debugging**: `-g` to include debugging information.
  - **Warnings**: `-Wall` to enable most warning messages.
  - **Standards Compliance**: `-std=c++11`, `-std=c++14`, `-std=c++17`, `-std=c++20` to specify the C++ standard version.

- **Linking Multiple Files**:
  - Command: `g++-11 file1.cpp file2.cpp -o myprogram`
  - This compiles and links `file1.cpp` and `file2.cpp` into the binary `myprogram`.