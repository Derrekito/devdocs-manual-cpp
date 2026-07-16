# assert

```cpp
Disabled assertion
#define assert(condition) ((void)0)  // (until C++26)
#define assert(...)       ((void)0)  // (since C++26)
Enabled assertion
#define assert(condition) /* unspecified */  // (until C++26)
#define assert(...)       /* unspecified */  // (since C++26)
```

The definition of the macro `assert` depends on another macro, `NDEBUG`, which
is not defined by the standard library.

1) If `NDEBUG` is defined as a macro name at the point in the source code where
   `<cassert>` or <assert.h> is included, the assertion is disabled: `assert`
   does nothing.

2) Otherwise, the assertion is enabled: `assert` checks if its argument (which
   must have scalar type) compares equal to zero. If it does, `assert` outputs
   implementation-specific diagnostic information on the standard error output
   and calls `std::abort`. The diagnostic information is required to include the
   text of `condition`, as well as the values of the predefined variable
   `__func__` and(since C++11) the predefined macros `__FILE__` and `__LINE__`.
   (until C++26) `assert` puts a diagnostic test into programs and expands to an
   expression of type void. `__VA_ARGS__` is evaluated and contextually
   converted to bool: If the evaluation yields `true`, there are no further
   effects. Otherwise, the assert macro’s expression creates a diagnostic on the
   standard error stream in an implementation-defined format and calls
   `std::abort()`. The diagnostic contains `#__VA_ARGS__` and information on the
   the source file name, the source line number, and the name of the enclosing
   function (such as provided by `std::source_location::current()`). (since
   C++26)

The expression `assert(E)` is guaranteed to be a constant subexpression, if
either
- `NDEBUG` is defined at the point where `assert` is last defined or redefined,
  or
- `E`, contextually converted to bool, is a constant subexpression that
  evaluates to `true`.
*(since C++17)*

### Parameters

- **condition** — expression of scalar type

### Return value

(none)

### Notes

Because `assert` is a function-like macro, commas anywhere in the argument that
are not protected by parentheses are interpreted as macro argument separators.
Such commas are often found in template argument lists and list-initialization:
```cpp
assert(std::is_same_v<int, int>);        // error: assert does not take two arguments
assert((std::is_same_v<int, int>));      // OK: one argument
static_assert(std::is_same_v<int, int>); // OK: not a macro
std::complex<double> c;
assert(c == std::complex<double>{0, 0});   // error
assert((c == std::complex<double>{0, 0})); // OK
```
*(until C++26)*

There is no standardized interface to add an additional message to `assert`
errors. A portable way to include one is to use a comma operator provided it has
not been overloaded, or use `&&` with a string literal:

```cpp
assert(("There are five lights", 2 + 2 == 5));
assert( (2 + 2 == 5) && "There are five lights");
```

The implementation of `assert` in Microsoft CRT does not conform to C++11 and
later revisions, because its underlying function (`_wassert`) takes neither
`__func__` nor an equivalent replacement.

### Example

```cpp
#include <iostream>
// uncomment to disable assert()
// #define NDEBUG
#include <cassert>

// Use (void) to silence unused warnings.
#define assertm(exp, msg) assert(((void)msg, exp))

int main()
{
    assert(2 + 2 == 4);
    std::cout << "Checkpoint #1\n";

    assert((void("void helps to avoid 'unused value' warning"), 2 * 2 == 4));
    std::cout << "Checkpoint #2\n";

    assert((010 + 010 == 16) && "Yet another way to add an assert message");
    std::cout << "Checkpoint #3\n";

    assertm((2 + 2) % 3 == 1, "Success");
    std::cout << "Checkpoint #4\n";

    assertm(2 + 2 == 5, "Failed"); // assertion fails
    std::cout << "Execution continues past the last assert\n"; // No output
}
```

Possible output:

```text
Checkpoint #1
Checkpoint #2
Checkpoint #3
Checkpoint #4
main.cpp:23: int main(): Assertion `((void)"Failed", 2 + 2 == 5)' failed.
Aborted
```

### See also

- **`static_assert` declaration(C++11)** — performs compile-time assertion
  checking
- **abort** — causes abnormal program termination (without cleaning up)
  (function)

**C documentation for `assert`**

---
*Source: https://en.cppreference.com/w/cpp/error/assert*
