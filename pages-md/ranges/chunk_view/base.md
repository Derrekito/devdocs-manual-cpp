# std::ranges::chunk_view<V>::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++23)
constexpr V base() &&;  // (2) (since C++23)
```

Returns a copy of the underlying view.

Let `base_` be the underlying view.

1) Copy constructs the result from the underlying view. Equivalent to `return
   base_;`

2) Move constructs the result from the underlying view. Equivalent to `return
   std::move(base_);`

### Parameters

(none)

### Return value

A copy of the underlying view.

### Example

A link to test the example online: Compiler Explorer.

```cpp
#include <print>
#include <ranges>

int main()
{
    static constexpr auto v = {1, 2, 3, 4};
    constexpr auto w{std::ranges::chunk_view(v, 2)};
    std::println("{}", w.base());
}
```

Output:

```text
[1, 2, 3, 4]
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/base*
