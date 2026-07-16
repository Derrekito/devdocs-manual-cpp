# operator+(std::move_iterator)

```cpp
template< class Iter >
move_iterator<Iter>
    operator+( typename move_iterator<Iter>::difference_type n,
               const move_iterator<Iter>& it );  // (since C++11) (until C++17)
template< class Iter >
constexpr move_iterator<Iter>
    operator+( typename move_iterator<Iter>::difference_type n,
               const move_iterator<Iter>& it );  // (since C++17)
```

Returns the iterator `it` incremented by `n`.

This overload participates in overload resolution only if `it.base() + n` is
well-formed and has type `Iter`.
*(since C++20)*

### Parameters

- **n** — the number of positions to increment the iterator
- **it** — the iterator adaptor to increment

### Return value

The incremented iterator, that is `move_iterator<Iter>(it.base() + n)`.

### Example

### See also

-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++11)** — advances or decrements the iterator (public member function)
- **operator- (C++11)** — computes the distance between two iterator adaptors
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/move_iterator/operator%2B*
