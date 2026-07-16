# iter_move(ranges::zip_view::*iterator*)

```cpp
friend constexpr auto iter_move( const iterator& i ) noexcept(/* see below */);  // (since C++23)
```

Equivalent to: `return /*tuple-transform*/(ranges::iter_move, i.current_);`,
where `current_` denotes the underlying tuple-like object that holds iterators
to elements of adapted views.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `zip_view::iterator<Const>`
is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

`std::move(*i)` if `*i` is an lvalue reference, otherwise `*i`

### Exceptions

`noexcept` specification:

`noexcept( ( noexcept ( ranges::iter_move ( declval<const
ranges::iterator_t</*maybe-const*/<Const, Views>>&>() ) ) and ... ) and (
std::is_nothrow_move_constructible_v <
ranges::range_rvalue_reference_t</*maybe-const*/<Const, Views>> > and ... ) )`

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/iter_move*
