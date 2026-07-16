# C++ named requirements: SharedTimedLockable (since C++14)

The **SharedTimedLockable** requirements describe the characteristics of types
that provide timed shared blocking semantics for execution agents (threads,
processes, tasks).

### Requirements

- SharedLockable

For type `L` to be SharedTimedLockable, given

- `rel_time`, a value of a specialization of `std::chrono::duration`, and
- `abs_time`, a value of a specialization of `std::chrono::time_point`,

the following conditions have to be satisfied for an object `m` of type `L`:

  Expression | Requires | Return value
  `m.try_lock_shared_for(rel_time)` | Blocks for the provided duration
      `rel_time` or until a lock on `m` is acquired. | `true` if the lock was
      acquired, `false` otherwise.
  `m.try_lock_shared_until(abs_time)` | Blocks until the provided time point
      `abs_time` is reached or a lock on `m` is acquired. | `true` if the lock
      was acquired, `false` otherwise.

### Notes

The `try_lock_shared_for` and `try_lock_shared_until` member functions obtain a
shared lock on `m` on succcess.

### See also

- Thread support library
- SharedTimedMutex
- SharedLockable

---
*Source: https://en.cppreference.com/w/cpp/named_req/SharedTimedLockable*
