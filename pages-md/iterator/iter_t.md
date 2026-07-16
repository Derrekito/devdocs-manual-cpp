# std::iter_value_t, std::iter_reference_t, std::iter_const_reference_t, std::iter_difference_t, std::iter_rvalue_reference_t, std::iter_common_reference_t

```cpp
template< class T >
concept /*dereferenceable*/ = /* see below */;  // (exposition only*)
template< class T >
using iter_value_t = /* see below */;  // (1) (since C++20)
template< /*dereferenceable*/ T >
using iter_reference_t = decltype(*std::declval<T&>());  // (2) (since C++20)
template< std::indirectly_readable T >
using iter_const_reference_t = std::common_reference_t<const std::iter_value_t<T>&&,
                                                       std::iter_reference_t<T>>;  // (3) (since C++23)
template< class T >
using iter_difference_t = /* see below */;  // (4) (since C++20)
template< /*dereferenceable*/ T>
    requires /* see below */
using iter_rvalue_reference_t = decltype(ranges::iter_move(std::declval<T&>()));  // (5) (since C++20)
template< std::indirectly_readable T >
using iter_common_reference_t = std::common_reference_t<std::iter_reference_t<T>,
                                                        std::iter_value_t<T>&>;  // (6) (since C++20)
```

Compute the associated types of an iterator. The exposition-only concept
`dereferenceable` is satisfied if and only if the expression
`*std::declval<T&>()` is valid and has a referenceable type (in particular, not
`void`).

1) Computes the *value type* of `T`. If
   `std::iterator_traits<std::remove_cvref_t<T>>` is not specialized, then
   `std::iter_value_t<T>` is
   `std::indirectly_readable_traits<std::remove_cvref_t<T>>::value_type`.
   Otherwise, it is `std::iterator_traits<std::remove_cvref_t<T>>::value_type`.

2) Computes the *reference type* of `T`.

3) Computes the *const reference type* of `T`.

4) Computes the *difference type* of `T`. If
   `std::iterator_traits<std::remove_cvref_t<T>>` is not specialized, then
   `std::iter_difference_t<T>` is
   `std::incrementable_traits<std::remove_cvref_t<T>>::difference_type`.
   Otherwise, it is
   `std::iterator_traits<std::remove_cvref_t<T>>::difference_type`.

5) Computes the *rvalue reference type* of `T`. The "see below" portion of the
   constraint on this alias template is satisfied if and only if the expression
   `ranges::iter_move(std::declval<T&>())` is valid and has a referenceable type
   (in particular, not `void`).

6) Computes the *common reference type* of `T`. This is the common reference
   type between its reference type and an lvalue reference to its value type.

### See also

- **indirectly_readable (C++20)** — specifies that a type is indirectly readable
  by applying operator `*` (concept)
- **weakly_incrementable (C++20)** — specifies that a `semiregular` type can be
  incremented with pre- and post-increment operators (concept)
- **indirectly_readable_traits (C++20)** — computes the value type of an
  `indirectly_readable` type (class template)
- **incrementable_traits (C++20)** — computes the difference type of a
  `weakly_incrementable` type (class template)
- **iterator_traits** — provides uniform interface to the properties of an
  iterator (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/iter_t*
