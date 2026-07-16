# std::recursive_mutex::try_lock

```cpp
bool try_lock() noexcept;  // (since C++11)
```

Tries to lock the mutex. Returns immediately. On successful lock acquisition
returns `true`, otherwise returns `false`.

This function is allowed to fail spuriously and return `false` even if the mutex
is not currently locked by any other thread.

A thread may call `try_lock` on a recursive mutex repeatedly. Successful calls
to `try_lock` increment the ownership count: the mutex will only be released
after the thread makes a matching number of calls to `unlock`.

The maximum number of levels of ownership is unspecified. A call to `try_lock`
will return `false` if this number is exceeded.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation if it returns `true`. Note that prior
`lock()` does not synchronize with this operation if it returns `false`.

### Parameters

(none)

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Throws nothing.

### Example

```cpp
#include <iostream>
#include <mutex>

int main()
{
    std::recursive_mutex test;
    if (test.try_lock())
    {
        std::cout << "lock acquired\n";
        test.unlock();
    }
    else
        std::cout << "lock not acquired\n";

    test.lock();
    // non-recursive mutex would return false from try_lock now
    if (test.try_lock())
    {
        std::cout << "lock acquired\n";
        test.unlock();
    }
    else
        std::cout << "lock not acquired\n";

    test.unlock();
}
```

Output:

```text
lock acquired
lock acquired
```

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **unlock** — unlocks the mutex (public member function)

**C documentation for `mtx_trylock`**

---
*Source: https://en.cppreference.com/w/cpp/thread/recursive_mutex/try_lock*
