# std::ranges::enumerate_view<V>::begin

```cpp
constexpr auto begin() requires (!__SimpleView<V>);  // (1) (since C++23)
constexpr auto begin() const requires /*range-with-movable-references*/<const V>;  // (2) (since C++23)
```

Returns an iterator to the first element of the `enumerate_view`.

Let `base_` denote the underlying view.

1) Equivalent to `return /*iterator*/<false>(ranges::begin(base_), 0);`.

2) Equivalent to `return /*iterator*/<true>(ranges::begin(base_), 0);`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Example

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/begin*
