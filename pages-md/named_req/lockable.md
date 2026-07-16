# C++ named requirements: Lockable (since C++11)

The **Lockable** requirements extends the BasicLockable requirements to include
attempted locking.

### Requirements

- BasicLockable

For type `L` to be Lockable, it must meet the above condition as well as the
following:

  Expression | Effects | Return value
  `m.try_lock()` | Attempts to acquire the lock for the current execution agent
      (thread, process, task) without blocking. If an exception is thrown, no
      lock is obtained. | `true` if the lock was acquired, `false` otherwise

### Notes

The `try_lock` member functions obtains a non-shared lock on `m` on succcess.

### See also

- Thread support library
- Mutex
- BasicLockable
- TimedLockable

---
*Source: https://en.cppreference.com/w/cpp/named_req/Lockable*
