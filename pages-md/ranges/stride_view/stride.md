# std::ranges::stride_view<V>::stride

```cpp
constexpr ranges::range_difference_t<_Vp> stride() const noexcept;  // (since C++23)
```

Returns a copy of the underlying stride object `stride_`. Equivalent to `return
stride_;`.

### Parameters

(none)

### Return value

The stride value.

### Example

```cpp
#include <ranges>

int main()
{
    constexpr auto view = std::views::iota(1337)
                        | std::views::stride(42);
    static_assert(view.stride() == 42);
}
```

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/stride*
