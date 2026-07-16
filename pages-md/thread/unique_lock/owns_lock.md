# std::unique_lock<Mutex>::owns_lock

```cpp
bool owns_lock() const noexcept;  // (since C++11)
```

Checks whether `*this` owns a locked mutex or not.

### Parameters

(none)

### Return value

`true` if `*this` has an associated mutex and has acquired ownership of it,
`false` otherwise.

### See also

- **operator bool** — tests whether the lock owns (i.e., has locked) its
  associated mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/owns_lock*
