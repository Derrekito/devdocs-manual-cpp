# operator==(ranges::transform_view::*sentinel*)

```cpp
friend constexpr bool operator==( const /*iterator*/<Const>& x, const /*sentinel*/& y );  // (since C++20)
```

Compares the underlying iterator of `x` with the underlying sentinel of `y`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`transform_view::sentinel<Const>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

`x.current_ == y.end_`, where `current_` denotes the underlying iterator, `end_`
denotes the underlying sentinel.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/sentinel/operator_cmp*
