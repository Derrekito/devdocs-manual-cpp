# iter_move(ranges::transform_view::*iterator*)

```cpp
friend constexpr decltype(auto) iter_move( const /*iterator*/& i )
    noexcept(/* see below */);  // (since C++20)
```

If `*i` is an lvalue reference, returns `std::move(*i)`; otherwise returns `*i`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`transform_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

`std::move(*i)` if `*i` is an lvalue reference, otherwise `*i`

### Exceptions

`noexcept` specification:

`noexcept(noexcept(std::invoke(*i.parent_->fun_, *i.current_)))`

where `*i.parent_->fun_` denotes the transformation function, `i.current_`
denotes the underlying iterator.

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/iter_move*
