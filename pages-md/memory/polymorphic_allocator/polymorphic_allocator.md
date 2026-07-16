# std::pmr::polymorphic_allocator<T>::polymorphic_allocator

```cpp
polymorphic_allocator() noexcept;  // (1)
polymorphic_allocator( const polymorphic_allocator& other ) = default;  // (2)
template< class U >
polymorphic_allocator( const polymorphic_allocator<U>& other ) noexcept;  // (3)
polymorphic_allocator( std::pmr::memory_resource* r );  // (4)
```

Constructs a new `polymorphic_allocator`.

1) Constructs a `polymorphic_allocator` using the return value of
   `std::pmr::get_default_resource()` as the underlying memory resource.

2,3) Constructs a `polymorphic_allocator` using `other.resource()` as the
   underlying memory resource.

4) Constructs a `polymorphic_allocator` using `r` as the underlying memory
   resource. This constructor provides an implicit conversion from
   `std::pmr::memory_resource*`.

### Parameters

- **other** — another `polymorphic_allocator` to copy from
- **r** — pointer to the memory resource to use. May not be null

### Exceptions

4) Throws nothing.

### Notes

Copying a container using a `polymorphic_allocator` will not call the
allocator's copy constructor. Instead, the new container will use the return
value of `select_on_container_copy_construction` (a default-constructed
`polymorphic_allocator`) as its allocator.

### See also

- **select_on_container_copy_construction** — create a new
  `polymorphic_allocator` for use by a container's copy constructor (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/polymorphic_allocator*
