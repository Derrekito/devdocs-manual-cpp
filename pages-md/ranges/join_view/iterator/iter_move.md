# iter_move(ranges::join_view::*iterator*)

```cpp
friend constexpr decltype(auto) iter_move( const /*iterator*/& i )
    noexcept( /*see below*/ );  // (since C++20)
```

Casts the result of dereferencing the underlying iterator `i.inner_` to its
associated rvalue reference type.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `join_view::iterator<Const>`
is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

Equivalent to: `ranges::iter_move(i.inner_)`.

### Exceptions

`noexcept` specification:

`noexcept(noexcept(ranges::iter_move(i.inner_)))`

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator/iter_move*
