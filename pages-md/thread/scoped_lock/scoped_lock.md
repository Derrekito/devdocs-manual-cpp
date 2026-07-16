# std::scoped_lock<MutexTypes...>::scoped_lock

```cpp
explicit scoped_lock( MutexTypes&... m );  // (1) (since C++17)
scoped_lock( std::adopt_lock_t, MutexTypes&... m );  // (2) (since C++17)
scoped_lock( const scoped_lock& ) = delete;  // (3) (since C++17)
```

Acquires ownership of the given mutexes `m`.

1) If `sizeof...(MutexTypes) == 0`, does nothing. Otherwise, if
   `sizeof...(MutexTypes) == 1`, effectively calls `m.lock()`. Otherwise,
   effectively calls `std::lock(m...)`.

2) Acquires ownership of the mutexes `m...` without attempting to lock any of
   them. The behavior is undefined unless the current thread holds a non-shared
   lock (i.e., a lock acquired by `lock`, `try_lock`, `try_lock_for`, or
   `try_lock_until`) on each object in `m...`.

3) Copy constructor is deleted.

The behavior is undefined if `m` is destroyed before the `scoped_lock` object
is.

### Parameters

- **m** — mutexes to acquire ownership of

### Exceptions

1) Throws any exceptions thrown by `m.lock()`.

2) Throws nothing.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0739R0 | C++17 | `adopt_lock_t` parameter was last, impeding class template
      argument deduction | moved to first

---
*Source: https://en.cppreference.com/w/cpp/thread/scoped_lock/scoped_lock*
