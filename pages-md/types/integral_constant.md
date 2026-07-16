# std::integral_constant

```cpp
template< class T, T v >
struct integral_constant;  // (since C++11)
```

`std::integral_constant` wraps a static constant of specified type. It is the
base class for the C++ type traits.

The behavior of a program that adds specializations for `std::integral_constant`
is undefined.

### Helper alias templates

A helper alias template `std::bool_constant` is defined for the common case
where `T` is `bool`.

```cpp
template< bool B >
using bool_constant = integral_constant<bool, B>;  // (since C++17)
```

### Specializations

Two typedefs for the common case where `T` is `bool` are provided:

- **`true_type`** — `std::integral_constant<bool, true>`
- **`false_type`** — `std::integral_constant<bool, false>`

### Member types

- **`value_type`** — `T`
- **`type`** — `std::integral_constant<T, v>`

### Member constants

- **constexpr T value [static]** — static constant of type `T` with value `v`
  (public static member constant)

### Member functions

- ****operator value_type**** — returns the wrapped value (public member
  function)
- ****operator()** (C++14)** — returns the wrapped value (public member
  function)

## std::integral_constant::operator value_type

```cpp
constexpr operator value_type() const noexcept;
```

Conversion function. Returns the wrapped value.

## std::integral_constant::operator()

```cpp
constexpr value_type operator()() const noexcept;  // (since C++14)
```

Returns the wrapped value. This function enables `std::integral_constant` to
serve as a source of compile-time function objects.

### Possible implementation

```cpp
template<class T, T v>
struct integral_constant
{
    static constexpr T value = v;
    using value_type = T;
    using type = integral_constant; // using injected-class-name
    constexpr operator value_type() const noexcept { return value; }
    constexpr value_type operator()() const noexcept { return value; } // since c++14
};
```

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_integral_constant_callable` | 201304L | (C++14) |
      `std::integral_constant::operator()`
  `__cpp_lib_bool_constant` | 201505L | (C++17) | `std::bool_constant`

### Example

```cpp
#include <type_traits>

int main()
{
    typedef std::integral_constant<int, 2> two_t;
    typedef std::integral_constant<int, 4> four_t;

    static_assert(not std::is_same<two_t, four_t>::value,
                  "two_t and four_t are equal!");

    static_assert(two_t::value * 2 == four_t::value, "2*2 != 4");

    enum class my_e { e1, e2 };

    typedef std::integral_constant<my_e, my_e::e1> my_e_e1;
    typedef std::integral_constant<my_e, my_e::e2> my_e_e2;

    static_assert(my_e_e1() == my_e::e1);

    static_assert(my_e_e1::value != my_e::e2,
                 "my_e_e1::value == my_e::e2");

    static_assert(std::is_same<my_e_e2, my_e_e2>::value,
                  "my_e_e2 != my_e_e2");
}
```

---
*Source: https://en.cppreference.com/w/cpp/types/integral_constant*
