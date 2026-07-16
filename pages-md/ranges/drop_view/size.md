# std::ranges::drop_view<V>::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (1) (since C++20)
constexpr auto size() const requires ranges::sized_range<const V>;  // (2) (since C++20)
```

Returns the number of elements.

Let `base_` be the underlying view, `count_` be the stored count (usually the
number passed to the constructor, or `​0​` if `*this` is default constructed).
Equivalent to

```cpp
const auto s = ranges::size(base_);
const auto c = static_cast<decltype(s)>(count_);
return s < c ? 0 : s - c;
```

### Parameters

(none)

### Return value

The number of elements.

### Example

```cpp
#include <array>
#include <ranges>

int main()
{
    constexpr std::array a{42, 43, 44};
    static_assert(std::ranges::drop_view{std::views::all(a), 0}.size() == 3);
    static_assert(std::ranges::drop_view{std::views::all(a), 1}.size() == 2);
    static_assert(std::ranges::drop_view{std::views::all(a), 2}.size() == 1);
    static_assert(std::ranges::drop_view{std::views::all(a), 3}.size() == 0);
    static_assert(std::ranges::drop_view{std::views::all(a), 4}.size() == 0);
}
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_view/size*
