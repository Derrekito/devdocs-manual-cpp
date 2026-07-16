# iter_swap(ranges::cartesian_product_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
    noexcept (/* see description */)
    requires (std::indirectly_swappable<
        ranges::iterator_t</*maybe-const*/<Const, First>>> and ... and
            std::indirectly_swappable<ranges::iterator_t</*maybe-const*/<Const, Vs>>>);  // (since C++23)
```

Applies `ranges::iter_swap` to the stored underlying iterators. Formally, for
every integer `0 ≤ i ≤ sizeof...(Vs)`, performs
`ranges::iter_swap(std::get<i>(x.current_), std::get<i>(y.current_))`, where
`current_` is the underlying tuple of iterators.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`cartesian_product_view::iterator<Const>` is an associated class of the
arguments.

### Parameters

- **x, y** — iterators to the elements to swap

### Return value

(none)

### Exceptions

The exception specification is equivalent to the logical AND of the expression
`noexcept(ranges::iter_swap(std::get<i>(x.current_), std::get<i>(y.current_)))`
for every integer `0 ≤ i ≤ sizeof...(Vs)`.

### See also

- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/iter_swap*
