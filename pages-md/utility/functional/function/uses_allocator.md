# std::uses_allocator<std::function>

```cpp
template< class R, class... ArgTypes, class Alloc >
struct uses_allocator<std::function<R(ArgTypes...)>, Alloc> : std::true_type { };  // (since C++11) (until C++17)
```

This specialization of `std::uses_allocator` informs other library components
that all objects of type `std::function` support *uses-allocator construction*,
even though they do not have a nested `allocator_type`.

### Notes

`std::function`'s allocator support was poorly specified and inconsistently
implemented. Some implementations do not support uses-allocator construction at
all, some provide the needed constructor overloads but ignore the supplied
allocator argument, and some provide the overloads and use the supplied
allocator for construction but not when the `std::function` is reassigned. As a
result, allocator support was removed in C++17.

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### See also

- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function/uses_allocator*
