# Standard library header <cstdbool> (C++11)(until C++20), <stdbool.h> (C++11)

This header was originally in the C standard library as `<stdbool.h>`.

Compatibility header. true, false and bool are keywords in C++, but they were
not keywords in C until C23.

**Macros**

- **__bool_true_false_are_defined (C++11)(deprecated)** — C compatibility macro
  constant, expands to integer literal `1` (macro constant)

### Notes

`<cstdbool>` is deprecated in C++17 and removed in C++20. Corresponding
`<stdbool.h>` is still available in C++20.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3827 | C++11 | C programs no longer need the compatibility macro
      `__bool_true_false_are_defined` since C23 | deprecated this macro in C++

---
*Source: https://en.cppreference.com/w/cpp/header/cstdbool*
