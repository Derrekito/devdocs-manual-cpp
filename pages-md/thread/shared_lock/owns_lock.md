# std::shared_lock<Mutex>::owns_lock

```cpp
bool owns_lock() const noexcept;  // (since C++14)
```

Checks whether `*this` owns a locked mutex or not.

### Parameters

(none)

### Return value

`true` if `*this` has an associated mutex and has acquired shared ownership of
it, `false` otherwise.

### See also

- **operator bool** — tests whether the lock owns its associated mutex (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/owns_lock*
