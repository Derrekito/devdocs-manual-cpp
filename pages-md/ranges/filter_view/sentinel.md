# std::ranges::filter_view<V,Pred>::*sentinel*

```cpp
class /*sentinel*/;  // (since C++20) (exposition only*)
```

The return type of `filter_view::end` when the underlying `view` type (`V`) does
not model `common_range`.

### Data members

- **`end_` (private)** — The sentinel of the underlying `view`. (exposition-only
  member object*)

### Member functions

- **(constructor) (C++20)** — constructs a sentinel (public member function)
- **base (C++20)** — returns the underlying sentinel (public member function)

## std::ranges::filter_view::*sentinel*::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++20)
constexpr explicit /*sentinel*/( filter_view& parent );  // (2) (since C++20)
```

1) Value-initializes `end_` via its default member initializer (`=
   ranges::sentinel_t<V>()`).

2) Initializes `end_` with `ranges::end(parent.base_)`.

## std::ranges::filter_view::*sentinel*::base

```cpp
constexpr ranges::sentinel_t<V> base() const;  // (since C++20)
```

Equivalent to `return end_;`.

### Non-member functions

- **operator== (C++20)** — compares the underlying iterator and the underlying
  sentinel (function)

## operator==(std::ranges::filter_view::*iterator*, std::ranges::filter_view::*sentinel*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x,
                                  const /*sentinel*/& y );  // (since C++20)
```

Equivalent to `return x.current_ == y.end_;`, where `current_` is the underlying
iterator wrapped in `filter_view::iterator`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::filter_view::sentinel` is an associated class of the arguments.

---
*Source: https://en.cppreference.com/w/cpp/ranges/filter_view/sentinel*
