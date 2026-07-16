# Standard library header <cstdalign> (C++11)(until C++20), <stdalign.h> (C++11)

This header was originally in the C standard library as `<stdalign.h>`.

C compatibility header. alignas and alignof are keywords in C++, but they were
not keywords in C until C23.

**Macros**

- **__alignas_is_defined (C++11)(deprecated)** — C compatibility macro constant,
  expands to integer literal `1` (macro constant)

### Notes

`<cstdalign>` is deprecated in C++17 and removed in C++20. Corresponding
`<stdalign.h>` is still available in C++20.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3827 | C++11 | C programs no longer need the compatibility macro
      `__alignas_is_defined` since C23 | deprecated this macro in C++

---
*Source: https://en.cppreference.com/w/cpp/header/cstdalign*
