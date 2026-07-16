# std::ranges::cartesian_product_view<First, Vs...>::size

```cpp
constexpr /* see description */ size()
    requires __cartesian_product_is_sized<First, Vs...>;  // (1) (since C++23)
constexpr /* see description */ size() const
    requires __cartesian_product_is_sized<const First, const Vs...>;  // (2) (since C++23)
```

Returns the number of elements. The return type is an implementation-defined
`unsigned-integer-like` type `U`.

Let `bases_` be the underlying tuple of views, and `prod` be the product of the
sizes of all the ranges in `bases_`.

1,2) Returns `prod`. The behavior is undefined if `prod` cannot be represented
   by the return type `U`. Equivalent to: return [&]<std::size_t...
   Is>(std::index_sequence<Is...>) { auto prod = static_cast<U>(1); prod =
   (static_cast<U>(ranges::size(std::get<Is>(bases_))) * ...); return prod; }
   (std::make_index_sequence<1U + sizeof...(Vs)>{});

### Parameters

(none)

### Return value

The number of elements, that is, the product of the sizes of all the underlying
ranges.

### Notes

The return type is the smallest `unsigned-integer-like` type that is
sufficiently wide to store the product of the maximum sizes of all the
underlying ranges, if such a type exists.

### Example

```cpp
#include <ranges>

int main()
{
    constexpr static auto w = {1};
    constexpr static auto x = {2, 3};
    constexpr static auto y = {4, 5, 6};
    constexpr static auto z = {7, 8, 9, 10, 11, 12, 13};
    constexpr auto v = std::ranges::cartesian_product_view(w, x, y, z);
    static_assert(v.size() == w.size() * x.size() * y.size() * z.size() and v.size() == 42);
}
```

### See also

- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)
- **ranges::ssize (C++20)** — returns a signed integer equal to the size of a
  range (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/size*
