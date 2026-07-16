# std::incrementable_traits

```cpp
template< class I >
struct incrementable_traits { };  // (1) (since C++20)
template< class T >
    requires std::is_object_v<T>
struct incrementable_traits<T*>;  // (2) (since C++20)
template< class T >
struct incrementable_traits<const T> : incrementable_traits<T> { };  // (3) (since C++20)
template< class T >
  requires requires { typename T::difference_type; }
struct incrementable_traits<T>;  // (4) (since C++20)
template< class T >
  requires (!requires { typename T::difference_type; }) &&
           requires(const T& a, const T& b) { { a - b } -> std::integral; }
struct incrementable_traits<T>;  // (5) (since C++20)
```

Computes the associated difference type of the type `I`, if any. Users may
specialize `incrementable_traits` for a program-defined type.

1) Primary template is an empty struct.

2) Specialization for pointers. Provides a member type `difference_type` equal
   to `std::ptrdiff_t`.

3) Specialization for const-qualified types.

4) Specialization for types that define a public and accessible member type
   `difference_type`. Provides a member type `difference_type` equal to
   `T::difference_type`.

5) Specialization for types that do not define a public and accessible member
   type `difference_type` but do support subtraction. Provides a member type
   `difference_type` equal to `std::make_signed_t<decltype(std::declval<T>() -
   std::declval<T>())>`. The implicit expression variations rule (see below)
   applies to the expression `a - b`.

### Implicit expression variations

A requires expression that uses an expression that is non-modifying for some
constant lvalue operand also requires implicit expression variations.

### Example

### See also

- **weakly_incrementable (C++20)** — specifies that a `semiregular` type can be
  incremented with pre- and post-increment operators (concept)
-
  **iter_value_titer_reference_titer_const_reference_titer_difference_titer_rvalue_reference_titer_common_reference_t
  (C++20)(C++20)(C++23)(C++20)(C++20)(C++20)** — computes the associated types
  of an iterator (alias template)
- **iterator_traits** — provides uniform interface to the properties of an
  iterator (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/incrementable_traits*
