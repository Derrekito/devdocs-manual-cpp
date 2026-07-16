# Standard library header <csetjmp>

This header was originally in the C standard library as `<setjmp.h>`.

This header is part of the program support library.

**Types**

- **jmp_buf** — execution context type (typedef)

**Macros**

- **setjmp** — saves the context (function macro)

**Functions**

- **longjmp** — jumps to specified location (function)

### Synopsis

```cpp
namespace std {
  using jmp_buf = /* see description */ ;
  [[noreturn]] void longjmp(jmp_buf env, int val);
}
#define setjmp(env) /* see description */
```

---
*Source: https://en.cppreference.com/w/cpp/header/csetjmp*
