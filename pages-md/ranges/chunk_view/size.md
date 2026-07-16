# std::ranges::chunk_view<V>::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (1) (since C++23)
constexpr auto size() const requires ranges::sized_range<const V>;  // (2) (since C++23)
```

Returns the number of elements, which is the smallest integer value that is not
less than the quotient of dividing the size of underlying view `base_` by the
underlying data member `n_`, that holds the number passed to the constructor
(`​0​` if default constructed). Equivalent to

```cpp
return __to_unsigned_like(__div_ceil(ranges::distance(base_), n_));
```

### Parameters

(none)

### Return value

The number of elements.

### Example

```cpp
#include <ranges>

int main()
{
    constexpr static auto v = {1, 2, 3, 4, 5};
    constexpr auto w{ std::ranges::chunk_view(v, 2) };
    static_assert(w.size() == (5 / 2 + (5 % 2 ? 1 : 0)));
}
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/size*
