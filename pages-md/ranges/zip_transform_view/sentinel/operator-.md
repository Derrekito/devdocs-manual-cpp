# operator-(ranges::zip_transform_view::*sentinel*)

```cpp
template< bool OtherConst >
  requires std::sized_sentinel_for</*zentinel*/<Const>, /*ziperator*/<OtherConst>>
friend constexpr ranges::range_difference_t</*maybe-const*/<OtherConst, /*InnerView*/>>
  operator-( const /*iterator*/<OtherConst>& x, const /*sentinel*/& y );  // (1) (since C++23)
template< bool OtherConst >
  requires std::sized_sentinel_for</*zentinel*/<Const>, /*ziperator*/<OtherConst>>
friend constexpr ranges::range_difference_t</*maybe-const*/<OtherConst, /*InnerView*/>>
  operator-( const /*sentinel*/& y, const /*iterator*/<OtherConst>& x );  // (2) (since C++23)
```

Computes the distance between the underlying iterator of `x` and the underlying
sentinel of `y`.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`zip_transform_view::sentinel` is an associated class of the arguments.

### Parameters

- **x** — an iterator
- **y** — a sentinel

### Return value

Let `inner_` denote the underlying iterator or sentinel respectively.

1) `x.inner_ - y.inner_`

2) `y.inner_ - x.inner_`

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/sentinel/operator-*
