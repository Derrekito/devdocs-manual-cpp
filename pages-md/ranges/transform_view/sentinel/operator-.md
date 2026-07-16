# operator-(ranges::transform_view::*sentinel*)

```cpp
friend constexpr ranges::range_difference_t<Base>
  operator-( const /*iterator*/<Const>& x, const /*sentinel*/& y )
    requires std::sized_sentinel_for<ranges::sentinel_t<Base>, ranges::iterator_t<Base>>;  // (1) (since C++20)
friend constexpr ranges::range_difference_t<Base>
  operator-( const /*sentinel*/& y, const /*iterator*/<Const>& x )
    requires std::sized_sentinel_for<ranges::sentinel_t<Base>, ranges::iterator_t<Base>>;  // (2) (since C++20)
```

Computes the distance between the underlying iterator of `x` and the underlying
sentinel of `y`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`transform_view::sentinel<Const>` is an associated class of the arguments.

### Parameters

- **x** — an iterator
- **y** — a sentinel

### Return value

Let `current_` denote the underlying iterator, `end_` denote the underlying
sentinel.

1) `x.current_ - y.end_`

2) `y.end_ - x.current_`

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/sentinel/operator-*
