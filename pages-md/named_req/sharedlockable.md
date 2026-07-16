# C++ named requirements: SharedLockable (since C++14)

The **SharedLockable** requirements describe the minimal characteristics of
types that provide shared blocking semantics for execution agents (i.e.
threads).

### Requirements

For type `L` to be SharedLockable, the following conditions have to be satisfied
for an object `m` of type `L`:

  Expression | Preconditions | Effects | Return value
  `m.lock_shared()` | Blocks until a lock can be obtained for the current
      execution agent (thread, process, task). If an exception is thrown, no
      lock is obtained.
  `m.try_lock_shared()` | Attempts to obtain a lock for the current execution
      agent (thread, process, task) without blocking. If an exception is thrown,
      no lock is obtained. | `true` if the lock was obtained, `false` otherwise
  `m.unlock_shared()` | The current execution agent holds a shared lock on `m`.
      | Releases the shared lock held by the execution agent. Throws no
      exceptions.

#### Shared locks

A lock on an object is said to be *shared lock* if it is acquired by a call to
`lock_shared`, `try_lock_shared`, `try_lock_shared_for`, or
`try_lock_shared_until` member function.

### See also

- Thread support library
- SharedTimedLockable
- SharedMutex

---
*Source: https://en.cppreference.com/w/cpp/named_req/SharedLockable*
