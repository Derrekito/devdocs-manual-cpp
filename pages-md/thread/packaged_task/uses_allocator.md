# std::uses_allocator<std::packaged_task>

```cpp
template< class R, class Alloc >
struct uses_allocator<std::packaged_task<R>, Alloc> : true_type {};  // (since C++11) (removed in C++17)
```

Provides a specialization of the `std::uses_allocator` type trait for
`std::packaged_task`.

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
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/uses_allocator*
