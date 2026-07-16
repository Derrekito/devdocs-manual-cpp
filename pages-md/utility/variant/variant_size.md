# std::variant_size, std::variant_size_v

```cpp
template< class T >
struct variant_size; /* undefined */  // (1) (since C++17)
template< class... Types >
struct variant_size<std::variant<Types...>>
    : std::integral_constant<std::size_t, sizeof...(Types)> {};  // (2) (since C++17)
template< class T >
class variant_size<const T>;  // (3) (since C++17)
template< class T >
class variant_size<volatile T>;  // (4) (since C++17) (deprecated in C++20)
template< class T >
class variant_size<const volatile T>;  // (5) (since C++17) (deprecated in C++20)
```

Provides access to the number of alternatives in a possibly cv-qualified variant
as a compile-time constant expression.

Formally,

2) meets the UnaryTypeTrait requirements with a base characteristic of
   `std::integral_constant<std::size_t, sizeof...(Types)>`

3-5) meets the UnaryTypeTrait requirements with a base characteristic of
   `std::integral_constant<std::size_t, std::variant_size<T>::value>`

### Helper variable template

```cpp
template< class T >
inline constexpr std::size_t variant_size_v = std::variant_size<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `sizeof...(Types)` (public static member constant)

### Member functions

- **operator std::size_t** — converts the object to std::size_t, returns `value`
  (public member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — std::size_t
- **`type`** — std::integral_constant<std::size_t, value>

### Notes

All specializations of `std::variant_size` satisfy UnaryTypeTrait with *base
characteristic* `std::integral_constant<std::size_t, N>` for some `N`.

### Example

```cpp
#include <any>
#include <cstdio>
#include <variant>

static_assert(std::variant_size_v<std::variant<>> == 0);
static_assert(std::variant_size_v<std::variant<int>> == 1);
static_assert(std::variant_size_v<std::variant<int, int>> == 2);
static_assert(std::variant_size_v<std::variant<int, int, int>> == 3);
static_assert(std::variant_size_v<std::variant<int, float, double>> == 3);
static_assert(std::variant_size_v<std::variant<std::monostate, void>> == 2);
static_assert(std::variant_size_v<std::variant<const int, const float>> == 2);
static_assert(std::variant_size_v<std::variant<std::variant<std::any>>> == 1);

int main()
{
    std::puts("All static assertions passed.");
}
```

Output:

```text
All static assertions passed.
```

### See also

- **variant_alternativevariant_alternative_t (C++17)** — obtains the type of the
  alternative specified by its index, at compile time (class template) (alias
  template)
- **std::tuple_size<std::tuple> (C++11)** — obtains the size of `tuple` at
  compile time (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/variant_size*
