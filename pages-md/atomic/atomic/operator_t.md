# std::atomic<T>::operator T

```cpp
operator T() const noexcept;  // (1) (since C++11)
operator T() const volatile noexcept;  // (2) (since C++11)
```

Atomically loads and returns the current value of the atomic variable.
Equivalent to `load()`.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and
overload (2) participates in overload resolution.
*(since C++20)*

### Parameters

(none)

### Return value

The current value of the atomic variable.

### See also

- **load** — atomically obtains the value of the atomic object (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/operator_T*
