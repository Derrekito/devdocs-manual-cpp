# std::is_volatile

```cpp
template< class T >
struct is_volatile;  // (since C++11)
```

`std::is_volatile` is a UnaryTypeTrait.

If `T` is a volatile-qualified type (that is, `volatile`, or `const volatile`),
provides the member constant `value` equal to `true`. For any other type,
`value` is `false`.

The behavior of a program that adds specializations for `std::is_volatile` or
`std::is_volatile_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_volatile_v = is_volatile<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a volatile-qualified type, `false`
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
template<class T> struct is_volatile : std::false_type {};
template<class T> struct is_volatile<volatile T> : std::true_type {};
```

### Example

```cpp
#include <type_traits>
#include <valarray>

static_assert(!std::is_volatile_v<int>);
static_assert(std::is_volatile_v<volatile int>);
static_assert(std::is_volatile_v<volatile const int>);
static_assert(std::is_volatile_v<volatile std::valarray<float>>);
static_assert(!std::is_volatile_v<std::valarray<volatile float>>);

int main() {}
```

### See also

- **is_const (C++11)** — checks if a type is const-qualified (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_volatile*
