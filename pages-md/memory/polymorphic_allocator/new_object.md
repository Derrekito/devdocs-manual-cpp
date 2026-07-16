# std::pmr::polymorphic_allocator<T>::new_object

```cpp
template< class U, class... CtorArgs >
[[nodiscard]] U* new_object( CtorArgs&&... ctor_args );  // (since C++20)
```

Allocates and constructs an object of type `U`.

Given `alloc` is a `std::pmr::polymorphic_allocator<T>`:

```cpp
U* p = alloc.new_object<U>(std::forward<CtorArgs>(ctor_args)...);
```

is equivalent to

```cpp
U* p = alloc.allocate_object<U>();
try
{
    alloc.construct(p, std::forward<CtorArgs>(ctor_args)...);
}
catch (...)
{
    alloc.deallocate_object(p);
    throw;
}
```

### Parameters

- **ctor_args** — the arguments to forward to the constructor of `U`

### Return value

A pointer to the allocated and constructed object.

### Notes

This function was introduced for use with the fully-specialized allocator
`std::pmr::polymorphic_allocator<>`, but it may be useful in any specialization
as a shortcut to avoid having to rebind from
`std::pmr::polymorphic_allocator<T>` to `std::pmr::polymorphic_allocator<U>`,
and having to call `allocate`, `construct`, and `deallocate` individually.

Since `U` is not deduced, it must be provided as a template argument when
calling this function.

### Exceptions

May throw any exceptions thrown by the call to `allocate_object` or the
constructor of `U`.

### See also

- **allocate_bytes (C++20)** — allocate raw aligned memory from the underlying
  resource (public member function)
- **allocate_object (C++20)** — allocates raw memory suitable for an object or
  an array (public member function)
- **allocate** — allocate memory (public member function)
- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)
- **allocate** — allocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/new_object*
