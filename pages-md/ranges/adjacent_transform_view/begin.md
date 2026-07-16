# std::ranges::adjacent_transform_view<V,F,N>::begin

```cpp
constexpr auto begin();  // (1) (since C++23)
constexpr auto begin() const
    requires ranges::range<const InnerView> &&
        std::regular_invocable<const F&,
            __REPEAT(ranges::range_reference_t<const V>, N)...>;  // (2) (since C++23)
```

Returns an iterator to the first element of the `adjacent_transform_view`.

Let `inner_` be the underlying `ranges::adjacent_view`.

1) Equivalent to `return /*iterator*/<false>(*this, inner_.begin());`.

2) Equivalent to `return /*iterator*/<true>(*this, inner_.begin());`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Example

```cpp
#include <ranges>

int main()
{
    auto sum = [](auto... args) { return (... + args); };

    constexpr auto view = std::views::iota(13, 1337)
                        | std::views::adjacent_transform<3>(sum);

    static_assert(*view.begin() == 42 and 42 == 13 + 14 + 15);
}
```

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/begin*
