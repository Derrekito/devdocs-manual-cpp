# std::ranges::cartesian_product_view<First, Vs...>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++( int );  // (2) (since C++23)
constexpr /*iterator*/ operator++( int )
  requires ranges::forward_range</*maybe-const*/<Const, First>>;  // (3) (since C++23)
constexpr /*iterator*/& operator--()
  requires /*cartesian-product-is-bidirectional*/<Const, First, Vs...>;  // (4) (since C++23)
constexpr /*iterator*/ operator--( int )
  requires /*cartesian-product-is-bidirectional*/<Const, First, Vs...>;  // (5) (since C++23)
constexpr /*iterator*/& operator+=( difference_type n )
  requires /*cartesian-product-is-random-access*/<Const, First, Vs...>;  // (6) (since C++23)
constexpr /*iterator*/& operator-=( difference_type n )
  requires /*cartesian-product-is-random-access*/<Const, First, Vs...>;  // (7) (since C++23)
```

Increments or decrements the iterator.

Let `current_` denote the underlying tuple of iterators and `parent_` denote the
underlying pointer to `cartesian_product_view`.

1) Equivalent to `next(); return *this;`

2) Equivalent to `++*this;`

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`

4) Equivalent to `prev(); return *this;`

5) Equivalent to `auto tmp = *this; --*this; return tmp;`
6) Sets the value of `*this` to `ret`, where `ret` is:

- if `n > 0`, the value of `*this` provided that `next` been called `n` times.
  Otherwise,
- if `n < 0`, the value of `*this` provided that `prev` been called `-n` times.
  Otherwise,
- the value of `*this` before the call.

The behavior is undefined if `n` is not in the range `[``ranges::distance(*this,
ranges::begin(*parent_))``,``ranges::distance(*this, ranges::end(*parent_))``)`.

7) Equivalent to `*this += -n; return *this;`.

### Parameters

- **n** — position relative to current location

### Return value

1,4,6,7) `*this`

2) (none)

3,5) a copy of `*this` that was made before the change.

### Complexity

6) Constant.

### Example

### See also

- **operator+operator- (C++23)** — performs iterator arithmetic (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator/operator_arith*
