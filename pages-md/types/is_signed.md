# std::is_signed

```cpp
template< class T >
struct is_signed;  // (since C++11)
```

`std::is_signed` is a UnaryTypeTrait.

If `T` is an arithmetic type, provides the member constant `value` equal to
`true` if `T(-1) < T(0)`: this results in `true` for the floating-point types
and the signed integer types, and in `false` for the unsigned integer types and
the type `bool`.

For any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_signed` or
`std::is_signed_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_signed_v = is_signed<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a signed arithmetic type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
namespace detail
{
    template<typename T, bool = std::is_arithmetic<T>::value>
    struct is_signed : std::integral_constant<bool, T(-1) < T(0)> {};

    template<typename T>
    struct is_signed<T, false> : std::false_type {};
}

template<typename T>
struct is_signed : detail::is_signed<T>::type {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

class A {};
static_assert(std::is_signed_v<A> == false);

class B { int i; };
static_assert(std::is_signed_v<B> == false);

enum C : int {};
static_assert(std::is_signed_v<C> == false);

enum class D : int {};
static_assert(std::is_signed_v<D> == false);

static_assert(
    std::is_signed<signed int>::value == true and // C++11
    std::is_signed<signed int>() == true and      // C++11
    std::is_signed<signed int>{} == true and      // C++11
    std::is_signed_v<signed int> == true and      // C++17
    std::is_signed_v<unsigned int> == false and
    std::is_signed_v<float> == true and
    std::is_signed_v<bool> == false and
    std::is_signed_v<signed char> == true and
    std::is_signed_v<unsigned char> == false
);

int main()
{
    // signedness of char is implementation-defined:
    std::cout << std::boolalpha << std::is_signed_v<char> << '\n';
}
```

Possible output:

```text
true
```

### See also

- **is_unsigned (C++11)** — checks if a type is an unsigned arithmetic type
  (class template)
- **is_signed [static]** — identifies signed types (public static member
  constant of `std::numeric_limits<T>`)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **make_signed (C++11)** — makes the given integral type signed (class
  template)
- **make_unsigned (C++11)** — makes the given integral type unsigned (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_signed*
