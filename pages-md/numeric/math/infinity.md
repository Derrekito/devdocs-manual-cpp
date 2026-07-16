# INFINITY

```cpp
#define INFINITY /*implementation defined*/  // (since C++11)
```

If the implementation supports floating-point infinities, the macro `INFINITY`
expands to constant expression of type `float` which evaluates to positive or
unsigned infinity.

If the implementation does not support floating-point infinities, the macro
`INFINITY` expands to a positive value that is guaranteed to overflow a `float`
at compile time, and the use of this macro generates a compiler warning.

### See also

- **isinf (C++11)** — checks if the given number is infinite (function)
- **HUGE_VALFHUGE_VALHUGE_VALL (C++11)(C++11)** — indicates the overflow value
  for float, double and long double respectively (macro constant)
- **has_infinity [static]** — identifies floating-point types that can represent
  the special value "positive infinity" (public static member constant of
  `std::numeric_limits<T>`)
- **infinity [static]** — returns the positive infinity value of the given
  floating-point type (public static member function of
  `std::numeric_limits<T>`)

**C documentation for `INFINITY`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/INFINITY*
