# std::mutex::unlock

```cpp
void unlock();  // (since C++11)
```

Unlocks the mutex.

The mutex must be locked by the current thread of execution, otherwise, the
behavior is undefined.

This operation *synchronizes-with* (as defined in `std::memory_order`) any
subsequent lock operation that obtains ownership of the same mutex.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws nothing.

### Notes

`unlock()` is usually not called directly: `std::unique_lock` and
`std::lock_guard` are used to manage exclusive locking.

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
        int g_num_running = ++g_num;
        g_num_mutex.unlock();
        std::cout << id << " => " << g_num_running << '\n';

        std::this_thread::sleep_for(std::chrono::seconds(1));
    }
}

int main()
{
    std::thread t1(slow_increment, 0);
    std::thread t2(slow_increment, 1);
    t1.join();
    t2.join();
}
```

Possible output:

```text
0 => 1
1 => 2
0 => 3
1 => 4
0 => 5
1 => 6
```

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)

**C documentation for `mtx_unlock`**

---
*Source: https://en.cppreference.com/w/cpp/thread/mutex/unlock*
