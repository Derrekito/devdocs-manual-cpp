# operator==(ranges::elements_view::*sentinel*)

```cpp
template< bool OtherConst >
  requires std::sentinel_for<ranges::sentinel_t<Base>,
    ranges::iterator_t</*maybe-const*/<OtherConst, V>>>
friend constexpr bool operator==( const /*iterator*/<OtherConst>& x,
                                  const /*sentinel*/& y );  // (since C++20)
```

Compares the underlying iterator of `x` with the underlying sentinel of `y`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`elements_view::sentinel<Const>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

`x.base() == y.base()`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/sentinel/operator_cmp*
