# std::unique_lock<Mutex>::operator bool

```cpp
explicit operator bool() const noexcept;  // (since C++11)
```

Checks whether `*this` owns a locked mutex or not. Effectively calls
`owns_lock()`.

### Parameters

(none)

### Return value

`true` if `*this` has an associated mutex and has acquired ownership of it,
`false` otherwise.

### See also

- **owns_lock** — tests whether the lock owns (i.e., has locked) its associated
  mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/operator_bool*
