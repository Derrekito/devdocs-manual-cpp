# std::condition_variable_any::wait

```cpp
template< class Lock >
void wait( Lock& lock );  // (1) (since C++11)
template< class Lock, class Predicate >
void wait( Lock& lock, Predicate stop_waiting );  // (2) (since C++11)
template< class Lock, class Predicate >
bool wait( Lock& lock, std::stop_token stoken, Predicate stop_waiting );  // (3) (since C++20)
```

`wait` causes the current thread to block until the condition variable is
notified or a spurious wakeup occurs, optionally looping until some predicate is
satisfied (`bool(stop_waiting()) == true`).

1) Atomically unlocks `lock`, blocks the current executing thread, and adds it
   to the list of threads waiting on `*this`. The thread will be unblocked when
   `notify_all()` or `notify_one()` is executed. It may also be unblocked
   spuriously. When unblocked, regardless of the reason, `lock` is reacquired
   and `wait` exits.

2) Equivalent to while (!stop_waiting()) { wait(lock); } This overload may be
   used to ignore spurious awakenings while waiting for a specific condition to
   become true. Note that `lock` must be acquired before entering this method,
   and it is reacquired after `wait(lock)` exits, which means that `lock` can be
   used to guard access to `stop_waiting()`.

3) An interruptible wait: registers the `condition_variable_any` for the
   duration of `wait()`, to be notified if a stop request is made on the given
   `stoken`'s associated stop-state; it is then equivalent to while
   (!stoken.stop_requested()) { if (stop_waiting()) return true; wait(lock); }
   return stop_waiting(); Note that the returned value indicates whether
   `stop_waiting` evaluated to `true`, regardless of whether there was a stop
   requested or not.

If these functions fail to meet the postconditions (`lock` is locked by the
calling thread), `std::terminate` is called. For example, this could happen if
relocking the mutex throws an exception.

### Parameters

- **lock** — an object of type `Lock` that meets the BasicLockable requirements,
  which must be locked by the current thread
- **stoken** — a `std::stop_token` to register interruption for
- **stop_waiting** — predicate which returns ​`false` if the waiting should be
  continued (`bool(stop_waiting()) == false`). The signature of the predicate
  function should be equivalent to the following: `bool pred();`​

### Return value

1,2) (none)

3) `stop_waiting()`

### Exceptions

1) Does not throw.

2) Same as (1) but may also propagate exceptions thrown by `stop_waiting`.

3) Same as (2).

### Notes

The effects of `notify_one()`/`notify_all()` and each of the three atomic parts
of `wait()`/`wait_for()`/`wait_until()` (unlock+wait, wakeup, and lock) take
place in a single total order that can be viewed as modification order of an
atomic variable: the order is specific to this individual condition variable.
This makes it impossible for `notify_one()` to, for example, be delayed and
unblock a thread that started waiting just after the call to `notify_one()` was
made.

### Example

```cpp
#include <chrono>
#include <condition_variable>
#include <iostream>
#include <thread>

std::condition_variable_any cv;
std::mutex cv_m; // This mutex is used for three purposes:
                 // 1) to synchronize accesses to i
                 // 2) to synchronize accesses to std::cerr
                 // 3) for the condition variable cv
int i = 0;

void waits()
{
    std::unique_lock<std::mutex> lk(cv_m);
    std::cerr << "Waiting... \n";
    cv.wait(lk, []{ return i == 1; });
    std::cerr << "...finished waiting. i == 1\n";
}

void signals()
{
    std::this_thread::sleep_for(std::chrono::seconds(1));
    {
        std::lock_guard<std::mutex> lk(cv_m);
        std::cerr << "Notifying...\n";
    }
    cv.notify_all();

    std::this_thread::sleep_for(std::chrono::seconds(1));

    {
        std::lock_guard<std::mutex> lk(cv_m);
        i = 1;
        std::cerr << "Notifying again...\n";
    }
    cv.notify_all();
}

int main()
{
    std::thread t1(waits), t2(waits), t3(waits), t4(signals);
    t1.join();
    t2.join();
    t3.join();
    t4.join();
}
```

Possible output:

```text
Waiting...
Waiting...
Waiting...
Notifying...
Notifying again...
...finished waiting. i == 1
...finished waiting. i == 1
...finished waiting. i == 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2135 | C++11 | `wait` threw an exception on unlocking/relocking failure |
      calls `std::terminate`

### See also

- **wait_for** — blocks the current thread until the condition variable is
  awakened or after the specified timeout duration (public member function)
- **wait_until** — blocks the current thread until the condition variable is
  awakened or until specified time point has been reached (public member
  function)

**C documentation for `cnd_wait`**

### External links

  1. | The Old New Thing article: Spurious wake-ups in Win32 condition
      variables.

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable_any/wait*
