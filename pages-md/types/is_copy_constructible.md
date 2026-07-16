# std::is_copy_constructible, std::is_trivially_copy_constructible, std::is_nothrow_copy_constructible

```cpp
template< class T >
struct is_copy_constructible;  // (1) (since C++11)
template< class T >
struct is_trivially_copy_constructible;  // (2) (since C++11)
template< class T >
struct is_nothrow_copy_constructible;  // (3) (since C++11)
```

1) If `T` is not a referenceable type (i.e., possibly cv-qualified `void` or a
   function type with a *cv-qualifier-seq* or a *ref-qualifier*), provides a
   member constant `value` equal to `false`. Otherwise, provides a member
   constant `value` equal to `std::is_constructible<T, const T&>::value`.

2) Same as (1), but uses `std::is_trivially_constructible<T, const T&>`.

3) Same as (1), but uses `std::is_nothrow_constructible<T, const T&>`.

`T` shall be a complete type, (possibly cv-qualified) void, or an array of
unknown bound. Otherwise, the behavior is undefined.

If an instantiation of a template above depends, directly or indirectly, on an
incomplete type, and that instantiation could yield a different result if that
type were hypothetically completed, the behavior is undefined.

The behavior of a program that adds specializations for any of the templates
described on this page is undefined.

### Helper variable templates

```cpp
template< class T >
inline constexpr bool is_copy_constructible_v = is_copy_constructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_trivially_copy_constructible_v = is_trivially_copy_constructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_copy_constructible_v = is_nothrow_copy_constructible<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is copy-constructible, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
template<class T>
struct is_copy_constructible :
    std::is_constructible<T, typename std::add_lvalue_reference<
        typename std::add_const<T>::type>::type> {};

template<class T>
struct is_trivially_copy_constructible :
    std::is_trivially_constructible<T, typename std::add_lvalue_reference<
        typename std::add_const<T>::type>::type> {};

template<class T>
struct is_nothrow_copy_constructible :
    std::is_nothrow_constructible<T, typename std::add_lvalue_reference<
        typename std::add_const<T>::type>::type> {};
```

### Notes

In many implementations, `is_nothrow_copy_constructible` also checks if the
destructor throws because it is effectively `noexcept(T(arg))`. Same applies to
`is_trivially_copy_constructible`, which, in these implementations, also
requires that the destructor is trivial: GCC bug 51452, LWG issue 2116.

### Example

```cpp
#include <iostream>
#include <type_traits>

struct Ex1
{
    std::string str; // member has a non-trivial copy ctor
};

struct Ex2
{
    int n;
    Ex2(const Ex2&) = default; // trivial and non-throwing
};

int main()
{
    std::cout << std::boolalpha << "Ex1 is copy-constructible? "
              << std::is_copy_constructible<Ex1>::value << '\n'
              << "Ex1 is trivially copy-constructible? "
              << std::is_trivially_copy_constructible<Ex1>::value << '\n'
              << "Ex2 is trivially copy-constructible? "
              << std::is_trivially_copy_constructible<Ex2>::value << '\n'
              << "Ex2 is nothrow copy-constructible? "
              << std::is_nothrow_copy_constructible<Ex2>::value << '\n';
}
```

Output:

```text
Ex1 is copy-constructible? true
Ex1 is trivially copy-constructible? false
Ex2 is trivially copy-constructible? true
Ex2 is nothrow copy-constructible? true
```

### See also

- **is_constructibleis_trivially_constructibleis_nothrow_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a constructor for specific
  arguments (class template)
-
  **is_default_constructibleis_trivially_default_constructibleis_nothrow_default_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a default constructor (class
  template)
-
  **is_move_constructibleis_trivially_move_constructibleis_nothrow_move_constructible
  (C++11)(C++11)(C++11)** — checks if a type can be constructed from an rvalue
  reference (class template)
- **copy_constructible (C++20)** — specifies that an object of a type can be
  copy constructed and move constructed (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_copy_constructible*
