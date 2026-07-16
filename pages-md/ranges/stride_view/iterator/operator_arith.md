# std::ranges::stride_view<V>::*iterator*<Const>::operator++,--,+=,-=

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

Let `current_`, `end_`, `stride_`, and `missing_` be the data members of the
iterator.

1) Equivalent to missing_ = ranges::advance(current_, stride_, end_); return
   *this Before the call `current_` should not be equal to `end_`.

2) Equivalent to `++*this;`.

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`.

4) Equivalent to ranges::advance(current_, missing_ - stride_); missing_ = 0;
   return *this;

5) Equivalent to `auto tmp = *this; --*this; return tmp;`.

6) Equivalent to if (n > 0) { ranges::advance(current_, stride_ * (n - 1));
   missing_ = ranges::advance(current_, stride_, end_); } else if (n < 0) {
   ranges::advance(current_, stride_ * n + missing_); missing_ = 0; } return
   *this; If `n > 0`, then before the call to this function the
   `ranges::distance(current_, end_)` must be greater than `stride_ * (n - 1)`.
   Note that if `n < 0`, the `ranges::distance(current_, end_)` is always
   greater than (non-positive) `stride_ * (n - 1)`.

7) Equivalent to `return *this += -n;`

### Parameters

- **n** — position relative to current location

### Return value

1,4,6,7) `*this`

2) (none)

3,5) a copy of `*this` that was made before the change

### Example

### See also

- **operator+operator- (C++23)** — performs iterator arithmetic (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator/operator_arith*
