# std::ranges::view_interface<D>::operator bool

```cpp
constexpr explicit operator bool() requires /* see below */;  // (1) (since C++20)
constexpr explicit operator bool() const requires /* see below */;  // (2) (since C++20)
```

The default implementation of `operator bool` member function checks whether the
view is non-empty. It makes the derived type contextually convertible to `bool`.

1) Let `derived` be `static_cast<D&>(*this)`. The expression in the
   requires-clause is equal to `requires { ranges::empty(derived); }`, and the
   function body is equivalent to `return !ranges::empty(derived);`.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

`false` if the value of the derived type is empty (determined by
`std::ranges::empty`), `true` otherwise.

### Notes

In C++20, no type derived from `std::ranges::view_interface` in the standard
library provides their own `operator bool`. Almost all of these types use the
default implementation.

A notable exception is `std::ranges::basic_istream_view`. For its iterator type
never satisfies `forward_iterator`, the view cannot use the inherited `operator
bool`.

### Example

```cpp
#include <array>
#include <iostream>
#include <ranges>

int main()
{
    const std::array ints {0, 1, 2, 3, 4};
    auto odds = ints | std::views::filter([](int i) { return 0 != i % 2; });
    auto negs = ints | std::views::filter([](int i) { return i < 0; });
    std::cout << std::boolalpha
              << "Has odd numbers: " << (!!odds) << ' ' << '\n'
              << "Has negative numbers: " << (!!negs) << ' ' << '\n';
}
```

Output:

```text
Has odd numbers: true
Has negative numbers: false
```

### See also

- **ranges::empty (C++20)** — checks whether a range is empty (customization
  point object)
- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function)
- **empty (C++17)** — checks whether the container is empty (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/operator_bool*
