# learn-Cpp
## Author: Louis Jarvis

# Description:
This is where I'll post everything I have learned about C++ from [Learn Cpp](https://www.learncpp.com/).

### Run time vs compile time

### How code is compiled:
Preprocessor

### File Structure
Typical structure:
- Header files - e.g. `add.h`
  - extension `.h`(sometimes `.hpp`).
  - generally should **not** contain function and varibale definitions.
  - it should include forward delcarations of the functions (just the name and args). 
  - use `<>` for referencing headers that come with the compiler like `iostream`, otherwise use `""`.
  - Each file should explicitly #include all of the header files it needs to compile. Do not rely on headers included transitively from other headers.
  - Use header guards to avoid namespace collision:
    ```
    #ifndef SOME_UNIQUE_NAME_HERE
    #define SOME_UNIQUE_NAME_HERE

    // your declarations (and certain types of definitions) here

    #endif
    ```
    Or you can use `# pragma once` but this is not supported by all compilers.
    
  - tips
    - each header file should have a specific job & be as independent as possible.
    - it should #include all the dependencies
- Function (source) files - e.g. `add.cpp`
  - extension `.cpp`.
  - needs to include its paired header, such as `#include "add.h"`.
  - typically defines a function.
  - do not #include .cpp files.
- `main.cpp` file
  - Calls all the other scripts 
  - needs an `#include "add.h"`
The compiler compiles each file individually - it does not check each file against others.

### Chapter 1: Intro
**Statements**(line of code) and **expressions** (combination of symbols to be evaluated) are concluded by `;`.
**Identifier**: a unique name for a varible, class, function etc.
The entry point of a program is:
```
int main()
{
    // some code
    return 0; ## the STATUS code for our program (0 =  successful exit)
}
```
#### Variables:
There are varous ways of initialising variables:
```
int a;
int b = 5;
int d{7}
```
You must declare the type: one of `int`, `float`,`double`,`bool`, `char`.
Variables can be declared without being initialised.
Variables can be initalised with expressions and function calls e.g. `int i { add(a,b) };`.

#### Input  & Output
- Get user input with `std::cin` e.g. take in 2 numbers from the user with`std::cin >> x >> y`.
- Printing output to the screen: use `std::cout << "hello";`.

### Chapter 2: Functions, Namespaces, Directives
- Rule of 3 / D.R.Y. (Do not repeat yourself) i.e. if you write the same piece of code more than 3 times, then write a function.
- Functions cannpt be defined within functions in C++ - i.e. not in `main()`

#### Defining a function
Functions are defined as:
```
<return type> <function name>(<arguments>){
    // code
}

// e.g. A function to add 2 integers
int add(int a, int b){
    return a + b; // because of the way C++ scopes these are only visible to the function
}
```
- If a function does not return anything then the return type is `void`.
- n.b. **Refactoring** is the process of splitting a long function into multiple sub functions.
- Functions can be declared before they are defined - this is called **forward declaration**. This tells the compiler about the existence of an *identifier*
```
int add(int x, int y); // declare before definition
```

We use *can* use forward declarations in the top of the `main.cpp` script so that the compiler knows that a function is defined somewhere else.

#### Naming collisions and namespaces
E.g. the same function name/identifier is used across multiple files, but the functions have *different* definitions. This will cause a **linker** error.
**Namespace**(s) are a region that allows you to declare names inside of it for the the purpose of avoiding collisions.
Any name not defined inside a class,function or namespace is considerd part of the **global namespace**.
```
## namespace contents are accessed with: the `::` operator
std::cout
```
Namespaces can also be activated using a directive: `using namespace std` which tells the compile to check the std namespace when looking for identifiers with no prefix. But this is *not* recommeneded.

#### Preprocessor Directives
##### include
This tells the preprocessor to include the contents of another file, such as `#include <iostream>` or `#include.
##### define
`#define MY_NAME "Louis"` this defines a variable called MY_NAME, in the global namespace,  with value "Louis".
Like any variable it doesnt have to be initialised.
##### if, ifndef, if 0
We can use these for debugging purposes.
```
#ifndef VARIABLE
this code will not be run if VARIABLE has not been defined
#endif

#if 0
this code will not be run
#end if
```

### Chapter 3 - Debugging
see website

### Chapter 4 - Fundamental Data Types, Conditions
#### Ints and floats
Different integer types have different sizes in bytes.
There are special NaN and inf types: `nan`,`posinf`,`neginf`.
If we divide integers this will carry out integer division - 8/5 = 1
Best practices for `int`:
  - Prefer `int` when the size of the integer doesn't matter (e.g. the number will always fit within the range of a 2-byte signed integer).
  - Prefer `std::int#_t` when storing a quantity that needs a guaranteed range.
  - Prefer `std::uint#_t` when doing bit manipulation or where well-defined wrap-around behavior is required.
Tips for `float`:
  - floats are useful for storing either very large or very small numbers.
  - `std::setprecision()` allows us to set the number of digits we want a floating point number to show.
  - Favour `double` over `float` because the lack of precision in float can lead to inaccuracies.
#### Issues:
- Rounding error - when a number cannot be adequately represented by a finite number of  binary digits (e.g 0.1 is represented as 0.100000000000001).
- integer overflow - when a number is too large to be stored in the specfied data type. This causes integers to *wrap around*.
Booleans:
 - set the value as either `true` or `false`. `bool b1 {true}`.
#### Conditions:
- if statement:
```
if (statement1){
    //***
} else if (statement2) {
   //***
} else {
   //***
}
```
- while loop:
```
while (condition)
{
      if (....)
      {
        // do something
        // update condition
      }
}
```
- switch statement:
#### Characters - `chars`
how to intialise a char `char ch1{ 'a' }` but you cannot initialise multiple chars at once (e.g. `abc`).

#### Type conversion
we can use the command `static_cast<type>(variable)` to convert between types. E.g. `static_cast<int>(5.5) will return 5`.
This is especially useful for converting from signed to unsigned and vice versa.

#### std::string
strings arent a fundamental data type - they are a **compound type** declared in the C standard library.
```
std::string myName {"Louis"}; //initialise a string as such.
```
To enter a full line of text use something like `std::getline()` with `std::ws()`. The latter tells std::cin to ignore any leading whitespace.
```
std::cout << "Enter your age: ";
std::string age{};
std::getline(std::cin >> std::ws, age); 
```
The length of a string can be determined with: `STRING.length()`

Also note that std::string::length() returns an unsigned integral value (most likely size_t). If you want to assign the length to an int variable, you should static_cast it to avoid compiler warnings about signed/unsigned conversions:
```
int length = static_cast<int>(myName.length());
```
#### Literals, constants
A constant is a fixed value that may not be changed.
Literal constants are unamed values inserted directly into the code - like the number 5 or 6e3.
Constants can be intialised with the `const` keyword.
such as:
```
// const values must be initialised
const double gravity = 9.98;
```
`constexpr` means the constant must be determined at compile time. It is best practice to use this.

### Chapter 5 operators
#### Some operators:
| Operator     | Description   |
| ------------ | --------------|
|    `%`       |               |
|    `++`      |               |
|    `--`      |               |               
|    `&`       |               |
|    `*`       |               |
|    `*=`      |               |
|    `/=`      |               |
|    `%=`      |               |
| `+=` and `-=`|               |
|     `?:`     |               |
|   `new`      |               |
|  `delete`    |               |
|  `&&` or `&` |               |
| `||` or `|`  |               |
|    `^`       |               |
|    `!=`      |               |

#### exponentiation & math functions
We need the `cmath` library.
```
#include <cmath>
double x {std::Pow(3,4) // this gives 3^{4} }
```
#### Side effects
A **side effect** occurs if the code does anything/affects anything after the expression/function has ended.
`++x` will modify x everywhere whereas `x++` will not.

#### Floating point equality
Due to arithmetic errors we should compare floating point values using: `std::abs(a - b) <= epsilon` which is found in the `<cmath>` header

### Chapter O
#### Bit manipulation
In the majority of cases, this is fine -- we’re usually not so hard-up for memory that we need to care about 7 wasted bits (we’re better off optimizing for understandability and maintainability). However, in some storage-intensive cases, it can be useful to “pack” 8 individual Boolean values into a single byte for storage efficiency purposes.

Doing these things requires that we can manipulate objects at the bit level. Fortunately, C++ gives us tools to do precisely this. Modifying individual bits within an object is called bit manipulation.

Bit manipulation is also useful in encryption and compression algorithms.
see [Bitwise manipulation](https://www.learncpp.com/cpp-tutorial/o-1-bit-flags-and-bit-manipulation-via-stdbitset/).

#### Chapter 6