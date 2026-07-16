# std::ranges::join_view<V>::*iterator*<Const>::operator++,--

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++20)
constexpr void operator++( int );  // (2) (since C++20)
constexpr /*iterator*/ operator++( int )
  requires /*ref-is-glvalue*/ && ranges::forward_range<Base> &&
           ranges::forward_range<ranges::range_reference_t<Base>>;  // (3) (since C++20)
constexpr iterator& operator--()
  requires /*ref-is-glvalue*/ && ranges::bidirectional_range<Base> &&
           ranges::bidirectional_range<ranges::range_reference_t<Base>> &&
           ranges::common_range<ranges::range_reference_t<Base>>;  // (4) (since C++20)
constexpr /*iterator*/ operator--( int )
  requires /*ref-is-glvalue*/ && ranges::bidirectional_range<Base> &&
           ranges::bidirectional_range<ranges::range_reference_t<Base>> &&
           ranges::common_range<ranges::range_reference_t<Base>>;  // (5) (since C++20)
```

Increments or decrements the underlying iterator.

Let `inner_` and `outer_` be the underlying iterators, and `parent_` be the
pointer to parent `ranges::join_view`, the constant `/*ref-is-glvalue*/` be
`std::is_reference_v<ranges::range_reference_t<Base>>`.

1) Let `/*inner-range*/` be:

- `*outer_`, if `/*ref-is-glvalue*/ == true`;
- `*parent_->inner_` otherwise.

Equivalent to:

```cpp
auto&& inner_rng = /*inner-range*/;
if (++inner_ == ranges::end(inner_rng)) {
    ++outer_;
    satisfy();
}
return *this;
```

2) Equivalent to: `++*this`.

3) Equivalent to: auto tmp = *this; ++*this; return tmp;

4) Equivalent to: if (outer_ == ranges::end(parent_->base_)) inner_ =
   ranges::end(*--outer_); while (inner_ == ranges::begin(*outer_)) inner_ =
   ranges::end(*--outer_); --inner_; return *this;

5) Equivalent to: auto tmp = *this; --*this; return tmp;

### Parameters

(none)

### Return value

1,4) `*this`

2) (none)

3,5) a copy of `*this` that was made before the change

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator/operator_arith*
