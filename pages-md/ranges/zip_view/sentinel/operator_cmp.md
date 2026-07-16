# operator==(ranges::zip_view::*iterator*, ranges::zip_view::*sentinel*)

```cpp
template< bool OtherConst >
    requires (std::sentinel_for<
        ranges::sentinel_t</*maybe-const*/<Const, Views>>,
        ranges::iterator_t</*maybe-const*/<OtherConst, Views>>> && ...)
friend constexpr bool operator==( const /*iterator*/<OtherConst>& x,
                                  const /*sentinel*/& y  );  // (since C++23)
```

Compares the underlying tuple of iterators of `x` with the underlying tuple of
sentinels of `y`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `zip_view::sentinel<Const>`
is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x** — iterator to compare
- **y** — sentinel to compare

### Return value

Let `x.current_` denote the underlying tuple of iterators, and `y.end_` denote
the underlying tuple of sentinels.

Returns

- `true` if at least one underlying iterator, obtained by expression equivalent
  to `std::get<i>(x.current_)`, evaluates equal (using an appropriate
  `operator==`) to some underlying sentinel, obtained by expression equivalent
  to `std::get<i>(y.end_)`, for some index `i` in ranges `0 <= i <
  sizeof...(Views)`,
- `false` otherwise.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/sentinel/operator_cmp*
