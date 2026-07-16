# std::is_member_object_pointer

```cpp
template< class T >
struct is_member_object_pointer;  // (since C++11)
```

`std::is_member_object_pointer` is a UnaryTypeTrait.

Checks whether `T` is a non-static member object pointer. Provides the member
constant `value` which is equal to `true`, if `T` is a non-static member object
pointer type. Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for
`std::is_member_object_pointer` or `std::is_member_object_pointer_v` is
undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_member_object_pointer_v = is_member_object_pointer<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a pointer to member object, `false`
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
struct is_member_object_pointer : std::integral_constant<
                                      bool,
                                      std::is_member_pointer<T>::value &&
                                      !std::is_member_function_pointer<T>::value
                                  > {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

int main()
{
    class C {};
    std::cout << "Is member object pointer?\n" << std::boolalpha
              << std::is_member_object_pointer_v<int(C::*)>
              << ": int(C::*)\n"
              << std::is_member_object_pointer_v<int(C::*)()>
              << ": int(C::*)()\n";
}
```

Output:

```text
Is member object pointer?
true: int(C::*)
false: int(C::*)()
```

### See also

- **is_pointer (C++11)** — checks if a type is a pointer type (class template)
- **is_member_pointer (C++11)** — checks if a type is a pointer to a non-static
  member function or object (class template)
- **is_member_function_pointer (C++11)** — checks if a type is a pointer to a
  non-static member function (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_member_object_pointer*
