# std::atomic<T>::is_always_lock_free

```cpp
static constexpr bool is_always_lock_free = /*implementation-defined*/;  // (since C++17)
```

Equals `true` if this atomic type is always lock-free and `false` if it is never
or sometimes lock-free.

The value of this constant is consistent with both the macro
`ATOMIC_xxx_LOCK_FREE`, where defined, with the member function `is_lock_free`
and non-member function `std::atomic_is_lock_free`.

### Notes

There is no non-member function equivalent of this static member constant
because non-member functions take pointers to atomic types, and therefore aren't
as useful in constant expressions.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_atomic_is_always_lock_free` | 201603L | (C++17) | `constexpr
      atomic<T>::is_always_lock_free`

### See also

- **is_lock_free** — checks if the atomic object is lock-free (public member
  function)
- **atomic_is_lock_free (C++11)** — checks if the atomic type's operations are
  lock-free (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/is_always_lock_free*
