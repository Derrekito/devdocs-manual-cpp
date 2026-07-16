# std::is_null_pointer

```cpp
template< class T >
struct is_null_pointer;  // (since C++14)
```

`std::is_null_pointer` is a UnaryTypeTrait.

Checks whether `T` is the type `std::nullptr_t`.

Provides the member constant `value` that is equal to `true`, if `T` is the type
`std::nullptr_t`, `const std::nullptr_t`, `volatile std::nullptr_t`, or `const
volatile std::nullptr_t`.

Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for `std::is_null_pointer`
or `std::is_null_pointer_v`(since C++17) is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_null_pointer_v = is_null_pointer<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is the type `std::nullptr_t` (possibly
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
struct is_null_pointer : std::is_same<std::nullptr_t, std::remove_cv_t<T>> {};
```

### Notes

`std::is_pointer` is false for `std::nullptr_t` because it is not a built-in
pointer type.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_null_pointer` | 201309L | (C++14) | `std::is_null_pointer`

### Example

```cpp
#include <type_traits>

static_assert(std::is_null_pointer_v<decltype(nullptr)>);
static_assert(!std::is_null_pointer_v<int*>);
static_assert(!std::is_pointer_v<decltype(nullptr)>);
static_assert(std::is_pointer_v<int*>);

int main()
{
}
```

### See also

- **is_void (C++11)** — checks if a type is void (class template)
- **is_array (C++11)** — checks if a type is an array type (class template)
- **is_pointer (C++11)** — checks if a type is a pointer type (class template)
- **is_enum (C++11)** — checks if a type is an enumeration type (class template)
- **is_union (C++11)** — checks if a type is a union type (class template)
- **is_class (C++11)** — checks if a type is a non-union class type (class
  template)
- **is_function (C++11)** — checks if a type is a function type (class template)
- **is_object (C++11)** — checks if a type is an object type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_null_pointer*
