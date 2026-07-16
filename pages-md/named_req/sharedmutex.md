# C++ named requirements: SharedMutex (since C++17)

The **SharedMutex** requirements extend the Mutex requirements to include shared
lock ownership mode.

### Requirements

- Mutex

Additionally, an object `m` of SharedMutex type supports another mode of
ownership: shared. Multiple threads (or, more generally, execution agents) can
simultaneously own this mutex in shared mode, but no thread may obtain shared
ownership if there is a thread that owns it in exclusive mode and no thread may
obtain exclusive ownership if there is a thread that owns it in shared mode. If
more than implementation-defined number of threads (no less than 10000) hold a
shared lock, another attempt to acquire the mutex in shared mode blocks until
the number of shared owners drops down below that threshold.

- The expression `m.lock_shared()` has the following properties:
- The expression `m.try_lock_shared()` has the following properties:
- The expression `m.unlock_shared()` has the following properties:
- All lock and unlock operations on a single mutex occur in a single total
  order.

### Library types

The following standard library types satisfy **SharedMutex**:

- `std::shared_mutex`(since C++17)
- `std::shared_timed_mutex`(since C++14)

### See also

- Thread support library
- Mutex
- TimedMutex
- SharedTimedMutex

---
*Source: https://en.cppreference.com/w/cpp/named_req/SharedMutex*
