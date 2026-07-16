# std::is_reference

```cpp
template< class T >
struct is_reference;  // (since C++11)
```

`std::is_reference` is a UnaryTypeTrait.

If `T` is a reference type (lvalue reference or rvalue reference), provides the
member constant `value` equal `true`. For any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_reference` or
`std::is_reference_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_reference_v = is_reference<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a reference type, `false` otherwise
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
template<class T> struct is_reference : std::false_type {};
template<class T> struct is_reference<T&> : std::true_type {};
template<class T> struct is_reference<T&&> : std::true_type {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

class A {};

int main()
{
#   define REF(x) << #x " ?: " << x << '\n'
    std::cout << std::boolalpha
    REF(std::is_reference_v<A>)
    REF(std::is_reference_v<A&>)
    REF(std::is_reference_v<A&&>)
    REF(std::is_reference_v<long>)
    REF(std::is_reference_v<long&>)
    REF(std::is_reference_v<long&&>)
    REF(std::is_reference_v<double*>)
    REF(std::is_reference_v<double*&>)
    REF(std::is_reference_v<double*&&>);
#   undef REF
}
```

Output:

```text
std::is_reference_v<A> ?: false
std::is_reference_v<A&> ?: true
std::is_reference_v<A&&> ?: true
std::is_reference_v<long> ?: false
std::is_reference_v<long&> ?: true
std::is_reference_v<long&&> ?: true
std::is_reference_v<double*> ?: false
std::is_reference_v<double*&> ?: true
std::is_reference_v<double*&&> ?: true
```

### See also

- **is_lvalue_reference (C++11)** — checks if a type is an *lvalue reference*
  (class template)
- **is_rvalue_reference (C++11)** — checks if a type is an *rvalue reference*
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_reference*
