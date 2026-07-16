# std::fegetexceptflag, std::fesetexceptflag

```cpp
int fegetexceptflag( std::fexcept_t* flagp, int excepts );  // (1) (since C++11)
int fesetexceptflag( const std::fexcept_t* flagp, int excepts );  // (2) (since C++11)
```

1) Attempts to obtain the full contents of the floating-point exception flags
that are listed in the bitmask argument `excepts`, which is a bitwise OR of the
floating point exception macros.

2) Attempts to copy the full contents of the floating-point exception flags that
are listed in `excepts` from `flagp` into the floating-point environment. Does
not raise any exceptions, only modifies the flags.

The full contents of a floating-point exception flag is not necessarily a
boolean value indicating whether the exception is raised or cleared. For
example, it may be a struct which includes the boolean status and the address of
the code that triggered the exception. These functions obtain all such content
and obtain/store it in `flagp` in implementation-defined format.

### Parameters

- **flagp** — pointer to an `std::fexcept_t` object where the flags will be
  stored or read from
- **excepts** — bitmask listing the exception flags to get/set

### Return value

`​0​` on success, non-zero otherwise.

### See also

**C documentation for `fegetexceptflag, fesetexceptflag`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/fenv/feexceptflag*
