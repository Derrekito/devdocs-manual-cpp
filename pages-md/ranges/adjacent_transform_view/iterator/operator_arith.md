# std::ranges::adjacent_transform_view<V,F,N>::*iterator*<Const>::operator++,--,+=,-=

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr /*iterator*/ operator++(int);  // (2) (since C++23)
constexpr /*iterator*/& operator--()
    requires ranges::bidirectional_range<Base>;  // (3) (since C++23)
constexpr /*iterator*/ operator--( int )
    requires ranges::bidirectional_range<Base>;  // (4) (since C++23)
constexpr /*iterator*/& operator+=( difference_type n )
    requires ranges::random_access_range<Base>;  // (5) (since C++23)
constexpr /*iterator*/& operator-=( difference_type n )
    requires ranges::random_access_range<Base>;  // (6) (since C++23)
```

Increments or decrements the iterator.

Let `inner_` be the underlying iterator and `Base` be the exposition-only member
type.

Equivalent to:

1) `++inner_; return *this;`

2) `auto tmp = *this; ++*this; return tmp;`

3) `--inner_; return *this;`

4) `auto tmp = *this; --*this; return tmp;`

5) `inner_ += n; return *this;`

6) `inner_ -= n; return *this;`

### Parameters

- **n** — position relative to current location

### Return value

1,3,5,6) `*this`

2,4) a copy of `*this` that was made before the change

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/iterator/operator_arith*
