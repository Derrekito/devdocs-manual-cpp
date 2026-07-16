# `inline` specifier

The inline specifier, when used in a function's decl-specifier-seq, declares the
function to be an *inline function*.

A function defined entirely inside a class/struct/union definition, whether it's
a member function or a non-member friend function, is implicitly an inline
function unless it is attached to a named module(since C++20).

A function declared constexpr or `consteval`(since C++20) on its first
declaration is implicitly an inline function.
A deleted function is implicitly an inline function: its (deleted) definition
can appear in more than one translation unit.
*(since C++11)*

The inline specifier, when used in a decl-specifier-seq of a variable with
static storage duration (static class member or namespace-scope variable),
declares the variable to be an *inline variable*.
A static data member declared constexpr on its first declaration is implicitly
an inline variable.
*(since C++17)*

### Explanation

An inline function or inline variable(since C++17) has the following properties:

- The definition of an inline function or variable(since C++17) must be
  reachable in the translation unit where it is accessed (not necessarily before
  the point of access).
- An inline function or variable(since C++17) with external linkage (e.g. not
  declared static) has the following additional properties:

In an inline function,

- Function-local static objects in all function definitions are shared across
  all translation units (they all refer to the same object defined in one
  translation unit).
- Types defined in all function definitions are also the same in all translation
  units.

Inline const variables at namespace scope have external linkage by default
(unlike the non-inline non-volatile const-qualified variables).
*(since C++17)*

The original intent of the inline keyword was to serve as an indicator to the
optimizer that inline substitution of a function is preferred over function
call, that is, instead of executing the function call CPU instruction to
transfer control to the function body, a copy of the function body is executed
without generating the call. This avoids overhead created by the function call
(passing the arguments and retrieving the result) but it may result in a larger
executable as the code for the function has to be repeated multiple times.

Since this meaning of the keyword inline is non-binding, compilers are free to
use inline substitution for any function that's not marked inline, and are free
to generate function calls to any function marked inline. Those optimization
choices do not change the rules regarding multiple definitions and shared
statics listed above.

Because the meaning of the keyword inline for functions came to mean "multiple
definitions are permitted" rather than "inlining is preferred", that meaning was
extended to variables.
*(since C++17)*

### Notes

If an inline function or variable(since C++17) with external linkage is defined
differently in different translation units, the program is ill-formed, no
diagnostic required.

The inline specifier cannot be used with a function or variable(since C++17)
declaration at block scope (inside another function).

The inline specifier cannot re-declare a function or variable(since C++17) that
was already defined in the translation unit as non-inline.

The implicitly-generated member functions and any member function declared as
defaulted on its first declaration are inline just like any other function
defined inside a class definition.

If an inline function is declared in different translation units, the
accumulated sets of default arguments must be the same at the end of each
translation unit.

In C, inline functions do not have to be declared inline in every translation
unit (at most one may be non-inline or extern inline), the function definitions
do not have to be identical (but the behavior of the program is unspecified if
it depends on which one is called), and the function-local statics are distinct
between different definitions of the same function.

See static data members for additional rules about inline static members.
Inline variables eliminate the main obstacle to packaging C++ code as
header-only libraries.
*(since C++17)*

  Feature-test macro | Value | Std | Feature
  `__cpp_inline_variables` | 201606L | (C++17) | Inline variables

### Example

Header "example.h":

```cpp
#ifndef EXAMPLE_H
#define EXAMPLE_H

#include <atomic>

// function included in multiple source files must be inline
inline int sum(int a, int b)
{
    return a + b;
}

// variable with external linkage included in multiple source files must be inline
inline std::atomic<int> counter(0);

#endif
```

Source file #1:

```cpp
#include "example.h"

int a()
{
    ++counter;
    return sum(1, 2);
}
```

Source file #2:

```cpp
#include "example.h"

int b()
{
    ++counter;
    return sum(3, 4);
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 281 | C++98 | a friend function declaration could use the inline specifier
      even if the friended function is not an inline function | prohibit such
      uses
  CWG 317 | C++98 | a function could be declared inline even if it has a
      non-inline definition in the same translation unit before the declaration
      | the program is ill- formed in this case
  CWG 765 | C++98 | a type defined in an inline function might be different in
      different translation units | such types are the same in all translation
      units
  CWG 1823 | C++98 | string literals in all definitions of an inline function
      were shared across all translation units | the requirement is removed due
      to consistency and implementations
  CWG 2531 | C++17 | a static data member could be implicitly inline even if it
      was not declared constexpr on its first declaration | it is not implicitly
      inline in this case

### See also

**C documentation for `inline`**

---
*Source: https://en.cppreference.com/w/cpp/language/inline*
