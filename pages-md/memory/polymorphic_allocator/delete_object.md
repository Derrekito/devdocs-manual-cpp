# std::pmr::polymorphic_allocator<T>::delete_object

```cpp
template< class U >
void delete_object( U* p );  // (since C++20)
```

Destroys the object of type `U` and deallocates storage allocated for it.

Equivalent to
`std::allocator_traits<polymorphic_allocator>::destroy(*this, p);
deallocate_object(p);`

### Parameters

- **p** — pointer to the object to destroy and deallocate

### Exceptions

Throws nothing.

### Notes

This function was introduced for use with the fully-specialized allocator
`std::pmr::polymorphic_allocator<>`, but it may be useful in any specialization.

### See also

- **deallocate_bytes (C++20)** — free raw memory obtained from `allocate_bytes`
  (public member function)
- **deallocate_object (C++20)** — frees raw memory obtained by `allocate_object`
  (public member function)
- **deallocate [static]** — deallocates storage using the allocator (public
  static member function of `std::allocator_traits<Alloc>`)
- **deallocate** — deallocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/delete_object*
