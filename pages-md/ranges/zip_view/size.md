# std::ranges::zip_view<Views...>::size

```cpp
constexpr auto size()
    requires (ranges::sized_range<Views> && ...);  // (1) (since C++23)
constexpr auto size() const
    requires (ranges::sized_range<const Views> && ...);  // (2) (since C++23)
```

Returns the number of elements in the `zip_view`. Provided only if each
underlying (adapted) range satisfies `sized_range`.

Equivalent to:

```cpp
return std::apply
(
    [](auto... sizes)
    {
        using CT = /*make-unsigned-like-t*/<std::common_type_t<decltype(sizes)...>>;
        return ranges::min({CT(sizes)...});
    },
    /*tuple-transform*/(ranges::size, views_)
);
```

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
#include <ranges>
#include <vector>

int main()
{
    auto x = std::vector{1, 2, 3, 4, 5};
    auto y = std::deque{'a', 'b', 'c'};
    auto z = std::forward_list{1., 2.};

    auto v1 = std::views::zip(x, y);
    assert(v1.size() == std::min(x.size(), y.size()));
    assert(v1.size() == 3);

    [[maybe_unused]] auto v2 = std::views::zip(x, z);
//  auto sz = v2.size(); // Error, v2 does not have size():
    static_assert(not std::ranges::sized_range<decltype(z)>);
}
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/size*
