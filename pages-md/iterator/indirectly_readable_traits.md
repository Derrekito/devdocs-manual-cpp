# std::indirectly_readable_traits

```cpp
template< class I >
struct indirectly_readable_traits {};  // (1) (since C++20)
template< class T >
struct indirectly_readable_traits<T*>;  // (2) (since C++20)
template< class I >
    requires std::is_array_v<I>
struct indirectly_readable_traits<I>;  // (3) (since C++20)
template< class T >
struct indirectly_readable_traits<const T> :
    indirectly_readable_traits<T> {};  // (4) (since C++20)
template</*has-member-value-type*/ T>
struct indirectly_readable_traits<T>;  // (5) (since C++20)
template</*has-member-element-type*/ T>
struct indirectly_readable_traits<T>;  // (6) (since C++20)
template</*has-member-value-type*/ T>
    requires /*has-member-element-type*/<T>
struct indirectly_readable_traits<T> {};  // (7) (since C++20)
template</*has-member-value-type*/ T>
    requires /*has-member-element-type*/<T> &&
             std::same_as<std::remove_cv_t<typename T::element_type>,
                          std::remove_cv_t<typename T::value_type>>
struct indirectly_readable_traits<T>;  // (8) (since C++20)
```

Computes the associated value type of the type `I`, if any. Users may specialize
`indirectly_readable_traits` for a program-defined type.

1) Primary template has no member `value_type`.

2) Specialization for pointers. If `T` is an object type, provides a member type
   `value_type` equal to `std::remove_cv_t<T>`. Otherwise, there is no member
   `value_type`.

3) Specialization for array types. Provides a member type `value_type` equal to
   `std::remove_cv_t<std::remove_extent_t<I>>`.

4) Specialization for const-qualified types.

5) Specialization for types that define a public and accessible member type
   `value_type` (e.g., `std::reverse_iterator`). If `T::value_type` is an object
   type, provides a member type `value_type` equal to `std::remove_cv_t<typename
   T::value_type>`. Otherwise, there is no member `value_type`.

6) Specialization for types that define a public and accessible member type
   `element_type` (e.g., `std::shared_ptr`). If `T::element_type` is an object
   type, provides a member type `value_type` equal to `std::remove_cv_t<typename
   T::element_type>`. Otherwise, there is no member `value_type`.

7,8) Specialization for types that define public and accessible member types
   `value_type` and `element_type` (e.g., `std::span`). If both `T::value_type`
   and `T::element_type` are object types and they become the same type after
   removing cv-qualifiers on the top-level, provides a member type `value_type`
   equal to `std::remove_cv_t<typename T::value_type>`. Otherwise, there is no
   member `value_type`.

### Notes

`value_type` is intended for use with `indirectly_readable` types such as
iterators. It is not intended for use with ranges.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3446 | C++20 | specializations were ambiguous for types containing both
      `value_type` and `element_type` member types | a disambiguating
      specialization added
  LWG 3541 | C++20 | LWG 3446 introduced hard error for ambiguous cases that
      `value_type` and `element_type` are different | made resulting
      substitution failure

### See also

- **indirectly_readable (C++20)** — specifies that a type is indirectly readable
  by applying operator `*` (concept)
-
  **iter_value_titer_reference_titer_const_reference_titer_difference_titer_rvalue_reference_titer_common_reference_t
  (C++20)(C++20)(C++23)(C++20)(C++20)(C++20)** — computes the associated types
  of an iterator (alias template)
- **iterator_traits** — provides uniform interface to the properties of an
  iterator (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirectly_readable_traits*
