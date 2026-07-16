# ATOMIC_VAR_INIT

```cpp
#define ATOMIC_VAR_INIT(value) /* implementation-defined */  // (since C++11) (deprecated in C++20)
```

Expands to an expression which can be used to initialize an `std::atomic` object
that can be initialized from `value`. If the atomic object has static storage
duration, this initialization is constant initialization.

### Notes

Accessing the variable during initialization from another thread, even through
an atomic operation, is a data race (it may happen if the address is immediately
passed to another thread with a `std::memory_order_relaxed` operation)

This macro is primarily provided for compatibility with C; it behaves the same
as the constructor of `std::atomic`.

### Example

```cpp
#include <atomic>
#include <iostream>

int main()
{
    std::atomic<int> a = ATOMIC_VAR_INIT(1);
    // std::atomic<int> a(1);   // C++-only alternative
    std::cout << "Initialized std::atomic<int> as: " << a << '\n';
}
```

Output:

```text
Initialized std::atomic<int> as: 1
```

### See also

- **atomic_init (C++11)(deprecated in C++20)** — non-atomic initialization of a
  default-constructed atomic object (function template)
- **(constructor)** — constructs an atomic object (public member function of
  `std::atomic<T>`)

**C documentation for `ATOMIC_VAR_INIT`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/ATOMIC_VAR_INIT*
