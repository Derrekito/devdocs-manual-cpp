# std::unique_lock

```cpp
template< class Mutex >
class unique_lock;  // (since C++11)
```

The class `unique_lock` is a general-purpose mutex ownership wrapper allowing
deferred locking, time-constrained attempts at locking, recursive locking,
transfer of lock ownership, and use with condition variables.

The class `unique_lock` is movable, but not copyable -- it meets the
requirements of MoveConstructible and MoveAssignable but not of
CopyConstructible or CopyAssignable.

The class `unique_lock` meets the BasicLockable requirements. If **Mutex** meets
the Lockable requirements, `unique_lock` also meets the Lockable requirements
(ex.: can be used in `std::lock`); if **Mutex** meets the TimedLockable
requirements, `unique_lock` also meets the TimedLockable requirements.

### Template parameters

- **Mutex** — the type of the mutex to lock. The type must meet the
  BasicLockable requirements

### Member types

- **`mutex_type`** — `Mutex`

### Member functions

- **(constructor)** — constructs a `unique_lock`, optionally locking (i.e.,
  taking ownership of) the supplied mutex (public member function)
- **(destructor)** — unlocks (i.e., releases ownership of) the associated mutex,
  if owned (public member function)
- **operator=** — unlocks (i.e., releases ownership of) the mutex, if owned, and
  acquires ownership of another (public member function)

**Locking**

- **lock** — locks (i.e., takes ownership of) the associated mutex (public
  member function)
- **try_lock** — tries to lock (i.e., takes ownership of) the associated mutex
  without blocking (public member function)
- **try_lock_for** — attempts to lock (i.e., takes ownership of) the associated
  TimedLockable mutex, returns if the mutex has been unavailable for the
  specified time duration (public member function)
- **try_lock_until** — tries to lock (i.e., takes ownership of) the associated
  TimedLockable mutex, returns if the mutex has been unavailable until specified
  time point has been reached (public member function)
- **unlock** — unlocks (i.e., releases ownership of) the associated mutex
  (public member function)

**Modifiers**

- **swap** — swaps state with another `std::unique_lock` (public member
  function)
- **release** — disassociates the associated mutex without unlocking (i.e.,
  releasing ownership of) it (public member function)

**Observers**

- **mutex** — returns a pointer to the associated mutex (public member function)
- **owns_lock** — tests whether the lock owns (i.e., has locked) its associated
  mutex (public member function)
- **operator bool** — tests whether the lock owns (i.e., has locked) its
  associated mutex (public member function)

### Non-member functions

- **std::swap(std::unique_lock) (C++11)** — specialization of `std::swap` for
  `unique_lock` (function template)

### Example

```cpp
#include <iostream>
#include <mutex>
#include <thread>

struct Box
{
    explicit Box(int num) : num_things{num} {}

    int num_things;
    std::mutex m;
};

void transfer(Box& from, Box& to, int num)
{
    // don't actually take the locks yet
    std::unique_lock lock1{from.m, std::defer_lock};
    std::unique_lock lock2{to.m, std::defer_lock};

    // lock both unique_locks without deadlock
    std::lock(lock1, lock2);

    from.num_things -= num;
    to.num_things += num;

    // 'from.m' and 'to.m' mutexes unlocked in 'unique_lock' dtors
}

int main()
{
    Box acc1{100};
    Box acc2{50};

    std::thread t1{transfer, std::ref(acc1), std::ref(acc2), 10};
    std::thread t2{transfer, std::ref(acc2), std::ref(acc1), 5};

    t1.join();
    t2.join();

    std::cout << "acc1: " << acc1.num_things << "\n"
                 "acc2: " << acc2.num_things << '\n';
}
```

Output:

```text
acc1: 95
acc2: 55
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2981 | C++17 | redundant deduction guide from `unique_lock<Mutex>` was
      provided | removed

### See also

- **lock (C++11)** — locks specified mutexes, blocks if any are unavailable
  (function template)
- **lock_guard (C++11)** — implements a strictly scope-based mutex ownership
  wrapper (class template)
- **scoped_lock (C++17)** — deadlock-avoiding RAII wrapper for multiple mutexes
  (class template)
- **mutex (C++11)** — provides basic mutual exclusion facility (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock*
