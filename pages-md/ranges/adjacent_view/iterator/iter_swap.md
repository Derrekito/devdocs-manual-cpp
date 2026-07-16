# iter_swap(ranges::adjacent_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
    noexcept( /*see below*/ )
        requires std::indirectly_swappable<ranges::iterator_t<Base>>;  // (since C++23)
```

Swaps the objects pointed to by two underlying arrays of iterators (denoted as
`current_`).

Equivalent to:

```cpp
for (std::size_t i{}; i != N; ++i)
{
    std::ranges::iter_swap(x.current_[i], y.current_[i]);
}
```

The behavior is undefined if before the operation none of the iterators in
`x.current_` is equal to an iterator in `y.current_`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`adjacent_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **x, y** — iterators

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept(std::ranges::iter_swap(declval<ranges::iterator_t<Base>>(),
declval<ranges::iterator_t<Base>>()))`

### See also

- **iter_swap (C++20)** — swaps the values referenced by two dereferenceable
  objects (customization point object)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/iterator/iter_swap*
