# std::notify_all_at_thread_exit

```cpp
void notify_all_at_thread_exit( std::condition_variable& cond,
                                std::unique_lock<std::mutex> lk );  // (since C++11)
```

`notify_all_at_thread_exit` provides a mechanism to notify other threads that a
given thread has completely finished, including destroying all thread_local
objects. It operates as follows:

- Ownership of the previously acquired lock `lk` is transferred to internal
  storage.
- The execution environment is modified such that when the current thread exits,
  the condition variable `cond` is notified as if by:

`lk.unlock();cond.notify_all();`

The implied `lk.unlock()` is *sequenced after* (as defined in
`std::memory_order`) the destruction of all objects with thread local storage
duration associated with the current thread.

An equivalent effect may be achieved with the facilities provided by
`std::promise` or `std::packaged_task`.

### Notes

Calling this function if `lock.mutex()` is not locked by the current thread is
undefined behavior.

Calling this function if `lock.mutex()` is not the same mutex as the one used by
all other threads that are currently waiting on the same condition variable is
undefined behavior.

The supplied lock `lk` is held until the thread exits. Once this function has
been called, no more threads may acquire the same lock in order to wait on
`cond`. If some threads are waiting on this condition variable, ensure that the
condition being waited for is satisfied while holding the lock on `lk`, and that
this lock is not released and reacquired prior to calling
`notify_all_at_thread_exit` to avoid confusion from spurious wakeups in other
threads.

In typical use cases, this function is the last thing called by a detached
thread.

### Parameters

- **cond** — the condition variable to notify at thread exit
- **lk** — the lock associated with the condition variable `cond`

### Return value

(none)

### Example

This partial code fragment illustrates how `notify_all_at_thread_exit` can be
used to avoid accessing data that depends on thread locals while those thread
locals are in the process of being destructed:

```cpp
#include <cassert>
#include <condition_variable>
#include <mutex>
#include <string>
#include <thread>

std::mutex m;
std::condition_variable cv;

bool ready = false;
std::string result; // some arbitrary type

void thread_func()
{
    thread_local std::string thread_local_data = "42";

    std::unique_lock<std::mutex> lk(m);

    // assign a value to result using thread_local data
    result = thread_local_data;
    ready = true;

    std::notify_all_at_thread_exit(cv, std::move(lk));

}   // 1. destroy thread_locals;
    // 2. unlock mutex;
    // 3. notify cv.

int main()
{
    std::thread t(thread_func);
    t.detach();

    // do other work
    // ...

    // wait for the detached thread
    std::unique_lock<std::mutex> lk(m);
    cv.wait(lk, []{ return ready; });

    // result is ready and thread_local destructors have finished, no UB
    assert(result == "42");
}
```

### See also

- **set_value_at_thread_exit** — sets the result to specific value while
  delivering the notification only at thread exit (public member function of
  `std::promise<R>`)
- **make_ready_at_thread_exit** — executes the function ensuring that the result
  is ready only once the current thread exits (public member function of
  `std::packaged_task<R(Args...)>`)

---
*Source: https://en.cppreference.com/w/cpp/thread/notify_all_at_thread_exit*
