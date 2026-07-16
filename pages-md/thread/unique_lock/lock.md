# std::unique_lock<Mutex>::lock

```cpp
void lock();  // (since C++11)
```

Locks (i.e., takes ownership of) the associated mutex. Effectively calls
`mutex()->lock()`.

### Parameters

(none)

### Return value

(none)

### Exceptions

- Any exceptions thrown by `mutex()->lock()`.
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.
- If the mutex is already locked by this `unique_lock` (in other words,
  owns_lock is `true`), `std::system_error` with an error code of
  `std::errc::resource_deadlock_would_occur`.

### Example

The following example uses `lock` to re-acquire a mutex that was unlocked.

```cpp
#include <chrono>
#include <iostream>
#include <mutex>
#include <thread>
#include <vector>

int main()
{
    int counter = 0;
    std::mutex counter_mutex;
    std::vector<std::thread> threads;

    auto worker_task = [&](int id)
    {
        std::unique_lock<std::mutex> lock(counter_mutex);
        ++counter;
        std::cout << id << ", initial counter: " << counter << '\n';
        lock.unlock();

        // don't hold the lock while we simulate an expensive operation
        std::this_thread::sleep_for(std::chrono::seconds(1));

        lock.lock();
        ++counter;
        std::cout << id << ", final counter: " << counter << '\n';
    };

    for (int i = 0; i < 10; ++i)
        threads.emplace_back(worker_task, i);

    for (auto& thread : threads)
        thread.join();
}
```

Possible output:

```text
0, initial counter: 1
1, initial counter: 2
2, initial counter: 3
3, initial counter: 4
4, initial counter: 5
5, initial counter: 6
6, initial counter: 7
7, initial counter: 8
8, initial counter: 9
9, initial counter: 10
6, final counter: 11
3, final counter: 12
4, final counter: 13
2, final counter: 14
5, final counter: 15
0, final counter: 16
1, final counter: 17
7, final counter: 18
9, final counter: 19
8, final counter: 20
```

### See also

- **try_lock** — tries to lock (i.e., takes ownership of) the associated mutex
  without blocking (public member function)
- **unlock** — unlocks (i.e., releases ownership of) the associated mutex
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/lock*
