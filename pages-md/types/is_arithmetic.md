# std::is_arithmetic

```cpp
template< class T >
struct is_arithmetic;  // (since C++11)
```

`std::is_arithmetic` is a UnaryTypeTrait.

If `T` is an arithmetic type (that is, an integral type or a floating-point
type) or a `cv-qualified` version thereof, provides the member constant `value`
equal to `true`. For any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_arithmetic` or
`std::is_arithmetic_v`(since C++17) is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_arithmetic_v = is_arithmetic<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an arithmetic type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Arithmetic types are the built-in types for which the arithmetic operators (`+`,
`-`, `*`, `/`) are defined (possibly in combination with the usual arithmetic
conversions).

Specializations of `std::numeric_limits` are provided for all arithmetic types.

### Possible implementation

```cpp
template<class T>
struct is_arithmetic : std::integral_constant<bool,
                                              std::is_integral<T>::value ||
                                              std::is_floating_point<T>::value> {};
```

### Example

```cpp
#include <atomic>
#include <cstddef>
#include <type_traits>

class A {};

enum class B : int { e };

static_assert(
    std::is_arithmetic_v<bool>            == true  and
    std::is_arithmetic_v<char>            == true  and
    std::is_arithmetic_v<char const>      == true  and
    std::is_arithmetic_v<int>             == true  and
    std::is_arithmetic_v<int const>       == true  and
    std::is_arithmetic_v<float>           == true  and
    std::is_arithmetic_v<float const>     == true  and
    std::is_arithmetic_v<std::size_t>     == true  and

    std::is_arithmetic_v<char&>           == false and
    std::is_arithmetic_v<char*>           == false and
    std::is_arithmetic_v<int&>            == false and
    std::is_arithmetic_v<int*>            == false and
    std::is_arithmetic_v<float&>          == false and
    std::is_arithmetic_v<float*>          == false and
    std::is_arithmetic_v<A>               == false and
    std::is_arithmetic_v<B>               == false and
    std::is_arithmetic_v<decltype(B::e)>  == false and
    std::is_arithmetic_v<std::byte>       == false and
    std::is_arithmetic_v<std::atomic_int> == false
);

int main() {}
```

### See also

- **is_integral (C++11)** — checks if a type is an integral type (class
  template)
- **is_floating_point (C++11)** — checks if a type is a floating-point type
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_arithmetic*
