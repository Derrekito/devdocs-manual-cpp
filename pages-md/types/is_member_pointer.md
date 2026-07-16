# std::is_member_pointer

```cpp
template< class T >
struct is_member_pointer;  // (since C++11)
```

`std::is_member_pointer` is a UnaryTypeTrait.

If `T` is pointer to non-static member object or a pointer to non-static member
function, provides the member constant `value` equal `true`. For any other type,
`value` is `false`.

The behavior of a program that adds specializations for `std::is_member_pointer`
or `std::is_member_pointer_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_member_pointer_v = is_member_pointer<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a member pointer type, `false` otherwise
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
struct is_member_pointer_helper : std::false_type {};

template<class T, class U>
struct is_member_pointer_helper<T U::*> : std::true_type {};

template<class T>
struct is_member_pointer : is_member_pointer_helper<typename std::remove_cv<T>::type> {};
```

### Example

```cpp
#include <type_traits>

class C {};
static_assert(std::is_member_pointer_v<int(C::*)>);

static_assert(!std::is_member_pointer_v<int>);

int main()
{
}
```

### See also

- **is_pointer (C++11)** — checks if a type is a pointer type (class template)
- **is_member_object_pointer (C++11)** — checks if a type is a pointer to a
  non-static member object (class template)
- **is_member_function_pointer (C++11)** — checks if a type is a pointer to a
  non-static member function (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_member_pointer*
