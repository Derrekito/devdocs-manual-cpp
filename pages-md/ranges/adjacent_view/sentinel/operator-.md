# operator-(ranges::adjacent_view::*sentinel*)

```cpp
template< bool OtherConst >
  requires std::sized_sentinel_for<ranges::sentinel_t<Base>,
                                   ranges::iterator_t<__maybe_const<OtherConst, V>>>
friend constexpr ranges::range_difference_t<__maybe_const<OtherConst, V>>
  operator-( const /*iterator*/<OtherConst>& x, const /*sentinel*/& y );  // (1) (since C++23)
template< bool OtherConst >
  requires std::sized_sentinel_for<ranges::sentinel_t<Base>,
                                   ranges::iterator_t<__maybe_const<OtherConst, V>>>
friend constexpr ranges::range_difference_t<__maybe_const<OtherConst, V>>
  operator-( const /*sentinel*/& y, const /*iterator*/<OtherConst>& x );  // (2) (since C++23)
```

Computes the distance between the underlying iterator of `x` and the underlying
sentinel of `y`.

Let `current_` denote the underlying array of iterators in `x`, and `end_`
denote the underlying sentinel in `y`.

1) Equivalent to: `return x.current_.back() - y.end_;`

2) Equivalent to: `return y.end_ - x.current_.back();`

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`adjacent_view::sentinel` is an associated class of the arguments.

### Parameters

- **x** — an iterator
- **y** — a sentinel

### Return value

The distance between `x` and `y`.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/sentinel/operator-*
