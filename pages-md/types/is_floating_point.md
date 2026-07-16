# std::is_floating_point

```cpp
template< class T >
struct is_floating_point;  // (since C++11)
```

`std::is_floating_point` is a UnaryTypeTrait.

Checks whether `T` is a floating-point type. Provides the member constant
`value` which is equal to `true`, if `T` is the type `float`, `double`, `long
double`, or any extended floating-point types (`std::float16_t`,
`std::float32_t`, `std::float64_t`, `std::float128_t`, or
`std::bfloat16_t`)(since C++23), including any cv-qualified variants. Otherwise,
`value` is equal to `false`.

The behavior of a program that adds specializations for `std::is_floating_point`
or `std::is_floating_point_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_floating_point_v = is_floating_point<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a floating-point type (possibly
  cv-qualified), `false` otherwise (public static member constant)

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
struct is_floating_point
     : std::integral_constant<
         bool,
         // Note: standard floating-point types
         std::is_same<float, typename std::remove_cv<T>::type>::value
         || std::is_same<double, typename std::remove_cv<T>::type>::value
         || std::is_same<long double, typename std::remove_cv<T>::type>::value
         // Note: extended floating-point types (C++23, if supported)
         || std::is_same<std::float16_t, typename std::remove_cv<T>::type>::value
         || std::is_same<std::float32_t, typename std::remove_cv<T>::type>::value
         || std::is_same<std::float64_t, typename std::remove_cv<T>::type>::value
         || std::is_same<std::float128_t, typename std::remove_cv<T>::type>::value
         || std::is_same<std::bfloat16_t, typename std::remove_cv<T>::type>::value
     > {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

class A {};

int main()
{
    std::cout << std::boolalpha;
    std::cout << "      A: " << std::is_floating_point<A>::value << '\n';
    std::cout << "  float: " << std::is_floating_point<float>::value << '\n';
    std::cout << " float&: " << std::is_floating_point<float&>::value << '\n';
    std::cout << " double: " << std::is_floating_point<double>::value << '\n';
    std::cout << "double&: " << std::is_floating_point<double&>::value << '\n';
    std::cout << "    int: " << std::is_floating_point<int>::value << '\n';
}
```

Output:

```text
      A: false
  float: true
 float&: false
 double: true
double&: false
    int: false
```

### See also

- **is_iec559 [static]** — identifies the IEC 559/IEEE 754 floating-point types
  (public static member constant of `std::numeric_limits<T>`)
- **is_integral (C++11)** — checks if a type is an integral type (class
  template)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **floating_point (C++20)** — specifies that a type is a floating-point type
  (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_floating_point*
