# iter_move(ranges::enumerate_view::*iterator*)

```cpp
friend constexpr auto iter_move( const /*iterator*/& i ) noexcept(/* see below */)  // (since C++23)
```

Let `current_` be the underlying iterator, `pos_` be the underlying index, and
`Base` be the (possibly cv-qualified) type of the underlying sequence.

Equivalent to:

```cpp
template<class D, class B>
using tuple = std::tuple<D, ranges::range_rvalue_reference_t<B>>;

return tuple<difference_type, Base>(i.pos_, ranges::iter_move(i.current_));
```

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`enumerate_view::iterator<Const>` is an associated class of the arguments.

### Parameters

- **i** — iterator

### Return value

A tuple that contains an index and the result of applying `ranges::iter_move` to
the stored iterator.

### Exceptions

`noexcept` specification:

`noexcept( noexcept(ranges::iter_move(i.current_)) and
std::is_nothrow_move_constructible_v< ranges::range_rvalue_reference_t<Base>> )`

### See also

- **iter_move (C++20)** — casts the result of dereferencing an object to its
  associated rvalue reference type (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator/iter_move*
