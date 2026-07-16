# std::jthread::get_id

```cpp
[[nodiscard]] std::jthread::id get_id() const noexcept;  // (since C++20)
```

Returns a value of `std::jthread::id` (which is a type alias for
`std::thread::id`) identifying the thread associated with `*this`.

### Parameters

(none)

### Return value

A value of type `std::jthread::id` identifying the thread associated with
`*this`. If there is no thread associated, default constructed
`std::jthread::id` is returned.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

void foo()
{
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main()
{
    std::jthread t1(foo);
    std::jthread::id t1_id = t1.get_id();

    std::jthread t2(foo);
    std::jthread::id t2_id = t2.get_id();

    std::cout << "t1's id: " << t1_id << '\n';
    std::cout << "t2's id: " << t2_id << '\n';

    t1.join();
    t2.join();

    std::cout << "t1's id after join: " << t1.get_id() << '\n';
    std::cout << "t2's id after join: " << t2.get_id() << '\n';
}
```

Possible output:

```text
t1's id: 140146221688576
t2's id: 140146213295872
t1's id after join: thread::id of a non-executing thread
t2's id after join: thread::id of a non-executing thread
```

### See also

- **id** — represents the *id* of a thread (public member class of
  `std::thread`)
- **joinable** — checks whether the thread is joinable, i.e. potentially running
  in parallel context (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/get_id*
