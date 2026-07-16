# std::ranges::adjacent_view<V,N>::*iterator*<Const>::operator[]

```cpp
constexpr auto operator[]( difference_type n ) const
    requires ranges::random_access_range<Base>;  // (since C++23)
```

Returns an element at specified relative location.

Let `current_` be an underlying array of iterators.

Equivalent to:

```cpp
return __tuple_transform([&](auto& i) -> decltype(auto) { return i[n]; }, current_);
```

### Parameters

- **n** — position relative to current location

### Return value

The element at displacement `n` relative to the current location.

### Example

```cpp
#include <ranges>
#include <tuple>

int main()
{
    constexpr static auto v = {0, 1, 2, 3, 4, 5};
    //                               └──┬──┘
    //                                  └─────────────────┐
    constexpr auto view = v | std::views::adjacent<3>; // │
    //                 ┌───────────────────┬──────────────┘
    //                 │                ┌──┴──┐
    static_assert(view[2] == std::tuple{2, 3, 4});
}
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/iterator/operator_at*
