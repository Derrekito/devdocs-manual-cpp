# operator+,-(ranges::zip_transform_view::*iterator*)

```cpp
friend constexpr /*iterator*/ operator+( const /*iterator*/& i, difference_type n )
    requires ranges::random_access_range<Base>;  // (1) (since C++23)
friend constexpr /*iterator*/ operator+( difference_type n, const /*iterator*/& i )
    requires ranges::random_access_range<Base>;  // (2) (since C++23)
friend constexpr /*iterator*/ operator-( const /*iterator*/& i, difference_type n )
    requires ranges::random_access_range<Base>;  // (3) (since C++23)
friend constexpr difference_type operator-( const /*iterator*/& i,
                                            const /*iterator*/& j )
    requires std::sized_sentinel_for</*ziperator*/<Const>, /*ziperator*/<Const>>;  // (4) (since C++23)
```

Let `inner_` denote the underlying iterator.

1,2) Returns the iterator `i` incremented by `n`. Equivalent to: `return
   /*iterator*/(*i.parent_, i.inner_ + n);`.

3) Returns the iterator `i` decremented by `n`. Equivalent to: `return
   /*iterator*/(*i.parent_, i.inner_ - n);`.

4) Calculates the *distance* between `i` and `j`. Equivalent to `return i.inner_
   - j.inner_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`zip_transform_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i, j** — the iterators
- **n** — position relative to current location

### Return value

1,2) `/*iterator*/{ inner_ + n }`

3) `/*iterator*/{ inner_ - n }`

4) `i.inner_ - j.inner_`

### Example

### See also

- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/iterator/operator_arith2*
