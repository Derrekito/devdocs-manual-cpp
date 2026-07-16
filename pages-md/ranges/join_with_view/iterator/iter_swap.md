# iter_swap(ranges::join_with_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
    requires std::indirectly_swappable<ranges::iterator_t<InnerBase>,
                                       ranges::iterator_t<PatternBase>>;  // (since C++23)
```

Applies `ranges::iter_swap` to the stored inner iterators.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`join_with_view::iterator<Const>` is an associated class of the arguments.

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
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator/iter_swap*
