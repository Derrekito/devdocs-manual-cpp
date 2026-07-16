# std::thread::join

```cpp
void join();  // (since C++11)
```

Blocks the current thread until the thread identified by `*this` finishes its
execution.

The completion of the thread identified by `*this` *synchronizes with* the
corresponding successful return from `join()`.

No synchronization is performed on `*this` itself. Concurrently calling `join()`
on the same thread object from multiple threads constitutes a data race that
results in undefined behavior.

### Parameters

(none)

### Return value

(none)

### Postconditions

`joinable()` is `false`.

### Exceptions

`std::system_error` if an error occurs.

### Error conditions

- `resource_deadlock_would_occur` if `this->get_id() ==
  std::this_thread::get_id()` (deadlock detected).
- `no_such_process` if the thread is not valid.
- `invalid_argument` if `joinable()` is `false`.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

void foo()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

void bar()
{
    // simulate expensive operation
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main()
{
    std::cout << "starting first helper...\n";
    std::thread helper1(foo);

    std::cout << "starting second helper...\n";
    std::thread helper2(bar);

    std::cout << "waiting for helpers to finish..." << std::endl;
    helper1.join();
    helper2.join();

    std::cout << "done!\n";
}
```

Output:

```text
starting first helper...
starting second helper...
waiting for helpers to finish...
done!
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):

### See also

- **detach** — permits the thread to execute independently from the thread
  handle (public member function)
- **joinable** — checks whether the thread is joinable, i.e. potentially running
  in parallel context (public member function)

**C documentation for `thrd_join`**

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/join*
