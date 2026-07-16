# std::ranges::adjacent_view<V,N>::*iterator*<Const>::operator*

```cpp
constexpr auto operator*() const;  // (since C++23)
```

Returns the elements into `V` the underlying array of iterators points to.

Let `current_` be an underlying array of iterators.

Equivalent to:

```cpp
return __tuple_transform([](auto& i) -> decltype(auto) { return *i; }, current_);
```

### Parameters

(none)

### Return value

The current element, which is a `std::tuple` of references to underlying
elements.

### Notes

`operator->` is not provided.

### Example

```cpp
#include <ranges>
#include <tuple>

int main()
{
    constexpr static auto v = {0, 1, 2, 3, 4, 5};
    //                               └──┬──┘
    //                                  └─────────────────┐
    constexpr auto view {v | std::views::adjacent<3>}; // │
    constexpr auto iter {view.begin() + 2};            // │
    //              ┌────────────────────┬────────────────┘
    //              │                 ┌──┴──┐
    static_assert(*iter == std::tuple{2, 3, 4});
}
```

### See also

- **operator[] (C++23)** — accesses an element by index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/iterator/operator**
