# std::condition_variable_any

```cpp
class condition_variable_any;  // (since C++11)
```

The `condition_variable_any` class is a generalization of
`std::condition_variable`. Whereas `std::condition_variable` works only on
`std::unique_lock<std::mutex>`, `condition_variable_any` can operate on any lock
that meets the BasicLockable requirements.

See `std::condition_variable` for the description of the semantics of condition
variables.

The class `std::condition_variable_any` is a StandardLayoutType. It is not
CopyConstructible, MoveConstructible, CopyAssignable, or MoveAssignable.

If the lock is `std::unique_lock<std::mutex>`, `std::condition_variable` may
provide better performance.

### Member functions

- **(constructor)** — constructs the object (public member function)
- **(destructor)** — destructs the object (public member function)
- **operator= [deleted]** — not copy-assignable (public member function)

**Notification**

- **notify_one** — notifies one waiting thread (public member function)
- **notify_all** — notifies all waiting threads (public member function)

**Waiting**

- **wait** — blocks the current thread until the condition variable is awakened
  (public member function)
- **wait_for** — blocks the current thread until the condition variable is
  awakened or after the specified timeout duration (public member function)
- **wait_until** — blocks the current thread until the condition variable is
  awakened or until specified time point has been reached (public member
  function)

### Notes

`std::condition_variable_any` can be used with `std::shared_lock` in order to
wait on a `std::shared_mutex` in shared ownership mode.

A possible use for `std::condition_variable_any` with custom Lockable types is
to provide convenient interruptible waits: the custom lock operation would both
lock the associated mutex as expected, and also perform the necessary setup to
notify this condition variable when the interrupting signal is received.[1]

### See also

- **condition_variable (C++11)** — provides a condition variable associated with
  a `std::unique_lock` (class)

### External links

  Anthony Williams (2012, 1st ed./ 2019, 2nd ed.), “C++ Concurrency in Action”,
      9.2.4 “Interrupting a wait on `std::condition_variable_any`”.

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable_any*
