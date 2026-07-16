# std::optional<T>::value

```cpp
constexpr T& value() &;
constexpr const T& value() const &;  // (1) (since C++17)
constexpr T&& value() &&;
constexpr const T&& value() const &&;  // (2) (since C++17)
```

If `*this` contains a value, returns a reference to the contained value.

Otherwise, throws a `std::bad_optional_access` exception.

### Parameters

(none)

### Return value

A reference to the contained value.

### Exceptions

`std::bad_optional_access` if `*this` does not contain a value.

### Notes

The dereference operator `operator*()` does not check if this optional contains
a value, which may be more efficient than `value()`.

### Example

```cpp
#include <optional>
#include <iostream>
int main()
{
    std::optional<int> opt = {};

    try {
        [[maybe_unused]] int n = opt.value();
    } catch(const std::bad_optional_access& e) {
        std::cout << e.what() << '\n';
    }
    try {
        opt.value() = 42;
    } catch(const std::bad_optional_access& e) {
        std::cout << e.what() << '\n';
    }

    opt = 43;
    std::cout << *opt << '\n';

    opt.value() = 44;
    std::cout << opt.value() << '\n';
}
```

Output:

```text
bad optional access
bad optional access
43
44
```

### See also

- **value_or** — returns the contained value if available, another value
  otherwise (public member function)
- **operator->operator*** — accesses the contained value (public member
  function)
- **bad_optional_access (C++17)** — exception indicating checked access to an
  optional that doesn't contain a value (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/value*
