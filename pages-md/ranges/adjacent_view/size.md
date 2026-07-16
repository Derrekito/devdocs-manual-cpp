# std::ranges::adjacent_view<V,N>::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (since C++23)
constexpr auto size() const requires ranges::sized_range<const V>;  // (since C++23)
```

Returns the number of elements.

Let `base_` be the underlying view. Equivalent to:

```cpp
using SizeType = decltype(ranges::size(base_));
using CommonType = ranges::common_type_t<SizeType, std::size_t>;
auto size = static_cast<CommonType>(ranges::size(base_));
size -= std::min<CommonType>(size, N - 1);
return static_cast<SizeType>(size);
```

### Parameters

(none)

### Return value

The number of elements, may be `​0​` if `ranges::size(base_)` is less than `N`.

### Example

```cpp
#include <ranges>

int main()
{
    constexpr static auto v = {1, 2, 3, 4, 5, 6};

    constexpr int width1 {4};
    constexpr auto view1 {std::views::adjacent<width1>(v)};
    static_assert(view1.size() == 3);
    static_assert(view1.size() == (v.size() - width1 + 1));

    constexpr int width2 {8};
    constexpr auto view2 {std::views::adjacent<width2>(v)};
    // window is too wide, so view2 has no elements:
    static_assert(view2.size() == 0);
}
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/size*
