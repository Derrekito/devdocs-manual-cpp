# std::is_default_constructible, std::is_trivially_default_constructible, std::is_nothrow_default_constructible

```cpp
template< class T >
struct is_default_constructible;  // (1) (since C++11)
template< class T >
struct is_trivially_default_constructible;  // (2) (since C++11)
template< class T >
struct is_nothrow_default_constructible;  // (3) (since C++11)
```

1) Provides the member constant `value` equal to
   `std::is_constructible<T>::value`.

2) Provides the member constant `value` equal to
   `std::is_trivially_constructible<T>::value`.

3) Provides the member constant `value` equal to
   `std::is_nothrow_constructible<T>::value`.

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
inline constexpr bool is_default_constructible_v =
    is_default_constructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_trivially_default_constructible_v =
    is_trivially_default_constructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_default_constructible_v =
    is_nothrow_default_constructible<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is default-constructible, `false` otherwise
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
struct is_default_constructible : std::is_constructible<T> {};

template<class T>
struct is_trivially_default_constructible : std::is_trivially_constructible<T> {};

template<class T>
struct is_nothrow_default_constructible : std::is_nothrow_constructible<T> {};
```

### Notes

In many implementations, `std::is_nothrow_default_constructible` also checks if
the destructor throws because it is effectively `noexcept(T())`. Same applies to
`std::is_trivially_default_constructible`, which, in these implementations, also
requires that the destructor is trivial: GCC bug 51452, LWG issue 2116.

`std::is_default_constructible<T>` does not test that `T x;` would compile; it
attempts direct-initialization with an empty argument list (see
`std::is_constructible`). Thus, `std::is_default_constructible_v<const int>` and
`std::is_default_constructible_v<const int[10]>` are `true`.

### Example

```cpp
#include <iostream>
#include <type_traits>

struct Ex1
{
    std::string str; // member has a non-trivial default ctor
};

struct Ex2
{
    int n;
    Ex2() = default; // trivial and non-throwing
};

int main()
{
    std::cout << std::boolalpha
              << "Ex1 is default-constructible? "
              << std::is_default_constructible<Ex1>::value << '\n'
              << "Ex1 is trivially default-constructible? "
              << std::is_trivially_default_constructible<Ex1>::value << '\n'
              << "Ex2 is trivially default-constructible? "
              << std::is_trivially_default_constructible<Ex2>::value << '\n'
              << "Ex2 is nothrow default-constructible? "
              << std::is_nothrow_default_constructible<Ex2>::value << '\n';
}
```

Output:

```text
Ex1 is default-constructible? true
Ex1 is trivially default-constructible? false
Ex2 is trivially default-constructible? true
Ex2 is nothrow default-constructible? true
```

### See also

- **is_constructibleis_trivially_constructibleis_nothrow_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a constructor for specific
  arguments (class template)
-
  **is_copy_constructibleis_trivially_copy_constructibleis_nothrow_copy_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a copy constructor (class
  template)
-
  **is_move_constructibleis_trivially_move_constructibleis_nothrow_move_constructible
  (C++11)(C++11)(C++11)** — checks if a type can be constructed from an rvalue
  reference (class template)
- **default_initializable (C++20)** — specifies that an object of a type can be
  default constructed (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_default_constructible*
