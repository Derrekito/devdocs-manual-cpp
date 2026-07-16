# C++ named requirements: SharedTimedMutex (since C++14)

The **SharedTimedMutex** requirements extend the TimedMutex requirements to
include shared lock ownership mode.

### Requirements

- TimedMutex
- SharedMutex

Additionally, an object `m` of SharedTimedMutex type supports timed shared
operations:

- The expression `m.try_lock_shared_for(duration)` has the following properties
- The expression `m.try_lock_shared_until(time_point)` has the following
  properties

### Library types

The following standard library types satisfy **SharedTimedMutex**:

- `std::shared_timed_mutex`

### See also

- Thread support library
- Mutex
- TimedMutex
- SharedMutex

---
*Source: https://en.cppreference.com/w/cpp/named_req/SharedTimedMutex*
