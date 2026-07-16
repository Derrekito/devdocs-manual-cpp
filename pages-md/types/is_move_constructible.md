# std::is_move_constructible, std::is_trivially_move_constructible, std::is_nothrow_move_constructible

```cpp
template< class T >
struct is_move_constructible;  // (1) (since C++11)
template< class T >
struct is_trivially_move_constructible;  // (2) (since C++11)
template< class T >
struct is_nothrow_move_constructible;  // (3) (since C++11)
```

1) If `T` is not a referenceable type (i.e., possibly cv-qualified `void` or a
   function type with a *cv-qualifier-seq* or a *ref-qualifier*), provides a
   member constant `value` equal to `false`. Otherwise, provides a member
   constant `value` equal to `std::is_constructible<T, T&&>::value`.

2) Same as (1), but uses `std::is_trivially_constructible<T, T&&>`.

3) Same as (1), but uses `std::is_nothrow_constructible<T, T&&>`.

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
inline constexpr bool is_move_constructible_v =
    is_move_constructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_trivially_move_constructible_v =
    is_trivially_move_constructible<T>::value;  // (since C++17)
template< class T >
inline constexpr bool is_nothrow_move_constructible_v =
    is_nothrow_move_constructible<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is move-constructible, `false` otherwise
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
struct is_move_constructible :
    std::is_constructible<T, typename std::add_rvalue_reference<T>::type> {};

template<class T>
struct is_trivially_move_constructible :
    std::is_trivially_constructible<T, typename std::add_rvalue_reference<T>::type> {};

template<class T>
struct is_nothrow_move_constructible :
    std::is_nothrow_constructible<T, typename std::add_rvalue_reference<T>::type> {};
```

### Notes

Types without a move constructor, but with a copy constructor that accepts
`const T&` arguments, satisfy `std::is_move_constructible`.

Move constructors are usually noexcept, since otherwise they are unusable in any
code that provides strong exception guarantee.

In many implementations, `std::is_nothrow_move_constructible` also checks if the
destructor throws because it is effectively `noexcept(T(arg))`. Same applies to
`std::is_trivially_move_constructible`, which, in these implementations, also
requires that the destructor is trivial: GCC bug 51452, LWG issue 2116.

### Example

```cpp
#include <iostream>
#include <type_traits>

struct Ex1
{
    std::string str; // member has a non-trivial but non-throwing move ctor
};

struct Ex2
{
    int n;
    Ex2(Ex2&&) = default; // trivial and non-throwing
};

struct NoMove
{
    // prevents implicit declaration of default move constructor
    // however, the class is still move-constructible because its
    // copy constructor can bind to an rvalue argument
    NoMove(const NoMove&) {}
};

#define OUT(...) std::cout << #__VA_ARGS__ << " : " << __VA_ARGS__ << '\n'

int main()
{
    std::cout << std::boolalpha;
    OUT(std::is_move_constructible_v<Ex1>);
    OUT(std::is_trivially_move_constructible_v<Ex1>);
    OUT(std::is_nothrow_move_constructible_v<Ex1>);
    OUT(std::is_trivially_move_constructible_v<Ex2>);
    OUT(std::is_nothrow_move_constructible_v<Ex2>);
    OUT(std::is_move_constructible_v<NoMove>);
    OUT(std::is_nothrow_move_constructible_v<NoMove>);
}
```

Output:

```text
std::is_move_constructible_v<Ex1> : true
std::is_trivially_move_constructible_v<Ex1> : false
std::is_nothrow_move_constructible_v<Ex1> : true
std::is_trivially_move_constructible_v<Ex2> : true
std::is_nothrow_move_constructible_v<Ex2> : true
std::is_move_constructible_v<NoMove> : true
std::is_nothrow_move_constructible_v<NoMove> : false
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
  **is_copy_constructibleis_trivially_copy_constructibleis_nothrow_copy_constructible
  (C++11)(C++11)(C++11)** — checks if a type has a copy constructor (class
  template)
- **move_constructible (C++20)** — specifies that an object of a type can be
  move constructed (concept)
- **move (C++11)** — obtains an rvalue reference (function template)
- **move_if_noexcept (C++11)** — obtains an rvalue reference if the move
  constructor does not throw (function template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_move_constructible*
