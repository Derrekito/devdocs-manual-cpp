# std::pmr::polymorphic_allocator<T>::destroy

```cpp
template< class U >
void destroy( U* p );  // (since C++17) (deprecated in C++20)
```

Destroys the object pointed to by `p`, as if by calling `p->~U()`.

### Parameters

- **p** — pointer to the object being destroyed

### Notes

This function is deprecated via LWG issue 3036, because its functionality can be
provided by the default implementation of `std::allocator_traits::destroy` and
hence extraneous.

### See also

- **destroy [static]** — destructs an object stored in the allocated storage
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/destroy*
