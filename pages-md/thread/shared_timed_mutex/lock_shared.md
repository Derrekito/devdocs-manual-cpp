# std::shared_timed_mutex::lock_shared

```cpp
void lock_shared();  // (since C++14)
```

Acquires shared ownership of the mutex. If another thread is holding the mutex
in exclusive ownership, a call to `lock_shared` will block execution until
shared ownership can be acquired.

If `lock_shared` is called by a thread that already owns the `mutex` in any mode
(exclusive or shared), the behavior is undefined.

If more than the implementation-defined maximum number of shared owners already
locked the mutex in shared mode, `lock_shared` blocks execution until the number
of shared owners is reduced. The maximum number of owners is guaranteed to be at
least 10000.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws `std::system_error` when errors occur, including errors from the
underlying operating system that would prevent `lock` from meeting its
specifications. The mutex is not locked in the case of any exception being
thrown.

### Notes

`lock_shared()` is usually not called directly: `std::shared_lock` is used to
manage shared locking.

### Example

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock_shared** — tries to lock the mutex for shared ownership, returns if
  the mutex is not available (public member function)
- **unlock_shared** — unlocks the mutex (shared ownership) (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex/lock_shared*
