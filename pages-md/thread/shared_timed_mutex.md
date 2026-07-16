# std::shared_timed_mutex

```cpp
class shared_timed_mutex;  // (since C++14)
```

The `shared_timed_mutex` class is a synchronization primitive that can be used
to protect shared data from being simultaneously accessed by multiple threads.
In contrast to other mutex types which facilitate exclusive access, a
`shared_timed_mutex` has two levels of access:

- *exclusive* - only one thread can own the mutex.
- *shared* - several threads can share ownership of the same mutex.

Shared mutexes are usually used in situations when multiple readers can access
the same resource at the same time without causing data races, but only one
writer can do so.

In a manner similar to `timed_mutex`, `shared_timed_mutex` provides the ability
to attempt to claim ownership of a `shared_timed_mutex` with a timeout via the
`try_lock_for()`, `try_lock_until()`, `try_lock_shared_for()`,
`try_lock_shared_until()` member functions.

The `shared_timed_mutex` class satisfies all requirements of SharedTimedMutex
and StandardLayoutType.

### Member functions

- **(constructor)** — constructs the mutex (public member function)
- **(destructor)** — destroys the mutex (public member function)
- **operator= [deleted]** — not copy-assignable (public member function)

**Exclusive locking**

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

**Shared locking**

- **lock_shared** — locks the mutex for shared ownership, blocks if the mutex is
  not available (public member function)
- **try_lock_shared** — tries to lock the mutex for shared ownership, returns if
  the mutex is not available (public member function)
- **try_lock_shared_for** — tries to lock the mutex for shared ownership,
  returns if the mutex has been unavailable for the specified timeout duration
  (public member function)
- **try_lock_shared_until** — tries to lock the mutex for shared ownership,
  returns if the mutex has been unavailable until specified time point has been
  reached (public member function)
- **unlock_shared** — unlocks the mutex (shared ownership) (public member
  function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_shared_timed_mutex` | 201402L | (C++14) | `std::shared_timed_mutex`

### Example

A copy assignment operator for a class that holds resources that can handle
multiple readers, but only one writer.

```cpp
#include <mutex>
#include <shared_mutex>

class R
{
    mutable std::shared_timed_mutex mut;
    /* data */
public:
    R& operator=(const R& other)
    {
        // requires exclusive ownership to write to *this
        std::unique_lock<std::shared_timed_mutex> lhs(mut, std::defer_lock);
        // requires shared ownership to read from other
        std::shared_lock<std::shared_timed_mutex> rhs(other.mut, std::defer_lock);
        std::lock(lhs, rhs);
        /* assign data */
        return *this;
    }
};

int main()
{
    R r;
}
```

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex*
