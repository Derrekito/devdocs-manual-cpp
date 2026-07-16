# iter_swap(ranges::zip_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
    noexcept(/* see below */)
    requires (std::indirectly_swappable<ranges::iterator_t<
      /*maybe-const*/<Const, Views>>> && ...);  // (since C++23)
```

Performs `ranges::iter_swap(std::get<i>(x.current_), std::get<i>(y.current_))`
for every integer `i` in `[0, sizeof...(Views))`, where `current_` denotes the
underlying tuple-like object that holds iterators to elements of adapted views.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `zip_view::iterator<Const>`
is an associated class of the arguments.

### Parameters

- **x, y** — iterators to the elements to swap

### Return value

(none)

### Exceptions

`noexcept` specification:

`noexcept( (noexcept(ranges::iter_swap( declval<const
ranges::iterator_t</*maybe-const*/<Const, Views>>&>(), declval <const
ranges::iterator_t</*maybe-const*/<Const, Views>>&>())) &&...))`

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/iter_swap*
