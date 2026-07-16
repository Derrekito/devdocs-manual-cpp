# std::is_aggregate

```cpp
template< class T >
struct is_aggregate;  // (since C++17)
```

`std::is_aggregate` is a UnaryTypeTrait.

If `T` is an aggregate type, provides the member constant `value` equal `true`.
For any other type, `value` is `false`.

If `T` is an incomplete type other than an array type or (possibly cv-qualified)
`void`, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_aggregate` or
`std::is_aggregate_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_aggregate_v = is_aggregate<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an aggregate type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_is_aggregate` | 201703L | (C++17) | `std::is_agregate`

### Example

```cpp
#include <new>
#include <type_traits>
#include <utility>

// constructs a T at the uninitialized memory pointed to by p using
// list-initialization for aggregates and non-list initialization otherwise
template<class T, class... Args>
T* construct(T* p, Args&&... args)
{
    if constexpr (std::is_aggregate_v<T>)
        return ::new (static_cast<void*>(p)) T{std::forward<Args>(args)...};
    else
        return ::new (static_cast<void*>(p)) T(std::forward<Args>(args)...);
}

struct A { int x, y; };
struct B { B(int, const char*) {} };

int main()
{
    std::aligned_union_t<1, A, B> storage;
    [[maybe_unused]] A* a = construct(reinterpret_cast<A*>(&storage), 1, 2);
    [[maybe_unused]] B* b = construct(reinterpret_cast<B*>(&storage), 1, "hello");
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3823 | C++17 | The behavior is undefined if `T` is an array type but
      `std::remove_all_extents_t<T>` is an incomplete type. | The behavior is
      defined regardless of the incompleteness of `std::remove_all_extents_t<T>`
      as long as `T` is an array type.

---
*Source: https://en.cppreference.com/w/cpp/types/is_aggregate*
