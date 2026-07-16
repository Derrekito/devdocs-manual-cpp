# std::condition_variable

```cpp
class condition_variable;  // (since C++11)
```

The `condition_variable` class is a synchronization primitive used with a
`std::mutex` to block one or more threads until another thread both modifies a
shared variable (the *condition*) and notifies the `condition_variable`.

The thread that intends to modify the shared variable must:

1. Acquire a `std::mutex` (typically via `std::lock_guard`).
1. Modify the shared variable while the lock is owned.
1. Call `notify_one` or `notify_all` on the `std::condition_variable` (can be
   done after releasing the lock).

Even if the shared variable is atomic, it must be modified while owning the
mutex to correctly publish the modification to the waiting thread.

Any thread that intends to wait on a `std::condition_variable` must:

1. Acquire a `std::unique_lock<std::mutex>` on the mutex used to protect the
   shared variable.
1. Do one of the following:

1. Check the condition, in case it was already updated and notified.
1. Call `wait`, `wait_for`, or `wait_until` on the `std::condition_variable`
   (atomically releases the mutex and suspends thread execution until the
   condition variable is notified, a timeout expires, or a spurious wakeup
   occurs, then atomically acquires the mutex before returning).
1. Check the condition and resume waiting if not satisfied.

or:

1. Use the predicated overload of `wait`, `wait_for`, and `wait_until`, which
   performs the same three steps.

`std::condition_variable` works only with `std::unique_lock<std::mutex>`, which
allows for maximal efficiency on some platforms. `std::condition_variable_any`
provides a condition variable that works with any BasicLockable object, such as
`std::shared_lock`.

Condition variables permit concurrent invocation of the `wait`, `wait_for`,
`wait_until`, `notify_one` and `notify_all` member functions.

The class `std::condition_variable` is a StandardLayoutType. It is not
CopyConstructible, MoveConstructible, CopyAssignable, or MoveAssignable.

### Member types

- **`native_handle_type`** — *implementation-defined*

### Member functions

- **(constructor)** — constructs the object (public member function)
- **(destructor)** — destructs the object (public member function)
- **operator= [deleted]** — not copy-assignable (public member function)

**Notification**

- **notify_one** — notifies one waiting thread (public member function)
- **notify_all** — notifies all waiting threads (public member function)

**Waiting**

- **wait** — blocks the current thread until the condition variable is awakened
  (public member function)
- **wait_for** — blocks the current thread until the condition variable is
  awakened or after the specified timeout duration (public member function)
- **wait_until** — blocks the current thread until the condition variable is
  awakened or until specified time point has been reached (public member
  function)

**Native handle**

- **native_handle** — returns the native handle (public member function)

### Example

`condition_variable` is used in combination with a `std::mutex` to facilitate
inter-thread communication.

```cpp
#include <condition_variable>
#include <iostream>
#include <mutex>
#include <string>
#include <thread>

std::mutex m;
std::condition_variable cv;
std::string data;
bool ready = false;
bool processed = false;

void worker_thread()
{
    // Wait until main() sends data
    std::unique_lock lk(m);
    cv.wait(lk, []{ return ready; });

    // after the wait, we own the lock.
    std::cout << "Worker thread is processing data\n";
    data += " after processing";

    // Send data back to main()
    processed = true;
    std::cout << "Worker thread signals data processing completed\n";

    // Manual unlocking is done before notifying, to avoid waking up
    // the waiting thread only to block again (see notify_one for details)
    lk.unlock();
    cv.notify_one();
}

int main()
{
    std::thread worker(worker_thread);

    data = "Example data";
    // send data to the worker thread
    {
        std::lock_guard lk(m);
        ready = true;
        std::cout << "main() signals data ready for processing\n";
    }
    cv.notify_one();

    // wait for the worker
    {
        std::unique_lock lk(m);
        cv.wait(lk, []{ return processed; });
    }
    std::cout << "Back in main(), data = " << data << '\n';

    worker.join();
}
```

Output:

```text
main() signals data ready for processing
Worker thread is processing data
Worker thread signals data processing completed
Back in main(), data = Example data after processing
```

### See also

- **condition_variable_any (C++11)** — provides a condition variable associated
  with any lock type (class)
- **mutex (C++11)** — provides basic mutual exclusion facility (class)
- **lock_guard (C++11)** — implements a strictly scope-based mutex ownership
  wrapper (class template)
- **unique_lock (C++11)** — implements movable mutex ownership wrapper (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/thread/condition_variable*
