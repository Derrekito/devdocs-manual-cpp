# operator==,<,>,<=,>=,<=>(ranges::slide_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y );  // (1) (since C++23)
friend constexpr bool operator<( const /*iterator*/& x, const /*iterator*/& y )
    requires ranges::random_access_range<Base>;  // (2) (since C++23)
friend constexpr bool operator>( const /*iterator*/& x, const /*iterator*/& y )
    requires ranges::random_access_range<Base>;  // (3) (since C++23)
friend constexpr bool operator<=( const /*iterator*/& x, const /*iterator*/& y )
    requires ranges::random_access_range<Base>;  // (4) (since C++23)
friend constexpr bool operator>=( const /*iterator*/& x, const /*iterator*/& y )
    requires ranges::random_access_range<Base>;  // (5) (since C++23)
friend constexpr auto operator<=>( const /*iterator*/& x, const /*iterator*/& y )
    requires ranges::random_access_range<Base> &&
             std::three_way_comparable<ranges::iterator_t<Base>>;  // (6) (since C++23)
```

Compares the underlying iterators.

Let `current_` and `last_ele_` be the underlying iterators to the begin and end
of the sliding window, respectively.

1) Equivalent to:

- `return x.last_ele_ == y.last_ele_;`, if `last_ele_` is present. Otherwise,
- `return x.current_ == y.current_;`.

2) Equivalent to `return x.current_ < y.current_;`.

3) Equivalent to `return y < x;`.

4) Equivalent to `return !(y < x);`.

5) Equivalent to `return !(x < y);`.

6) Equivalent to `return x.current_ <=> y.current_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::slide_view::iterator<Const>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x, y** — iterators to compare

### Return value

The result of comparison.

### Example

### See also

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `slide_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/iterator/operator_cmp*
