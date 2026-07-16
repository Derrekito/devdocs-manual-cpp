# iter_move(ranges::join_with_view::*iterator*)

```cpp
friend constexpr decltype(auto) iter_move( const /*iterator*/& i );  // (since C++23)
```

Returns the result of applying `ranges::iter_move` to the stored inner iterator.
The result is implicitly converted to:

```cpp
std::common_reference_t<ranges::range_rvalue_reference_t<InnerBase>,
                        ranges::range_rvalue_reference_t<PatternBase>>
```

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`join_with_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

The result of applying `ranges::iter_move` to the stored inner iterator,
implicitly converted to the return type.

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator/iter_move*
