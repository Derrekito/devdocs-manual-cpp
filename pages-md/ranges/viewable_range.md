# std::ranges::viewable_range

```cpp
template<class T>
concept viewable_range =
  ranges::range<T> &&
  ((ranges::view<std::remove_cvref_t<T>> &&
    std::constructible_from<std::remove_cvref_t<T>, T>) ||
   (!ranges::view<std::remove_cvref_t<T>> &&
    (std::is_lvalue_reference_v<T> ||
     (std::movable<std::remove_reference_t<T>> && !/*is-initializer-list*/<T>))));  // (since C++20)
```

The `viewable_range` concept is a refinement of `range` that describes a range
that can be converted into a `view` through `views::all`.

The constant `/*is-initializer-list*/<T>` is `true` if and only if
`std::remove_cvref_t<T>` is a specialization of `std::initializer_list`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3481 | C++20 | `viewable_range` accepted an lvalue of a move-only view |
      rejects
  P2415R2 | C++20 | `viewable_range` only accepted non-`view` rvalues that are
      `borrowed_range` | accepts more types

### See also

- **views::all_tviews::all (C++20)** — a `view` that includes all elements of a
  `range` (alias template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/viewable_range*
