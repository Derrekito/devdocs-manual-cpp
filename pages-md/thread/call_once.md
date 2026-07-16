# std::call_once

```cpp
template< class Callable, class... Args >
void call_once( std::once_flag& flag, Callable&& f, Args&&... args );  // (since C++11)
```

Executes the Callable object `f` exactly once, even if called concurrently from
several threads.

In detail:

- If, by the time `std::call_once` is called, `flag` indicates that `f` was
  already called, `std::call_once` returns right away (such a call to
  `std::call_once` is known as *passive*).
- Otherwise, `std::call_once` calls `INVOKE(std::forward<Callable>(f),
  std::forward<Args>(args)...)`. Unlike the `std::thread` constructor or
  `std::async`, the arguments are not moved or copied because they do not need
  to be transferred to another thread of execution (such a call to
  `std::call_once` is known as *active*).

All *active* calls on the same `flag` form a single total order consisting of
zero or more *exceptional* calls, followed by one *returning* call. The end of
each *active* call synchronizes-with the next *active* call in that order.

The return from the *returning* call synchronizes-with the returns from all
*passive* calls on the same `flag`: this means that all concurrent calls to
`std::call_once` are guaranteed to observe any side-effects made by the *active*
call, with no additional synchronization.

### Parameters

- **flag** — an object, for which exactly one function gets executed
- **f** — Callable object to invoke
- **args...** — arguments to pass to the function

### Return value

(none)

### Exceptions

- `std::system_error` if any condition prevents calls to `std::call_once` from
  executing as specified.
- Any exception thrown by `f`.

### Notes

If concurrent calls to `std::call_once` pass different functions `f`, it is
unspecified which `f` will be called. The selected function runs in the same
thread as the `std::call_once` invocation it was passed to.

Initialization of function-local statics is guaranteed to occur only once even
when called from multiple threads, and may be more efficient than the equivalent
code using `std::call_once`.

The POSIX equivalent of this function is `pthread_once`.

### Example

```cpp
#include <iostream>
#include <mutex>
#include <thread>

std::once_flag flag1, flag2;

void simple_do_once()
{
    std::call_once(flag1, [](){ std::cout << "Simple example: called once\n"; });
}

void may_throw_function(bool do_throw)
{
    if (do_throw)
    {
        std::cout << "Throw: call_once will retry\n"; // this may appear more than once
        throw std::exception();
    }
    std::cout << "Did not throw, call_once will not attempt again\n"; // guaranteed once
}

void do_once(bool do_throw)
{
    try
    {
        std::call_once(flag2, may_throw_function, do_throw);
    }
    catch (...) {}
}

int main()
{
    std::thread st1(simple_do_once);
    std::thread st2(simple_do_once);
    std::thread st3(simple_do_once);
    std::thread st4(simple_do_once);
    st1.join();
    st2.join();
    st3.join();
    st4.join();

    std::thread t1(do_once, true);
    std::thread t2(do_once, true);
    std::thread t3(do_once, false);
    std::thread t4(do_once, true);
    t1.join();
    t2.join();
    t3.join();
    t4.join();
}
```

Possible output:

```text
Simple example: called once
Throw: call_once will retry
Throw: call_once will retry
Throw: call_once will retry
Did not throw, call_once will not attempt again
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2080 | C++11 | `std::invalid_argument` would be thrown if `f` is invalid,
      but the scenario where `f` is invalidated is not specified | removed this
      error condition
  LWG 2442 | C++11 | the arguments were copied and/or moved before invocation |
      no copying/moving is performed

### See also

- **once_flag (C++11)** — helper object to ensure that **`call_once`** invokes
  the function only once (class)

**C documentation for `call_once`**

---
*Source: https://en.cppreference.com/w/cpp/thread/call_once*
