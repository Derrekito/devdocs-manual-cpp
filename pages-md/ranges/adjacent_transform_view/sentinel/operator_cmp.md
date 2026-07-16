# operator==(ranges::adjacent_transform_view::*sentinel*)

```cpp
template< bool OtherConst >
    requires std::sentinel_for</*inner-sentinel*/<Const>,
                               /*inner-iterator*/<OtherConst>>
friend constexpr bool operator==( const /*iterator*/<OtherConst>& x,
                                  const /*sentinel*/& y );  // (since C++23)
```

Compares the underlying iterator of `x` with the underlying sentinel of `y`.

Equivalent to: `return x. inner_ == y. inner_;`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`adjacent_transform_view::sentinel` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — an iterator to compare
- **y** — a sentinel to compare

### Return value

The result of comparison.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/sentinel/operator_cmp*
