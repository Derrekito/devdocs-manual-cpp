# std::ranges::drop_view<V>::drop_view

```cpp
drop_view() requires std::default_initializable<V> = default;  // (1) (since C++20)
constexpr explicit drop_view( V base, ranges::range_difference_t<V> count );  // (2) (since C++20)
```

Constructs a `drop_view`.

1) Default constructor. Value-initializes the underlying view and initializes
   the count to `​0​`. After construction, `base()` returns a copy of `V()` and
   `size()` equals to the size of the underlying view.

2) Initializes the underlying view with `std::move(base)` and the count with
   `count`. After construction, `base()` returns a copy of `base` and `size()`
   returns `ranges::size(base) - count` if the size of `base` is not less than
   `count`, or `​0​` otherwise.

### Parameters

- **base** — the underlying view
- **count** — number of elements to skip

### Example

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <iterator>
#include <ranges>

int main()
{
    constexpr std::array hi{'H', 'e', 'l', 'l', 'o', ',',
                            ' ', 'C', '+', '+', '2', '0'};

    std::ranges::for_each(hi, [](const char c){ std::cout << c; });
    std::cout << '\n';

    constexpr auto n = std::distance(hi.cbegin(), std::ranges::find(hi, 'C'));

    auto cxx = std::ranges::drop_view{hi, n};

    std::ranges::for_each(cxx, [](const char c){ std::cout << c; });
    std::cout << '\n';
}
```

Output:

```text
Hello, C++20
C++20
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3714 (P2711R1) | C++20 | the multi-parameter constructor was not explicit
      | made explicit

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_view/drop_view*
