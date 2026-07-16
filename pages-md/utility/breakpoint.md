# std::breakpoint

```cpp
void breakpoint() noexcept;  // (since C++26)
```

Unconditional breakpoint: attempts to temporarily halt the execution of the
program and transfer control to the debugger whether or not it's not possible to
determine if the debugger is present. Formally, the behavior of this function is
completely implementation-defined.

### Notes

The intent of this function is allowing for runtime control of breakpoints
beyond what might be available from a debugger while not causing the program to
exit. For example, breaking when an infrequent non-critical condition is
detected, allowing programmatic control with complex runtime sensitive
conditions, breaking on user input to inspect context in interactive programs
without needing to switch to the debugger application, etc.

This function standardizes many similar existing facilities:
`__builtin_debugtrap` from LLVM, `__debugbreak` from Win32 API, debugger_break
from boost.test, `assert(false)`, `_asm { int 3 }` (MSVC) and `asm("int3")`
(GCC/clang) for x86 targets, etc.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_debugging` | 202311L | (C++26) | debugging support library

### Example

### See also

- **is_debugger_present (C++26)** — checks whether a program is running under
  the control of a debugger (function)
- **breakpoint_if_debugging (C++26)** — calls `std::breakpoint` if
  `std::is_debugger_present` returns `true` (function)

### External links

  1. | armKEIL: `__breakpoint` intrinsic
  2. | Portable Snippets: Debugging assertions and traps
  3. | EASTL: `EASTL_DEBUG_BREAK`
  4. | Catch2: `CATCH_TRAP` and `CATCH_BREAK_INTO_DEBUGGER`
  5. | Unreal Engine: `DebugBreak`
  6. | JUCE: `JUCE_BREAK_IN_DEBUGGER`
  7. | Dear ImGui: `IM_DEBUG_BREAK`
  8. | AWS C Common: `aws_debug_break`

---
*Source: https://en.cppreference.com/w/cpp/utility/breakpoint*
