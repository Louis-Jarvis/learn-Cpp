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
int d{7};
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
it can also be written without blocks:
`if (age >= 21) purchaseBeer()`

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
Because all the statements that are satisfied (TRUE) are executed a `break` statement should be used, as well as a `default statement`.
```
switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
  default:
    // code block
}
```
- do while statement:
A do while statement is a looping construct that works just like a while loop, except the statement always executes at least once. After the statement has been executed, the do-while loop checks the condition. If the condition evaluates to true, the path of execution jumps back to the top of the do-while loop and executes it again.
```
do 
{
  // this will be executed right away and again if the while condition is met.
}
while (condition)
{
  // some code
}
```
- for statement:
```
for (int x = 0 ; x < 10 ; ++x )
{
  //code to loop through
}

// You can also loop over multiple variables
for (int x=0, y=0; x <=100 ; ++x, --y)
{
  // do something with two variables x, y
}
```

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
See [Bitwise manipulation](https://www.learncpp.com/cpp-tutorial/o-1-bit-flags-and-bit-manipulation-via-stdbitset/).

### Chapter 6
#### User defined namespaces
Define namespaces with
```
namespace foo // this is the name of my namespace
{
  int add(int x, int y)
  {
    return x + y;
  }
}

// Call it with
foo::add(1,2)
```
#### Local and Global variables
 - Variables are local to blocks `{ ..... }`, they will no longer exist after the code in blocks is executed. Hence these are called **local variables**.
 - Global variables can be defined outside the function but can not automatically be be accessed anywhere in the file. See below for information about linkage.
 sometimes th prefix "g_" is used to indicate that a variable is global.
#### Internal & External Linkage:
- Internal linkage
  - An identifier with internal linkage can be seen and used *within a single file*, but it is **not accessible** from other files.
  - To make a non-constant global variable internal, we use the `static` keyword.
  - [The static keyword can also allow local variables to persist out of scope](https://www.learncpp.com/cpp-tutorial/static-local-variables/).
  - const global variables have internal linkage by default.
- External linkage
  - In lesson 2.7 -- Programs with multiple code files, you learned
    that you can call a function defined in one file from another file. This is because **functions have external linkage by default**.
  - In order to call a function defined in another file, you must   
    place a forward declaration for the function in any other files wishing to use the function. 
  - The forward declaration tells the compiler about the existence
    of the function, and the linker connects the function calls to the actual function definition.
  - We can do the same thing with external variables.
  - The keyword `extern1`, will create external linkage.

  ##### EXTERNAL LINKAGE EXAMPLE 
    We could define a variable in another file such as:
    
     - `constants.h `
    ```
    #ifndef CONSTANTS_H
    #define CONSTANTS_H

    namespace constants
    {
        // since the actual variables are inside a namespace, the forward declarations need to be inside a namespace as well
        extern const double pi;
        extern const double avogadro;
        extern const double myGravity;
    }

    #endif
    ```

    - `constants.cpp`.
    ```
    #include "constants.h"

    namespace constants
    {
        // actual global variables
        extern const double pi { 3.14159 };
        extern const double avogadro { 6.0221413e23 };
        extern const double myGravity { 9.2 }; // m/s^2 -- gravity is light on this planet
    }
    ```
    
    - `main.cpp`
    ```
    #include <iostream>

    extern int g_x; // this extern is a forward declaration of a variable named g_x that is defined somewhere else
    extern const int g_y; // this extern is a forward declaration of a const variable named g_y that is defined somewhere else

    int main()
    {
        std::cout << g_x; // prints 2

        return 0;
    }
    ```

### Chapter 7 - Control Flow
#### Types of control flow
|       Category           |         Description         |
|  --------------------    |   -----------------------   |
|      conditional         |                             |
|        halts             |                             |
|       loops              |                             |
|       halts              |                             |
|       Exceptions         |                             |

#### Break statement and continue
 - A `break` statement terminates the switch or loop, and execution continues at the first statement beyond the switch or loop. 
 - The `continue` statement provides a convenient way to   end the current iteration of a loop without terminating  the entire loop.
 - `std::exit(0);` This will cause the program to exit normally with status code 0.
 - The `std::abort()` function causes your program to terminate abnormally. This won't do any cleanup.

#### Handling errors
 - Just use an if (or switch) statement to handle errors
 - When the user enters input in response to an extraction operation, that data is placed in a buffer inside of std::cin. 
- A **buffer** (also called a data buffer) is simply a piece of memory set aside for storing data temporarily while it’s moved from one place to another. In this case, the buffer is used to hold user input while it’s waiting to be extracted to variables.
- To ignore everything up to and including the next ‘\n’ character, we call
 `std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');`
- More can be found out about [handling invalid user input](https://www.learncpp.com/cpp-tutorial/stdcin-and-handling-invalid-input/).

##### Asserts
 - An assertion is an expression that will be true unless there is a bug in the program. If the expression evaluates to true, the assertion statement does nothing. If the conditional expression evaluates to false, an error message is displayed and the program is terminated (via std::abort). 
 - This error message typically contains the expression that failed as text, along with the name of the code file and the line number of the assertion. 
 - Need to include `#include <cassert> // for assert()`
 - call it with `assert(condition && "Need to handle case where student was just moved to another classroom");`
- We can also use `static_assert(sizeof(long) == 8, "long must be 8 bytes");`

#### (Pseudo) Random Number Generators
We can use the Mersenne Twister algorithm
```
#include <iostream>
#include <random> // for std::mt19937

int main()
{
	std::mt19937 mt; // Instantiate a 32-bit Mersenne Twister

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << mt() << '\t'; // generate a random number

		// If we've printed 5 numbers, start a new row
		if (count % 5 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

### Chapter 8 - Aliases, auto, FUNCTIONS extended
#### aliases
 - Aliases (names) for an existing data type can be  created with the `using` keyword such as in `using distance_t = double;`
 - `typedef` also accomplishes this, e.g. `typedef long miles_t`
 - Aliases like this can be useful for the purpose of code readability.
 - It can also make long complex types easier to deal with.
 ```
 using pairlist_t = std::vector<std::pair<std::string, int>>;
 ```

#### auto
- The `auto` keyword automatically infers the type.
- This can be applied to variables or to functions.

#### Functional overloading
Function overloading allows us to create multiple functions with the *same name*, so long as each identically named function has *different parameters* (or the functions can be otherwise differentiated).
E.g.
```
int add(int x, int y) // integer version
{
    return x + y;
}

double add(double x, double y) // floating point version
{
    return x + y;
}
```
This means there are different number of arguments or argument types.

#### Default arguments
Ex. `void print(int x, int y=4)`

#### Function templates
This is an alternative to function overloading and allows us to create a function template when we want to use inputs with different input and output types.

```
// template for the max class
// the arguments and returns will be of type T.
template <typename T> 
T max(T x, T y) 
{
    return (x > y) ? x : y; //ternary operator
}

// to instantiate this (and call it) we need to do
max<int>(1,2)
```
**NOTE** you have to be careful about defining template helper functions in other files.

### Chapter 9 - intro to: pointers, pass by address, enums, structs

### Chapter 10 - Arrays, Strings, Pointers, and References

### Chapter 11 - Functions V3

### Chapter 12 - Basic OOP

### Chapter 13 - Operators

##### Chapter 14 --> Chapter 16 doesn't exist

### Chapter 17 - Inheritance

### Chapter 18 - Virtual Functions

### Chapter 19 - Templates and Classes

### Chapter 20 - Exceptions

### Chapter 21 - Standard template library

### Chapter 22 std::string
