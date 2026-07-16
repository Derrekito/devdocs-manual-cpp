# std::make_obj_using_allocator

```cpp
template< class T, class Alloc, class... Args >
constexpr T make_obj_using_allocator( const Alloc& alloc, Args&&... args );  // (since C++20)
```

Creates an object of the given type `T` by means of uses-allocator construction.

Equivalent to

```cpp
return std::make_from_tuple<T>(
    std::uses_allocator_construction_args<T>(alloc, std::forward<Args>(args)...)
);
```

### Parameters

- **alloc** — the allocator to use
- **args** — the arguments to pass to T's constructor

### Return value

The newly-created object of type `T`.

### Exceptions

May throw any exception thrown by the constructor of `T`, typically including
`std::bad_alloc`.

### Example

### See also

- **uses_allocator_construction_args (C++20)** — prepares the argument list
  matching the flavor of uses-allocator construction required by the given type
  (function template)
- **uninitialized_construct_using_allocator (C++20)** — creates an object of the
  given type at specified memory location by means of uses-allocator
  construction (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/make_obj_using_allocator*
