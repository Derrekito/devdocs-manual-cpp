# std::common_reference_with

```cpp
template< class T, class U >
concept common_reference_with =
    std::same_as<std::common_reference_t<T, U>, std::common_reference_t<U, T>> &&
    std::convertible_to<T, std::common_reference_t<T, U>> &&
    std::convertible_to<U, std::common_reference_t<T, U>>;  // (since C++20)
```

The concept `common_reference_with<T, U>` specifies that two types `T` and `U`
share a *common reference type* (as computed by `std::common_reference_t`) to
which both can be converted.

### Semantic requirements

T and U model std::common_reference_with<T, U> only if, given
equality-preserving expressions `t1`, `t2`, `u1` and `u2` such that
`decltype((t1))` and `decltype((t2))` are both `T` and `decltype((u1))` and
`decltype((u2))` are both `U`,

- `std::common_reference_t<T, U>(t1)` equals `std::common_reference_t<T, U>(t2)`
  if and only if `t1` equals `t2`; and
- `std::common_reference_t<T, U>(u1)` equals `std::common_reference_t<T, U>(u2)`
  if and only if `u1` equals `u2`.

In other words, the conversion to the common reference type must preserve
equality.

### See also

- **common_referencebasic_common_reference (C++20)** — determines the common
  reference type of a group of types (class template)
- **common_with (C++20)** — specifies that two types share a common type
  (concept)
- **common_type (C++11)** — determines the common type of a group of types
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/common_reference_with*
