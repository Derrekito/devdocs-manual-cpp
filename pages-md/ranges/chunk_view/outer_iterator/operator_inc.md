# std::ranges::chunk_view<V>::*outer-iterator*::operator++

```cpp
constexpr /*outer-iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++( int );  // (2) (since C++23)
```

Increments the iterator.

Let `parent_` be the underlying pointer to enclosing `chunk_view`.

1) Equivalent to: ranges::advance(*parent_->current_, parent_->remainder_,
   ranges::end(parent_->base_)); parent_->remainder_ = parent_->n_; return
   *this; Before invocation of this operator the expression `*this ==
   std::default_sentinel` must be `false`.

2) Equivalent to `++*this`.

### Parameters

(none)

### Return value

1) `*this`

2) (none)

### Example

### See also

- **operator- (C++23)** — calculates the number of chunks remained (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/outer_iterator/operator_inc*
