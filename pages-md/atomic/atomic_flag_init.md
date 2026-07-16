# ATOMIC_FLAG_INIT

```cpp
#define ATOMIC_FLAG_INIT /* implementation-defined */  // (since C++11)
```

Defines the initializer which can be used to initialize `std::atomic_flag` to
clear (false) state with the statement `std::atomic_flag v = ATOMIC_FLAG_INIT;`.
It is unspecified if it can be used with other initialization contexts.

If the flag has static storage duration, this initialization is static.

This is the only way to initialize `std::atomic_flag` to a definite value: the
value held after any other initialization is unspecified.
*(until C++20)*

This macro is no longer needed since default constructor of `std::atomic_flag`
initializes it to clear state. It is kept for the compatibility with C.
*(since C++20)*

### Example

```cpp
#include <atomic>

std::atomic_flag static_flag = ATOMIC_FLAG_INIT; // static initialization,
// guaranteed to be available during dynamic initialization of static objects.

int main()
{
    std::atomic_flag automatic_flag = ATOMIC_FLAG_INIT; // guaranteed to work
//    std::atomic_flag another_flag(ATOMIC_FLAG_INIT); // unspecified
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3659 | C++20 | `ATOMIC_FLAG_INIT` was deprecated, but needed in C on some
      platforms | it is undeprecated

### See also

- **atomic_flag (C++11)** — the lock-free boolean atomic type (class)

**C documentation for `ATOMIC_FLAG_INIT`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/ATOMIC_FLAG_INIT*
