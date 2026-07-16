# std::timed_mutex

```cpp
class timed_mutex;  // (since C++11)
```

The `timed_mutex` class is a synchronization primitive that can be used to
protect shared data from being simultaneously accessed by multiple threads.

In a manner similar to `mutex`, `timed_mutex` offers exclusive, non-recursive
ownership semantics. In addition, `timed_mutex` provides the ability to attempt
to claim ownership of a `timed_mutex` with a timeout via the member functions
`try_lock_for()` and `try_lock_until()`.

The `timed_mutex` class satisfies all requirements of TimedMutex and
StandardLayoutType.

### Member types

- **`native_handle_type`(not always present)** — *implementation-defined*

### Member functions

- **(constructor)** — constructs the mutex (public member function)
- **(destructor)** — destroys the mutex (public member function)
- **operator= [deleted]** — not copy-assignable (public member function)

**Locking**

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **try_lock_for** — tries to lock the mutex, returns if the mutex has been
  unavailable for the specified timeout duration (public member function)
- **try_lock_until** — tries to lock the mutex, returns if the mutex has been
  unavailable until specified time point has been reached (public member
  function)
- **unlock** — unlocks the mutex (public member function)

**Native handle**

- **native_handle** — returns the underlying implementation-defined native
  handle object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/timed_mutex*
