# std::unexpected

```cpp
void unexpected();  // (until C++11)
[[noreturn]] void unexpected();  // (since C++11) (deprecated) (removed in C++17)
```

`std::unexpected()` is called by the C++ runtime when a dynamic exception
specification is violated: an exception is thrown from a function whose
exception specification forbids exceptions of this type.

`std::unexpected()` may also be called directly from the program.

In either case, `std::unexpected` calls the currently installed
`std::unexpected_handler`. The default `std::unexpected_handler` calls
`std::terminate`.

If a destructor reset the unexpected handler during stack unwinding and the
unwinding later led to `unexpected` being called, the handler that was installed
at the end of the throw expression is the one that will be called (note: it was
ambiguous whether re-throwing applied the new handlers).
*(until C++11)*

If a destructor reset the unexpected handler during stack unwinding, it is
unspecified which handler is called if the unwinding later led to `unexpected`
being called.
*(since C++11)*

### Parameters

(none)

### Return value

(none)

### Exceptions

Throw any exception thrown by the currently installed `std::unexpected_handler`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2111 | C++11 | effect of calling `set_unexpected` during stack unwinding
      differs from C++98 and breaks some ABIs | made unspecified

### See also

- **unexpected (C++23)** — represented as an unexpected value (class template)
- **unexpected_handler (removed in C++17)** — the type of the function called by
  `std::unexpected` (typedef)

---
*Source: https://en.cppreference.com/w/cpp/error/unexpected*
