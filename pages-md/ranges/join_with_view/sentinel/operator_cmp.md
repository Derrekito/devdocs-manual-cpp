# operator==(ranges::join_with_view::*iterator*, ranges::join_with_view::*sentinel*)

```cpp
friend constexpr bool operator==( const /*iterator*/<Const>& x, const /*sentinel*/& y );  // (since C++23)
```

Compares the underlying iterator of `x` with the underlying sentinel of `y`. The
comparison returns true if the underlying outer iterator stored in `x` is the
end iterator.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`join_with_view::sentinel<Const>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

`x.outer_it_ == y.end_`, where `outer_it_` denotes the underlying outer
iterator, `end_` denotes the underlying sentinel.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/sentinel/operator_cmp*
