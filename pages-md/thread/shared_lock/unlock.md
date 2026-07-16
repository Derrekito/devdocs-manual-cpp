# std::shared_lock<Mutex>::unlock

```cpp
void unlock();  // (since C++14)
```

Unlocks the associated mutex from shared mode. Effectively calls
`mutex()->unlock_shared()`.

`std::system_error` is thrown if there is no associated mutex or if the mutex is
not locked.

### Parameters

(none)

### Return value

(none)

### Exceptions

- Any exceptions thrown by `mutex()->unlock_shared()`.
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.

### Example

### See also

- **lock** — locks the associated mutex (public member function)
- **release** — disassociates the mutex without unlocking (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/unlock*
