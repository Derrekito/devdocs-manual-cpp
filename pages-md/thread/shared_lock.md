# std::shared_lock

```cpp
template< class Mutex >
class shared_lock;  // (since C++14)
```

The class `shared_lock` is a general-purpose shared mutex ownership wrapper
allowing deferred locking, timed locking and transfer of lock ownership. Locking
a `shared_lock` locks the associated shared mutex in shared mode (to lock it in
exclusive mode, `std::unique_lock` can be used).

The `shared_lock` class is movable, but not copyable – it meets the requirements
of MoveConstructible and MoveAssignable but not of CopyConstructible or
CopyAssignable.

`shared_lock` meets the Lockable requirements. If `Mutex` meets the
SharedTimedLockable requirements, `shared_lock` also meets TimedLockable
requirements.

In order to wait in a shared mutex in shared ownership mode,
`std::condition_variable_any` can be used (`std::condition_variable` requires
`std::unique_lock` and so can only wait in unique ownership mode).

### Template parameters

- **Mutex** — the type of the shared mutex to lock. The type must meet the
  SharedLockable requirements

### Member types

- **`mutex_type`** — `Mutex`

### Member functions

- **(constructor)** — constructs a `shared_lock`, optionally locking the
  supplied mutex (public member function)
- **(destructor)** — unlocks the associated mutex (public member function)
- **operator=** — unlocks the mutex, if owned, and acquires ownership of another
  (public member function)

**Shared locking**

- **lock** — locks the associated mutex (public member function)
- **try_lock** — tries to lock the associated mutex (public member function)
- **try_lock_for** — tries to lock the associated mutex, for the specified
  duration (public member function)
- **try_lock_until** — tries to lock the associated mutex, until a specified
  time point (public member function)
- **unlock** — unlocks the associated mutex (public member function)

**Modifiers**

- **swap** — swaps the data members with another `shared_lock` (public member
  function)
- **release** — disassociates the mutex without unlocking (public member
  function)

**Observers**

- **mutex** — returns a pointer to the associated mutex (public member function)
- **owns_lock** — tests whether the lock owns its associated mutex (public
  member function)
- **operator bool** — tests whether the lock owns its associated mutex (public
  member function)

### Non-member functions

- **std::swap(std::shared_lock) (C++14)** — specialization of `std::swap` for
  `shared_lock` (function template)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2981 | C++17 | redundant deduction guide from `shared_lock<Mutex>` was
      provided | removed

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock*
