# std::atomic_ref<T>::operator T

```cpp
operator T() const noexcept;  // (since C++20)
```

Atomically loads and returns the current value of the referenced object.
Equivalent to `load()`.

### Parameters

(none)

### Return value

The current value of the referenced object.

### See also

- **load** — atomically obtains the value of the referenced object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/operator_T*
