# std::ranges::zip_transform_view<F,Views...>::size

```cpp
constexpr auto size()
    requires ranges::sized_range</*InnerView*/>;  // (1) (since C++23)
constexpr auto size() const
    requires ranges::sized_range<const /*InnerView*/>  // (2) (since C++23)
```

Returns the number of elements in the `zip_transform_view`. Provided only if
each underlying (adapted) range satisfies `sized_range`.

1,2) Equivalent to: `return zip_.size();`.

### Parameters

(none)

### Return value

The number of elements, which is the minimum size among all sizes of adapted
`view`s.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <deque>
#include <forward_list>
#include <functional>
#include <iostream>
#include <ranges>
#include <vector>

int main()
{
    auto x = std::vector{1, 2, 3, 4, 5};
    auto y = std::deque<short>{10, 20, 30};
    auto z = std::forward_list{100., 200.};

    auto v1 = std::views::zip_transform(std::plus{}, x, y);
    assert(v1.size() == std::min(x.size(), y.size()));
    assert(v1.size() == 3);
    for (int i : v1)
        std::cout << i << ' ';
    std::cout << '\n';

    [[maybe_unused]] auto v2 = std::views::zip_transform(std::plus{}, x, z);
//  auto sz = v2.size(); // Error: z doesn't have size(), so neither does v2
    static_assert(not std::ranges::sized_range<decltype(z)>);
}
```

Output:

```text
11 22 33
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/size*
