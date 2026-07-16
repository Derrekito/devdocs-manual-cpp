# std::is_integral

```cpp
template< class T >
struct is_integral;  // (since C++11)
```

`std::is_integral` is a UnaryTypeTrait.

Checks whether `T` is an integral type. Provides the member constant `value`
which is equal to `true`, if `T` is the type bool, char, `char8_t`(since C++20),
char16_t, char32_t, wchar_t, short, int, long, long long, or any
implementation-defined extended integer types, including any signed, unsigned,
and cv-qualified variants. Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for `std::is_integral` or
`std::is_integral_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_integral_v = is_integral<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an integral type, `false` otherwise
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
// Note: this implementation uses C++20 facilities
template<class T>
struct is_integral : std::bool_constant<
    requires (T t, T* p, void (*f)(T)) // T* parameter excludes reference types
    {
        reinterpret_cast<T>(t); // Exclude class types
        f(0); // Exclude enumeration types
        p + t; // Exclude everything not yet excluded but integral types
    }> {};
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <type_traits>

class A {};

struct B { int x:4; };
using BF = decltype(B::x); // bit-field's type

enum E : int {};

template <class T>
T f(T i)
{
    static_assert(std::is_integral<T>::value, "Integral required.");
    return i;
}

#define SHOW(...) \
    std::cout << std::setw(29) << #__VA_ARGS__ << " == " << __VA_ARGS__ << '\n'

int main()
{
    std::cout << std::boolalpha;

    SHOW(std::is_integral<A>::value);
    SHOW(std::is_integral_v<E>);
    SHOW(std::is_integral_v<float>);
    SHOW(std::is_integral_v<int*>);
    SHOW(std::is_integral_v<int>);
    SHOW(std::is_integral_v<const int>);
    SHOW(std::is_integral_v<bool>);
    SHOW(std::is_integral_v<char>);
    SHOW(std::is_integral_v<BF>);
    SHOW(f(123));
}
```

Output:

```text
   std::is_integral<A>::value == false
        std::is_integral_v<E> == false
    std::is_integral_v<float> == false
     std::is_integral_v<int*> == false
      std::is_integral_v<int> == true
std::is_integral_v<const int> == true
     std::is_integral_v<bool> == true
     std::is_integral_v<char> == true
       std::is_integral_v<BF> == true
                       f(123) == 123
```

### See also

- **integral (C++20)** — specifies that a type is an integral type (concept)
- **is_integer [static]** — identifies integer types (public static member
  constant of `std::numeric_limits<T>`)
- **is_floating_point (C++11)** — checks if a type is a floating-point type
  (class template)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **is_enum (C++11)** — checks if a type is an enumeration type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_integral*
