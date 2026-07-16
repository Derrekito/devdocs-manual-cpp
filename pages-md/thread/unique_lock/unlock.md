# std::unique_lock<Mutex>::unlock

```cpp
void unlock();  // (since C++11)
```

Unlocks (i.e., releases ownership of) the associated mutex and releases
ownership.

`std::system_error` is thrown if there is no associated mutex or if the mutex is
not locked.

### Parameters

(none)

### Return value

(none)

### Exceptions

- Any exceptions thrown by `mutex()->unlock()`.
- If there is no associated mutex or the mutex is not locked,
  `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.

### Example

### See also

- **lock** — locks (i.e., takes ownership of) the associated mutex (public
  member function)
- **release** — disassociates the associated mutex without unlocking (i.e.,
  releasing ownership of) it (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/unlock*
