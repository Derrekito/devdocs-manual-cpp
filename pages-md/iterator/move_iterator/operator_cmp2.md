# operator==(std::move_iterator<Iter>, std::move_sentinel)

```cpp
template< std::sentinel_for<Iter> S >
    friend constexpr bool
        operator==( const move_iterator& i, const std::move_sentinel<S>& s );  // (since C++20)
```

Compare a `move_iterator` and a `move_sentinel`.

This function template is not visible to ordinary unqualified or qualified
lookup, and can only be found by argument-dependent lookup when
`std::move_iterator<Iter>` is an associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **i** — `std::move_iterator<Iter>`
- **s** — `std::move_sentinel<S>`, where `S` models `std::sentinel_for<Iter>`

### Return value

`i.base() == s.base()`

### Example

### See also

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++11)(C++11)(removed in C++20)(C++11)(C++11)(C++11)(C++11)(C++20)** —
  compares the underlying iterators (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/operator_cmp2*
