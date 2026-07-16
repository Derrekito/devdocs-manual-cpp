# std::is_const

```cpp
template< class T >
struct is_const;  // (since C++11)
```

`std::is_const` is a UnaryTypeTrait.

If `T` is a const-qualified type (that is, `const`, or `const volatile`),
provides the member constant `value` equal to `true`. For any other type,
`value` is `false`.

The behavior of a program that adds specializations for `std::is_const` or
`std::is_const_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_const_v = is_const<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a const-qualified type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

If `T` is a reference type then `is_const<T>::value` is always `false`. The
proper way to check a potentially-reference type for const-ness is to remove the
reference: `is_const<typename remove_reference<T>::type>`.

### Possible implementation

```cpp
template<class T> struct is_const          : std::false_type {};
template<class T> struct is_const<const T> : std::true_type {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

int main()
{
    std::cout << std::boolalpha
        << std::is_const_v<int> << '\n' // false
        << std::is_const_v<const int> << '\n' // true
        << std::is_const_v<const int*> // false
        << " because the pointer itself can be changed but not the int pointed at\n"
        << std::is_const_v<int* const> // true
        << " because the pointer itself can't be changed but the int pointed at can\n"
        << std::is_const_v<const int&> << '\n' // false
        << std::is_const_v<std::remove_reference_t<const int&>> << '\n' // true
        ;
}
```

Output:

```text
false
true
false because the pointer itself can be changed but not the int pointed at
true because the pointer itself can't be changed but the int pointed at can
false
true
```

### See also

- **is_volatile (C++11)** — checks if a type is volatile-qualified (class
  template)
- **as_const (C++17)** — obtains a reference to const to its argument (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_const*
