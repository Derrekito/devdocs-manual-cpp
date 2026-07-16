# std::ranges::join_with_view<V,Pattern>::*iterator*<Const>::operator++,--

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++( int );  // (2) (since C++23)
constexpr /*iterator*/ operator++( int )
  requires std::is_reference_v<InnerBase> &&
           ranges::forward_range<Base> &&
           ranges::forward_range<InnerBase>;  // (3) (since C++23)
constexpr /*iterator*/& operator--()
  requires std::is_reference_v<InnerBase> &&
           ranges::bidirectional_range<Base> &&
           ranges::bidirectional_range<InnerBase> &&
           ranges::common_range<InnerBase> &&
           ranges::bidirectional_range<PatternBase> &&
           ranges::common_range<PatternBase>;  // (4) (since C++23)
constexpr /*iterator*/ operator--( int )
  requires std::is_reference_v<InnerBase> &&
           ranges::bidirectional_range<Base> &&
           ranges::bidirectional_range<InnerBase> &&
           ranges::common_range<InnerBase> &&
           ranges::bidirectional_range<PatternBase> &&
           ranges::common_range<PatternBase>;  // (5) (since C++23)
```

Increments or decrements the iterator.

1) Increments the stored inner iterator. (The inner iterator may point to either
`InnerBase` or `PatternBase`.)

- If the incremented inner iterator reaches the end of the pattern range, it is
  destroyed, and an iterator to the beginning of the next inner range is
  constructed.
- If the incremented inner iterator reaches the end of the inner range, the
  outer iterator is incremented, and if the outer iterator is not the end
  iterator, the inner iterator is destroyed and an iterator to the beginning of
  the pattern range is constructed.
- The above steps may be repeated (e.g. if the pattern is empty), until either
  the inner range is not empty, or the outer iterator reaches the end.

2) Equivalent to `++*this;`

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`
4) If the outer iterator is the end iterator, it is decremented. Then:

- If the stored inner iterator refers to the beginning of the inner range, it is
  destroyed, and an iterator to the end of the pattern range is constructed.
- If the stored inner iterator refers to the beginning of the pattern range, it
  is destroyed, the outer iterator is decremented, and an iterator to end of the
  inner range is constructed.
- The above steps may be repeated (e.g. if the pattern is empty), until the
  inner range is not empty.

Finally, the inner iterator is decremented.

5) Equivalent to `auto tmp = *this; --*this; return tmp;`

If `InnerBase` is not a reference, the inner range is stored in the parent
`join_with_view` for iteration. The inner range need not be movable.

If `InnerBase` is a reference, and the outer iterator reaches the end, the inner
iterator points to the beginning of the pattern range.

### Parameters

(none)

### Return value

1,4) `*this`

3,5) A copy of `*this` that was made before the change.

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator/operator_arith*
