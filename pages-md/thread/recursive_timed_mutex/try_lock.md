# std::recursive_timed_mutex::try_lock

```cpp
bool try_lock() noexcept;  // (since C++11)
```

Tries to lock the mutex. Returns immediately. On successful lock acquisition
returns `true`, otherwise returns `false`.

This function is allowed to fail spuriously and return `false` even if the mutex
is not currently locked by any other thread.

A thread may call `try_lock` on a recursive mutex repeatedly. Successful calls
to `try_lock` increment the ownership count: the mutex will only be released
after the thread makes a matching number of calls to `unlock`.

The maximum number of levels of ownership is unspecified. A call to `try_lock`
will return `false` if this number is exceeded.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation if it returns `true`. Note that prior
`lock()` does not synchronize with this operation if it returns `false`.

### Parameters

(none)

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Throws nothing.

### Example

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock_for** — tries to lock the mutex, returns if the mutex has been
  unavailable for the specified timeout duration (public member function)
- **try_lock_until** — tries to lock the mutex, returns if the mutex has been
  unavailable until specified time point has been reached (public member
  function)
- **unlock** — unlocks the mutex (public member function)

**C documentation for `mtx_trylock`**

---
*Source: https://en.cppreference.com/w/cpp/thread/recursive_timed_mutex/try_lock*
