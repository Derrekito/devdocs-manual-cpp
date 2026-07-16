# operator-(ranges::elements_view::*sentinel*)

```cpp
template< bool OtherConst >
  requires std::sized_sentinel_for<ranges::sentinel_t<Base>,
                                   ranges::iterator_t</*maybe-const*/<OtherConst, V>>>
friend constexpr ranges::range_difference_t</*maybe-const*/<OtherConst, V>>
  operator-( const /*iterator*/<OtherConst>& x, const /*sentinel*/& y );  // (1) (since C++20)
template< bool OtherConst >
  requires std::sized_sentinel_for<ranges::sentinel_t<Base>,
                                   ranges::iterator_t</*maybe-const*/<OtherConst, V>>>
friend constexpr ranges::range_difference_t</*maybe-const*/<OtherConst, V>>
  operator-( const /*sentinel*/& y, const /*iterator*/<OtherConst>& x );  // (2) (since C++20)
```

Computes the distance between the underlying iterator of `x` and the underlying
sentinel of `y`.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`elements_view::sentinel` is an associated class of the arguments.

### Parameters

- **x** — an iterator
- **y** — a sentinel

### Return value

1) `x.base() - y.base()`

2) `y.base() - x.base()`

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/sentinel/operator-*
