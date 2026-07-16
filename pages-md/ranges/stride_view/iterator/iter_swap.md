# iter_swap(ranges::stride_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
  noexcept( /*see below*/ )
    requires std::indirectly_swappable<ranges::iterator_t<Base>>;  // (since C++23)
```

Swaps the objects pointed to by two underlying iterators (each denoted as
`current_`).

Equivalent to `ranges::iter_swap(x.current_, y.current_);`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`stride_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **x, y** — iterators

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_swap(x.current_, y.current_)))`

### Example

### See also

- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/iter_swap*
