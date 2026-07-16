# iter_move(ranges::cartesian_product_view::*iterator*)

```cpp
friend constexpr auto iter_move( const /*iterator*/& i ) noexcept(/* see below */)  // (since C++23)
```

Let `current_` be the underlying tuple of iterators.

Equivalent to: `return /*tuple-transform*/(ranges::iter_move, i.current_);`

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`cartesian_product_view::iterator<Const>` is an associated class of the
arguments.

### Parameters

- **i** — iterator

### Return value

A tuple that contains the result of applying `ranges::iter_move` to the stored
underlying iterators converted to the return type, as described above.

### Exceptions

The exception specification is equivalent to the logical AND of the following
expressions:

- `noexcept(ranges::iter_move(std::get<N>(i.current_)))` for every integer `0 ≤
  N ≤ sizeof...(Vs)`,
-
  `std::is_nothrow_move_constructible_v<ranges::range_rvalue_reference_t</*maybe-const*/<Const,
  T>>>` for every type `T` in `First, Vs...`.

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/iter_move*
