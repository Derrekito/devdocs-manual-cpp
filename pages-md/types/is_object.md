# std::is_object

```cpp
template< class T >
struct is_object;  // (since C++11)
```

`std::is_object` is a UnaryTypeTrait.

If `T` is an object type (that is any possibly cv-qualified type other than
function, reference, or `void` types), provides the member constant `value`
equal `true`. For any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_object` or
`std::is_object_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_object_v = is_object<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an object type, `false` otherwise
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
struct is_object : std::integral_constant<bool,
                       std::is_scalar<T>::value ||
                       std::is_array<T>::value ||
                       std::is_union<T>::value ||
                       std::is_class<T>::value> {};
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <type_traits>

#define IS_OBJECT(...) \
    std::cout << std::boolalpha << std::left << std::setw(9) << #__VA_ARGS__ \
              << (std::is_object_v<__VA_ARGS__> ? " is object\n" \
                                                : " is not an object\n")

int main()
{
    class cls {};

    IS_OBJECT(void);
    IS_OBJECT(int);
    IS_OBJECT(int&);
    IS_OBJECT(int*);
    IS_OBJECT(int*&);
    IS_OBJECT(cls);
    IS_OBJECT(cls&);
    IS_OBJECT(cls*);
    IS_OBJECT(int());
    IS_OBJECT(int(*)());
    IS_OBJECT(int(&)());
}
```

Output:

```text
void      is not an object
int       is object
int&      is not an object
int*      is object
int*&     is not an object
cls       is object
cls&      is not an object
cls*      is object
int()     is not an object
int(*)()  is object
int(&)()  is not an object
```

### See also

- **is_scalar (C++11)** — checks if a type is a scalar type (class template)
- **is_array (C++11)** — checks if a type is an array type (class template)
- **is_union (C++11)** — checks if a type is a union type (class template)
- **is_class (C++11)** — checks if a type is a non-union class type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_object*
