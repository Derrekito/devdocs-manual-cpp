# std::negation

```cpp
template< class B >
struct negation;  // (since C++17)
```

Forms the logical negation of the type trait `B`.

The type `std::negation<B>` is a UnaryTypeTrait with a base characteristic of
`std::bool_constant<!bool(B::value)>`.

The behavior of a program that adds specializations for `std::negation` or
`std::negation_v` is undefined.

### Template parameters

- **B** — any type such that the expression `bool(B::value)` is a valid constant
  expression

### Helper variable template

```cpp
template< class B >
inline constexpr bool negation_v = negation<B>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `B` has a member `::value` that is `false` when
  explicitly converted to `bool`, `false` otherwise (public static member
  constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
template<class B>
struct negation : std::bool_constant<!bool(B::value)> { };
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_logical_traits` | 201510L | (C++17) | Logical operator type traits

### Example

```cpp
#include <iostream>
#include <type_traits>

static_assert(
    std::is_same<
        std::bool_constant<false>,
        typename std::negation<std::bool_constant<true>>::type>::value,
    "");
static_assert(
    std::is_same<
        std::bool_constant<true>,
        typename std::negation<std::bool_constant<false>>::type>::value,
    "");

int main()
{
    std::cout << std::boolalpha;
    std::cout << std::negation<std::bool_constant<true>>::value << '\n';
    std::cout << std::negation<std::bool_constant<false>>::value << '\n';
}
```

Output:

```text
false
true
```

### See also

- **conjunction (C++17)** — variadic logical AND metafunction (class template)
- **disjunction (C++17)** — variadic logical OR metafunction (class template)
- **integral_constantbool_constant (C++11)(C++17)** — compile-time constant of
  specified type with specified value (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/negation*
