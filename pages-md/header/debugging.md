# Standard library header <debugging> (C++26)

This header is part of the utility library.

**Functions**

- **breakpoint (C++26)** — pauses the running program when called (function)
- **breakpoint_if_debugging (C++26)** — calls `std::breakpoint` if
  `std::is_debugger_present` returns `true` (function)
- **is_debugger_present (C++26)** — checks whether a program is running under
  the control of a debugger (function)

### Synopsis

```cpp
// all freestanding
namespace std {
  // debugging utility
  void breakpoint() noexcept;
  void breakpoint_if_debugging() noexcept;
  bool is_debugger_present() noexcept;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/debugging*
