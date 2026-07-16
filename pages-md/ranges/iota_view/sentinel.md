# std::ranges::iota_view<W, Bound>::*sentinel*

```cpp
struct /*sentinel*/;  // (since C++20) (exposition only*)
```

The return type of `iota_view::end`, used when the `iota_view` is bounded (i.e.
`Bound` is not `std::unreachable_sentinel_t`) and `Bound` and `W` are not the
same type.

### Data members

- **`bound_` (private)** — Typically, a numeric object of type `Bound`.
  (exposition-only member object*)

### Member functions

## std::ranges::iota_view::*sentinel*::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++20)
constexpr explicit /*sentinel*/( Bound bound );  // (2) (since C++20)
```

1) Value-initializes `bound_` via its default member initializer (`= Bound()`).

2) Initializes `bound_` with `bound`.

### Non-member functions

In the following descriptions, `value_` is an underlying data member of
`iterator` from which its `operator*` copies.

## operator==(std::ranges::iota_view::*iterator*, std::ranges::iota_view::*sentinel*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x,
                                  const /*sentinel*/& y );  // (since C++20)
```

Equivalent to: `return x.value_ == y.bound_;`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `sentinel` is an associated
class of the arguments.

## operator-(std::ranges::iota_view::*iterator*, std::ranges::iota_view::*sentinel*)

```cpp
friend constexpr std::iter_difference_t<W>
    operator-(const /*iterator*/& x, const /*sentinel*/& y)
    requires std::sized_sentinel_for<Bound, W>;  // (1) (since C++20)
friend constexpr std::iter_difference_t<W>
    operator-(const /*sentinel*/& x, const /*iterator*/& y)
    requires std::sized_sentinel_for<Bound, W>;  // (2) (since C++20)
```

1) Equivalent to `return x.value_ - y.bound_;`.

2) Equivalent to `return -(y.value_ - x.bound_);`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `sentinel` is an associated
class of the arguments.

---
*Source: https://en.cppreference.com/w/cpp/ranges/iota_view/sentinel*
