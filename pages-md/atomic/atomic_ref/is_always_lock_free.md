# std::atomic_ref<T>::is_always_lock_free

```cpp
static constexpr bool is_always_lock_free = /*implementation-defined*/;  // (since C++20)
```

Equals `true` if the operations on this `atomic_ref` type is always lock-free
and `false` if it is never or sometimes lock-free.

The value of this constant is consistent with the result of member function
`is_lock_free`.

### See also

- **is_lock_free** — checks if the `atomic_ref` object is lock-free (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/is_always_lock_free*
