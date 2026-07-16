# iter_swap(ranges::join_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
  noexcept( /*see below*/ )
    requires std::indirectly_swappable<InnerIter>;  // (since C++20)
```

Swaps the objects pointed to by two underlying iterators (denoted as `inner_`).

Equivalent to: `ranges::iter_swap(x.inner_, y.inner_);`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `join_view::iterator<Const>`
is an associated class of the arguments.

### Parameters

- **x, y** — iterators

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_swap(x.inner_, y.inner_)))`

### See also

- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator/iter_swap*
