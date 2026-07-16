# std::lock_guard<Mutex>::lock_guard

```cpp
explicit lock_guard( mutex_type& m );  // (1) (since C++11)
lock_guard( mutex_type& m, std::adopt_lock_t t );  // (2) (since C++11)
lock_guard( const lock_guard& ) = delete;  // (3) (since C++11)
```

Acquires ownership of the given mutex `m`.

1) Effectively calls `m.lock()`.

2) Acquires ownership of the mutex `m` without attempting to lock it. The
   behavior is undefined if the current thread does not hold a non-shared lock
   (i.e., a lock acquired by `lock`, `try_lock`, `try_lock_for`, or
   `try_lock_until`) on `m`.

3) Copy constructor is deleted.

The behavior is undefined if `m` is destroyed before the `lock_guard` object is.

### Parameters

- **m** — mutex to acquire ownership of
- **t** — tag parameter used to select non-locking version of the constructor

### Exceptions

1) Throws any exceptions thrown by `m.lock()`.

2) Throws nothing.

---
*Source: https://en.cppreference.com/w/cpp/thread/lock_guard/lock_guard*
