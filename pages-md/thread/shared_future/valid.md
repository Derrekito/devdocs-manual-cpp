# std::shared_future<T>::valid

```cpp
bool valid() const noexcept;  // (since C++11)
```

Checks if the future refers to a shared state.

This is the case only for futures that were not default-constructed or moved
from. Unlike `std::future`, `std::shared_future`'s shared state is not
invalidated when `get()` is called.

The behavior is undefined if any member function other than the destructor, the
copy-assignment operator, the move-assignment operator, or `valid` is called on
a `shared_future` that does not refer to shared state (although implementations
are encouraged to throw `std::future_error` indicating `no_state` in this case).
It is valid to move or copy from a shared_future object for which `valid()` is
`false`.

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
    std::shared_future<void> f = p.get_future();

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
true
```

### See also

- **wait** — waits for the result to become available (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/valid*
