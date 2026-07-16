# operator-(ranges::chunk_view::*inner-iterator*)

```cpp
friend constexpr difference_type operator-( std::default_sentinel_t s,
                                            const /*inner-iterator*/& i )
    requires ranges::sized_sentinel_for<ranges::sentinel_t<V>,
                                        ranges::iterator_t<V>>;  // (1) (since C++23)
friend constexpr difference_type operator-( const /*inner-iterator*/& i,
                                            std::default_sentinel_t s )
    requires ranges::sized_sentinel_for<ranges::sentinel_t<V>,
                                        ranges::iterator_t<V>>;  // (2) (since C++23)
```

Calculates the distance (in number of underlying elements) between the iterator
and sentinel.

Let `parent_` be the underlying pointer to the enclosing `chunk_view`.

1) Equivalent to: `return ranges::min(i.parent_->remainder_,
   ranges::end(i.parent_->base_) - *i.parent_->current_);`

2) Equivalent to: `return -(s - i);`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::chunk_view::inner-iterator` is an associated class of the
arguments.

### Parameters

- **i** — the iterator
- **s** — the sentinel

### Return value

The distance between given iterator and sentinel.

### Example

### See also

- **operator++ (C++23)** — increments the iterator (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/inner_iterator/operator-*
