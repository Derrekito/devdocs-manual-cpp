# std::ranges::slide_view<V>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr /*iterator*/ operator++( int );  // (2) (since C++23)
constexpr /*iterator*/& operator--()
    requires ranges::bidirectional_range<Base>;  // (3) (since C++23)
constexpr /*iterator*/ operator--( int )
    requires ranges::bidirectional_range<Base>;  // (4) (since C++23)
constexpr /*iterator*/& operator+=( difference_type n )
    requires ranges::random_access_range<Base>;  // (5) (since C++23)
constexpr /*iterator*/& operator-=( difference_type n )
    requires ranges::random_access_range<Base>;  // (6) (since C++23)
```

Advances or decrements the iterator.

Let `current_` and `last_ele_` be the underlying iterators to the begin and end
of the sliding window.

1) Equivalent to: current_ = ranges::next(current_); last_ele_ =
   ranges::next(last_ele_); // if last_ele_ is present return *this; Before the
   invocation, the `current_` and `last_ele_` (if present) must be
   incrementable.

2) Equivalent to: `auto tmp = *this; ++*this; return tmp;`

3) Equivalent to: current_ = ranges::prev(current_); last_ele_ =
   ranges::prev(last_ele_); // if last_ele_ is present return *this; Before the
   invocation, the `current_` and `last_ele_` (if present) must be
   decrementable.

4) Equivalent to: `auto tmp = *this; --*this; return tmp;`

5) Equivalent to: current_ = current_ + n; last_ele_ = last_ele_ + n; // if
   last_ele_ is present return *this; Before the invocation, the expressions
   `current_ + n` and `last_ele_ + n` (if `last_ele_` is present) must have
   well-defined behavior.

6) Equivalent to: current_ = current_ - n; last_ele_ = last_ele_ - n; // if
   last_ele_ is present return *this; Before the invocation, the expressions
   `current_ - n` and `last_ele_ - n` (if `last_ele_` is present) must have
   well-defined behavior.

### Parameters

- **n** — position relative to current location

### Return value

1,3,5,6) `*this`

2,4) a copy of `*this` that was made before the change

### Example

### See also

- **operator+operator- (C++23)** — performs iterator arithmetic (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/iterator/operator_arith*
