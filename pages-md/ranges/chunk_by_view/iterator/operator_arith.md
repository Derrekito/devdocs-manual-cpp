# std::ranges::chunk_by_view<V,Pred>::*iterator*::operator++,--

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr /*iterator*/ operator++(int);  // (2) (since C++23)
constexpr /*iterator*/& operator--() requires ranges::bidirectional_range<V>;  // (3) (since C++23)
constexpr /*iterator*/ operator--(int) requires ranges::bidirectional_range<V>;  // (4) (since C++23)
```

Increments or decrements the iterator.

Let `parent_`, `current_`, and `next_` be the appropriate underlying
(exposition-only) data-members of iterator.

Let `find-next` and `find-prev` be appropriate (exposition-only) member
functions of `ranges::chunk_by_view`.

1) Equivalent to: current_ = next_; next_ = parent_->/*find-next*/(current_);
   return *this;

The behavior is undefined if before the call to this operator `current_` is
   equal to `next_`.

2) Equivalent to: `auto tmp = *this; ++*this; return tmp;`

3) Equivalent to: next_ = current_; current_ = parent_->/*find-prev*/(next_);
   return *this;

4) Equivalent to: `auto tmp = *this; --*this; return tmp;`

### Parameters

(none)

### Return value

1,3) `*this`

2,4) a copy of `*this` that was made before the change.

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/iterator/operator_arith*
