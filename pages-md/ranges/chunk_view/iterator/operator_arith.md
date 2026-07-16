# std::ranges::chunk_view<V>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr /*iterator*/ operator++( int );  // (2) (since C++23)
constexpr /*iterator*/& operator--()
    requires ranges::bidirectional_range<Base>;  // (3) (since C++23)
constexpr /*iterator*/ operator--( int )
    requires ranges::bidirectional_range<Base>;  // (4) (since C++23)
constexpr /*iterator*/& operator+=( difference_type x )
    requires ranges::random_access_range<Base>;  // (5) (since C++23)
constexpr /*iterator*/& operator-=( difference_type x )
    requires ranges::random_access_range<Base>;  // (6) (since C++23)
```

Advances or decrements the iterator.

Let `current_`, `end_`, and `n_` be the underlying data members of
`chunk_view::iterator`.

1) Equivalent to: missing_ = ranges::advance(current_, n_, end_); return *this;
   Before the invocation the expression `current_ != end_` must be `true`,
   otherwise the behavior is undefined.

2) Equivalent to: `auto tmp = *this; ++*this; return tmp;`.

3) Equivalent to: ranges::advance(current_, missing_ - n_); missing_ = 0; return
   *this;

4) Equivalent to: `auto tmp = *this; --*this; return tmp;`.

5) Equivalent to: if (x > 0) { ranges::advance(current_, n_ * (x - 1)); missing_
   = ranges::advance(current_, n_, end_); } else if (x < 0) {
   ranges::advance(current_, n_ * x + missing_); missing_ = 0; } return *this;
   If `x` is positive, then before the invocation the expression
   `ranges::distance(current_, end_) > n_ * (x - 1)` must be `true` (i.e.,
   informally, the requested chunk should be "inside" the underlying sequence).
   If `x` is negative, this precondition is always met.

6) Equivalent to: `return *this += -x;`.

### Parameters

- **x** — a position relative to current location

### Return value

1,3,5,6) `*this`

2,4) a copy of `*this` that was made before the change

### Example

### See also

- **operator+operator- (C++23)** — performs iterator arithmetic (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/iterator/operator_arith*
