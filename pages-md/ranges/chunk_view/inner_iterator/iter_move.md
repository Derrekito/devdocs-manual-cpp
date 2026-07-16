# iter_move(ranges::chunk_view::*inner-iterator*)

```cpp
friend constexpr auto iter_move( const /*inner-iterator*/& i ) noexcept(/* see below */)  // (since C++23)
```

Returns the result of applying `ranges::iter_move` to the stored inner iterator.

Let `parent_` be the underlying pointer to the enclosing `chunk_view`, and
`*i.parent_->current_` denote the cached underlying iterator of type
`ranges::iterator_t<V>`.

Equivalent to: `return ranges::iter_move(*i.parent_->current_);`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `chunk_view::inner-iterator`
is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

The result of applying `ranges::iter_move` to the stored iterator of type
`ranges::iterator_t<V>`.

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_move(*i.parent_->current_)))`

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/iter_move*
