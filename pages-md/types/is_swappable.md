# std::is_swappable_with, std::is_swappable, std::is_nothrow_swappable_with, std::is_nothrow_swappable

```cpp
template< class T, class U >
struct is_swappable_with;  // (1) (since C++17)
template< class T >
struct is_swappable;  // (2) (since C++17)
template< class T, class U >
struct is_nothrow_swappable_with;  // (3) (since C++17)
template< class T >
struct is_nothrow_swappable;  // (4) (since C++17)
```

1) If the expressions `swap(std::declval<T>(), std::declval<U>())` and
   `swap(std::declval<U>(), std::declval<T>())` are both well-formed in
   unevaluated context after `using std::swap;` (see Swappable), provides the
   member constant `value` equal `true`. Otherwise, `value` is `false`. Access
   checks are performed as if from a context unrelated to either type.

2) If `T` is not a referenceable type (i.e., possibly cv-qualified `void` or a
   function type with a *cv-qualifier-seq* or a *ref-qualifier*), provides a
   member constant `value` equal to `false`. Otherwise, provides a member
   constant `value` equal to `std::is_swappable_with<T&, T&>::value`.

3) Same as (1), but evaluations of both expressions from (1) are known not to
   throw exceptions.

4) Same as (2), but uses `std::is_nothrow_swappable_with`.

`T` and `U` shall each be a complete type, (possibly cv-qualified) void, or an
array of unknown bound. Otherwise, the behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Helper variable templates

```cpp
template< class T, class U >
inline constexpr bool is_swappable_with_v = is_swappable_with<T, U>::value;  // (since C++17)
template< class T >
inline constexpr bool is_swappable_v = is_swappable<T>::value;  // (since C++17)
template< class T, class U >
inline constexpr bool is_nothrow_swappable_with_v = is_nothrow_swappable_with<T, U>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_swappable_v = is_nothrow_swappable<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is swappable with `U`, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

This trait does not check anything outside the immediate context of the swap
expressions: if the use of `T` or `U` would trigger template specializations,
generation of implicitly-defined special member functions etc, and those have
errors, the actual swap may not compile even if
`std::is_swappable_with<T,U>::value` compiles and evaluates to `true`.

### Example

### See also

- **swap** — swaps the values of two objects (function template)
- **is_move_assignableis_trivially_move_assignableis_nothrow_move_assignable
  (C++11)(C++11)(C++11)** — checks if a type has a move assignment operator
  (class template)
- **swappableswappable_with (C++20)** — specifies that a type can be swapped or
  that two types can be swapped with each other (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_swappable*
