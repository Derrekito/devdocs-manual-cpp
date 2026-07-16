# operator==,<,>,<=,>=,<=>(ranges::stride_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, std::default_sentinel_t );  // (1) (since C++23)
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
  requires std::equality_comparable<ranges::iterator_t<Base>>;  // (2) (since C++23)
friend constexpr bool operator<( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (3) (since C++23)
friend constexpr bool operator>( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (4) (since C++23)
friend constexpr bool operator<=( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (5) (since C++23)
friend constexpr bool operator>=( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base>;  // (6) (since C++23)
friend constexpr auto operator<=>( const /*iterator*/& x, const /*iterator*/& y )
  requires ranges::random_access_range<Base> and
           std::three_way_comparable<ranges::iterator_t<Base>>;  // (7) (since C++23)
```

Compares the underlying iterators/sentinels.

Let `current_` be the underlying iterator, and `end_` be the underlying
sentinel.

1) Equivalent to `return x.current_ == x.end_;`.

2) Equivalent to `return x.current_ == y.current_;`.

3) Equivalent to `return x.current_ < y.current_;`.

4) Equivalent to `return y < x;`

5) Equivalent to `return !(y < x);`

6) Equivalent to `return !(x < y);`

7) Equivalent to `return x.current_ <=> y.current_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::stride_view::iterator<Const>` is an associated class of the
arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **x, y** — iterators to compare

### Return value

result of comparison

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/operator_cmp*
