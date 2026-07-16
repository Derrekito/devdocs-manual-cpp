# std::thread::~thread

```cpp
~thread();  // (since C++11)
```

Destroys the thread object.

If `*this` has an associated thread (`joinable() == true`), `std::terminate()`
is called.

### Notes

A thread object does not have an associated thread (and is safe to destroy)
after

- it was default-constructed.
- it was moved from.
- `join()` has been called.
- `detach()` has been called.

### Example

```cpp
#include <thread>
using namespace std::chrono_literals;

int main()
{
    auto bleah = std::thread{[]{ std::this_thread::sleep_for(13ms); }};

}   // ~thread calls std::terminate()
```

Possible output:

```text
terminate called without an active exception
```

### See also

- **(destructor)** — if the thread is joinable, then a stop is requested and the
  thread joins (public member function of `std::jthread`)

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/%7Ethread*
