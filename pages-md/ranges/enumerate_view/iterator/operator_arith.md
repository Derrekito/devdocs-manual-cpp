# std::ranges::enumerate_view<V>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++( int );  // (2) (since C++23)
constexpr /*iterator*/ operator++( int )
  requires ranges::forward_range<Base>;  // (3) (since C++23)
constexpr /*iterator*/& operator--()
  requires ranges::bidirectional_range<Base>;  // (4) (since C++23)
constexpr /*iterator*/ operator--( int )
  requires ranges::bidirectional_range<Base>;  // (5) (since C++23)
constexpr /*iterator*/& operator+=( difference_type n )
  requires ranges::random_access_range<Base>;  // (6) (since C++23)
constexpr /*iterator*/& operator-=( difference_type n )
  requires ranges::random_access_range<Base>;  // (7) (since C++23)
```

Increments or decrements the iterator.

Let `current_` be the underlying iterator and `pos_` be the underlying index.

1) Equivalent to `++current_; ++pos_; return *this;`

2) Equivalent to `++current_;`

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`

4) Equivalent to `--current_; --pos_; return *this;`

5) Equivalent to `auto tmp = *this; --*this; return tmp;`

6) Equivalent to `current_ += n; pos_ += n; return *this;`

7) Equivalent to `current_ -= n; pos_ -= n; return *this;`

### Parameters

- **n** — position relative to current location

### Return value

1,4,6,7) `*this`

2) (none)

3,5) a copy of `*this` that was made before the change

### See also

- **operator+operator- (C++23)** — performs iterator arithmetic (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator/operator_arith*
