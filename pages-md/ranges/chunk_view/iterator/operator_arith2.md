# operator+,-(ranges::chunk_view::*iterator*)

```cpp
friend constexpr /*iterator*/ operator+( const /*iterator*/& i,
                                         difference_type pos )
    requires ranges::random_access_range<Base>;  // (1) (since C++23)
friend constexpr /*iterator*/ operator+( difference_type pos,
                                         const /*iterator*/& i )
    requires ranges::random_access_range<Base>;  // (2) (since C++23)
friend constexpr /*iterator*/ operator-( const /*iterator*/& i,
                                         difference_type pos )
    requires ranges::random_access_range<Base>;  // (3) (since C++23)
friend constexpr difference_type operator-( const /*iterator*/& i,
                                            const /*iterator*/& j )
    requires std::sized_sentinel_for<ranges::iterator_t<Base>,
                                     ranges::iterator_t<Base>>;  // (4) (since C++23)
friend constexpr difference_type operator-( std::default_sentinel_t,
                                            const /*iterator*/& i )
    requires std::sized_sentinel_for<ranges::sentinel_t<Base>,
                                     ranges::iterator_t<Base>>;  // (5) (since C++23)
friend constexpr difference_type operator-( const /*iterator*/& i,
                                            std::default_sentinel_t )
    requires std::sized_sentinel_for<ranges::sentinel_t<Base>,
                                     ranges::iterator_t<Base>>;  // (6) (since C++23)
```

Performs iterator arithmetic or calculates the distance.

Let `current_`, `end_`, `n_`, and `missing_` be the underlying data members.

Equivalent to:

1,2) `auto r = i; r += pos; return r;`.

3) `auto r = i; r -= pos; return r;`.

4) `return (i.current_ - j.current_ + i.missing_ - j.missing_) / i.n_;`.

5) `return /*div-ceil*/(i.end_ - i.current_, i.n_);`.

6) `return -(y - x);`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`chunk_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i, j** — the iterators
- **pos** — the position relative to current location

### Return value

1,2) An incremented iterator.

3) A decremented iterator.

4) A distance (in number of elements, i.e. chunks) between given iterators.

5,6) A distance (in number of elements) between given iterator and sentinel.

### Example

### See also

- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/iterator/operator_arith2*
