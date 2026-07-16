# std::is_fundamental

```cpp
template< class T >
struct is_fundamental;  // (since C++11)
```

`std::is_fundamental` is a UnaryTypeTrait.

If `T` is a fundamental type (that is, arithmetic type, `void`, or `nullptr_t`),
provides the member constant `value` equal `true`. For any other type, `value`
is `false`.

The behavior of a program that adds specializations for `std::is_fundamental` or
`std::is_fundamental_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_fundamental_v = is_fundamental<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a fundamental type, `false` otherwise
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
struct is_fundamental
    : std::integral_constant<
        bool,
        std::is_arithmetic<T>::value ||
        std::is_void<T>::value ||
        std::is_same<std::nullptr_t, typename std::remove_cv<T>::type>::value
        // you can also use 'std::is_null_pointer<T>::value' instead in C++14
> {};
```

### Example

```cpp
#include <type_traits>

static_assert(std::is_fundamental_v<int> == true);
static_assert(std::is_fundamental_v<int&> == false);
static_assert(std::is_fundamental_v<int*> == false);
static_assert(std::is_fundamental_v<void> == true);
static_assert(std::is_fundamental_v<void*> == false);
static_assert(std::is_fundamental_v<float> == true);
static_assert(std::is_fundamental_v<float&> == false);
static_assert(std::is_fundamental_v<float*> == false);
static_assert(std::is_fundamental_v<std::nullptr_t> == true);
static_assert(std::is_fundamental_v<std::is_fundamental<int>> == false);

class A {};
static_assert(std::is_fundamental_v<A> == false);
static_assert(std::is_fundamental_v<std::is_fundamental<A>::value_type>);

int main() {}
```

### See also

- **is_compound (C++11)** — checks if a type is a compound type (class template)
- **is_arithmetic (C++11)** — checks if a type is an arithmetic type (class
  template)
- **is_void (C++11)** — checks if a type is void (class template)
- **is_null_pointer (C++14)** — checks if a type is `std::nullptr_t` (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_fundamental*
