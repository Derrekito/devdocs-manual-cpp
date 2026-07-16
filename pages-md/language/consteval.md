# `consteval` specifier (since C++20)

- `consteval` - specifies that a function is an *immediate function*, that is,
  every call to the function must produce a compile-time constant

### Explanation

The `consteval` specifier declares a function or function template to be an
*immediate function*, that is, every potentially-evaluated call to the function
must (directly or indirectly) produce a compile time constant expression.

An immediate function is a constexpr function, subject to its requirements as
the case may be. Same as `constexpr`, a `consteval` specifier implies `inline`.
However, it may not be applied to destructors, allocation functions, or
deallocation functions.

A function or function template declaration specifying `consteval` may not also
specify `constexpr`, and any redeclarations of that function or function
template must also specify `consteval`.

A potentially-evaluated invocation of an immediate function whose innermost
non-block scope is not a function parameter scope of an immediate function or
the true-branch of a consteval if statement(since C++23) must produce a constant
expression; such an invocation is known as an *immediate invocation*.

```cpp
consteval int sqr(int n)
{
    return n*n;
}
constexpr int r = sqr(100); // OK

int x = 100;
int r2 = sqr(x);            // Error: Call does not produce a constant

consteval int sqrsqr(int n)
{
    return sqr(sqr(n));     // Not a constant expression at this point, but OK
}

constexpr int dblsqr(int n)
{
    return 2 * sqr(n);      // Error: Enclosing function is not consteval
                            // and sqr(n) is not a constant
}
```

An identifier expression that denotes an immediate function may only appear
within a subexpression of an immediate invocation or within an *immediate
function context* (i.e. a context mentioned above, in which a call to an
immediate function needs not to be a constant expression). A pointer or
reference to an immediate function can be taken but cannot escape constant
expression evaluation:

```cpp
consteval int f() { return 42; }
consteval auto g() { return &f; }
consteval int h(int (*p)() = g()) { return p(); }
constexpr int r = h();  // OK
constexpr auto e = g(); // ill-formed: a pointer to an immediate function is
                        // not a permitted result of a constant expression
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_consteval` | 201811L | (C++20) | Immediate functions
  202211L | (C++20) (DR) | Making `consteval` propagate up

### Keywords

`consteval`

### Example

```cpp
#include <iostream>

// This function might be evaluated at compile-time, if the input
// is known at compile-time. Otherwise, it is executed at run-time.
constexpr unsigned factorial(unsigned n)
{
    return n < 2 ? 1 : n * factorial(n - 1);
}

// With consteval we enforce that the function will be evaluated at compile-time.
consteval unsigned combination(unsigned m, unsigned n)
{
    return factorial(n) / factorial(m) / factorial(n - m);
}

static_assert(factorial(6) == 720);
static_assert(combination(4, 8) == 70);

int main(int argc, const char*[])
{
    constexpr unsigned x{factorial(4)};
    std::cout << x << '\n';

    [[maybe_unused]]
    unsigned y = factorial(argc); // OK
//  unsigned z = combination(argc, 7); // error: 'argc' is not a constant expression
}
```

Output:

```text
24
```

### See also

- **`constexpr` specifier(C++11)** — specifies that the value of a variable or
  function can be computed at compile time
- **`constinit` specifier(C++20)** — asserts that a variable has static
  initialization, i.e. zero initialization and constant initialization
- **constant expression** — defines an expression that can be evaluated at
  compile time

---
*Source: https://en.cppreference.com/w/cpp/language/consteval*
