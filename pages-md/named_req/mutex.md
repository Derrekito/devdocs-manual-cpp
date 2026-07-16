# C++ named requirements: Mutex (since C++11)

The **Mutex** requirements extends the Lockable requirements to include
inter-thread synchronization.

### Requirements

- Lockable
- DefaultConstructible
- Destructible
- not copyable
- not movable

For an object `m` of Mutex type:

- The expression `m.lock()` has the following properties
- Behaves as an atomic operation.
- Blocks the calling thread until exclusive ownership of the mutex can be
  obtained.
- Prior `m.unlock()` operations on the same mutex *synchronize-with* this lock
  operation (equivalent to release-acquire `std::memory_order`).
- The behavior is undefined if the calling thread already owns the mutex (except
  if m is `std::recursive_mutex` or `std::recursive_timed_mutex`).
- Exception of type `std::system_error` may be thrown on errors, with the
  following error codes:
- The expression `m.try_lock()` has the following properties
- The expression `m.unlock()` has the following properties
- All lock and unlock operations on a single mutex occur in a single total order
  that can be viewed as modification order of an atomic variable: the order is
  specific to this individual mutex.

### Library types

The following standard library types satisfy **Mutex**:

- `std::mutex`
- `std::recursive_mutex`
- `std::timed_mutex`
- `std::recursive_timed_mutex`
- `std::shared_mutex`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2309 | C++11 | `lock` might throw `std::system_error` with error code
      `std::errc::device_or_resource_busy` | not allowed

### See also

- Thread support library
- Lockable
- TimedMutex

---
*Source: https://en.cppreference.com/w/cpp/named_req/Mutex*
