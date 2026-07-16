# std::condition_variable_any::wait_for

```cpp
template< class Lock, class Rep, class Period >
std::cv_status wait_for( Lock& lock,
                         const std::chrono::duration<Rep, Period>& rel_time );  // (1) (since C++11)
template< class Lock, class Rep, class Period, class Predicate >
bool wait_for( Lock& lock,
               const std::chrono::duration<Rep, Period>& rel_time,
               Predicate stop_waiting );  // (2) (since C++11)
template< class Lock, class Rep, class Period, class Predicate >
bool wait_for( Lock& lock,
               std::stop_token stoken,
               const std::chrono::duration<Rep, Period>& rel_time,
               Predicate stop_waiting );  // (3) (since C++20)
```

1) Atomically releases `lock`, blocks the current executing thread, and adds it
   to the list of threads waiting on `*this`. The thread will be unblocked when
   `notify_all()` or `notify_one()` is executed, or when the relative timeout
   `rel_time` expires. It may also be unblocked spuriously. When unblocked,
   regardless of the reason, `lock` is reacquired and `wait_for()` exits.

2) Equivalent to `return wait_until(lock, std::chrono::steady_clock::now() +
   rel_time, std::move(stop_waiting));`. This overload may be used to ignore
   spurious awakenings by looping until some predicate is satisfied
   (`bool(stop_waiting()) == true`).

3) Equivalent to `return wait_until(lock, std::move(stoken),
   std::chrono::steady_clock::now() + rel_time, std::move(stop_waiting));`.

The standard recommends that a steady clock be used to measure the duration.
This function may block for longer than `timeout_duration` due to scheduling or
resource contention delays.

If these functions fail to meet the postcondition (`lock` is locked by the
calling thread), `std::terminate` is called. For example, this could happen if
relocking the mutex throws an exception.

### Parameters

- **lock** — an object of type `Lock` that meets the BasicLockable requirements,
  which must be locked by the current thread
- **stoken** — a `std::stop_token` to register interruption for
- **rel_time** — an object of type `std::chrono::duration` representing the
  maximum time to spend waiting. Note that rel_time must be small enough not to
  overflow when added to `std::chrono::steady_clock::now()`.
- **stop_waiting** — predicate which returns ​`false` if the waiting should be
  continued (`bool(stop_waiting()) == false`). The signature of the predicate
  function should be equivalent to the following: `bool pred();`​

### Return value

1) `std::cv_status::timeout` if the relative timeout specified by `rel_time`
   expired, `std::cv_status::no_timeout` otherwise.

2) `false` if the predicate `stop_waiting` still evaluates to `false` after the
   `rel_time` timeout expired, otherwise `true`.

3) `stop_waiting()`, regardless of whether the timeout was met or stop was
   requested.

### Exceptions

1) Any exception thrown by clock, time_point, or duration during the execution
   (clocks, time points, and durations provided by the standard library never
   throw).

2) Same as (1) but may also propagate exceptions thrown by `stop_waiting`.

3) Same as (2).

### Notes

Even if notified under lock, overload (1) makes no guarantees about the state of
the associated predicate when returning due to timeout.

The effects of `notify_one()`/`notify_all()` and each of the three atomic parts
of `wait()`/`wait_for()`/`wait_until()` (unlock+wait, wakeup, and lock) take
place in a single total order that can be viewed as modification order of an
atomic variable: the order is specific to this individual condition variable.
This makes it impossible for `notify_one()` to, for example, be delayed and
unblock a thread that started waiting just after the call to `notify_one()` was
made.

### Example

```cpp
#include <atomic>
#include <chrono>
#include <condition_variable>
#include <iostream>
#include <thread>
using namespace std::chrono_literals;

std::condition_variable_any cv;
std::mutex cv_m;
int i;

void waits(int idx)
{
    std::unique_lock<std::mutex> lk(cv_m);
    if (cv.wait_for(lk, idx*100ms, []{ return i == 1; }))
        std::cerr << "Thread " << idx << " finished waiting. i == " << i << '\n';
    else
        std::cerr << "Thread " << idx << " timed out. i == " << i << '\n';
}

void signals()
{
    std::this_thread::sleep_for(120ms);
    std::cerr << "Notifying...\n";
    cv.notify_all();
    std::this_thread::sleep_for(100ms);
    {
        std::lock_guard<std::mutex> lk(cv_m);
        i = 1;
    }
    std::cerr << "Notifying again...\n";
    cv.notify_all();
}

int main()
{
    std::thread t1(waits, 1), t2(waits, 2), t3(waits, 3), t4(signals);
    t1.join();
    t2.join();
    t3.join();
    t4.join();
}
```

Output:

```text
Thread 1 timed out. i == 0
Notifying...
Thread 2 timed out. i == 0
Notifying again...
Thread 3 finished waiting. i == 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2093 | C++11 | timeout-related exceptions were missing in the
      specification | mentioned
  LWG 2135 | C++11 | `wait_for` threw an exception on unlocking/relocking
      failure | calls `std::terminate`

### See also

- **wait** — blocks the current thread until the condition variable is awakened
  (public member function)
- **wait_until** — blocks the current thread until the condition variable is
  awakened or until specified time point has been reached (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable_any/wait_for*
