# std::ranges::elements_view<V,N>::begin

```cpp
constexpr auto begin() requires (!__SimpleView<V>);  // (1) (since C++20)
constexpr auto begin() const requires ranges::range<const V>;  // (2) (since C++20)
```

Returns an iterator to the first element of the `elements_view`.

Let `base_` be the underlying view.

1) Equivalent to `return /*iterator*/<false>(ranges::begin(base_));`.

2) Equivalent to `return /*iterator*/<true>(ranges::begin(base_));`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Example

### See also

- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/begin*
