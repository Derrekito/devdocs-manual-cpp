# std::is_member_function_pointer

```cpp
template< class T >
struct is_member_function_pointer;  // (since C++11)
```

`std::is_member_function_pointer` is a UnaryTypeTrait.

Checks whether `T` is a non-static member function pointer. Provides the member
constant `value` which is equal to `true`, if `T` is a non-static member
function pointer type. Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for
`std::is_member_function_pointer` or `std::is_member_function_pointer_v` is
undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_member_function_pointer_v =
    is_member_function_pointer<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a member function pointer type, `false`
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
template<class T>
struct is_member_function_pointer_helper : std::false_type {};

template<class T, class U>
struct is_member_function_pointer_helper<T U::*> : std::is_function<T> {};

template<class T>
struct is_member_function_pointer
    : is_member_function_pointer_helper<typename std::remove_cv<T>::type> {};
```

### Example

```cpp
#include <type_traits>

class A
{
public:
    void member() {}
};

int main()
{
    // fails at compile time if A::member is a data member and not a function
    static_assert(std::is_member_function_pointer<decltype(&A::member)>::value,
                  "A::member is not a member function.");
}
```

### See also

- **is_pointer (C++11)** — checks if a type is a pointer type (class template)
- **is_member_object_pointer (C++11)** — checks if a type is a pointer to a
  non-static member object (class template)
- **is_member_pointer (C++11)** — checks if a type is a pointer to a non-static
  member function or object (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_member_function_pointer*
