# Functions

Functions are C++ entities that associate a sequence of statements (a *function
body*) with a *name* and a list of zero or more *function parameters*.

```cpp
// function name: "isodd"
// parameter list has one parameter, with name "n" and type int
// the return type is bool
bool isodd(int n)
{                 // the body of the function begins
    return n % 2;
}                 // the body of the function ends
```

When a function is invoked, e.g. in a function-call expression, the parameters
are initialized from the arguments (either provided at the place of call or
defaulted) and the statements in the function body are executed. If the
parameter list ends with `...`, extra arguments can be supplied to the function,
such a function is called variadic function.

```cpp
int main()
{
    for (int arg : {-3, -2, -1, 0, 1, 2, 3})
        std::cout << isodd(arg) << ' '; // isodd called 7 times, each
                                        // time n is copy-initialized from arg
}
```

Unqualified function names in function-call expressions are looked up with an
extra set of rules called "argument-dependent lookup" (ADL).

A function can terminate by returning or by throwing an exception.

A function may be a coroutine, in which case it can suspend execution to be
resumed later.
*(since C++20)*

A function declaration may appear in any scope, but a function definition may
only appear in namespace scope or, for member and friend functions, in class
scope. A function that is declared in a class body without a friend specifier is
a class member function. Such functions have many additional properties, see
member functions for details.

Functions are not objects: there are no arrays of functions and functions cannot
be passed by value or returned from other functions. Pointers and references to
functions (except for the main function and most standard library
functions(since C++20)) are allowed, and may be used where these functions
themselves cannot. Therefore we say these functions are "addressable".

Each function has a type, which consists of the function's return type, the
types of all parameters (after array-to-pointer and function-to-pointer
transformations, see parameter list) , whether the function is `noexcept` or
not(since C++17), and, for non-static member functions, cv-qualification and
ref-qualification(since C++11). Function types also have language linkage. There
are no cv-qualified function types (not to be confused with the types of
cv-qualified functions such as `int f() const;` or functions returning
cv-qualified types, such as `std::string const f();`). Any cv-qualifier is
ignored if it is added to an alias for a function type.

Multiple functions in the same scope may have the same name, as long as their
parameter lists and, for non-static member functions, cv/ref(since
C++11)-qualifications are different. This is known as function overloading.
Function declarations that differ only in the return type and the noexcept
specification(since C++17) cannot be overloaded. The address of an overloaded
function is determined differently.

C++ implements anonymous functions using lambda-expressions.
*(since C++11)*

### Function objects

Besides function lvalues, the function call expression supports pointers to
functions, and any value of class type that overloads the function-call operator
or is convertible to function pointer (including lambda-expressions)(since
C++11). Together, these types are known as FunctionObjects, and they are used
ubiquitously through the C++ standard library, see for example, usages of
BinaryPredicate and Compare.

The standard library also provides a number of predefined function object
templates as well as the methods to compose new ones (including `std::less`,
`std::mem_fn`, `std::bind`, `std::function`(since C++11), `std::not_fn`(since
C++17), `std::bind_front`(since C++20), `std::bind_back`,
`std::move_only_function`(since C++23), `std::copyable_function`, and
`std::function_ref`(since C++26)).

---
*Source: https://en.cppreference.com/w/cpp/language/functions*
