# std::ranges::split_view<V,Pattern>::*sentinel*

```cpp
class /*sentinel*/;  // (since C++20) (exposition only*)
```

The return type of `split_view::end` when the underlying `view` type (`V`) does
not models `common_range`.

### Data members

- **`end_` (private)** — An object of type `ranges::sentinel_t<V>` that is the
  sentinel of the underlying `view`. (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a sentinel (public member function)

## std::ranges::split_view::*sentinel*::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++20)
constexpr explicit /*sentinel*/( ranges::split_view& parent );  // (2) (since C++20)
```

1) Value-initializes `end_` via its default member initializer (`=
   ranges::sentinel_t<V>()`).

2) Initializes `end_` with `ranges::end(parent.base_)`.

### Non-member functions

- **operator== (C++20)** — compares the underlying iterator and the underlying
  sentinel (function)

## operator==(std::ranges::split_view::*iterator*, std::ranges::split_view::*sentinel*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x,
                                  const /*sentinel*/& y );  // (since C++20)
```

Equivalent to `return x.cur_ == y.end_ and !x.trailing_empty_;`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::split_view::sentinel` is an associated class of the arguments.

---
*Source: https://en.cppreference.com/w/cpp/ranges/split_view/sentinel*
