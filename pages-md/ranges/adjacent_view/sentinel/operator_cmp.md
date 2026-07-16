# operator==(ranges::adjacent_view::*iterator*, ranges::adjacent_view::*sentinel*)

```cpp
template< bool OtherConst >
    requires std::sentinel_for<ranges::sentinel_t<Base>,
                               ranges::iterator_t<__maybe_const<OtherConst, V>>>
friend constexpr bool operator==( const /*iterator*/<OtherConst>& x,
                                  const /*sentinel*/& y );  // (since C++23)
```

Compares the underlying iterator of `x` with the underlying sentinel of `y`.

Equivalent to: `return x.current_.back() == y.end_`, where `current_` is
underlying array of iterators in `x`, and `end_` is the underlying sentinel in
`y`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`adjacent_view::sentinel<Const>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

`true` if the underlying iterator stored in `x` is the end iterator.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/sentinel/operator_cmp*
