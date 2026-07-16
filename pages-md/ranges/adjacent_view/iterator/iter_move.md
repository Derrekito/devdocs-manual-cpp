# iter_move(ranges::adjacent_view::*iterator*)

```cpp
friend constexpr auto iter_move( const /*iterator*/& i )
  noexcept(/* see description */);  // (since C++23)
```

Returns the result of applying `ranges::iter_move` to the underlying iterators.

Equivalent to: `return __tuple_transform(ranges::iter_move, i.current_);`, where
`current_` is an underlying array of iterators.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`adjacent_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

The result of applying `ranges::iter_move` to the underlying iterators.

### Exceptions

`noexcept` specification:

`noexcept( noexcept(ranges::iter_move(declval<const
ranges::iterator_t<Base>&>())) &&
std::is_nothrow_move_constructible_v<ranges::range_rvalue_reference_t<Base>> )`

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/iterator/iter_move*
