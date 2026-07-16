# std::ranges::cartesian_product_view<First, Vs...>::*iterator*<Const>::operator[]

```cpp
constexpr reference operator[]( difference_type n ) const
    requires /*cartesian-product-is-random-access*/<Const, First, Vs...>;  // (since C++23)
```

Returns an element at specified relative location. Equivalent to: `return
*((*this) + n);`.

### Parameters

- **n** — position relative to current location

### Return value

The element at displacement `n` relative to the current location.

### Example

### See also

- **operator* (C++23)** — accesses the element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/operator_at*
