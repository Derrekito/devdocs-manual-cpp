# std::is_debugger_present

```cpp
bool is_debugger_present() noexcept;  // (since C++26)
```

Attempts to determine if the program is being executed with debugger present.
Formally, the behavior of this function is completely implementation-defined.

### Return value

`true` if, to the best of its knowledge, the program is executed under a
debugger.

### Notes

The intent of this function is allowing printing out extra output to help
diagnose problems, executing extra test code, displaying an extra user interface
to help in debugging, etc.

This function standardizes many similar existing facilities: `under_debugger`
from boost.test, `IsDebuggerPresent()` from Win32 API, `isDebuggerActive` from
Catch2, `IsDebuggerPresent` from Unreal Engine, etc.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_debugging` | 202311L | (C++26) | debugging support library

### Example

### See also

- **breakpoint (C++26)** — pauses the running program when called (function)
- **breakpoint_if_debugging (C++26)** — calls `std::breakpoint` if
  `std::is_debugger_present` returns `true` (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/is_debugger_present*
