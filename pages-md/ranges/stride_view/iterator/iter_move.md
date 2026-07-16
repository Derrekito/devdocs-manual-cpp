# iter_move(ranges::stride_view::*iterator*)

```cpp
friend constexpr ranges::range_rvalue_reference_t<Base>
    iter_move( const /*iterator*/& i ) noexcept(/* see below */);  // (since C++23)
```

Returns the result of applying `ranges::iter_move` to the underlying iterator
`current_`.

Equivalent to `return ranges::iter_move(i.current_);`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`stride_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

The result of applying `ranges::iter_move` to the stored iterator.

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_move(i.current_)))`

### Example

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/iter_move*
