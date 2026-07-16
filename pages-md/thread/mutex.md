# std::mutex

```cpp
class mutex;  // (since C++11)
```

The `mutex` class is a synchronization primitive that can be used to protect
shared data from being simultaneously accessed by multiple threads.

`mutex` offers exclusive, non-recursive ownership semantics:

- A calling thread *owns* a `mutex` from the time that it successfully calls
  either `lock` or `try_lock` until it calls `unlock`.
- When a thread owns a `mutex`, all other threads will block (for calls to
  `lock`) or receive a `false` return value (for `try_lock`) if they attempt to
  claim ownership of the `mutex`.
- A calling thread must not own the `mutex` prior to calling `lock` or
  `try_lock`.

The behavior of a program is undefined if a `mutex` is destroyed while still
owned by any threads, or a thread terminates while owning a `mutex`. The `mutex`
class satisfies all requirements of Mutex and StandardLayoutType.

`std::mutex` is neither copyable nor movable.

### Member types

- **`native_handle_type`(not always present)** — *implementation-defined*

### Member functions

- **(constructor)** — constructs the mutex (public member function)
- **(destructor)** — destroys the mutex (public member function)
- **operator= [deleted]** — not copy-assignable (public member function)

**Locking**

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **unlock** — unlocks the mutex (public member function)

**Native handle**

- **native_handle** — returns the underlying implementation-defined native
  handle object (public member function)

### Notes

`std::mutex` is usually not accessed directly: `std::unique_lock`,
`std::lock_guard`, or `std::scoped_lock`(since C++17) manage locking in a more
exception-safe manner.

### Example

This example shows how a `mutex` can be used to protect an `std::map` shared
between two threads.

```cpp
#include <chrono>
#include <iostream>
#include <map>
#include <mutex>
#include <string>
#include <thread>

std::map<std::string, std::string> g_pages;
std::mutex g_pages_mutex;

void save_page(const std::string& url)
{
    // simulate a long page fetch
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::string result = "fake content";

    std::lock_guard<std::mutex> guard(g_pages_mutex);
    g_pages[url] = result;
}

int main()
{
    std::thread t1(save_page, "http://foo");
    std::thread t2(save_page, "http://bar");
    t1.join();
    t2.join();

    // safe to access g_pages without lock now, as the threads are joined
    for (const auto& pair : g_pages)
        std::cout << pair.first << " => " << pair.second << '\n';
}
```

Output:

```text
http://bar => fake content
http://foo => fake content
```

### See also

- **recursive_mutex (C++11)** — provides mutual exclusion facility which can be
  locked recursively by the same thread (class)
- **lock_guard (C++11)** — implements a strictly scope-based mutex ownership
  wrapper (class template)
- **unique_lock (C++11)** — implements movable mutex ownership wrapper (class
  template)
- **scoped_lock (C++17)** — deadlock-avoiding RAII wrapper for multiple mutexes
  (class template)
- **condition_variable (C++11)** — provides a condition variable associated with
  a `std::unique_lock` (class)

---
*Source: https://en.cppreference.com/w/cpp/thread/mutex*
