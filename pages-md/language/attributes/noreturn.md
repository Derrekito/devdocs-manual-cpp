# C++ attribute: noreturn (since C++11)

Indicates that the function does not return.

### Syntax

```text
[[noreturn]]
```

### Explanation

Indicates that the function will not return control flow to the calling function
after it finishes (e.g. functions that terminate the application, throw
exceptions, loop indefinitely, etc.).

This attribute applies to the name of the function being declared in function
declarations only. The behavior is undefined if the function with this attribute
actually returns.

The first declaration of the function must specify this attribute if any
declaration specifies it. If a function is declared with `[[noreturn]]` in one
translation unit, and the same function is declared without `[[noreturn]]` in
another translation unit, the program is ill-formed; no diagnostic required.

### Example

```cpp
[[noreturn]] void f()
{
    throw "error";
    // OK
}

void q [[noreturn]] (int i)
{
    // behavior is undefined if called with an argument <= 0
    if (i > 0)
        throw "positive";
}

// void h() [[noreturn]]; // error: attribute applied to function type of h, not h itself

int main()
{
    try { f(); } catch(...) {}
    try { q(42); } catch(...) {}
}
```

### Standard library

The following standard functions are declared with `noreturn` attribute:

**Terminating functions**

- **_Exit (C++11)** — causes normal program termination without cleaning up
  (function)
- **abort** — causes abnormal program termination (without cleaning up)
  (function)
- **exit** — causes normal program termination with cleaning up (function)
- **quick_exit (C++11)** — causes quick program termination without completely
  cleaning up (function)
- **terminate** — function called when exception handling fails (function)
- **unexpected (removed in C++17)** — function called when dynamic exception
  specification is violated (function)

**Compiler hints**

- **unreachable (C++23)** — marks unreachable point of execution (function)

**Always-throwing functions**

- **rethrow_exception (C++11)** — throws the exception from an
  `std::exception_ptr` (function)
- **rethrow_nested** — throws the stored exception (public member function of
  `std::nested_exception`)
- **throw_with_nested (C++11)** — throws its argument with
  `std::nested_exception` mixed in (function template)

**Non-local jumps (since C++17)**

- **longjmp** — jumps to specified location (function)

### See also

**C documentation for `_Noreturn`**

**C documentation for `[[noreturn]]`**

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/noreturn*
