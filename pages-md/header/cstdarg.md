# Standard library header <cstdarg>

This header was originally in the C standard library as `<stdarg.h>`.

This header provides support for C-style variadic functions, while the C
definition of "default argument promotions" is replaced with the C++ definition.

**Types**

- **va_list** — holds the information needed by `va_start`, `va_arg`, `va_end`,
  and `va_copy` (typedef)

**Macros**

- **va_start** — enables access to variadic function arguments (function macro)
- **va_arg** — accesses the next variadic function argument (function macro)
- **va_copy (C++11)** — makes a copy of the variadic function arguments
  (function macro)
- **va_end** — ends traversal of the variadic function arguments (function
  macro)

### Synopsis

```cpp
namespace std {
  using va_list = /*see description*/ ;
}
#define va_arg(V, P) /*see description*/
#define va_copy(VDST, VSRC) /*see description*/
#define va_end(V) /*see description*/
#define va_start(V, P) /*see description*/
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2645 | C++98 | C++ defines "default argument promotions", but its C
      definition was used | the C++ definition overrides the C definition

---
*Source: https://en.cppreference.com/w/cpp/header/cstdarg*
