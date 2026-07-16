# HUGE_VALF, HUGE_VAL, HUGE_VALL

```cpp
#define HUGE_VALF /* implementation defined */  // (since C++11)
#define HUGE_VAL  /* implementation defined */
#define HUGE_VALL /* implementation defined */  // (since C++11)
```

The macros `HUGE_VALF`, `HUGE_VAL` and `HUGE_VALL` macros expand to positive
floating point constant expressions which compare equal to the values returned
by floating-point functions and operators in case of overflow (see
`math_errhandling`).

- **`HUGE_VALF`** — Expands to positive float expression that indicates overflow
- **`HUGE_VAL`** — Expands to positive double expression that indicates
  overflow, not necessarily representable as a float
- **`HUGE_VALL`** — Expands to positive long double expression that indicates
  overflow, not necessarily representable as a float or double

On implementations that support floating-point infinities, these macros always
expand to the positive infinities of float, double, and long double,
respectively.

### Notes

On implementations that do not support floating-point infinities, these macros
expand to the maximum finite number of their respective types.

C++98 added float and long double overloads of mathematical functions. There is
a problem that the float overloads cannot return `HUGE_VAL` to indicate overflow
because this macro is not guaranteed to be representable as a float.

LWG issue 357 was raised to target this problem. LWG found that C99 has the same
problem (float and long double overloads were also added in C99), and C99
introduced new macros `HUGE_VALF` and `HUGE_VALL` to solve the problem.
Therefore the issue was closed and the C99 resolution was adopted in C++11.

### See also

- **INFINITY (C++11)** — evaluates to positive infinity or the value guaranteed
  to overflow a float (macro constant)

**C documentation for `HUGE_VAL`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/math/HUGE_VAL*
