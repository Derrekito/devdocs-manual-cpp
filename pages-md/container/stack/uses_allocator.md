# std::uses_allocator<std::stack>

```cpp
template< class T, class Container, class Alloc >
struct uses_allocator<stack<T,Container>,Alloc>
    : std::uses_allocator<Container, Alloc>::type {};  // (since C++11)
```

Provides a transparent specialization of the `std::uses_allocator` type trait
for `std::stack`: the container adaptor uses allocator if and only if the
underlying container does.

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
*Source: https://en.cppreference.com/w/cpp/container/stack/uses_allocator*
