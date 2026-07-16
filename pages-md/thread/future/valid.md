# std::future<T>::valid

```cpp
bool valid() const noexcept;  // (since C++11)
```

Checks if the future refers to a shared state.

This is the case only for futures that were not default-constructed or moved
from (i.e. returned by `std::promise::get_future()`,
`std::packaged_task::get_future()` or `std::async()`) until the first time
`get()` or `share()` is called.

The behavior is undefined if any member function other than the destructor, the
move-assignment operator, or `valid` is called on a `future` that does not refer
to shared state (although implementations are encouraged to throw
`std::future_error` indicating `no_state` in this case). It is valid to move
from a future object for which `valid()` is `false`.

### Parameters

(none)

### Return value

`true` if `*this` refers to a shared state, otherwise `false`.

### Example

```cpp
#include <future>
#include <iostream>

int main()
{
    std::promise<void> p;
    std::future<void> f = p.get_future();

    std::cout << std::boolalpha;

    std::cout << f.valid() << '\n';
    p.set_value();
    std::cout << f.valid() << '\n';
    f.get();
    std::cout << f.valid() << '\n';
}
```

Output:

```text
true
true
false
```

### See also

- **wait** — waits for the result to become available (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/future/valid*
