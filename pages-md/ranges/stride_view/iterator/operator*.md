# std::ranges::stride_view<V>::*iterator*<Const>::operator*

```cpp
constexpr decltype(auto) operator*() const;  // (since C++23)
```

Returns the elements into `V` the underlying iterator `current_` points to.

Equivalent to: `return *current_;`.

### Parameters

(none)

### Return value

The current element.

### Notes

`operator->` is not provided.

### Example

```cpp
#include <ranges>

int main()
{
    constexpr static auto v = {'a', 'b', 'c', 'd', 'e'};
    constexpr auto view{v | std::views::stride(2)};
    constexpr auto iter{view.begin() + 1};
    static_assert(*iter == 'c');
    static_assert(*(view.begin() + 2) == 'e');
}
```

### See also

- **operator[] (C++23)** — accesses an element by index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/operator**
