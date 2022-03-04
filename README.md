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

### Chapter 2: Functions, namespaces, directives
- Rule of 3 / D.R.Y. (Do not repeat yourself) i.e. if you write the same piece of code more than 3 times, then write a function.
- Functions cannpt be defined within functions in C++ - i.e. not in `main()`

#### Defining a function
Functions are defined as:
```
<return type> <function name>(<arguments>){
    ## code
}

## e.g. A function to add 2 integers
int add(int a, int b){
    return a + b; ## because of the way C++ scopes these are only visible to the function
}
```
- If a function does not return anything then the return type is `void`.
- n.b. **Refactoring** is the process of splitting a long function into multiple sub functions.
- Functions can be declared before they are defined - this is called **forward declaration**. This tells the compiler about the existence of an *identifier*
```
int add(int x, int y); ## declare before definition
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
This tells the preprocessor to include the contents of another file, such as `#include <iostream>`.
##### define
`#define MY_NAME "Louis"` this defines a variable called MY_NAME, in the global namespace,  with value "Louis".
Like any variable it doesnt have to be initialised.
##### if,ifndef, if 0
We can use these for debugging purposes.
```
#ifndef VARIABLE
this code will not be run if VARIABLE has not been defined
#endif

#if 0
this code will not be run
#end if
```


