# std::recursive_mutex::lock

```cpp
void lock();  // (since C++11)
```

Locks the mutex. If another thread has already locked the mutex, a call to
`lock` will block execution until the lock is acquired.

A thread may call `lock` on a recursive mutex repeatedly. Ownership will only be
released after the thread makes a matching number of calls to `unlock`.

The maximum number of levels of ownership is unspecified. An exception of type
`std::system_error` will be thrown if this number is exceeded.

Prior `unlock()` operations on the same mutex *synchronize-with* (as defined in
`std::memory_order`) this operation.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws `std::system_error` when errors occur, including errors from the
underlying operating system that would prevent `lock` from meeting its
specifications. The mutex is not locked in the case of any exception being
thrown.

### Notes

`lock()` is usually not called directly: `std::unique_lock`, `std::scoped_lock`,
and `std::lock_guard` are used to manage exclusive locking.

### Example

This example shows how `lock` and `unlock` can be used to protect shared data.

```cpp
#include <chrono>
#include <iostream>
#include <mutex>
#include <thread>

int g_num = 0; // protected by g_num_mutex
std::mutex g_num_mutex;

void slow_increment(int id)
{
    for (int i = 0; i < 3; ++i)
    {
        g_num_mutex.lock();
        ++g_num;
        // note, that the mutex also syncronizes the output
        std::cout << "id: " << id << ", g_num: " << g_num << '\n';
        g_num_mutex.unlock();

        std::this_thread::sleep_for(std::chrono::milliseconds(234));
    }
}

int main()
{
    std::thread t1{slow_increment, 0};
    std::thread t2{slow_increment, 1};
    t1.join();
    t2.join();
}
```

Possible output:

```text
id: 0, g_num: 1
id: 1, g_num: 2
id: 1, g_num: 3
id: 0, g_num: 4
id: 0, g_num: 5
id: 1, g_num: 6
```

### See also

- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **unlock** — unlocks the mutex (public member function)

**C documentation for `mtx_lock`**

---
*Source: https://en.cppreference.com/w/cpp/thread/recursive_mutex/lock*
