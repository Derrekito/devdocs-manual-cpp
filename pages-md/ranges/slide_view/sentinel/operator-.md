# operator-(ranges::slide_view::*sentinel*)

```cpp
friend constexpr ranges::range_difference_t<V>
operator-( const /*iterator*/<false>& x, const /*sentinel*/& y )
    requires std::sized_sentinel_for<ranges::sentinel_t<V>,
                                     ranges::iterator_t<V>>;  // (1) (since C++23)
friend constexpr ranges::range_difference_t<V>
operator-( const /*sentinel*/& y, const /*iterator*/<false>& x )
    requires std::sized_sentinel_for<ranges::sentinel_t<V>,
                                     ranges::iterator_t<V>>;  // (2) (since C++23)
```

Computes the distance between the underlying iterator of `x` and the underlying
sentinel of `y`.

Let `last_ele_` denote the underlying iterator of `x` and `end_` denote the
underlying sentinel of `y`.

1) Equivalent to: `return x.last_ele_ - y.end_;`.

2) Equivalent to: `return y.end_ - x.last_ele_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `slide_view::sentinel` is an
associated class of the arguments.

### Parameters

- **x** — an iterator
- **y** — a sentinel

### Return value

The distance between the iterator and the sentinel.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/sentinel/operator-*
