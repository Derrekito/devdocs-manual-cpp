# std::basic_const_iterator<Iter>::operator++,--,+=,-=

```cpp
constexpr basic_const_iterator& operator++();  // (1) (since C++23)
constexpr void operator++(int);  // (2) (since C++23)
constexpr basic_const_iterator operator++(int)
    requires std::forward_iterator<Iter>;  // (3) (since C++23)
constexpr basic_const_iterator& operator--()
    requires std::bidirectional_iterator<Iter>;  // (4) (since C++23)
constexpr basic_const_iterator operator--(int)
    requires std::bidirectional_iterator<Iter>;  // (5) (since C++23)
constexpr basic_const_iterator& operator+=( difference_type n )
    requires std::random_access_iterator<Iter>;  // (6) (since C++23)
constexpr basic_const_iterator& operator-=( difference_type n )
    requires std::random_access_iterator<Iter>;  // (7) (since C++23)
```

Increments or decrements the iterator, by applying the corresponding operation
on the underlying iterator.

1-3) Increments by one.

4,5) Decrements by one.

6,7) Advances the iterator by `n` or `-n` positions respectively.

### Return value

1,4,6,7) `*this`

3,5) A copy of `*this` that was made before the change

2) (none)

### Example

### See also

-
  **operator++operator++(int)operator+=operator+operator--operator--(int)operator-=operator-
  (C++11)** — advances or decrements the iterator (public member function of
  `std::move_iterator<Iter>`)

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/operator_arith*
