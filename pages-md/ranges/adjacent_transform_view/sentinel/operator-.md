# operator-(ranges::adjacent_transform_view::*sentinel*)

```cpp
template< bool OtherConst >
    requires std::sized_sentinel_for</*inner-sentinel*/<Const>,
                                     /*inner-iterator*/<OtherConst>>
friend constexpr
    ranges::range_difference_t</*maybe-const*/<OtherConst, InnerView>>
        operator-( const /*iterator*/<OtherConst>& x, const /*sentinel*/& y );  // (1) (since C++23)
template< bool OtherConst >
    requires std::sized_sentinel_for</*inner-sentinel*/<Const>,
                                     /*inner-iterator*/<OtherConst>>
friend constexpr
    ranges::range_difference_t</*maybe-const*/<OtherConst, InnerView>>
        operator-( const /*sentinel*/& y, const /*iterator*/<OtherConst>& x );  // (2) (since C++23)
```

Computes the distance between the iterator `x` and the sentinel `y`.

1) Equivalent to: `return x.inner_ - y.inner_;`.

2) Equivalent to: `return y.inner_ - x.inner_;`.

These function templates are not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`adjacent_transform_view::sentinel` is an associated class of the arguments.

### Parameters

- **x** — the iterator
- **y** — the sentinel

### Return value

The distance between iterator and sentinel.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/sentinel/operator-*
