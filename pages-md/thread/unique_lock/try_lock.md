# std::unique_lock<Mutex>::try_lock

```cpp
bool try_lock();  // (since C++11)
```

Tries to lock (i.e., takes ownership of) the associated mutex without blocking.
Effectively calls `mutex()->try_lock()`.

`std::system_error` is thrown if there is no associated mutex or if the mutex is
already locked by this `std::unique_lock`.

### Parameters

(none)

### Return value

`true` if the ownership of the mutex has been acquired successfully, `false`
otherwise.

### Exceptions

- Any exceptions thrown by `mutex()->try_lock()` (Mutex types do not throw in
  `try_lock`, but a custom Lockable might).
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.
- If the mutex is already locked by this `std::unique_lock`, `std::system_error`
  with an error code of `std::errc::resource_deadlock_would_occur`.

### Example

The following examples try to acquire a mutex that was locked and unlocked.

```cpp
#include <chrono>
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

using namespace std::chrono_literals;

int main()
{
    std::mutex counter_mutex;
    std::vector<std::thread> threads;
    using Id = int;

    auto worker_task = [&](Id id, std::chrono::seconds wait, std::chrono::seconds acquire)
    {
        // wait for a few seconds before acquiring lock.
        std::this_thread::sleep_for(wait);

        std::unique_lock<std::mutex> lock(counter_mutex, std::defer_lock);
        if (lock.try_lock())
            std::cout << '#' << id << ", lock acquired.\n";
        else
        {
            std::cout << '#' << id << ", failed acquiring lock.\n";
            return;
        }

        // keep the lock for a while.
        std::this_thread::sleep_for(acquire);

        std::cout << '#' << id << ", releasing lock (via destructor).\n";
    };

    threads.emplace_back(worker_task, Id{0}, 0s, 2s);
    threads.emplace_back(worker_task, Id{1}, 1s, 0s);
    threads.emplace_back(worker_task, Id{2}, 3s, 0s);

    for (auto& thread : threads)
        thread.join();
}
```

Output:

```text
#0, lock acquired.
#1, failed acquiring lock.
#0, releasing lock (via destructor).
#2, lock acquired.
#2, releasing lock (via destructor).
```

### See also

- **lock** — locks (i.e., takes ownership of) the associated mutex (public
  member function)
- **try_lock_for** — attempts to lock (i.e., takes ownership of) the associated
  TimedLockable mutex, returns if the mutex has been unavailable for the
  specified time duration (public member function)
- **try_lock_until** — tries to lock (i.e., takes ownership of) the associated
  TimedLockable mutex, returns if the mutex has been unavailable until specified
  time point has been reached (public member function)
- **unlock** — unlocks (i.e., releases ownership of) the associated mutex
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/try_lock*
