# std::jthread::swap

```cpp
void swap( std::jthread& other ) noexcept;  // (since C++20)
```

Exchanges the underlying handles of two jthread objects.

### Parameters

- **other** — the jthread to swap with

### Return value

(none)

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

void foo()
{
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

void bar()
{
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

int main()
{
    std::jthread t1(foo);
    std::jthread t2(bar);

    std::cout << "thread 1 id: " << t1.get_id() << '\n'
              << "thread 2 id: " << t2.get_id() << '\n';

    std::swap(t1, t2);

    std::cout << "after std::swap(t1, t2):" << '\n'
              << "thread 1 id: " << t1.get_id() << '\n'
              << "thread 2 id: " << t2.get_id() << '\n';

    t1.swap(t2);

    std::cout << "after t1.swap(t2):" << '\n'
              << "thread 1 id: " << t1.get_id() << '\n'
              << "thread 2 id: " << t2.get_id() << '\n';

}
```

Possible output:

```text
thread 1 id: 140185268262656
thread 2 id: 140185259869952
after std::swap(t1, t2):
thread 1 id: 140185259869952
thread 2 id: 140185268262656
after t1.swap(t2):
thread 1 id: 140185268262656
thread 2 id: 140185259869952
```

### See also

- **swap(std::jthread) (C++20)** — specializes the `std::swap` algorithm
  (function)

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/swap*
