# std::common_with

```cpp
template< class T, class U >
concept common_with =
    std::same_as<std::common_type_t<T, U>, std::common_type_t<U, T>> &&
    requires {
        static_cast<std::common_type_t<T, U>>(std::declval<T>());
        static_cast<std::common_type_t<T, U>>(std::declval<U>());
    } &&
    std::common_reference_with<
        std::add_lvalue_reference_t<const T>,
        std::add_lvalue_reference_t<const U>> &&
    std::common_reference_with<
        std::add_lvalue_reference_t<std::common_type_t<T, U>>,
        std::common_reference_t<
            std::add_lvalue_reference_t<const T>,
            std::add_lvalue_reference_t<const U>>>;  // (since C++20)
```

The concept `common_with<T, U>` specifies that two types `T` and `U` share a
*common type* (as computed by `std::common_type_t`) to which both can be
converted.

### Semantic requirements

T and U model std::common_with<T, U> only if, given equality-preserving
expressions `t1`, `t2`, `u1` and `u2` such that `decltype((t1))` and
`decltype((t2))` are both `T` and `decltype((u1))` and `decltype((u2))` are both
`U`,

- `std::common_type_t<T, U>(t1)` equals `std::common_type_t<T, U>(t2)` if and
  only if `t1` equals `t2`; and
- `std::common_type_t<T, U>(u1)` equals `std::common_type_t<T, U>(u2)` if and
  only if `u1` equals `u2`.

In other words, the conversion to the common type must preserve equality.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

### See also

- **common_type (C++11)** — determines the common type of a group of types
  (class template)
- **common_referencebasic_common_reference (C++20)** — determines the common
  reference type of a group of types (class template)
- **common_reference_with (C++20)** — specifies that two types share a common
  reference type (concept)

---
*Source: https://en.cppreference.com/w/cpp/concepts/common_with*
