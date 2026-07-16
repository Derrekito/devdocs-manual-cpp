# std::ranges::slide_view<V>::size

```cpp
constexpr auto size()
  requires ranges::sized_range<V>;  // (since C++23)
constexpr auto size() const
  requires ranges::sized_range<const V>;  // (since C++23)
```

Returns the number of elements.

Let `base_` and `n_` be the underlying view and "window size" respectively.

Equivalent to:

```cpp
auto sz = ranges::distance(base_) - n_ + 1;
if (sz < 0)
    sz = 0;
return __to_unsigned_like(sz);
```

### Parameters

(none)

### Return value

The number of elements. Equals to `​0​`, if the number of elements
(`ranges::size(base_)`) in underlying view `base_` is less than "window size"
`n_`.

### Example

```cpp
#include <forward_list>
#include <iostream>
#include <list>
#include <ranges>

int main()
{
    constexpr static auto v = {1, 2, 3, 4, 5, 6};

    constexpr int width1{4};
    constexpr auto view1{std::views::slide(v, width1)};
    static_assert(view1.size() == 3);
    static_assert(view1.size() == (v.size() - width1 + 1));

    constexpr int width2{8};
    constexpr auto view2{std::views::slide(v, width2)};
    // window is too wide, so view2 has no elements:
    static_assert(view2.size() == 0);

    std::forward_list forward_list = v;
    const auto view3{std::views::slide(forward_list, width1)};
//  auto x = view3.size(); // error: sized_range constraint is not satisfied

    std::list list = v;
    const auto view4{std::views::slide(list, width1)};
    std::cout << view4.size() << '\n'; // prints 3
}
```

Output:

```text
3
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/size*
