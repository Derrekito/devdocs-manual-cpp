# iter_swap(ranges::chunk_view::*inner-iterator*)

```cpp
friend constexpr void iter_swap( const /*inner-iterator*/& x,
                                 const /*inner-iterator*/& y )
    noexcept(noexcept(ranges::iter_swap(*x.parent_->current_,
                                        *y.parent_->current_)))
    requires std::indirectly_swappable<ranges::iterator_t<V>>;  // (since C++23)
```

Applies `ranges::iter_swap` to the underlying cached iterators.

Let `parent_` be the underlying pointer to the enclosing `chunk_view`, and
`*i.parent_->current_` denote the underlying cached iterator of type
`ranges::iterator_t<V>`.

Equivalent to: `ranges::iter_swap(*x.parent_->current_, *y.parent_->current_);`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `chunk_view::inner-iterator`
is an associated class of the arguments.

### Parameters

- **x, y** — iterators to the elements to swap

### Return value

(none)

### See also

- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/iter_swap*
