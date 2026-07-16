# std::is_unsigned

```cpp
template< class T >
struct is_unsigned;  // (since C++11)
```

`std::is_unsigned` is a UnaryTypeTrait.

If `T` is an arithmetic type, provides the member constant `value` equal to
`true` if `T(0) < T(-1)`: this results in `true` for the unsigned integer types
and the type `bool` and in `false` for the signed integer types and the
floating-point types.

For any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_unsigned` or
`std::is_unsigned_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_unsigned_v = is_unsigned<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an unsigned integral type, `false`
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
    template<typename T,bool = std::is_arithmetic<T>::value>
    struct is_unsigned : std::integral_constant<bool, T(0) < T(-1)> {};

    template<typename T>
    struct is_unsigned<T,false> : std::false_type {};
} // namespace detail

template<typename T>
struct is_unsigned : detail::is_unsigned<T>::type {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

class A {};
static_assert(std::is_unsigned_v<A> == false);

enum B : unsigned {};
static_assert(std::is_unsigned_v<B> == false);

enum class C : unsigned {};
static_assert(std::is_unsigned_v<C> == false);

struct S { unsigned p : 1; int q : 1; };
static_assert(
    std::is_unsigned_v<decltype(S::p)> not_eq
    std::is_unsigned_v<decltype(S::q)>
);

static_assert(
    std::is_unsigned_v<float> == false &&
    std::is_unsigned_v<signed int> == false &&
    std::is_unsigned_v<unsigned int> == true &&
    std::is_unsigned_v<bool> == true
);

int main()
{
    std::cout << std::boolalpha << std::is_unsigned<char>::value << '\n';
}
```

Possible output:

```text
false
```

### See also

- **is_signed (C++11)** — checks if a type is a signed arithmetic type (class
  template)
- **is_signed [static]** — identifies signed types (public static member
  constant of `std::numeric_limits<T>`)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **make_signed (C++11)** — makes the given integral type signed (class
  template)
- **make_unsigned (C++11)** — makes the given integral type unsigned (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_unsigned*
