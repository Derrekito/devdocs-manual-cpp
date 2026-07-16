# C++ named requirements: TimedMutex (since C++11)

The **TimedMutex** requirements extend the TimedLockable requirements to include
inter-thread synchronization.

### Requirements

- TimedLockable
- Mutex

Additionally, for an object `m` of TimedMutex type:

- The expression `m.try_lock_for(duration)` has the following properties
- The expression `m.try_lock_until(time_point)` has the following properties

### Library types

The following standard library types satisfy **TimedMutex**:

- `std::timed_mutex`
- `std::recursive_timed_mutex`
- `std::shared_timed_mutex`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2093 | C++11 | timeout-related exceptions were missing in the
      specification | mentioned

### See also

- Thread support library
- TimedLockable
- Mutex

---
*Source: https://en.cppreference.com/w/cpp/named_req/TimedMutex*
