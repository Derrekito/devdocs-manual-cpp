# swap(std::jthread)

```cpp
friend void swap( jthread& lhs, jthread& rhs ) noexcept;  // (since C++20)
```

Overloads the `std::swap` algorithm for `std::jthread`. Exchanges the state of
`lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`. This function is
not visible to ordinary unqualified or qualified lookup, and can only be found
by argument-dependent lookup when `std::jthread` is an associated class of the
arguments.

### Parameters

- **lhs, rhs** — jthreads whose states to swap

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
    using std::swap;

    std::jthread t1(foo);
    std::jthread t2(bar);

    std::cout << "thread 1 id: " << t1.get_id() << '\n'
              << "thread 2 id: " << t2.get_id() << '\n';

    swap(t1, t2);

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
thread 1 id: 1892
thread 2 id: 2584
after std::swap(t1, t2):
thread 1 id: 2584
thread 2 id: 1892
after t1.swap(t2):
thread 1 id: 1892
thread 2 id: 2584
```

### See also

- **swap** — swaps two jthread objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/swap2*
