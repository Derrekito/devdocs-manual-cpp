# operator==(ranges::zip_transform_view::*sentinel*)

```cpp
template< bool OtherConst >
    requires std::sentinel_for</*zentinel*/<Const>, /*ziperator*/<OtherConst>>
friend constexpr bool operator==( const /*iterator*/<OtherConst>& x,
                                  const /*sentinel*/& y );  // (since C++23)
```

Compares the underlying iterator of `x` with the underlying sentinel of `y`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`zip_transform_view::sentinel` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

`x.inner_ == y.inner_`, where `inner_` denotes the underlying iterator or
sentinel respectively.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/sentinel/operator_cmp*
