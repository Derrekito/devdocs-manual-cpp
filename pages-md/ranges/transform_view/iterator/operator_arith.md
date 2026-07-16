# std::ranges::transform_view<V,F>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++20)
constexpr void operator++( int );  // (2) (since C++20)
constexpr /*iterator*/ operator++( int )
  requires ranges::forward_range<Base>;  // (3) (since C++20)
constexpr /*iterator*/& operator--()
  requires ranges::bidirectional_range<Base>;  // (4) (since C++20)
constexpr /*iterator*/ operator--( int )
  requires ranges::bidirectional_range<Base>;  // (5) (since C++20)
constexpr /*iterator*/& operator+=( difference_type n )
  requires ranges::random_access_range<Base>;  // (6) (since C++20)
constexpr /*iterator*/& operator-=( difference_type n )
  requires ranges::random_access_range<Base>;  // (7) (since C++20)
```

Increments or decrements the iterator.

Let `current_` be the underlying iterator.

1) Equivalent to `++current_; return *this;`

2) Equivalent to `++current_;`

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`

4) Equivalent to `--current_; return *this;`

5) Equivalent to `auto tmp = *this; --*this; return tmp;`

6) Equivalent to `current_ += n; return *this;`

7) Equivalent to `current_ -= n; return *this;`

### Parameters

- **n** — position relative to current location

### Return value

1,4,6,7) `*this`

3,5) a copy of `*this` that was made before the change

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/operator_arith*
