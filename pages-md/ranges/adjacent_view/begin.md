# std::ranges::adjacent_view<V,N>::begin

```cpp
constexpr auto begin() requires (!__SimpleView<V>);  // (1) (since C++23)
constexpr auto begin() const requires ranges::range<const V>;  // (2) (since C++23)
```

Returns an iterator to the first element of the `adjacent_view`.

Let `base_` be the underlying view.

1) Equivalent to `return /*iterator*/<false>(ranges::begin(base_),
   ranges::end(base_));`.

2) Equivalent to `return /*iterator*/<true>(ranges::begin(base_),
   ranges::end(base_));`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Example

```cpp
#include <ranges>
#include <tuple>
#include <type_traits>

int main()
{
    constexpr static auto v = {'A', 'B', 'C', 'D', 'E'};

    constexpr auto view = std::views::adjacent<3>(v);

    constexpr auto tuple = *view.begin();

    static_assert
    (
        std::is_same_v
        <
            decltype(tuple),
            const std::tuple<char const&, char const&, char const&>
        >
    );

    static_assert
    (
        std::get<0>(tuple) == 'A' &&
        std::get<1>(tuple) == 'B' &&
        std::get<2>(tuple) == 'C'
    );
}
```

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/begin*
