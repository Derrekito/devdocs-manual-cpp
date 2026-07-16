# std::ranges::adjacent_transform_view<V,F,N>::size

```cpp
constexpr auto size() requires ranges::sized_range<InnerView>;  // (since C++23)
constexpr auto size() const requires ranges::sized_range<const InnerView>;  // (since C++23)
```

Returns the number of elements.

Let `inner_` be the underlying object of type `InnerView` (that is the
`ranges::adjacent_view<V,N>`).

1,2) Equivalent to `return inner_.size();`.

### Parameters

(none)

### Return value

The number of elements, may be `​0​` if the size of the underlying view `V` is
less than `N`.

### Example

```cpp
#include <ranges>

int main()
{
    constexpr static auto v = {1, 2, 3, 4, 5, 6};

    auto f = [](auto...) { return 0; }; // dummy

    constexpr int width1 {4};
    constexpr auto view1 = v | std::views::adjacent_transform<width1>(f);
    static_assert(view1.size() == 3);
    static_assert(view1.size() == (v.size() - width1 + 1));

    constexpr int width2 {8};
    constexpr auto view2 = v | std::views::adjacent_transform<width2>(f);
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
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/size*
