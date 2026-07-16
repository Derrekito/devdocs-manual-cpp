# std::breakpoint_if_debugging

```cpp
void breakpoint_if_debugging() noexcept;  // (since C++26)
```

Conditional breakpoint: attempts to temporarily halt the execution of the
program and transfer control to the debugger if it were able to determine that
the debugger is present. Acts as a no-op otherwise. Formally, the behavior of
this function is completely implementation-defined.

Equivalent to

```cpp
if (std::is_debugger_present())
    std::breakpoint();
```

### Parameters

(none)

### Return value

(none)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_debugging` | 202311L | (C++26) | debugging support library

### Example

### See also

- **is_debugger_present (C++26)** — checks whether a program is running under
  the control of a debugger (function)
- **breakpoint (C++26)** — pauses the running program when called (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/breakpoint_if_debugging*
