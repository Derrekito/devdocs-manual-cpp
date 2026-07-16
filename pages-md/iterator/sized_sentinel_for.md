# std::sized_sentinel_for, std::disable_sized_sentinel_for

```cpp
template< class S, class I >
    concept sized_sentinel_for =
        std::sentinel_for<S, I> &&
        !std::disable_sized_sentinel_for<std::remove_cv_t<S>, std::remove_cv_t<I>> &&
        requires(const I& i, const S& s) {
            { s - i } -> std::same_as<std::iter_difference_t<I>>;
            { i - s } -> std::same_as<std::iter_difference_t<I>>;
        };  // (1) (since C++20)
template< class S, class I >
    inline constexpr bool disable_sized_sentinel_for = false;  // (2) (since C++20)
```

1) The `sized_sentinel_for` concept specifies that an object of the iterator
   type `I` and an object of the sentinel type `S` can be subtracted to compute
   the distance between them in constant time.

2) The `disable_sized_sentinel_for` variable template can be used to prevent
   iterators and sentinels that can be subtracted but do not actually model
   `sized_sentinel_for` from satisfying the concept. The variable template is
   allowed to be specialized for cv-unqualified non-array object type `S` and
   `I`, as long as at least one of which is a program-defined type. Such
   specializations shall be usable in constant expressions and have type `const
   bool`.

### Semantic requirements

Let `i` be an iterator of type `I`, and `s` a sentinel of type `S` such that
`[i, s)` denotes a range. Let `n` be the smallest number of applications of
`++i` necessary to make `bool(i == s)` be `true`. `I` and `S` model
`sized_sentinel_for<S, I>` only if:

- If `n` is representable by `std::iter_difference_t<I>`, then `s - i` is
  well-defined and equals `n`; and
- If `-n` is representable by `std::iter_difference_t<I>`, then `i - s` is
  well-defined and equals `-n`.
- Subtraction between `i` and `s` has constant time complexity.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### Implicit expression variations

A requires expression that uses an expression that is non-modifying for some
constant lvalue operand also requires implicit expression variations.

### See also

- **ranges::sized_range (C++20)** — specifies that a range knows its size in
  constant time (concept)
- **ranges::size (C++20)** — returns an integer equal to the size of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/iterator/sized_sentinel_for*
