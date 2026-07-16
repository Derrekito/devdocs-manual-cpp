# std::uninitialized_construct_using_allocator

```cpp
template< class T, class Alloc, class... Args >
constexpr T* uninitialized_construct_using_allocator( T* p,
                                                      const Alloc& alloc,
                                                      Args&&... args );  // (since C++20)
```

Creates an object of the given type `T` by means of uses-allocator construction
at the uninitialized memory location indicated by `p`.

Equivalent to

```cpp
return std::apply(
    [&]<class... Xs>(Xs&&...xs)
    {
        return std::construct_at(p, std::forward<Xs>(xs)...);
    },
    std::uses_allocator_construction_args<T>(alloc, std::forward<Args>(args)...));
```

### Parameters

- **p** — the memory location where the object will be placed
- **alloc** — the allocator to use
- **args** — the arguments to pass to T's constructor

### Return value

Pointer to the newly-created object of type `T`.

### Exceptions

May throw any exception thrown by the constructor of `T`, typically including
`std::bad_alloc`.

### Example

### See also

- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)
- **make_obj_using_allocator (C++20)** — creates an object of the given type by
  means of uses-allocator construction (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/uninitialized_construct_using_allocator*
