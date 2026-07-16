# operator+(std::basic_const_iterator<Iter>)

```cpp
friend constexpr basic_const_iterator
    operator+( const basic_const_iterator& i, difference_type n )
  requires std::random_access_iterator<Iter>;  // (1) (since C++23)
friend constexpr basic_const_iterator
    operator+( difference_type n, const basic_const_iterator& i )
  requires std::random_access_iterator<Iter>;  // (2) (since C++23)
```

Returns the iterator `i` incremented by `n`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::basic_const_iterator<Iter>` is an associated class of the arguments..

### Parameters

- **i** — the iterator adaptor to increment
- **n** — the number of positions to increment the iterator

### Return value

`std::basic_const_iterator<Iter>(i.base() + n)`

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/operator%2B*
